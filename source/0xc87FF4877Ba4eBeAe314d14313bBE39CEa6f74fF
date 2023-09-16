// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;

/**
 * @title I2Coin Token
 * @dev A BEP-20 token with additional features.
 * Developer: JuFerrigno21
 */
contract I2Coin {
    // Token information
    string public name = "i2coin";
    string public symbol = "I2C";
    uint8 public decimals = 18;
    uint256 public totalSupply;
    address public owner;

    // Balances and allowances
    mapping(address => uint256) private balances;
    mapping(address => mapping(address => uint256)) private allowances;

    // Events
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    // Constructor
    constructor(uint256 initialSupply) {
        totalSupply = initialSupply * 10 ** uint256(decimals);
        owner = msg.sender;
        balances[msg.sender] = totalSupply;
    }

    // Modifier to restrict function to owner
    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    // Get the balance of an account
    function balanceOf(address account) external view returns (uint256) {
        return balances[account];
    }

    // Transfer tokens to a given address
    function transfer(address to, uint256 value) external returns (bool) {
        require(to != address(0), "Invalid address");
        require(balances[msg.sender] >= value, "Insufficient balance");
        
        balances[msg.sender] -= value;
        balances[to] += value;
        emit Transfer(msg.sender, to, value);
        return true;
    }

    // Approve a spender to use a specific amount of tokens
    function approve(address spender, uint256 value) external returns (bool) {
        allowances[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    // Transfer tokens from a specific address to another address
    function transferFrom(address from, address to, uint256 value) external returns (bool) {
        require(from != address(0), "Invalid address");
        require(to != address(0), "Invalid address");
        require(balances[from] >= value, "Insufficient balance");
        require(allowances[from][msg.sender] >= value, "Allowance exceeded");
        
        balances[from] -= value;
        balances[to] += value;
        allowances[from][msg.sender] -= value;
        emit Transfer(from, to, value);
        return true;
    }

    // Mint new tokens and add them to an account
    function mint(address account, uint256 value) external onlyOwner {
        require(account != address(0), "Invalid address");
        
        balances[account] += value;
        totalSupply += value;
        emit Transfer(address(0), account, value);
    }

    // Burn a specific amount of tokens from the sender's balance
    function burn(uint256 value) external {
        require(balances[msg.sender] >= value, "Insufficient balance");
        
        balances[msg.sender] -= value;
        totalSupply -= value;
        emit Transfer(msg.sender, address(0), value);
    }

    // Add liquidity-related functions here
    // ...
}