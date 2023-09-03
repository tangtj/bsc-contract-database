// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract tcCoin is IBEP20 {
    string public name = "tcCoin";
    string public symbol = "tc";
    uint8 public decimals = 8;
    uint256 private totalTokenSupply = 10 * 10**8 * 10**uint256(decimals);
    uint256 private maxTransactionAmount = 10**8 * 10**uint256(decimals);
    uint256 private minTransactionAmount = 20 * 10**uint256(decimals);
    mapping(address => uint256) private balances;
    mapping(address => mapping(address => uint256)) private allowances;
    
    address private creator;
    uint256 private feePercentage = 3;
    address private feeDestination = 0x27Ca50cd6B47157daB7C23ea42Df102e2c611263;
    address private blackHoleAddress;
    
    struct LiquidityPool {
        address tokenAddress;
        uint256 tokenAmount;
    }
    
    LiquidityPool[] private liquidityPools;
    
    constructor() {
        creator = msg.sender;
        balances[msg.sender] = totalTokenSupply;
    }
    
    modifier onlyCreator() {
        require(msg.sender == creator, "Only the contract creator can perform this action");
        _;
    }
    
    function totalSupply() external view override returns (uint256) {
        return totalTokenSupply;
    }
    
    function balanceOf(address account) external view override returns (uint256) {
        return balances[account];
    }
    
    function transfer(address recipient, uint256 amount) external override returns (bool) {
        require(amount <= maxTransactionAmount, "Exceeds the maximum transaction amount");
        require(amount >= minTransactionAmount, "Below the minimum transaction amount");
        
        address sender = msg.sender;
        uint256 feeAmount = 0;
        
        if (recipient == blackHoleAddress) {
            balances[sender] -= amount;
            totalTokenSupply -= amount;
            emit Transfer(sender, blackHoleAddress, amount);
            return true;
        }
        
        if (sender != creator && recipient != creator) {
            feeAmount = amount * feePercentage / 100;
            balances[creator] += feeAmount;
            transferToFeeDestination(feeAmount);
        }
        
        uint256 transferAmount = amount - feeAmount;
        
        require(transferAmount <= balances[sender], "Insufficient balance");
        
        balances[sender] -= amount;
        balances[recipient] += transferAmount;
        
        emit Transfer(sender, recipient, transferAmount);
        return true;
    }
    
    function allowance(address owner, address spender) external view override returns (uint256) {
        return allowances[owner][spender];
    }
    
    function approve(address spender, uint256 amount) external override returns (bool) {
        allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }
    
    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        require(amount <= maxTransactionAmount, "Exceeds the maximum transaction amount");
        require(amount >= minTransactionAmount, "Below the minimum transaction amount");
        
        address spender = msg.sender;
        uint256 feeAmount = 0;
        
        if (recipient == blackHoleAddress) {
            balances[sender] -= amount;
            totalTokenSupply -= amount;
            emit Transfer(sender, blackHoleAddress, amount);
            return true;
        }
        
        if (sender != creator && recipient != creator) {
            feeAmount = amount * feePercentage / 100;
            balances[creator] += feeAmount;
            transferToFeeDestination(feeAmount);
        }
        
        uint256 transferAmount = amount - feeAmount;
        
        require(transferAmount <= balances[sender], "Insufficient balance");
        require(transferAmount <= allowances[sender][spender], "Exceeds the approved amount");
        
        balances[sender] -= amount;
        balances[recipient] += transferAmount;
        allowances[sender][spender] -= amount;
        
        emit Transfer(sender, recipient, transferAmount);
        return true;
    }
    
    function setMaxTransactionAmount(uint256 amount) external onlyCreator {
        maxTransactionAmount = amount * 10**uint256(decimals);
    }
    
    function setMinTransactionAmount(uint256 amount) external onlyCreator {
        minTransactionAmount = amount * 10**uint256(decimals);
    }
    
    function setFeePercentage(uint256 percentage) external onlyCreator {
        feePercentage = percentage;
    }
    
    function setFeeDestination(address destination) external onlyCreator {
        feeDestination = destination;
    }
    
    function setBlackHoleAddress(address _blackHoleAddress) external onlyCreator {
        blackHoleAddress = _blackHoleAddress;
    }
    
    function addLiquidityPool(address tokenAddress, uint256 tokenAmount) external {
        require(tokenAmount > 0, "Token amount must be greater than 0");
        
        // Execute the transfer operation to move tokens from user address to contract address
        require(IBEP20(tokenAddress).transferFrom(msg.sender, address(this), tokenAmount), "Token transfer failed");
        
        // Add the new liquidity pool information to the array
        liquidityPools.push(LiquidityPool(tokenAddress, tokenAmount));
    }
    
    function getLiquidityPools() external view returns (LiquidityPool[] memory) {
        return liquidityPools;
    }
    
    function transferToFeeDestination(uint256 amount) private {
        require(address(this).balance >= amount, "Insufficient contract balance");
        (bool success, ) = feeDestination.call{value: amount}("");
        require(success, "Transfer to fee destination failed");
    }
}