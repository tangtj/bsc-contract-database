// SpdX-License-Identifier: MIT 
 
pragma solidity >=0.4.22 <0.9.0;

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

contract PURPLE {
    uint256 public totalInvested;
    uint256[] public affiliateCommision = [50, 25, 5];
    uint256 constant public pd = 1000;
    uint256 public totalAffilateBonus;
    uint256 constant public projectCommision = 50;
    uint256 constant public reInvestBonus = 100;
    uint256 constant public reInvestMinimum = 0.25 ether;
	uint256 constant public timer = 1 days;
    using SafeMath for uint256;
    uint256 constant public minimum = 0.1 ether;

    struct Investment {
        uint8 package;
        uint256 startTime;
		uint256 roi;
		uint256 amount;
		uint256 endTime;
        uint256 profit;
	}

	struct Client {
		Investment [] investments;
        uint256 reward;
		uint256 cp;
		address recruiter;
		uint256 totalReward;
        uint256[3] levels;
        uint256 PNCX;
	}

     struct Package {
        uint256 timeStamp;
        uint256 roi;
    }

    Package[] internal packages;

    mapping (address => Client) internal clients;

	uint256 public timeContractStarts;
	address payable public projectPurse;
    address payable owner;

    modifier ownerPass(){
     require(owner == msg.sender, "owner authrisation needed");
     _;
   }


	event InvestmentEvent(address indexed client, uint8 package, uint256 roi, uint256 amount, uint256 profit, uint256 startTime, uint256 endTime);
	event CashOut(address indexed client, uint256 amount);
    event NewClient(address client);
	
	constructor(uint256 contractStartTime, address payable projectWallet) public {
		require(!isContract(projectWallet));
		require(contractStartTime >= block.timestamp);
		timeContractStarts = contractStartTime;
        projectPurse = projectWallet;
        owner = msg.sender;

        packages.push(Package(7, 150));
        packages.push(Package(14, 82));
        packages.push(Package(21, 59));
        packages.push(Package(28, 48));
        packages.push(Package(35, 41));
        packages.push(Package(42, 40));
        packages.push(Package(49, 45));
	}
    

    function deposit(address recruiter, uint8 package) public payable {
		require(msg.value >= minimum, "Minimum amount allowed is 0.10");
        require(package < 7, "you selected wrong package!");
		uint256 depositFee = msg.value.mul(projectCommision).div(pd);
		projectPurse.transfer(depositFee);
		Client storage client = clients[msg.sender];
		if (client.recruiter == address(0)) {
			if (clients[recruiter].investments.length > 0 && recruiter != msg.sender) {
				client.recruiter = recruiter;
			}
			address pyramind = client.recruiter;
			for (uint256 i = 0; i < 3; i++) {
				if (pyramind != address(0) && pyramind != msg.sender) {
					clients[pyramind].levels[i] = clients[pyramind].levels[i].add(1);
					pyramind = clients[pyramind].recruiter;
				} else break;
			}
		}

		if (client.recruiter != address(0)) {
			address pyramind = client.recruiter;
			for (uint256 i = 0; i < 3; i++) {
				if (pyramind != address(0) && pyramind != msg.sender) {
					uint256 amount = msg.value.mul(affiliateCommision[i]).div(pd);
					clients[pyramind].reward = clients[pyramind].reward.add(amount);
					clients[pyramind].totalReward = clients[pyramind].totalReward.add(amount);
					pyramind = clients[pyramind].recruiter;
				} else break;
			}

		}

		if (client.investments.length == 0) {
			client.cp = block.timestamp;
			emit NewClient(msg.sender);
		}
        
        if(package == 6) client.PNCX = client.PNCX.add(msg.value);
		(uint256 roi, uint256 profit, uint256 endTime) = getInvestmentsInfo(package, msg.value);
		client.investments.push(Investment(package, block.timestamp, roi, msg.value, endTime, profit));

		totalInvested = totalInvested.add(msg.value);
		emit InvestmentEvent(msg.sender, package, roi, msg.value, profit, block.timestamp, endTime);
	}


	function isContract(address wallet) internal view returns (bool) {
        uint size;
        assembly { size := extcodesize(wallet) }
        return size > 0;
    }

    
    function reInvest() public {
         Client storage client = clients[msg.sender];
         uint8 package = 6; 
         uint256 cashAvailable = getClientDividends(msg.sender);
         require(cashAvailable >= reInvestMinimum, "minimum you reinvest is 0.25 BNB");
         uint256 reinvest_bonus = cashAvailable.mul(reInvestBonus).div(pd);
         cashAvailable = cashAvailable.add(reinvest_bonus);
         client.PNCX = client.PNCX.add(cashAvailable);
		 (uint256 roi, uint256 profit, uint256 endTime) = getInvestmentsInfo(package, cashAvailable);
		 client.investments.push(Investment(package, block.timestamp, roi, cashAvailable, endTime, profit));
		 client.cp = block.timestamp;
	}

	function getContractBalance() public view returns (uint256) {
		return address(this).balance;
	}

      function cashOut() public {
		Client storage client = clients[msg.sender];
		uint256 cashAvailable = getClientDividends(msg.sender);
		uint256 project_fees = cashAvailable.mul(projectCommision).div(pd);
		   cashAvailable = cashAvailable.sub(project_fees);
		uint256 affliate_commision = getClientAffiliateReward(msg.sender);
		if (affliate_commision > 0) {
			client.reward = 0;
			cashAvailable = cashAvailable.add(affliate_commision);
		}
		require(cashAvailable > 0, "Client has no dividends");
		uint256 contractBalance = address(this).balance;
		if (contractBalance < cashAvailable) {
			cashAvailable = contractBalance;
		}
		client.cp = block.timestamp;
		msg.sender.transfer(cashAvailable);
		emit CashOut(msg.sender, cashAvailable);

	}

	function getPackageInfo(uint8 package) public view returns(uint256 timestamp, uint256 roi) {
		timestamp = packages[package].timeStamp;
		roi = packages[package].roi;
	}

	function getRoi(uint8 package) public view returns (uint256) {
        return packages[package].roi;
    }


	function getClientDividends(address clientAddress) public view returns (uint256) {
		Client storage client = clients[clientAddress];
		uint256 cashAvailable;

		for (uint256 i = 0; i < client.investments.length; i++) {
			if (client.cp < client.investments[i].endTime) {
					uint256 share = client.investments[i].amount.mul(client.investments[i].roi).div(pd);
					uint256 from = client.investments[i].startTime > client.cp ? client.investments[i].startTime : client.cp;
					uint256 to = client.investments[i].endTime < block.timestamp ? client.investments[i].endTime : block.timestamp;
					if (from < to) {
						cashAvailable = cashAvailable.add(share.mul(to.sub(from)).div(timer));
					}else if (block.timestamp > client.investments[i].endTime) {
					cashAvailable = cashAvailable.add(client.investments[i].profit);
				}
			}
		}

		return cashAvailable;
	}



    function getClientInvestmentDetails(address clientAddress, uint256 index) public view returns(uint8 package, uint256 roi, uint256 amount, uint256 profit, uint256 startTime, uint256 endTime, uint256 PNCX) {
	    Client storage client = clients[clientAddress];
        profit = client.investments[index].profit;
		startTime = client.investments[index].startTime;
		endTime = client.investments[index].endTime;
        PNCX = client.PNCX;
		package = client.investments[index].package;
		roi = client.investments[index].roi;
		amount = client.investments[index].amount;
	}

	function getClientAffiliateTotalReward(address clientAddress) public view returns(uint256) {
		return clients[clientAddress].totalReward;
	}

	function getClientAffiliateCashout(address clientAddress) public view returns(uint256) {
		return clients[clientAddress].totalReward.sub(clients[clientAddress].reward);
	}

	function getClientAvailable(address clientAddress) public view returns(uint256) {
		return getClientAffiliateReward(clientAddress).add(getClientDividends(clientAddress));
	}

	function getClientAmountOfDeposits(address clientAddress) public view returns(uint256) {
		return clients[clientAddress].investments.length;
	}

	function getClientTotalInvesments(address clientAddress) public view returns(uint256 amount) {
		for (uint256 i = 0; i < clients[clientAddress].investments.length; i++) {
			amount = amount.add(clients[clientAddress].investments[i].amount);
		}
	}

    function getClientCheckpoint(address clientAddress) public view returns(uint256) {
		return clients[clientAddress].cp;
	}

	function getClientRecruiter(address clientAddress) public view returns(address) {
		return clients[clientAddress].recruiter;
	}

	function getClientDownlineCount(address clientAddress) public view returns(uint256, uint256, uint256) {
		return (clients[clientAddress].levels[0], clients[clientAddress].levels[1], clients[clientAddress].levels[2]);
	}

    function getInvestmentsInfo(uint8 package, uint256 amount) public view returns (uint256 roi, uint256 profit, uint256 endTime) {
                roi = getRoi(package);
                profit = amount.mul(roi).div(pd).mul(packages[package].timeStamp);
                endTime = block.timestamp.add(packages[package].timeStamp.mul(timer));
	}

	function getClientAffiliateReward(address clientAddress) public view returns(uint256) {
		return clients[clientAddress].reward;

	}

	
}