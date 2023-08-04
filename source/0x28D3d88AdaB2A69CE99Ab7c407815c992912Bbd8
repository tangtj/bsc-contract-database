// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;

contract Token {
    mapping(address => uint) public balances;
    mapping(address => mapping(address => uint)) public allowance;
    uint public totalSupply = 100000000 * 10 ** 18;
    string public name = "dtoothfairy";
    string public symbol = "DTTF";
    uint public decimals = 18;

    // Added variables for liquidity and marketing fees
    uint public liquidityFee = 4; // 2% liquidity fee
    uint public marketingFee = 5; // 1% marketing fee
    address public liquidityWallet;
    address public marketingWallet;

    // Added variable for max token per transaction and max total tokens per wallet
    uint public maxTokensPerTransaction = 100 * 10 ** 18; // 100 tokens
    uint public maxTokensPerWallet = 1000000 * 10 ** 18; // 1000 tokens

    // Added variables to pause/unpause trade and track contract ownership
    bool public tradingPaused = false;
    address public owner;

    // Added variable to track whether a transfer is a buy or sell transaction
    mapping(address => bool) public isExcludedFromFee;
    
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
    event TradingPaused(bool paused);
    event MaxTokensPerTransactionUpdated(uint newValue);
    event MaxTokensPerWalletUpdated(uint newValue);
    event LiquidityFeeUpdated(uint newFee);
    event MarketingFeeUpdated(uint newFee);
    event LiquidityWalletUpdated(address indexed newWallet);
    event MarketingWalletUpdated(address indexed newWallet);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function");
        _;
    }

    modifier tradeNotPaused() {
        require(!tradingPaused, "Trading is currently paused");
        _;
    }

    constructor(address _liquidityWallet, address _marketingWallet) {
        owner = msg.sender;
        balances[msg.sender] = totalSupply;
        // The contract owner is excluded from both buy and sell fees
        isExcludedFromFee[msg.sender] = true;

        // Set the liquidity and marketing wallet addresses during contract deployment
        liquidityWallet = _liquidityWallet;
        marketingWallet = _marketingWallet;
    }

    function balanceOf(address account) public view returns(uint) {
        return balances[account];
    }

    function transfer(address to, uint value) public tradeNotPaused returns(bool) {
        require(to != address(0), "Invalid address");
        require(value > 0, "Value must be greater than 0");
        require(value <= balances[msg.sender], "Insufficient balance");

        // Check max tokens per transaction
        require(value <= maxTokensPerTransaction, "Exceeds max tokens per transaction");

        // Check max tokens per wallet
        require(balances[to] + value <= maxTokensPerWallet, "Exceeds max tokens per wallet");

        uint feeAmount = 0;
        if (!isExcludedFromFee[msg.sender]) {
            // If it's not the contract owner, then it's a sell transaction
            feeAmount = (value * liquidityFee) / 100;
        }

        uint transferAmount = value - feeAmount;

        balances[to] += transferAmount;
        balances[msg.sender] -= value;

        emit Transfer(msg.sender, to, transferAmount);
        if (feeAmount > 0) {
            // Send liquidity fee to the liquidity wallet
            balances[liquidityWallet] += feeAmount;
            emit Transfer(msg.sender, liquidityWallet, feeAmount);
        }

        return true;
    }

    function transferFrom(address from, address to, uint value) public tradeNotPaused returns(bool) {
        require(from != address(0), "Invalid 'from' address");
        require(to != address(0), "Invalid 'to' address");
        require(value > 0, "Value must be greater than 0");
        require(value <= balances[from], "Insufficient balance");

        // Check max tokens per transaction
        require(value <= maxTokensPerTransaction, "Exceeds max tokens per transaction");

        // Check max tokens per wallet
        require(balances[to] + value <= maxTokensPerWallet, "Exceeds max tokens per wallet");

        // Check allowance
        require(value <= allowance[from][msg.sender], "Allowance exceeded");

        uint feeAmount = 0;
        if (!isExcludedFromFee[from]) {
            // If the 'from' address is not excluded, then it's a buy transaction
            feeAmount = (value * marketingFee) / 100;
        }

        uint transferAmount = value - feeAmount;

        balances[to] += transferAmount;
        balances[from] -= value;
        allowance[from][msg.sender] -= value;

        emit Transfer(from, to, transferAmount);
        if (feeAmount > 0) {
            // Send marketing fee to the marketing wallet
            balances[marketingWallet] += feeAmount;
            emit Transfer(from, marketingWallet, feeAmount);
        }

        return true;
    }

    function approve(address spender, uint value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;   
    }

    // Function to pause/unpause trading (only the owner can call this)
    function setTradingPaused(bool paused) public onlyOwner {
        tradingPaused = paused;
        emit TradingPaused(paused);
    }

    // Function to update max tokens per transaction (only the owner can call this)
    function setMaxTokensPerTransaction(uint newValue) public onlyOwner {
        maxTokensPerTransaction = newValue;
        emit MaxTokensPerTransactionUpdated(newValue);
    }

    // Function to update max tokens per wallet (only the owner can call this)
    function setMaxTokensPerWallet(uint newValue) public onlyOwner {
        maxTokensPerWallet = newValue;
        emit MaxTokensPerWalletUpdated(newValue);
    }

    // Function to update liquidity fee (only the owner can call this)
    function setLiquidityFee(uint newFee) public onlyOwner {
        liquidityFee = newFee;
        emit LiquidityFeeUpdated(newFee);
    }

    // Function to update marketing fee (only the owner can call this)
    function setMarketingFee(uint newFee) public onlyOwner {
        marketingFee = newFee;
        emit MarketingFeeUpdated(newFee);
    }

    // Function to transfer contract ownership (only the owner can call this)
    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Invalid address");
        owner = newOwner;
    }

    // Function to exclude an address from fees (only the owner can call this)
    function setExcludedFromFee(address account, bool excluded) public onlyOwner {
        isExcludedFromFee[account] = excluded;
    }

    // Function to update the liquidity wallet address (only the owner can call this)
    function setLiquidityWallet(address newWallet) public onlyOwner {
        require(newWallet != address(0), "Invalid address");
        liquidityWallet = newWallet;
        emit LiquidityWalletUpdated(newWallet);
    }

    // Function to update the marketing wallet address (only the owner can call this)
    function setMarketingWallet(address newWallet) public onlyOwner {
        require(newWallet != address(0), "Invalid address");
        marketingWallet = newWallet;
        emit MarketingWalletUpdated(newWallet);
    }
}