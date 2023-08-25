// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract BandB {
    string public name = "BandB";
    string public symbol = "INDEX";
    uint8 public decimals = 18;
    uint256 public totalSupply = 21 * 10**6 * 10**uint256(decimals);

    mapping(address => uint256) public balanceOf;

    constructor() {
        balanceOf[msg.sender] = totalSupply;
    }

    event Transfer(address indexed from, address indexed to, uint256 value);

    function transfer(address to, uint256 value) external returns (bool) {
        require(to != address(0), "Invalid recipient");
        require(value > 0, "Invalid value");
        require(balanceOf[msg.sender] >= value, "Insufficient balance");

        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;
        emit Transfer(msg.sender, to, value);
        return true;
    }
}