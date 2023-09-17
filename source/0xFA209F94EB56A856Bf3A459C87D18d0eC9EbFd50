pragma solidity 0.5.10;

interface IERC20 {
    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

library SafeERC20 {
    using SafeMath for uint;

    function safeTransfer(IERC20 token, address to, uint value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(IERC20 token, address from, address to, uint value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    function safeApprove(IERC20 token, address spender, uint value) internal {
        require((value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }
    function callOptionalReturn(IERC20 token, bytes memory data) private {
        require(isContract(address(token)), "SafeERC20: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = address(token).call(data);
        require(success, "SafeERC20: low-level call failed");

        if (returndata.length > 0) { // Return data is optional
            // solhint-disable-next-line max-line-length
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }

	function isContract(address addr) internal view returns (bool) {
        uint size;
        assembly { size := extcodesize(addr) }
        return size > 0;
    }
}

contract CityOfParadise {
	using SafeMath for uint256;
	using SafeERC20 for IERC20;

	IERC20 public token;
	uint256[] public REFERRAL_PERCENTS = [200, 200, 200, 50, 50, 50, 50];
	uint256 constant public TOTAL_REF = 800;
	uint256 constant public PROJECT_FEE = 200;
	uint256 constant public PERCENTS_DIVIDER = 10000;
	uint256 constant public TIME_STEP = 1 days;

	uint256 public totalInvested;

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
		uint256[7] levels;
		uint256 bonus;
		uint256 totalBonus;
		uint256 withdrawn;
	}

	mapping (address => User) internal users;

	uint256 public startDate;
	address payable public commissionWallet;

	event Newbie(address user);
	event NewDeposit(address indexed user, uint8 plan, uint256 amount);
	event Withdrawn(address indexed user, uint256 amount);
	event RefBonus(address indexed referrer, address indexed referral, uint256 indexed level, uint256 amount);
	event FeePayed(address indexed user, uint256 totalAmount);

	constructor(address tokenAddr, address payable wallet, uint256 start) public {
		require(!isContract(wallet) && isContract(tokenAddr), "Wrong Requirement");
		token = IERC20(tokenAddr);
		commissionWallet = wallet;

		if(start>0){
            startDate = start;
        }
        else{
            startDate = block.timestamp;
        }

        plans.push(Plan(100, 288));
        plans.push(Plan(100, 408));
		plans.push(Plan(100, 504));
        plans.push(Plan(100, 600));
	}

	function getAllowance(address userAddress) public view returns(uint256) {
		return token.allowance(userAddress, address(this));
	}

	function invest(address referrer, uint8 plan) public {
		require(block.timestamp > startDate, "Contract does not launch yet");
		uint256 value;
		uint256 rent;
		uint256 total_value;

        if (plan == 0) {
			value = 50 ether;
			rent = 5 ether;
		} else if (plan == 1) {
			value = 100 ether;
			rent = 10 ether;
		} else if (plan == 2) {
			value = 200 ether;
			rent = 20 ether;
		} else if (plan == 3) {
			value = 500 ether;
			rent = 50 ether;
		} else {
			revert("Invalid plan");
		}
		total_value = value.add(rent);

		require(token.balanceOf(msg.sender) >= total_value, "Insufficient token balance");
		require(total_value <= token.allowance(msg.sender, address(this)), "Approve token before transferring");
		token.safeTransferFrom(msg.sender, address(this), total_value);

		token.safeTransfer(commissionWallet, rent);
		emit FeePayed(msg.sender, rent);

		User storage user = users[msg.sender];

		if (user.referrer == address(0)) {
			if (referrer != msg.sender) {
				user.referrer = referrer;
			}

			address upline = user.referrer;
			for (uint256 i = 0; i < 7; i++) {
				if (upline != address(0)) {
					users[upline].levels[i] = users[upline].levels[i].add(1);
					upline = users[upline].referrer;
				} else break;
			}
		}

		if (user.deposits.length == 0) {
			emit Newbie(msg.sender);
		}

		user.deposits.push(Deposit(plan, value, block.timestamp));

		totalInvested = totalInvested.add(value);

		emit NewDeposit(msg.sender, plan, value);
	}

	function withdraw() public {
		User storage user = users[msg.sender];

		require(user.checkpoint.add(TIME_STEP) < block.timestamp, "Withdrawal only once a day");
		uint256 totalAmount = getUserDividends(msg.sender);
		require(totalAmount >= 10 ether, "Minimum 10");

		if(totalAmount > 5000 ether) {
			totalAmount = 5000 ether;
		}

		uint256 contractBalance = token.balanceOf(address(this));
		if (contractBalance < totalAmount) {
			totalAmount = contractBalance;
		}

		user.checkpoint = block.timestamp;
		user.withdrawn = user.withdrawn.add(totalAmount);

		uint256 totalAmount_deducted = totalAmount.sub(totalAmount.mul(1000).div(PERCENTS_DIVIDER));
		token.safeTransfer(msg.sender, totalAmount_deducted);
		uint256 system_fee = totalAmount.mul(PROJECT_FEE).div(PERCENTS_DIVIDER);
		token.safeTransfer(commissionWallet, system_fee);
		emit Withdrawn(msg.sender, totalAmount);

		if (user.referrer != address(0)) {
			address upline = user.referrer;
			for (uint256 i = 0; i < 7; i++) {
				if (upline != address(0)) {
					uint256 amount = totalAmount.mul(REFERRAL_PERCENTS[i]).div(PERCENTS_DIVIDER);
					users[upline].totalBonus = users[upline].totalBonus.add(amount);
					token.safeTransfer(upline, amount);
					emit RefBonus(upline, msg.sender, i, amount);
					upline = users[upline].referrer;
				} else break;
			}
		}
	}

	function getContractBalance() public view returns (uint256) {
		return token.balanceOf(address(this));
	}

	function getPlanInfo(uint8 plan) public view returns(uint256 time, uint256 percent) {
		time = plans[plan].time;
		percent = plans[plan].percent;
	}

	function getUserDividends(address userAddress) public view returns (uint256) {
		User storage user = users[userAddress];

		uint256 totalAmount;

		for (uint256 i = 0; i < user.deposits.length; i++) {
			uint256 finish = user.deposits[i].start.add(plans[user.deposits[i].plan].time.mul(1 days));
			if (user.checkpoint < finish) {
				uint256 share = user.deposits[i].amount.mul(plans[user.deposits[i].plan].percent).div(PERCENTS_DIVIDER);
				uint256 from = user.deposits[i].start > user.checkpoint ? user.deposits[i].start : user.checkpoint;
				uint256 to = finish < block.timestamp ? finish : block.timestamp;
				if (from < to) {
					totalAmount = totalAmount.add(share.mul(to.sub(from)).div(TIME_STEP));
				}
			}
		}

		return totalAmount;
	}

	function getUserTotalWithdrawn(address userAddress) public view returns (uint256) {
		return users[userAddress].withdrawn;
	}

	function getUserCheckpoint(address userAddress) public view returns(uint256) {
		return users[userAddress].checkpoint;
	}

	function getUserReferrer(address userAddress) public view returns(address) {
		return users[userAddress].referrer;
	}

	function getUserDownlineCount(address userAddress) public view returns(uint256[7] memory referrals) {
		return (users[userAddress].levels);
	}

	function getUserTotalReferrals(address userAddress) public view returns(uint256) {
		return users[userAddress].levels[0]+users[userAddress].levels[1]+users[userAddress].levels[2]+users[userAddress].levels[3]+users[userAddress].levels[4]+users[userAddress].levels[5]+users[userAddress].levels[6];
	}

	function getUserReferralTotalBonus(address userAddress) public view returns(uint256) {
		return users[userAddress].totalBonus;
	}

	function getUserAvailable(address userAddress) public view returns(uint256) {
		return getUserDividends(userAddress);
	}

	function getUserAmountOfDeposits(address userAddress) public view returns(uint256) {
		return users[userAddress].deposits.length;
	}

	function getUserTotalDeposits(address userAddress) public view returns(uint256 amount) {
		for (uint256 i = 0; i < users[userAddress].deposits.length; i++) {
			amount = amount.add(users[userAddress].deposits[i].amount);
		}
	}

	function getUserDepositInfo(address userAddress, uint256 index) public view returns(uint8 plan, uint256 percent, uint256 amount, uint256 start, uint256 finish) {
	    User storage user = users[userAddress];

		plan = user.deposits[index].plan;
		percent = plans[plan].percent;
		amount = user.deposits[index].amount;
		start = user.deposits[index].start;
		finish = user.deposits[index].start.add(plans[user.deposits[index].plan].time.mul(1 days));
	}

	function getSiteInfo() public view returns(uint256 _totalInvested, uint256 _totalBonus) {
		return(totalInvested, totalInvested.mul(TOTAL_REF).div(PERCENTS_DIVIDER));
	}

	function getUserInfo(address userAddress) public view returns(uint256 availableBalance, uint256 depositLength, uint256 totalDeposit, uint256 totalWithdrawn, uint256 totalReferrals, uint256 totalBonus) {
		return(getUserAvailable(userAddress), getUserAmountOfDeposits(userAddress), getUserTotalDeposits(userAddress), getUserTotalWithdrawn(userAddress), getUserTotalReferrals(userAddress), getUserReferralTotalBonus(userAddress));
	}

	function isContract(address addr) internal view returns (bool) {
        uint size;
        assembly { size := extcodesize(addr) }
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