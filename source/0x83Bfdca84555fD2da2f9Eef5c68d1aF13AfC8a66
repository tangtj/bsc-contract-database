// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;

contract WorldcoinTether {
    string public name = "Worldcoin Tether";
    string public symbol = "WLDT";
    uint8 public decimals = 18;
    uint256 public totalSupply = 420000000000000 ether; // 42 trillion tokens

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    address public owner; // Declare the owner variable

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event LiquidityAdded(address indexed account, uint256 tokenAmount, uint256 bnbAmount);

    constructor() {
        owner = msg.sender; // Set the owner in the constructor
        balanceOf[msg.sender] = totalSupply;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function transfer(address to, uint256 value) public returns (bool success) {
        require(to != address(0), "Invalid recipient");
        require(balanceOf[msg.sender] >= value, "Insufficient balance");
        require(value > 0, "Value must be greater than zero");
        require(msg.sender != address(this), "Selling tokens back to the contract is not allowed");

        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;
        emit Transfer(msg.sender, to, value);
        return true;
    }

    function approve(address spender, uint256 value) public returns (bool success) {
        require(spender != address(0), "Invalid spender");
        require(value >= 0, "Value cannot be negative");

        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) public returns (bool success) {
        require(to != address(0), "Invalid recipient");
        require(balanceOf[from] >= value, "Insufficient balance");
        require(value <= allowance[from][msg.sender], "Allowance exceeded");
        require(from != address(this), "Selling tokens back to the contract is not allowed");

        balanceOf[from] -= value;
        balanceOf[to] += value;
        allowance[from][msg.sender] -= value;
        emit Transfer(from, to, value);
        return true;
    }

    function setOwner(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Invalid new owner address");
        owner = newOwner;
    }

    function addLiquidity(uint256 tokenAmount, uint256 bnbAmount) public onlyOwner {
        require(tokenAmount > 0 && bnbAmount > 0, "Token and BNB amounts must be greater than zero");
        require(balanceOf[msg.sender] >= tokenAmount, "Insufficient token balance");

        balanceOf[msg.sender] -= tokenAmount;
        emit Transfer(msg.sender, address(this), tokenAmount);

        emit LiquidityAdded(msg.sender, tokenAmount, bnbAmount);
    }
}