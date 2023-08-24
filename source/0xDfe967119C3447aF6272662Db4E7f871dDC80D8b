// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract BrcQuest {
    string public name = "BrcQuest";
    string public symbol = "BQST";
    uint8 public decimals = 18;
    uint256 public totalSupply = 21000000 * 10**uint256(decimals);

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    bool public tradingEnabled = false;
    address public owner;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event TradingEnabled(bool enabled);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can perform this action");
        _;
    }

    modifier canTrade() {
        require(tradingEnabled || msg.sender == owner, "Trading is currently disabled");
        _;
    }

    constructor() {
        balanceOf[msg.sender] = totalSupply;
        owner = msg.sender;
    }

    function enableTrading() public onlyOwner {
        require(!tradingEnabled, "Trading is already enabled");
        tradingEnabled = true;
        emit TradingEnabled(true);
    }

    function transfer(address to, uint256 value) public canTrade returns (bool) {
        require(to != address(0), "Invalid address");
        require(balanceOf[msg.sender] >= value, "Insufficient balance");

        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;

        emit Transfer(msg.sender, to, value);
        return true;
    }

    function approve(address spender, uint256 value) public canTrade returns (bool) {
        allowance[msg.sender][spender] = value;

        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) public canTrade returns (bool) {
        require(from != address(0), "Invalid address");
        require(to != address(0), "Invalid address");
        require(balanceOf[from] >= value, "Insufficient balance");
        require(allowance[from][msg.sender] >= value, "Allowance exceeded");

        balanceOf[from] -= value;
        balanceOf[to] += value;
        allowance[from][msg.sender] -= value;

        emit Transfer(from, to, value);
        return true;
    }

    // Burn tokens by sending them to the zero address (0x0000000000000000000000000000000000000000)
    function burn(uint256 value) public canTrade returns (bool) {
        require(balanceOf[msg.sender] >= value, "Insufficient balance");

        balanceOf[msg.sender] -= value;
        totalSupply -= value;

        emit Transfer(msg.sender, address(0), value);
        return true;
    }
}