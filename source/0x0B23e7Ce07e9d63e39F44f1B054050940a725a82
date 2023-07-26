pragma solidity ^0.6.12;

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
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

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return msg.sender;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() internal {
        _transferOwnership(_msgSender());
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}


contract NBLToken is Context, IERC20, Ownable {
    using SafeMath for uint256;
    
    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;

    mapping (address => bool) public _isExcludedFee;

    string private _name = "$Nobel DAO";
    string private _symbol = "$NBL";
    uint8  private _decimals = 18;
    uint256 private _totalSupply = 15888 * 10**18;
    uint256 private _minTotalSupply = 7777 * 10**18;

    uint256 private _totalFeeRatio = 100; 
    uint256 public baseFee = 10;
    uint256 public liquidityFee = 30;
    uint256 public lpRewardFee = 50;
    uint256 public destroyFee = 20;

    address public mainAddr = address(0x34Dafc3239eBF2906aDF3a263DD50160E2Fcf594);
    address public liquidAddr = address(0xBf89aB4A5d41C7683cdBa0eec41eD676eF02197C);
    address public lpRewardAddr = address(0xda418FBb0edf550Fc570cCdA68ef54AeA20C8484);
    address public destroyAddress = address(0x000000000000000000000000000000000000dEaD);

    constructor () public {
        _isExcludedFee[mainAddr] = true;
        _isExcludedFee[liquidAddr] = true;
        _isExcludedFee[lpRewardAddr] = true;
        _isExcludedFee[address(this)] = true;

        _balances[mainAddr] = _totalSupply;
        emit Transfer(address(0), mainAddr, _totalSupply);
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint256) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
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
        _approve(sender, msg.sender, _allowances[sender][msg.sender].sub(amount));
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender].sub(subtractedValue));
        return true;
    }

    function _mint(address account, uint256 amount) internal {
        require(account != address(0), "ERC20: mint to the zero address");

        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
        emit Transfer(address(0), account, amount);
    }

    function _burn(address account, uint256 value) internal {
        require(account != address(0), "ERC20: burn from the zero address");

        _totalSupply = _totalSupply.sub(value);
        _balances[account] = _balances[account].sub(value);
        emit Transfer(account, address(0), value);
    }

    function _approve(address owner, address spender, uint256 value) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = value;
        emit Approval(owner, spender, value);
    }

    /* onlyOwner */

    function setExcludedFee(address addr, bool state) public onlyOwner {
        _isExcludedFee[addr] = state;
    }
    
    function setMainAddr(address addr) public onlyOwner {
        mainAddr = addr;
    }

    function setFeeAddr(address _liquidAddr,address _lpRewardAddr) public onlyOwner {
        liquidAddr = _liquidAddr;
        lpRewardAddr = _lpRewardAddr;
    }

    function setBaseFee(uint256 _baseFee) public onlyOwner {
        baseFee = _baseFee;
    }

    function _getValues(uint256 amount, uint256 _baseFee, uint256 _liquidityFee, uint256 _lpRewardFee, uint256 _destroyFee, uint256 _totalFeeRatio) 
    private view returns (uint256, uint256, uint256, uint256, uint256) {
        uint256 _amount = amount; 
        uint256 _totalFeeAmount = _amount.mul(_baseFee).div(_totalFeeRatio);
        uint256 _liquidityFeeAmount = _totalFeeAmount.mul(_liquidityFee).div(_totalFeeRatio);
        uint256 _lpRewardFeeAmount = _totalFeeAmount.mul(_lpRewardFee).div(_totalFeeRatio);
        uint256 _destroyFeeAmount = _totalFeeAmount.mul(_destroyFee).div(_totalFeeRatio);

        if(_totalSupply - _destroyFeeAmount <= _minTotalSupply){
            _destroyFeeAmount = _totalSupply - _minTotalSupply;
        }

        uint256 _receiveAmount = _amount.sub(_liquidityFeeAmount).sub(_lpRewardFeeAmount).sub(_destroyFeeAmount);
        return (_totalFeeAmount, _receiveAmount, _liquidityFeeAmount, _lpRewardFeeAmount, _destroyFeeAmount);
    }

    function _takeLiquidity(address sender, uint256 _liquidityFeeAmount) private {
        _balances[liquidAddr] =  _balances[liquidAddr].add(_liquidityFeeAmount);
        emit Transfer(sender, liquidAddr, _liquidityFeeAmount);
    }

    function _takeLpReward(address sender, uint256 _lpRewardFeeAmount) private {
        _balances[lpRewardAddr] =  _balances[lpRewardAddr].add(_lpRewardFeeAmount);
        emit Transfer(sender, lpRewardAddr, _lpRewardFeeAmount);
    }

    function _take2blackHole(address sender, uint256 _destroyFeeAmount) private {
        if (_destroyFeeAmount == 0) {
            return;
        }
        _totalSupply = _totalSupply.sub(_destroyFeeAmount);
        emit Transfer(sender, destroyAddress, _destroyFeeAmount);
    }

    function _transferExcluded(address sender, address recipient, uint256 amount) private {
        _balances[sender] = _balances[sender].sub(amount);
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }

    function _transferStandard(address sender, address recipient, uint256 amount) private {
       (uint256 _totalFeeAmount, uint256 _receiveAmount, uint256 _liquidityFeeAmount, uint256 _lpRewardFeeAmount, uint256 _destroyFeeAmount) = _getValues(amount, baseFee, liquidityFee, lpRewardFee, destroyFee, _totalFeeRatio);
        _balances[sender] = _balances[sender].sub(amount);
        _balances[recipient] = _balances[recipient].add(_receiveAmount);
        _takeLiquidity(sender, _liquidityFeeAmount);
        _takeLpReward(sender, _lpRewardFeeAmount);
        _take2blackHole(sender, _destroyFeeAmount);
        emit Transfer(sender, recipient, _receiveAmount);
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");

        if(_isExcludedFee[sender] || _isExcludedFee[recipient]) {
            _transferExcluded(sender, recipient, amount);
        }else{
            _transferStandard(sender, recipient, amount);
        }
    }
}