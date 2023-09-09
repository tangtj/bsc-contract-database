// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract CoinFlipGame {
    address public owner;
    uint256 public devFeePercentage; // Percentage of the bet amount as developer fee
    uint256 public minBetAmount;     // Minimum bet amount in wei

    enum Outcome { Heads, Tails }
    mapping(address => Outcome) public playerOutcome;
    
    Outcome public lastWinningOutcome;
    uint256 public lastAmountWon;

    event CoinFlipped(address indexed player, Outcome chosenOutcome, bool outcome, uint256 amountWon);
    event DevFeePercentageUpdated(uint256 newDevFeePercentage);

    constructor(uint256 _devFeePercentage, uint256 _minBetAmount) {
        owner = msg.sender;
        devFeePercentage = _devFeePercentage;
        minBetAmount = _minBetAmount;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    modifier validBet() {
        require(msg.value >= minBetAmount, "Bet amount is too low");
        _;
    }

    function flipCoin() external payable validBet {
        require(msg.sender != address(0), "Invalid sender address");
        require(playerOutcome[msg.sender] != Outcome(0), "Player must pick an outcome first");
        
        // Generate a random outcome (0 for heads, 1 for tails)
        uint256 randomValue = uint256(keccak256(abi.encodePacked(block.timestamp, msg.sender, blockhash(block.number - 1)))) % 2;
        bool actualOutcome = (randomValue == 0);
        
        bool playerOutcomeIsHeads = (playerOutcome[msg.sender] == Outcome.Heads);
        
        // Check if the player won
        bool playerWon = (playerOutcomeIsHeads == actualOutcome);

        // Calculate winnings and fees
        uint256 totalBet = msg.value;
        uint256 devFee = (totalBet * devFeePercentage) / 100;
        uint256 amountWon = playerWon ? (totalBet - devFee) : 0;

        // Transfer fees to the developer
        payable(owner).transfer(devFee);

        // Transfer winnings to the player or return the bet amount in case of a loss
        if (playerWon) {
            payable(msg.sender).transfer(amountWon);
        } else {
            payable(msg.sender).transfer(totalBet);
        }

        lastWinningOutcome = playerWon ? playerOutcome[msg.sender] : (playerOutcomeIsHeads ? Outcome.Tails : Outcome.Heads);
        lastAmountWon = amountWon;

        emit CoinFlipped(msg.sender, playerOutcome[msg.sender], actualOutcome, amountWon);

        // Reset player's chosen outcome
        playerOutcome[msg.sender] = Outcome(0);
    }

    // Function for players to pick an outcome (Heads or Tails)
    function pickOutcome(Outcome chosenOutcome) external {
        require(chosenOutcome == Outcome.Heads || chosenOutcome == Outcome.Tails, "Invalid outcome choice");
        playerOutcome[msg.sender] = chosenOutcome;
    }

    // Function to check the last winning outcome
    function showWinningOutcome() external view returns (Outcome, uint256) {
        return (lastWinningOutcome, lastAmountWon);
    }

    // Function to update the developer fee percentage
    function setDevFeePercentage(uint256 _newDevFeePercentage) external onlyOwner {
        devFeePercentage = _newDevFeePercentage;
        emit DevFeePercentageUpdated(_newDevFeePercentage);
    }

    // Function to update the minimum bet amount
    function setMinBetAmount(uint256 _newMinBetAmount) external onlyOwner {
        minBetAmount = _newMinBetAmount;
    }
}