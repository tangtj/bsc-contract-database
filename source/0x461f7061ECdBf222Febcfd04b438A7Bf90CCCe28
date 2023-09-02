// SPDX-License-Identifier: MIT
// Staking for BITSBAI Silver Tier
// Website: https://bitsbai.xyz/
// Other Social just visit our website to check.

pragma solidity ^0.8.0;

/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

pragma solidity ^0.8.0;

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner account will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

pragma solidity ^0.8.0;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

pragma solidity ^0.8.0;

contract Staking is Ownable {
    IERC20 public token;
    IERC20 public rewardToken;
    uint256 public apr;
    uint256 public duration;
    uint256 public minStake;
    uint256 public maxStake;
    uint256 public earlyUnstakePenalty;
    string public stakeName;

    address public deadWallet = 0x000000000000000000000000000000000000dEaD;
    uint256 public totalRewardsPaid;
    uint256 public totalTokensBurnt;

    address[] public stakers;

    struct Stake {
        uint256 amount;
        uint256 startTime;
        bool claimed;
    }

    mapping(address => Stake) public stakes;

    struct HistoricalStake {
        address staker;
        uint256 stakeAmount;
        uint256 rewardAmount;
    }

    HistoricalStake[] public historicalStakes;

    event Staked(address indexed user, uint256 amount);
    event Withdrawn(address indexed user, uint256 amount);
    event RewardsClaimed(address indexed user, uint256 amount);

    constructor() {
        token = IERC20(0xF1CD113CC6E01E336DEE56658592Dde766a33e81);
        rewardToken = IERC20(0xF1CD113CC6E01E336DEE56658592Dde766a33e81);
        apr = 15 * 10**18;
        duration = 45 days;
        minStake = 10000 * 10**18;
        maxStake = 500000 * 10**18;
        earlyUnstakePenalty = 2 * 10**18;
        stakeName = "Silver Tier 15% 45 Days";
    }

    function setToken(IERC20 _token) external onlyOwner {
        token = _token;
    }

    function setRewardToken(IERC20 _rewardToken) external onlyOwner {
        rewardToken = _rewardToken;
    }

	function setApr(uint256 _apr) external onlyOwner {
		require(_apr >= 5 * 10**18 && _apr <= 50 * 10**18, "APR must be >= 5% and <= 50% in WEI units");
		apr = _apr;
	}

	function setDuration(uint256 _duration) external onlyOwner {
		require(_duration >= 1 && _duration <= 365, "Duration must be >= 1 day and <= 365 days");
		duration = _duration * 1 days;
	}

    function setMinStake(uint256 _minStake) external onlyOwner {
        minStake = _minStake;
    }

    function setMaxStake(uint256 _maxStake) external onlyOwner {
        maxStake = _maxStake;
    }

    function setEarlyUnstakePenalty(uint256 _earlyUnstakePenalty) external onlyOwner {
        earlyUnstakePenalty = _earlyUnstakePenalty;
    }
	
    function claimRewards() internal {
    Stake storage stakeInfo = stakes[msg.sender];
    require(stakeInfo.amount > 0, "No stake found");
    require(!stakeInfo.claimed, "Rewards already claimed");

    uint256 timeElapsed = block.timestamp - stakeInfo.startTime;
    if (timeElapsed > duration) timeElapsed = duration;

    uint256 rewardAmount = (stakeInfo.amount * apr * timeElapsed) / (100 * 365 days * (10 ** 18));
    require(rewardToken.transfer(msg.sender, rewardAmount), "Transfer failed");
    stakeInfo.claimed = true;
    totalRewardsPaid += rewardAmount;

    emit RewardsClaimed(msg.sender, rewardAmount);
    }

	function stake(uint256 _amount) external {
		require(_amount >= minStake, "Amount is less than minimum stake");
		require(stakes[msg.sender].amount + _amount <= maxStake, "Total stake amount is greater than maximum stake");
		require(getRemainingRewards() >= (_amount * apr * duration) / (100 * 365 days * (10 ** 18)), "Insufficient rewards left in pool");
		require(token.transferFrom(msg.sender, address(this), _amount), "Transfer failed");

		Stake storage stakeInfo = stakes[msg.sender];
		if (stakeInfo.amount > 0) {
			// User already has a stake
			claimRewards(); // Call claimRewards function to claim any unclaimed rewards
			stakeInfo.amount += _amount; // Add new stake amount to existing stake amount
			stakeInfo.startTime = block.timestamp; // Reset timestamp
			stakeInfo.claimed = false; // Reset claimed status
		} else {
			// User does not have a stake yet
			stakes[msg.sender] = Stake(_amount, block.timestamp, false);
			stakers.push(msg.sender); // Add user's address to stakers array
		}

		emit Staked(msg.sender, _amount);
	}

	function withdraw() external {
	
    Stake storage stakeInfo = stakes[msg.sender];
    require(stakeInfo.amount > 0, "No stake found");

    if (block.timestamp < stakeInfo.startTime + duration) {
        // Early unstaking
        uint256 penaltyAmount = (stakeInfo.amount * earlyUnstakePenalty) / (100 * 10**18);
        require(token.transfer(deadWallet, penaltyAmount), "Transfer failed");
        require(token.transfer(msg.sender, stakeInfo.amount - penaltyAmount), "Transfer failed");
        totalTokensBurnt += penaltyAmount;
    } else {
        // On-time unstaking

        if (!stakeInfo.claimed) {
            // Release rewards together with the stake amount
            uint256 rewardAmount = (stakeInfo.amount * apr * duration) / (100 * 365 days * (10 ** 18));
            require(rewardToken.transfer(msg.sender, stakeInfo.amount + rewardAmount), "Transfer failed");
            totalRewardsPaid += rewardAmount;

            emit RewardsClaimed(msg.sender, rewardAmount);

            // Add information about the completed stake and rewards to the historicalStakes array
            historicalStakes.push(HistoricalStake(msg.sender, stakeInfo.amount, rewardAmount));
        }
    }

    delete stakes[msg.sender];

    // Remove all occurrences of the address from the stakers array
    for (uint i = 0; i < stakers.length; i++) {
        if (stakers[i] == msg.sender) {
            stakers[i] = stakers[stakers.length - 1];
            stakers.pop();
        }
    }

		emit Withdrawn(msg.sender, stakeInfo.amount);
	}

     function getTotalBurnt() external view returns (uint256) {
         return totalTokensBurnt;
     }

     function getStakeByAddress(address _address) external view returns (uint256) {
        return stakes[_address].amount;
    }

	function getTotalValueLocked() public view returns (uint256) {
		uint256 totalStaked = 0;
		for (uint i = 0; i < stakers.length; i++) {
			address staker = stakers[i];
			totalStaked += stakes[staker].amount;
		}
		return totalStaked;
	}


	function getRemainingRewardsInPool() public view returns (uint256) {
		return rewardToken.balanceOf(address(this)) - getTotalValueLocked();
	}


    function getRemainingRewards() public view returns (uint256) {
        uint256 totalStaked = 0;
        for (uint i = 0; i < stakers.length; i++) {
            address staker = stakers[i];
            totalStaked += stakes[staker].amount;
        }
        uint256 totalRewards = (totalStaked * apr * duration) / (100 * 365 days * (10 ** 18));
        return getRemainingRewardsInPool() - totalRewards;
    }

    function getContractParameters() external view returns (uint256, uint256, uint256, uint256, uint256) {
        return (apr, duration, minStake, maxStake, earlyUnstakePenalty);
    }

    function getLeaderboard() external view returns (address[10] memory, uint256[10] memory, uint256[10] memory) {
        address[10] memory topStakers;
        uint256[10] memory topStakes;
        uint256[10] memory topRewards;

        for (uint i = 0; i < stakers.length; i++) {
            address staker = stakers[i];
            uint256 stakeAmount = stakes[staker].amount;
            uint256 rewardAmount = (stakeAmount * apr * duration) / (100 * 365 days * (10 ** 18));

            for (uint j = 0; j < 10; j++) {
                if (stakeAmount > topStakes[j]) {
                    for (uint k = 9; k > j; k--) {
                        topStakers[k] = topStakers[k - 1];
                        topStakes[k] = topStakes[k - 1];
                        topRewards[k] = topRewards[k - 1];
                    }
                    topStakers[j] = staker;
                    topStakes[j] = stakeAmount;
                    topRewards[j] = rewardAmount;
                    break;
                }
            }
        }

        return (topStakers, topStakes, topRewards);
    }

    function getHistoricalLeaderboard() external view returns (address[10] memory, uint256[10] memory, uint256[10] memory) {
        address[10] memory topStakers;
        uint256[10] memory topStakes;
        uint256[10] memory topRewards;

    for (uint i = 0; i < historicalStakes.length; i++) {
        HistoricalStake storage historicalStake = historicalStakes[i];
        address staker = historicalStake.staker;
        uint256 stakeAmount = historicalStake.stakeAmount;
        uint256 rewardAmount = historicalStake.rewardAmount;

        for (uint j = 0; j < 10; j++) {
            if (stakeAmount > topStakes[j]) {
                for (uint k = 9; k > j; k--) {
                    topStakers[k] = topStakers[k - 1];
                    topStakes[k] = topStakes[k - 1];
                    topRewards[k] = topRewards[k - 1];
                }
                topStakers[j] = staker;
                topStakes[j] = stakeAmount;
                topRewards[j] = rewardAmount;
                break;
            }
        }
    }

    return (topStakers, topStakes, topRewards);
    }
	
	function getRunningRewards(address _wallet) external view returns (uint256) {
    Stake storage stakeInfo = stakes[_wallet];
    if (stakeInfo.amount == 0) {
        return 0;
    }

    uint256 timeElapsed = block.timestamp - stakeInfo.startTime;
    if (timeElapsed > duration) timeElapsed = duration;

    uint256 rewardAmount = (stakeInfo.amount * apr * timeElapsed) / (100 * 365 days * (10 ** 18));
    return rewardAmount;
	}
	
	function getStartStakingDate(address _staker) public view returns (uint256) {
    return stakes[_staker].startTime;
	}

	function getMaturityDate(address _staker) public view returns (uint256) {
		uint256 startTime = stakes[_staker].startTime;
		uint256 maturityDate = startTime + duration;
		return maturityDate;
	}

	function getTotalStakers() public view returns (uint256) {
		return stakers.length;
	}
	
	function hasMatured() public view returns (bool) {
		Stake storage stakeInfo = stakes[msg.sender];
		if (stakeInfo.amount == 0) return false;
		return block.timestamp >= stakeInfo.startTime + duration;
	}

}