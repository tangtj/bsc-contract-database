// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

/********************************************************************************************
  LIBRARY / DEPENDENCY
********************************************************************************************/

abstract contract Ownable {
    
    // DATA

    address private _owner;

    // ERROR

    error OwnableUnauthorizedAccount(address account);

    error OwnableInvalidOwner(address owner);

    // MODIFIER

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    // CONSTRUCCTOR

    constructor(address initialOwner) {
        _transferOwnership(initialOwner);
    }

    // EVENT
    
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    // FUNCTION
    
    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        if (owner() != msg.sender) {
            revert OwnableUnauthorizedAccount(msg.sender);
        }
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        if (newOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

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

    function addLiquidityETH(address token, uint256 amountTokenDesired, uint256 amountTokenMin, uint256 amountETHMin, address to, uint256 deadline) external payable returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);
}

/********************************************************************************************
  TOKEN
********************************************************************************************/

contract HAMSTERSRC is Ownable, IERC20 {

    // DATA

    string private constant NAME = "HAMSTERSRC";
    string private constant SYMBOL = "HAMSTERSRC";

    uint8 private constant DECIMALS = 9;

    uint256 private _totalSupply;
    
    uint256 public constant FEEDENOMINATOR = 10_000;

    uint256 public buyMarketingFee = 500;
    uint256 public sellMarketingFee = 500;
    uint256 public transferMarketingFee = 0;
    uint256 public marketingFeeCollected = 0;
    uint256 public totalFeeCollected = 0;
    uint256 public marketingFeeRedeemed = 0;
    uint256 public totalFeeRedeemed = 0;
    uint256 public minSwap = 1_000 gwei;

    bool private constant ISHAMSTERSRC = true;

    bool public isFeeActive = false;
    bool public isFeeLocked = false;
    bool public isTradeEnabled = false;
    bool public isSwapEnabled = false;
    bool public inSwap = false;

    address public constant ZERO = address(0);
    address public constant DEAD = address(0xdead);

    address public pair;

    address public marketingReceiver = 0xAE217Da94238c8963b1852ce9346ee61BDE5605D;

    IRouter public router;
    
    // MAPPING

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) public isExcludeFromFees;
    mapping(address => bool) public allowTrade;

    // MODIFIER

    modifier swapping() {
        inSwap = true;
        _;
        inSwap = false;
    }

    // CONSTRUCTOR

    constructor(
        address routerAddress
    ) Ownable (msg.sender) {
        _mint(msg.sender, 1 ether);

        router = IRouter(routerAddress);
        pair = IFactory(router.factory()).createPair(address(this), router.WETH());

        isExcludeFromFees[msg.sender] = true;
        isExcludeFromFees[marketingReceiver] = true;

        allowTrade[msg.sender] = true;
        allowTrade[marketingReceiver] = true;
    }

    // EVENT

    event UpdateRouter(address oldRouter, address newRouter, uint256 timestamp);

    event UpdateMinSwap(uint256 oldMinSwap, uint256 newMinSwap, uint256 timestamp);

    event UpdateFeeActive(bool oldStatus, bool newStatus, uint256 timestamp);

    event UpdateSwapEnabled(bool oldStatus, bool newStatus, uint256 timestamp);

    event TradeEnabled(uint256 timestamp);

    event UpdateMarketingReceiver(address oldMarketingReceiver, address newMarketingReceiver, uint256 timestamp);
    
    event AutoRedeem(uint256 marketingFeeDistribution, uint256 amountToRedeem, uint256 timestamp);

    event UpdateMarketingBuyFee(uint256 oldMarketingFee, uint256 newMarketingFee, address sender, uint256 timestamp);

    event UpdateMarketingSellFee(uint256 oldMarketingFee, uint256 newMarketingFee, address sender, uint256 timestamp);

    event UpdateMarketingTransferFee(uint256 oldMarketingFee, uint256 newMarketingFee, address sender, uint256 timestamp);

    // FUNCTION

    /* General */

    receive() external payable {}

    function enabledTrade() external onlyOwner {
        require(!isTradeEnabled, "Enable Trade: Trade already enabled.");
        isTradeEnabled = true;
        emit TradeEnabled(block.timestamp);
    }

    function finalizePresale() external onlyOwner {
        require(!isFeeActive, "Finalize Presale: Fee already active.");
        require(!isSwapEnabled, "Finalize Presale: Swap already enabled.");
        require(!isTradeEnabled, "Finalize Presale: Trade already enabled.");
        isFeeActive = true;
        isSwapEnabled = true;
        isTradeEnabled = true;
    }

    function lockFees() external onlyOwner {
        require(!isFeeLocked, "Lock Fees: All fees were already locked.");
        isFeeLocked = true;
    }

    function redeemAllMarketingFee() external {
        uint256 amountToRedeem = marketingFeeCollected - marketingFeeRedeemed;
        
        _redeemMarketingFee(amountToRedeem);
    }

    function redeemPartialMarketingFee(uint256 amountToRedeem) external {
        require(amountToRedeem <= marketingFeeCollected - marketingFeeRedeemed, "Redeem Partial Marketing Fee: Insufficient marketing fee collected.");
        
        _redeemMarketingFee(amountToRedeem);
    }

    function _redeemMarketingFee(uint256 amountToRedeem) internal swapping { 
        marketingFeeRedeemed += amountToRedeem;
        totalFeeRedeemed += amountToRedeem;
 
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();

        _approve(address(this), address(router), amountToRedeem);

        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            amountToRedeem,
            0,
            path,
            marketingReceiver,
            block.timestamp
        );
    }

    /* Check */

    function IsHAMSTERSRC() external pure returns (bool) {
        return ISHAMSTERSRC;
    }

    function circulatingSupply() external view returns (uint256) {
        return totalSupply() - balanceOf(DEAD) - balanceOf(ZERO);
    }

    /* Update */

    function updateRouter(address newRouter) external onlyOwner {
        require(address(router) != newRouter, "Update Router: This is the current router address.");
        address oldRouter = address(router);
        router = IRouter(newRouter);
        emit UpdateRouter(oldRouter, newRouter, block.timestamp);
        pair = IFactory(router.factory()).createPair(address(this), router.WETH());
    }

    function updateMinSwap(uint256 newMinSwap) external onlyOwner {
        require(minSwap != newMinSwap, "Update Min Swap: This is the current value of min swap.");
        uint256 oldMinSwap = minSwap;
        minSwap = newMinSwap;
        emit UpdateMinSwap(oldMinSwap, newMinSwap, block.timestamp);
    }

    function updateMarketingBuyFee(uint256 newMarketingFee) external onlyOwner {
        require(!isFeeLocked, "Update Marketing Buy Fee: All buy fees were locked and cannot be updated.");
        require(newMarketingFee <= 1000, "Update Marketing Buy Fee: Total fees cannot exceed 10%.");
        uint256 oldMarketingFee = buyMarketingFee;
        buyMarketingFee = newMarketingFee;
        emit UpdateMarketingBuyFee(oldMarketingFee, newMarketingFee, msg.sender, block.timestamp);
    }

    function updateMarketingSellFee(uint256 newMarketingFee) external onlyOwner {
        require(!isFeeLocked, "Update Marketing Sell Fee: All sell fees were locked and cannot be updated.");
        require(newMarketingFee <= 1000, "Update Marketing Sell Fee: Total fees cannot exceed 10%.");
        uint256 oldMarketingFee = sellMarketingFee;
        sellMarketingFee = newMarketingFee;
        emit UpdateMarketingSellFee(oldMarketingFee, newMarketingFee, msg.sender, block.timestamp);
    }

    function updateMarketingTransferFee(uint256 newMarketingFee) external onlyOwner {
        require(!isFeeLocked, "Update Marketing Transfer Fee: All transfer fees were locked and cannot be updated.");
        require(newMarketingFee <= 1000, "Update Marketing Transfer Fee: Total fees cannot exceed 10%.");
        uint256 oldMarketingFee = transferMarketingFee;
        transferMarketingFee = newMarketingFee;
        emit UpdateMarketingTransferFee(oldMarketingFee, newMarketingFee, msg.sender, block.timestamp);
    }

    function updateFeeActive(bool newStatus) external onlyOwner {
        require(isFeeActive != newStatus, "Update Fee Active: This is the current state for the fee.");
        bool oldStatus = isFeeActive;
        isFeeActive = newStatus;
        emit UpdateFeeActive(oldStatus, newStatus, block.timestamp);
    }

    function updateSwapEnabled(bool newStatus) external onlyOwner {
        require(isSwapEnabled != newStatus, "Update Swap Enabled: This is the current state for the swap.");
        bool oldStatus = isSwapEnabled;
        isSwapEnabled = newStatus;
        emit UpdateSwapEnabled(oldStatus, newStatus, block.timestamp);
    }

    function updateMarketingReceiver(address newMarketingReceiver) external onlyOwner {
        require(marketingReceiver != newMarketingReceiver, "Update Marketing Receiver: This is the current marketing receiver address.");
        address oldMarketingReceiver = marketingReceiver;
        marketingReceiver = newMarketingReceiver;
        emit UpdateMarketingReceiver(oldMarketingReceiver, newMarketingReceiver, block.timestamp);
    }

    function setExcludeFromFees(address user, bool status) external onlyOwner {
        require(isExcludeFromFees[user] != status, "Set Exclude From Fees: This is the current state for this address.");
        isExcludeFromFees[user] = status;
    }

    function setAllowTrade(address user, bool status) external onlyOwner {
        require(allowTrade[user] != status, "Set Allow Trade: This is the current state for this address.");
        allowTrade[user] = status;
    }

    /* Fee */

    function takeMarketingBuyFee(address from, uint256 amount) internal swapping returns (uint256) {
        uint256 feeAmount = amount * buyMarketingFee / FEEDENOMINATOR;
        uint256 newAmount = amount - feeAmount;
        tallyBuyFee(from, feeAmount, buyMarketingFee);
        return newAmount;
    }

    function takeMarketingSellFee(address from, uint256 amount) internal swapping returns (uint256) {
        uint256 feeAmount = amount * sellMarketingFee / FEEDENOMINATOR;
        uint256 newAmount = amount - feeAmount;
        tallySellFee(from, feeAmount, sellMarketingFee);
        return newAmount;
    }

    function takeMarketingTransferFee(address from, uint256 amount) internal swapping returns (uint256) {
        uint256 feeAmount = amount * transferMarketingFee / FEEDENOMINATOR;
        uint256 newAmount = amount - feeAmount;
        tallyTransferFee(from, feeAmount, transferMarketingFee);
        return newAmount;
    }

    function tallyBuyFee(address from, uint256 amount, uint256 fee) internal swapping {
        uint256 collectMarketing = amount * buyMarketingFee / fee;
        tallyCollection(collectMarketing, amount);
        
        _balances[from] -= amount;
        _balances[address(this)] += amount;
    }

    function tallySellFee(address from, uint256 amount, uint256 fee) internal swapping {
        uint256 collectMarketing = amount * sellMarketingFee / fee;
        tallyCollection(collectMarketing, amount);
        
        _balances[from] -= amount;
        _balances[address(this)] += amount;
    }

    function tallyTransferFee(address from, uint256 amount, uint256 fee) internal swapping {
        uint256 collectMarketing = amount * transferMarketingFee / fee;
        tallyCollection(collectMarketing, amount);

        _balances[from] -= amount;
        _balances[address(this)] += amount;
    }

    function tallyCollection(uint256 collectMarketing, uint256 amount) internal swapping {
        marketingFeeCollected += collectMarketing;
        totalFeeCollected += amount;

    }

    function autoRedeem(uint256 amountToRedeem) public swapping {  
        uint256 marketingToRedeem = marketingFeeCollected - marketingFeeRedeemed;
        uint256 totalToRedeem = totalFeeCollected - totalFeeRedeemed;
        uint256 marketingFeeDistribution = amountToRedeem * marketingToRedeem / totalToRedeem;

        marketingFeeRedeemed += marketingFeeDistribution;
        totalFeeRedeemed += amountToRedeem;

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();

        _approve(address(this), address(router), amountToRedeem);
    
        emit AutoRedeem(marketingFeeDistribution, amountToRedeem, block.timestamp);

        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            marketingFeeDistribution,
            0,
            path,
            marketingReceiver,
            block.timestamp
        );
    }

    /* Buyback */

    function triggerZeusBuyback(uint256 amount) external onlyOwner {
        buyTokens(amount, DEAD);
    }

    function buyTokens(uint256 amount, address to) internal swapping {
        require(msg.sender != DEAD, "Buy Tokens: Dead address cannot call this function.");
        address[] memory path = new address[](2);
        path[0] = router.WETH();
        path[1] = address(this);

        router.swapExactETHForTokensSupportingFeeOnTransferTokens{
            value: amount
        }(0, path, to, block.timestamp);
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
        require(account != address(0), "ERC20: mint to the zero address");

        _totalSupply += amount;
        unchecked {
            _balances[account] += amount;
        }
        emit Transfer(address(0), account, amount);
    }

    function _approve(address provider, address spender, uint256 amount) internal virtual {
        require(provider != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

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
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        if(!isTradeEnabled) {
            require(allowTrade[from], "HAMSTERSRC: Trade not yet enabled.");
        }

        if (inSwap || isExcludeFromFees[from]) {
            return _basicTransfer(from, to, amount);
        }

        if (from != pair && isSwapEnabled && totalFeeCollected - totalFeeRedeemed >= minSwap) {
            autoRedeem(minSwap);
        }

        uint256 newAmount = amount;

        if (isFeeActive && !isExcludeFromFees[from]) {
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
        if (from == pair && (buyMarketingFee > 0)) {
            return takeMarketingBuyFee(from, amount);
        }
        if (to == pair && (sellMarketingFee > 0)) {
            return takeMarketingSellFee(from, amount);
        }
        if (from != pair && to != pair && (transferMarketingFee > 0)) {
            return takeMarketingTransferFee(from, amount);
        }
        return amount;
    }
}