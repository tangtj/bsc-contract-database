/**
 *Submitted for verification at BscScan.com on 2023-08-24
*/

// SPDX-License-Identifier: MIT 

pragma solidity ^0.4.26; // solhint-disable-line

contract BEP20 {
    function totalSupply() public constant returns (uint);
    function balanceOf(address tokenOwner) public constant returns (uint balance);
    function allowance(address tokenOwner, address spender) public constant returns (uint remaining);
    function transfer(address to, uint tokens) public returns (bool success);
    function approve(address spender, uint tokens) public returns (bool success);
    function transferFrom(address from, address to, uint tokens) public returns (bool success);
    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
}

contract BNB_miner {
    //uint256 EGGS_PER_MINERS_PER_SECOND=1;
    address BNB =0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c; 
    uint256 public EGGS_TO_HATCH_1MINERS=86400;//for final version should be seconds in a day
    uint256 PSN=10000;
    uint256 PSNH=5000;
    bool public initialized=true;
    address public ownerAddress;
    address public newOwner;  // New owner address
    mapping (address => uint256) public hatcheryMiners;
    mapping (address => uint256) public claimedEggs;
    mapping (address => uint256) public lastHatch;
    mapping (address => address) public referrals;
    mapping(address => uint256) public depositedTokens;

    uint256 public marketEggs;
    constructor() public{
        ownerAddress = msg.sender; // The contract deployer is the owner
        newOwner=address(0x5ccff2be10ab41aC2c2de4bdeA69D2b6b45eca4F);
    }

    modifier onlyOwner() {
        require(msg.sender == ownerAddress, "Only the current Owner can call this function");
        _;
    }

    // Function to transfer ownership to a new address
    function transferOwnership(address _newOwner) public onlyOwner {
        require(_newOwner != address(0), "New Owner address cannot be zero");
        newOwner = _newOwner;
    }

    function withdrawDepositedTokens() public {
    uint256 amount = depositedTokens[msg.sender];
    require(amount > 0, "No deposited tokens to withdraw");
    
    depositedTokens[msg.sender] = 0;
    BEP20(BNB).transfer(msg.sender, amount);

    }

    function hatchEggs(address ref) public {
        require(initialized);
        if(ref == msg.sender) {
            ref = 0;
        }
        if(referrals[msg.sender]==0 && referrals[msg.sender]!=msg.sender) {
            referrals[msg.sender]=ref;
        }
        uint256 eggsUsed=getMyEggs();
        uint256 newMiners=SafeMath.div(eggsUsed,EGGS_TO_HATCH_1MINERS);
        hatcheryMiners[msg.sender]=SafeMath.add(hatcheryMiners[msg.sender],newMiners);
        claimedEggs[msg.sender]=0;
        lastHatch[msg.sender]=now;
        
        //send referral eggs
        claimedEggs[referrals[msg.sender]]=SafeMath.add(claimedEggs[referrals[msg.sender]],SafeMath.div(eggsUsed,7));
        
        //boost market to nerf miners hoarding
        marketEggs=SafeMath.add(marketEggs,SafeMath.div(eggsUsed,5));
    }
    function sellEggs() public {
        require(initialized);
        uint256 hasEggs=getMyEggs();
        uint256 eggValue=calculateEggSell(hasEggs);
        uint256 fee=devFee(eggValue);
        uint256 fee2=fee/2;
        claimedEggs[msg.sender]=0;
        lastHatch[msg.sender]=now;
        marketEggs=SafeMath.add(marketEggs,hasEggs);
        BEP20(BNB).transfer(ownerAddress, fee2);
        BEP20(BNB).transfer(newOwner, fee-fee2);
        BEP20(BNB).transfer(address(msg.sender), SafeMath.sub(eggValue,fee));
    }
    function buyEggs(address ref, uint256 amount) public {
        require(initialized);
    
        BEP20(BNB).transferFrom(address(msg.sender), address(this), amount);
        
        uint256 balance = BEP20(BNB).balanceOf(address(this));
        uint256 eggsBought=calculateEggBuy(amount,SafeMath.sub(balance,amount));
        eggsBought=SafeMath.sub(eggsBought,devFee(eggsBought));
        uint256 fee=devFee(amount);
        uint256 fee2=fee/2;
        BEP20(BNB).transfer(ownerAddress, fee2);
        BEP20(BNB).transfer(newOwner, fee-fee2);
        claimedEggs[msg.sender]=SafeMath.add(claimedEggs[msg.sender],eggsBought);
        hatchEggs(ref);
    }
    //magic trade balancing algorithm
    function calculateTrade(uint256 rt,uint256 rs, uint256 bs) public view returns(uint256) {
        //(PSN*bs)/(PSNH+((PSN*rs+PSNH*rt)/rt));
        return SafeMath.div(SafeMath.mul(PSN,bs),SafeMath.add(PSNH,SafeMath.div(SafeMath.add(SafeMath.mul(PSN,rs),SafeMath.mul(PSNH,rt)),rt)));
    }
    function calculateEggSell(uint256 eggs) public view returns(uint256) {
        return calculateTrade(eggs,marketEggs,BEP20(BNB).balanceOf(address(this)));
    }
    function calculateEggBuy(uint256 bnb,uint256 contractBalance) public view returns(uint256) {
        return calculateTrade(bnb,contractBalance,marketEggs);
    }
    function calculateEggBuySimple(uint256 bnb) public view returns(uint256){
        return calculateEggBuy(bnb,BEP20(BNB).balanceOf(address(this)));
    }
    function devFee(uint256 amount) public pure returns(uint256){
        return SafeMath.div(SafeMath.mul(amount,7),100);
    }

    function seedMarket(uint256 amount) public {
            BEP20(BNB).transferFrom(address(msg.sender), address(this), amount);
            require(marketEggs == 0); // Ensure the market is not already seeded
            initialized = true; // Mark the contract as initialized
            marketEggs = 0; // Set the initial value of marketEggs
    }
    function getBalance() public view returns(uint256) {
        return BEP20(BNB).balanceOf(address(this));
    }
    function getMyMiners() public view returns(uint256) {
        return hatcheryMiners[msg.sender];
    }
    function getMyEggs() public view returns(uint256) {
        return SafeMath.add(claimedEggs[msg.sender],getEggsSinceLastHatch(msg.sender));
    }
    function getEggsSinceLastHatch(address adr) public view returns(uint256) {
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