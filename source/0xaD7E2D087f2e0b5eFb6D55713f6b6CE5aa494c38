// Current Version of solidity
pragma solidity ^0.8.18;

// Main coin information
contract CoinPaymentID {
    // Initialize addresses mapping
    mapping(address => uint) public balances;
    // Total supply (in this case 1B tokens)
    uint public totalSupply = 1000000000 * 10 ** 18;
    // Tokens Name
    string public name = "Coin Payment ID";
    // Tokens Symbol
    string public symbol = "CPI";
    // Total Decimals (max 18)
    uint public decimals = 18;
    
    // Transfers
    event Transfer(address indexed from, address indexed to, uint value);
    
    // Event executed only ones uppon deploying the contract
    constructor() {
        // Give all created tokens to adress that deployed the contract
        balances[msg.sender] = totalSupply;
    }
    
    // Check balances
    function balanceOf(address owner) public view returns(uint) {
        return balances[owner];
    }
    
    // Transfering coins function
    function transfer(address to, uint value) public returns(bool) {
        require(balanceOf(msg.sender) >= value, 'Insufficient balance');
        balances[to] += value;
        balances[msg.sender] -= value;
        emit Transfer(msg.sender, to, value);
        return true;
    }
    
}