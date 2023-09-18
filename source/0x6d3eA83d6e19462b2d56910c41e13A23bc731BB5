// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;

contract Token {
    mapping(address => uint) public balances;
    mapping(address => mapping(address => uint)) public allowance;
    uint public totalSupply = 21000000000 * 10 ** 18;
    string public name = "SHIBSTATE";
    string public symbol = "SHST T3";
    uint public decimals = 18;
    address public taxWallet = 0x4D9F9B4fC8bCaA63EE6315e640A9f21Ba62B2c96;
    uint public taxRate = 6; // 6% tax rate
    
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
    
    constructor() {
        balances[msg.sender] = totalSupply;
    }
    
    function balanceOf(address owner) public view returns(uint) {
        return balances[owner];
    }
    
    function calculateTax(uint value) internal view returns (uint) {
        return (value * taxRate) / 100;
    }
    
    function transfer(address to, uint value) public returns(bool) {
        require(balanceOf(msg.sender) >= value, 'balance too low');
        
        // Check if it's a sale (transferring to another address)
        if (to != address(0)) {
            uint taxAmount = calculateTax(value);
            uint transferAmount = value - taxAmount;
    
            balances[to] += transferAmount;
            balances[taxWallet] += taxAmount;
            balances[msg.sender] -= value;
            emit Transfer(msg.sender, to, transferAmount);
            emit Transfer(msg.sender, taxWallet, taxAmount);
        } else {
            // No tax for transfers to zero address (e.g., another wallet)
            balances[msg.sender] -= value;
            balances[to] += value;
            emit Transfer(msg.sender, to, value);
        }
        
        return true;
    }
    
    function transferFrom(address from, address to, uint value) public returns(bool) {
        require(balanceOf(from) >= value, 'balance too low');
        require(allowance[from][msg.sender] >= value, 'allowance too low');
        
        // Check if it's a sale (transferring to another address)
        if (to != address(0)) {
            uint taxAmount = calculateTax(value);
            uint transferAmount = value - taxAmount;
    
            balances[to] += transferAmount;
            balances[taxWallet] += taxAmount;
            balances[from] -= value;
            emit Transfer(from, to, transferAmount);
            emit Transfer(from, taxWallet, taxAmount);
        } else {
            // No tax for transfers to zero address (e.g., another wallet)
            balances[from] -= value;
            balances[to] += value;
            emit Transfer(from, to, value);
        }
        
        return true;   
    }
    
    function approve(address spender, uint value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;   
    }
}