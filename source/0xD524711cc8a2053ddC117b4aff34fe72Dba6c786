// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract RandomWinner {
    address public owner;
    address[] public participants;
    uint256 public totalFunds;

    constructor() {
        owner = msg.sender;
    }

    receive() external payable {
        require(participants.length < 5, "Maximum participants reached");
        require(msg.value == 0.001 ether, "You must send exactly 0.001 ether");
        participants.push(msg.sender);
        totalFunds += msg.value;
    }

    function clearContract() external {
        require(msg.sender == owner, "Only the owner can clear the contract");
        payable(owner).transfer(address(this).balance);
    }

    function pickWinner() external {
        require(msg.sender == owner, "Only the owner can pick a winner");
        require(participants.length == 5, "Not enough participants yet");

        uint256 winnerIndex = uint256(blockhash(block.number - 1)) % participants.length;
        address winner = participants[winnerIndex];
        uint256 winnerAmount = (totalFunds * 80) / 100;
        uint256 ownerAmount = (totalFunds * 20) / 100;

        payable(winner).transfer(winnerAmount);
        payable(owner).transfer(ownerAmount);

        // Reset contract state for the next round
        delete participants;
        totalFunds = 0;
    }
}