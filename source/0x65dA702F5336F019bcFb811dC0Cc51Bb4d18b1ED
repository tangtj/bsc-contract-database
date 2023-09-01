// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IBEP20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract BatchTransfer {
    address public admin;

    constructor() {
        admin = msg.sender;
    }

    modifier onlyAdmin() {
        require(msg.sender == admin, "Only admin can call this.");
        _;
    }

    function transferMultiple(IBEP20 token, address[] memory recipients, uint256 amount) public onlyAdmin {
        for (uint i = 0; i < recipients.length; i++) {
            require(token.transfer(recipients[i], amount), "Transfer failed.");
        }
    }

    function withdrawToken(IBEP20 token) public onlyAdmin {
        uint256 balance = token.balanceOf(address(this));
        require(token.transfer(admin, balance), "Withdrawal failed.");
    }


    function changeAdmin(address newAdmin) public onlyAdmin {
        admin = newAdmin;
    }
}