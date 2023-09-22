// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract RandomWinner {
    address public owner;
    uint256 public totalFunds;
    uint256 public playerCount; // Track the number of players
    uint256 public requiredAmount = 0.005 ether; // Initial required amount

    event WinnerPicked(string category, address winner, uint256 amount);

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function setRequiredAmount(uint256 newAmount) external onlyOwner {
        requiredAmount = newAmount;
    }

    receive() external payable {
        require(msg.value == requiredAmount, "You must send the required amount");

        playerCount++; // Increment player count
        totalFunds += msg.value;

        emit WinnerPicked("Participant Joined", msg.sender, msg.value);

        if (playerCount >= 5) {
            pickWinners();
        }
    }

    function clearContract() external onlyOwner {
        payable(owner).transfer(address(this).balance);
    }

    function pickWinners() internal {
        require(playerCount >= 5, "Not enough participants yet");

        uint256 totalAmount = totalFunds;
        uint256 winnerAmount1 = (totalAmount * 50) / 100;
        uint256 winnerAmount2 = (totalAmount * 20) / 100;
        uint256 winnerAmount3 = (totalAmount * 10) / 100;
        uint256 ownerAmount = totalAmount - winnerAmount1 - winnerAmount2 - winnerAmount3;

        for (uint256 i = 0; i < 3; i++) {
            address winner = address(uint160(uint256(keccak256(abi.encodePacked(blockhash(block.number - 1), block.coinbase, block.gaslimit, i))) % playerCount));

            uint256 winnerAmount;

            if (i == 0) {
                winnerAmount = winnerAmount1;
                emit WinnerPicked("Gold", winner, winnerAmount);
            } else if (i == 1) {
                winnerAmount = winnerAmount2;
                emit WinnerPicked("Silver", winner, winnerAmount);
            } else {
                winnerAmount = winnerAmount3;
                emit WinnerPicked("Bronze", winner, winnerAmount);
            }

            payable(winner).transfer(winnerAmount);
        }

        payable(owner).transfer(ownerAmount);

        totalFunds = 0;
        playerCount = 0; // Reset player count
    }
}