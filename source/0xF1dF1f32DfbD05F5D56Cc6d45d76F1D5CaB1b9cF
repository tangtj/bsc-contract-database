// File: openzeppelin-solidity/contracts/math/Math.sol

pragma solidity ^0.5.0;

/**
 * @dev Standard math utilities missing in the Solidity language.
 */
library Math {
    /**
     * @dev Returns the largest of two numbers.
     */
    function max(uint256 a, uint256 b) internal pure returns (uint256) {
        return a >= b ? a : b;
    }

    /**
     * @dev Returns the smallest of two numbers.
     */
    function min(uint256 a, uint256 b) internal pure returns (uint256) {
        return a < b ? a : b;
    }

    /**
     * @dev Returns the average of two numbers. The result is rounded towards
     * zero.
     */
    function average(uint256 a, uint256 b) internal pure returns (uint256) {
        // (a + b) / 2 can overflow, so we distribute
        return (a / 2) + (b / 2) + ((a % 2 + b % 2) / 2);
    }
}

// File: openzeppelin-solidity/contracts/math/SafeMath.sol

pragma solidity ^0.5.0;

/**
 * @dev Wrappers over Solidity's arithmetic operations with added overflow
 * checks.
 *
 * Arithmetic operations in Solidity wrap on overflow. This can easily result
 * in bugs, because programmers usually assume that an overflow raises an
 * error, which is the standard behavior in high level programming languages.
 * `SafeMath` restores this intuition by reverting the transaction when an
 * operation overflows.
 *
 * Using this library instead of the unchecked operations eliminates an entire
 * class of bugs, so it's recommended to use it always.
 */
library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     * - Subtraction cannot overflow.
     *
     * _Available since v2.4.0._
     */
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    /**
     * @dev Returns the integer division of two unsigned integers. Reverts on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    /**
     * @dev Returns the integer division of two unsigned integers. Reverts with custom message on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     *
     * _Available since v2.4.0._
     */
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts with custom message when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     *
     * _Available since v2.4.0._
     */
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

// File: openzeppelin-solidity/contracts/token/ERC20/IERC20.sol

pragma solidity ^0.5.0;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP. Does not include
 * the optional functions; to access them see {ERC20Detailed}.
 */
interface IERC20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount) external returns (bool);

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
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

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
}

// File: openzeppelin-solidity/contracts/token/ERC20/ERC20Detailed.sol

pragma solidity ^0.5.0;


/**
 * @dev Optional functions from the ERC20 standard.
 */
contract ERC20Detailed is IERC20 {
    string private _name;
    string private _symbol;
    uint8 private _decimals;

    /**
     * @dev Sets the values for `name`, `symbol`, and `decimals`. All three of
     * these values are immutable: they can only be set once during
     * construction.
     */
    constructor (string memory name, string memory symbol, uint8 decimals) public {
        _name = name;
        _symbol = symbol;
        _decimals = decimals;
    }

    /**
     * @dev Returns the name of the token.
     */
    function name() public view returns (string memory) {
        return _name;
    }

    /**
     * @dev Returns the symbol of the token, usually a shorter version of the
     * name.
     */
    function symbol() public view returns (string memory) {
        return _symbol;
    }

    /**
     * @dev Returns the number of decimals used to get its user representation.
     * For example, if `decimals` equals `2`, a balance of `505` tokens should
     * be displayed to a user as `5,05` (`505 / 10 ** 2`).
     *
     * Tokens usually opt for a value of 18, imitating the relationship between
     * Ether and Wei.
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * no way affects any of the arithmetic of the contract, including
     * {IERC20-balanceOf} and {IERC20-transfer}.
     */
    function decimals() public view returns (uint8) {
        return _decimals;
    }
}

// File: openzeppelin-solidity/contracts/utils/Address.sol

pragma solidity ^0.5.5;

/**
 * @dev Collection of functions related to the address type
 */
library Address {
    /**
     * @dev Returns true if `account` is a contract.
     *
     * [IMPORTANT]
     * ====
     * It is unsafe to assume that an address for which this function returns
     * false is an externally-owned account (EOA) and not a contract.
     *
     * Among others, `isContract` will return false for the following
     * types of addresses:
     *
     *  - an externally-owned account
     *  - a contract in construction
     *  - an address where a contract will be created
     *  - an address where a contract lived, but was destroyed
     * ====
     */
    function isContract(address account) internal view returns (bool) {
        // According to EIP-1052, 0x0 is the value returned for not-yet created accounts
        // and 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470 is returned
        // for accounts without code, i.e. `keccak256('')`
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly { codehash := extcodehash(account) }
        return (codehash != accountHash && codehash != 0x0);
    }

    /**
     * @dev Converts an `address` into `address payable`. Note that this is
     * simply a type cast: the actual underlying value is not changed.
     *
     * _Available since v2.4.0._
     */
    function toPayable(address account) internal pure returns (address payable) {
        return address(uint160(account));
    }

    /**
     * @dev Replacement for Solidity's `transfer`: sends `amount` wei to
     * `recipient`, forwarding all available gas and reverting on errors.
     *
     * https://eips.ethereum.org/EIPS/eip-1884[EIP1884] increases the gas cost
     * of certain opcodes, possibly making contracts go over the 2300 gas limit
     * imposed by `transfer`, making them unable to receive funds via
     * `transfer`. {sendValue} removes this limitation.
     *
     * https://diligence.consensys.net/posts/2019/09/stop-using-soliditys-transfer-now/[Learn more].
     *
     * IMPORTANT: because control is transferred to `recipient`, care must be
     * taken to not create reentrancy vulnerabilities. Consider using
     * {ReentrancyGuard} or the
     * https://solidity.readthedocs.io/en/v0.5.11/security-considerations.html#use-the-checks-effects-interactions-pattern[checks-effects-interactions pattern].
     *
     * _Available since v2.4.0._
     */
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-call-value
        (bool success, ) = recipient.call.value(amount)("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
}

// File: openzeppelin-solidity/contracts/token/ERC20/SafeERC20.sol

pragma solidity ^0.5.0;




/**
 * @title SafeERC20
 * @dev Wrappers around ERC20 operations that throw on failure (when the token
 * contract returns false). Tokens that return no value (and instead revert or
 * throw on failure) are also supported, non-reverting calls are assumed to be
 * successful.
 * To use this library you can add a `using SafeERC20 for ERC20;` statement to your contract,
 * which allows you to call the safe operations as `token.safeTransfer(...)`, etc.
 */
library SafeERC20 {
    using SafeMath for uint256;
    using Address for address;

    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    function safeApprove(IERC20 token, address spender, uint256 value) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        // solhint-disable-next-line max-line-length
        require((value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).add(value);
        callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).sub(value, "SafeERC20: decreased allowance below zero");
        callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     */
    function callOptionalReturn(IERC20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves.

        // A Solidity high level call has three parts:
        //  1. The target address is checked to verify it contains contract code
        //  2. The call itself is made, and success asserted
        //  3. The return value is decoded, which in turn checks the size of the returned data.
        // solhint-disable-next-line max-line-length
        require(address(token).isContract(), "SafeERC20: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = address(token).call(data);
        require(success, "SafeERC20: low-level call failed");

        if (returndata.length > 0) { // Return data is optional
            // solhint-disable-next-line max-line-length
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}

// File: openzeppelin-solidity/contracts/utils/ReentrancyGuard.sol

pragma solidity ^0.5.0;

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
 *
 * _Since v2.5.0:_ this module is now much more gas efficient, given net gas
 * metering changes introduced in the Istanbul hardfork.
 */
contract ReentrancyGuard {
    bool private _notEntered;

    constructor () internal {
        // Storing an initial non-zero value makes deployment a bit more
        // expensive, but in exchange the refund on every call to nonReentrant
        // will be lower in amount. Since refunds are capped to a percetange of
        // the total transaction's gas, it is best to keep them low in cases
        // like this one, to increase the likelihood of the full refund coming
        // into effect.
        _notEntered = true;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and make it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        // On the first call to nonReentrant, _notEntered will be true
        require(_notEntered, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _notEntered = false;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _notEntered = true;
    }
}

// File: Owned.sol

pragma solidity ^0.5.16;

// https://docs.synthetix.io/contracts/source/contracts/owned
contract Owned {
    address public owner;
    address public nominatedOwner;

    constructor(address _owner) public {
        require(_owner != address(0), "Owner address cannot be 0");
        owner = _owner;
        emit OwnerChanged(address(0), _owner);
    }

    function nominateNewOwner(address _owner) external onlyOwner {
        nominatedOwner = _owner;
        emit OwnerNominated(_owner);
    }

    function acceptOwnership() external {
        require(msg.sender == nominatedOwner, "You must be nominated before you can accept ownership");
        emit OwnerChanged(owner, nominatedOwner);
        owner = nominatedOwner;
        nominatedOwner = address(0);
    }

    modifier onlyOwner {
        _onlyOwner();
        _;
    }

    function _onlyOwner() private view {
        require(msg.sender == owner, "Only the contract owner may perform this action");
    }

    event OwnerNominated(address newOwner);
    event OwnerChanged(address oldOwner, address newOwner);
}

// File: Pausable.sol

pragma solidity ^0.5.16;

// Inheritance



// https://docs.synthetix.io/contracts/source/contracts/pausable
contract Pausable is Owned {
    uint public lastPauseTime;
    bool public paused;

    constructor() internal {
        // This contract is abstract, and thus cannot be instantiated directly
        require(owner != address(0), "Owner must be set");
        // Paused will be false, and lastPauseTime will be 0 upon initialisation
    }

    /**
     * @notice Change the paused state of the contract
     * @dev Only the contract owner may call this.
     */
    function setPaused(bool _paused) external onlyOwner {
        // Ensure we're actually changing the state before we do anything
        if (_paused == paused) {
            return;
        }

        // Set our paused state.
        paused = _paused;

        // If applicable, set the last pause time.
        if (paused) {
            lastPauseTime = now;
        }

        // Let everyone know that our pause state has changed.
        emit PauseChanged(paused);
    }

    event PauseChanged(bool isPaused);

    modifier notPaused {
        require(!paused, "This action cannot be performed while the contract is paused");
        _;
    }
}

// File: newStaking.sol

pragma solidity ^0.5.16;
pragma experimental ABIEncoderV2;







contract AlphanetStaking is ReentrancyGuard, Pausable {

    /// @notice Emitted when setLevel
    event SetLevel(string name,uint256 min , uint256 max, uint256 weight);

    /// @notice Emitted when staking
    event Staked(address indexed user, uint256 amount, uint256 userId);

    /// @notice Emitted when claiming
    event Claimed(address indexed user, uint256 amount);

    /// @notice Emitted when withdrawing
    event Withdrawn(address indexed user, uint256 amount);

    /// @notice Emitted when set lockdown duration
    event LockDownDurationUpdated(uint256 newLockDownDuration);

    /// @notice Emitted when set inflation speed
    event InflationSpeedUpdated(uint256 newSpeed);

    /// @notice Emitted when apply withdraw
    event ApplyWithdraw(address indexed user,uint256 amount, uint256 time, uint256 userId);

    using SafeMath for uint256;
    using SafeERC20 for IERC20;

    IERC20 public rewardsToken;
    IERC20 public stakingToken;

    uint256 public lockDownDuration = 5 seconds;
    uint256 public totalStakes ;

    uint256 public virtualTotalStakes;

    uint constant doubleScale = 1e36;

    // uint256 RateScale = 10000;
    uint256 phbDecimals = 1e18;
    uint256 WeightScale = 100;
    address public rewardProvider =0xa1F128FFb55FF901aAD2D640A361E0AcB89E1E42;

    //withdraw rate 5 for 0.05%
    uint256 public withdrawRate = 0;
    uint256 public feeScale = 10000;

    //NOTE:modify me before mainnet
    address public feeCollector =0xa1F128FFb55FF901aAD2D640A361E0AcB89E1E42;

    /// @notice The initial global index
    uint256 public constant globalInitialIndex = 1e36;

    uint256 public inflationSpeed = 0;

    struct Double {
        uint mantissa;
    }

    string [] levels = ["Carbon","Genesis","Platinum","Zirconium","Diamond"];


    struct RateLevel {
        uint256 min;
        uint256 max;
        uint256 weight;
    }

    mapping(string => RateLevel) _ratesLevel;
    mapping(string => uint256) levelAmount;

    mapping(address=>uint256) virtualUserbalance;

    struct withdrawApplication {
        uint256 amount;
        uint256 applyTime;
    }

    mapping(address => uint256) _userRewards;

    mapping(address => uint256) _userIds;
    mapping(uint256 => uint256) _idBalances;

    //this record the user withdraw application
    //according to the requirement, when user want to withdraw his staking phb, he needs to
    //1. call applyWithdraw , this will add a lock period (7 days by default, can be changed by admin)
    //2. call withdraw to withdraw the "withdrawable amounts"
    struct TimedWithdraw {
        uint256 totalApplied;                      //total user applied for withdraw
        mapping(uint256 => uint256) applications;  //apply detail time=>amount
        uint256[] applyTimes;                      //apply timestamp, used for the key of applications mapping
    }

    mapping(address => TimedWithdraw) timeApplyInfo;   //user => TimedWithdraw mapping
    mapping(address => uint256) _balances;

    struct Index {
        uint256 index;
        uint256 lastTime;
    }

    Index globalIndex;
    mapping(address => Index) userIndex;

    /* ========== CONSTRUCTOR ========== */
    constructor(
                address _owner,
                address _rewardsToken,
                address _stakingToken
    ) public Owned(_owner){
        rewardsToken = IERC20(_rewardsToken);
        stakingToken = IERC20(_stakingToken);
        initLevel();
        globalIndex.index = globalInitialIndex;
        globalIndex.lastTime = now;
    }


    /* ========== internals ======== */
    function initLevel() internal {
        _ratesLevel[levels[0]] = RateLevel({min:0,max:1000,weight:100});
        _ratesLevel[levels[1]] = RateLevel({min:1000,max:10000,weight:125});
        _ratesLevel[levels[2]] = RateLevel({min:10000,max:50000,weight:150});
        _ratesLevel[levels[3]] = RateLevel({min:50000,max:100000,weight:175});
        _ratesLevel[levels[4]] = RateLevel({min:100000,max:999999999,weight:200});
    }


    /* =========== views ==========*/

    function balanceOf(address account) external view returns (uint256) {
        return _balances[account];
    }

    function getIdBalance(uint256 userId) external view returns (uint256) {
        return _idBalances[userId];
    }

    function getUserId(address account) public view returns (uint256) {
        return _userIds[account];
    }

    //to calculate the rewards by the gap of userindex and globalindx
    function getUserRewards(address account) public view returns(uint256){
        // updateGlobalIndex();
        uint256 rewardSpeed = inflationSpeed;
        uint256 deltaTime = now.sub(globalIndex.lastTime);

        uint256 rewardAccued = deltaTime.mul(rewardSpeed);
        // Double memory ratio = totalStakes > 0 ? fraction(rewardAccued,totalStakes):Double({mantissa:0});
        Double memory ratio = virtualTotalStakes > 0 ? fraction(rewardAccued,virtualTotalStakes):Double({mantissa:0});

        Double memory gIndex = add_(Double({mantissa: globalIndex.index}), ratio);

        Double memory uIndex = Double({mantissa:userIndex[account].index});

        if (uIndex.mantissa == 0 && gIndex.mantissa > 0) {
            uIndex.mantissa = globalInitialIndex;
        }

        Double memory deltaIndex = sub_(gIndex,uIndex);

        uint256 supplierDelta = mul_(virtualUserbalance[account],deltaIndex);

        return supplierDelta.add(_userRewards[account] );
    }

    //calculate the total amount which apply time already passed [7 days]
    function withdrawableAmount(address account)public view returns(uint256){
        uint256 amount = 0;
        TimedWithdraw storage withdrawApplies = timeApplyInfo[account];

        for (uint8 index = 0; index < withdrawApplies.applyTimes.length; index++) {
            uint256 key = withdrawApplies.applyTimes[index];
            if (now.sub(key) > lockDownDuration){
                amount = amount.add(withdrawApplies.applications[key]);
            }
        }
        return amount;
    }

    //for front end display, the following two method should be used together
    //
    //we return total applied, applied times and applied amounts here
    function getUserApplication(address account) external view returns(uint256, uint256[] memory, uint256[] memory){
        uint256[] memory applyTimes = timeApplyInfo[account].applyTimes;
        uint256[] memory applyAmounts = new uint256[](applyTimes.length);
        for (uint8 index = 0 ;index < applyTimes.length; index++){
            applyAmounts[index] = timeApplyInfo[account].applications[applyTimes[index]];
        }

        return (timeApplyInfo[account].totalApplied, applyTimes, applyAmounts);
    }

    //return levels config in contract
    function getLevelInfos() external view returns(string[] memory){
        return levels;
    }

    //return level detail,key is result of previous function
    function getLevelDetail(string calldata lv) external view returns(RateLevel memory){
        return _ratesLevel[lv];
    }

    //return the total staked amount for the given level
    function getLevelStakes(string calldata lv) external view returns(uint256){
        return levelAmount[lv];
    }


    /* ========== MUTATIVE FUNCTIONS ========== */
    function setLevel( string calldata lv,uint256 min,uint256 max,uint256 weight) external onlyOwner{
        _ratesLevel[lv] = RateLevel({min:min,max:max,weight:weight});
        emit SetLevel(lv,min,max,weight);
    }

    function stake(uint256 amount,uint256 userId) external nonReentrant notPaused returns (bool){
        require(amount > 0, "Cannot stake 0");
        require(userId > 0, "UserId can not be 0");
        require(_userIds[msg.sender]==0 || _userIds[msg.sender]==userId, "Cannot stake with different userId");

        updateGlobalIndex();
        distributeReward(msg.sender);

        //we calculate the "vamount" first here
        updateVAmounts(msg.sender,amount,true);


        totalStakes = totalStakes.add(amount);
        _balances[msg.sender] = _balances[msg.sender].add(amount);
        if(_userIds[msg.sender]==0){
            _userIds[msg.sender]=userId;
        }
        _idBalances[userId].add(amount);

        stakingToken.safeTransferFrom(msg.sender, address(this), amount);

        uint256 rewards = _userRewards[msg.sender];
        if (rewards > 0){
            require(rewardsToken.transferFrom(rewardProvider, msg.sender, rewards),"claim rewards failed");
            delete(_userRewards[msg.sender]);
            emit Claimed(msg.sender,rewards);
        }

        emit Staked(msg.sender,amount,userId);
    }

    //add user withdraw application
    function applyWithdraw(uint256 amount) external nonReentrant{
        require(amount > 0, "Cannot withdraw 0");

        TimedWithdraw storage withdrawApplies = timeApplyInfo[msg.sender];
        //should have enough un-applied balance
        require(amount <= _balances[msg.sender], "exceeded user balance!");

        withdrawApplies.totalApplied = withdrawApplies.totalApplied.add(amount);
        withdrawApplies.applications[now] = amount;
        withdrawApplies.applyTimes.push(now);

        timeApplyInfo[msg.sender] = withdrawApplies;

        updateGlobalIndex();
        distributeReward(msg.sender);

        updateVAmounts(msg.sender,amount,false);

        totalStakes = totalStakes.sub(amount);
        _balances[msg.sender] = _balances[msg.sender].sub(amount);
        uint256 userId = getUserId(msg.sender);
        _idBalances[userId]=_idBalances[userId].sub(amount);

        uint256 rewards = _userRewards[msg.sender];
        if (rewards > 0){
            require(rewardsToken.transferFrom(rewardProvider, msg.sender, rewards),"claim rewards failed");
            delete(_userRewards[msg.sender]);
            emit Claimed(msg.sender,rewards);
        }

        emit ApplyWithdraw(msg.sender,amount,now,userId);
    }

    //withdraw staked amount if possible
    function withdraw(uint256 amount) public nonReentrant {
        require(amount > 0, "Cannot withdraw 0");
        require(withdrawableAmount(msg.sender) >= amount,"not enough withdrawable balance");
        dealwithLockdown(amount,msg.sender);
        uint256 fee = amount.mul(withdrawRate).div(feeScale);
        stakingToken.safeTransfer(msg.sender, amount.sub(fee));
        if (fee > 0 ){
            stakingToken.safeTransfer(feeCollector, fee);
        }

        emit Withdrawn(msg.sender, amount.sub(fee));
    }

    function setLockDownDuration(uint256 _lockdownDuration) external onlyOwner {
        lockDownDuration = _lockdownDuration;
        emit LockDownDurationUpdated(_lockdownDuration);
    }

    function setWithdrawRate(uint256 _rate) external onlyOwner {
        require(_rate < 10000,"withdraw rate is too high");
        withdrawRate = _rate;
    }

    function setFeeCollector(address _feeCollector) external onlyOwner{
        feeCollector = _feeCollector;
    }

    function setRewardProvider(address _rewardProvider) external onlyOwner{
        rewardProvider = _rewardProvider;
    }

    function claimReward() external nonReentrant notPaused returns (bool) {
        updateGlobalIndex();
        distributeReward(msg.sender);
        uint256 rewards = _userRewards[msg.sender];
        require( rewards > 0,"no rewards for this account");
        require(rewardsToken.transferFrom(rewardProvider, msg.sender, rewards),"claim rewards failed");
        delete(_userRewards[msg.sender]);

        emit Claimed(msg.sender,rewards);
    }

    //calculate tokens per seconds

    function setInflationSpeed(uint256 speed) public onlyOwner {
        updateGlobalIndex();
        inflationSpeed = speed;
        emit InflationSpeedUpdated(speed);
    }

    /* ========== INTERNAL FUNCTIONS ========== */
    function updateGlobalIndex() internal {
        uint256 rewardSpeed = inflationSpeed;
        uint256 deltaTime = now.sub(globalIndex.lastTime);

        if (deltaTime > 0 && rewardSpeed > 0) {
            uint256 rewardAccued = deltaTime.mul(rewardSpeed);
            // Double memory ratio = totalStakes > 0 ? fraction(rewardAccued,totalStakes):Double({mantissa:0});
            Double memory ratio = virtualTotalStakes > 0 ? fraction(rewardAccued,virtualTotalStakes):Double({mantissa:0});

            Double memory newIndex = add_(Double({mantissa: globalIndex.index}), ratio);
            globalIndex.index = newIndex.mantissa;
            globalIndex.lastTime = now;
        }else if(deltaTime > 0) {
            globalIndex.lastTime = now;
        }
    }

    function distributeReward(address account) internal {
        Double memory gIndex = Double({mantissa:globalIndex.index});
        Double memory uIndex = Double({mantissa:userIndex[account].index});

        userIndex[account].index = globalIndex.index;

        if (uIndex.mantissa == 0 && gIndex.mantissa > 0) {
            uIndex.mantissa = globalInitialIndex;
        }

        Double memory deltaIndex = sub_(gIndex,uIndex);
        // uint256 supplierDelta = mul_(_balances[account],deltaIndex);
        uint256 supplierDelta = mul_(virtualUserbalance[account],deltaIndex);

        // string memory lv = getBalanceLevel(_balances[account]);
        // uint weight = bytes(lv).length==0 ?0:_ratesLevel[lv].weight;
        // supplierDelta =  supplierDelta.mul(weight).div(WeightScale);
        _userRewards[account] = supplierDelta.add(_userRewards[account] );
    }


    function remove(uint256[] storage array, uint index) internal returns(uint256[] storage) {
        if (index >= array.length) return array;

        if(array.length == 1){
            delete(array[index]);
            return array;
        }
        for (uint i = index; i<array.length-1; i++){
            array[i] = array[i+1];
        }
        array.length--;
        return array;
    }

    function dealwithLockdown(uint256 amount, address account) internal {
        uint256 _total = amount;
        TimedWithdraw storage withdrawApplies = timeApplyInfo[account];
        //applyTimesLen cannot be change
        uint256 applyTimesLen = withdrawApplies.applyTimes.length;
         for (uint8 index = 0; index < applyTimesLen; index++) {
           if (_total > 0){
              uint256 key = withdrawApplies.applyTimes[0];
              if (now.sub(key) > lockDownDuration){
                  if(_total >= withdrawApplies.applications[key]){
                      _total = _total.sub(withdrawApplies.applications[key]);
                      delete( withdrawApplies.applications[key]);
                      remove(withdrawApplies.applyTimes, 0);
                    //   delete( withdrawApplies.applyTimes[index]);
                  }else{
                      withdrawApplies.applications[key] = withdrawApplies.applications[key].sub(_total);
                      _total = 0;
                      break;
                  }
              }
           }
        }

        withdrawApplies.totalApplied  = withdrawApplies.totalApplied.sub(amount);
    }

    function getBalanceLevel(uint256 balance) view internal returns(string memory){
        for (uint8 index = 0 ;index < levels.length; index++){
            RateLevel memory tmp = _ratesLevel[levels[index]];
            if (balance >= tmp.min.mul(phbDecimals) && balance < tmp.max.mul(phbDecimals)){
                return levels[index];
            }
        }
        return "";
    }

    //optimise the stake weight based reward calculation
    //"virtualbalance" is record user real balance * level weight
    //if user total staked 100k, virtualbalance is 100k * 100% = 100k
    //if user total staked 1M virtualbalance si 1M * 250% = 2.5M
    //the staking reward is calculated based on this virtualbalance
    function updateVAmounts(address userAcct,uint256 amount,bool increase) internal {
        uint256 balanceBefore = _balances[userAcct];
        string memory lvBefore = getBalanceLevel(balanceBefore);
        uint weightBefore = bytes(lvBefore).length==0 ? 0 : _ratesLevel[lvBefore].weight;
        uint256 balanceAfter = increase ? balanceBefore.add(amount) : balanceBefore.sub(amount);

        string memory lvAfter = getBalanceLevel(balanceAfter);
        uint weightAfter = bytes(lvAfter).length==0 ?0:_ratesLevel[lvAfter].weight;

        //update vbalance
        uint256 vbalanceBefore =  virtualUserbalance[userAcct] ;
        virtualUserbalance[userAcct] = balanceAfter.mul(weightAfter).div(WeightScale);

        //update vtotalstake
        virtualTotalStakes = virtualTotalStakes.sub(vbalanceBefore).add( virtualUserbalance[userAcct]);

        if (weightBefore == weightAfter){
            //update amount to lv
            levelAmount[lvBefore] = levelAmount[lvBefore].sub(balanceBefore).add(balanceAfter);
        }else{
            levelAmount[lvBefore] = levelAmount[lvBefore].sub(balanceBefore);
            levelAmount[lvAfter] = levelAmount[lvAfter].add(balanceAfter);
        }

    }

    /*========Double============*/
    function fraction(uint a, uint b) pure internal returns (Double memory) {
        return Double({mantissa: div_(mul_(a, doubleScale), b)});
    }

    function add_(Double memory a, Double memory b) pure internal returns (Double memory) {
        return Double({mantissa: add_(a.mantissa, b.mantissa)});
    }

    function add_(uint a, uint b) pure internal returns (uint) {
        return add_(a, b, "addition overflow");
    }

    function add_(uint a, uint b, string memory errorMessage) pure internal returns (uint) {
        uint c = a + b;
        require(c >= a, errorMessage);
        return c;
    }

    function sub_(Double memory a, Double memory b) pure internal returns (Double memory) {
        return Double({mantissa: sub_(a.mantissa, b.mantissa)});
    }

    function sub_(uint a, uint b) pure internal returns (uint) {
        return sub_(a, b, "subtraction underflow");
    }

    function sub_(uint a, uint b, string memory errorMessage) pure internal returns (uint) {
        require(b <= a, errorMessage);
        return a - b;
    }


    function mul_(Double memory a, Double memory b) pure internal returns (Double memory) {
        return Double({mantissa: mul_(a.mantissa, b.mantissa) / doubleScale});
    }
   function mul_(Double memory a, uint b) pure internal returns (Double memory) {
        return Double({mantissa: mul_(a.mantissa, b)});
    }

    function mul_(uint a, Double memory b) pure internal returns (uint) {
        return mul_(a, b.mantissa) / doubleScale;
    }

    function mul_(uint a, uint b) pure internal returns (uint) {
        return mul_(a, b, "multiplication overflow");
    }

    function mul_(uint a, uint b, string memory errorMessage) pure internal returns (uint) {
        if (a == 0 || b == 0) {
            return 0;
        }
        uint c = a * b;
        require(c / a == b, errorMessage);
        return c;
    }
    function div_(Double memory a, Double memory b) pure internal returns (Double memory) {
        return Double({mantissa: div_(mul_(a.mantissa, doubleScale), b.mantissa)});
    }
    function div_(Double memory a, uint b) pure internal returns (Double memory) {
        return Double({mantissa: div_(a.mantissa, b)});
    }

    function div_(uint a, Double memory b) pure internal returns (uint) {
        return div_(mul_(a, doubleScale), b.mantissa);
    }

    function div_(uint a, uint b) pure internal returns (uint) {
        return div_(a, b, "divide by zero");
    }

    function div_(uint a, uint b, string memory errorMessage) pure internal returns (uint) {
        require(b > 0, errorMessage);
        return a / b;
    }


}