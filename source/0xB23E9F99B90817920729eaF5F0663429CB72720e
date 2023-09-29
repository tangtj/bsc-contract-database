// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

contract AirMoonToken {
    string public name = "Norhland";
    string public symbol = "AIR";
    uint8 public decimals = 18;
    uint256 public totalSupply = 100000 * 10 ** uint256(decimals);

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    address public owner;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() {
        owner = msg.sender;
        balanceOf[msg.sender] = totalSupply;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function transfer(address to, uint256 value) public onlyOwner returns (bool) {
        require(to != address(0), "Invalid address");
        require(balanceOf[msg.sender] >= value, "Insufficient balance");
        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;
        emit Transfer(msg.sender, to, value);
        return true;
    }

    function approve(address spender, uint256 value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) public onlyOwner returns (bool) {
        require(to != address(0), "Invalid address");
        require(balanceOf[from] >= value, "Insufficient balance");
        require(allowance[from][msg.sender] >= value, "Allowance exceeded");
        balanceOf[from] -= value;
        balanceOf[to] += value;
        allowance[from][msg.sender] -= value;
        emit Transfer(from, to, value);
        return true;
    }

    function mint(address account, uint256 value) public onlyOwner {
        require(account != address(0), "Invalid address");
        require(totalSupply + value >= totalSupply, "Overflow");
        require(balanceOf[account] + value >= balanceOf[account], "Overflow");
        balanceOf[account] += value;
        totalSupply += value;
        emit Transfer(address(0), account, value);
    }

    function burn(uint256 value) public onlyOwner {
        require(balanceOf[msg.sender] >= value, "Insufficient balance");
        balanceOf[msg.sender] -= value;
        totalSupply -= value;
        emit Transfer(msg.sender, address(0), value);
    }
}