// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract HTJLottery {
    address public owner;
    // address[] public players;
    address public winner;
    uint256 public startBlock;
    uint256 public gameNumber;
    address public lotteryCroupier;

    address public houseFeeReciever = address(0x34BE7B28fB0Bde821918D6A22F508D8108559Fa4);
    uint public houseFeePercent = 18;
    uint public croupierFeePercent = 2;

    struct LotterySettings {
        uint256 entryAmount;
        uint queueSize;
        bool isActive;
    }

    struct LotteryInstance {
        address[] players;
        uint256 poolAmount;
        address gameWinner;
        bool isQueued;
    }

    event LotteryEntry(
        address player,
        uint256 entryAmount,
        uint256 lotteryId
    );

    event QueueFull(
        uint256 queuedLotteryId,
        bool isFull
    );

    event LotteryStarted(
        address[] players,
        address winner,
        uint256 prize,
        uint outcome,
        uint256 gameNumber,
        uint256 random
    );

    mapping(uint256 => LotterySettings) public lotterySettings;
    mapping(uint256 => LotteryInstance) public lotteryInstances;
    uint256 public currentLotteryId;

    constructor() {
        owner = msg.sender;
        currentLotteryId = 1; // Start with lottery ID 1
        lotteryCroupier = 0x9b9a0eDEe77cAB1f738d5b0c6102D5cc9208DACb;
    }

    function createLottery(uint256 entryFee, uint256 maxPlayers) external onlyOwner {
        // Ensure that the new lottery settings are valid
        require(entryFee > 0 && maxPlayers > 0, "Invalid lottery settings");

        // Add the new lottery settings
        lotterySettings[currentLotteryId] = LotterySettings(entryFee, maxPlayers, true);

        // Increment the currentLotteryId for the next lottery
        currentLotteryId++;
    }

    function enterLottery(uint8 lotteryId) public payable {
        // Find the settings for the specified lottery
        LotterySettings storage currentLotterySettings = lotterySettings[lotteryId];
        LotteryInstance storage currentLotteryInstance = lotteryInstances[lotteryId];
        require(currentLotterySettings.entryAmount > 0, "Lottery not found");
        require(currentLotterySettings.isActive == true, "Lottery is not live");
        require(msg.value == currentLotterySettings.entryAmount, "You must send the correct amount to enter");
        require(currentLotteryInstance.players.length < currentLotterySettings.queueSize, "Queue is full, wait for the next round.");

        currentLotteryInstance.players.push(msg.sender);
        currentLotteryInstance.poolAmount += msg.value;

        if (currentLotteryInstance.players.length == currentLotterySettings.queueSize) {
            // selectWinner(lotteryId);
            emit QueueFull(lotteryId, true);
            currentLotteryInstance.isQueued = true;
        }

        emit LotteryEntry(msg.sender, msg.value, lotteryId);
    }
       

    function selectWinner(uint8 lotteryId) external onlyCroupier {
        LotteryInstance storage currentLotteryInstance = lotteryInstances[lotteryId];

        require(currentLotteryInstance.isQueued == true, "The queue is not full.");
               
        uint256 randNumber = rand();
        uint index = randNumber % currentLotteryInstance.players.length;
        if(index == currentLotteryInstance.players.length) {
            index = index - 1;
        }
        currentLotteryInstance.gameWinner = currentLotteryInstance.players[index];

        gameNumber++;
        
        uint houseFee = (currentLotteryInstance.poolAmount * houseFeePercent) / 100;
        uint croupierFee = (currentLotteryInstance.poolAmount * croupierFeePercent) / 100;
        uint winnings = currentLotteryInstance.poolAmount - houseFee - croupierFee;

        payable(currentLotteryInstance.gameWinner).transfer(winnings);
        payable(houseFeeReciever).transfer(houseFee);
        payable(lotteryCroupier).transfer(croupierFee);

        emit LotteryStarted(currentLotteryInstance.players, currentLotteryInstance.gameWinner, winnings, index, gameNumber, randNumber);

        // Reset the contract for the next round
        reset(lotteryId);
    }

    function reset(uint8 lotteryId) private {
        LotteryInstance storage currentLotteryInstance = lotteryInstances[lotteryId];
        delete currentLotteryInstance.players;
        currentLotteryInstance.poolAmount = 0;
        currentLotteryInstance.isQueued = false;
        winner = address(0);
    }

    function rand() private view returns (uint256) {

        uint256 seed = uint256(keccak256(abi.encodePacked(
            block.timestamp + block.difficulty +
            ((uint256(keccak256(abi.encodePacked(block.coinbase)))) / (block.timestamp)) +
            block.gaslimit + 
            ((uint256(keccak256(abi.encodePacked(msg.sender)))) / (block.timestamp)) +
            block.number
        )));

        return seed;
    }

    function getPlayers(uint8 lotteryId) public view returns (address[] memory) {
        LotteryInstance storage currentLotteryInstance = lotteryInstances[lotteryId];
        return currentLotteryInstance.players;
    }

    function getContractBalance() public view returns (uint) {
        return address(this).balance;
    }

    // Allow the owner to withdraw any excess funds (if any).
    function withdrawExcess() public onlyOwner {
        payable(owner).transfer(address(this).balance);
    }

    function setHouseFeePercent(uint8 newPercent) external onlyOwner {
        require(newPercent != houseFeePercent, "This is already the house fee percent");
        require(newPercent <= 40, "Cannot set house fee percentage higher than 40 percent");

        houseFeePercent = newPercent;
    }

    function setCroupierFeePercent(uint8 newPercent) external onlyOwner {
        require(newPercent != croupierFeePercent, "This is already the croupier fee percent");
        require(newPercent <= 5, "Cannot set croupier fee percentage higher than 40 percent");

        croupierFeePercent = newPercent;
    }

    function setQueueSize(uint8 newSize, uint8 lotteryId) external onlyOwner {
        LotterySettings storage currentLotterySettings = lotterySettings[lotteryId];
        require(newSize != currentLotterySettings.queueSize, "This is already the queue size!");
        require(newSize > 0, "Cannot set queue size to 0!");

        currentLotterySettings.queueSize = newSize;
    }

    function setEntryAmount(uint256 newAmount, uint8 lotteryId) external onlyOwner {
        LotterySettings storage currentLotterySettings = lotterySettings[lotteryId];
        require(newAmount != currentLotterySettings.entryAmount, "This is already the entry amount!");
        require(newAmount > 0, "Cannot set entry amount to 0!");

        currentLotterySettings.entryAmount = newAmount;
    }

    function setCroupier(address newCroupier) external onlyOwner {
        require(newCroupier != lotteryCroupier, "This is already the entry amount!");
        lotteryCroupier = newCroupier;
    }

    function setHouseFeeReceiver(address newReceiver) external onlyOwner {
        require(newReceiver != houseFeeReciever, "This is already the house wallet!");
        houseFeeReciever = newReceiver;
    }

    function getLotteryDetails(uint8 lotteryId) public view returns (
        uint256 entryAmount,
        uint256 queueSize,
        address[] memory players,
        uint256 poolAmount,
        address gameWinner
        ) {
        // Access the lottery settings for the specified lotteryId
        LotterySettings storage currentLotterySettings = lotterySettings[lotteryId];
        // Access the current lottery instance
        LotteryInstance storage currentLotteryInstance = lotteryInstances[lotteryId];

        // Retrieve the relevant details from the settings
        entryAmount = currentLotterySettings.entryAmount;
        queueSize = currentLotterySettings.queueSize;
        players = currentLotteryInstance.players;
        poolAmount = currentLotteryInstance.poolAmount;
        gameWinner = currentLotteryInstance.gameWinner;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Invalid new owner address");
        owner = newOwner;
    }

    receive() external payable {
        for (uint8 lotteryId = 0; lotteryId < currentLotteryId; lotteryId++) {
            LotterySettings storage currentLotterySettings = lotterySettings[lotteryId];

        // Check if the entry amount matches
            if (msg.value == currentLotterySettings.entryAmount) {
                enterLottery(lotteryId); // Enter the found lottery
                break; // Stop after entering the first matching lottery
            }
        }
    }

    function receiveBNB() external payable {
        require(msg.value > 0, "Amount must be greater than 0");
    }
    

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    modifier onlyCroupier() {
        require(msg.sender == lotteryCroupier, "Only the croupier can call this function");
        _;
    }
}