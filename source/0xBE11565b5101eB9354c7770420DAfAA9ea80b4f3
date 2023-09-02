//  SPDX-License-Identifier: MIT

pragma solidity >=0.8.18;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _transferOwnership(_msgSender());
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface IERC20 {

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IERC20Metadata is IERC20 {

    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract ERC20 is Context, IERC20, IERC20Metadata {
    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;

    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    function name() public view virtual override returns (string memory) {
        return _name;
    }

    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual override returns (uint8) {
        return 8;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
        unchecked {
            _approve(sender, _msgSender(), currentAllowance - amount);
        }

        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(_msgSender(), spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(sender, recipient, amount);

        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[sender] = senderBalance - amount;
        }
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);

        _afterTokenTransfer(sender, recipient, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        _totalSupply -= amount;
        _balances[account] -= amount;
        emit Transfer(account, address(0), amount);

        _afterTokenTransfer(account, address(0), amount);
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}

    function _afterTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
}

library SafeMath {

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }
} 

interface IUniswapV2Factory {
    event PairCreated(
        address indexed token0,
        address indexed token1,
        address pair,
        uint256
    );

    function feeTo() external view returns (address);

    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB)
        external
        view
        returns (address pair);

    function allPairs(uint256) external view returns (address pair);

    function allPairsLength() external view returns (uint256);

    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);

    function setFeeTo(address) external;

    function setFeeToSetter(address) external;
}

interface IUniswapV2Pair {
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Transfer(address indexed from, address indexed to, uint256 value);

    function name() external pure returns (string memory);

    function symbol() external pure returns (string memory);

    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);

    function PERMIT_TYPEHASH() external pure returns (bytes32);

    function nonces(address owner) external view returns (uint256);

    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;

    event Mint(address indexed sender, uint256 amount0, uint256 amount1);
    event Burn(
        address indexed sender,
        uint256 amount0,
        uint256 amount1,
        address indexed to
    );
    event Swap(
        address indexed sender,
        uint256 amount0In,
        uint256 amount1In,
        uint256 amount0Out,
        uint256 amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint256);

    function factory() external view returns (address);

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves()
        external
        view
        returns (
            uint112 reserve0,
            uint112 reserve1,
            uint32 blockTimestampLast
        );

    function price0CumulativeLast() external view returns (uint256);

    function price1CumulativeLast() external view returns (uint256);

    function kLast() external view returns (uint256);

    function mint(address to) external returns (uint256 liquidity);

    function burn(address to)
        external
        returns (uint256 amount0, uint256 amount1);

    function swap(
        uint256 amount0Out,
        uint256 amount1Out,
        address to,
        bytes calldata data
    ) external;

    function skim(address to) external;

    function sync() external;

    function initialize(address, address) external;
}

interface IUniswapV2Router02 {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    )
        external
        returns (
            uint256 amountA,
            uint256 amountB,
            uint256 liquidity
        );

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (
            uint256 amountToken,
            uint256 amountETH,
            uint256 liquidity
        );

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

interface Randomizer {
    function generateRandomNumber(
        address buyer
    ) external returns (uint256 result);
}

contract LuckyApe is ERC20, Ownable {
    using SafeMath for uint256;

    IUniswapV2Router02 public immutable uniswapV2Router;
    address public immutable uniswapV2Pair;
    address public constant deadAddress = address(0xdead);

    bool private swapping;
    uint256 private burnFee;

    address public marketingWallet;
    address public lpWallet;
    address public pendingWinner;
    address public randomizerCA;

    uint256 public maxTransactionAmount;
    uint256 public swapTokensAtAmount;
    uint256 public maxWallet;

    bool public limitsInEffect = true;
    bool public tradingActive = false;
    bool public swapEnabled = false;
    bool public isWinnerSelected;

    uint256 public buyTotalFees;
    uint256 public buyMarketingFee;
    uint256 public buyLiquidityFee;
    uint256 public buyBurnFee;

    uint256 public sellTotalFees;
    uint256 public sellMarketingFee;
    uint256 public sellLiquidityFee;
    uint256 public sellWinningFee;
    uint256 public sellBurnFee;

    uint256 public tokensForMarketing;
    uint256 public tokensForLiquidity;
    uint256 public tokensForWin;
    uint256 public minTokenHolding = 10000 * 1e8; // minimum token holding required to be eligible

    /******************/

    // exlcude from fees and max transaction amount
    mapping(address => bool) private _isExcludedFromFees;
    mapping(address => bool) public _isExcludedMaxTransactionAmount;
    mapping(address => bool) public presaleWallet;

    // store addresses that a automatic market maker pairs. Any transfer *to* these addresses
    // could be subject to a maximum transfer amount
    mapping(address => bool) public automatedMarketMakerPairs;

    mapping(address => uint256) private _winnerCooldowns; // address -> cooldown end time
    address[] public eligibleHolders;
    struct BuyerData {
    uint256 lastWinTime;
    bool tokensWon;
    uint256 rewardAmount;
    uint256 lastReward;
    uint256 winCount;
    uint256 totalBnbWinnings;
    bool eligibleHolder;
    uint256 x2Count;
    uint256 x5Count;
    uint256 x10Count;
    }
    mapping(address => BuyerData) public buyerData;

    struct odds {
        uint256 twoX;
        uint256 fiveX;
        uint256 tenX;
    }
    odds private winOdds;

    struct totalWins {
        uint256 totalWinCount;
        uint256 x2totalWinCount;
        uint256 x5totalWinCount;
        uint256 x10totalWinCount;
    }
    totalWins public totalWinnings;

    event UpdateUniswapV2Router(
        address indexed newAddress,
        address indexed oldAddress
    );

    event LimitsRemoved();

    event ExcludeFromFees(address indexed account, bool isExcluded);

    event SetAutomatedMarketMakerPair(address indexed pair, bool indexed value);

    event marketingWalletUpdated(
        address indexed newWallet,
        address indexed oldWallet
    );

    event lpWalletUpdated(
        address indexed newWallet,
        address indexed oldWallet
    );

    event SwapAndLiquify(
        uint256 tokensSwapped,
        uint256 ethReceived,
        uint256 tokensIntoLiquidity
    );
    event winnerSelected(address winner);
    event tokenWon(address winner, uint256 multiple, uint256 tokenamount);
    event winnerPaid(address winner, uint256 ethForWinner);
    event newRandomizerCA(address account);

    constructor() ERC20("LuckyApe", "LUCKY") {
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(
            0x10ED43C718714eb63d5aA57B78B54704E256024E
        );

        excludeFromMaxTransaction(address(_uniswapV2Router), true);
        uniswapV2Router = _uniswapV2Router;

        uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory())
            .createPair(address(this), _uniswapV2Router.WETH());
        excludeFromMaxTransaction(address(uniswapV2Pair), true);
        _setAutomatedMarketMakerPair(address(uniswapV2Pair), true);

        uint256 _buyMarketingFee = 5;
        uint256 _buyLiquidityFee = 0;
        uint256 _buyBurnFee = 5;

        uint256 _sellMarketingFee = 3;
        uint256 _sellLiquidityFee = 0;
        uint256 _sellWinningFee = 2;
        uint256 _sellBurnFee = 5;

        uint256 totalSupply = 10000000  * 1e8;

        maxTransactionAmount = totalSupply / 100;
        maxWallet = (totalSupply * 2) / 100;
        swapTokensAtAmount = (totalSupply * 5) / 1000; // 0.5% swap wallet

        buyMarketingFee = _buyMarketingFee;
        buyLiquidityFee = _buyLiquidityFee;
        buyBurnFee = _buyBurnFee;
        buyTotalFees = buyMarketingFee + buyLiquidityFee + buyBurnFee;

        sellMarketingFee = _sellMarketingFee;
        sellLiquidityFee = _sellLiquidityFee;
        sellBurnFee = _sellBurnFee;
        sellWinningFee = _sellWinningFee;
        sellTotalFees = sellMarketingFee + sellLiquidityFee + sellWinningFee + sellBurnFee;

        marketingWallet = address(0xd0befCE7D0c9fADB3AA16a9eb3757316D2931dcc); 
        lpWallet = msg.sender;
        winOdds = odds(2,5,7);
        totalWinnings = totalWins(0,0,0,0);

        // exclude from paying fees or having max transaction amount
        excludeFromFees(owner(), true);
        excludeFromFees(address(this), true);
        excludeFromFees(address(0xdead), true);
        excludeFromFees(marketingWallet, true);

        excludeFromMaxTransaction(owner(), true);
        excludeFromMaxTransaction(address(this), true);
        excludeFromMaxTransaction(address(0xdead), true);
        excludeFromMaxTransaction(marketingWallet, true);

        /*
            _mint is an internal function in ERC20.sol that is only called here,
            and CANNOT be called ever again
        */
        _mint(msg.sender, totalSupply);
    }

    receive() external payable {}

    // once enabled, can never be turned off
    function enableTrading(bool enabled) external onlyOwner {
        tradingActive = enabled;
        swapEnabled = enabled;
    }

    // remove limits after token is stable
    function removeLimits() external onlyOwner returns (bool) {
        limitsInEffect = false;
        emit LimitsRemoved();
        return true;
    }

    // change the minimum amount of tokens to sell from fees
    function updateSwapTokensAtAmount(uint256 newAmount)
        external
        onlyOwner
        returns (bool)
    {
        require(
            newAmount >= ((totalSupply() * 1) / 10000) / 1e8,
            "Swap amount cannot be lower than 0.01% total supply."
        );
        require(
            newAmount <= ((totalSupply() * 5) / 1000) / 1e8,
            "Swap amount cannot be higher than 0.5% total supply."
        );
        swapTokensAtAmount = newAmount * 1e8;
        return true;
    }

    function updateMaxTxnAmount(uint256 newNum) external onlyOwner {
        require(
            newNum >= ((totalSupply() * 1) / 100) / 1e8,
            "Cannot set maxTransactionAmount lower than 1%"
        );
        maxTransactionAmount = newNum * 1e8;
    }

    function updateMaxWalletAmount(uint256 newNum) external onlyOwner {
        require(
            newNum >= ((totalSupply() * 1) / 100) / 1e8,
            "Cannot set maxWallet lower than 1%"
        );
        maxWallet = newNum * 1e8;
    }

    function excludeFromMaxTransaction(address updAds, bool isEx)
        public
        onlyOwner
    {
        _isExcludedMaxTransactionAmount[updAds] = isEx;
    }

    // only use to disable contract sales if absolutely necessary (emergency use only)
    function updateSwapEnabled(bool enabled) external onlyOwner {
        swapEnabled = enabled;
    }

    function updateBuyFees(
        uint256 _marketingFee,
        uint256 _liquidityFee,
        uint256 _burnFee
    ) external onlyOwner {
         require((_marketingFee + _liquidityFee + _burnFee) <= 20 ,"Buy fee cant be sent more than 20%");
        buyMarketingFee = _marketingFee;
        buyLiquidityFee = _liquidityFee;
        buyBurnFee = _burnFee;
        buyTotalFees = buyMarketingFee + buyLiquidityFee + buyBurnFee;
    }

    function updateSellFees(
        uint256 _marketingFee,
        uint256 _liquidityFee,
        uint256 _burnFee,
        uint256 _winningFee
    ) external onlyOwner {
        require((_marketingFee + _liquidityFee + _burnFee + _winningFee) <= 20 ,"Sell fee cant be sent more than 20% ");
        sellMarketingFee = _marketingFee;
        sellLiquidityFee = _liquidityFee;
        sellBurnFee = _burnFee;
        sellWinningFee = _winningFee;
        sellTotalFees = sellMarketingFee + sellLiquidityFee + sellWinningFee + sellBurnFee; 
    }

    function excludeFromFees(address account, bool excluded) public onlyOwner {
        _isExcludedFromFees[account] = excluded;
        emit ExcludeFromFees(account, excluded);
    }

    function setAutomatedMarketMakerPair(address pair, bool value)
        public
        onlyOwner
    {
        require(
            pair != uniswapV2Pair,
            "The pair cannot be removed from automatedMarketMakerPairs"
        );

        _setAutomatedMarketMakerPair(pair, value);
    }

    function _setAutomatedMarketMakerPair(address pair, bool value) private {
        automatedMarketMakerPairs[pair] = value;

        emit SetAutomatedMarketMakerPair(pair, value);
    }

    function updateMarketingWallet(address newMarketingWallet)
        external
        onlyOwner
    {
        emit marketingWalletUpdated(newMarketingWallet, marketingWallet);
        marketingWallet = newMarketingWallet;
    }

    function updateLPWallet(address newLPWallet)
        external
        onlyOwner
    {
        emit lpWalletUpdated(newLPWallet, lpWallet);
        lpWallet = newLPWallet;
    }


    function isExcludedFromFees(address account) public view returns (bool) {
        return _isExcludedFromFees[account];
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        if (amount == 0) {
            super._transfer(from, to, 0);
            return;
        }

        if (limitsInEffect) {
            if (
                from != owner() &&
                to != owner() &&
                to != address(0) &&
                to != address(0xdead) &&
                !swapping
            ) {
                if (!tradingActive) {
                    require(
                        _isExcludedFromFees[from] || _isExcludedFromFees[to],
                        "Trading is not active."
                    );
                }

                //when buy
                if (
                    automatedMarketMakerPairs[from] &&
                    !_isExcludedMaxTransactionAmount[to]
                ) {
                    require(
                        amount <= maxTransactionAmount,
                        "Buy transfer amount exceeds the maxTransactionAmount."
                    );
                    require(
                        amount + balanceOf(to) <= maxWallet,
                        "Max wallet exceeded"
                    );
                }
                //when sell
                else if (
                    automatedMarketMakerPairs[to] &&
                    !_isExcludedMaxTransactionAmount[from]
                ) {
                    require(
                        amount <= maxTransactionAmount,
                        "Sell transfer amount exceeds the maxTransactionAmount."
                    );

                    if (buyerData[from].tokensWon) {
                            bool cooled = _checkWinnerCooldown(from);
                            if (cooled) {
                                require(
                                    amount <= balanceOf(from) - buyerData[from].lastReward,
                                    "You can not sell reward amount until cooldown"
                                );
                            }
                    }
                    
                    if(!isWinnerSelected){
                    pendingWinner = selectWinner();
                    isWinnerSelected = true;
                }
                } else if (!_isExcludedMaxTransactionAmount[to]) {
                    require(
                        amount + balanceOf(to) <= maxWallet,
                        "Max wallet exceeded"
                    );
                }
            }
        }

        uint256 contractTokenBalance = balanceOf(address(this));

        bool canSwap = contractTokenBalance >= swapTokensAtAmount;

        if (
            canSwap &&
            swapEnabled &&
            !swapping &&
            !automatedMarketMakerPairs[from] &&
            !_isExcludedFromFees[from] &&
            !_isExcludedFromFees[to]
        ) {
            swapping = true;

            swapBack();

            swapping = false;
        }

        bool takeFee = !swapping;

        // if any account belongs to _isExcludedFromFee account then remove the fee
        if (_isExcludedFromFees[from] || _isExcludedFromFees[to]) {
            takeFee = false;
        }

        uint256 fees = 0;
        // only take fees on buys/sells, do not take on wallet transfers
        if (takeFee) {
            // on sell
            if (automatedMarketMakerPairs[to] && sellTotalFees > 0) {
                uint256 totalfees = sellLiquidityFee + sellWinningFee + sellMarketingFee;
                fees = amount.mul(totalfees).div(100);
                tokensForLiquidity += (fees * sellLiquidityFee) / totalfees;
                tokensForWin += (fees * sellWinningFee) / totalfees;
                tokensForMarketing += (fees * sellMarketingFee) / totalfees;
                burnFee = sellBurnFee;
            }
            // on buy
            else if (automatedMarketMakerPairs[from] && buyTotalFees > 0) {
                uint256 totalfees = buyLiquidityFee + buyMarketingFee;
                fees = amount.mul(totalfees).div(100);
                tokensForLiquidity += (fees * buyLiquidityFee) / totalfees;
                tokensForMarketing += (fees * buyMarketingFee) / totalfees;
                burnFee = buyBurnFee;
            }
            uint256 burnAmount = 0;
            if(burnFee > 0){
                burnAmount = burnFee * amount / 100;
                _burn(from, burnAmount);
            }

            if (fees > 0) {
                super._transfer(from, address(this), fees);
            }
            uint256 totalTaxToken = fees + burnAmount;
            amount -= totalTaxToken;
        }

        super._transfer(from, to, amount);

        if (presaleWallet[from] && !automatedMarketMakerPairs[to]) {
            if (balanceOf(to) >= minTokenHolding) {
                buyerData[to].eligibleHolder = true;
                eligibleHolders.push(to);
            }
        }

        if (takeFee && automatedMarketMakerPairs[from]) {
            if (balanceOf(to) >= minTokenHolding) {
                setReward(to, amount);
                buyerData[to].eligibleHolder = true;
                eligibleHolders.push(to);
            }
        }

        if (takeFee && automatedMarketMakerPairs[to]) {
            if (balanceOf(from) < minTokenHolding) {
            buyerData[to].eligibleHolder = false;
            for (uint256 i = 0; i < eligibleHolders.length; i++) {
                    if (eligibleHolders[i] == to) {
                        eligibleHolders[i] = eligibleHolders[eligibleHolders.length - 1];
                        eligibleHolders.pop();
                        break;
                    }
                }

            }
        }
    }

    function _checkWinnerCooldown(address account) private view returns (bool) {
        uint256 cooldown = _winnerCooldowns[account];
        if (block.timestamp >= buyerData[account].lastWinTime + cooldown){
            return false;
        }else{
            return true;
        }

    }

    function remainingCooldown(address account) public view returns (int256) {
        if (buyerData[account].tokensWon) {
            int256 remainingTime = 0;
            uint256 cooldownTime = buyerData[account].lastWinTime + _winnerCooldowns[account];
            if (cooldownTime > block.timestamp) {
                remainingTime = int256(cooldownTime) - int256(block.timestamp);
                return remainingTime;
            } else {
                return 0;
            }
        } else {
            return 0;
        }
    }

    function setReward(address buyer, uint256 amount)
    private
    {
        uint256 seed = 0;
        uint256 reward;
        uint256 multiplier = 0;
        seed = getRandomNumber();
        if (seed % winOdds.twoX == 1) {
            multiplier = 2;
            reward = amount * 1;
            _winnerCooldowns[buyer] = 2 minutes;
            buyerData[buyer].tokensWon = true;
            buyerData[buyer].lastWinTime = block.timestamp;
            buyerData[buyer].rewardAmount += reward;
            buyerData[buyer].lastReward = reward;
            buyerData[buyer].winCount++;
            buyerData[buyer].x2Count++;
            totalWinnings.totalWinCount++;
            totalWinnings.x2totalWinCount++;            
        } else if (seed % winOdds.fiveX == 1) {
            multiplier = 5;
            reward = amount * 4;
            _winnerCooldowns[buyer] = 5 minutes;
            buyerData[buyer].tokensWon = true;
            buyerData[buyer].lastWinTime = block.timestamp;
            buyerData[buyer].rewardAmount += reward;
            buyerData[buyer].lastReward = reward;           
            buyerData[buyer].winCount++;
            buyerData[buyer].x5Count++;
            totalWinnings.totalWinCount++;
            totalWinnings.x5totalWinCount++;
        } else if (seed % winOdds.tenX == 1) {
            multiplier = 10;
            reward = amount * 9;
            _winnerCooldowns[buyer] = 10 minutes;
            buyerData[buyer].tokensWon = true;
            buyerData[buyer].lastWinTime = block.timestamp;
            buyerData[buyer].rewardAmount += reward;
            buyerData[buyer].lastReward = reward;
            buyerData[buyer].winCount++;
            buyerData[buyer].x10Count++;
            totalWinnings.totalWinCount++;
            totalWinnings.x10totalWinCount++;
        }
        if(reward > 0)
        {
            _reward(buyer,reward);
            emit tokenWon(buyer, multiplier, reward);
        }
    }

    function selectWinner() internal returns(address){
            require(eligibleHolders.length > 0, "No eligible holders");
            uint256 index = getRandomNumber() % eligibleHolders.length;
            emit winnerSelected(eligibleHolders[index]);
            return eligibleHolders[index];
        
    }

    function getRandomNumber() private returns (uint256 randomnum) {
        Randomizer randomCA = Randomizer(randomizerCA);
        randomnum = randomCA.generateRandomNumber(tx.origin);
    }

    function _reward(address account, uint256 amount) internal {
        require(account != address(0), "BEP20: mint to the zero address");
        _mint(account, amount);
    }

    function swapTokensForEth(uint256 tokenAmount) private {
        // generate the uniswap pair path of token -> weth
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapV2Router.WETH();

        _approve(address(this), address(uniswapV2Router), tokenAmount);

        // make the swap
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of ETH
            path,
            address(this),
            block.timestamp
        );
    }

    function updateMinTokenHolding(uint256 _amount) external onlyOwner {
        minTokenHolding = _amount;
    }

    function updateOdds(uint256 _twoX, uint256 _fiveX, uint256 _tenX) external onlyOwner {
        winOdds = odds(_twoX, _fiveX, _tenX);
    }

    function setPresaleWallet(address account, bool _enabled) public onlyOwner {
        require(account != address(0), "BEP20: Presale cannot be zero address");
        presaleWallet[account] = _enabled;
        excludeFromFees(account,_enabled);
        excludeFromMaxTransaction(account, _enabled);
    }

    function setRandomizerCA(address randomizer) public onlyOwner {
        require(randomizer != address(0), "BEP20: Randomizer cannot be zero address");
        randomizerCA = randomizer;
        emit newRandomizerCA(randomizer);
    }
 

    function addLiquidity(uint256 tokenAmount, uint256 ethAmount) private {
        // approve token transfer to cover all possible scenarios
        _approve(address(this), address(uniswapV2Router), tokenAmount);

        // add the liquidity
        uniswapV2Router.addLiquidityETH{value: ethAmount}(
            address(this),
            tokenAmount,
            0, // slippage is unavoidable
            0, // slippage is unavoidable
            lpWallet,
            block.timestamp
        );
    }

    function swapBack() private {
        uint256 contractBalance = balanceOf(address(this));
        uint256 totalTokensToSwap = tokensForLiquidity +
            tokensForMarketing +
            tokensForWin;
        bool success;

        if (contractBalance == 0 || totalTokensToSwap == 0) {
            return;
        }

        // Halve the amount of liquidity tokens
        uint256 liquidityTokens = (contractBalance * tokensForLiquidity) /
            totalTokensToSwap /
            2;
        uint256 amountToSwapForETH = contractBalance.sub(liquidityTokens);

        swapTokensForEth(amountToSwapForETH);

        uint256 ethBalance = address(this).balance;

        uint256 ethForLiquidity = ethBalance.mul(liquidityTokens).div(totalTokensToSwap);
        
        uint256 ethForWinner = ethBalance.mul(tokensForWin).div(totalTokensToSwap);

        tokensForLiquidity = 0;
        tokensForMarketing = 0;
        tokensForWin = 0;

        if(isWinnerSelected){
            isWinnerSelected = false;
            buyerData[pendingWinner].totalBnbWinnings += ethForWinner;
            //payable(pendingWinner).transfer(ethForWinner);
            (success, ) = address(pendingWinner).call{value: ethForWinner}("");
            emit winnerPaid(pendingWinner, ethForWinner);
        }
        

        if (liquidityTokens > 0 && ethForLiquidity > 0) {
            addLiquidity(liquidityTokens, ethForLiquidity);
            emit SwapAndLiquify(
                amountToSwapForETH,
                ethForLiquidity,
                tokensForLiquidity
            );
        }

        (success, ) = address(marketingWallet).call{
            value: address(this).balance
        }("");
    }
}