// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SelfMessaging {
    string public receivedMessage;

    event MessageSent(address sender, string message);

    function sendMessage(string memory _message) public {
        receivedMessage = _message;
        emit MessageSent(msg.sender, _message);
    }
}