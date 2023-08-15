// SPDX-License-Identifier: UNLICENSED

pragma solidity ^0.8.0;

contract mamba101 {
    address public owner;
    
    event TotalUsedGems(uint256 TotalUsedGems);
    event RandomPickedIndex(uint256 RandomPickedIndex);

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }

    function pickRandomIndex(uint256 gemsUsed) external onlyOwner returns (uint256) {
        require(gemsUsed > 0);
        
        // Use various real-time changing variables such as the current block's hash, block difficulty, gas limit, and timestamp mixture and generate a random number.
        bytes32 randomHash = keccak256(abi.encodePacked(blockhash(block.number - 1), block.difficulty, block.gaslimit, block.timestamp));
        uint256 randomIndex = uint256(randomHash) % gemsUsed;
        
        // Saving Logs
        emit TotalUsedGems(gemsUsed);
        emit RandomPickedIndex(randomIndex); 

        return randomIndex;
    }
}