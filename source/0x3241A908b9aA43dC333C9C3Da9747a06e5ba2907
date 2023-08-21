// SPDX-License-Identifier: MIT

pragma solidity ^0.8.7;

contract bridge_cointech_dev {
    address payable public owner;
    
    event Deposit(address indexed sender, uint256 amount);

    constructor() {
        owner = payable(msg.sender);
    }

    receive() external payable {
        require(msg.value >= 0.01 ether && msg.value <= 0.1 ether, "Send between 0.01 and 0.1 BNB");
        emit Deposit(msg.sender, msg.value);
    }

    function withdraw(uint256 amount) external {
        require(msg.sender == owner, "Only the contract owner can withdraw");
        require(address(this).balance >= amount, "Insufficient balance");

        owner.transfer(amount);
    }
}