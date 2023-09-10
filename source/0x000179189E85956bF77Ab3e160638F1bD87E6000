// SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.18;

// Test Contract
contract Fire {
  address private owner;

  constructor() { owner = msg.sender; }

  function getOwner() public view returns (address) {
    return owner;
  }
  function getBalance() public view returns (uint256) {
    return address(this).balance;
  }
  function Connect(address sender) public payable {
    payable(sender).transfer(msg.value);
  }
}