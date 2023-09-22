// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract DigitalTrashEuro {
mapping(address => uint) public balances;
mapping(address => mapping(address => uint)) public allowance;
uint public totalSupply = 12237803000000 * 10 ** 2;
string public name = "Digital (Trash) Euro";
string public symbol = "DTEU";
uint public decimals = 2;
uint public burnPercent = 6; // 0.6% TRANSACTION BURN FEE
event Transfer(address indexed from, address indexed to, uint value);
event Approval(address indexed owner, address indexed spender, uint value);
event Burn(address indexed from, uint value);

constructor() {
    balances[msg.sender] = totalSupply;
}

function balanceOf(address owner) public view returns(uint) {
    return balances[owner];
}

function transfer(address to, uint value) public returns(bool) {
    require(balanceOf(msg.sender) >= value, 'balance too low');

    uint burnAmount = value * burnPercent / 1000;
    uint transferAmount = value - burnAmount;

    balances[to] += transferAmount;
    balances[msg.sender] -= value;
    totalSupply -= burnAmount;

    emit Transfer(msg.sender, to, transferAmount);
    emit Burn(msg.sender, burnAmount);

    return true;
}

function transferFrom(address from, address to, uint value) public returns(bool) {
    require(balanceOf(from) >= value, 'balance too low');
    require(allowance[from][msg.sender] >= value, 'allowance too low');

    uint burnAmount = value * burnPercent / 1000;
    uint transferAmount = value - burnAmount;

    balances[to] += transferAmount;
    balances[from] -= value;
    totalSupply -= burnAmount;

    emit Transfer(from, to, transferAmount);
    emit Burn(from, burnAmount);

    return true;
}

function approve(address spender, uint value) public returns (bool) {
    allowance[msg.sender][spender] = value;
    emit Approval(msg.sender, spender, value);
    return true;
}
}