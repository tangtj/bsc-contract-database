// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract TransferContract {
    
    // 合约的所有者，通常是部署合约的地址
    address public owner;

    // 在合约创建时，设置合约所有者为部署者
    constructor() {
        owner = msg.sender;
    }

    // 确保只有合约所有者可以调用某些函数
    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can execute this");
        _;
    }

    // 允许合约所有者从合约转账到指定地址
    function transferFunds(address payable recipient, uint256 amount) external onlyOwner {
        require(address(this).balance >= amount, "Insufficient contract balance");
        recipient.transfer(amount);
    }

    // 允许任何人向合约发送资金
    receive() external payable {}

    // 查询合约余额
    function getBalance() external view returns (uint256) {
        return address(this).balance;
    }
}