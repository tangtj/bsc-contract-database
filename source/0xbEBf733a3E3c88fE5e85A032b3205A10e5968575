// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

contract SignIn {
    // Event to log the signIn action
    event UserSignedIn(address indexed user, uint256 timestamp);

    /**
     * @dev Allows a user to sign in. This function emits an event with the current timestamp.
     */
    function signIn() public {
        emit UserSignedIn(msg.sender, block.timestamp);
        return;
    }
}