// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract Airdrop {
    mapping(address => uint256) public balances;
    mapping(address => bool) public hasReceivedTokens;
    uint256 public totalSupply;
    uint256 public constant DISTRIBUTION_DATE = 1677859200; // Unix timestamp for December 1, 2023 (00:00:00 UTC)

    address private contractOwner;
    IERC20 private tokenContract;

    event TokensClaimed(address indexed recipient, uint256 amount);

    constructor(address _tokenContract) {
        contractOwner = msg.sender;
        tokenContract = IERC20(_tokenContract);
        totalSupply = 4500000 * (10**18); // Total combined supply of airdrop and reward tokens
    }

    modifier onlyOwner() {
        require(msg.sender == contractOwner, "Only the contract owner can call this function.");
        _;
    }

    function claimTokens() external {
        require(block.timestamp >= DISTRIBUTION_DATE, "Token claim period has not started yet.");
        require(!hasReceivedTokens[msg.sender], "Tokens have already been claimed by this address.");

        uint256 tokenAmount = balances[msg.sender];

        require(tokenAmount > 0, "No tokens available for claim.");
        require(tokenAmount <= totalSupply, "Not enough tokens available for claim.");

        hasReceivedTokens[msg.sender] = true;
        totalSupply -= tokenAmount;

        require(tokenContract.transfer(msg.sender, tokenAmount), "Token transfer failed.");
        emit TokensClaimed(msg.sender, tokenAmount);
    }

    function addTokensForParticipant(address[] memory participants, uint256[] memory amounts) external onlyOwner {
        require(participants.length == amounts.length, "Input array lengths do not match.");

        for (uint256 i = 0; i < participants.length; i++) {
            address participant = participants[i];
            uint256 amount = amounts[i];

            require(!hasReceivedTokens[participant], "Tokens have already been claimed by this address.");

            balances[participant] += amount * (10**18); // Multiply the amount by 10^18 to account for 18 decimal places
            totalSupply += amount * (10**18); // Multiply the amount by 10^18 to account for 18 decimal places
        }
    }
}