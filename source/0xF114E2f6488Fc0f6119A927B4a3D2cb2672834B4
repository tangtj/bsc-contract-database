pragma solidity 0.5.16;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function decimals() external view returns (uint8);
    function symbol() external view returns (string memory);
    function name() external view returns (string memory);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address _owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}
interface IFactory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);
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

contract EAF is IERC20 {
    
    using SafeMath for uint256;

    mapping (address => uint256) private _balances;

    mapping (address => mapping (address => uint256)) private _allowances;

    address private _usdt = address(0x55d398326f99059fF775485246999027B3197955);
    address private _feePair;

    address private _fee1 = address(0x35fA40Cf135c14f3EE0Ee3e2ea96fAE3158a186A);
    address private _fee2 = address(0xdd3BbB2DE336Fd67e1Ae36f521caFbfadB867561);
    address private _fee3 = address(0x5e2d879416465398bb975a5Cc9703a7d57af563c);

    uint256 private _totalSupply;
    uint8 public _decimals = 18;
    string public _symbol = "EAF";
    string public _name = "EAF";

    IFactory _factory = IFactory(0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73);

    constructor(address addr) public {
        _totalSupply = 3100 * 10**18;
        _balances[addr] = _totalSupply;
        _feePair = _factory.getPair(address(this), _usdt);
        emit Transfer(address(0), addr, _totalSupply);
    }

    function decimals() external view returns (uint8) {
        return _decimals;
    }

    function symbol() external view returns (string memory) {
        return _symbol;
    }

    function name() external view returns (string memory) {
        return _name;
    }

    function totalSupply() external view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) external returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) external view returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender].sub(amount, "BEP20: transfer amount exceeds allowance"));
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender].sub(subtractedValue, "BEP20: decreased allowance below zero"));
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");
        _balances[sender] = _balances[sender].sub(amount, "BEP20: transfer amount exceeds balance");      
        uint256 recipientAmount = amount;
        if(_feePair == address(0)) _feePair = _factory.getPair(address(this), _usdt);

        if(_feePair != address(0) && (sender == _feePair || recipient == _feePair)){
            uint256 feeAmount = amount.mul(3).div(100);
            recipientAmount = amount.sub(feeAmount);

            _balances[_fee1] = _balances[_fee1].add(feeAmount.div(4));
            emit Transfer(sender, _fee1, feeAmount.div(4));
            _balances[_fee2] = _balances[_fee2].add(feeAmount.div(4));
            emit Transfer(sender, _fee2, feeAmount.div(4));
            _balances[_fee3] = _balances[_fee3].add(feeAmount.div(2));
            emit Transfer(sender, _fee3, feeAmount.div(2));
            
        }
        require(recipientAmount > 0, "BEP20: recipient amount > 0");
        _balances[recipient] = _balances[recipient].add(recipientAmount);
        emit Transfer(sender, recipient, recipientAmount);
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function isContract(address account) internal view returns (bool) {
        // According to EIP-1052, 0x0 is the value returned for not-yet created accounts
        // and 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470 is returned
        // for accounts without code, i.e. `keccak256('')`
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly { codehash := extcodehash(account) }
        return (codehash != accountHash && codehash != 0x0);
    }

  
}