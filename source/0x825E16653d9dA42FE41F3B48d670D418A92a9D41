// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract StakingContract {
    struct StakingInfo {
        uint256 amountStaked;
        uint256 lastStakedTimestamp;
    }

    IERC20 public stakingToken;
    address public owner;
    uint256 public APY; 
    uint256 public lockDays;
    mapping(address => StakingInfo) public stakers;

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function");
        _;
    }

    constructor(address _stakingTokenAddress, uint256 _initialAPY, uint256 _lockDays) {
        stakingToken = IERC20(_stakingTokenAddress);
        owner = msg.sender;
        APY = _initialAPY;
        lockDays = _lockDays;
    }

    function getUserInfo(address _user) external view returns (uint256 balance, uint256 totalStaked, uint256 totalReward, uint256 currentAPY, uint256 currentLockDays, uint256 unlockDay) {
        balance = stakingToken.balanceOf(_user);
        totalStaked = stakers[_user].amountStaked;

        uint256 timeStaked = block.timestamp - stakers[_user].lastStakedTimestamp;
        totalReward = (stakers[_user].amountStaked * APY * timeStaked) / (365 days * 100); // Basic formula for calculating reward
        
        currentAPY = APY;
        currentLockDays = lockDays;
        unlockDay = stakers[_user].lastStakedTimestamp + lockDays * 1 days;

        return (balance, totalStaked, totalReward, currentAPY, currentLockDays, unlockDay);
    }

    function setAPY(uint256 _newAPY) external onlyOwner {
        APY = _newAPY;
    }

    function setLockDays(uint256 _newLockDays) external onlyOwner {
        lockDays = _newLockDays;
    }

    function stake(uint256 _amount) external {
        require(_amount > 0, "Amount must be greater than 0");
        stakingToken.transferFrom(msg.sender, address(this), _amount);

        stakers[msg.sender].amountStaked += _amount;
        stakers[msg.sender].lastStakedTimestamp = block.timestamp;
    }

    function claimReward() external {
        require(stakers[msg.sender].amountStaked > 0, "No staked amount found");

        uint256 timeStaked = block.timestamp - stakers[msg.sender].lastStakedTimestamp;
        uint256 reward = (stakers[msg.sender].amountStaked * APY * timeStaked) / (365 days * 100); // Basic formula for calculating reward

        stakers[msg.sender].lastStakedTimestamp = block.timestamp; // Reset last staked time to now
        stakingToken.transfer(msg.sender, reward);
    }

    function withdraw() external {
        require(stakers[msg.sender].amountStaked > 0, "No staked amount found");
        require(block.timestamp >= stakers[msg.sender].lastStakedTimestamp + lockDays * 1 days, "Your staked amount is still locked");

        uint256 amountToWithdraw = stakers[msg.sender].amountStaked;
        stakers[msg.sender].amountStaked = 0; // Reset staked amount to 0
        stakingToken.transfer(msg.sender, amountToWithdraw);
    }

    function emergencyWithdraw() external {
        uint256 amountToWithdraw = stakers[msg.sender].amountStaked;
        stakers[msg.sender].amountStaked = 0; // Reset staked amount to 0
        stakingToken.transfer(msg.sender, amountToWithdraw);
    }
}