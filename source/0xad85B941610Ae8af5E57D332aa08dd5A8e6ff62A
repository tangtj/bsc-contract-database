pragma solidity ^0.8.18;
// SPDX-License-Identifier: MIT


contract Token {
    mapping(address => uint) public balances;
    mapping(address => mapping(address => uint)) public allowance;
    uint public totalSupply = 19341109196219631968 * 10 ** 9;
    string public name = "CosmicDoomPEPE";
    string public symbol = "CosDoomPE2";
    uint public decimals = 9;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    address private constant TAX_WALLET = 0x6EDaF404DF6C17E406Aedf1377723C09745C5038;
    uint private constant BUY_TAX_PERCENT = 2;
    uint private constant BUY_BURN_PERCENT = 1;
    uint private constant BUY_TRANSFER_PERCENT = 1;
    uint private constant SELL_TAX_PERCENT = 4;
    uint private constant SELL_BURN_PERCENT = 2;
    uint private constant SELL_TRANSFER_PERCENT = 2;

    constructor() {
        balances[msg.sender] = totalSupply;
    }
    
    function balanceOf(address owner) public view returns(uint) {
        return balances[owner];
    }

    function transfer(address to, uint value) public returns(bool) {
        require(balanceOf(msg.sender) >= value, 'balance too low');
        uint taxAmount = calculateBuyTax(value);
        uint netAmount = value - taxAmount;

        balances[to] += netAmount;
        balances[msg.sender] -= value;

        if (taxAmount > 0) {
            // Burn tokens
            uint burnAmount = (taxAmount * BUY_BURN_PERCENT) / BUY_TAX_PERCENT;
            totalSupply -= burnAmount;

            // Move tokens to the tax wallet
            uint transferAmount = (taxAmount * BUY_TRANSFER_PERCENT) / BUY_TAX_PERCENT;
            balances[TAX_WALLET] += transferAmount;
            emit Transfer(msg.sender, TAX_WALLET, transferAmount);
        }
        
        emit Transfer(msg.sender, to, netAmount);
        return true;
    }

    function transferFrom(address from, address to, uint value) public returns(bool) {
        require(balanceOf(from) >= value, 'balance too low');
        require(allowance[from][msg.sender] >= value, 'allowance too low');
        uint taxAmount = calculateSellTax(value);
        uint netAmount = value - taxAmount;

        balances[to] += netAmount;
        balances[from] -= value;

        if (taxAmount > 0) {
            // Burn tokens
            uint burnAmount = (taxAmount * SELL_BURN_PERCENT) / SELL_TAX_PERCENT;
            totalSupply -= burnAmount;

            // Move tokens to the tax wallet
            uint transferAmount = (taxAmount * SELL_TRANSFER_PERCENT) / SELL_TAX_PERCENT;
            balances[TAX_WALLET] += transferAmount;
            emit Transfer(from, TAX_WALLET, transferAmount);
        }
        
        emit Transfer(from, to, netAmount);
        return true;
    }

    function approve(address spender, uint value) public returns(bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }
    
    function calculateBuyTax(uint value) private pure returns (uint) {
        uint taxAmount = (value * BUY_TAX_PERCENT) / 100;
        uint burnAmount = (taxAmount * BUY_BURN_PERCENT) / BUY_TAX_PERCENT;
        uint transferAmount = taxAmount - burnAmount;
        
        return transferAmount;
    }
    
    function calculateSellTax(uint value) private pure returns (uint) {
        uint taxAmount = (value * SELL_TAX_PERCENT) / 100;
        uint burnAmount = (taxAmount * SELL_BURN_PERCENT) / SELL_TAX_PERCENT;
        uint transferAmount = taxAmount - burnAmount;
        
        return transferAmount;
    }
}