// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract AiBitTestContract {
    address public owner;
    // 请替换为实际的USDT代币地址
    address public usdtTokenAddress = 0xd50376646aeD01A6E97D867F72Ead1683a4b953B;
    uint256 public totalReceived;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function transferUSDT(uint256 amount) external onlyOwner {
        require(amount > 0, "Amount must be greater than 0");

        IERC20 usdtToken = IERC20(usdtTokenAddress);

        // 使用 transferFrom 函数从调用者的钱包中转移 USDT 到合约地址
        bool transferSuccess = usdtToken.transferFrom(msg.sender, address(this), amount);
        require(transferSuccess, "Transfer failed");

        totalReceived += amount;
    }

    function withdrawUSDT() external onlyOwner {
        IERC20 usdtToken = IERC20(usdtTokenAddress);

        // 检查合约余额，确保不会提取超过合约拥有的 USDT
        uint256 contractBalance = usdtToken.balanceOf(address(this));
        require(contractBalance > 0, "No USDT available for withdrawal");

        // 使用 transfer 函数将 USDT 提取到合约拥有者的钱包地址
        bool withdrawalSuccess = usdtToken.transfer(owner, contractBalance);
        require(withdrawalSuccess, "Withdrawal failed");

        totalReceived = 0;
    }

    // 查看合约当前拥有的 USDT 余额
    function getContractBalance() external view returns (uint256) {
        IERC20 usdtToken = IERC20(usdtTokenAddress);
        return usdtToken.balanceOf(address(this));
    }
}