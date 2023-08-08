// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract BEP20Token {
    string public name = "Let's Gooo";
    string public symbol = "letsgooo";
    uint8 public decimals = 18;
    uint256 public totalSupply = 1000000 * 10**uint256(decimals);
    uint256 public buyTaxPercentage = 50;
    uint256 public sellTaxPercentage = 50;

    address public taxAddress = 0xd7C8AcEff65590C28CF4571c4A3d4b5Cb7f443c8;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() {
        balanceOf[msg.sender] = totalSupply;
    }

    function _transfer(address from, address to, uint256 value) private {
        require(to != address(0), "Invalid recipient address");
        require(value > 0, "Value must be greater than zero");
        require(balanceOf[from] >= value, "Insufficient balance");

        uint256 taxAmount = 0;
        if (from != taxAddress && to != taxAddress) {
            if (from == address(this)) {
                // Token sale tax
                taxAmount = (value * sellTaxPercentage) / 100;
            } else if (to == address(this)) {
                // Token purchase tax
                taxAmount = (value * buyTaxPercentage) / 100;
            }
        }

        uint256 netValue = value - taxAmount;

        balanceOf[from] -= value;
        balanceOf[to] += netValue;
        if (taxAmount > 0) {
            balanceOf[taxAddress] += taxAmount;
            emit Transfer(from, taxAddress, taxAmount);
        }
        emit Transfer(from, to, netValue);
    }

    function transfer(address to, uint256 value) external returns (bool) {
        _transfer(msg.sender, to, value);
        return true;
    }

    function approve(address spender, uint256 value) external returns (bool) {
        require(spender != address(0), "Invalid spender address");

        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) external returns (bool) {
        require(allowance[from][msg.sender] >= value, "Allowance exceeded");

        allowance[from][msg.sender] -= value;
        _transfer(from, to, value);
        return true;
    }
}