// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract DataUrl {
    string[] private data;
    address private owner;

    constructor() {
        owner = msg.sender;        
    }

    modifier OnlyOwner() {
        require(msg.sender == owner, "no owner");
        _;
    }

    function getData() external view OnlyOwner returns (string[] memory) {
        return data;
    }

    function pushData(string memory dataValue) external OnlyOwner {
        data.push(dataValue);
    }
}