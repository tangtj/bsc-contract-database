// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract India {
    string public name = "India";
    string public symbol = "IND";
    uint8 public decimals = 18;
    uint256 public totalSupply = 1000000000 * 10**uint256(decimals); // 1 billion tokens

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    mapping(address => bool) public whitelist;
    mapping(address => bool) public blacklist;

    bool public lpLocked = true;
    uint256 public buyTax = 0;
    uint256 public sellTax = 0;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event WhitelistUpdated(address indexed account, bool isWhitelisted);
    event BlacklistUpdated(address indexed account, bool isBlacklisted);

    constructor() {
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
    }

    function setLPUnlock(bool _lpLocked) external {
        require(msg.sender == address(0), "Only the contract owner can change LP status");
        lpLocked = _lpLocked;
    }

    function setBuyTax(uint256 _tax) external {
        require(msg.sender == address(0), "Only the contract owner can set the buy tax");
        buyTax = _tax;
    }

    function setSellTax(uint256 _tax) external {
        require(msg.sender == address(0), "Only the contract owner can set the sell tax");
        sellTax = _tax;
    }

    function addToWhitelist(address account) external {
        require(msg.sender == address(0), "Only the contract owner can add to whitelist");
        whitelist[account] = true;
        emit WhitelistUpdated(account, true);
    }

    function removeFromWhitelist(address account) external {
        require(msg.sender == address(0), "Only the contract owner can remove from whitelist");
        whitelist[account] = false;
        emit WhitelistUpdated(account, false);
    }

    function addToBlacklist(address account) external {
        require(msg.sender == address(0), "Only the contract owner can add to blacklist");
        blacklist[account] = true;
        emit BlacklistUpdated(account, true);
    }

    function removeFromBlacklist(address account) external {
        require(msg.sender == address(0), "Only the contract owner can remove from blacklist");
        blacklist[account] = false;
        emit BlacklistUpdated(account, false);
    }

    function transfer(address to, uint256 value) external returns (bool) {
        require(to != address(0), "Invalid address");
        require(balanceOf[msg.sender] >= value, "Insufficient balance");
        require(!blacklist[msg.sender], "Sender is blacklisted");
        require(!blacklist[to], "Recipient is blacklisted");
        require(lpLocked == false || whitelist[msg.sender] || whitelist[to], "LP is locked");

        uint256 taxAmount = (msg.sender == to) ? 0 : sellTax;

        uint256 taxedValue = value * (100 - taxAmount) / 100;

        balanceOf[msg.sender] -= value;
        balanceOf[to] += taxedValue;

        emit Transfer(msg.sender, to, taxedValue);
        return true;
    }

    function approve(address spender, uint256 value) external returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) external returns (bool) {
        require(from != address(0) && to != address(0), "Invalid address");
        require(balanceOf[from] >= value, "Insufficient balance");
        require(allowance[from][msg.sender] >= value, "Allowance exceeded");
        require(!blacklist[from], "Sender is blacklisted");
        require(!blacklist[to], "Recipient is blacklisted");
        require(lpLocked == false || whitelist[from] || whitelist[to], "LP is locked");

        uint256 taxAmount = (from == to) ? 0 : buyTax;

        uint256 taxedValue = value * (100 - taxAmount) / 100;

        balanceOf[from] -= value;
        balanceOf[to] += taxedValue;
        allowance[from][msg.sender] -= value;

        emit Transfer(from, to, taxedValue);
        return true;
    }
}