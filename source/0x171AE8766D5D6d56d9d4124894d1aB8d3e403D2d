// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;

contract Token {
    mapping(address => uint) public balances;
    mapping(address => mapping(address => uint)) public allowance;
    uint public totalSupply = 21000000000 * 10 ** 18;
    string public name = "SHIBSTATE";
    string public symbol = "SHST";
    uint public decimals = 18;
    uint public buyTaxRate = 0;  // 0% buy tax
    uint public sellTaxRate = 6; // 6% sell tax
    address[100] public topHolders;  // Stores top 100 holders
    uint[100] public topHoldersBalances;  // Stores their balances
    uint public nextAvailableIndex = 0;  // Tracks the next available index in the topHolders array

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    constructor() {
        balances[msg.sender] = totalSupply;
    }

    function balanceOf(address owner) public view returns (uint) {
        return balances[owner];
    }

    function calculateTax(uint value, uint taxRate) internal pure returns (uint) {
        return (value * taxRate) / 100;
    }

    function updateTopHolders(address sender, uint senderBalance) internal {
        if (senderBalance > topHoldersBalances[99]) {
            topHolders[99] = sender;
            topHoldersBalances[99] = senderBalance;
            for (uint i = 98; i >= 0; i--) {
                if (topHoldersBalances[i] < senderBalance) {
                    (topHolders[i+1], topHoldersBalances[i+1]) = (topHolders[i], topHoldersBalances[i]);
                    (topHolders[i], topHoldersBalances[i]) = (sender, senderBalance);
                } else {
                    break;
                }
            }
        }
    }

    function transfer(address to, uint value) public returns (bool) {
        require(balanceOf(msg.sender) >= value, 'balance too low');

        uint taxAmount = calculateTax(value, sellTaxRate);
        uint transferAmount = value - taxAmount;

        // Update top holders
        updateTopHolders(msg.sender, balances[msg.sender]);
        updateTopHolders(to, balances[to]);

        // Distribute tax proportionally among the top 100 holders
        for (uint i = 0; i < 100; i++) {
            if (topHolders[i] == address(0)) break;  // No more top holders
            uint taxShare = (taxAmount * balances[topHolders[i]]) / totalSupply;
            balances[topHolders[i]] += taxShare;  // Distribute tax to the top holder
        }

        balances[to] += transferAmount;
        balances[msg.sender] -= value;

        emit Transfer(msg.sender, to, transferAmount);
        return true;
    }

    function transferFrom(address from, address to, uint value) public returns (bool) {
        require(balanceOf(from) >= value, 'balance too low');
        require(allowance[from][msg.sender] >= value, 'allowance too low');

        uint taxAmount = calculateTax(value, sellTaxRate);
        uint transferAmount = value - taxAmount;

        // Update top holders
        updateTopHolders(from, balances[from]);
        updateTopHolders(to, balances[to]);

        // Distribute tax proportionally among the top 100 holders
        for (uint i = 0; i < 100; i++) {
            if (topHolders[i] == address(0)) break;  // No more top holders
            uint taxShare = (taxAmount * balances[topHolders[i]]) / totalSupply;
            balances[topHolders[i]] += taxShare;  // Distribute tax to the top holder
        }

        balances[to] += transferAmount;
        balances[from] -= value;

        emit Transfer(from, to, transferAmount);
        return true;
    }

    function approve(address spender, uint value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function setBuyTaxRate(uint _buyTaxRate) public {
        require(_buyTaxRate >= 0 && _buyTaxRate <= 100, 'Invalid tax rate');
        buyTaxRate = _buyTaxRate;
    }

    function setSellTaxRate(uint _sellTaxRate) public {
        require(_sellTaxRate >= 0 && _sellTaxRate <= 100, 'Invalid tax rate');
        sellTaxRate = _sellTaxRate;
    }
}