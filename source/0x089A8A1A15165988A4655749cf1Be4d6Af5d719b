// ██████  ███████ ██    ██  ██████  ██      ██    ██ ███████ ██  ██████  ███    ██                   
// ██   ██ ██      ██    ██ ██    ██ ██      ██    ██     ██  ██ ██    ██ ████   ██                  
// ██████  █████   ██    ██ ██    ██ ██      ██    ██   ██    ██ ██    ██ ██ ██  ██                   
// ██   ██ ██       ██  ██  ██    ██ ██      ██    ██  ██     ██ ██    ██ ██  ██ ██                   
// ██   ██ ███████   ████    ██████  ███████  ██████  ███████ ██  ██████  ██   ████    

// CONTRACT DEVELOPED BY REVOLUZION

//TheDivergentSociety
//Telegram: https://t.me/TheDivergentSociety_Official
//Web: https://divergentsociety.cloud

// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

/********************************************************************************************
  INTERFACE
********************************************************************************************/

interface IERC20 {
    
    // EVENT 

    event Transfer(address indexed from, address indexed to, uint256 value);
    
    event Approval(address indexed owner, address indexed spender, uint256 value);

    // FUNCTION

    function name() external view returns (string memory);
    
    function symbol() external view returns (string memory);
    
    function decimals() external view returns (uint8);
    
    function totalSupply() external view returns (uint256);
    
    function balanceOf(address account) external view returns (uint256);
    
    function transfer(address to, uint256 amount) external returns (bool);
    
    function allowance(address owner, address spender) external view returns (uint256);
    
    function approve(address spender, uint256 amount) external returns (bool);
    
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

interface IPair {

    // FUNCTION

    function token0() external view returns (address);

    function token1() external view returns (address);
}

interface IFactory {

    // FUNCTION

    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IRouter {

    // FUNCTION

    function WETH() external pure returns (address);
        
    function factory() external pure returns (address);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(uint256 amountIn, uint256 amountOutMin, address[] calldata path, address to, uint256 deadline) external;
    
    function swapExactETHForTokensSupportingFeeOnTransferTokens(uint256 amountOutMin, address[] calldata path, address to, uint256 deadline) external payable;
    
    function swapExactTokensForTokens(uint256 amountIn, uint256 amountOutMin, address[] calldata path, address to, uint256 deadline) external returns (uint256[] memory amounts);

    function addLiquidityETH(address token, uint256 amountTokenDesired, uint256 amountTokenMin, uint256 amountETHMin, address to, uint256 deadline) external payable returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);
}

interface IAuthError {

    // ERROR

    error InvalidOwner(address account);

    error UnauthorizedAccount(address account);

    error InvalidAuthorizedAccount(address account);

    error CurrentAuthorizedState(address account, bool state);
}

interface ICommonError {

    // ERROR

    error CannotUseCurrentAddress(address current);

    error CannotUseCurrentValue(uint256 current);

    error CannotUseCurrentState(bool current);

    error InvalidAddress(address invalid);

    error InvalidValue(uint256 invalid);
}

/********************************************************************************************
  ACCESS
********************************************************************************************/

abstract contract Auth is IAuthError {
    
    // DATA

    address private _owner;

    // MAPPING

    mapping(address => bool) public authorization;

    // MODIFIER

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    modifier authorized() {
        _checkAuthorized();
        _;
    }

    // CONSTRUCCTOR

    constructor(address initialOwner) {
        _transferOwnership(initialOwner);
        authorization[initialOwner] = true;
        if (initialOwner != msg.sender) {
            authorization[msg.sender] = true;
        }
    }

    // EVENT
    
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    event UpdateAuthorizedAccount(address authorizedAccount, address caller, bool state, uint256 timestamp);

    // FUNCTION
    
    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        if (owner() != msg.sender) {
            revert UnauthorizedAccount(msg.sender);
        }
    }

    function _checkAuthorized() internal view virtual {
        if (!authorization[msg.sender]) {
            revert UnauthorizedAccount(msg.sender);
        }
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        if (newOwner == address(0)) {
            revert InvalidOwner(address(0));
        }
        _transferOwnership(newOwner);
    }

    function authorize(address account) public virtual onlyOwner {
        if (account == address(0) || account == address(0xdead)) {
            revert InvalidAuthorizedAccount(account);
        }
        _authorization(account, msg.sender, true);
    }

    function unauthorize(address account) public virtual onlyOwner {
        if (account == address(0) || account == address(0xdead)) {
            revert InvalidAuthorizedAccount(account);
        }
        _authorization(account, msg.sender, false);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }

    function _authorization(address account, address caller, bool state) internal virtual {
        if (authorization[account] == state) {
            revert CurrentAuthorizedState(account, state);
        }
        authorization[account] = state;
        emit UpdateAuthorizedAccount(account, caller, state, block.timestamp);
    }
}

/********************************************************************************************
  TOKEN
********************************************************************************************/

contract TheDivergentSociety is Auth, ICommonError, IERC20 {

    // DATA

    struct Fee {
        uint256 marketing;
        uint256 charity;
        uint256 liquidity;
    }

    IRouter public router;

    Fee public buyFee = Fee(400, 300, 300);
    Fee public sellFee = Fee(300, 300, 400);
    Fee public transferFee = Fee(0, 0, 0);
    Fee public collectedFee = Fee(0, 0, 0);
    Fee public redeemedFee = Fee(0, 0, 0);
    
    string private constant NAME = "TheDivergentSociety";
    string private constant SYMBOL = "TDS";

    uint8 private constant DECIMALS = 18;

    uint256 private _totalSupply;
    
    uint256 public constant FEEDENOMINATOR = 10_000;

    uint256 public startBlock = 0;
    uint256 public totalFeeCollected = 0;
    uint256 public totalFeeRedeemed = 0;
    uint256 public totalTriggerZeusBuyback = 0;
    uint256 public lastTriggerZeusTimestamp = 0;
    uint256 public minSwap = 10_000 ether;

    bool private constant ISTDS = true;

    bool public presaleFinalized = false;
    bool public isFeeActive = false;
    bool public isFeeLocked = false;
    bool public isSwapEnabled = false;
    bool public inSwap = false;

    address public constant ZERO = address(0);
    address public constant DEAD = address(0xdead);
    address public constant PROJECTOWNER = 0xC4c01D6853B19D31D5b522B8479B628B126Fc4C8;
    
    address public pair;

    address public charityReceiver = 0x032700E5020D29964F406207601C3429063419f9;
    address public marketingReceiver = 0xA6c2Ce060e82c67b9ba62298bf8cfc79137e8617;
    
    // MAPPING

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    mapping(address => bool) public isExcludeFromFees;
    mapping(address => bool) public isPairLP;

    // MODIFIER

    modifier swapping() {
        inSwap = true;
        _;
        inSwap = false;
    }

    // ERROR

    error InvalidTotalFee(uint256 current, uint256 max);

    error InvalidFeeActiveState(bool current);

    error InvalidSwapEnabledState(bool current);

    error PresaleAlreadyFinalized(bool current);

    error TriggerZeusBuybackInCooldown(uint256 timeleft);

    error FeeLocked();

    error WaitFiveBlock();

    // CONSTRUCTOR

    constructor() Auth (msg.sender) {
        _mint(msg.sender, 100_000_000 * 10**DECIMALS);

        router = IRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        pair = IFactory(router.factory()).createPair(address(this), router.WETH());

        isPairLP[pair] = true;

        isExcludeFromFees[msg.sender] = true;
        isExcludeFromFees[PROJECTOWNER] = true;
        isExcludeFromFees[address(router)] = true;
    }

    // EVENT

    event UpdateRouter(address oldRouter, address newRouter, address caller, uint256 timestamp);

    event UpdateMinSwap(uint256 oldMinSwap, uint256 newMinSwap, address caller, uint256 timestamp);

    event UpdateFeeActive(bool oldStatus, bool newStatus, address caller, uint256 timestamp);

    event UpdateSwapEnabled(bool oldStatus, bool newStatus, address caller, uint256 timestamp);

    event UpdateBuyFee(uint256 oldMarketingFee, uint256 oldCharityFee, uint256 oldLiquidityFee, uint256 newMarketingFee, uint256 newCharityFee, uint256 newLiquidityFee, address caller, uint256 timestamp);

    event UpdateSellFee(uint256 oldMarketingFee, uint256 oldCharityFee, uint256 oldLiquidityFee, uint256 newMarketingFee, uint256 newCharityFee, uint256 newLiquidityFee, address caller, uint256 timestamp);

    event UpdateTransferFee(uint256 oldMarketingFee, uint256 oldCharityFee, uint256 oldLiquidityFee, uint256 newMarketingFee, uint256 newCharityFee, uint256 newLiquidityFee, address caller, uint256 timestamp);

    event UpdateMarketingReceiver(address oldMarketingReceiver, address newMarketingReceiver, address caller, uint256 timestamp);

    event UpdateCharityReceiver(address oldCharityReceiver, address newCharityReceiver, address caller, uint256 timestamp);
    
    event AutoRedeem(uint256 marketingFeeDistribution, uint256 liquidityFeeDistribution, uint256 charityFeeDistribution, uint256 redeemAmount, address caller, uint256 timestamp);

    // FUNCTION

    /* General */

    receive() external payable {}

    function finalizePresale() external authorized {
        if (presaleFinalized) { revert PresaleAlreadyFinalized(presaleFinalized); }
        if (isFeeActive) { revert InvalidFeeActiveState(isFeeActive); }
        if (isSwapEnabled) { revert InvalidSwapEnabledState(isSwapEnabled); }
        isFeeActive = true;
        isSwapEnabled = true;
        presaleFinalized = true;
        startBlock = block.number;
    }

    function lockFees() external onlyOwner {
        if (isFeeLocked) { revert FeeLocked(); }
        isFeeLocked = true;
    }

    /* Redeem */

    function autoRedeem(uint256 amountToRedeem) public swapping returns (uint256) {  
        uint256 marketingToRedeem = collectedFee.marketing - redeemedFee.marketing;
        uint256 liquidityToRedeem = collectedFee.liquidity - redeemedFee.liquidity;
        uint256 totalToRedeem = totalFeeCollected - totalFeeRedeemed;

        uint256 initialBalance = address(this).balance;
        uint256 marketingFeeDistribution = amountToRedeem * marketingToRedeem / totalToRedeem;
        uint256 liquidityFeeDistribution = amountToRedeem * liquidityToRedeem / totalToRedeem;
        uint256 charityFeeDistribution = amountToRedeem - marketingFeeDistribution - liquidityFeeDistribution;
        uint256 firstLiquidityHalf = liquidityFeeDistribution / 2;
        uint256 secondLiquidityHalf = liquidityFeeDistribution - firstLiquidityHalf;
        uint256 redeemAmount = amountToRedeem;

        redeemedFee.marketing += marketingFeeDistribution;
        redeemedFee.liquidity += liquidityFeeDistribution;
        redeemedFee.charity += charityFeeDistribution;
        totalFeeRedeemed += amountToRedeem;

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();

        _approve(address(this), address(router), redeemAmount);
    
        emit AutoRedeem(marketingFeeDistribution, liquidityFeeDistribution, charityFeeDistribution, redeemAmount, msg.sender, block.timestamp);

        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            marketingFeeDistribution,
            0,
            path,
            marketingReceiver,
            block.timestamp
        );

        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            firstLiquidityHalf,
            0,
            path,
            address(this),
            block.timestamp
        );
        
        (, , uint256 liquidity) = router.addLiquidityETH{
            value: address(this).balance - initialBalance
        }(
            address(this),
            secondLiquidityHalf,
            0,
            0,
            DEAD,
            block.timestamp + 1_200
        );

        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            charityFeeDistribution,
            0,
            path,
            charityReceiver,
            block.timestamp
        );
        
        return liquidity;
    }

    /* Check */

    function isTDS() external pure returns (bool) {
        return ISTDS;
    }

    function circulatingSupply() external view returns (uint256) {
        return totalSupply() - balanceOf(DEAD) - balanceOf(ZERO);
    }

    /* Update */

    function updateRouter(address newRouter) external onlyOwner {
        if (address(router) == newRouter) { revert CannotUseCurrentAddress(newRouter); }
        address oldRouter = address(router);
        router = IRouter(newRouter);
        
        isExcludeFromFees[newRouter] = true;

        emit UpdateRouter(oldRouter, newRouter, msg.sender, block.timestamp);
        pair = IFactory(router.factory()).createPair(address(this), router.WETH());
    }

    function updateMinSwap(uint256 newMinSwap) external authorized {
        if (minSwap == newMinSwap) { revert CannotUseCurrentValue(newMinSwap); }
        uint256 oldMinSwap = minSwap;
        minSwap = newMinSwap;
        emit UpdateMinSwap(oldMinSwap, newMinSwap, msg.sender, block.timestamp);
    }

    function updateBuyFee(uint256 newMarketingFee, uint256 newCharityFee, uint256 newLiquidityFee) external authorized {
        if (isFeeLocked) { revert FeeLocked(); }
        if (newMarketingFee + newCharityFee + newLiquidityFee > 1000) { revert InvalidTotalFee(newMarketingFee + newCharityFee + newLiquidityFee, 1000); }
        uint256 oldMarketingFee = buyFee.marketing;
        uint256 oldCharityFee = buyFee.charity;
        uint256 oldLiquidityFee = buyFee.liquidity;
        buyFee.marketing = newMarketingFee;
        buyFee.charity = newCharityFee;
        buyFee.liquidity = newLiquidityFee;
        emit UpdateBuyFee(oldMarketingFee, oldCharityFee, oldLiquidityFee, newMarketingFee, newCharityFee, newLiquidityFee, msg.sender, block.timestamp);
    }

    function updateSellFee(uint256 newMarketingFee, uint256 newCharityFee, uint256 newLiquidityFee) external authorized {
        if (isFeeLocked) { revert FeeLocked(); }
        if (newMarketingFee + newCharityFee + newLiquidityFee > 1000) { revert InvalidTotalFee(newMarketingFee + newCharityFee + newLiquidityFee, 1000); }
        uint256 oldMarketingFee = sellFee.marketing;
        uint256 oldCharityFee = sellFee.charity;
        uint256 oldLiquidityFee = sellFee.liquidity;
        sellFee.marketing = newMarketingFee;
        sellFee.charity = newCharityFee;
        sellFee.liquidity = newLiquidityFee;
        emit UpdateSellFee(oldMarketingFee, oldCharityFee, oldLiquidityFee, newMarketingFee, newCharityFee, newLiquidityFee, msg.sender, block.timestamp);
    }

    function updateTransferFee(uint256 newMarketingFee, uint256 newCharityFee, uint256 newLiquidityFee) external authorized {
        if (isFeeLocked) { revert FeeLocked(); }
        if (newMarketingFee + newCharityFee + newLiquidityFee > 1000) { revert InvalidTotalFee(newMarketingFee + newCharityFee + newLiquidityFee, 1000); }
        uint256 oldMarketingFee = transferFee.marketing;
        uint256 oldCharityFee = transferFee.charity;
        uint256 oldLiquidityFee = transferFee.liquidity;
        transferFee.marketing = newMarketingFee;
        transferFee.charity = newCharityFee;
        transferFee.liquidity = newLiquidityFee;
        emit UpdateTransferFee(oldMarketingFee, oldCharityFee, oldLiquidityFee, newMarketingFee, newCharityFee, newLiquidityFee, msg.sender, block.timestamp);
    }

    function updateFeeActive(bool newStatus) external authorized {
        if (isFeeActive == newStatus) { revert CannotUseCurrentState(newStatus); }
        bool oldStatus = isFeeActive;
        isFeeActive = newStatus;
        emit UpdateFeeActive(oldStatus, newStatus, msg.sender, block.timestamp);
    }

    function updateSwapEnabled(bool newStatus) external authorized {
        if (isSwapEnabled == newStatus) { revert CannotUseCurrentState(newStatus); }
        bool oldStatus = isSwapEnabled;
        isSwapEnabled = newStatus;
        emit UpdateSwapEnabled(oldStatus, newStatus, msg.sender, block.timestamp);
    }

    function updateMarketingReceiver(address newMarketingReceiver) external authorized {
        if (marketingReceiver == newMarketingReceiver) { revert CannotUseCurrentAddress(newMarketingReceiver); }
        address oldMarketingReceiver = marketingReceiver;
        marketingReceiver = newMarketingReceiver;
        emit UpdateMarketingReceiver(oldMarketingReceiver, newMarketingReceiver, msg.sender, block.timestamp);
    }

    function updateCharityReceiver(address newCharityReceiver) external authorized {
        if (charityReceiver == newCharityReceiver) { revert CannotUseCurrentAddress(newCharityReceiver); }
        address oldCharityReceiver = charityReceiver;
        charityReceiver = newCharityReceiver;
        emit UpdateCharityReceiver(oldCharityReceiver, newCharityReceiver, msg.sender, block.timestamp);
    }

    function setExcludeFromFees(address user, bool status) external authorized {
        if (isExcludeFromFees[user] == status) { revert CannotUseCurrentState(status); }
        isExcludeFromFees[user] = status;
    }

    function setPairLP(address lpPair, bool status) external authorized {
        if (isPairLP[lpPair] == status) { revert CannotUseCurrentState(status); }
        if (IPair(lpPair).token0() != address(this) && IPair(lpPair).token1() != address(this)) { revert InvalidAddress(lpPair); }
        isPairLP[lpPair] = status;
    }

    /* Fee */

    function takeBuyFee(address from, uint256 amount) internal swapping returns (uint256) {
        uint256 feeTotal = buyFee.marketing + buyFee.charity + buyFee.liquidity;
        uint256 feeAmount = amount * feeTotal / FEEDENOMINATOR;
        uint256 newAmount = amount - feeAmount;
        tallyFee(from, feeAmount, feeTotal, buyFee);
        return newAmount;
    }

    function takeSellFee(address from, uint256 amount) internal swapping returns (uint256) {
        uint256 feeTotal = sellFee.marketing + sellFee.charity + sellFee.liquidity;
        uint256 feeAmount = amount * feeTotal / FEEDENOMINATOR;
        uint256 newAmount = amount - feeAmount;
        tallyFee(from, feeAmount, feeTotal, sellFee);
        return newAmount;
    }

    function takeTransferFee(address from, uint256 amount) internal swapping returns (uint256) {
        uint256 feeTotal = sellFee.marketing + sellFee.charity + sellFee.liquidity;
        uint256 feeAmount = amount * feeTotal / FEEDENOMINATOR;
        uint256 newAmount = amount - feeAmount;
        tallyFee(from, feeAmount, feeTotal, transferFee);
        return newAmount;
    }

    function tallyFee(address from, uint256 amount, uint256 feeTotal, Fee memory fee) internal swapping {
        uint256 collectMarketing = amount * fee.marketing / feeTotal;
        uint256 collectLiquidity = amount * fee.charity / feeTotal;
        uint256 collectCharity = amount - collectMarketing - collectLiquidity;
        
        collectedFee.marketing += collectMarketing;
        collectedFee.charity += collectCharity;
        collectedFee.liquidity += collectLiquidity;
        totalFeeCollected += amount;

        _balances[from] -= amount;
        _balances[address(this)] += amount;
    }

    /* Buyback */

    function triggerZeusBuyback(uint256 amount) external authorized {
        totalTriggerZeusBuyback += amount;
        lastTriggerZeusTimestamp = block.timestamp;
        buyTokens(amount, DEAD);
    }

    function buyTokens(uint256 amount, address to) internal swapping {
        if (msg.sender == DEAD) { revert InvalidAddress(DEAD); }
        address[] memory path = new address[](2);
        path[0] = router.WETH();
        path[1] = address(this);

        router.swapExactETHForTokensSupportingFeeOnTransferTokens{
            value: amount
        } (0, path, to, block.timestamp);
    }

    /* ERC20 Standard */

    function name() external view virtual override returns (string memory) {
        return NAME;
    }
    
    function symbol() external view virtual override returns (string memory) {
        return SYMBOL;
    }
    
    function decimals() external view virtual override returns (uint8) {
        return DECIMALS;
    }
    
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }
    
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }
    
    function transfer(address to, uint256 amount) external virtual override returns (bool) {
        address provider = msg.sender;
        return _transfer(provider, to, amount);
    }
    
    function allowance(address provider, address spender) public view virtual override returns (uint256) {
        return _allowances[provider][spender];
    }
    
    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        address provider = msg.sender;
        _approve(provider, spender, amount);
        return true;
    }
    
    function transferFrom(address from, address to, uint256 amount) external virtual override returns (bool) {
        address spender = msg.sender;
        _spendAllowance(from, spender, amount);
        return _transfer(from, to, amount);
    }
    
    function increaseAllowance(address spender, uint256 addedValue) external virtual returns (bool) {
        address provider = msg.sender;
        _approve(provider, spender, allowance(provider, spender) + addedValue);
        return true;
    }
    
    function decreaseAllowance(address spender, uint256 subtractedValue) external virtual returns (bool) {
        address provider = msg.sender;
        uint256 currentAllowance = allowance(provider, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(provider, spender, currentAllowance - subtractedValue);
        }

        return true;
    }
    
    function _mint(address account, uint256 amount) internal virtual {
        if (account == ZERO) { revert InvalidAddress(account); }

        _totalSupply += amount;
        unchecked {
            _balances[account] += amount;
        }
        emit Transfer(address(0), account, amount);
    }

    function _approve(address provider, address spender, uint256 amount) internal virtual {
        if (provider == ZERO) { revert InvalidAddress(provider); }
        if (spender == ZERO) { revert InvalidAddress(spender); }

        _allowances[provider][spender] = amount;
        emit Approval(provider, spender, amount);
    }
    
    function _spendAllowance(address provider, address spender, uint256 amount) internal virtual {
        uint256 currentAllowance = allowance(provider, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked {
                _approve(provider, spender, currentAllowance - amount);
            }
        }
    }

    /* Additional */

    function _basicTransfer(address from, address to, uint256 amount ) internal returns (bool) {
        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[from] = fromBalance - amount;
            _balances[to] += amount;
        }

        emit Transfer(from, to, amount);
        return true;
    }
    
    /* Overrides */
 
    function _transfer(address from, address to, uint256 amount) internal virtual returns (bool) {
        if (from == ZERO) { revert InvalidAddress(from); }
        if (to == ZERO) { revert InvalidAddress(to); }

        if (presaleFinalized && startBlock + 5 >= block.number) { revert WaitFiveBlock(); }

        if (inSwap || isExcludeFromFees[from]) {
            return _basicTransfer(from, to, amount);
        }

        if (from != pair && isSwapEnabled && balanceOf(address(this)) >= minSwap && totalFeeCollected - totalFeeRedeemed >= minSwap) {
            autoRedeem(minSwap);
        }

        uint256 newAmount = amount;

        if (isFeeActive && !isExcludeFromFees[from] && !isExcludeFromFees[to]) {
            newAmount = _beforeTokenTransfer(from, to, amount);
        }

        require(_balances[from] >= newAmount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[from] = _balances[from] - newAmount;
            _balances[to] += newAmount;
        }

        emit Transfer(from, to, newAmount);

        return true;
    }

    function _beforeTokenTransfer(address from, address to, uint256 amount) internal swapping virtual returns (uint256) {
        if (isPairLP[from] && (buyFee.marketing + buyFee.charity + buyFee.liquidity > 0)) {
            return takeBuyFee(from, amount);
        }
        if (isPairLP[to] && (sellFee.marketing + sellFee.charity + sellFee.liquidity > 0)) {
            return takeSellFee(from, amount);
        }
        if (!isPairLP[from] && !isPairLP[to] && (transferFee.marketing + transferFee.charity + transferFee.liquidity > 0)) {
            return takeTransferFee(from, amount);
        }
        return amount;
    }

}