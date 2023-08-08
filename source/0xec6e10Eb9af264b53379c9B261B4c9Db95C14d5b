// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract Farm {
    address public owner;
    IERC20 public token;  // Token being farmed
    uint256 public rewardPerToken = 200;  // Reward per token (as a basis point, e.g., 2%)

    struct UserInfo {
        uint256 amountStaked;
        uint256 rewardDebt;
    }

    mapping(address => UserInfo) public userInfo;
    uint256 public totalStaked;
    uint256 public lastRewardBlock;

    event Staked(address indexed user, uint256 amount);
    event Withdrawn(address indexed user, uint256 amount);
    event Claimed(address indexed user, uint256 amount);

    constructor(address _tokenAddress) {
        owner = msg.sender;
        token = IERC20(_tokenAddress);
        lastRewardBlock = block.number;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function updateRewardPerToken(uint256 _newRewardPerToken) external onlyOwner {
        rewardPerToken = _newRewardPerToken;
    }

    function pendingReward(address _user) external view returns (uint256) {
        UserInfo storage user = userInfo[_user];
        uint256 accRewardPerToken = rewardPerToken * (block.number - lastRewardBlock);
        return user.amountStaked * accRewardPerToken / 10000 - user.rewardDebt;
    }

    function stake(uint256 _amount) external {
        UserInfo storage user = userInfo[msg.sender];
        require(_amount > 0, "Amount must be greater than 0");

        updatePool();

        if (user.amountStaked > 0) {
            uint256 pending = user.amountStaked * rewardPerToken * (block.number - lastRewardBlock) / 10000 - user.rewardDebt;
            if (pending > 0) {
                token.transfer(msg.sender, pending);
                emit Claimed(msg.sender, pending);
            }
        }

        token.transferFrom(msg.sender, address(this), _amount);
        user.amountStaked += _amount;
        user.rewardDebt = user.amountStaked * rewardPerToken * (block.number - lastRewardBlock) / 10000;

        totalStaked += _amount;
        emit Staked(msg.sender, _amount);
    }

    function withdraw(uint256 _amount) external {
        UserInfo storage user = userInfo[msg.sender];
        require(user.amountStaked >= _amount, "Insufficient staked amount");

        updatePool();

        uint256 pending = user.amountStaked * rewardPerToken * (block.number - lastRewardBlock) / 10000 - user.rewardDebt;
        if (pending > 0) {
            token.transfer(msg.sender, pending);
            emit Claimed(msg.sender, pending);
        }

        user.amountStaked -= _amount;
        user.rewardDebt = user.amountStaked * rewardPerToken * (block.number - lastRewardBlock) / 10000;

        totalStaked -= _amount;
        token.transfer(msg.sender, _amount);
        emit Withdrawn(msg.sender, _amount);
    }

    function claim() external {
        UserInfo storage user = userInfo[msg.sender];
        updatePool();

        uint256 pending = user.amountStaked * rewardPerToken * (block.number - lastRewardBlock) / 10000 - user.rewardDebt;
        require(pending > 0, "No pending reward to claim");

        token.transfer(msg.sender, pending);
        emit Claimed(msg.sender, pending);

        user.rewardDebt = user.amountStaked * rewardPerToken * (block.number - lastRewardBlock) / 10000;
    }

    function updatePool() internal {
        if (block.number <= lastRewardBlock) {
            return;
        }
        uint256 accRewardPerToken = rewardPerToken * (block.number - lastRewardBlock);
        totalStaked += accRewardPerToken;
        lastRewardBlock = block.number;
    }

    function getStakedAmount(address _user) external view returns (uint256) {
        return userInfo[_user].amountStaked;
    }

    function getRewardDebt(address _user) external view returns (uint256) {
        return userInfo[_user].rewardDebt;
    }

    function getTotalStaked() external view returns (uint256) {
        return totalStaked;
    }

    function getLastRewardBlock() external view returns (uint256) {
        return lastRewardBlock;
    }
}