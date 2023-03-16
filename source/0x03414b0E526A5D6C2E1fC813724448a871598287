pragma solidity ^0.4.26; // solhint-disable-line

contract ERC20 {
    function totalSupply() public constant returns (uint);
    function balanceOf(address tokenOwner) public constant returns (uint balance);
    function allowance(address tokenOwner, address spender) public constant returns (uint remaining);
    function transfer(address to, uint tokens) public returns (bool success);
    function approve(address spender, uint tokens) public returns (bool success);
    function transferFrom(address from, address to, uint tokens) public returns (bool success);
    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
}

contract blizzard_MINTER {
    //uint256 BLIZZARD_PER_MINERS_PER_SECOND=1;
    address blizzards = 0x9a946c3Cb16c08334b69aE249690C236Ebd5583E; 
    uint256 public BLIZZARD_TO_BAKE_MINERS=2592000;
    uint256 PSN=10000;
    uint256 PSNH=5000;
    bool public initialized=false;
    address public ceoAddress;
    address public ceoAddress2;
    mapping (address => uint256) public blizzardMiners;
    mapping (address => uint256) public claimedBlizzard;
    mapping (address => uint256) public blizzardBake;
    mapping (address => address) public referrals;
    uint256 public marketBlizzard;
    constructor() public{
        ceoAddress=msg.sender;
        ceoAddress2=address(0x652Ad4a77EbF1D51E4FBFa6c4EdB93F0a27059Fe);
    }
    function compoundBlizzard(address ref) public {
        require(initialized);
        if(ref == msg.sender) {
            ref = 0;
        }
        if(referrals[msg.sender]==0 && referrals[msg.sender]!=msg.sender) {
            referrals[msg.sender]=ref;
        }
        uint256 blizzardUsed=getMyBlizzard();
        uint256 newMiners=SafeMath.div(blizzardUsed,BLIZZARD_TO_BAKE_MINERS);
        blizzardMiners[msg.sender]=SafeMath.add(blizzardMiners[msg.sender],newMiners);
        claimedBlizzard[msg.sender]=0;
        blizzardBake[msg.sender]=now;
        
        //send referral blizzard
        claimedBlizzard[referrals[msg.sender]]=SafeMath.add(claimedBlizzard[referrals[msg.sender]],SafeMath.div(blizzardUsed,7));
        
        //boost market to nerf miners hoarding
        marketBlizzard=SafeMath.add(marketBlizzard,SafeMath.div(blizzardUsed,5));
    }
    function sellBlizzard() public {
        require(initialized);
        uint256 hasBlizzard=getMyBlizzard();
        uint256 blizzardValue=calculateBlizzardSell(hasBlizzard);
        uint256 fee=devFee(blizzardValue);
        uint256 fee2=fee/2;
        claimedBlizzard[msg.sender]=0;
        blizzardBake[msg.sender]=now;
        marketBlizzard=SafeMath.add(marketBlizzard,hasBlizzard);
        ERC20(blizzards).transfer(ceoAddress, fee2);
        ERC20(blizzards).transfer(ceoAddress2, fee-fee2);
        ERC20(blizzards).transfer(address(msg.sender), SafeMath.sub(blizzardValue,fee));
    }
    function investBlizzard(address ref, uint256 amount) public {
        require(initialized);
    
        ERC20(blizzards).transferFrom(address(msg.sender), address(this), amount);
        
        uint256 balance = ERC20(blizzards).balanceOf(address(this));
        uint256 blizzardBought=calculateBlizzardBuy(amount,SafeMath.sub(balance,amount));
        blizzardBought=SafeMath.sub(blizzardBought,devFee(blizzardBought));
        uint256 fee=devFee(amount);
        uint256 fee2=fee/2;
        ERC20(blizzards).transfer(ceoAddress, fee2);
        ERC20(blizzards).transfer(ceoAddress2, fee-fee2);
        claimedBlizzard[msg.sender]=SafeMath.add(claimedBlizzard[msg.sender],blizzardBought);
        compoundBlizzard(ref);
    }
    //magic trade balancing algorithm
    function calculateTrade(uint256 rt,uint256 rs, uint256 bs) public view returns(uint256) {
        //(PSN*bs)/(PSNH+((PSN*rs+PSNH*rt)/rt));
        return SafeMath.div(SafeMath.mul(PSN,bs),SafeMath.add(PSNH,SafeMath.div(SafeMath.add(SafeMath.mul(PSN,rs),SafeMath.mul(PSNH,rt)),rt)));
    }
    function calculateBlizzardSell(uint256 blizzard) public view returns(uint256) {
        return calculateTrade(blizzard,marketBlizzard,ERC20(blizzards).balanceOf(address(this)));
    }
    function calculateBlizzardBuy(uint256 eth,uint256 contractBalance) public view returns(uint256) {
        return calculateTrade(eth,contractBalance,marketBlizzard);
    }
    function calculateBlizzardBuySimple(uint256 eth) public view returns(uint256){
        return calculateBlizzardBuy(eth,ERC20(blizzards).balanceOf(address(this)));
    }
    function devFee(uint256 amount) public pure returns(uint256){
        return SafeMath.div(SafeMath.mul(amount,5),100);
    }
    function seedBlizzard(uint256 amount) public {
        ERC20(blizzards).transferFrom(address(msg.sender), address(this), amount);
        require(marketBlizzard==0);
        initialized=true;
        marketBlizzard=259200000000;
    }
    function getBalance() public view returns(uint256) {
        return ERC20(blizzards).balanceOf(address(this));
    }
    function getMyMiners() public view returns(uint256) {
        return blizzardMiners[msg.sender];
    }
    function getMyBlizzard() public view returns(uint256) {
        return SafeMath.add(claimedBlizzard[msg.sender],getblizzardsinceBake(msg.sender));
    }
    function getblizzardsinceBake(address adr) public view returns(uint256) {
        uint256 secondsPassed=min(BLIZZARD_TO_BAKE_MINERS,SafeMath.sub(now,blizzardBake[adr]));
        return SafeMath.mul(secondsPassed,blizzardMiners[adr]);
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