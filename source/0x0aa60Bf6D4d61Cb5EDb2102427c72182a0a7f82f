// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

/*
✅ BOOMRANG DeFi PROTOCOL - Official Smart Contract.

Web site - boomrang.io or check by the function - getOfficialWebsite.

Mutual Aid Fund.
*/

contract BoomrangGame {
 
    constructor() {
        owner = msg.sender;
    }
    address public owner;
    address private newOwner;

    struct Player {
        address payable addr; 
        uint8 position;
    }

    struct Table {
        uint8 tableType; 
        bool isActive;
        uint8 playerCount;
        mapping(uint8 => Player) players;
        mapping(address => bool) isPlayerPresent;
    }
  
    uint8 constant MAX_PLAYERS = 15;
    uint8 FEE_PERCENTAGE = 125;
    uint256 constant ENTRY_FEE_1 = 0.1 ether;
    uint256 constant ENTRY_FEE_2 = 1 ether;
    uint256 constant ENTRY_FEE_3 = 5 ether;
    uint256 public customFeeMultiplier = 115;
    uint256 public numberOfTables;
    string public officialWebsite;

    mapping(bytes32 => Table) public tables;
    mapping(address => uint256) public pendingWithdrawals;
    mapping(address => address) public referrers;
    mapping(address => uint256) public referralRewards;
    mapping(address => bytes32[]) public playerTables;
    bool public gameStarted = false;
    bool private paused = false;
    uint256 tableCounter = 0;

    modifier whenNotPaused() {
        require(!paused, "Contract is paused");
        _;
    }

    modifier whenPaused() {
        require(paused, "Contract is not paused");
        _;
    }
    function pause() external onlyOwner whenNotPaused {
        paused = true;
        emit Paused();
    }

    function unpause() external onlyOwner whenPaused {
        paused = false;
        emit Unpaused();
    }

   
   
    event Paused();
    event Unpaused();
    event PlayerJoined(address indexed playerAddress, bytes32 indexed tableId, uint8 indexed position);
    event TableClosed(bytes32 indexed tableId, address winner);
    event NewTableCreated(bytes32 tableId);
    event CustomTableCreated(bytes32 tableId, address creator);
    event OwnershipTransferProposed(address indexed currentOwner, address indexed proposedOwner);    
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);


    function proposeNewOwner(address _newOwner) external onlyOwner {
        require(_newOwner != address(0) && _newOwner != owner, "Invalid new owner address");
        newOwner = _newOwner;
        emit OwnershipTransferProposed(owner, newOwner);
    }
  
    function acceptOwnership() external {
        require(msg.sender == newOwner, "Only the proposed owner can accept ownership");
        emit OwnershipTransferred(owner, newOwner);
        owner = newOwner;
        newOwner = address(0);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }
  
    struct PlayerInfo {
        address playerAddress;
        uint8 position;
    }

    function claimReferralReward() external {
        uint256 reward = referralRewards[msg.sender];
        require(reward > 0, "No referral rewards to claim");

        referralRewards[msg.sender] = 0;
        payable(msg.sender).transfer(reward);
    }

    function claim() external {
        uint256 amount = pendingWithdrawals[msg.sender];
        require(amount > 0, "No funds to withdraw");

        pendingWithdrawals[msg.sender] = 0;
        payable(msg.sender).transfer(amount);
    }

    function startGame() external onlyOwner {
        require(!gameStarted, "The game has already been started.");

        uint8 tableTypeFirst = 1;
        bytes32 firstTableId = getNextTableId(tableTypeFirst);  

        Table storage table = tables[firstTableId];

        Player memory ownerPlayer = Player({
            addr: payable(owner),
            position: 1
        });

        table.players[1] = ownerPlayer;
        table.playerCount = 1;
        table.isPlayerPresent[owner] = true;
        table.tableType = tableTypeFirst;
        gameStarted = true;  
        playerTables[msg.sender].push(firstTableId);
        emit PlayerJoined(owner, firstTableId, 1);  
    }


    function joinGame(bytes32 tableId, uint8 chosenPosition, address _referrer) external payable whenNotPaused {
        require(tables[tableId].isActive, "Table is not active.");
        
        uint8 tableType = tables[tableId].tableType;
        if (tableType == 1) require(msg.value == ENTRY_FEE_1, "Incorrect BNB amount sent");
        if (tableType == 2) require(msg.value == ENTRY_FEE_2, "Incorrect BNB amount sent");
        if (tableType == 3) require(msg.value == ENTRY_FEE_3, "Incorrect BNB amount sent");
        require(tables[tableId].playerCount > 0, "This table doesn't exist.");
        require(chosenPosition >= 2 && chosenPosition <= 15, "Position should be between 2 and 15");

        Table storage table = tables[tableId];

        require(!table.isPlayerPresent[msg.sender], "You are already a player at this table.");
        require(table.players[chosenPosition].addr == address(0), "Position is already taken");

        
        Player memory newPlayer = Player({
            addr: payable(msg.sender),
            position: chosenPosition
        });

        table.players[chosenPosition] = newPlayer;
        table.isPlayerPresent[msg.sender] = true;
        emit PlayerJoined(msg.sender, tableId, chosenPosition);
        table.playerCount++;
        if (_referrer == address(0)) {
            _referrer = owner;
        }
        require(_referrer != msg.sender, "You cannot refer yourself");
        if (referrers[msg.sender] == address(0)) {
            referrers[msg.sender] = _referrer;
        }

        address referrer = referrers[msg.sender];
        uint256 reward = (msg.value * 3) / 100;
        referralRewards[referrer] += reward;

        playerTables[msg.sender].push(tableId);
        if (table.playerCount == MAX_PLAYERS) {
            closeTable(tableId);
        }
    }

    function getTablePlayers(bytes32 tableId) external view returns (PlayerInfo[] memory) {
        uint8 maxPosition = 15; 
        PlayerInfo[] memory playersList = new PlayerInfo[](maxPosition);

        for (uint8 i = 1; i <= maxPosition; i++) {
            if (tables[tableId].players[i].addr != address(0)) {
                playersList[i-1] = PlayerInfo({
                    playerAddress: tables[tableId].players[i].addr,
                    position: tables[tableId].players[i].position
                });
            } else {
                playersList[i-1] = PlayerInfo({
                    playerAddress: address(0),
                    position: 0
                });
            }
        }

        return playersList;
    }

    
    function setReferrer(address _referrer) external {
            require(_referrer != msg.sender, "You cannot refer yourself");
            require(referrers[msg.sender] == address(0), "You already have a referrer");

            referrers[msg.sender] = _referrer;
          
        }
          
    function getCustomEntryFee(uint8 tableType) public view returns (uint256) {
        uint256 baseFee;
        
        if (tableType == 1) {
            baseFee = ENTRY_FEE_1;
        } else if (tableType == 2) {
            baseFee = ENTRY_FEE_2 ;
        } else if (tableType == 3) {
            baseFee = ENTRY_FEE_3;
        } else {
            revert("Invalid table type.");
        }

        return baseFee * customFeeMultiplier / 100;
    }      
        
    function createCustomTable(uint8 tableType) external payable whenNotPaused {
        require(tableType >= 1 && tableType <= 3, "Invalid table type.");
        require(msg.value == getCustomEntryFee(tableType), "Incorrect BNB amount sent for custom table creation.");

        bytes32 newTableId = createTable(tableType);

        Table storage table = tables[newTableId];

        Player memory customPlayer = Player({
            addr: payable(msg.sender),
            position: 1
        });

        table.players[1] = customPlayer;
        table.isPlayerPresent[msg.sender] = true;
        table.playerCount = 1;

        playerTables[msg.sender].push(newTableId);
        emit CustomTableCreated(newTableId, msg.sender);
    }

    

    function getNextTableId(uint8 tableType) internal returns (bytes32) {
    tableCounter++;
    bytes32 newTableId = keccak256(abi.encodePacked(block.timestamp, block.prevrandao, msg.sender, tableCounter, tableType));
    while(tables[newTableId].playerCount != 0) {
        tableCounter++;
        newTableId = keccak256(abi.encodePacked(block.timestamp, block.prevrandao, msg.sender, tableCounter, tableType));
    }
    numberOfTables++;
    emit NewTableCreated(newTableId);
    return newTableId;
}
     
     function createTable(uint8 tableType) internal returns (bytes32) {
            bytes32 newTableId = getNextTableId(tableType);
            
            tables[newTableId].isActive = true;
            tables[newTableId].tableType = tableType;
            
            return newTableId;
     }

    
    function closeTable(bytes32 tableId) internal {
        Table storage table = tables[tableId];
        table.isActive = false;

        uint256 entryFee;
        if (table.tableType == 1) entryFee = ENTRY_FEE_1;
        else if (table.tableType == 2) entryFee = ENTRY_FEE_2;
        else if (table.tableType == 3) entryFee = ENTRY_FEE_3;
        else revert("Invalid table type.");

        address payable winner = table.players[1].addr;
        uint256 prize = entryFee * 8;
        uint256 fee = (prize * FEE_PERCENTAGE) / 1000;
        prize -= fee;

        pendingWithdrawals[winner] += prize;
        emit TableClosed(tableId, winner);


        bytes32 newTableId1 = createTable(table.tableType);
        bytes32 newTableId2 = createTable(table.tableType);
     
        tables[newTableId1].isActive = true;
        tables[newTableId1].tableType = table.tableType; 
        tables[newTableId1].players[1] = Player(table.players[2].addr, 1);
        tables[newTableId1].players[2] = Player(table.players[4].addr, 2);
        tables[newTableId1].players[3] = Player(table.players[5].addr, 3);
        tables[newTableId1].players[4] = Player(table.players[8].addr, 4);
        tables[newTableId1].players[5] = Player(table.players[9].addr, 5);
        tables[newTableId1].players[6] = Player(table.players[10].addr, 6);
        tables[newTableId1].players[7] = Player(table.players[11].addr, 7);
        tables[newTableId1].playerCount += 7;
        for (uint8 i = 1; i <= 7; i++) {
            tables[newTableId1].isPlayerPresent[tables[newTableId1].players[i].addr] = true;
            playerTables[tables[newTableId1].players[i].addr].push(newTableId1);
        }

      
        tables[newTableId2].isActive = true; 
        tables[newTableId2].tableType = table.tableType; 
        tables[newTableId2].players[1] = Player(table.players[3].addr, 1);
        tables[newTableId2].players[2] = Player(table.players[6].addr, 2);
        tables[newTableId2].players[3] = Player(table.players[7].addr, 3);
        tables[newTableId2].players[4] = Player(table.players[12].addr, 4);
        tables[newTableId2].players[5] = Player(table.players[13].addr, 5);
        tables[newTableId2].players[6] = Player(table.players[14].addr, 6);
        tables[newTableId2].players[7] = Player(table.players[15].addr, 7);
        tables[newTableId2].playerCount += 7;
        for (uint8 i = 1; i <= 7; i++) {
            tables[newTableId2].isPlayerPresent[tables[newTableId2].players[i].addr] = true;
            playerTables[tables[newTableId2].players[i].addr].push(newTableId2);
        }

        for (uint8 i = 1; i <= MAX_PLAYERS; i++) {
            removeTableIdFromPlayer(tableId, table.players[i].addr); 
            delete table.isPlayerPresent[table.players[i].addr];
            delete table.players[i];
        }
    }


    function getPlayerTables(address player) public view returns (bytes32[] memory) {
        return playerTables[player];
    }

    function removeTableIdFromPlayer(bytes32 tableId, address player) internal {
        bytes32[] storage tableList = playerTables[player];
        for (uint256 i = 0; i < tableList.length; i++) {
            if (tableList[i] == tableId) {
                tableList[i] = tableList[tableList.length - 1];
                tableList.pop();
                break;
            }
        }
    } 

    function changeFeePercentage(uint8 newPercentage) external onlyOwner {
        require(newPercentage >= 0 && newPercentage <= 100, "Percentage should be between 0 and 100");
        FEE_PERCENTAGE = newPercentage;
    }
  
    function feeOut(uint256 amount) external onlyOwner {
        require(address(this).balance >= amount, "Try later");
        uint256 previousBalance = address(this).balance;
        payable(owner).transfer(amount);
        assert(address(this).balance == previousBalance - amount);
    }

    function getBalance(address player) public view returns (uint256) {
        return pendingWithdrawals[player];
    }

    function setCustomFeeMultiplier(uint256 newMultiplier) external onlyOwner {
        require(newMultiplier > 0, "Multiplier should be greater than 0");
        customFeeMultiplier = newMultiplier;
    }
    function setOfficialWebsite(string calldata newWebsite) external onlyOwner {
        officialWebsite = newWebsite;
    }

    function getOfficialWebsite() external view returns (string memory) {
        return officialWebsite;
    }
}