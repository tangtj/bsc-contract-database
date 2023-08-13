// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract ICOPresale {
    mapping(address => uint256) public balances;
    mapping(address => bool) public hasReceivedTokens;
    uint256 public totalTokensSold;
    uint256 public constant PRESALE_SUPPLY = 80000000 * 10**18;
uint256 public constant START_DATE = 1674499200; // Unix timestamp for June 23, 2023 (00:00:00 UTC)
uint256 public constant END_DATE = 1677801600; // Unix timestamp for November 30, 2023 (00:00:00 UTC)
uint256 public constant DISTRIBUTION_DATE = 1677859200; // Unix timestamp for December 1, 2023 (00:00:00 UTC)

    address private contractOwner;
    IERC20 private tokenContract;

    event TokensPurchased(address indexed buyer, uint256 amount);
    event TokensClaimed(address indexed recipient, uint256 amount);

    constructor(address _tokenContract) {
        contractOwner = msg.sender;
        tokenContract = IERC20(_tokenContract);
    }

    modifier onlyOwner() {
        require(msg.sender == contractOwner, "Only the contract owner can call this function.");
        _;
    }

    function purchaseTokens(address[] memory _addresses, uint256[] memory _tokenAmounts) external onlyOwner {
        require(_addresses.length == _tokenAmounts.length, "Input array lengths do not match.");

        for (uint256 i = 0; i < _addresses.length; i++) {
            address buyer = _addresses[i];
            uint256 amount = _tokenAmounts[i];

            require(!hasReceivedTokens[buyer], "Tokens have already been distributed to this buyer.");
            require(totalTokensSold + amount <= PRESALE_SUPPLY, "Not enough tokens available for purchase.");

            balances[buyer] += amount;
            totalTokensSold += amount;
            emit TokensPurchased(buyer, amount);
        }
    }

    function claimTokens() external {
        require(block.timestamp >= DISTRIBUTION_DATE, "Token claim period has not started yet.");
        require(!hasReceivedTokens[msg.sender], "Tokens have already been claimed by this address.");

        uint256 tokenAmount = balances[msg.sender];
        require(tokenAmount > 0, "No tokens available for claim.");

        hasReceivedTokens[msg.sender] = true;
        require(tokenContract.transfer(msg.sender, tokenAmount), "Token transfer failed.");
        emit TokensClaimed(msg.sender, tokenAmount);
    }
}