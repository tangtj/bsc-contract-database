// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
}

contract CadinuAirDrop {
    IERC20 public token;
    address public owner;

    event TransferredTokens(address[] recipients, uint256 value);
    event FailedTransfer(address recipient, uint256 value);

    constructor() {
        owner = msg.sender;
        token = IERC20(0x6e64fCF15Be3eB71C3d42AcF44D85bB119b2D98b);  // Set your token address here
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function");
        _;
    }

    function transferOwnership(address newOwner) external onlyOwner {
        require(newOwner != address(0), "New owner cannot be the zero address");
        owner = newOwner;
    }
  
    function cadinuAirDrop(address[] calldata recipients, uint256 value) external onlyOwner {
        uint256 valueInWei = value * 10**18; // Convert the value to the token's smallest unit
        uint256 totalToSend = valueInWei * recipients.length;
        uint256 contractBalance = token.balanceOf(address(this));
        require(contractBalance >= totalToSend, "Not enough tokens in contract for the airdrop");

        for (uint256 i = 0; i < recipients.length; i++) {
            bool sent = token.transfer(recipients[i], valueInWei);
            if (!sent) {
                emit FailedTransfer(recipients[i], valueInWei);
            }
        }
        emit TransferredTokens(recipients, valueInWei);
    }  

    function destroy() external onlyOwner {
        uint256 balance = token.balanceOf(address(this));
        require(balance > 0, "No tokens available for destruction");
        token.transfer(owner, balance);
        selfdestruct(payable(owner));
    }
}