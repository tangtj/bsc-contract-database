// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

contract DragonsBaneCoin {
    string public name = "Dragon's Bane Legends NFT";
    string public symbol = "DBC";
    uint8 public decimals = 18;
    uint256 public totalSupply;
    address public owner;

    mapping(address => uint256) public balanceOf;
    mapping(address => bool) public isRewardsWallet;
    mapping(address => uint256) public vestingEndTimes;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Vesting(address indexed beneficiary, uint256 value, uint256 vestingEndTime);

    constructor() {
        uint256 initialSupply = 100000000 * 10**uint256(decimals);
        totalSupply = initialSupply;
        balanceOf[msg.sender] = initialSupply;
        owner = msg.sender;
        isRewardsWallet[msg.sender] = true;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function transfer(address to, uint256 value) public returns (bool) {
        require(to != address(0), "Invalid address");
        require(balanceOf[msg.sender] >= value, "Balance too low");

        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;

        emit Transfer(msg.sender, to, value);

        return true;
    }

    function burn(uint256 value) public returns (bool) {
        require(balanceOf[msg.sender] >= value, "Balance too low");

        balanceOf[msg.sender] -= value;
        totalSupply -= value;

        emit Transfer(msg.sender, address(0), value);

        return true;
    }

    uint256 public vestingPeriod = 60;

    function vesting(address beneficiary, uint256 value) public returns (bool) {
        require(isRewardsWallet[msg.sender], "Only rewards wallet can call this function");
        require(balanceOf[beneficiary] == 0, "Beneficiary's balance must be zero");

        vestingEndTimes[beneficiary] = block.timestamp + vestingPeriod * 1 days;

        balanceOf[owner] -= value;
        balanceOf[beneficiary] += value;

        emit Transfer(owner, beneficiary, value);
        emit Vesting(beneficiary, value, vestingEndTimes[beneficiary]);

        return true;
    }

    function redistribute(address to, uint256 value) public returns (bool) {
        require(balanceOf[msg.sender] >= value, "Balance too low");
        require(to != address(0), "Invalid address");

        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;

        emit Transfer(msg.sender, to, value);

        return true;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Invalid address");
        owner = newOwner;
    }
}