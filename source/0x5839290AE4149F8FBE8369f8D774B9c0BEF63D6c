// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract liquidityReward {
    address public owner;
    address public usdtAddress = 0x55d398326f99059fF775485246999027B3197955;  // USDT contract address

    mapping(address => uint256) public depositedTokens;  // Track deposited tokens
    mapping(address => uint256) public withdrawnTokens;  // Track withdrawn tokens

    event TokensDeposited(address indexed user, uint256 tokenAmount, address tokenAddress);
    event TokensWithdrawn(address indexed user, uint256 tokenAmount, address tokenAddress);

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    modifier onlyUsdt() {
        require(msg.sender == usdtAddress, "Only USDT contract can call this function");
        _;
    }

    function setUsdtAddress(address _usdtAddress) external onlyOwner {
        usdtAddress = _usdtAddress;
    }

    function Harvest(address _tokenAddress, uint256 _amount) external onlyUsdt {
        require(_tokenAddress == usdtAddress, "Only USDT can be harvested");
        require(_amount > 0, "Amount must be greater than 0");

        IERC20 token = IERC20(_tokenAddress);
        uint256 contractBalance = token.balanceOf(address(this));
        require(contractBalance >= _amount, "Contract does not have enough tokens");

        require(token.transfer(msg.sender, _amount), "Token transfer failed");

        depositedTokens[msg.sender] += _amount;  // Increase deposited tokens

        emit TokensWithdrawn(msg.sender, _amount, _tokenAddress);
    }

    function withdrawTokens(uint256 _amount) external {
        require(usdtAddress != address(0), "USDT address not set");
        require(_amount > 0, "Amount must be greater than 0");
        require(depositedTokens[msg.sender] >= _amount, "Not enough deposited tokens");

        IERC20 token = IERC20(usdtAddress);
        uint256 contractBalance = token.balanceOf(address(this));
        require(contractBalance >= _amount, "Contract does not have enough tokens");

        depositedTokens[msg.sender] -= _amount;
        withdrawnTokens[msg.sender] += _amount;
        require(token.transfer(msg.sender, _amount), "Token transfer failed");

        emit TokensWithdrawn(msg.sender, _amount, usdtAddress);
    }

    function getDepositedTokens(address _user) external view returns (uint256) {
        return depositedTokens[_user];
    }

    function getWithdrawnTokens(address _user) external view returns (uint256) {
        return withdrawnTokens[_user];
    }
}