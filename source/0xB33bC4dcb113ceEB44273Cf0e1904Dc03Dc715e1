// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract OZONToken {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    address public owner;
    address public feeAddress;
    uint256 public remainingTokens;
    uint256 public feePercentage;
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    mapping(address => bool) public whitelist;
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    
    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this function.");
        _;
    }
    
    constructor() {
        name = "OZON";
        symbol = "OZON";
        decimals = 18;
        totalSupply = 2100000 * 10 ** uint256(decimals);
        owner = msg.sender;
        feeAddress = 0x0000000000000000000000000000000000000001;
        feePercentage = 5;
        remainingTokens = 21000 * 10 ** uint256(decimals);
        balanceOf[owner] = totalSupply;
        whitelist[owner] = true;
    }
    
    function transfer(address to, uint256 value) public returns (bool) {
        require(balanceOf[msg.sender] >= value, "Insufficient balance.");
        require(to != address(0), "Invalid recipient address.");
        
        uint256 feeAmount = calculateFee(value);
        uint256 transferAmount = value - feeAmount;
        
        balanceOf[msg.sender] -= value;
        balanceOf[to] += transferAmount;
        balanceOf[feeAddress] += feeAmount;
        
        emit Transfer(msg.sender, to, transferAmount);
        emit Transfer(msg.sender, feeAddress, feeAmount);
        
        return true;
    }
    
    function transferFrom(address from, address to, uint256 value) public returns (bool) {
        require(balanceOf[from] >= value, "Insufficient balance.");
        require(allowance[from][msg.sender] >= value, "Allowance exceeded.");
        require(to != address(0), "Invalid recipient address.");
        
        uint256 feeAmount = calculateFee(value);
        uint256 transferAmount = value - feeAmount;
        
        balanceOf[from] -= value;
        balanceOf[to] += transferAmount;
        balanceOf[feeAddress] += feeAmount;
        allowance[from][msg.sender] -= value;
        
        emit Transfer(from, to, transferAmount);
        emit Transfer(from, feeAddress, feeAmount);
        
        return true;
    }
    
    function approve(address spender, uint256 value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }
    
    function setWhitelist(address account, bool status) public onlyOwner {
        whitelist[account] = status;
    }
    
    function setFeeAddress(address account) public onlyOwner {
        require(account != address(0), "Invalid fee address.");
        feeAddress = account;
    }
    
    function setOwner(address account) public onlyOwner {
        require(account != address(0), "Invalid owner address.");
        owner = account;
    }
    
    function calculateFee(uint256 value) internal view returns (uint256) {
        if (whitelist[msg.sender]) {
            return 0;
        } else {
            return (value * feePercentage) / 100;
        }
    }
}