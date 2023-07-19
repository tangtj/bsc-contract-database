//SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.19;

contract Token {

    string public name;
    string public symbol;
    uint public totalSupply;
    uint public decimals;

    address public owner;
    address private _previousOwner;
    uint private _unlockTime;
    
    mapping(address => uint) private _balances;
    mapping(address => mapping(address => uint)) private _allowed;

    bool public ownershipRenounced;
 
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can do this!");
        _;
    }
    
    constructor(
        string memory _name, 
        string memory _symbol, 
        uint[] memory _numbers, 
        address[] memory _addresses
    ) {
        name = _name;
        symbol = _symbol;
        decimals = _numbers[0];
        totalSupply = _numbers[1] * 10 ** decimals;

        owner = _addresses[0];
        _previousOwner = _addresses[0];

        _balances[owner] = totalSupply;

        ownershipRenounced = false;
        
        emit Transfer(address(0), owner, totalSupply);
        emit OwnershipTransferred(address(0), owner);
    }
    
    function balanceOf(address _owner) public view returns(uint) {
        return _balances[_owner];
    }
    
    function transfer(address _to, uint _value) public returns(bool) {
        require(_balances[msg.sender] >= _value, "Balance is too low");
        
        _balances[msg.sender] -= _value;
        _balances[_to] += _value;

        emit Transfer(msg.sender, _to, _value);
        return true;
    }
    
    function transferFrom(address _from, address _to, uint _value) public returns(bool) {
        require(_balances[_from] >= _value, "Balance is too low");
        require(_allowed[_from][msg.sender] >= _value, "Allowance is too low");
        
        _balances[_from] -= _value;
        _allowed[_from][msg.sender] -= _value;
        _balances[_to] += _value;

        emit Transfer(_from, _to, _value);
        return true;   
    }
    
    function approve(address _spender, uint _value) public returns (bool) {
        _allowed[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;   
    }
    
    function allowance(address _owner, address _spender) public view returns (uint) {
        return _allowed[_owner][_spender];
    }

    function mint(address _to, uint _amount) public onlyOwner {
        require(_to != address(0), "Don't mint to zero address!");
        totalSupply += _amount;
        _balances[_to] += _amount;
        
        emit Transfer(address(0), _to, _amount);
    }

    function burn(uint _amount) public {
        require(_amount <= _balances[msg.sender], "Balance is too low");
        
        totalSupply -= _amount;
        _balances[msg.sender] -= _amount;
        
        emit Transfer(msg.sender, address(0), _amount);
    }

    function burnFrom(address _from, uint _amount) public {
        require(_amount <= _balances[_from], "Balance is too low");
        require(_amount <= _allowed[_from][msg.sender], "Allowance is too low");
        
        totalSupply -= _amount;
        _balances[_from] -= _amount;
        _allowed[_from][msg.sender] -= _amount;
        
        emit Transfer(_from, address(0), _amount);
    }

    function renounceOwnership() public onlyOwner {
        owner = address(0);
        _previousOwner = address(0);
        ownershipRenounced = true;
        emit OwnershipTransferred(owner, address(0));
    }

    function transferOwnership(address _newOwner) public onlyOwner {
        require(_newOwner != address(0), "New owner can't be zero address. If you still want to do this use renounceOwnership function.");
        owner = _newOwner;
        _previousOwner = _newOwner;
        emit OwnershipTransferred(owner, _newOwner);
    }

    function getUnlockTime() public view returns (uint) {
        return _unlockTime;
    }
    
    function lockOwnership(uint _duration) public onlyOwner {
        _previousOwner = owner;
        owner = address(0);
        _unlockTime = block.timestamp + _duration;
        emit OwnershipTransferred(_previousOwner, address(0));
    }
    
    function unlockOwnership() public {
        require(_previousOwner == msg.sender, "You don't have permission to unlock ownership");
        require(block.timestamp > _unlockTime , "Ownership is still locked");
        owner = _previousOwner;
        emit OwnershipTransferred(owner, _previousOwner);
    }
}