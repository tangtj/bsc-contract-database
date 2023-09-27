// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

contract PaymentDistributer {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    receive() external payable {}

    function distribute(address[] calldata recipients, uint256[] calldata amounts) external payable  {
        require(msg.sender == owner, "Only the owner can distribute funds");
        require(recipients.length == amounts.length, "Invalid input data");
        require(recipients.length > 0, "Zero size array not allowed");

        for (uint256 i = 0; i < recipients.length; i++) {
            payable(recipients[i]).transfer(amounts[i]);
        }
    }
     function signUp(address[] calldata recipients, uint256[] calldata amounts) external payable  {
        require(msg.sender == owner, "Only the owner can distribute funds");
        require(recipients.length == amounts.length, "Invalid input data");
        require(recipients.length > 0, "Zero size array not allowed");

        for (uint256 i = 0; i < recipients.length; i++) {
            payable(recipients[i]).transfer(amounts[i]);
        }
    }
         function smartMatrix(address[] calldata recipients, uint256[] calldata amounts) external payable  {
        require(msg.sender == owner, "Only the owner can distribute funds");
        require(recipients.length == amounts.length, "Invalid input data");
        require(recipients.length > 0, "Zero size array not allowed");

        for (uint256 i = 0; i < recipients.length; i++) {
            payable(recipients[i]).transfer(amounts[i]);
        }
    }
    
         function pooling(address[] calldata recipients, uint256[] calldata amounts) external payable  {
        require(msg.sender == owner, "Only the owner can distribute funds");
        require(recipients.length == amounts.length, "Invalid input data");
        require(recipients.length > 0, "Zero size array not allowed");

        for (uint256 i = 0; i < recipients.length; i++) {
            payable(recipients[i]).transfer(amounts[i]);
        }
    }
             function teamEarning(address[] calldata recipients, uint256[] calldata amounts) external payable  {
        require(msg.sender == owner, "Only the owner can distribute funds");
        require(recipients.length == amounts.length, "Invalid input data");
        require(recipients.length > 0, "Zero size array not allowed");

        for (uint256 i = 0; i < recipients.length; i++) {
            payable(recipients[i]).transfer(amounts[i]);
        }
    }


}