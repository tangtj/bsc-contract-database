// SPDX-License-Identifier: MIT
// File: @openzeppelin/contracts/utils/Address.sol
// OpenZeppelin Contracts (last updated v4.7.0) (utils/Address.sol)

pragma solidity ^0.8.1;

library Address {
    
    function isContract(address account) internal view returns (bool) {

        return account.code.length > 0;
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

  
    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCall(target, data, "Address: low-level call failed");
    }

    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }

    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }


    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        require(isContract(target), "Address: call to non-contract");

        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        return functionStaticCall(target, data, "Address: low-level static call failed");
    }


    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        require(isContract(target), "Address: static call to non-contract");

        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    
    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionDelegateCall(target, data, "Address: low-level delegate call failed");
    }

    
    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(isContract(target), "Address: delegate call to non-contract");

        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }


    function verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal pure returns (bytes memory) {
        if (success) {
            return returndata;
        } else {
            // Look for revert reason and bubble it up if present
            if (returndata.length > 0) {
                // The easiest way to bubble the revert reason is using memory via assembly
                /// @solidity memory-safe-assembly
                assembly {
                    let returndata_size := mload(returndata)
                    revert(add(32, returndata), returndata_size)
                }
            } else {
                revert(errorMessage);
            }
        }
    }
}

// File: @openzeppelin/contracts/proxy/utils/Initializable.sol


// OpenZeppelin Contracts (last updated v4.7.0) (proxy/utils/Initializable.sol)

pragma solidity ^0.8.2;

abstract contract Initializable {
    /**
     * @dev Indicates that the contract has been initialized.
     * @custom:oz-retyped-from bool
     */
    uint8 private _initialized;

    /**
     * @dev Indicates that the contract is in the process of being initialized.
     */
    bool private _initializing;

    /**
     * @dev Triggered when the contract has been initialized or reinitialized.
     */
    event Initialized(uint8 version);

    
    modifier initializer() {
        bool isTopLevelCall = !_initializing;
        require(
            (isTopLevelCall && _initialized < 1) || (!Address.isContract(address(this)) && _initialized == 1),
            "Initializable: contract is already initialized"
        );
        _initialized = 1;
        if (isTopLevelCall) {
            _initializing = true;
        }
        _;
        if (isTopLevelCall) {
            _initializing = false;
            emit Initialized(1);
        }
    }

  
    modifier reinitializer(uint8 version) {
        require(!_initializing && _initialized < version, "Initializable: contract is already initialized");
        _initialized = version;
        _initializing = true;
        _;
        _initializing = false;
        emit Initialized(version);
    }

   
    modifier onlyInitializing() {
        require(_initializing, "Initializable: contract is not initializing");
        _;
    }

   
    function _disableInitializers() internal virtual {
        require(!_initializing, "Initializable: contract is initializing");
        if (_initialized < type(uint8).max) {
            _initialized = type(uint8).max;
            emit Initialized(type(uint8).max);
        }
    }
}

// File: @openzeppelin/contracts/security/ReentrancyGuard.sol


// OpenZeppelin Contracts v4.4.1 (security/ReentrancyGuard.sol)

pragma solidity ^0.8.0;

/**
 * @dev Contract module that helps prevent reentrant calls to a function.
 *
 * Inheriting from `ReentrancyGuard` will make the {nonReentrant} modifier
 * available, which can be applied to functions to make sure there are no nested
 * (reentrant) calls to them.
 *
 * Note that because there is a single `nonReentrant` guard, functions marked as
 * `nonReentrant` may not call one another. This can be worked around by making
 * those functions `private`, and then adding `external` `nonReentrant` entry
 * points to them.
 *
 * TIP: If you would like to learn more about reentrancy and alternative ways
 * to protect against it, check out our blog post
 * https://blog.openzeppelin.com/reentrancy-after-istanbul/[Reentrancy After Istanbul].
 */
abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}

// File: @openzeppelin/contracts/utils/Context.sol


// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)

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

// File: @openzeppelin/contracts/access/Ownable.sol


// OpenZeppelin Contracts (last updated v4.7.0) (access/Ownable.sol)

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
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
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

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.6.0) (token/ERC20/IERC20.sol)

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
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

// File: TokenStaking.sol
pragma solidity ^0.8.0;
contract TokenStaking is Ownable, ReentrancyGuard, Initializable {
    // Struct to store the User's Details
    struct User {
        uint256 stakeAmount; // Stake Amount
        uint256 rewardAmount; // Reward Amount
        uint256 lastStakeTime; // Last Stake Timestamp
        uint256 lastRewardCalculationTime; // Last Reward Calculation Timestamp
        uint256 rewardsClaimedSoFar; // Sum of rewards claimed so far
        uint256 stakeStartTime; // Stake Start Time
    }

    uint256 _minimumStakingAmount; // minimum staking amount

    uint256 _maxStakeTokenLimit; // maximum staking token limit for program

    uint256 _stakeEndDate; // end date for program

    uint256 _stakeStartDate; // end date for program

    uint256 _totalStakedTokens; // Total no of tokens that are staked

    uint256 _totalUsers; // Total no of users

    uint256 _stakeDays; // staking days

    uint256 _earlyUnstakeFeePercentage; // early unstake fee percentage

    bool _isStakingPaused; // staking status

    // Token contract address
    address private _tokenAddress;

    // APY
    uint256 _apyRate;

    uint256 public constant PERCENTAGE_DENOMINATOR = 10000;
    uint256 public constant APY_RATE_CHANGE_THRESHOLD = 10;

    // User address => User
    mapping(address => User) private _users;
    mapping(address => bool) public isAllowedStaker;
    mapping(address => uint256) public maxStakeLimit;


    event StakerAdded(address indexed staker, uint256 maxStake);
    event StakerRemoved(address indexed staker);
    event Stake(address indexed user, uint256 amount);
    event UnStake(address indexed user, uint256 amount);
    event EarlyUnStakeFee(address indexed user, uint256 amount);
    event ClaimReward(address indexed user, uint256 amount);
    event StakerLimitUpdated(address indexed staker, uint256 newMaxStake);

    modifier whenTreasuryHasBalance(uint256 amount) {
        require(
            IERC20(_tokenAddress).balanceOf(address(this)) >= amount,
            "TokenStaking: insufficient funds in the treasury"
        );
        _;
    }

    function initialize(
        address owner_,
        address tokenAddress_,
        uint256 apyRate_,
        uint256 minimumStakingAmount_,
        uint256 maxStakeTokenLimit_,
        uint256 stakeStartDate_,
        uint256 stakeEndDate_,
        uint256 stakeDays_,
        uint256 earlyUnstakeFeePercentage_
    ) public virtual initializer {
        __TokenStaking_init_unchained(
            owner_,
            tokenAddress_,
            apyRate_,
            minimumStakingAmount_,
            maxStakeTokenLimit_,
            stakeStartDate_,
            stakeEndDate_,
            stakeDays_,
            earlyUnstakeFeePercentage_
        );
    }

    function __TokenStaking_init_unchained(
        address owner_,
        address tokenAddress_,
        uint256 apyRate_,
        uint256 minimumStakingAmount_,
        uint256 maxStakeTokenLimit_,
        uint256 stakeStartDate_,
        uint256 stakeEndDate_,
        uint256 stakeDays_,
        uint256 earlyUnstakeFeePercentage_
    ) internal onlyInitializing {
        require(_apyRate <= 10000, "TokenStaking: apy rate should be less than 10000");
        require(stakeDays_ > 0, "TokenStaking: stake days must be non-zero");
        require(tokenAddress_ != address(0), "TokenStaking: token address cannot be 0 address");
        require(stakeStartDate_ < stakeEndDate_, "TokenStaking: start date must be less than end date");

        _transferOwnership(owner_);
        _tokenAddress = tokenAddress_;
        _apyRate = apyRate_;
        _minimumStakingAmount = minimumStakingAmount_;
        _maxStakeTokenLimit = maxStakeTokenLimit_;
        _stakeStartDate = stakeStartDate_;
        _stakeEndDate = stakeEndDate_;
        _stakeDays = stakeDays_ * 1 days;
        _earlyUnstakeFeePercentage = earlyUnstakeFeePercentage_;
    }

   function addStaker(address staker, uint256 maxStake) external onlyOwner {
    require(staker != address(0), "TokenStaking: Invalid staker address");
    require(maxStake > 0, "TokenStaking: Invalid stake limit");

    isAllowedStaker[staker] = true;
    maxStakeLimit[staker] += maxStake * 10**18;
    _stakeDays = 120 days; // Set the stake period to 120 days
    if(_users[staker].stakeStartTime == 0) { // Eğer daha önce stake yapılmamışsa
        _users[staker].stakeStartTime = block.timestamp; // Stake başlangıç zamanını belirle
    }
    
    emit StakerAdded(staker, maxStake);
}




    function updateStakerLimit(address staker, uint256 newMaxStake) external onlyOwner {
    require(staker != address(0), "TokenStaking: Invalid staker address");
    require(isAllowedStaker[staker], "TokenStaking: Address is not a staker");
    
    maxStakeLimit[staker] = newMaxStake * 10**18; // assuming token has 18 decimals

    emit StakerLimitUpdated(staker, newMaxStake);
}

    function resetStaker(address staker) external onlyOwner {
    require(staker != address(0), "TokenStaking: Invalid staker address");
    require(isAllowedStaker[staker], "TokenStaking: Address is not a staker");

    User storage user = _users[staker];
    user.stakeAmount = 0;
    user.rewardAmount = 0;
    user.lastStakeTime = 0;
    user.lastRewardCalculationTime = 0;
    user.rewardsClaimedSoFar = 0;

    emit StakerRemoved(staker);
}
     


     function removeStaker(address staker) external onlyOwner {
        require(staker != address(0), "TokenStaking: Invalid staker address");
        require(isAllowedStaker[staker], "TokenStaking: Address is not a staker");

        isAllowedStaker[staker] = false;

        emit StakerRemoved(staker);
    }
    
    function getMinimumStakingAmount() external view returns (uint256) {
        return _minimumStakingAmount / 10**18;
    }

    
    function getMaxStakingTokenLimit() external view returns (uint256) {
        return _maxStakeTokenLimit / 10**18;
    }
    
    function getMaxStakeLimitOfUser(address staker) external view returns (uint256) {
    require(staker != address(0), "TokenStaking: Invalid staker address");
    require(isAllowedStaker[staker], "TokenStaking: Address is not a staker");

    // Return the value divided by 10^18 to get the actual limit
    return maxStakeLimit[staker] / 10**18;
}

    
    function getStakeStartDate() external view returns (uint256) {
        return _stakeStartDate;
    }

    
    function getStakeEndDate() external view returns (uint256) {
        return _stakeEndDate;
    }
    
    function getRemainingStakeAmount(address staker) external view returns (uint256) {
    require(staker != address(0), "TokenStaking: Invalid staker address");
    require(isAllowedStaker[staker], "TokenStaking: Address is not a staker");

    uint256 stakedAmount = _users[staker].stakeAmount / 10**18;
    uint256 stakerLimit = maxStakeLimit[staker] / 10**18;

    if (stakedAmount >= stakerLimit) {
        return 0;
    } else {
        return stakerLimit - stakedAmount;
    }
}


    function getRemainingStakeDays(address user) external view returns (uint256) {
    uint256 stakeDuration = block.timestamp - _users[user].stakeStartTime;
    if (stakeDuration >= _stakeDays) {
        return 0;
    } else {
        uint256 remainingSeconds = _stakeDays - stakeDuration; 
        return remainingSeconds / 1 days;
    }
}


    function isStakePeriodOver(address user) private view returns (bool) {
    return block.timestamp >= _users[user].stakeStartTime + _stakeDays;
}



    function getTotalStakedTokens() external view returns (uint256) {
        return _totalStakedTokens;
    }

    
    function getTotalUsers() external view returns (uint256) {
        return _totalUsers;
    }

    
    function getStakeDays() external view returns (uint256) {
        return _stakeDays;
    }

    
    function getEarlyUnstakeFeePercentage() external view returns (uint256) {
        return _earlyUnstakeFeePercentage;
    }

   
    function getStakingStatus() external view returns (bool) {
        return _isStakingPaused;
    }

    
    function getAPY() external view returns (uint256) {
        return _apyRate;
    }

    
    function getUserEstimatedRewards() external view returns (uint256) {
        (uint256 amount, ) = _getUserEstimatedRewards(msg.sender);
        return _users[msg.sender].rewardAmount + amount;
    }

    
    function getWithdrawableAmount() external view returns (uint256) {
        return IERC20(_tokenAddress).balanceOf(address(this)) - _totalStakedTokens;
    }

    
    function getUser(address userAddress) external view returns (User memory) {
        return _users[userAddress];
    }

   
    function isStakeHolder(address _user) external view returns (bool) {
        return _users[_user].stakeAmount != 0;
    }

   
    function updateMinimumStakingAmount(uint256 newAmount) external onlyOwner {
        _minimumStakingAmount = newAmount;
    }

    function updateMaximumStakingAmount(uint256 newAmount) external onlyOwner {
        _maxStakeTokenLimit = newAmount;
    }

    function updateStakingEndDate(uint256 newDate) external onlyOwner {
        _stakeEndDate = newDate;
    }


    function updateEarlyUnstakeFeePercentage(uint256 newPercentage) external onlyOwner {
        _earlyUnstakeFeePercentage = newPercentage;
    }

    function updateAPY(uint256 newApyRate) external onlyOwner {
    require(newApyRate <= 1000000, "TokenStaking: APY rate should be less than or equal to 1000000");
    _apyRate = newApyRate;
    }


    function stakeForUser(uint256 amount, address user) external onlyOwner nonReentrant {
        _stakeTokens(amount, user);
    }


    function toggleStakingStatus() external onlyOwner {
        _isStakingPaused = !_isStakingPaused;
    }

  
    function withdraw(uint256 amount) external onlyOwner nonReentrant {
        require(this.getWithdrawableAmount() >= amount, "TokenStaking: not enough withdrawable tokens");
        IERC20(_tokenAddress).transfer(msg.sender, amount);
    }


        function stake(uint256 _amount) external nonReentrant {
    require(isAllowedStaker[msg.sender], "TokenStaking: You are not allowed to stake tokens");
    require(_amount <= maxStakeLimit[msg.sender], "TokenStaking: Amount exceeds your maximum stake limit");

    // Check if staking period is over
    if(_users[msg.sender].stakeStartTime > 0 && getCurrentTime() > _users[msg.sender].stakeStartTime + _stakeDays) {
        revert("Your farming time has expired. Please first collect your rewards from the 'Farming Harvest' button and then remove all your tokens from the system with the 'Withdraw' button.");
    }

    _stakeTokens(_amount, msg.sender);
}


    function _stakeTokens(uint256 _amount, address user_) private {
        require(!_isStakingPaused, "TokenStaking: staking is paused");

        uint256 currentTime = getCurrentTime();
         uint256 remainingStakeTime = _users[user_].stakeStartTime + _stakeDays - currentTime;
        require(currentTime > _stakeStartDate, "TokenStaking: staking not started yet");
        require(currentTime < _stakeEndDate, "TokenStaking: staking ended");
        require(remainingStakeTime <= 120 days, "TokenStaking: Staking period cannot exceed 120 days");
        require(_totalStakedTokens + _amount <= _maxStakeTokenLimit, "TokenStaking: max staking token limit reached");
        require(_amount > 0, "TokenStaking: stake amount must be non-zero");
        require(
            _amount >= _minimumStakingAmount,
            "TokenStaking: stake amount must greater than minimum amount allowed"
        );

        if (_users[user_].stakeAmount != 0) {
            _calculateRewards(user_);
        } else {
            _users[user_].lastRewardCalculationTime = currentTime;
            _totalUsers += 1;
        }
         if (_totalStakedTokens + _amount > _maxStakeTokenLimit) {
        revert("TokenStaking: max staking token limit exceeded");
        }
        
        _users[user_].stakeAmount += _amount;
        _users[user_].lastStakeTime = currentTime;
        _users[user_].stakeStartTime = block.timestamp; // Save the stake start time

        _totalStakedTokens += _amount;

        require(
            IERC20(_tokenAddress).transferFrom(msg.sender, address(this), _amount),
            "TokenStaking: failed to transfer tokens"
        );
        emit Stake(user_, _amount);
    }
    
    function checkStakingPeriod(address userAddress) public view returns (bool) {
    User storage user = _users[userAddress];
    uint256 stakeEndTime = user.stakeStartTime + _stakeDays;
    if (block.timestamp > stakeEndTime) {
        return true;
    } else {
        return false;
    }
}

   
  function unstake(uint256 _amount) external nonReentrant {
    address user = msg.sender;
    require(_amount != 0, "TokenStaking: amount should be non-zero");
    require(this.isStakeHolder(user), "TokenStaking: not a stakeholder");

    uint256 currentTime = getCurrentTime();
    bool stakePeriodOver = currentTime > _users[user].stakeStartTime + _stakeDays;

    // Check if the staking period is over
    if (stakePeriodOver) {
        require(_users[user].stakeAmount >= _amount, "TokenStaking: not enough stake to unstake");
        // Calculate User's rewards until now
        _calculateRewards(user);

        if (_users[user].rewardAmount > 0) {
            require(IERC20(_tokenAddress).transfer(user, _users[user].rewardAmount), "TokenStaking: failed to transfer reward");
            emit ClaimReward(user, _users[user].rewardAmount);
            _users[user].rewardAmount = 0;
        }
    } else {
        // Calculate early unstake fee
        uint256 earlyUnstakeFee = (_amount * _earlyUnstakeFeePercentage) / PERCENTAGE_DENOMINATOR;
        require(_users[user].stakeAmount >= _amount, "TokenStaking: not enough stake to unstake");
        _amount -= earlyUnstakeFee;  // Decrease the unstaking amount by the early unstake fee
        emit EarlyUnStakeFee(user, earlyUnstakeFee);
    }

    // Update the staker's details after unstaking
    _users[user].stakeAmount -= _amount;
    _totalStakedTokens -= _amount;
    IERC20(_tokenAddress).transfer(user, _amount);
    emit UnStake(user, _amount);

    // If the staking period is over, reset staker's data
    if (stakePeriodOver) {
        _users[user].stakeAmount = 0;
        _users[user].rewardAmount = 0;
        _users[user].lastStakeTime = 0;
        _users[user].lastRewardCalculationTime = 0;
        _users[user].rewardsClaimedSoFar = 0;
        _users[user].stakeStartTime = 0;

        _totalUsers -= 1;
    }
}





   function claimReward() external nonReentrant {
    // Şu anda ne zaman olduğunu belirlemek için
    uint256 currentTime = block.timestamp;

    _calculateRewards(msg.sender);

    // Bu kullanıcının ödüllerini saklamak için
    uint256 rewardAmount = _users[msg.sender].rewardAmount;

    // Süre kontrolü, _stakeDays süresi dolduğunda yalnızca ödül çekebilir
    require(currentTime - _users[msg.sender].stakeStartTime <= _stakeDays * 1 days || _users[msg.sender].rewardAmount == 0, "TokenStaking: Stake period has ended");

    require(rewardAmount > 0, "TokenStaking: No Reward to Claim");

    require(IERC20(_tokenAddress).transfer(msg.sender, rewardAmount), "TokenStaking: failed to transfer");

    _users[msg.sender].rewardAmount = 0;
    _users[msg.sender].rewardsClaimedSoFar += rewardAmount;

    emit ClaimReward(msg.sender, rewardAmount);
}



   function _calculateRewards(address _userAddress) private {
    User storage user = _users[_userAddress];
    uint256 currentTime = getCurrentTime();
    uint256 timeDiff = currentTime - user.lastRewardCalculationTime;
    uint256 stakeDuration = currentTime - user.stakeStartTime;

  
    if (stakeDuration > _stakeDays) {
        
        return;
    }

    uint256 reward = (user.stakeAmount * _apyRate * timeDiff) / (365 days * PERCENTAGE_DENOMINATOR);

    user.rewardAmount += reward;
    user.lastRewardCalculationTime = currentTime;
}



    function _getUserEstimatedRewards(address _user) private view returns (uint256, uint256) {
    uint256 userReward;
    uint256 userTimestamp = _users[_user].lastRewardCalculationTime;

    uint256 currentTime = getCurrentTime();
    uint256 stakeEndTime = _users[_user].stakeStartTime + _stakeDays;
    if (currentTime > stakeEndTime) {
        currentTime = stakeEndTime;
    }

    uint256 totalStakedTime = currentTime - userTimestamp;

    userReward += ((totalStakedTime * _users[_user].stakeAmount * _apyRate) / 365 days) / PERCENTAGE_DENOMINATOR;

    return (userReward, currentTime);
}


    function getCurrentTime() internal view virtual returns (uint256) {
        return block.timestamp;
    }
}