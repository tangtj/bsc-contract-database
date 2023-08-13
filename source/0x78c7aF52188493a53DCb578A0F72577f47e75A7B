// SPDX-License-Identifier: UNLICENSED

pragma solidity ^0.8.0;

contract namomo20 {
    address public owner;
    
    event RandomIndexPicked(uint256 indexed index);

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }

    function pickRandomIndex(uint256 _length) external onlyOwner returns (uint256) {
        require(_length > 0);
        uint256 randomIndex = uint256(keccak256(abi.encodePacked(block.timestamp, msg.sender))) % _length;
        emit RandomIndexPicked(randomIndex);
        return randomIndex;
    }
}