// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

// File: @openzeppelin/contracts/security/ReentrancyGuard.sol
// OpenZeppelin Contracts (last updated v4.9.0) (security/ReentrancyGuard.sol)

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
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _status will be _NOT_ENTERED
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Returns true if the reentrancy guard is currently set to "entered", which indicates there is a
     * `nonReentrant` function in the call stack.
     */
    function _reentrancyGuardEntered() internal view returns (bool) {
        return _status == _ENTERED;
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
// OpenZeppelin Contracts (last updated v4.9.0) (access/Ownable.sol)


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

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol
// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)


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


/// @title Staking smart contract with 4 pools
/// @notice each pool has different lockup period,
/// different apy's
contract ISOMAStaking is Ownable, ReentrancyGuard {

    /// @notice StakingPool struct
    struct StakingPool {
        uint256 maxCap;
        uint256 lockedPeriod;
        uint256 apy;
        uint256 totalRewardPercent;
        uint256 totalStaked;
    }

    /// @notice User struct
    struct User {
        uint256 stakedAmount;
        uint256 lastDepositTime;
        uint256 lastRewardClaim;
        uint256 rewardClaimed;
    }

    IERC20 public token;//token
    address public feeWallet; //wallet to receive fee
    uint256 public depositFeePercentage = 1; //1% deposit fee
    uint256 public withdrawalFeePercentage = 1; //1% withdraw fee
    uint256 public penaltyPercentage = 5; // penalty fee
    uint256 public totalRewards; //total available rewards


    StakingPool[] public pools;
    mapping(address => uint256) public stakingOption; // used to check active pool so user can stake in single pool at a time
    mapping(uint256 => mapping(address => User)) public users; //see user stats for particular pool
    mapping(uint256 => uint256) public walletCap; // pool wise max cap per wallet
    mapping(uint256 => uint256) public poolRewards; // set pool allocation

    ///events
    event Deposit(address indexed user, uint256 indexed poolId, uint256 amount);
    event Withdraw(address indexed user, uint256 indexed poolId, uint256 amount);
    event RewardClaimed(address indexed user, uint256 indexed poolId, uint256 amount);
    event EmergencyWithdraw(address indexed user, uint256 indexed poolId, uint256 amount);

    ///custom errors
    error AmountShouldBeGreaterThanZero();
    error InvalidPool();
    error ExceedPoolCap();
    error NothingStaked();
    error AmountExceedStakedAmount();
    error LockupPeriodNotPassed();
    error WalletCapExceeds();
    error AlreadyStakedInOtherPool();
    error ZeroAddress();
    error MaxFeeCap();
    error ApyRangeExceeds();
    error InvalidMaxCapPerWallet();
    error InvalidMaxPoolLimit();
    error PercentShouldBeAtleastFive();
    error CanNotClaimMainToken();


    constructor(address _token) {
        token = IERC20(_token);
        feeWallet = msg.sender;
        walletCap[0] = 1e9 * 1e9; //max cap per wallet for first pool
        walletCap[1] = 5e8 * 1e9; //max cap per wallet for second pool
        walletCap[2] = 3e7 * 1e9; // max cap per wallet for third pool
        walletCap[3] = 2e5 * 1e9; // max cap per wallet for forth pool
        // pools setup
        pools.push(StakingPool(1e9 * 1e9, 0, 5,10, 0));    // Pool 0: 1 billion maxCap, 0 locked period, 0.5% APY, 10% reward percentage allocation
        pools.push(StakingPool(2e9 * 1e9, 30 days, 10,20, 0));   // Pool 1: 2 billion maxCap, 30 days locked period, 1.0% APY, 20% reward percentage allocation
        pools.push(StakingPool(3e9 * 1e9, 90 days, 15,30, 0));  // Pool 2: 3 billion maxCap, 90 days locked period, 1.5% APY, 30% reward percentage allocation
        pools.push(StakingPool(2e9 * 1e9, 180 days, 20, 50, 0)); // pool 3; 2 billion maxCap, 180 days locked period, 2.0% APY, 50% reward percentage allocation


    }

    ///@dev inject reward to pool
    ///@param tokenAmount: number of tokens that owner want to inject to the pool
    ///Requirements-
    ///Amount must be greator than zero
    ///Owner must have approved tokenAmount before calling this function
    function injectReward (uint256 tokenAmount) external onlyOwner {
        if(tokenAmount <= 0) {revert AmountShouldBeGreaterThanZero();}
        token.transferFrom(msg.sender, (address(this)), tokenAmount);
        uint256 pool1 = (pools[0].totalRewardPercent * tokenAmount) / 100;
        uint256 pool2 = (pools[1].totalRewardPercent * tokenAmount) / 100;
        uint256 pool3 = (pools[2].totalRewardPercent * tokenAmount) / 100;
        uint256 pool4 = (pools[3].totalRewardPercent * tokenAmount) / 100;
        poolRewards[0] = poolRewards[0] + pool1;
        poolRewards[1] = poolRewards[1] + pool2;
        poolRewards[2] = poolRewards[2] + pool3;
        poolRewards[3] = poolRewards[3] + pool4;

        totalRewards = totalRewards + tokenAmount;

    }

    /// @notice user can deposit tokens to his choice of pool
    /// @param _poolId: pool id in which user want to stake
    /// @param _amountToStake: number of tokens user want to stake
    /// Requirements --
    /// number of tokens to be staked must be approved.
    /// pool id should be valid
    /// amount must greator than zero
    /// amount must be within pool cap
    /// amount must be userWalletCap for particular pool
    /// if staked in one pool already, can't switch to another pool untill he withdraw
    function deposit(uint256 _poolId, uint256 _amountToStake) external nonReentrant {

        if(_poolId > pools.length) {revert InvalidPool();}
        if(_amountToStake <= 0) { revert AmountShouldBeGreaterThanZero();}

        StakingPool storage pool = pools[_poolId];
        if(pool.totalStaked + _amountToStake > pool.maxCap) {  revert ExceedPoolCap();}

        User storage user = users[_poolId][msg.sender];
        if(user.stakedAmount + _amountToStake > walletCap[_poolId]) {revert WalletCapExceeds();}

        for (uint256 i = 0; i < pools.length; i++) {
            if (i != _poolId && users[i][msg.sender].stakedAmount > 0) {
                revert AlreadyStakedInOtherPool();
            }
        }

        if (user.stakedAmount == 0) {
            stakingOption[msg.sender] = _poolId;
        }
        _claimReward(_poolId, msg.sender);
        uint256 depositFee = (_amountToStake * depositFeePercentage) / 100;
        uint256 amountAfterFee = _amountToStake - depositFee;
        if(depositFee > 0){
            token.transferFrom(msg.sender, feeWallet, depositFee);
        }
        token.transferFrom(msg.sender, address(this), amountAfterFee);

        pool.totalStaked = pool.totalStaked + amountAfterFee;
        user.stakedAmount = user.stakedAmount + amountAfterFee;
        user.lastDepositTime = block.timestamp;
        user.lastRewardClaim = block.timestamp;


        emit Deposit(msg.sender, _poolId, amountAfterFee);

    }

    /// @notice user can withdraw his tokens
    /// @param _amountToWithdraw: number of tokens he want to withdraw
    /// Requirements--
    /// staked amount must be greater than zero
    /// input amount must be greater than zero
    /// lock period should have been passed
    function withdraw(uint256 _amountToWithdraw) external nonReentrant{

        uint256 poolId = stakingOption[msg.sender];
        StakingPool storage pool = pools[poolId];

        User storage user = users[poolId][msg.sender];

        if(user.stakedAmount == 0){revert NothingStaked();}
        if(_amountToWithdraw == 0){revert AmountShouldBeGreaterThanZero();}
        if(_amountToWithdraw > user.stakedAmount){revert AmountExceedStakedAmount();}
        if(block.timestamp < user.lastDepositTime + pool.lockedPeriod){revert LockupPeriodNotPassed();}

        uint256 withdrawalFee = (_amountToWithdraw * withdrawalFeePercentage) / 100;
        uint256 amountAfterFee = _amountToWithdraw - withdrawalFee;
        pool.totalStaked = pool.totalStaked - _amountToWithdraw;
        user.stakedAmount = user.stakedAmount - _amountToWithdraw;
        if(withdrawalFee > 0){
            token.transfer(feeWallet, withdrawalFee);
        }
        _claimReward(poolId, msg.sender);
        token.transfer(msg.sender, amountAfterFee);


        if (user.stakedAmount == 0) {
            stakingOption[msg.sender] = 0;
        }
        emit Withdraw(msg.sender, poolId, _amountToWithdraw);

    }

    /// @notice user claim pending earning
    /// @param _poolId: pool id
    function claimReward (uint256 _poolId) external nonReentrant {
        _claimReward(_poolId, msg.sender);
    }


    /// @notice this function can be used to withdraw tokens earlier than lock period
    /// there is  penalty on initial deposit for doing that.
    function emergencyWithdraw() external nonReentrant {
        uint256 poolId = stakingOption[msg.sender];

        StakingPool storage pool = pools[poolId];
        User storage user = users[poolId][msg.sender];

        if(user.stakedAmount == 0){ revert NothingStaked();}

        uint256 stakedAmount = user.stakedAmount;
        uint256 penalty = (stakedAmount * penaltyPercentage) / 100;
        uint256 amountAfterPenalty = stakedAmount - penalty;

        pool.totalStaked = pool.totalStaked - stakedAmount;
        user.stakedAmount = 0;
        user.lastRewardClaim = block.timestamp;
        stakingOption[msg.sender] = 0;
        token.transfer(feeWallet, penalty);
        token.transfer(msg.sender, amountAfterPenalty);
        emit EmergencyWithdraw(msg.sender, poolId, amountAfterPenalty);
    }

    /// @dev update pools apy
    /// @param _poolId: pool id to be updated
    /// @param _newAPY: new apy to be set
    /// Requirements-
    /// pool id must be valid, and new apy should be greator than 0.2% and less than 50%
    function updatePoolAPY (uint256 _poolId, uint256 _newAPY) external onlyOwner {
        StakingPool storage pool = pools[_poolId];
        if(_newAPY <= 2 && _newAPY > 500){ revert ApyRangeExceeds();}
        pool.apy = _newAPY;
    }

    /// @dev update reward allocation percentage per pool
    /// @param _poolId: pool id to allocated new percentage
    /// @param newPercentage: new percentage amount
    function updateRewardAllocationPercentage (uint256 _poolId, uint256 newPercentage) external onlyOwner {
        if(_poolId > pools.length) {revert InvalidPool();}
        if(newPercentage < 5) {revert PercentShouldBeAtleastFive();}
        StakingPool storage pool = pools[_poolId];
        pool.totalRewardPercent = newPercentage;
        uint256 rewards = (totalRewards * newPercentage) / 100;
        poolRewards[_poolId] = rewards;

    }


    /// @dev update fee wallet
    /// @param newFeeWallet: owner can update the new fee wallet
    function updateFeeWallet (address newFeeWallet) external onlyOwner {
        if(newFeeWallet == address(0)) {revert ZeroAddress();}
        feeWallet = newFeeWallet;
    }

    /// @dev update max cap per wallet per pool
    /// @param poolId: pool id to set new cap per wallet
    /// @param newCap: new cap limit amount
    function updateMaxCapPerWalletForPool (uint256 poolId, uint256 newCap) external onlyOwner {
        if(poolId > pools.length) {revert InvalidPool();}
        if(newCap <= 1e3 * 1e9){revert InvalidMaxCapPerWallet();}
        walletCap[poolId] = newCap;
    }

    /// @dev update Max pool limit (how many tokens total can be staked in particular pool)
    /// @param poolId: pool id to update max Cap
    /// @param newLimit: new cap till which user can stake in particular pool
    function updatePoolMaxLimit (uint256 poolId, uint256 newLimit) external onlyOwner {
        if(poolId > pools.length) {revert InvalidPool();}
        if(newLimit <= 1e5 * 1e9){revert InvalidMaxPoolLimit();}
        StakingPool storage pool = pools[poolId];
        pool.maxCap = newLimit;
    }

    /// @dev update Deposit and withdrawal fee (must be within 10 percent combined)
    /// @param newDepositFee: new deposit fees
    /// @param newWithdrawlFee: new withdrawl fee
    function updateDepositAndWithdrawFee (uint256 newDepositFee, uint256 newWithdrawlFee) external onlyOwner {
        if (newDepositFee + newWithdrawlFee > 10) {revert MaxFeeCap();}
        depositFeePercentage = newDepositFee;
        withdrawalFeePercentage = newWithdrawlFee;
    }

    /// @dev owner can claim other tokens from staking contract
    /// @param otherToken: token address to be rescued
    /// Requirements - Can't claim staking token
    function claimOtherERC20Tokens (address otherToken) external onlyOwner {
        if(otherToken == address(token)){
            revert CanNotClaimMainToken();
        }
        IERC20 tkn = IERC20(otherToken);
        uint256 balance = tkn.balanceOf(address(this));
        tkn.transfer(owner(), balance);
    }

    /// @notice internal function to handle user claim
    /// @param _poolId: pool id from which pending earning needs to be claimed
    /// @param _user: user wallet address
    /// Requirements:
    /// user staked amount must be greator than zero
    /// pool id must be valid
    function _claimReward(uint256 _poolId, address _user) private {
        if(_poolId > pools.length) {revert InvalidPool();}

        User storage user = users[_poolId][_user];

        uint256 rewards = calculateRewards(_poolId, _user);

        if (rewards > 0){

            uint256 availableRewards = poolRewards[_poolId];

            if(availableRewards < rewards){
                totalRewards = totalRewards - availableRewards;
                poolRewards[_poolId] = 0;
                user.rewardClaimed = user.rewardClaimed + availableRewards;
                user.lastRewardClaim = block.timestamp;
                token.transfer(_user, availableRewards);
            }
            else {
                totalRewards = totalRewards - rewards;
                poolRewards[_poolId] = poolRewards[_poolId] - rewards;
                user.rewardClaimed = user.rewardClaimed + rewards;
                user.lastRewardClaim = block.timestamp;
                token.transfer(_user, rewards);
            }
        }

        emit RewardClaimed(_user, _poolId, rewards);
    }

    /// @notice Returns the pending earning based on pool id and user address
    /// @param _poolId: pool id
    /// @param _user: user wallet address
    /// it calculate rewards based on fixed apy of 365 days
    function calculateRewards(uint256 _poolId, address _user) public view returns (uint256) {
        StakingPool storage pool = pools[_poolId];
        User storage user = users[_poolId][_user];

        uint256 lastRewardClaim = user.lastRewardClaim;
        uint256 stakedAmount = user.stakedAmount;
        uint256 elapsedTime = block.timestamp - lastRewardClaim;

        uint256 rewardAmount = (stakedAmount * pool.apy * elapsedTime) / (1000 * 365 days);

        return rewardAmount;
    }
}