// File: @openzeppelin/contracts/security/ReentrancyGuard.sol


// OpenZeppelin Contracts (last updated v4.9.0) (security/ReentrancyGuard.sol)

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

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

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

// File: contracts/RapidStake.sol


pragma solidity ^0.8.0;



contract StakingContract is ReentrancyGuard {
    IERC20 public stakingToken;

    enum StakeType { ThreeMonths, SixMonths, TwelveMonths }
    uint256[] public interestRates = [2, 5, 12]; // Interest rates in percentages
    address private owner;

    struct Stake {
        address staker;
        uint256 amount;
        StakeType stakeType;
        uint256 startTime;
    }

    Stake[] public stakes;
    mapping(address => uint256) public stakerAmounts; // Tracks total stake amount for each staker
    mapping(address => bool) public stakerExists; // Tracks if address has staked before
    mapping(address => uint256[]) public stakerToStakes; // Mapping of staker address to stake IDs

    uint256 public totalStakers; // Total unique stakers

    constructor(address _stakingToken) {
        stakingToken = IERC20(_stakingToken);
        owner = msg.sender;
    }

    // Function to stake tokens with a specific stake type
    function stakeTokens(uint256 _amount, StakeType _stakeType) external nonReentrant {
        require(_amount > 0, "Amount must be greater than zero");
        require(stakingToken.transferFrom(msg.sender, address(this), _amount), "Transfer failed");

        if (!stakerExists[msg.sender]) {
            stakerExists[msg.sender] = true;
            totalStakers++;
        }
        stakerAmounts[msg.sender] += _amount;

        Stake memory newStake = Stake(msg.sender, _amount, _stakeType, block.timestamp);
        uint256 stakeID = stakes.length;
        stakes.push(newStake);
        stakerToStakes[msg.sender].push(stakeID); // Link stake ID to the staker
    }

    // Function to unstake tokens and claim reward
    function unstakeTokens(uint256 _stakeID) external nonReentrant {
        Stake storage userStake = stakes[_stakeID];
        require(userStake.staker == msg.sender, "Only the stake owner can withdraw");
        require(stakeUnlockTime(_stakeID) <= block.timestamp, "Stake cannot be withdrawn yet");

        uint256 reward = calculateReward(_stakeID);
        uint256 totalAmount = userStake.amount + reward;
        userStake.amount = 0;
        stakerAmounts[msg.sender] -= totalAmount;

        require(stakingToken.transfer(msg.sender, totalAmount), "Transfer failed");
    }

    // Function to calculate reward for a specific stake
    function calculateReward(uint256 _stakeID) public view returns (uint256) {
        Stake storage userStake = stakes[_stakeID];
        uint256 rate = interestRates[uint256(userStake.stakeType)];
        return (userStake.amount * rate) / 100;
    }

    // Function to get unlock time for a specific stake
    function stakeUnlockTime(uint256 _stakeID) public view returns (uint256) {
        Stake storage userStake = stakes[_stakeID];
        if (userStake.stakeType == StakeType.ThreeMonths) return userStake.startTime + 90 days;
        if (userStake.stakeType == StakeType.SixMonths) return userStake.startTime + 180 days;
        if (userStake.stakeType == StakeType.TwelveMonths) return userStake.startTime + 365 days;
        revert("Invalid stake type");
    }

    // Function to get all stakes for a user
    function getUserStakes(address _user) external view returns (Stake[] memory) {
        uint256[] storage stakeIDs = stakerToStakes[_user];
        Stake[] memory userStakes = new Stake[](stakeIDs.length);
        for (uint256 i = 0; i < stakeIDs.length; i++) {
            userStakes[i] = stakes[stakeIDs[i]];
        }
        return userStakes;
    }

    // Function to get the total number of staking users
    function getTotalStakers() external view returns (uint256) {
        return totalStakers;
    }

    // Function to get the total amount of staked tokens
    function getTotalStakedTokens() external view returns (uint256) {
        return stakingToken.balanceOf(address(this));
    }

    function withdrawTokens(IERC20 token) external {
        require(msg.sender == owner, "Caller is not the owner");
        uint256 balance = token.balanceOf(address(this));
        require(token.transfer(msg.sender, balance), "Transfer failed");
    }
}