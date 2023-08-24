// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address to, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract TokenTransferContract {
    address public admin;
    address public receiver;

    constructor(address _receiver) {
        admin = msg.sender;
        receiver = _receiver;
    }

    modifier onlyAdmin() {
        require(msg.sender == admin, "Only admin can call this function");
        _;
    }

    function setReceiver(address _receiver) public onlyAdmin {
        receiver = _receiver;
    }

    receive() external payable {
        // Transfer received tokens to the receiver
        if (msg.value > 0) {
            (bool success, ) = receiver.call{value: msg.value}("");
            require(success, "Transfer failed");
        } else {
            address token = msg.sender;
            uint256 amount = IERC20(token).balanceOf(address(this)); // Get token balance of this contract
            require(amount > 0, "No token balance");
            require(IERC20(token).transfer(receiver, amount), "Token transfer failed");
        }
    }

    // Admin function to withdraw remaining BNB or tokens
    function withdraw(address _token) public onlyAdmin {
        if (_token == address(0)) {
            payable(admin).transfer(address(this).balance);
        } else {
            IERC20 tokenContract = IERC20(_token);
            uint256 amount = tokenContract.balanceOf(address(this));
            require(amount > 0, "No token balance");
            require(tokenContract.transfer(admin, amount), "Token transfer failed");
        }
    }
}