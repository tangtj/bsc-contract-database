// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 value) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}

contract StakeToken {
    address public owner;
    uint256 public totalStaked;
    uint256 public totalRewardsDistributed;

    IERC20 public rewardToken; // Declare the BEP-20 token contract

    struct Staker {
        uint256 amount;
        uint256 startTimestamp;
        uint256 duration; 
        uint256 claimedRewards;
    }

    mapping(address => Staker) public stakers;
    address[] public allStakers;

    // APY percentages that can be adjusted by the owner
    uint256 public apy3Minutes;
    uint256 public apy5Minutes;
    uint256 public apy10Minutes;
    uint256 public penaltyPercentage = 30; // Penalty percentage for early unstaking

    uint256 public rewardDistributionInterval = 10 minutes;
    uint256 public lastRewardDistributionTimestamp;

    constructor(address _rewardTokenAddress) {
        owner = msg.sender;
        rewardToken = IERC20(_rewardTokenAddress); // Initialize the reward token contract

        // Default APY percentages (adjustable by owner)
        apy3Minutes = 15;
        apy5Minutes = 30;
        apy10Minutes = 60;

        // Initialize the last reward distribution timestamp to the contract deployment time
        lastRewardDistributionTimestamp = block.timestamp;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function setAPY(uint256 _apy3Minutes, uint256 _apy5Minutes, uint256 _apy10Minutes) external onlyOwner {
        apy3Minutes = _apy3Minutes;
        apy5Minutes = _apy5Minutes;
        apy10Minutes = _apy10Minutes;
    }

    function stake(uint256 minutesDuration, uint256 amount) external {
        require(
            minutesDuration == 3 || minutesDuration == 5 || minutesDuration == 10,
            "Invalid staking duration. Use 3, 5, or 10 minutes."
        );
        require(amount > 0, "Amount to stake must be greater than 0");
        require(stakers[msg.sender].amount == 0, "You already have an active stake");

        uint256 apy;

        if (minutesDuration == 3) {
            apy = apy3Minutes;
        } else if (minutesDuration == 5) {
            apy = apy5Minutes;
        } else if (minutesDuration == 10) {
            apy = apy10Minutes;
        }

        Staker storage newStaker = stakers[msg.sender];
        newStaker.amount = amount;
        newStaker.startTimestamp = block.timestamp;
        newStaker.duration = minutesDuration;

        allStakers.push(msg.sender);

        // Transfer BEP-20 tokens from the sender to the contract
        require(rewardToken.transferFrom(msg.sender, address(this), amount), "Token transfer failed");

        totalStaked += amount;
    }

    function unstake() external {
        Staker storage staker = stakers[msg.sender];
        require(staker.amount > 0, "You do not have an active stake");

        if (block.timestamp < staker.startTimestamp + staker.duration * 1 minutes) {
            // Calculate and apply the penalty for early unstaking
            uint256 penaltyAmount = (staker.amount * penaltyPercentage) / 100;
            uint256 remainingAmount = staker.amount - penaltyAmount;

            // Transfer the remaining amount (after penalty) to the user
            require(rewardToken.transfer(msg.sender, remainingAmount), "Token transfer failed");
        } else {
            // No penalty if the staking period has ended
            require(rewardToken.transfer(msg.sender, staker.amount), "Token transfer failed");
        }

        totalStaked -= staker.amount;
        delete stakers[msg.sender];

        // Remove the staker from the allStakers array
        for (uint256 i = 0; i < allStakers.length; i++) {
            if (allStakers[i] == msg.sender) {
                allStakers[i] = allStakers[allStakers.length - 1];
                allStakers.pop();
                break;
            }
        }
    }

    function claimReward() external {
        Staker storage staker = stakers[msg.sender];
        require(staker.amount > 0, "You do not have an active stake");

        uint256 apy;

        if (staker.duration == 3) {
            apy = apy3Minutes;
        } else if (staker.duration == 5) {
            apy = apy5Minutes;
        } else if (staker.duration == 10) {
            apy = apy10Minutes;
        }

        uint256 currentTime = block.timestamp;

        require(currentTime >= staker.startTimestamp, "Staking period has not started yet");
        require(currentTime > staker.startTimestamp + staker.claimedRewards, "You have already claimed your rewards for this period");

        uint256 timeSinceLastClaim = currentTime - (staker.startTimestamp + staker.claimedRewards);
        uint256 rewards = (staker.amount * apy * timeSinceLastClaim) / (365 days * 100);

        staker.claimedRewards += timeSinceLastClaim;

        // Transfer rewards in the form of BEP-20 tokens
        require(rewardToken.transfer(msg.sender, rewards), "Token transfer failed");
    }

    function approveRewardToken(uint256 amount) external {
        require(amount > 0, "Approval amount must be greater than 0");
        require(rewardToken.approve(address(this), amount), "Approval failed");
    }

    function distributeRewards() external onlyOwner {
        require(block.timestamp >= lastRewardDistributionTimestamp + rewardDistributionInterval, "Distribution interval not reached yet");

        // Iterate through the allStakers array and calculate rewards for each staker
        for (uint256 i = 0; i < allStakers.length; i++) {
            address stakerAddress = allStakers[i];
            Staker storage staker = stakers[stakerAddress];
            if (staker.amount > 0) {
                uint256 rewards = calculateRewards(staker);
                require(rewardToken.transfer(stakerAddress, rewards), "Reward distribution failed");
            }
        }

        // Update the last reward distribution timestamp
        lastRewardDistributionTimestamp = block.timestamp;
    }

    function calculateRewards(Staker storage staker) internal view returns (uint256) {
        uint256 apy;

        if (staker.duration == 3) {
            apy = apy3Minutes;
        } else if (staker.duration == 5) {
            apy = apy5Minutes;
        } else if (staker.duration == 10) {
            apy = apy10Minutes;
        }

        uint256 currentTime = block.timestamp;
        uint256 timeSinceLastClaim = currentTime - (staker.startTimestamp + staker.claimedRewards);
        uint256 rewards = (staker.amount * apy * timeSinceLastClaim) / (365 days * 100);

        return rewards;
    }
}