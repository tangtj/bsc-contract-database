// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

contract DragonsBaneCoin {
    mapping(address => uint) public balances;
    mapping(address => mapping(address => uint)) public allowance;
    mapping(address => bool) public isRewardsWallet;
    mapping(address => uint) public vestingEndTimes; // Ajout du mapping pour les dates de fin de vesting
    uint public totalSupply = 100000000 * 10 ** 18;
    string public name = "Dragon's Bane Legends v2";
    string public symbol = "DBC";
    uint public decimals = 18;
    address public contractOwner;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
    event Vesting(address indexed beneficiary, uint value);

    constructor() {
        balances[msg.sender] = totalSupply;
        contractOwner = msg.sender;
        isRewardsWallet[msg.sender] = true; // Owner's wallet is rewards wallet
    }

    function balanceOf(address owner) public view returns (uint) {
        return balances[owner];
    }

    function transfer(address to, uint value) public returns (bool) {
        require(balanceOf(msg.sender) >= value, "Balance too low");
        balances[to] += value;
        balances[msg.sender] -= value;
        emit Transfer(msg.sender, to, value);
        return true;
    }

    function transferFrom(address from, address to, uint value) public returns (bool) {
        require(balanceOf(from) >= value, "Balance too low");
        require(allowance[from][msg.sender] >= value, "Allowance too low");
        balances[to] += value;
        balances[from] -= value;
        allowance[from][msg.sender] -= value;
        emit Transfer(from, to, value);
        return true;
    }

    function burn(uint value) public returns (bool) {
        require(balanceOf(msg.sender) >= value, "Balance too low");
        balances[msg.sender] -= value;
        totalSupply -= value;
        emit Transfer(msg.sender, address(0), value);
        return true;
    }

    // Vesting period in days
    uint public vestingPeriod = 60;

    function vesting(address beneficiary, uint value) public returns (bool) {
        require(isRewardsWallet[msg.sender], "Only rewards wallet can call this function");
        require(balanceOf(beneficiary) == 0, "Beneficiary's balance must be zero"); // Added check for beneficiary's balance
        uint vestingEndTime = block.timestamp + vestingPeriod;
        // Set the vesting end time for the beneficiary
        vestingEndTimes[beneficiary] = vestingEndTime;
        balances[beneficiary] += value;
        balances[msg.sender] -= value;
        emit Vesting(beneficiary, value);
        return true;
    }

    function redistribute(address from, address to, uint value) public returns (bool) {
        require(balanceOf(from) >= value, "Balance too low");
        balances[to] += value;
        balances[from] -= value;
        emit Transfer(from, to, value);
        return true;
    }
}