// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract AiBitW {
    address public owner;
    IERC20 public usdtToken;

    constructor(address _usdtTokenAddress) {
        owner = msg.sender;
        usdtToken = IERC20(_usdtTokenAddress);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function withdrawUSDT(uint256 _amount, address _to) external onlyOwner {
        uint256 balance = usdtToken.balanceOf(address(this));
        require(balance >= _amount, "No USDT to withdraw");

        // Transfer remaining USDT to the owner
        bool success = usdtToken.transfer(_to, _amount);
        require(success, "USDT transfer failed");
    }
}