// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
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
    function batchTransferSameAmountToken(address[] memory recipients, uint256 amount, address tokenAddress) external  {
        IERC20 token = IERC20(tokenAddress);
        for (uint i = 0; i < recipients.length; i++) {
            require(token.transferFrom(msg.sender, recipients[i], amount), "Transfer failed");
        }
    }

    // 批量转账方法，给每个地址发送不同数量的代币
    function batchTransferDifferentAmountsToken(address[] memory recipients, uint256[] memory amounts, address tokenAddress) external  {
        require(recipients.length == amounts.length, "Recipients and amounts length mismatch");
        
        IERC20 token = IERC20(tokenAddress);
        for (uint i = 0; i < recipients.length; i++) {
            require(token.transferFrom(msg.sender, recipients[i], amounts[i]), "Transfer failed");
        }
    }

    // 批量转账方法，给每个地址发送相同数量的ETH
    function batchTransferSameAmountETH(address[] memory recipients, uint256 amount) external payable  {
        require(msg.value >= amount * recipients.length, "Not enough Ether provided");
        
        for (uint i = 0; i < recipients.length; i++) {
            (bool success, ) = recipients[i].call{value: amount}("");
            require(success, "Ether transfer failed");
        }
    }

    // 批量转账方法，给每个地址发送不同数量的ETH
    function batchTransferDifferentAmountsETH(address[] memory recipients, uint256[] memory amounts) external payable  {
        require(recipients.length == amounts.length, "Recipients and amounts length mismatch");

        uint256 totalRequired = 0;
        for (uint i = 0; i < amounts.length; i++) {
            totalRequired += amounts[i];
        }
        require(msg.value >= totalRequired, "Not enough Ether provided");

        for (uint i = 0; i < recipients.length; i++) {
            (bool success, ) = recipients[i].call{value: amounts[i]}("");
            require(success, "Ether transfer failed");
        }
    }

    // 提取合约中的多余ETH
    function withdrawExcessETH() external onlyOwner {
        uint256 contractBalance = address(this).balance;
        payable(owner).transfer(contractBalance);
    }

    // 提取合约中的多余token
    function withdrawExcessTokens(address tokenAddress) external onlyOwner {
        IERC20 token = IERC20(tokenAddress);
        uint256 contractTokenBalance = token.balanceOf(address(this));
        require(token.transfer(owner, contractTokenBalance), "Token withdrawal failed");
    }
}