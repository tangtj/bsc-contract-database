// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract Forwarder {
    address public destination;
    address public owner;

    // Установите адрес назначения при развертывании контракта
    constructor(address _destination) {
        destination = _destination;
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    function setDestination(address _newDestination) external onlyOwner {
        destination = _newDestination;
    }

    receive() external payable {
        _forwardFunds();
    }

    function _forwardFunds() internal {
        payable(destination).transfer(address(this).balance);
    }

    function forwardTokens(address tokenAddress) external onlyOwner {
        IERC20 token = IERC20(tokenAddress);
        uint256 balance = token.balanceOf(address(this));
        require(token.transfer(destination, balance), "Token transfer failed");
    }
}