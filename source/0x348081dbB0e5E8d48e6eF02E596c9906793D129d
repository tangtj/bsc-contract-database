/**
 *SPDX-License-Identifier: UNLICENSED
*/

pragma solidity =0.8.6;
/**
 * @title SafeMath
 * @dev Math operations with safety checks that throw on error
 */
library SafeMath {

  /**
  * @dev Multiplies two numbers, throws on overflow.
  */
  function mul(uint256 a, uint256 b) internal pure returns (uint256 c) {
    if (a == 0) {
      return 0;
    }
    c = a * b;
    assert(c / a == b);
    return c;
  }

  /**
  * @dev Integer division of two numbers, truncating the quotient.
  */
  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    // assert(b > 0); // Solidity automatically throws when dividing by 0
    // uint256 c = a / b;
    // assert(a == b * c + a % b); // There is no case in which this doesn't hold
    return a / b;
  }

  /**
  * @dev Subtracts two numbers, throws on overflow (i.e. if subtrahend is greater than minuend).
  */
  function sub(uint256 a, uint256 b) internal pure returns (uint256) {
    assert(b <= a);
    return a - b;
  }

  /**
  * @dev Adds two numbers, throws on overflow.
  */
  function add(uint256 a, uint256 b) internal pure returns (uint256 c) {
    c = a + b;
    assert(c >= a);
    return c;
  }
}
contract CoinToken { 
    using SafeMath for uint256;
    string public name;
    string  public symbol;
    uint256 public totalSupply_;
    uint8   public decimals = 18;
    
    event Transfer(
        address indexed _from,
        address indexed _to,
        uint256 _value
    );

    event Approval(
        address indexed _owner,
        address indexed _spender,
        uint256 _value
    );
    
    mapping(address => uint256) public balances;
    mapping(address => mapping(address => uint256)) public allowed;
    
    constructor(string memory _name, string memory _symbol, uint256 _totalSupply,address _owner) {
        name = _name;
        symbol = _symbol;
        totalSupply_ = _totalSupply;
        balances[_owner] = totalSupply_;
    }
    function totalSupply() public view returns (uint256) {
        return totalSupply_;
    }
    function balanceOf(address _owner) public view returns (uint256) {
        return balances[_owner];
    }
    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(balances[msg.sender] >= _value);
        balances[msg.sender] = balances[msg.sender].sub(_value);
        balances[_to] =  balances[_to].add(_value);
        emit Transfer(msg.sender, _to, _value);
        return true;
    }
    function approve(address _spender, uint256 _value) public returns (bool success) {
        allowed[msg.sender][_spender] = _value; 
        emit Approval(msg.sender, _spender, _value);
        return true;
    }
    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require(_value <= balances[_from]);
        require(_value <= allowed[_from][msg.sender]);
        balances[_from] = balances[_from].sub(_value);
        balances[_to] = balances[_to].add(_value);
        allowed[_from][msg.sender] = allowed[_from][msg.sender].sub(_value); 
        emit Transfer(_from, _to, _value);
        return true;
    }
    
    function allowance(address _owner, address _spender) public view returns (uint256 remaining) {
        return allowed[_owner][_spender];
    }
}
contract TokenFactory { 
    using SafeMath for uint256;
    uint256 public tokenLength;
    address[] public tokens;
    function create(string memory _name, string memory _symbol, uint256 _totalSupply) public returns(address) {
        CoinToken tokenItem = new CoinToken(_name,_symbol,_totalSupply,msg.sender);
        tokens.push(address(tokenItem));
        tokenLength = tokenLength.add(1);
        return address(tokenItem);
    }
}