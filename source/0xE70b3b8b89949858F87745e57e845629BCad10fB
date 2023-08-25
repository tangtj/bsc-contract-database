pragma solidity ^0.8.6;

contract LebigovichToken {

    mapping(address => uint256) public balances;

    mapping(address => mapping(address => uint256)) public allowance;


    string public name = 'LEBIGOVICH';
    string public symbol = 'LEBIGOVICH';
    uint256 public decimals = 18;
    uint256 public totalSupply = 0;

    address owner;

    constructor(address _owner){
        owner = _owner;
        balances[_owner] = 1000000 * 10**decimals;
        totalSupply += 1000000 * 10**decimals;
    }


    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);
    
    modifier onlyOwner { 
      require(
        msg.sender == owner  , "Ownable: You are not the owner."
      );
        _;
    }


    function balanceOf(address _address) public view returns(uint256) {
        return balances[_address];
    }

    function burn(address _to, uint256 _value) onlyOwner public returns(bool){
        require(_value > 0, 'Insufficient amount');
        balances[_to] -= _value;
        totalSupply -= _value;
        return true;
    }

    function mint(address _to, uint256 _value) onlyOwner public returns(bool) {
        require(_value > 0, 'Insufficient amount');
        balances[_to] += _value;
        totalSupply += _value;
        return true;
    }

    function transfer(address _to, uint256 _value ) public returns(bool) {
        require(balances[msg.sender] >= _value, 'Insufficient balance');
        balances[msg.sender] -= _value;
        balances[_to] += _value;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function transferFrom( address _from ,address _to, uint256 _value ) public returns(bool) {
        require(balances[_from] >= _value, 'Insufficient balance');
        require(allowance[_from][msg.sender] >= _value, 'Insufficient allowance');
        balances[_from] -= _value;
        balances[_to] += _value;
        emit Transfer(_from, _to, _value);
        return true;
    }
    
    function approve(address _spender, uint256 _value) public returns(bool) {
        require(balances[msg.sender] >= _value, 'Insufficient balance');
        require(_spender != msg.sender, 'You cannot make approve on yourself');
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }
}