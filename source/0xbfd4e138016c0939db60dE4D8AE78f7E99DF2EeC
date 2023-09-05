// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract BatchTransfer {
    address public owner;
    
    modifier onlyOwner() {
        require(msg.sender == owner, "Only contract owner can call this function");
        _;
    }

    constructor() {
        owner = msg.sender;
    }

    function batchTransfer(address[] memory recipients, uint256[] memory amounts, address tokenAddress) public onlyOwner {
        require(recipients.length == amounts.length, "Invalid input");

        for (uint256 i = 0; i < recipients.length; i++) {
            (bool success, ) = tokenAddress.call(abi.encodeWithSignature("transfer(address,uint256)", recipients[i], amounts[i]));
            require(success, "Transfer failed");
        }
    }

    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Invalid address");
        owner = newOwner;
    }
}