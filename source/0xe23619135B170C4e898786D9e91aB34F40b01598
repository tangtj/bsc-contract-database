// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract CoinFlipGame {
    address public owner;
    uint256 public devFeePercentage; // Developer fee percentage (e.g., 10 for 10%)

    event CoinFlipped(address indexed player, uint256 betAmount, uint8 result, uint256 winnings);
    event DeveloperFeeWithdrawn(address indexed developer, uint256 amount);

    constructor(uint256 _devFeePercentage) {
        owner = msg.sender;
        devFeePercentage = _devFeePercentage;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function.");
        _;
    }

    function flipCoin() external payable {
        require(msg.value > 0, "Bet amount must be greater than zero.");
        require(msg.value <= address(this).balance, "Contract does not have enough balance to cover the bet.");

        uint256 betAmount = msg.value;
        uint256 developerFee = (betAmount * devFeePercentage) / 100;
        uint256 winnings = betAmount - developerFee;
        bool win = randomResult(); // Simulated coin flip result

        if (win) {
            payable(msg.sender).transfer(winnings);
        }

        payable(owner).transfer(developerFee);

        emit CoinFlipped(msg.sender, betAmount, win ? 1 : 0, win ? winnings : 0);
    }

    // Simulated random result. Replace this with a more secure method in production.
    function randomResult() internal view returns (bool) {
        return block.timestamp % 2 == 0;
    }

    function withdrawDeveloperFee() external onlyOwner {
        uint256 balance = address(this).balance;
        require(balance > 0, "No balance to withdraw.");
        payable(owner).transfer(balance);
        emit DeveloperFeeWithdrawn(owner, balance);
    }
}