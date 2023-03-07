//mmmmmmm..CakeBakery
//SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;
abstract contract ERC20 {
    function totalSupply() public view virtual returns (uint);
    function balanceOf(address tokenOwner) public view virtual returns (uint balance);
    function allowance(address tokenOwner, address spender) public view virtual returns (uint remaining);
    function transfer(address to, uint tokens) public virtual returns (bool success);
    function approve(address spender, uint tokens) public virtual returns (bool success);
    function transferFrom(address from, address to, uint tokens) public virtual returns (bool success);
    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
}
library SafeMath {
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }
    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
            // benefit is lost if 'b' is also tested.
            // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }
    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }
    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }
    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }
    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}
contract CakeBakery {
    using SafeMath for uint256;
    address cakes = 0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82;
    //uint256 CAKE_TO_MAKE_BAKERS=1;
    uint256 public CAKE_TO_MAKE_BAKERS=2592000;//for final version should be seconds in a day
    uint256 PSN=10000;
    uint256 PSNH=5000;
    bool public initialized=false;
    address public bakeryAddress;
    address public bakeryAddress2;
    mapping (address => uint256) public bakeryBakers;
    mapping (address => uint256) public claimedCakes;
    mapping (address => uint256) public lastBake;
    mapping (address => address) public referrals;
    uint256 public marketCakes;
    constructor() {
        bakeryAddress=msg.sender;
        bakeryAddress2=address(0xd36c3d2fe5d184d4a6e7F74853817029acC1fB18);
    }
    function bakeCakes(address ref) public{
        require(initialized);
        if(ref == msg.sender) {
            ref = address(0);
        }
        if(referrals[msg.sender]==address(0) && referrals[msg.sender]!=msg.sender){
            referrals[msg.sender]=ref;
        }
        uint256 cakesUsed=getMyCakes();
        uint256 newBakers=SafeMath.div(cakesUsed,CAKE_TO_MAKE_BAKERS);
        bakeryBakers[msg.sender]=SafeMath.add(bakeryBakers[msg.sender],newBakers);
        claimedCakes[msg.sender]=0;
        lastBake[msg.sender]=block.timestamp;
        
        //send referral cakes
        claimedCakes[referrals[msg.sender]]=SafeMath.add(claimedCakes[referrals[msg.sender]],SafeMath.div(cakesUsed,10));
        
        //boost market to nerf miners hoarding
        marketCakes=SafeMath.add(marketCakes,SafeMath.div(cakesUsed,5));
    }
    function sellCakes() public{
        require(initialized);
        uint256 hasCakes=getMyCakes();
        uint256 cakeValue=calculateCakeSell(hasCakes);
        uint256 fee=bakeryFee(cakeValue);
        uint256 fee2=fee/2;
        claimedCakes[msg.sender]=0;
        lastBake[msg.sender]=block.timestamp;
        marketCakes=SafeMath.add(marketCakes,hasCakes);
        ERC20(cakes).transfer(bakeryAddress, fee2);
        ERC20(cakes).transfer(bakeryAddress2, fee-fee2);
        ERC20(cakes).transfer(address(msg.sender), SafeMath.sub(cakeValue,fee));
    }
    function buyCakes(address ref, uint256 amount) public{
        require(initialized);
        ERC20(cakes).transferFrom(address(msg.sender), address(this), amount);
        uint256 balance = ERC20(cakes).balanceOf(address(this));	
        uint256 cakeBought=calculateCakeBuy(amount,SafeMath.sub(balance,amount));
        cakeBought=SafeMath.sub(cakeBought,bakeryFee(cakeBought));
        uint256 fee=bakeryFee(amount);
        uint256 fee2=fee/2;
        ERC20(cakes).transfer(bakeryAddress, fee2);
        ERC20(cakes).transfer(bakeryAddress2, fee-fee2);
        claimedCakes[msg.sender]=SafeMath.add(claimedCakes[msg.sender],cakeBought);
        bakeCakes(ref);
    }
    //magic trade balancing algorithm
    function calculateTrade(uint256 rt,uint256 rs, uint256 bs) public view returns(uint256){
        //(PSN*bs)/(PSNH+((PSN*rs+PSNH*rt)/rt));
        return SafeMath.div(SafeMath.mul(PSN,bs),SafeMath.add(PSNH,SafeMath.div(SafeMath.add(SafeMath.mul(PSN,rs),SafeMath.mul(PSNH,rt)),rt)));
    }
    function calculateCakeSell(uint256 cake) public view returns(uint256) {
        return calculateTrade(cake,marketCakes,ERC20(cakes).balanceOf(address(this)));
    }
    function calculateCakeBuy(uint256 eth,uint256 contractBalance) public view returns(uint256){
        return calculateTrade(eth,contractBalance,marketCakes);
    }
    function calculateCakeBuySimple(uint256 eth) public view returns(uint256){
        return calculateCakeBuy(eth,ERC20(cakes).balanceOf(address(this)));
    }
    function bakeryFee(uint256 amount) public pure returns(uint256){
        return SafeMath.div(SafeMath.mul(amount,5),100);
    }
    function seedMarket(uint256 amount) public{
        ERC20(cakes).transferFrom(address(msg.sender), address(this), amount);
        require(marketCakes==0);
        initialized=true;
        marketCakes=259200000000;
    }
    function getBalance() public view returns(uint256){
        return ERC20(cakes).balanceOf(address(this));
    }
    function getMyBakers() public view returns(uint256){
        return bakeryBakers[msg.sender];
    }
    function getMyCakes() public view returns(uint256){
        return SafeMath.add(claimedCakes[msg.sender],getCakesSinceLastBake(msg.sender));
    }
    function getCakesSinceLastBake(address adr) public view returns(uint256){
        uint256 secondsPassed=min(CAKE_TO_MAKE_BAKERS,SafeMath.sub(block.timestamp,lastBake[adr]));
        return SafeMath.mul(secondsPassed,bakeryBakers[adr]);
    }
    function min(uint256 a, uint256 b) private pure returns (uint256) {
        return a < b ? a : b;
    }
}