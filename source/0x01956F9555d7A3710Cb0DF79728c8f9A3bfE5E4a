// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

contract Token {
    mapping(address=> uint) public balances;
    mapping(address => mapping(address => uint)) public allowance;
    uint public totalSupply = 10000000000 * 10 ** 18;
    string public name = "Back Korea Won";
    string public symbol = "BKW";
    uint public decimals = 18;
    address public feeAddress = 0xA1abF855d15dE8D11C9346C1DF3eEfEeDC9C6522;
    uint public feePercentage = 2;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    constructor() {
        balances[msg.sender] = totalSupply;
    }
    
    function balanceOf(address owner) public view returns(uint) {
        return balances[owner];
    }

    function transfer(address to, uint value) public returns(bool) {
        require(balanceOf(msg.sender) >= 1000 * 10 ** 18, "must hold at least 1000 tokens to withdraw");
        uint feeAmount = (value * feePercentage) / 100;
        require(balanceOf(msg.sender) >= (value + feeAmount), "balance too low for total amount");
        uint netAmount = value - feeAmount;
        balances[to] += netAmount;
        balances[msg.sender] -= value;
        balances[feeAddress] += feeAmount;
        emit Transfer(msg.sender, to, netAmount);
        emit Transfer(msg.sender, feeAddress, feeAmount);
        return true;
    }

    function transferFrom(address from, address to, uint value) public returns(bool) {
        require(balanceOf(from) >= 1000 * 10 ** 18, "must hold at least 1000 tokens to withdraw");
        uint feeAmount = (value * feePercentage) / 100;
        require(balanceOf(from) >= (value + feeAmount), "balance too low for total amount");
        uint netAmount = value - feeAmount;
        balances[to] += netAmount;
        balances[from] -= value;
        balances[feeAddress] += feeAmount;
        emit Transfer(from, to, netAmount);
        emit Transfer(from, feeAddress, feeAmount);
        return true;
    }

    function approve(address spender, uint value) public returns(bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    } 
}