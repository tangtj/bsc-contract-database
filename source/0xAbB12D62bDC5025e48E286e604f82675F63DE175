//SPDX-License-Identifier: MIT
//3f80

pragma solidity ^0.8.0;



contract SUPERGYM {
    using SafeMath for uint256;

    struct Character {
        uint256 food;
        uint256 gold;
        uint256 yield;
        uint256 level;
        uint256 class;
        uint256 lastUpgrade;
    }

    struct User {
        uint256 checkpoint;
        uint256 totalDeposited;
        uint256 totalWithdrawn;
        uint256[3] refRewards;
        uint256[3] referrals;
        address referrer;
        Character character;
    }

    modifier notContract {
        require(!isContract(msg.sender), "caller is a contract");
        _;
    }

   


    uint256 constant public PERCENTS_DIVIDER = 10000;
    uint256 constant public TIME_STEP = 24 hours;

    uint256 constant public CHARACTER_UPGRADE_TIME = 24 hours;

    uint256 constant public MARKETING_FEE = 1000; //10%

    uint256 constant public FOOD_PRICE = 0.00001 ether;
    uint256 constant public GOLD_PRICE = 0.00001 ether;

    uint256 constant public MIN_PURCHASE = 0.05 ether;
    uint256 constant public MIN_WITHDRAWAL = 0.01 ether;

    uint256 constant public GOLD_TO_FOOD_BONUS = 500; //5% 

    mapping(address => User) public users;

    uint256 public totalDeposits;
    uint256 public totalStaked;
    uint256 public totalEarned;
    uint256[] public characters = [0,0,0,0,0];

    address private marketingFund;

    uint256 public launchTime;



    event gymEntered(uint256 characterType, address indexed userAddress, uint256 timestamp);
    event foodBought(uint256 foodAmount, address indexed userAddress, uint256 timestamp);
    event characterUpgraded(uint256 lvl, address indexed userAddress, uint256 timestamp);
    event goldSold(uint256 etherAmount, address indexed userAddress, uint256 timestamp);
    event goldCollected(uint256 goldAmount, address indexed userAddress, uint256 timestamp);

    constructor(address marketingAddress, uint256 time) {
        require(!isContract(marketingAddress));
        marketingFund = marketingAddress;

        launchTime = time;
    }


    function enterGym(uint256 characterType, address referrer) public notContract {
        require(block.timestamp>=launchTime, "not launched yet");
        require(users[msg.sender].character.class == 0, "Already hatched");
        require(characterType > 0, "Must be greater than 0");
        require(characterType < 5, "Must be less than 5");
        require(referrer != msg.sender, "wrong referrer");

        User storage user = users[msg.sender];

        if(referrer != address(0) && users[referrer].character.class != 0) {
            user.referrer = referrer;
        } 

        address upline = user.referrer;


        for(uint256 i = 0; i < 3; i++) {
                if(upline != address(0)) {
                    users[upline].referrals[i] = users[upline].referrals[i].add(1);
                    upline = users[upline].referrer;
                } else break;
        }

        user.character.class = characterType;
        user.character.level = 1;
        user.character.yield = getYieldByLevel(1);
        user.checkpoint = block.timestamp;
        user.character.lastUpgrade = block.timestamp;

        characters[characterType] = characters[characterType].add(1);

        emit gymEntered(characterType, msg.sender, block.timestamp);
        
    }

    function buyFood() public payable notContract {
        require(msg.value >= MIN_PURCHASE, "insufficient amount");
        _buyFood(msg.value, msg.sender);
    }

    function _buyFood(uint256 value, address sender) private {
        require(users[sender].character.class > 0, "no character in gym");
    

        uint256 foodAmount = value.div(FOOD_PRICE);

        syncCharacter(sender);
        payFee(value);
        payRefRewards(msg.sender, value);

        users[sender].character.food = users[sender].character.food.add(foodAmount);

        

        totalStaked = totalStaked.add(value);
        totalDeposits = totalDeposits.add(1);
        characters[users[sender].character.class] = characters[users[sender].character.class].add(1);

        users[sender].totalDeposited = users[sender].totalDeposited.add(value);

        emit foodBought(foodAmount,sender, block.timestamp);
    }

    function buyFoodForGold() public notContract {
        require(users[msg.sender].character.gold > 0, "zero gold");

        uint256 goldToEther = users[msg.sender].character.gold.mul(GOLD_PRICE);
        uint256 amountWithBonus = goldToEther.add(goldToEther.mul(GOLD_TO_FOOD_BONUS).div(PERCENTS_DIVIDER));

        users[msg.sender].character.gold = 0;

        _buyFood(amountWithBonus, msg.sender);

    }

    function sellGold() public payable notContract{
        require(users[msg.sender].character.class > 0, "no character in gym");
        require(users[msg.sender].character.gold > 0, "zero gold");

        uint256 payout = users[msg.sender].character.gold.mul(GOLD_PRICE);

        require(payout >= MIN_WITHDRAWAL, "insufficient amount");

        if(payout > address(this).balance) {
            payout = address(this).balance;
        }

        users[msg.sender].character.gold = 0;
        users[msg.sender].totalWithdrawn = users[msg.sender].totalWithdrawn.add(payout);

        users[msg.sender].character.level = 1;
        users[msg.sender].character.yield = getYieldByLevel(1);


        payable(msg.sender).transfer(payout);



        emit goldSold(payout, msg.sender, block.timestamp);
    }

    function collectGold() public notContract {
        require(users[msg.sender].character.class > 0, "no character in gym");

        uint256 goldBefore = users[msg.sender].character.gold;

        syncCharacter(msg.sender);

        uint256 goldAfter = users[msg.sender].character.gold;

        uint256 totalCollected = goldAfter.sub(goldBefore);

        totalEarned = totalEarned.add(totalCollected.mul(GOLD_PRICE));

        emit goldCollected(totalCollected, msg.sender, block.timestamp);


    }


    function upgradeCharacter() public notContract {
        require(users[msg.sender].character.class > 0, "no character in gym");
        require(users[msg.sender].character.level < 21, "Character has max level");

        bool upgradable = block.timestamp.sub(users[msg.sender].character.lastUpgrade) >= CHARACTER_UPGRADE_TIME ? true : false ;

        require(upgradable, "upgrade only once a day");

        User storage user = users[msg.sender];

        syncCharacter(msg.sender);

        user.character.level = user.character.level.add(1);

        user.character.yield = getYieldByLevel(user.character.level);

        user.character.lastUpgrade = block.timestamp;

        emit characterUpgraded(user.character.level, msg.sender, block.timestamp);
    }

    
    
    
    
    function payFee(uint256 amount) private {

        uint256 marketingFee = amount.mul(MARKETING_FEE).div(PERCENTS_DIVIDER);

        payable(marketingFund).transfer(marketingFee);

    }

    function payRefRewards(address userAddress, uint256 value) private {
        User storage user = users[userAddress];

        uint256 earned = 0;
	        
	    	if (user.referrer != address(0)) {

			address upline = user.referrer;


			for (uint256 i = 0; i < 3; i++) {  
				if (upline != address(0)) {
				
    					uint256 amount = value.mul(getRefRewards(i)).div(PERCENTS_DIVIDER);
                        earned = earned.add(amount);

                        uint256 goldAmount = amount.div(GOLD_PRICE);
    					
    					users[upline].refRewards[i] = users[upline].refRewards[i].add(goldAmount);
                        users[upline].character.gold = users[upline].character.gold.add(goldAmount);
				    
					
					upline = users[upline].referrer;
				} else break;
			}

		}

        totalEarned = totalEarned.add(earned);
		
    }

    function syncCharacter(address userAddress) private {
        if(users[userAddress].checkpoint > 0) {

            uint256 foodToEther = users[userAddress].character.food.mul(FOOD_PRICE); 
            uint256 share = foodToEther.mul(users[userAddress].character.yield).div(PERCENTS_DIVIDER); 
            uint256 from = users[userAddress].checkpoint;
            uint256 to = block.timestamp;

            uint256 totalAmount = share.mul(to.sub(from)).div(TIME_STEP);

            uint256 goldAmount = totalAmount.div(GOLD_PRICE);

            if(goldAmount > 0) {

                users[userAddress].character.gold = users[userAddress].character.gold.add(goldAmount);

                
            }

        }

        users[userAddress].checkpoint = block.timestamp;
    }    
    
    
    function getYieldByLevel(uint256 level) internal pure returns(uint256) {
        return [0, 200, 205, 210, 215, 220, 225, 230, 235, 240, 245, 250, 255, 260, 265, 270, 275, 280, 285, 290, 295, 300 ][level];
    } 

    function getRefRewards(uint256 index) internal pure returns(uint256) {
        return [700, 300, 200][index];
    }

    function getUserCharacter(address userAddress) public view returns(Character memory) {

        return users[userAddress].character;
    }

    function getUpgradeTimer(address userAddress) public view returns(uint256) {
        
        return block.timestamp.sub(users[userAddress].character.lastUpgrade) > CHARACTER_UPGRADE_TIME ? 0 : CHARACTER_UPGRADE_TIME.sub(block.timestamp.sub(users[userAddress].character.lastUpgrade));
    }

    function getUserPendingGold(address userAddress) public view returns(uint256) {
            uint256 foodToEther = users[userAddress].character.food.mul(FOOD_PRICE); 
            uint256 share = foodToEther.mul(users[userAddress].character.yield).div(PERCENTS_DIVIDER); 
            uint256 from = users[userAddress].checkpoint;
            uint256 to = block.timestamp;

            uint256 totalAmount = share.mul(to.sub(from)).div(TIME_STEP);

            return totalAmount.div(GOLD_PRICE);
    }


    function getUserInfo(address userAddress) public view returns(uint256 deposited, uint256 withdrawn,uint256[3] memory _referrals, uint256[3] memory _refRewards, address _referrer) {
        deposited = users[userAddress].totalDeposited;
        withdrawn = users[userAddress].totalWithdrawn;
        _referrals = users[userAddress].referrals;
        _refRewards = users[userAddress].refRewards;
        _referrer = users[userAddress].referrer;
    }


    function getCharacters() public view returns(uint256 type1, uint256 type2, uint256 type3, uint256 type4) {
        type1 =  characters[1];
        type2 =  characters[2];
        type3 =  characters[3];
        type4 =  characters[4];
    }

    function getContractInfo() public view returns(uint256 _totalDeposits, uint256 _totalStaked, uint256 _totalEarned) {
        _totalDeposits = totalDeposits;
        _totalStaked = totalStaked;
        _totalEarned = totalEarned;
    }
    
    function isContract(address account) internal view returns (bool) {
        return account.code.length > 0;
    }
    
   
}

library SafeMath {

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        uint256 c = a / b;

        return c;
    }
    
     function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0);
        return a % b;
    }
}