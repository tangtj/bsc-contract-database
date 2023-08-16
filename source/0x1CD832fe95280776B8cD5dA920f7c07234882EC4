pragma solidity ^0.8.0;

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        return a - b;
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

interface IBEP20 {
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function balanceOf(address account) external view returns (uint256);
}

contract StakingContract {
    using SafeMath for uint256;

    struct Stake {
        uint256 amount;
        uint256 timestamp;
        uint256 lastInterestAccruedTimestamp;
        uint256 lockPeriod;
        uint256 apr;
        uint256 totalInterestPaid;
    }

    mapping(address => Stake) public stakes;
    mapping(uint256 => uint256) public totalStakedPerLockPeriod;
    IBEP20 public currencyToken;
    address public owner;
    uint256 public stakingOpenTimestamp;
    uint256 public constant maxStakeAmount = 1e6 * 10**18;
    mapping(uint256 => uint256) public quotaForLockPeriod;
    event StakeDeposited(address indexed user, uint256 amount, uint256 timestamp);
    event StakeWithdrawn(address indexed user, uint256 amount, uint256 timestamp);
    event InterestWithdrawn(address indexed user, uint256 amount, uint256 timestamp);
    event UnclaimedTokensRetrieved(address indexed owner, uint256 amount, uint256 timestamp);
    uint256 percentageBase = 10000; 

    constructor() {
        currencyToken = IBEP20(0xE02DEe9267E21A43E19658B50983102765594854);
        owner = msg.sender;
        stakingOpenTimestamp = block.timestamp.add(90 days);
        
        quotaForLockPeriod[7 days] = 1.5e6 * 10**18;   // 1.5M BRIUM for 7 days
        quotaForLockPeriod[15 days] = 2.5e6 * 10**18;  // 2.5M BRIUM for 15 days
        quotaForLockPeriod[30 days] = 4e6 * 10**18;    // 4M BRIUM for 30 days
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function");
        _;
    }

     function depositStake(uint256 _amount, uint256 _lockPeriod) external {
        require(block.timestamp < stakingOpenTimestamp, "Deposit period is over");
        require(_amount > 0 && _amount <= maxStakeAmount, "Amount must be greater than 0 and less than or equal to the max stake amount");
        require(stakes[msg.sender].amount == 0, "Existing stake found. Withdraw previous stake to deposit a new one.");
        require(_lockPeriod == 7 days || _lockPeriod == 15 days || _lockPeriod == 30 days, "Invalid lock period, only 7, 15 or 30 days are allowed");
        
        require(totalStakedPerLockPeriod[_lockPeriod].add(_amount) <= quotaForLockPeriod[_lockPeriod], "The max staking limit for this lock period has been reached");

        uint256 apr;
        if (_lockPeriod == 7 days) {
            apr = 4575; 
        } else if (_lockPeriod == 15 days) {
            apr = 2985; 
        } else if (_lockPeriod == 30 days) {
            apr = 1750; 
        }

        require(currencyToken.transferFrom(msg.sender, address(this), _amount), "Token transfer failed");

        stakes[msg.sender] = Stake({
            amount: _amount,
            timestamp: block.timestamp,
            lastInterestAccruedTimestamp: block.timestamp,
            lockPeriod: _lockPeriod,
            apr: apr,
            totalInterestPaid: 0
        });

        totalStakedPerLockPeriod[_lockPeriod] = totalStakedPerLockPeriod[_lockPeriod].add(_amount);

        emit StakeDeposited(msg.sender, _amount, block.timestamp);
    }

    function withdrawStake() external {
        require(block.timestamp >= stakes[msg.sender].timestamp.add(stakes[msg.sender].lockPeriod), "Lock period not yet expired");
        require(stakes[msg.sender].amount > 0, "No stake found");

        uint256 stakeAmount = stakes[msg.sender].amount;

        stakes[msg.sender].amount = 0;
        totalStakedPerLockPeriod[stakes[msg.sender].lockPeriod] = totalStakedPerLockPeriod[stakes[msg.sender].lockPeriod].sub(stakeAmount);

        require(currencyToken.transfer(msg.sender, stakeAmount), "Token transfer failed");

        emit StakeWithdrawn(msg.sender, stakeAmount, block.timestamp);
    }

    function withdrawInterest() external {
        Stake storage stake = stakes[msg.sender];
        require(stake.amount > 0, "No stake found");

        uint256 accruedInterest = calculateAccruedInterest(msg.sender);
        require(accruedInterest > 0, "No interest accrued");

        uint256 totalInterestForStake = stake.amount.mul(stake.apr).mul(stake.lockPeriod).div(365 days).div(percentageBase);
        require(stake.totalInterestPaid.add(accruedInterest) <= totalInterestForStake, "Total interest paid out cannot exceed the total interest for the lock period");

        require(currencyToken.balanceOf(address(this)) >= accruedInterest, "Insufficient contract balance for interest");

        stake.lastInterestAccruedTimestamp = block.timestamp;
        stake.totalInterestPaid = stake.totalInterestPaid.add(accruedInterest);

        require(currencyToken.transfer(msg.sender, accruedInterest), "Token transfer failed");

        emit InterestWithdrawn(msg.sender, accruedInterest, block.timestamp);
    }

    function remainingQuota(uint256 _lockPeriod) public view returns(uint256) {
        return quotaForLockPeriod[_lockPeriod].sub(totalStakedPerLockPeriod[_lockPeriod]);
    }

     function calculateAccruedInterest(address _staker) public view returns(uint256) {
        Stake memory stake = stakes[_staker];
        uint256 timeElapsed = block.timestamp.sub(stake.lastInterestAccruedTimestamp);
        if (timeElapsed > stake.lockPeriod) {
            timeElapsed = stake.lockPeriod;
        }
        uint256 daysInYear = 365;  
        uint256 interest = stake.amount.mul(stake.apr).mul(timeElapsed).div(daysInYear.mul(percentageBase));

        uint256 totalInterestForStake = stake.amount.mul(stake.apr).mul(stake.lockPeriod).div(365 days).div(percentageBase);
        uint256 remainingInterest = totalInterestForStake.sub(stake.totalInterestPaid);

        if(interest > remainingInterest){
            return remainingInterest;
        }

        return interest;
    }

    function canDeposit(uint256 _lockPeriod) public view returns(bool) {
        if(block.timestamp < stakingOpenTimestamp && totalStakedPerLockPeriod[_lockPeriod] < quotaForLockPeriod[_lockPeriod]) {
            return true;
        }
        return false;
    }


    function retrieveUnclaimedTokens() external onlyOwner {
        require(block.timestamp >= stakingOpenTimestamp.add(90 days), "Can only retrieve after 90 days of staking opening");
        uint256 balance = currencyToken.balanceOf(address(this));
        require(currencyToken.transfer(owner, balance), "Token transfer failed");

        emit UnclaimedTokensRetrieved(owner, balance, block.timestamp);
    }

    function getStakingOpenTimestamp() external view returns (uint256) {
        return stakingOpenTimestamp;
    }
}