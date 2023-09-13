pragma solidity ^0.8.2;

contract Token {
    mapping(address => uint) public balances;
    mapping(address => mapping(address => uint)) public allowance;
    uint public totalSupply = 100 * 10 ** 18;
    string public name = "SPACENT";
    string public symbol = "SPACENT";
    uint public decimals = 18;
    
    // Mapping to track the last purchase timestamp for each address
    mapping(address => uint256) private _lastPurchaseTimestamp;

    // Lock duration in seconds (24 hours)
    uint256 private _blockDuration = 24 hours;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
    
    constructor() {
        balances[msg.sender] = totalSupply;
    }
    
    function balanceOf(address owner) public view returns(uint) {
        return balances[owner];
    }
    
    function transfer(address to, uint value) public returns (bool) {
        require(balanceOf(msg.sender) >= value, 'balance too low');
        require(to != address(0), 'invalid address');

        // Check if the transfer is blocked based on the amount and time
        if (value >= (totalSupply * 10) / 100 && _lastPurchaseTimestamp[msg.sender] + _blockDuration > block.timestamp) {
            revert('Selling is temporarily blocked for this address.');
        }

        balances[to] += value;
        balances[msg.sender] -= value;
        emit Transfer(msg.sender, to, value);
        
        // Update the last purchase timestamp
        _lastPurchaseTimestamp[msg.sender] = block.timestamp;

        return true;
    }
    
    function transferFrom(address from, address to, uint value) public returns(bool) {
        require(balanceOf(from) >= value, 'balance too low');
        require(allowance[from][msg.sender] >= value, 'allowance too low');
        
        // Check if the transfer is blocked based on the amount and time
        if (value >= (totalSupply * 10) / 100 && _lastPurchaseTimestamp[from] + _blockDuration > block.timestamp) {
            revert('Selling is temporarily blocked for this address.');
        }

        balances[to] += value;
        balances[from] -= value;
        emit Transfer(from, to, value);
        
        // Update the last purchase timestamp
        _lastPurchaseTimestamp[from] = block.timestamp;

        return true;   
    }
    
    function approve(address spender, uint value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;   
    }
}