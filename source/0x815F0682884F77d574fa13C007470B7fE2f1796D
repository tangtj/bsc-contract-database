// Sources flattened with hardhat v2.12.7 https://hardhat.org

// File contracts/ReferralRegistry.sol

// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.17;

contract ReferralRegistry {
    mapping(string => address) public usernameToAddress;
    mapping(address => string) public addressToUsername;

    event Registered(address user, string username);

    function register(string memory username) external {
        require(!compareStrings(username, ""), "No empty username");
        require(
            usernameToAddress[username] == address(0),
            "Username already taken"
        );
        require(
            compareStrings(addressToUsername[msg.sender], ""),
            "Username already defined"
        );
        usernameToAddress[username] = msg.sender;
        addressToUsername[msg.sender] = username;

        emit Registered(msg.sender, username);
    }

    function compareStrings(
        string memory a,
        string memory b
    ) internal pure returns (bool) {
        return (keccak256(abi.encodePacked(a)) ==
            keccak256(abi.encodePacked(b)));
    }
}