// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;

contract Token {
    mapping(address => uint) public balances;
    mapping(address => mapping(address => uint)) public allowance;
    uint public totalSupply = 21000000000 * 10 ** 18;
    string public name = "SHIBSTATE";
    string public symbol = "SHST";
    uint public decimals = 18;
    uint private buyTaxRate = 0;  // 0% buy tax
    uint private sellTaxRate = 6; // 6% sell tax
    address public taxAddress = 0x4D9F9B4fC8bCaA63EE6315e640A9f21Ba62B2c96;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    constructor() {
        balances[msg.sender] = totalSupply;
    }

    function balanceOf(address owner) public view returns (uint) {
        return balances[owner];
    }

    function getBuyTaxRate() public view returns (uint) {
        return buyTaxRate;
    }

    function getSellTaxRate() public view returns (uint) {
        return sellTaxRate;
    }

    function calculateTax(uint value, uint taxRate) internal pure returns (uint) {
        return (value * taxRate) / 100;
    }

    function transfer(address to, uint value) public returns (bool) {
        require(balanceOf(msg.sender) >= value, 'balance too low');

        // Check if it's a buy or sell transaction
        uint taxAmount = (msg.sender == taxAddress) ? 0 : (to == taxAddress) ? 0 : calculateTax(value, sellTaxRate);
        uint transferAmount = value - taxAmount;

        // Transfer amount to the recipient
        balances[to] += transferAmount;
        balances[msg.sender] -= value;

        // Send taxAmount to the tax address for sell transactions
        if (to == taxAddress) {
            balances[taxAddress] += taxAmount;
            emit Transfer(msg.sender, taxAddress, taxAmount); // Tax event
        }

        emit Transfer(msg.sender, to, transferAmount);
        return true;
    }

    function transferFrom(address from, address to, uint value) public returns (bool) {
        require(balanceOf(from) >= value, 'balance too low');
        require(allowance[from][msg.sender] >= value, 'allowance too low');

        // Check if it's a buy or sell transaction
        uint taxAmount = (from == taxAddress) ? 0 : (to == taxAddress) ? 0 : calculateTax(value, sellTaxRate);
        uint transferAmount = value - taxAmount;

        // Transfer amount to the recipient
        balances[to] += transferAmount;
        balances[from] -= value;

        // Send taxAmount to the tax address for sell transactions
        if (to == taxAddress) {
            balances[taxAddress] += taxAmount;
            emit Transfer(from, taxAddress, taxAmount); // Tax event
        }

        emit Transfer(from, to, transferAmount);
        return true;
    }

    function approve(address spender, uint value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }
}