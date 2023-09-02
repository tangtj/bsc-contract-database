//SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

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


abstract contract ReentrancyGuard {
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    error ReentrancyGuardReentrantCall();

    constructor() {
        _status = _NOT_ENTERED;
    }

    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        if (_status == _ENTERED) {
            revert ReentrancyGuardReentrantCall();
        }

        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        _status = _NOT_ENTERED;
    }

    function _reentrancyGuardEntered() internal view returns (bool) {
        return _status == _ENTERED;
    }
}

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

    error OwnableUnauthorizedAccount(address account);

    error OwnableInvalidOwner(address owner);

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor(address initialOwner) {
        _transferOwnership(initialOwner);
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        if (owner() != _msgSender()) {
            revert OwnableUnauthorizedAccount(_msgSender());
        }
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        if (newOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}


contract DDRStaking is Ownable, ReentrancyGuard {

    uint256 private constant DENOMINATOR = 10000;
    uint256 private constant REWARD_PERIOD = 365 days;
    uint256 private constant EARLY_WITHDRAWAL_PENALTY = 60;

    bool private lockupPeriodEnabled = true;

    uint256 private withdrawalTaxes = 0;

    struct Pool {
        uint256 apy;
        uint256 lockupPeriod;
        bool isOpen;
        uint256 totalStaked;
        uint256 totalWithdrawn;
    }

    Pool firstPool = Pool(1000, 7 days, false, 0, 0);
    Pool secondPool = Pool(2500, 30 days, false, 0, 0);
    Pool thirdPool = Pool(5000, 60 days, false, 0, 0);

    struct User {
        uint256 stakedOnTimestamp;
        uint256 stakeEndTimestamp;
        uint256 stakedAmount;
        uint256 lockupTimestamp;
        uint256 lastActionTimestamp;
    }

    IERC20 private quackToken = IERC20(0x647f0Ba82243314f78fb7B61d06926C98040a00E);

    mapping(uint256 => Pool) private pools; // Pool ID => Pool struct

    mapping(address => mapping(uint256 => User)) private userInPool; // User address => Pool ID => User struct of this individual pool

    constructor() Ownable(msg.sender) {
        pools[0] = firstPool;
        pools[1] = secondPool;
        pools[2] = thirdPool;
    }

    function stake(uint256 poolId, uint256 amount) external nonReentrant {
        require(amount > 0, "Amount must be greater than 0");
        require(poolId >= 0 && poolId <= 2, "Pool ID must be between 0 and 2");
        require(pools[poolId].isOpen, "Pool is not open");
        
        IERC20(quackToken).transferFrom(msg.sender, address(this), amount);

        Pool memory pool = pools[poolId];

        User memory user = userInPool[msg.sender][poolId];

        if (user.stakedAmount > 0) {

            uint256 secondsPassed = block.timestamp - user.lastActionTimestamp;

            uint256 rewards = calculateRewards(msg.sender, poolId, secondsPassed, user.stakedAmount);

            uint256 addAmount = amount + rewards;

            userInPool[msg.sender][poolId].lastActionTimestamp = block.timestamp;

            userInPool[msg.sender][poolId].stakedAmount += addAmount;

            userInPool[msg.sender][poolId].lockupTimestamp = block.timestamp + pool.lockupPeriod;

            userInPool[msg.sender][poolId].stakeEndTimestamp = block.timestamp + pool.lockupPeriod;

            pools[poolId].totalStaked += addAmount;

        } else {
            
            userInPool[msg.sender][poolId] = User(block.timestamp, block.timestamp + pool.lockupPeriod, amount, block.timestamp + pool.lockupPeriod, block.timestamp);

            pools[poolId].totalStaked += amount;

        }

        emit newStake(msg.sender, poolId, amount);
    }

    function reStake(uint256 poolId) external nonReentrant {
        require(poolId >= 0 && poolId <= 2, "Pool ID must be between 0 and 2");
        require(pools[poolId].isOpen, "Pool is not open");

        Pool memory pool = pools[poolId];

        User memory user = userInPool[msg.sender][poolId];

        require(user.stakedAmount > 0, "User has no staked amount in this pool.");

        uint256 secondsPassed = block.timestamp - user.lastActionTimestamp;

        uint256 rewards = calculateRewards(msg.sender, poolId, secondsPassed, user.stakedAmount);

        userInPool[msg.sender][poolId].lastActionTimestamp = block.timestamp;

        userInPool[msg.sender][poolId].stakedAmount += rewards;

        userInPool[msg.sender][poolId].stakeEndTimestamp = block.timestamp + pool.lockupPeriod;

        userInPool[msg.sender][poolId].lockupTimestamp = block.timestamp + pool.lockupPeriod;

        pools[poolId].totalStaked += rewards;

        emit reStaked(msg.sender, poolId, rewards);
    }

    function claimRewards(uint256 poolId) external nonReentrant {
        require(poolId >= 0 && poolId <= 2, "Pool ID must be between 0 and 2.");

        User memory user = userInPool[msg.sender][poolId];

        require(user.stakedAmount > 0, "User has no staked amount in this pool.");

        uint256 secondsPassed = block.timestamp - user.lastActionTimestamp;

        uint256 rewards = calculateRewards(msg.sender, poolId, secondsPassed, user.stakedAmount);

        require(rewards > 0, "No rewards to claim.");

        userInPool[msg.sender][poolId].lastActionTimestamp = block.timestamp;

        IERC20(quackToken).transfer(msg.sender, rewards);

        emit claimedRewards(msg.sender, poolId, rewards);
    }

    function withdrawStake(uint256 poolId) external nonReentrant {
        require(poolId >= 0 && poolId <= 2, "Pool ID must be between 0 and 2.");

        User memory user = userInPool[msg.sender][poolId];

        require(user.stakedAmount > 0, "User has no staked amount in this pool.");
        
        if(lockupPeriodEnabled) {
            require(block.timestamp >= user.lockupTimestamp, "Lockup period has not passed yet.");
        }

        uint256 secondsPassed = block.timestamp - user.lastActionTimestamp;

        uint256 rewards = calculateRewards(msg.sender, poolId, secondsPassed, user.stakedAmount);

        uint256 totalAmount;

        if(rewards > 0) {
            totalAmount = user.stakedAmount + rewards;
        } else {
            totalAmount = user.stakedAmount;
        }

        userInPool[msg.sender][poolId].stakedAmount = 0;
        userInPool[msg.sender][poolId].stakedOnTimestamp = 0;
        userInPool[msg.sender][poolId].stakeEndTimestamp = 0;
        userInPool[msg.sender][poolId].lockupTimestamp = 0;
        userInPool[msg.sender][poolId].lastActionTimestamp = 0;

        pools[poolId].totalStaked -= (totalAmount - rewards);
        pools[poolId].totalWithdrawn += totalAmount;

        IERC20(quackToken).transfer(msg.sender, totalAmount);

        emit withdrewStake(msg.sender, poolId, totalAmount);
    }

    function forceWithdrawStake(uint256 poolId) external nonReentrant {
        require(poolId >= 0 && poolId <= 2, "Pool ID must be between 0 and 2.");

        User memory user = userInPool[msg.sender][poolId];

        require(user.stakedAmount > 0, "User has no staked amount in this pool.");

        uint256 secondsPassed = block.timestamp - user.lastActionTimestamp;

        uint256 rewards = calculateRewards(msg.sender, poolId, secondsPassed, user.stakedAmount);

        uint256 totalAmount;

        if(rewards > 0) {
            totalAmount = user.stakedAmount + rewards;
        } else {
            totalAmount = user.stakedAmount;
        }

        uint256 earlyWithdrawalPenalty = totalAmount * EARLY_WITHDRAWAL_PENALTY / 100;

        totalAmount -= earlyWithdrawalPenalty;

        withdrawalTaxes += earlyWithdrawalPenalty;

        userInPool[msg.sender][poolId].stakedAmount = 0;
        userInPool[msg.sender][poolId].stakedOnTimestamp = 0;
        userInPool[msg.sender][poolId].stakeEndTimestamp = 0;
        userInPool[msg.sender][poolId].lockupTimestamp = 0;
        userInPool[msg.sender][poolId].lastActionTimestamp = 0;

        pools[poolId].totalStaked -= (totalAmount - rewards) + earlyWithdrawalPenalty;
        pools[poolId].totalWithdrawn += totalAmount + earlyWithdrawalPenalty;

        IERC20(quackToken).transfer(msg.sender, totalAmount);

        emit forcedWithdrawStake(msg.sender, poolId, totalAmount);
    }

    function getTotalStakedPool(uint256 poolId) external view returns (uint256) {
        return pools[poolId].totalStaked;
    }

    function getTotalWithdrawnPool(uint256 poolId) external view returns (uint256) {
        return pools[poolId].totalWithdrawn;
    }

    function getPool(uint256 poolId) external view returns (Pool memory) {
        return pools[poolId];
    }

    function getUserInPool(address user, uint256 poolId) external view returns (User memory) {
        return userInPool[user][poolId];
    }

    function getPoolApy(uint256 poolId) external view returns (uint256) {
        return pools[poolId].apy;
    }

    function getPoolLockupPeriod(uint256 poolId) external view returns (uint256) {
        return pools[poolId].lockupPeriod;
    }

    function getPoolIsOpen(uint256 poolId) external view returns (bool) {
        return pools[poolId].isOpen;
    }

    function getLockupPeriodEnabled() external view returns (bool) {
        return lockupPeriodEnabled;
    }

    function getUserStakedAmount(address user, uint256 poolId) external view returns (uint256) {
        return userInPool[user][poolId].stakedAmount;
    }

    function getUserLockupEndTimestamp(address user, uint256 poolId) external view returns (uint256) {
        return userInPool[user][poolId].lockupTimestamp;
    }

    function getUserStakedOnTimestamp(address user, uint256 poolId) external view returns (uint256) {
        return userInPool[user][poolId].stakedOnTimestamp;
    }

    function getUserStakeEndTimestamp(address user, uint256 poolId) external view returns (uint256) {
        return userInPool[user][poolId].stakeEndTimestamp;
    }

    function getUserPendingRewards(address user, uint256 poolId) external view returns (uint256) {
        User memory _user = userInPool[user][poolId];

        uint256 secondsPassed = block.timestamp - _user.lastActionTimestamp;

        uint256 rewards = calculateRewards(user, poolId, secondsPassed, _user.stakedAmount);

        return rewards;
    }

    function openAllPools() external onlyOwner {
        pools[0].isOpen = true;
        pools[1].isOpen = true;
        pools[2].isOpen = true;
        emit openedAllPools();
    }

    function closeAllPools() external onlyOwner {
        pools[0].isOpen = false;
        pools[1].isOpen = false;
        pools[2].isOpen = false;
        emit closedAllPools();
    }

    function setApy(uint256 poolId, uint256 _apy) external onlyOwner {
        require(poolId >= 0 && poolId <= 2, "Pool ID must be between 0 and 2.");
        require(_apy >= 0, "APY must be greater than or equal to 0.");
        require(_apy <= 10000, "APY must be less than or equal to 100.");
        pools[poolId].apy = _apy;
        emit changedApy(poolId, _apy);
    }

    function setPoolOpen(uint256 poolId, bool _isOpen) external onlyOwner {
        require(poolId >= 0 && poolId <= 2, "Pool ID must be between 0 and 2.");
        pools[poolId].isOpen = _isOpen;
        emit switchedPoolOpen(poolId, _isOpen);
    }
        
    function setLockupPeriodEnabled(bool _lockupPeriodEnabled) external onlyOwner {
        lockupPeriodEnabled = _lockupPeriodEnabled;
        emit switchedLockupPeriodEnabled(_lockupPeriodEnabled);
    }

    function ownerWithdraw(uint256 amount) external onlyOwner {
        uint256 balance = IERC20(quackToken).balanceOf(address(this));
        uint256 withdrawable = balance - pools[0].totalStaked - pools[1].totalStaked - pools[2].totalStaked;
        require(amount <= withdrawable, "Amount must be less than or equal to withdrawable amount.");
        IERC20(quackToken).transfer(msg.sender, amount);
        emit ownerWithdrawed(amount);
    }

    function calculateRewards(address userAddr, uint256 poolId, uint256 secondsPassed, uint256 amount) private view returns (uint256) {
        require(poolId >= 0 && poolId <= 2, "Pool ID must be between 0 and 2.");

        Pool memory pool = pools[poolId];
        User memory user = userInPool[userAddr][poolId];

        if(block.timestamp >= user.stakeEndTimestamp && user.lastActionTimestamp < user.stakeEndTimestamp) {
            secondsPassed = user.stakeEndTimestamp - user.lastActionTimestamp;
        }

        uint256 apy = pool.apy;

        uint256 rewards = (secondsPassed * apy * amount) / (REWARD_PERIOD * DENOMINATOR);

        return rewards;
    }

    event switchedLockupPeriodEnabled(bool _lockupPeriodEnabled);
    event switchedPoolOpen(uint256 poolId, bool _isOpen);
    event changedApy(uint256 poolId, uint256 _apy);
    event ownerWithdrawed(uint256 amount);
    event openedAllPools();
    event closedAllPools();
    event newStake(address user, uint256 poolId, uint256 amount);
    event reStaked(address user, uint256 poolId, uint256 amount);
    event claimedRewards(address user, uint256 poolId, uint256 amount);
    event withdrewStake(address user, uint256 poolId, uint256 amount);
    event forcedWithdrawStake(address user, uint256 poolId, uint256 amount);

}