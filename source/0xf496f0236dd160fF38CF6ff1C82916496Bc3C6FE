// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
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

interface IBEP20 {
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

interface IBEP20Metadata is IBEP20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
}

contract BEP20 is Context, IBEP20, IBEP20Metadata {
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
        return 18;
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
        require(currentAllowance >= amount, "BEP20: transfer amount exceeds allowance");
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
        require(currentAllowance >= subtractedValue, "BEP20: decreased allowance below zero");
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
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");

        _beforeTokenTransfer(sender, recipient, amount);

        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "BEP20: transfer amount exceeds balance");
        unchecked {
            _balances[sender] = senderBalance - amount;
        }
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);

        _afterTokenTransfer(sender, recipient, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "BEP20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "BEP20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "BEP20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
        }
        _totalSupply -= amount;

        emit Transfer(account, address(0), amount);

        _afterTokenTransfer(account, address(0), amount);
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");

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
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}

interface IPancakeswapV2Factory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
}

interface IPancakeswapV2Router02 {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

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

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

contract Heist2 is BEP20, Ownable {
    using SafeMath for uint256;

    IPancakeswapV2Router02 public immutable pancakeswapV2Router;
    address public immutable pancakeswapV2Pair;

    bool private swapping;

    address public lpWallet;
    address public marketingWallet;
    address public buybackWallet;
    address public constant BURN_WALLET = 0x000000000000000000000000000000000000dEaD;

    uint256 public maxTransactionAmount;
    uint256 public swapTokensAtAmount;
    uint256 public maxWallet;

    bool public limitsInEffect = true;
    bool public tradingActive = true;
    bool public swapEnabled = true;

    uint256 public buyTotalFees;
    uint256 public buyLiquidityFee;
    uint256 public buyMarketingFee;
    uint256 public buyBuybackFee;
    uint256 public buyBurnFee;

    uint256 public sellTotalFees;
    uint256 public sellLiquidityFee;
    uint256 public sellMarketingFee;
    uint256 public sellBuybackFee;
    uint256 public sellBurnFee;

	uint256 public tokensForLiquidity;
    uint256 public tokensForMarketing;
    uint256 public tokensForBuyback;
    uint256 public tokensForBurn;
    

    // exclude from fees and max transaction amount
    mapping(address => bool) private _isExcludedFromFees;
    mapping(address => bool) public _isExcludedMaxTransactionAmount;

    // store addresses that a automatic market maker pairs. Any transfer *to* these addresses
    // could be subject to a maximum transfer amount
    mapping(address => bool) public automatedMarketMakerPairs;

    event ExcludeFromFees(address indexed account, bool isExcluded);

    event SetAutomatedMarketMakerPair(address indexed pair, bool indexed value);

    event SwapAndLiquify(
        uint256 tokensSwapped,
        uint256 ethReceived,
        uint256 tokensIntoLiquidity
    );

    constructor() BEP20("HEIST2.0", "HES") {
        IPancakeswapV2Router02 _pancakeswapV2Router = IPancakeswapV2Router02(
            0x10ED43C718714eb63d5aA57B78B54704E256024E //Pancakeswap Router V2
        );

        excludeFromMaxTransaction(address(_pancakeswapV2Router), true);
        pancakeswapV2Router = _pancakeswapV2Router;

        pancakeswapV2Pair = IPancakeswapV2Factory(_pancakeswapV2Router.factory())
            .createPair(address(this), _pancakeswapV2Router.WETH());
        excludeFromMaxTransaction(address(pancakeswapV2Pair), true);
        _setAutomatedMarketMakerPair(address(pancakeswapV2Pair), true);

        

        uint256 totalSupply =  420e15 * 1e9; //420 quadrillion (420,000,000,000,000,000)
        uint256 burnSupply = (totalSupply * 60) / 100;

        uint256 remainingSupply = totalSupply - burnSupply;

        maxTransactionAmount = (remainingSupply * 2) / 100; 
        maxWallet = (remainingSupply * 2) / 100; 
        swapTokensAtAmount = (remainingSupply * 10) / 50000; 

        buyLiquidityFee = 2;
        buyMarketingFee = 5;
        buyBurnFee = 2;
        buyBuybackFee =1;
        buyTotalFees = 10;

        sellLiquidityFee = 2;
        sellMarketingFee = 5;
        sellBurnFee = 2;
        sellBuybackFee = 1;
        sellTotalFees = 10;

        lpWallet = address(0x28F5913612E98688E38E5A83eE2759E2292e7168);
        marketingWallet = address(0xF6e348a7eF7413F8B7b2B6f545Ec757C475D4128); 
        buybackWallet = address(0xf21D4342bAdd386a1325c22937c297042A79452e);

        // exclude from paying fees or having max transaction amount
        excludeFromFees(owner(), true);
        excludeFromFees(address(this), true);
        excludeFromFees(BURN_WALLET, true);
        excludeFromFees(0xc911c420C6D6a8bc8441DD7BcbFd455Dd2F9893f, true);
        excludeFromMaxTransaction(owner(), true);
        excludeFromMaxTransaction(address(this), true);
        excludeFromMaxTransaction(BURN_WALLET, true);

       

        _mint(msg.sender, totalSupply - burnSupply);
        _mint(BURN_WALLET, burnSupply);
    }

    receive() external payable {}

    function decimals () public pure override returns (uint8) {
        return 9;
    }

    // once enabled, can never be turned off
    function enableTrading() external onlyOwner {
        tradingActive = true;
        swapEnabled = true;
    }

    function updateBuyFees(uint256 _buyLiquidityFee, uint256 _buyMarketingFee, uint256 _buyBurnFee, uint256 _buyBuybackFee) external onlyOwner {
        buyLiquidityFee = _buyLiquidityFee;
        buyMarketingFee = _buyMarketingFee;
        buyBurnFee = _buyBurnFee;
        buyBuybackFee = _buyBuybackFee;
        buyTotalFees = buyLiquidityFee + buyMarketingFee + buyBuybackFee + buyBuybackFee;
        require (buyTotalFees <= 10, "Max buy fees limit is 10 percent");

    } 

    function updateSellFees(uint256 _sellLiquidityFee, uint256 _sellMarketingFee, uint256 _sellBurnFee, uint256 _sellBuybackFee) external onlyOwner {
        sellLiquidityFee = _sellLiquidityFee;
        sellMarketingFee = _sellMarketingFee;
        sellBurnFee = _sellBurnFee;
        sellBuybackFee = _sellBuybackFee;
        sellTotalFees = sellLiquidityFee + sellMarketingFee + sellBuybackFee + sellBuybackFee;
        require (sellTotalFees <= 10, "Max sell fees limit is 10 percent");

    } 

    function removeLimits() external onlyOwner returns (bool) {
        limitsInEffect = false;
        return true;
    }

    // change the minimum amount of tokens to sell from fees
    function updateSwapTokensAtAmount(uint256 newAmount)
        external
        onlyOwner
        returns (bool)
    {
        require (newAmount >= 1e9 * 1e9, "amount is too low");
       
        swapTokensAtAmount = newAmount;
        return true;
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

    function excludeFromFees(address account, bool excluded) public onlyOwner {
        _isExcludedFromFees[account] = excluded;
        emit ExcludeFromFees(account, excluded);
    }

    function setAutomatedMarketMakerPair(address pair, bool value)
        public
        onlyOwner
    {
        require(
            pair != pancakeswapV2Pair,
            "The pair cannot be removed from automatedMarketMakerPairs"
        );

        _setAutomatedMarketMakerPair(pair, value);
    }


    function _setAutomatedMarketMakerPair(address pair, bool value) private {
        automatedMarketMakerPairs[pair] = value;

        emit SetAutomatedMarketMakerPair(pair, value);
    }

    function isExcludedFromFees(address account) public view returns (bool) {
        return _isExcludedFromFees[account];
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        require(from != address(0), "BEP20: transfer from the zero address");
        require(to != address(0), "BEP20: transfer to the zero address");

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
                fees = amount.mul(sellTotalFees).div(100);
                tokensForLiquidity += (fees * sellLiquidityFee) / sellTotalFees;
                tokensForMarketing += (fees * sellMarketingFee) / sellTotalFees; 
                tokensForBurn = (fees * sellBurnFee) / sellTotalFees;
                tokensForBuyback += (fees * sellBuybackFee) / sellTotalFees;               
            }
            // on buy
            else if (automatedMarketMakerPairs[from] && buyTotalFees > 0) {
                fees = amount.mul(buyTotalFees).div(100);
                tokensForLiquidity += (fees * buyLiquidityFee) / buyTotalFees;
                tokensForMarketing += (fees * buyMarketingFee) / buyTotalFees;
                tokensForBurn = (fees * buyBurnFee) / sellTotalFees;
                tokensForBuyback += (fees * buyBuybackFee) / sellTotalFees; 
            }
           
            if (fees > 0) {
                super._transfer(from, address(this), fees - tokensForBurn);
                if(tokensForBurn > 0){
                    super._transfer(from, BURN_WALLET, tokensForBurn);
                }
            }

            amount -= fees;
        }

        super._transfer(from, to, amount);
    }

    function swapTokensForBnb(uint256 tokenAmount) private {
        // generate the pancakeswap pair path of token -> wbnb
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = pancakeswapV2Router.WETH();

        _approve(address(this), address(pancakeswapV2Router), tokenAmount);

        // make the swap
        pancakeswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of BNB
            path,
            address(this),
            block.timestamp
        );
    }

    function addLiquidity(uint256 tokenAmount, uint256 ethAmount) private {
        // approve token transfer to cover all possible scenarios
        _approve(address(this), address(pancakeswapV2Router), tokenAmount);

        // add the liquidity
        pancakeswapV2Router.addLiquidityETH{value: ethAmount}(
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
        uint256 totalTokensToSwap = tokensForLiquidity + tokensForMarketing + tokensForBuyback;
        bool success;
        bool successTwo;

        if (contractBalance == 0 || totalTokensToSwap == 0) {
            return;
        }
        uint256 maxSwapAmount = swapTokensAtAmount * 25;
        if (contractBalance > maxSwapAmount) {
            contractBalance = maxSwapAmount;
        }

        // Halve the amount of liquidity tokens
        uint256 liquidityTokens = (contractBalance * tokensForLiquidity) / totalTokensToSwap / 2;
        uint256 amountToSwapForBNB = contractBalance.sub(liquidityTokens);

        uint256 initialETHBalance = address(this).balance;

        swapTokensForBnb(amountToSwapForBNB);

        uint256 bnbBalance = address(this).balance.sub(initialETHBalance);
	
        uint256 bnbForMarketing = bnbBalance.mul(tokensForMarketing).div(totalTokensToSwap);
        uint256 bnbForBuyback = bnbBalance.mul(tokensForBuyback).div(totalTokensToSwap);

        uint256 bnbForLiquidity = bnbBalance - bnbForMarketing - bnbForBuyback;

        tokensForLiquidity = 0;
        tokensForMarketing = 0;
        tokensForBuyback = 0;

        if (liquidityTokens > 0 && bnbForLiquidity > 0) {
            addLiquidity(liquidityTokens, bnbForLiquidity);
            emit SwapAndLiquify(
                amountToSwapForBNB,
                bnbForLiquidity,
                tokensForLiquidity
            );
        }
        if(bnbForMarketing > 0){
        (success, ) = address(marketingWallet).call{value: bnbForMarketing}("");
        }
         //there will be no leftover bnb in the contract 
         //buyback wallet will get bnbForBuyback part and any remaining bnb
         if(address(this).balance > 0){
         (successTwo,) = address(buybackWallet).call{value: address(this).balance}("");
         }
    }

}