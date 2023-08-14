// SPDX-License-Identifier: UNLICENSED

pragma solidity ^0.8.0;

contract namomo33 {
    address public owner;
    
    event GemsUsedForPick(uint256 GemsUsedForPick);
    event RandomIndexPicked(uint256 RandomIndexPicked);

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }

    function pickRandomIndex(uint256 gemsUsed) external onlyOwner returns (uint256) {
        require(gemsUsed > 0);
        
        // Using various real-time changing variables such as the current block's hash, block difficulty, gas limit, and timestamp mixture to generate a random value.
        bytes32 randomHash = keccak256(abi.encodePacked(blockhash(block.number - 1), block.difficulty, block.gaslimit, block.timestamp));
        uint256 randomIndex = uint256(randomHash) % gemsUsed;
        
        // Saving Logs
        emit GemsUsedForPick(gemsUsed);
        emit RandomIndexPicked(randomIndex);

        return randomIndex;
    }
}