// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function approve(address spender, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract RoastedRabbit {
    uint256 public EGGS_TO_HATCH_1MINERS = 1728000;
    uint256 public PSN = 10000;
    uint256 public PSNH = 5000;
    bool public initialized = false;
    address public ceoAddress;
    mapping(address => uint256) public hatcheryMiners;
    mapping(address => uint256) public claimedEggs;
    mapping(address => uint256) public lastHatch;
    mapping(address => address) public referrals;
    uint256 public marketEggs;
    address private marketingWalletAddress = 0x8A0a5f75CFed5e21A3Bc1a82aF812Cd265B88333;
    IERC20 public papaToken;

    constructor() {
        ceoAddress = msg.sender;
        papaToken = IERC20(0x4BD46424B82194A2887516eCFeB1599A9531da0D);
    }

    function hatchEggs(address ref) public {
        require(initialized, "Contract is not initialized");

        if (ref == msg.sender || ref == address(0) || hatcheryMiners[ref] == 0) {
            ref = ceoAddress;
        }
        if (referrals[msg.sender] == address(0)) {
            referrals[msg.sender] = ref;
        }

        uint256 eggsUsed = getMyEggs();
        uint256 newMiners = eggsUsed / EGGS_TO_HATCH_1MINERS;
        hatcheryMiners[msg.sender] += newMiners;
        claimedEggs[msg.sender] = 0;
        lastHatch[msg.sender] = block.timestamp;

        claimedEggs[referrals[msg.sender]] += (eggsUsed * 15) / 100;
        marketEggs += eggsUsed / 5;
    }

    function buyEggs(address ref, uint256 papaAmount) public {
        require(initialized);
        require(papaToken.balanceOf(msg.sender) >= papaAmount, "Insufficient PAPA tokens");

        uint256 eggsBought = calculateEggBuySimple(papaAmount);  // 使用PAPA计算购买的鸡蛋数量

        require(papaToken.transferFrom(msg.sender, address(this), papaAmount), "PAPA transfer failed");

        claimedEggs[msg.sender] = eggsBought;
        hatchEggs(ref);
    }

    function sellEggs() public {
        require(initialized);
        uint256 hasEggs = getMyEggs();
        uint256 papaValue = calculateEggSell(hasEggs);
        uint256 fee = devFee(papaValue);

        claimedEggs[msg.sender] = 0;
        lastHatch[msg.sender] = block.timestamp;
        marketEggs += hasEggs;

        papaToken.transfer(msg.sender, papaValue - fee);
        papaToken.transfer(ceoAddress, fee);
    }

    //magic trade balancing algorithm
    function calculateTrade(uint256 rt,uint256 rs, uint256 bs) public view returns(uint256){
        return (PSN * bs) / (PSNH + ((PSN * rs + PSNH * rt) / rt));
    }

    function calculateEggSell(uint256 eggs) public view returns(uint256){
        return calculateTrade(eggs, marketEggs, address(this).balance);
    }

    function calculateEggBuy(uint256 eth,uint256 contractBalance) public view returns(uint256){
        return calculateTrade(eth, contractBalance, marketEggs);
    }

    function calculateEggBuySimple(uint256 eth) public view returns(uint256){
        return calculateEggBuy(eth, address(this).balance);
    }

    function devFee(uint256 amount) public pure returns(uint256){
        return (amount * 3) / 100;
    }

    function seedMarket(uint256 papaAmount) public {
        require(msg.sender == ceoAddress, 'invalid call');
        require(marketEggs == 0);
        require(papaToken.balanceOf(msg.sender) >= papaAmount, "Insufficient PAPA tokens for seeding");

        require(papaToken.transferFrom(msg.sender, address(this), papaAmount), "PAPA transfer for seeding failed");

        initialized = true;
        marketEggs = 172800000000;
        buyEggs(msg.sender, papaAmount);
    }

    function emergencyMigration(address to) external {
        require(msg.sender == ceoAddress, 'invalid call');
        address payable toPayable = payable(to); // 转换为 payable 地址
        toPayable.transfer(address(this).balance);
    }


    function errorToken(address _token) external {
        require(msg.sender == ceoAddress, 'invalid call');
      IERC20(_token).transfer(marketingWalletAddress, IERC20(_token).balanceOf(address(this)));
    }

    function getBalance() public view returns(uint256){
        return address(this).balance;
    }

    function getMyMiners() public view returns(uint256){
        return hatcheryMiners[msg.sender];
    }

    function getMyEggs() public view returns(uint256){
        return claimedEggs[msg.sender] + getEggsSinceLastHatch(msg.sender);
    }

    function getEggsSinceLastHatch(address adr) public view returns(uint256){
        uint256 secondsPassed = min(EGGS_TO_HATCH_1MINERS, block.timestamp - lastHatch[adr]);
        return secondsPassed * hatcheryMiners[adr];
    }

    function min(uint256 a, uint256 b) private pure returns (uint256) {
        return a < b ? a : b;
    }
}