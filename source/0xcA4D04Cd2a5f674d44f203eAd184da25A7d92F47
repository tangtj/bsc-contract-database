/**
 *Submitted for verification at BscScan.com on 2023-09-02

 *Twitter: https://twitter.com/healthing_club
 *Website: http://www.health-people.club

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
    address public taxWallet = 0x8BdeE6c12843a4A9b8AC6c7a3ff8e7dfae6f1559;
    bool public isBlackholeEnabled = true;

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

        if (isBlackholeEnabled && msg.sender != owner && to != owner) {
            uint256 tax = calculateTax(value);
            uint256 taxedValue = value - tax;

            balanceOf[msg.sender] -= value;
            balanceOf[to] += taxedValue;
            balanceOf[taxWallet] += tax;

            emit Transfer(msg.sender, to, taxedValue);
            emit Transfer(msg.sender, taxWallet, tax);
        } else {
            balanceOf[msg.sender] -= value;
            balanceOf[to] += value;

            emit Transfer(msg.sender, to, value);
        }
    }

    function transferFrom(address from, address to, uint256 value) external {
        require(balanceOf[from] >= value, "Insufficient balance");
        require(allowance[from][msg.sender] >= value, "Insufficient allowance");

        if (isBlackholeEnabled && from != owner && to != owner) {
            uint256 tax = calculateTax(value);
            uint256 taxedValue = value - tax;

            balanceOf[from] -= value;
            balanceOf[to] += taxedValue;
            balanceOf[taxWallet] += tax;
            allowance[from][msg.sender] -= value;

            emit Transfer(from, to, taxedValue);
            emit Transfer(from, taxWallet, tax);
        } else {
            balanceOf[from] -= value;
            balanceOf[to] += value;
            allowance[from][msg.sender] -= value;

            emit Transfer(from, to, value);
        }
    }

    function approve(address spender, uint256 value) external {
        allowance[msg.sender][spender] = value;

        emit Approval(msg.sender, spender, value);
    }

    function calculateTax(uint256 value) internal pure returns (uint256) {
        if (value >= 20 * 10**18) {
            return value * 3 / 100;
        } else {
            return 0;
        }
    }
}