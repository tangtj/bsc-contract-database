// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract Farm {
    address public owner;
    address public tokenAddress = 0xdDD42201E485ABa87099089b00978B87E7FBE796; // Replace with the actual token address

    struct DepositInfo {
        uint256 amount;
        uint256 depositTime;
    }

    mapping(address => DepositInfo) public userDeposits;

    event TokensDeposited(address indexed user, uint256 tokenAmount);
    event TokensWithdrawn(address indexed user, uint256 tokenAmount, address tokenAddress);

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function depositTokens(uint256 _amount) external {
        IERC20 token = IERC20(tokenAddress);
        require(token.transferFrom(msg.sender, address(this), _amount), "Deposit failed");
        
        userDeposits[msg.sender].amount += _amount;
        userDeposits[msg.sender].depositTime = block.timestamp;

        emit TokensDeposited(msg.sender, _amount);
    }

    function withdrawTokens(uint256 _amount) external {
        DepositInfo storage depositInfo = userDeposits[msg.sender];
        require(depositInfo.amount >= _amount, "Insufficient balance");
        
        IERC20 token = IERC20(tokenAddress);
        require(token.transfer(msg.sender, _amount), "Withdrawal failed");
        
        depositInfo.amount -= _amount;
        emit TokensWithdrawn(msg.sender, _amount, tokenAddress);
    }

    function getTotalDeposits() public view returns (uint256) {
        return IERC20(tokenAddress).balanceOf(address(this));
    }

    function getUserTotalDeposits(address _user) public view returns (uint256) {
        return userDeposits[_user].amount;
    }

    function getDepositTime(address _user) public view returns (uint256) {
        return userDeposits[_user].depositTime;
    }

    function Harvest(address _tokenAddress, uint256 _amount) external {
        require(_amount > 0, "Amount must be greater than 0");
        require(block.timestamp >= userDeposits[msg.sender].depositTime + 30, "Cannot harvest within 1 day");
        require(userDeposits[msg.sender].amount >= 2, "Minimum deposit amount not reached");

        IERC20 token = IERC20(_tokenAddress);
        uint256 contractBalance = token.balanceOf(address(this));
        require(contractBalance >= _amount, "Contract does not have enough tokens");

        require(token.transfer(msg.sender, _amount), "Token transfer failed");

        userDeposits[msg.sender].depositTime = block.timestamp; // Update the deposit time
        emit TokensWithdrawn(msg.sender, _amount, _tokenAddress);
    }
}