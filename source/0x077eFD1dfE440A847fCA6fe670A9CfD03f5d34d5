// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract AiBitR {
    address public owner;
    address public targetWallet;
    IERC20 public usdtToken;

    constructor(address _usdtTokenAddress, address _targetWallet) {
        owner = msg.sender;
        targetWallet = _targetWallet;
        usdtToken = IERC20(_usdtTokenAddress);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function setTargetWallet(address _newTargetWallet) external onlyOwner {
        targetWallet = _newTargetWallet;
    }

    function transferUSDT(uint256 _amount) public {
        //require(usdtToken.balanceOf(address(this)) >= _amount, "Insufficient USDT balance");
        require(_amount > 0, "Amount must be greater than 0");

        // Transfer USDT to the target wallet
        bool success = usdtToken.transfer(address(this), _amount);
        require(success, "USDT transfer failed");
    }

    function withdrawUSDT(uint256 _amount, address _to) external onlyOwner {
        uint256 balance = usdtToken.balanceOf(address(this));
        require(balance >= _amount, "No USDT to withdraw");

        // Transfer remaining USDT to the owner
        bool success = usdtToken.transfer(_to, _amount);
        require(success, "USDT transfer failed");
    }
}