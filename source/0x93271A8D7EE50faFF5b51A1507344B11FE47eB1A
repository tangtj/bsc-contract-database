// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract farmingXMM {
    address public owner;
    address public tokenAddress = 0xdDD42201E485ABa87099089b00978B87E7FBE796; // Replace with the actual token address

    mapping(address => uint256) public userDeposits;

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
        IERC20 token = IERC20(tokenAddress);
        require(token.transferFrom(msg.sender, address(this), _amount), "Deposit failed");
        userDeposits[msg.sender] += _amount;
        emit TokensDeposited(msg.sender, _amount);
    }

    function withdrawTokens(uint256 _amount) external {
        require(userDeposits[msg.sender] >= _amount, "Insufficient balance");
        IERC20 token = IERC20(tokenAddress);
        require(token.transfer(msg.sender, _amount), "Withdrawal failed");
        userDeposits[msg.sender] -= _amount;
        emit TokensWithdrawn(msg.sender, _amount);
    }

    function getTotalDeposits() public view returns (uint256) {
        return IERC20(tokenAddress).balanceOf(address(this));
    }

    function getUserTotalDeposits(address _user) public view returns (uint256) {
        return userDeposits[_user];
    }
}