// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;

contract QUICO {
    mapping(address => uint) public balances;
    mapping(address => mapping(address => uint)) public allowance;
    uint public totalSupply = 28000000 * 10 ** 18;
    string public name = "QUICO";
    string public symbol = "QC";
    uint public decimals = 18;
    address public owner; // Store the contract owner's address
    address public marketingWallet = 0x1186FAd6c2a7f80c1061FA61958b1621a5c270B8; // Marketing wallet address
    uint public buySellTaxRate = 3; // 3% buy and sell tax rate
    uint public maxHoldingsPercentage = 1; // 1% maximum holdings percentage

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    constructor() {
        owner = msg.sender; // Set the contract owner's address
        balances[owner] = totalSupply; // Transfer the initial supply to the owner
        emit Transfer(address(0), owner, totalSupply); // Emit a Transfer event to indicate the initial transfer
    }

    function balanceOf(address _owner) public view returns(uint) {
        return balances[_owner];
    }

    // Function to calculate the percentage of the total supply a wallet holds
    function getBalancePercentage(address wallet) public view returns (uint) {
        return (balances[wallet] * 100) / totalSupply;
    }

    function transfer(address to, uint value) public returns(bool) {
        require(balances[msg.sender] >= value, 'balance too low');
        require(getBalancePercentage(to) + (value * 100) / totalSupply <= maxHoldingsPercentage, 'recipient exceeds maximum holdings');
        uint taxAmount = (value * buySellTaxRate) / 100; // Calculate the tax amount
        uint transferAmount = value - taxAmount; // Calculate the amount to be transferred after tax
        balances[to] += transferAmount;
        balances[msg.sender] -= value;
        emit Transfer(msg.sender, to, transferAmount);
        emit Transfer(msg.sender, marketingWallet, taxAmount); // Send the tax to the marketing wallet
        return true;
    }

    function transferFrom(address from, address to, uint value) public returns(bool) {
        require(balances[from] >= value, 'balance too low');
        require(allowance[from][msg.sender] >= value, 'allowance too low');
        require(getBalancePercentage(to) + (value * 100) / totalSupply <= maxHoldingsPercentage, 'recipient exceeds maximum holdings');
        uint taxAmount = (value * buySellTaxRate) / 100; // Calculate the tax amount
        uint transferAmount = value - taxAmount; // Calculate the amount to be transferred after tax
        balances[to] += transferAmount;
        balances[from] -= value;
        emit Transfer(from, to, transferAmount);
        emit Transfer(from, marketingWallet, taxAmount); // Send the tax to the marketing wallet
        return true;   
    }

    function approve(address spender, uint value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;   
    }
}