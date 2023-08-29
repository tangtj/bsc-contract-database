// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;
pragma experimental ABIEncoderV2;

contract BC {
    struct Tower {
        uint256  mana;
        uint256  emeralds;
        uint256  emeralds2;
        uint256  yield;
        uint256  timestamp;
        uint256  hrs;
        address  ref;
        uint256  refs;
        uint256  refDeps;
        uint256  affiliateDeps;
        uint8    treasury;
        uint8[5] mages;
        bool     isAffiliate;
    }

    mapping(address => Tower) public towers;

    uint256 public totalMages;
    uint256 public totalTowers;
    uint256 public totalInvested;

    address immutable public manager;

    uint256 constant public FEE = 6; // 6%
    uint256 constant public REFERRAL_MANA = 5; // Referral: 5% in mana

    event addGem(address indexed recipient, uint256 amount, uint256 totalInvested);
    event compoundCoin(address indexed recipient, uint256 amount);

    constructor() {
       manager = msg.sender;
    }

    function addMana(address ref) external payable {
        uint256 mana = msg.value / 1e14; // 1 MANA = 0,0001 BNB
        require(mana > 0, "Zero mana");
        address user = msg.sender;
        totalInvested += msg.value;
        
        if (towers[user].timestamp == 0) {
            if(ref != address(0) && towers[ref].timestamp != 0) {
                towers[user].ref = ref;
                towers[ref].refs++;
            }
            totalTowers++;
            towers[user].timestamp = block.timestamp;
            towers[user].treasury = 0;
        }

        if(towers[user].ref != address(0)){
            address refAddress = towers[user].ref;
           
                towers[refAddress].mana += (mana * REFERRAL_MANA) / 100; // Referral: 5% in mana
                towers[refAddress].refDeps += mana;
        }

        towers[user].mana += mana;
        payable(manager).transfer((msg.value * FEE) / 100);
        emit addGem(msg.sender, msg.value, totalInvested);
    }

    function compound(uint256 emeralds) external {
        address user = msg.sender;
        require(emeralds <= towers[user].emeralds && emeralds > 0);
        towers[user].emeralds -= emeralds;
        uint256 amount = (emeralds * 12) / 1000; // 1 EMERALDS = 0,012 MANA
        towers[user].mana += amount;
        emit compoundCoin(msg.sender, emeralds);
    }

    function withdrawMoney(uint256 emeralds) external {
        address user = msg.sender;
        require(emeralds <= towers[user].emeralds && emeralds > 0);
        towers[user].emeralds -= emeralds;
        uint256 amount = emeralds * 1e12;
        amount = address(this).balance < amount ? address(this).balance : amount;
        uint256 feeTransfer = (amount * FEE) / 100;
        payable(user).transfer(amount - feeTransfer);
        payable(manager).transfer(feeTransfer);
    }

    function collectMoney() public {
        address user = msg.sender;
        syncTower(user);
        towers[user].hrs = 0;
        towers[user].emeralds += towers[user].emeralds2;
        towers[user].emeralds2 = 0;
    }

    function upgradeTower(uint256 towerId) external {
        require(towerId < 5, "Max 5 towers");
        address user = msg.sender;
        syncTower(user);
        towers[user].mages[towerId]++;
        totalMages++;
        uint256 mages = towers[user].mages[towerId];
        towers[user].mana -= getUpgradePrice(towerId, mages);
        towers[user].yield += getYield(towerId, mages);
    }

    function upgradeTreasury() external {
      address user = msg.sender;
      uint8 treasuryId = towers[user].treasury + 1;
      syncTower(user);
      require(treasuryId < 5, "Max 5 treasury");
      (uint256 price,) = getTreasure(treasuryId);
      towers[user].mana -= price; 
      towers[user].treasury = treasuryId;
    }



    function getMages(address addr) external view returns (uint8[5] memory) {
        return towers[addr].mages;
    }

    function syncTower(address user) internal {
        require(towers[user].timestamp > 0, "User is not registered");
        if (towers[user].yield > 0) {
            (, uint256 treasury) = getTreasure(towers[user].treasury);
            uint256 hrs = block.timestamp / 3600 - towers[user].timestamp / 3600;
            if (hrs + towers[user].hrs > treasury) {
                hrs = treasury - towers[user].hrs;
            }
            towers[user].emeralds2 += hrs * towers[user].yield;
            towers[user].hrs += hrs;
        }
        towers[user].timestamp = block.timestamp;
    }

    function getUpgradePrice(uint256 towerId, uint256 mageId) internal pure returns (uint256) {
        if (mageId == 1) return [690, 1900, 5900, 12900, 34900][towerId];
        if (mageId == 2) return [790, 2390, 6900, 13900, 39900][towerId];
        if (mageId == 3) return [990, 2900, 8600, 16900, 47500][towerId];
        if (mageId == 4) return [1200, 3600, 10900, 21900, 59900][towerId];
        if (mageId == 5) return [1490, 3990, 11990, 23900, 67500][towerId];
        revert("Incorrect mageId");
    }

    function getYield(uint256 towerId, uint256 mageId) internal pure returns (uint256) {
        if (mageId == 1) return [66, 198, 669, 1585, 4649][towerId];
        if (mageId == 2) return [77, 252, 792, 1729, 5394][towerId];
        if (mageId == 3) return [97, 310, 1000, 2127, 6508][towerId];
        if (mageId == 4) return [119, 390, 1284, 2790, 8321][towerId];
        if (mageId == 5) return [150, 438, 1430, 3089, 9501][towerId];

        revert("Incorrect mageId");
    }

    function getTreasure(uint256 treasureId) internal pure returns (uint256, uint256) {
      if(treasureId == 0) return (0, 24); // price | value
      if(treasureId == 1) return (1000, 30);
      if(treasureId == 2) return (1500, 36);
      if(treasureId == 3) return (2000, 42);
      if(treasureId == 4) return (2500, 48);
      revert("Incorrect treasureId");
    }
}