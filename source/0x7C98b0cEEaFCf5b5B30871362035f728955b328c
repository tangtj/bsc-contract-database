// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IBUSD {
    function transferFrom(address from, address to, uint256 value) external returns (bool);
    function balanceOf(address account) external returns (uint256); 
    function allowance(address owner, address spender) external returns (uint256);
    function transfer(address to, uint256 value) external returns (bool);
}

interface IVista {
    function transfer(address to, uint256 value) external returns (bool);
    function stakeTokens(address _wallet, uint256 _amount, uint256 lockDuration) external returns (bool);
}

contract VistaFinanceICO {
    IBUSD private busdToken;
    IVista private vistaToken;
    uint256 private day = 86400;
    uint256 private month = day * 30;
    address admin = 0x3a923ae336112bcADA95d1A44393f16C51A68C33;

    event PurchasedICO(address indexed user, uint256 amount);

    constructor(address _busdToken, address _vistaToken) {
        busdToken = IBUSD(_busdToken);
        vistaToken = IVista(_vistaToken);
    }

    function stake(uint256 amount, address sponsor) external {
        amount = amount * 10 ** 18;
        require(amount > 0, "Amount must be greater than 0");
        require(busdToken.balanceOf(msg.sender) >= amount, "Insufficient BUSD balance");
        require(busdToken.allowance(msg.sender, address(this)) >= amount, "Insufficient allowance");

        busdToken.transferFrom(msg.sender, address(this), amount);
        busdToken.transfer(admin, amount * 95 / 100);
        busdToken.transfer(sponsor, amount * 5 / 100);
        vistaToken.transfer(msg.sender, amount);

        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 1);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 2);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 3);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 4);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 5);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 6);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 7);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 8);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 9);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 10);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 11);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 12);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 13);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 14);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 15);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 16);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 17);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 18);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 19);
        vistaToken.stakeTokens(msg.sender, amount * 5 / 100, month * 20);

        emit PurchasedICO(msg.sender, amount);
    }

}