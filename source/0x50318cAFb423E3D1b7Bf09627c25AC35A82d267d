pragma solidity ^0.8.2;

contract Token {
    address public owner;
    uint public taxFee;
    mapping(address => uint) public balances;
    mapping(address => mapping(address => uint)) public allowance;
    uint public totalSupply = 1000000 * 10 ** 18;
    string public name = "FINE 2.0";
    string public symbol = "FINE 2.0";
    uint public decimals = 18;
    
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    event TaxFeeUpdated(uint newTaxFee);
    
    constructor() {
        balances[msg.sender] = totalSupply;
        owner = msg.sender;
        taxFee = 1;
    }
    
    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(owner, address(0));
        owner = address(0);
    }
    
    function setTaxFee(uint newFee) public onlyOwner {
        require(newFee >= 0 && newFee <= 100, "Tax fee should be between 0 and 100");
        taxFee = newFee;
        emit TaxFeeUpdated(newFee);
    }
    
    function balanceOf(address _owner) public view returns(uint) {
        return balances[_owner];
    }
    
    function transfer(address to, uint value) public returns (bool) {
        require(balanceOf(msg.sender) >= value, 'balance too low');
        require(to != address(0), 'invalid address');
        
        uint feeAmount = (value * taxFee) / 100;
        uint transferAmount = value - feeAmount;
        
        balances[to] += transferAmount;
        balances[msg.sender] -= value;
        emit Transfer(msg.sender, to, transferAmount);
        
        balances[owner] += feeAmount;
        emit Transfer(msg.sender, owner, feeAmount);
        
        return true;
    }
    
    function transferFrom(address from, address to, uint value) public returns(bool) {
        require(balanceOf(from) >= value, 'balance too low');
        require(allowance[from][msg.sender] >= value, 'allowance too low');
        
        uint feeAmount = (value * taxFee) / 100;
        uint transferAmount = value - feeAmount;
        
        balances[to] += transferAmount;
        balances[from] -= value;
        emit Transfer(from, to, transferAmount);
        
        balances[owner] += feeAmount;
        emit Transfer(from, owner, feeAmount);
        
        return true;   
    }
    
    function approve(address spender, uint value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;   
    }
}