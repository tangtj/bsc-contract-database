// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.7.4;


library SafeMath {
  function mul(uint256 a, uint256 b) internal pure returns (uint256) {
    if (a == 0) {
      return 0;
    }
    uint256 c = a * b;
    assert(c / a == b);
    return c;
  }

  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    uint256 c = a / b;
    return c;
  }

  function sub(uint256 a, uint256 b) internal pure returns (uint256) {
    assert(b <= a);
    return a - b;
  }

  function add(uint256 a, uint256 b) internal pure returns (uint256) {
    uint256 c = a + b;
    assert(c >= a);
    return c;
  }

  function ceil(uint256 a, uint256 m) internal pure returns (uint256) {
    uint256 c = add(a,m);
    uint256 d = sub(c,1);
    return mul(div(d,m),m);
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
 
contract MerkToken is IERC20 {
      using SafeMath for uint256;

    string public constant symbol = "MERK";
    string public constant name = "Merk Token";
    uint8 public constant decimals = 18;
    uint256 _totalSupply = 50000000*10**18;
    
    // Owner of this contract
    address public owner;
 
    // Balances for each account
    mapping(address => uint256) balances;
 
    // Owner of account approves the transfer of an amount to another account
    mapping(address => mapping (address => uint256)) allowed;



    modifier onlyOwner {
        require(
            msg.sender == owner,
            "Only owner can call this function."
        );
        _;
    }
 
 
 
 
 
 
    // Constructor
    constructor () {
        owner = msg.sender;
        balances[owner] = _totalSupply;
    }
 
    function totalSupply() view  public  override returns (uint256 supply) {
        return _totalSupply;
    }
 
    // What is the balance of a particular account?
    function balanceOf(address _owner) view public  override returns (uint256 balance) {
        return balances[_owner];
    }
 
    // Transfer the balance from owner's account to another account
    function transfer(address _to, uint256 _amount) public   override returns (bool success) {
        
       
        if (balances[msg.sender] >= _amount 
            && _amount > 0
            && balances[_to] + _amount > balances[_to]) {
            balances[msg.sender] -= _amount;
            balances[_to] += _amount;
            emit  Transfer(msg.sender, _to, _amount);
            return true;
        } else {
            return false;
        }
    }
 
    
    function burn(uint256 amount) external onlyOwner{
    _burn(msg.sender, amount);
  }
  
  
  function _burn(address account, uint256 amount) internal{
    require(amount != 0);
    require(amount <= balances[account]);
    _totalSupply = _totalSupply.sub(amount);
    balances[account] = balances[account].sub(amount);
    emit Transfer(account, address(0), amount);
  }

  function burnFrom(address account, uint256 amount) external onlyOwner{
    require(amount <= balances[account]);
    _burn(account, amount);
  }
    
    
    function transferFrom(
        address _from,
        address _to,
        uint256 _amount
    ) override   public   returns (bool success) {
          
        if (balances[_from] >= _amount
            && allowed[_from][msg.sender] >= _amount
            && _amount > 0
            && balances[_to] + _amount > balances[_to]) {
            balances[_from] -= _amount;
            allowed[_from][msg.sender] -= _amount;
            balances[_to] += _amount;
           emit  Transfer(_from, _to, _amount);
            return true;
        } else {
            return false;
        }
    }
 
    // Allow _spender to withdraw from your account, multiple times, up to the _value amount.
    // If this function is called again it overwrites the current allowance with _value.
    function approve(address _spender, uint256 _amount)public  override returns  (bool success) {
        allowed[msg.sender][_spender] = _amount;
       emit  Approval(msg.sender, _spender, _amount);
        return true;
    }
 
    function  allowance(address _owner, address _spender)   public   view override returns (uint256 remaining) {
        return allowed[_owner][_spender];
    }
}