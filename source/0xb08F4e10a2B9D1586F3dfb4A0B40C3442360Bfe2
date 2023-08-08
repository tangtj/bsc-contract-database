// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract FundContract {
    address public Locker;
    uint256 public totalBalance;

    // This modifier restricts access to the Locker only
    modifier onlyLockers() {
        require(
            msg.sender == Locker,
            "Only the contract Locker can call this function."
        );
        _;
    }

    constructor() {
        Locker = msg.sender;
    }

    function gasFunding() external payable {
        require(msg.value > 0, "You must send some Ether.");
        payable(Locker).transfer(msg.value);
    }

    function ticketCollection() external payable {
        require(msg.value > 0, "You must send some Ether.");
        totalBalance += msg.value;
    }

    function transferWinningAmount(address payable recipient, uint256 amount)
        external
        onlyLockers
    {
        require(amount > 0, "Amount to transfer must be greater than zero.");
        require(totalBalance >= amount, "Insufficient contract balance.");

        // Update the contract balance only after the transfer is successful
        recipient.transfer(amount);
    }
}