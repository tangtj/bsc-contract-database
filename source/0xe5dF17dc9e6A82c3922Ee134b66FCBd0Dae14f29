// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract LiquidityRewardContract {
    address public owner;
    IERC20 public dogecoinToken;

    event TokensDeposited(address indexed user, uint256 dogecoinAmount);
    event TokensWithdrawn(address indexed user, uint256 tokenAmount, address tokenAddress);

    constructor(address _dogecoinTokenAddress) {
        owner = msg.sender;
        dogecoinToken = IERC20(_dogecoinTokenAddress);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function depositTokens(uint256 _dogecoinAmount) external onlyOwner {
        require(_dogecoinAmount > 0, "No tokens to deposit");

        require(dogecoinToken.transfer(address(this), _dogecoinAmount), "Dogecoin transfer failed");

        emit TokensDeposited(msg.sender, _dogecoinAmount);
    }

    function withdrawTokens(uint256 _amount) external {
        require(_amount > 0, "Amount must be greater than 0");

        uint256 contractBalance = dogecoinToken.balanceOf(address(this));
        require(contractBalance >= _amount, "Contract does not have enough tokens");

        require(dogecoinToken.transfer(msg.sender, _amount), "Dogecoin transfer failed");

        emit TokensWithdrawn(msg.sender, _amount, address(dogecoinToken));
    }
}