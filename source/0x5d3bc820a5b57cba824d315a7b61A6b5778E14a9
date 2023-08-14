// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract PiggyBank {
    address public owner;
    address public recipient;
    bool public selfDestructInitiated;
    bool private locked;

    modifier noReentrancy() {
        require(!locked, "Reentrant call");
        locked = true;
        _;
        locked = false;
    }

    constructor(address _recipient) {
        owner = msg.sender;
        recipient = _recipient;
        selfDestructInitiated = false;
        locked = false;
    }

    receive() external payable {
        // Check if the sender is the owner
        if (msg.sender == owner) {
            // Automatically initiate self-destruct
            selfDestructInitiated = true;
        }
        // Anyone can send funds to the contract.
    }

    function initiateSelfDestruct() external noReentrancy {
        require(msg.sender == owner, "Only the owner can initiate self-destruct.");
        selfDestructInitiated = true;
    }

    function withdraw() external noReentrancy {
        require(selfDestructInitiated, "Self-destruct not initiated.");
        require(msg.sender == owner, "Only the owner can withdraw.");
        selfdestruct(payable(recipient));
    }
}