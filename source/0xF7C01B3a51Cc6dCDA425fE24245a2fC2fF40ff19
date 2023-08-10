// SPDX-License-Identifier: MIT
pragma solidity 0.8.9;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

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

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }
}

contract Ownable is Context {
    address private _owner;
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

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }
}

interface IUniswapV2Factory {
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);

    function getPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);
}

interface IUniswapV2Router01 {
    function getAmountsOut(
        uint amountIn,
        address[] calldata path
    ) external view returns (uint[] memory amounts);
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function getAmountOut(uint amountIn) external pure returns (uint amountOut);
}

contract Awesome is Context, IERC20, Ownable {
    using SafeMath for uint256;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcludedFromFee;
    mapping(address => bool) private bots;
    mapping(address => uint256) private _holderLastTransferTimestamp;
    bool public transferDelayEnabled = false;
    address public _taxWallet;
    address public _devWallet;
    address public _marketingWallet;
    address public _liquidityWallet;

    uint256 public _BuyTax = 0;
    uint256 public _SellTax = 0;

    uint256 public _previousBuyTax = _BuyTax;
    uint256 public _previousSellTax = _SellTax;

    uint256 public _tTeam;

    uint256 private _preventSwapBefore = 20;
    uint256 private _buyCount = 0;

    uint8 private constant _decimals = 9;
    uint256 private _tTotal = 1000000000 * 10 ** _decimals;
    string private constant _name = unicode"AWESOME";
    string private constant _symbol = unicode"$AWSM";
    uint256 public _maxTxAmount = _tTotal.mul(2).div(100);
    uint256 public _maxWalletSize = _tTotal.mul(2).div(100);
    uint256 public _taxSwapThreshold = _tTotal.mul(2).div(100);
    uint256 public _maxTaxSwap = _tTotal.mul(2).div(100);
    uint256 public slippage = 1; // Default value of 1% slippage (100 = 1%)

    IUniswapV2Router02 public uniswapV2Router =
        IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
    address public uniswapV2Pair;
    bool private tradingOpen;
    bool private inSwap = false;
    bool private swapEnabled = false;
    uint256 public _totalBurned;

    event MaxTxAmountUpdated(uint _maxTxAmount);
    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor(
        address taxWallet,
        address devWallet,
        address marketingWallet,
        address liquidityWallet
    ) {
        _taxWallet = payable(taxWallet);
        _devWallet = payable(devWallet);
        _marketingWallet = payable(marketingWallet);
        _liquidityWallet = payable(liquidityWallet);
        _balances[_devWallet] = _tTotal.mul(3).div(100);
        _balances[_marketingWallet] = _tTotal.mul(7).div(100);
        _tTeam = balanceOf(_devWallet).add(balanceOf(_marketingWallet));
        _balances[_liquidityWallet] = _tTotal.sub(_tTeam);
        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[_taxWallet] = true;
        _isExcludedFromFee[_liquidityWallet] = true;
        _isExcludedFromFee[_devWallet] = true;
        _isExcludedFromFee[_marketingWallet] = true;

        emit Transfer(address(0), _msgSender(), _tTotal);
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

    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(
        address owner,
        address spender
    ) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(
        address spender,
        uint256 amount
    ) public override returns (bool) {
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
            _allowances[sender][_msgSender()].sub(
                amount,
                "ERC20: transfer amount exceeds allowance"
            )
        );
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(address from, address to, uint256 amount) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        require(to != address(this), "You cannot send tokens to the contract");
        bool takeFee = false;

        if (from != owner() && to != owner()) {
            require(!bots[from] && !bots[to]);
            takeFee = true;

            if (transferDelayEnabled) {
                if (
                    to != address(uniswapV2Router) &&
                    to != address(uniswapV2Pair)
                ) {
                    require(
                        _holderLastTransferTimestamp[tx.origin] < block.number,
                        "Transfer Delay enabled. Only one purchase per block allowed."
                    );
                    _holderLastTransferTimestamp[tx.origin] = block.number;
                }
            }

            if (
                from == uniswapV2Pair &&
                to != address(uniswapV2Router) &&
                !_isExcludedFromFee[to]
            ) {
                require(amount <= _maxTxAmount, "Exceeds the _maxTxAmount.");
                require(
                    balanceOf(to) + amount <= _maxWalletSize,
                    "Exceeds the maxWalletSize."
                );
                _buyCount++;
            }

            if (
                to == uniswapV2Pair &&
                from != address(uniswapV2Router) &&
                !_isExcludedFromFee[from]
            ) {
                require(
                    amount <= _maxTxAmount,
                    "Transfer amount exceeds the maxSellAmount."
                );
            }
        }

        if (_isExcludedFromFee[from] || _isExcludedFromFee[to]) {
            takeFee = false;
        }

        uint256 contractTokenBalance = balanceOf(address(this));
        if (
            !inSwap &&
            to == uniswapV2Pair &&
            swapEnabled &&
            contractTokenBalance > _taxSwapThreshold &&
            _buyCount > _preventSwapBefore
        ) {
            if (!takeFee) {
                removeAllFee();
            } else {
                amount = _takeFees(from, amount);
            }
            swapTokensForEth(
                min(amount, min(contractTokenBalance, _maxTaxSwap))
            );
            uint256 contractETHBalance = address(this).balance;
            if (contractETHBalance > 0) {
                sendETHToFee(address(this).balance);
            }
        }

        _tokenTransfer(from, to, amount, takeFee);
    }

    function removeAllFee() private {
        if (_BuyTax == 0 && _SellTax == 0) return;
        _BuyTax = 0;
        _SellTax = 0;
    }

    function restoreAllFee() private {
        _SellTax = _previousSellTax;
        _BuyTax = _previousBuyTax;
    }

    function _tokenTransfer(
        address from,
        address to,
        uint256 amount,
        bool takeFee
    ) private {
        if (!takeFee) {
            removeAllFee();
        } else {
            amount = _takeFees(from, amount);
        }

        _transferStandard(from, to, amount);

        if (!takeFee) {
            restoreAllFee();
        }
    }

    function _transferStandard(
        address from,
        address to,
        uint256 tAmount
    ) private {
        _balances[from] = _balances[from].sub(tAmount);
        if (to == address(0)) {
            _tTotal = _tTotal.sub(tAmount);
            _totalBurned = _totalBurned.add(tAmount);
        } else {
            _balances[to] = _balances[to].add(tAmount);
        }
        emit Transfer(from, to, tAmount);
    }

    function _takeFees(address from, uint256 amount) private returns (uint256) {
        uint256 tAmount = amount.mul(_BuyTax).div(100);

        if (tAmount > 0) {
            uint256 burnAmount = tAmount.mul(2).div(100);

            _transferStandard(from, _taxWallet, tAmount);

            burnFromTSupply(_taxWallet, burnAmount);
        }
        return amount - tAmount;
    }

    function min(uint256 a, uint256 b) private pure returns (uint256) {
        return (a > b) ? b : a;
    }

    function swapTokensForEth(uint256 tokenAmount) private lockTheSwap {
        require(tokenAmount > 0, "Not enough tokens");
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapV2Router.WETH();

        uint256 actualAmountOut = _fetchAmountOut(tokenAmount, path);

        uint256 slippagedAmountOut = slippage == 100
            ? actualAmountOut
            : actualAmountOut.mul(100 - slippage).div(100);

        // require(amountAccepted < executionPrice, "INSUFFICIENT_AMOUNT");

        _approve(address(this), address(uniswapV2Router), tokenAmount);
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            slippagedAmountOut,
            path,
            address(this),
            block.timestamp
        );
    }

    function burn(address from, uint256 burnAmount) public {
        require(from == msg.sender, "You cannot burn tokens you do not have");
        require(from != address(0), "cannot burn from the zero address");
        require(
            burnAmount > 0 && burnAmount <= balanceOf(from),
            "amount must be greater than zero"
        );
        //trasfer to a zero address
        _transferStandard(from, address(0), burnAmount);
    }

    function burnFromTSupply(address from, uint256 burnAmount) internal {
        require(from != address(0), "cannot burn from the zero address");
        require(
            burnAmount > 0 && burnAmount <= balanceOf(from),
            "amount must be greater than zero"
        );
        //trasfer to a zero address
        _transferStandard(from, address(0), burnAmount);
    }

    function setUniswapV2Router(address routerAddress) public onlyOwner {
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(routerAddress);

        uniswapV2Router = _uniswapV2Router;
    }

    function setUniswapV2Pair(
        address tokenA,
        address tokenB
    ) external onlyOwner {
        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).getPair(
            tokenA,
            tokenB
        );
    }

    function _fetchAmountOut(
        uint256 amountIn,
        address[] memory path
    ) internal view returns (uint256 getAmountsAmountOut) {
        uint256[] memory amountOut = uniswapV2Router.getAmountsOut(
            amountIn,
            path
        );

        getAmountsAmountOut = amountOut[amountOut.length - 1];
    }

    function removeLimits() public onlyOwner {
        _maxTxAmount = _tTotal;
        _maxWalletSize = _tTotal;
        transferDelayEnabled = false;
        emit MaxTxAmountUpdated(_tTotal);
    }

    function sendETHToFee(uint256 amount) internal {
        (bool success, ) = _devWallet.call{value: amount}("");
        require(success, "ETH transfer failed");
    }

    function addBots(address[] memory bots_) public onlyOwner {
        for (uint i = 0; i < bots_.length; i++) {
            bots[bots_[i]] = true;
        }
    }

    function delBots(address[] memory notbot) public onlyOwner {
        for (uint i = 0; i < notbot.length; i++) {
            bots[notbot[i]] = false;
        }
    }

    function isBot(address a) public view returns (bool) {
        return bots[a];
    }

    function reduceFees(uint256 _newFee) external {
        require(_msgSender() == _taxWallet);
        _BuyTax = _newFee;
        _SellTax = _newFee;
    }

    function setSlippage(uint256 _slippage) external onlyOwner {
        require(
            _slippage <= 100,
            "Slippage value should be in percentage"
        );
        slippage = _slippage;
    }

    receive() external payable {}

    function manualSwap() external {
        require(_msgSender() == _taxWallet);
        uint256 tokenBalance = balanceOf(address(this));

        if (tokenBalance > 0) {
            swapTokensForEth(tokenBalance);
        }
        uint256 ethBalance = address(this).balance;
        if (ethBalance > 0) {
            sendETHToFee(ethBalance);
        }
    }

    function excludeFromFee(
        address[] calldata accounts,
        bool isExcluded
    ) external onlyOwner {
        for (uint256 i = 0; i < accounts.length; i++) {
            _isExcludedFromFee[accounts[i]] = isExcluded;
        }
    }
}