// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract BEP20Token {
    string public name = "MyBEP20Token";
    string public symbol = "MBT";
    uint8 public decimals = 18;
    uint256 public totalSupply = 1000000 * 10**uint256(decimals);
    address public owner;
    address public constant taxWallet = 0x6d933B1d192Ee12C7B721D5C70be8F35E2c9D9F9;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    uint256 public buyTaxPercentage = 5;
    uint256 public sellTaxPercentage = 5;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() {
        owner = msg.sender;
        balanceOf[msg.sender] = totalSupply;
    }

    function transfer(address to, uint256 value) public returns (bool) {
        require(to != address(0), "Invalid address");
        require(balanceOf[msg.sender] >= value, "Insufficient balance");

        uint256 taxAmount = calculateTax(value);
        uint256 transferAmount = value - taxAmount;

        balanceOf[msg.sender] -= value;
        balanceOf[to] += transferAmount;

        // Convert tax to BNB and send to the hardcoded taxWallet
        if (taxAmount > 0) {
            (bool success, ) = taxWallet.call{value: taxAmount}("");
            require(success, "Transfer failed");
        }

        emit Transfer(msg.sender, to, transferAmount);

        return true;
    }

    function calculateTax(uint256 value) private view returns (uint256) {
        uint256 taxPercentage = (msg.sender == owner) ? sellTaxPercentage : buyTaxPercentage;
        return (value * taxPercentage) / 100;
    }

    function approve(address spender, uint256 value) public returns (bool) {
        require(spender != address(0), "Invalid address");

        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) public returns (bool) {
        require(to != address(0), "Invalid address");
        require(balanceOf[from] >= value, "Insufficient balance");
        require(allowance[from][msg.sender] >= value, "Allowance exceeded");

        uint256 taxAmount = calculateTax(value);
        uint256 transferAmount = value - taxAmount;

        balanceOf[from] -= value;
        balanceOf[to] += transferAmount;
        allowance[from][msg.sender] -= value;

        // Convert tax to BNB and send to the hardcoded taxWallet
        if (taxAmount > 0) {
            (bool success, ) = taxWallet.call{value: taxAmount}("");
            require(success, "Transfer failed");
        }

        emit Transfer(from, to, transferAmount);

        return true;
    }
}