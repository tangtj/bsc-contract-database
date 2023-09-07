// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

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

    // APY percentages that can be adjusted by the owner
    uint256 public apy7Days;
    uint256 public apy30Days;
    uint256 public apy90Days;
    uint256 public penaltyPercentage = 10; // Penalty percentage for early unstaking

    constructor(address _rewardTokenAddress) {
        owner = msg.sender;
        rewardToken = IERC20(_rewardTokenAddress); // Initialize the reward token contract

        // Default APY percentages (adjustable by owner)
        apy7Days = 15;
        apy30Days = 65;
        apy90Days = 200;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function setAPY(uint256 _apy7Days, uint256 _apy30Days, uint256 _apy90Days) external onlyOwner {
        apy7Days = _apy7Days;
        apy30Days = _apy30Days;
        apy90Days = _apy90Days;
    }

    function stake(uint256 duration, uint256 amount) external {
        require(
            duration == 7 || duration == 30 || duration == 90,
            "Invalid staking duration. Use 7, 30, or 90."
        );
        require(amount > 0, "Amount to stake must be greater than 0");
        require(stakers[msg.sender].amount == 0, "You already have an active stake");

        uint256 apy;

        if (duration == 7) {
            apy = apy7Days;
        } else if (duration == 30) {
            apy = apy30Days;
        } else if (duration == 90) {
            apy = apy90Days;
        }

        Staker storage newStaker = stakers[msg.sender];
        newStaker.amount = amount;
        newStaker.startTimestamp = block.timestamp;
        newStaker.duration = duration;

        // Transfer BEP-20 tokens from the sender to the contract
        require(rewardToken.transferFrom(msg.sender, address(this), amount), "Token transfer failed");

        totalStaked += amount;
    }

    function unstake() external {
        Staker storage staker = stakers[msg.sender];
        require(staker.amount > 0, "You do not have an active stake");
        
        if (block.timestamp < staker.startTimestamp + staker.duration) {
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
    }

    function claimReward() external {
        Staker storage staker = stakers[msg.sender];
        require(staker.amount > 0, "You do not have an active stake");

        uint256 apy;

        if (staker.duration == 7 days) {
            apy = apy7Days;
        } else if (staker.duration == 30 days) {
            apy = apy30Days;
        } else if (staker.duration == 90 days) {
            apy = apy90Days;
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
}