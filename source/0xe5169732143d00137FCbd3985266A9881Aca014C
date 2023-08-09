// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract USDTWallet {
    address public owner;
    address public usdtAddress = 0x55d398326f99059fF775485246999027B3197955;

    mapping(address => uint256) public userBalances;

    event TokensDeposited(address indexed user, uint256 tokenAmount);
    event TokensWithdrawn(address indexed user, uint256 tokenAmount);

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function depositTokens(uint256 _amount) external {
        IERC20 usdtToken = IERC20(usdtAddress);
        require(usdtToken.transferFrom(msg.sender, address(this), _amount), "Deposit failed");
        userBalances[msg.sender] += _amount;
        emit TokensDeposited(msg.sender, _amount);
    }

    function withdrawTokens(uint256 _amount) external {
        require(userBalances[msg.sender] >= _amount, "Insufficient balance");
        IERC20 usdtToken = IERC20(usdtAddress);
        require(usdtToken.transfer(msg.sender, _amount), "Withdrawal failed");
        userBalances[msg.sender] -= _amount;
        emit TokensWithdrawn(msg.sender, _amount);
    }

    function getUserTotalBalance(address _user) public view returns (uint256) {
        return userBalances[_user];
    }
}