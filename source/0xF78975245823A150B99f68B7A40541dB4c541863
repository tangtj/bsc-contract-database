// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

contract O7SpSSLpool {
    address public owner;
    mapping(address => mapping(address => uint256)) public depositedTokens;
    mapping(address => uint256) public depositedBNB;

    event TokensDeposited(address indexed user, address indexed token, uint256 amount);
    event BNBDeposited(address indexed user, uint256 amount);
    event TokensTransferred(address indexed to, address indexed token, uint256 amount);

    constructor() {
        owner = msg.sender;
    }

    // Function to receive BNB directly into the contract
    receive() external payable {
        depositBNB(msg.sender, msg.value);
    }

    // Function to deposit BEP-20 tokens into the contract
    function depositTokens(address tokenAddress, uint256 amount) external {
        require(amount > 0, "Amount must be greater than 0");

        // Ensure the token contract is valid (simplified BEP-20 interface)
        (bool success, ) = tokenAddress.call{value: 0}(
            abi.encodeWithSelector(bytes4(keccak256("transferFrom(address,address,uint256)")), msg.sender, address(this), amount)
        );

        require(success, "Token transfer failed");

        // Update the depositedTokens mapping
        depositedTokens[msg.sender][tokenAddress] += amount;

        emit TokensDeposited(msg.sender, tokenAddress, amount);
    }

    // Function to deposit BNB directly into the contract
    function depositBNB(address sender, uint256 amount) internal {
        require(amount > 0, "Amount must be greater than 0");
        depositedBNB[sender] += amount;

        emit BNBDeposited(sender, amount);
    }

    // Function to transfer BEP-20 tokens to another address (only owner)
    function transferTokens(address tokenAddress, address to, uint256 amount) external {
        require(msg.sender == owner, "Only the owner can transfer tokens");
        require(depositedTokens[to][tokenAddress] >= amount, "Insufficient deposited tokens");

        // Ensure the token contract is valid (simplified BEP-20 interface)
        (bool success, ) = tokenAddress.call{value: 0}(
            abi.encodeWithSelector(bytes4(keccak256("transfer(address,uint256)")), to, amount)
        );

        require(success, "Token transfer failed");

        // Update the depositedTokens mapping
        depositedTokens[to][tokenAddress] -= amount;

        emit TokensTransferred(to, tokenAddress, amount);
    }

    // Function to transfer BNB to another address (only owner)
    function transferBNB(address payable to, uint256 amount) external {
        require(msg.sender == owner, "Only the owner can transfer BNB");
        require(depositedBNB[to] >= amount, "Insufficient deposited BNB");

        (bool success, ) = to.call{value: amount}("");
        require(success, "BNB transfer failed");

        depositedBNB[to] -= amount;
    }
}