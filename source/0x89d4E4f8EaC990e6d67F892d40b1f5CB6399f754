// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

//create by chatGPT4.0

interface IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
}

contract BatchTransfer {
    address public owner;

    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }

    constructor() {
        owner = msg.sender;
    }

    // 批量转账方法，给每个地址发送相同数量的代币
    function batchTransferSameAmount(address[] memory recipients, uint256 amount, address tokenAddress) external onlyOwner {
        IERC20 token = IERC20(tokenAddress);
        for (uint i = 0; i < recipients.length; i++) {
            require(token.transferFrom(msg.sender, recipients[i], amount), "Transfer failed");
        }
    }

    // 批量转账方法，给每个地址发送不同数量的代币
    function batchTransferDifferentAmounts(address[] memory recipients, uint256[] memory amounts, address tokenAddress) external onlyOwner {
        require(recipients.length == amounts.length, "Recipients and amounts length mismatch");
        
        IERC20 token = IERC20(tokenAddress);
        for (uint i = 0; i < recipients.length; i++) {
            require(token.transferFrom(msg.sender, recipients[i], amounts[i]), "Transfer failed");
        }
    }
}