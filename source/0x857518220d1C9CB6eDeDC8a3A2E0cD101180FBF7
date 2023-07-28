// ██████  ███████ ██    ██  ██████  ██      ██    ██ ███████ ██  ██████  ███    ██                   
// ██   ██ ██      ██    ██ ██    ██ ██      ██    ██     ██  ██ ██    ██ ████   ██                  
// ██████  █████   ██    ██ ██    ██ ██      ██    ██   ██    ██ ██    ██ ██ ██  ██                   
// ██   ██ ██       ██  ██  ██    ██ ██      ██    ██  ██     ██ ██    ██ ██  ██ ██                   
// ██   ██ ███████   ████    ██████  ███████  ██████  ███████ ██  ██████  ██   ████          

// CONTRACT DEVELOPED BY REVOLUZION ECOSYSTEM

//Revoluzion Ecosystem
//WEB: https://revoluzion.io
//DAPP: https://revoluzion.app

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

/* LIBRARY */

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
     *
     * Furthermore, `isContract` will also return true if the target contract within
     * the same transaction is already scheduled for destruction by `SELFDESTRUCT`,
     * which only has an effect at the end of a transaction.
     * ====
     *
     * [IMPORTANT]
     * ====
     * You shouldn't rely on `isContract` to protect against flash loan attacks!
     *
     * Preventing calls from contracts is highly discouraged. It breaks composability, breaks support for smart wallets
     * like Gnosis Safe, and does not provide security since it can be circumvented by calling from a contract
     * constructor.
     * ====
     */
    function isContract(address account) internal view returns (bool) {
        // This method relies on extcodesize/address.code.length, which returns 0
        // for contracts in construction, since the code is only stored at the end
        // of the constructor execution.

        return account.code.length > 0;
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`], but with
     * `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCallWithValue-address-bytes-uint256-}[`functionCallWithValue`], but
     * with `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(
            address(this).balance >= value,
            "Address: insufficient balance for call"
        );
        (bool success, bytes memory returndata) = target.call{value: value}(
            data
        );
        return
            verifyCallResultFromTarget(
                target,
                success,
                returndata,
                errorMessage
            );
    }

    /**
     * @dev Tool to verify that a low level call to smart-contract was successful, and revert (either by bubbling
     * the revert reason or using the provided one) in case of unsuccessful call or if target was not a contract.
     *
     * _Available since v4.8._
     */
    function verifyCallResultFromTarget(
        address target,
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        if (success) {
            if (returndata.length == 0) {
                // only check isContract if the call was successful and the return data is empty
                // otherwise we already know that it was a contract
                require(isContract(target), "Address: call to non-contract");
            }
            return returndata;
        } else {
            _revert(returndata, errorMessage);
        }
    }

    function _revert(
        bytes memory returndata,
        string memory errorMessage
    ) private pure {
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

/**
 * @title SafeERC20
 * @dev Wrappers around ERC20 operations that throw on failure (when the token
 * contract returns false). Tokens that return no value (and instead revert or
 * throw on failure) are also supported, non-reverting calls are assumed to be
 * successful.
 * To use this library you can add a `using SafeERC20 for IERC20;` statement to your contract,
 * which allows you to call the safe operations as `token.safeTransfer(...)`, etc.
 */
library SafeERC20 {
    using Address for address;

    /**
     * @dev Transfer `value` amount of `token` from the calling contract to `to`. If `token` returns no value,
     * non-reverting calls are assumed to be successful.
     */
    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        _callOptionalReturn(
            token,
            abi.encodeWithSelector(token.transfer.selector, to, value)
        );
    }

    /**
     * @dev Transfer `value` amount of `token` from `from` to `to`, spending the approval given by `from` to the
     * calling contract. If `token` returns no value, non-reverting calls are assumed to be successful.
     */
    function safeTransferFrom(
        IERC20 token,
        address from,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(
            token,
            abi.encodeWithSelector(token.transferFrom.selector, from, to, value)
        );
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     */
    function _callOptionalReturn(IERC20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We use {Address-functionCall} to perform this call, which verifies that
        // the target address contains contract code and also asserts for success in the low-level call.

        bytes memory returndata = address(token).functionCall(
            data,
            "SafeERC20: low-level call failed"
        );
        require(
            returndata.length == 0 || abi.decode(returndata, (bool)),
            "SafeERC20: ERC20 operation did not succeed"
        );
    }
}

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
}

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

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

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
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
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
}

/* INTERFACE */

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
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

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
    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

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

/**
 * @dev Interface of the ERC20 Permit extension allowing approvals to be made via signatures, as defined in
 * https://eips.ethereum.org/EIPS/eip-2612[EIP-2612].
 *
 * Adds the {permit} method, which can be used to change an account's ERC20 allowance (see {IERC20-allowance}) by
 * presenting a message signed by the account. By not relying on {IERC20-approve}, the token holder account doesn't
 * need to send a transaction, and thus is not required to hold Ether at all.
 */
interface IERC20Permit {
    /**
     * @dev Sets `value` as the allowance of `spender` over ``owner``'s tokens,
     * given ``owner``'s signed approval.
     *
     * IMPORTANT: The same issues {IERC20-approve} has related to transaction
     * ordering also apply here.
     *
     * Emits an {Approval} event.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     * - `deadline` must be a timestamp in the future.
     * - `v`, `r` and `s` must be a valid `secp256k1` signature from `owner`
     * over the EIP712-formatted function arguments.
     * - the signature must use ``owner``'s current nonce (see {nonces}).
     *
     * For more information on the signature format, see the
     * https://eips.ethereum.org/EIPS/eip-2612#specification[relevant EIP
     * section].
     */
    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;

    /**
     * @dev Returns the current nonce for `owner`. This value must be
     * included whenever a signature is generated for {permit}.
     *
     * Every successful call to {permit} increases ``owner``'s nonce by one. This
     * prevents a signature from being used multiple times.
     */
    function nonces(address owner) external view returns (uint256);

    /**
     * @dev Returns the domain separator used in the encoding of the signature for {permit}, as defined by {EIP712}.
     */
    // solhint-disable-next-line func-name-mixedcase
    function DOMAIN_SEPARATOR() external view returns (bytes32);
}

/* LOTTERY */

interface IRandomNumberGenerator {
    /**
     * Requests randomness from a user-provided seed
     */
    function getRandomNumber(uint256 theSeed) external;

    /**
     * View latest lotteryId numbers
     */
    function viewLatestLotteryId() external view returns (uint256);

    /**
     * Views random result
     */
    function viewRandomResult() external view returns (uint32);
}

interface ILottery {
    /**
     * @notice Buy tickets for the current lottery
     * @param idLottery: lotteryId
     * @param ticketNumbers: array of ticket numbers between 1,000,000 and 1,999,999
     * @dev Callable by users
     */
    function buyTickets(
        uint256 idLottery,
        uint32[] calldata ticketNumbers
    ) external;

    /**
     * @notice Claim a set of winning tickets for a lottery
     * @param idLottery: lottery id
     * @param idsTicket: array of ticket ids
     * @param brackets: array of brackets for the ticket ids
     * @dev Callable by users only, not contract!
     */
    function claimTickets(
        uint256 idLottery,
        uint256[] calldata idsTicket,
        uint32[] calldata brackets
    ) external;

    /**
     * @notice Close lottery
     * @param idLottery: lottery id
     * @dev Callable by operator
     */
    function closeLottery(uint256 idLottery) external;

    /**
     * @notice Draw the final number, calculate reward in TOKEN per group, and make lottery claimable
     * @param idLottery: lottery id
     * @param autoInjection: reinjects funds into next lottery (vs. withdrawing all)
     * @dev Callable by operator
     */
    function drawFinalNumberAndMakeLotteryClaimable(
        uint256 idLottery,
        bool autoInjection
    ) external;

    /**
     * @notice Inject funds
     * @param idLottery: lottery id
     * @param _amount: amount to inject in TOKEN token
     * @dev Callable by operator
     */
    function injectFunds(uint256 idLottery, uint256 _amount) external;

    /**
     * @notice Start the lottery
     * @dev Callable by operator
     * @param timeEnd: endTime of the lottery
     * @param ticketPriceInToken: price of a ticket in TOKEN
     * @param divisorForDiscount: the divisor to calculate the discount magnitude for bulks
     * @param breakdownRewards: breakdown of rewards per bracket (must sum to 10,000)
     * @param feeTreasury: treasury fee (10,000 = 100%, 100 = 1%)
     */
    function startLottery(
        uint256 timeEnd,
        uint256 ticketPriceInToken,
        uint256 divisorForDiscount,
        uint256[6] calldata breakdownRewards,
        uint256 feeTreasury
    ) external;

    /**
     * @notice View current lottery id
     */
    function viewCurrentLotteryId() external returns (uint256);
}

/** @title Lottery.
 * @notice It is a contract for a lottery system using
 * randomness provided externally.
 */
contract LotteryContract is ReentrancyGuard, ILottery, Ownable {
    using SafeERC20 for IERC20;

    address public injectorAddress = address(0);
    address public operatorAddress = address(0);
    address public treasuryAddress = address(0);

    uint256 public currentLotteryId = 0;
    uint256 public currentTicketId = 0;
    uint256 public pendingInjectionNextLottery = 0;

    uint256 public maxNumberTicketsPerBuyOrClaim = 100;

    uint256 public maximumTicketPrice = 500000 ether;
    uint256 public minimumTicketPrice = 0.0000000001 ether;

    uint256 public lotteryDuration;
    uint256 public defaultTicketPriceInToken;
    uint256 public defaultDivisorForDiscount;
    uint256 public defaultFeeTreasury;
    uint256[6] public defaultBreakdownRewards;

    uint256 public constant MIN_DISCOUNT_DIVISOR = 100;
    uint256 public constant MIN_LENGTH_LOTTERY = 1 minutes + 1 minutes; // 1 min
    uint256 public constant MAX_LENGTH_LOTTERY = 7 days + 5 minutes; // 7 days
    uint256 public constant MAX_TREASURY_FEE = 1000; // 10%

    IERC20 public immutable lotteryToken;
    IRandomNumberGenerator public randomGenerator;

    enum Status {
        Pending,
        Open,
        Close,
        Claimable
    }

    struct Lottery {
        Status status;
        uint256 startTime;
        uint256 endTime;
        uint256 priceTicketInToken;
        uint256 discountDivisor;
        uint256[6] rewardsBreakdown; // 0: 1 matching number // 5: 6 matching numbers
        uint256 treasuryFee; // 500: 5% // 200: 2% // 50: 0.5%
        uint256[6] tokenPerBracket;
        uint256[6] countWinnersPerBracket;
        uint256 firstTicketId;
        uint256 firstTicketIdNextLottery;
        uint256 amountCollectedInToken;
        uint32 finalNumber;
    }

    struct Ticket {
        uint32 number;
        address owner;
    }

    // Mapping are cheaper than arrays
    mapping(uint256 => Lottery) private _lotteries;
    mapping(uint256 => Ticket) private _tickets;

    // Bracket calculator is used for verifying claims for ticket prizes
    mapping(uint32 => uint32) private _bracketCalculator;

    // Keeps track of number of ticket per unique combination for each lotteryId
    mapping(uint256 => mapping(uint32 => uint256))
        private _numberTicketsPerLotteryId;

    // Keep track of user ticket ids for a given lotteryId
    mapping(address => mapping(uint256 => uint256[]))
        private _userTicketIdsPerLotteryId;

    modifier notContract() {
        require(!_isContract(msg.sender), "Contract not allowed");
        require(msg.sender == tx.origin, "Proxy contract not allowed");
        _;
    }

    modifier onlyOperator() {
        require(msg.sender == operatorAddress, "Not operator");
        _;
    }

    modifier onlyOwnerOrInjector() {
        require(
            (msg.sender == owner()) || (msg.sender == injectorAddress),
            "Not owner or injector"
        );
        _;
    }

    event AdminTokenRecovery(address token, uint256 amount);
    event LotteryClose(
        uint256 indexed lotteryId,
        uint256 firstTicketIdNextLottery
    );
    event LotteryInjection(uint256 indexed lotteryId, uint256 injectedAmount);
    event LotteryOpen(
        uint256 indexed lotteryId,
        uint256 startTime,
        uint256 endTime,
        uint256 priceTicketInToken,
        uint256 firstTicketId,
        uint256 injectedAmount
    );
    event LotteryNumberDrawn(
        uint256 indexed lotteryId,
        uint256 finalNumber,
        uint256 countWinningTickets
    );
    event NewOperatorAndTreasuryAndInjectorAddresses(
        address operator,
        address treasury,
        address injector
    );
    event NewRandomGenerator(
        address indexed randomGenerator,
        uint32 finalNumber
    );
    event TicketsPurchase(
        address indexed buyer,
        uint256 indexed lotteryId,
        uint256 numberTickets
    );
    event TicketsClaim(
        address indexed claimer,
        uint256 amount,
        uint256 indexed lotteryId,
        uint256 numberTickets
    );

    /**
     * @notice Constructor
     * @dev RandomNumberGenerator must be deployed prior to this contract
     * @param _lotteryTokenAddress: address of the TOKEN token
     */
    constructor(
        address _lotteryTokenAddress,
        uint256 initLotteryDuration,
        uint256 initDefaultTicketPriceInToken,
        uint256 initDefaultDivisorForDiscount,
        uint256 initDefaultFeeTreasury,
        uint256[6] memory initDefaultBreakdownRewards
    ) {
        require(
            (initLotteryDuration > MIN_LENGTH_LOTTERY) &&
                (initLotteryDuration < MAX_LENGTH_LOTTERY),
            "Lottery length outside of range"
        );

        require(
            (initDefaultTicketPriceInToken >= minimumTicketPrice) &&
                (initDefaultTicketPriceInToken <= maximumTicketPrice),
            "Outside of limits"
        );

        require(
            initDefaultDivisorForDiscount >= MIN_DISCOUNT_DIVISOR,
            "Discount divisor too low"
        );
        require(
            initDefaultFeeTreasury <= MAX_TREASURY_FEE,
            "Treasury fee too high"
        );

        require(
            (initDefaultBreakdownRewards[0] +
                initDefaultBreakdownRewards[1] +
                initDefaultBreakdownRewards[2] +
                initDefaultBreakdownRewards[3] +
                initDefaultBreakdownRewards[4] +
                initDefaultBreakdownRewards[5]) == 10000,
            "Rewards must equal 10000"
        );

        lotteryToken = IERC20(_lotteryTokenAddress);
        randomGenerator = new RandomNumberGenerator(
            address(this),
            _msgSender()
        );

        lotteryDuration = initLotteryDuration;
        defaultTicketPriceInToken = initDefaultTicketPriceInToken;
        defaultDivisorForDiscount = initDefaultDivisorForDiscount;
        defaultFeeTreasury = initDefaultFeeTreasury;
        defaultBreakdownRewards = initDefaultBreakdownRewards;

        // Initializes a mapping
        _bracketCalculator[0] = 1;
        _bracketCalculator[1] = 11;
        _bracketCalculator[2] = 111;
        _bracketCalculator[3] = 1111;
        _bracketCalculator[4] = 11111;
        _bracketCalculator[5] = 111111;
    }

    /**
     * @notice Update lottery duration
     * @param initLotteryDuration: duration of the lottery
     * @dev Callable by owner
     */
    function updateLotteryDuration(
        uint256 initLotteryDuration
    ) external onlyOwner {
        require(
            (initLotteryDuration > MIN_LENGTH_LOTTERY) &&
                (initLotteryDuration < MAX_LENGTH_LOTTERY),
            "Lottery length outside of range"
        );
        require(
            initLotteryDuration != lotteryDuration,
            "This is the current value"
        );

        lotteryDuration = initLotteryDuration;
    }

    /**
     * @notice Update default ticket price in token
     * @param initDefaultTicketPriceInToken: price of a ticket in TOKEN
     * @dev Callable by owner
     */
    function updateDefaultTicketPriceInToken(
        uint256 initDefaultTicketPriceInToken
    ) external onlyOwner {
        require(
            (initDefaultTicketPriceInToken >= minimumTicketPrice) &&
                (initDefaultTicketPriceInToken <= maximumTicketPrice),
            "Outside of limits"
        );
        require(
            initDefaultTicketPriceInToken != defaultTicketPriceInToken,
            "This is the current value"
        );
        defaultTicketPriceInToken = initDefaultTicketPriceInToken;
    }

    /**
     * @notice Update default divisor for discount
     * @param initDefaultDivisorForDiscount: the divisor to calculate the discount magnitude for bulks
     * @dev Callable by owner
     */
    function updateDefaultDivisorForDiscount(
        uint256 initDefaultDivisorForDiscount
    ) external onlyOwner {
        require(
            initDefaultDivisorForDiscount >= MIN_DISCOUNT_DIVISOR,
            "Discount divisor too low"
        );
        require(
            initDefaultDivisorForDiscount != defaultDivisorForDiscount,
            "This is the current value"
        );

        defaultDivisorForDiscount = initDefaultDivisorForDiscount;
    }

    /**
     * @notice Update default fee treasury
     * @param initDefaultFeeTreasury: treasury fee (10,000 = 100%, 100 = 1%)
     * @dev Callable by owner
     */
    function updateDefaultFeeTreasury(
        uint256 initDefaultFeeTreasury
    ) external onlyOwner {
        require(
            initDefaultFeeTreasury <= MAX_TREASURY_FEE,
            "Treasury fee too high"
        );
        require(
            initDefaultFeeTreasury != defaultFeeTreasury,
            "This is the current value"
        );

        defaultFeeTreasury = initDefaultFeeTreasury;
    }

    /**
     * @notice Update default breakdown rewards
     * @param initDefaultBreakdownRewards: breakdown of rewards per bracket (must sum to 10,000)
     * @dev Callable by owner
     */
    function updateDefaultBreakdownRewards(
        uint256[6] memory initDefaultBreakdownRewards
    ) external onlyOwner {
        require(
            (initDefaultBreakdownRewards[0] +
                initDefaultBreakdownRewards[1] +
                initDefaultBreakdownRewards[2] +
                initDefaultBreakdownRewards[3] +
                initDefaultBreakdownRewards[4] +
                initDefaultBreakdownRewards[5]) == 10000,
            "Rewards must equal 10000"
        );

        defaultBreakdownRewards = initDefaultBreakdownRewards;
    }

    /**
     * @notice Buy tickets for the current lottery
     * @param idLottery: lotteryId
     * @param ticketNumbers: array of ticket numbers between 1,000,000 and 1,999,999
     * @dev Callable by users
     */
    function buyTickets(
        uint256 idLottery,
        uint32[] calldata ticketNumbers
    ) external override notContract nonReentrant {
        require(ticketNumbers.length != 0, "No ticket specified");
        require(
            ticketNumbers.length <= maxNumberTicketsPerBuyOrClaim,
            "Too many tickets"
        );

        require(
            _lotteries[idLottery].status == Status.Open,
            "Lottery is not open"
        );
        require(
            block.timestamp < _lotteries[idLottery].endTime,
            "Lottery is over"
        );

        // Calculate number of TOKEN to this contract
        uint256 amountTokenToTransfer = _calculateTotalPriceForBulkTickets(
            _lotteries[idLottery].discountDivisor,
            _lotteries[idLottery].priceTicketInToken,
            ticketNumbers.length
        );

        // Increment the total amount collected for the lottery round
        _lotteries[idLottery].amountCollectedInToken += amountTokenToTransfer;

        for (uint256 i = 0; i < ticketNumbers.length; i++) {
            uint32 thisTicketNumber = ticketNumbers[i];

            require(
                (thisTicketNumber >= 0.001 gwei) &&
                    (thisTicketNumber <= 1999999),
                "Outside range"
            );

            _numberTicketsPerLotteryId[idLottery][
                1 + (thisTicketNumber % 10)
            ]++;
            _numberTicketsPerLotteryId[idLottery][
                11 + (thisTicketNumber % 100)
            ]++;
            _numberTicketsPerLotteryId[idLottery][
                111 + (thisTicketNumber % 1000)
            ]++;
            _numberTicketsPerLotteryId[idLottery][
                1111 + (thisTicketNumber % 10000)
            ]++;
            _numberTicketsPerLotteryId[idLottery][
                11111 + (thisTicketNumber % 0.0001 gwei)
            ]++;
            _numberTicketsPerLotteryId[idLottery][
                111111 + (thisTicketNumber % 0.001 gwei)
            ]++;

            _userTicketIdsPerLotteryId[msg.sender][idLottery].push(
                currentTicketId
            );

            _tickets[currentTicketId] = Ticket({
                number: thisTicketNumber,
                owner: msg.sender
            });

            // Increase lottery ticket number
            currentTicketId++;
        }

        // Transfer token tokens to this contract
        lotteryToken.safeTransferFrom(
            address(msg.sender),
            address(this),
            amountTokenToTransfer
        );

        emit TicketsPurchase(msg.sender, idLottery, ticketNumbers.length);
    }

    /**
     * @notice Claim a set of winning tickets for a lottery
     * @param idLottery: lottery id
     * @param idsTicket: array of ticket ids
     * @param brackets: array of brackets for the ticket ids
     * @dev Callable by users only, not contract!
     */
    function claimTickets(
        uint256 idLottery,
        uint256[] calldata idsTicket,
        uint32[] calldata brackets
    ) external override notContract nonReentrant {
        require(idsTicket.length == brackets.length, "Not same length");
        require(idsTicket.length != 0, "Length must be >0");
        require(
            idsTicket.length <= maxNumberTicketsPerBuyOrClaim,
            "Too many tickets to claim"
        );
        require(
            _lotteries[idLottery].status == Status.Claimable,
            "Lottery not claimable"
        );

        // Initializes the rewardInTokenToTransfer
        uint256 rewardInTokenToTransfer;

        for (uint256 i = 0; i < idsTicket.length; i++) {
            require(brackets[i] < 6, "Bracket out of range"); // Must be between 0 and 5

            uint256 thisTicketId = idsTicket[i];

            bool high = _lotteries[idLottery].firstTicketIdNextLottery >
                thisTicketId;
            bool low = _lotteries[idLottery].firstTicketId <= thisTicketId;

            require(high, "TicketId too high");
            require(low, "TicketId too low");
            require(
                msg.sender == _tickets[thisTicketId].owner,
                "Not the owner"
            );

            // Update the lottery ticket owner to 0x address
            _tickets[thisTicketId].owner = address(0);

            uint256 rewardForTicketId = _calculateRewardsForTicketId(
                idLottery,
                thisTicketId,
                brackets[i]
            );

            // Check user is claiming the correct bracket
            require(rewardForTicketId != 0, "No prize for this bracket");

            if (brackets[i] != 5) {
                require(
                    _calculateRewardsForTicketId(
                        idLottery,
                        thisTicketId,
                        brackets[i] + 1
                    ) < 1,
                    "Bracket must be higher"
                );
            }

            // Increment the reward to transfer
            rewardInTokenToTransfer += rewardForTicketId;
        }

        emit TicketsClaim(
            msg.sender,
            rewardInTokenToTransfer,
            idLottery,
            idsTicket.length
        );

        // Transfer money to msg.sender
        lotteryToken.safeTransfer(msg.sender, rewardInTokenToTransfer);
    }

    /**
     * @notice Public user can use this function to finalize lottery.
     * @param idLottery: lottery id
     * @dev Callable by anyone
     */
    function userInitiateLotteryFinalize(uint256 idLottery) external {
        _closeLottery(idLottery);
        _drawFinalNumberAndMakeLotteryClaimable(idLottery, true);
        uint256 endTime = block.timestamp + lotteryDuration;
        _startLottery(
            endTime,
            defaultTicketPriceInToken,
            defaultDivisorForDiscount,
            defaultBreakdownRewards,
            defaultFeeTreasury
        );
    }

    /**
     * @notice Close lottery
     * @param idLottery: lottery id
     * @dev Callable by operator
     */
    function closeLottery(
        uint256 idLottery
    ) external override onlyOperator nonReentrant {
        _closeLottery(idLottery);
    }

    function _closeLottery(uint256 idLottery) internal {
        require(
            _lotteries[idLottery].status == Status.Open,
            "Lottery not open"
        );
        require(
            block.timestamp > _lotteries[idLottery].endTime,
            "Lottery not over"
        );
        _lotteries[idLottery].firstTicketIdNextLottery = currentTicketId;

        _lotteries[idLottery].status = Status.Close;

        // Request a random number from the generator based on a seed
        randomGenerator.getRandomNumber(
            uint256(keccak256(abi.encodePacked(idLottery, currentTicketId)))
        );

        emit LotteryClose(idLottery, currentTicketId);
    }

    /**
     * @notice Draw the final number, calculate reward in TOKEN per group, and make lottery claimable
     * @param idLottery: lottery id
     * @param autoInjection: reinjects funds into next lottery (vs. withdrawing all)
     * @dev Callable by operator
     */
    function drawFinalNumberAndMakeLotteryClaimable(
        uint256 idLottery,
        bool autoInjection
    ) external override onlyOperator nonReentrant {
        _drawFinalNumberAndMakeLotteryClaimable(idLottery, autoInjection);
    }

    function _drawFinalNumberAndMakeLotteryClaimable(
        uint256 idLottery,
        bool autoInjection
    ) internal {
        bool close = _lotteries[idLottery].status == Status.Close;
        require(close, "Lottery not close");
        require(
            idLottery == randomGenerator.viewLatestLotteryId(),
            "Numbers not drawn"
        );

        // Calculate the finalNumber based on the randomResult generated by ChainLink's fallback
        uint32 finalNumber = randomGenerator.viewRandomResult();

        // Initialize a number to count addresses in the previous bracket
        uint256 numberAddressesInPreviousBracket = 0;

        // Calculate the amount to share post-treasury fee
        uint256 totalAmountToShareToWinners = (
            _lotteries[idLottery].amountCollectedInToken
        ) * (10000 - _lotteries[idLottery].treasuryFee);
        uint256 amountToShareToWinners = totalAmountToShareToWinners / 10000;

        // Initializes the amount to withdraw to treasury
        uint256 amountToWithdrawToTreasury = 0;

        uint256 idLotteryVal = idLottery;
        // Calculate prizes in TOKEN for each bracket by starting from the highest one
        for (uint32 i = 0; i < 6; i++) {
            uint32 j = 5 - i;
            uint32 transformedWinningNumber = _bracketCalculator[j] +
                (finalNumber % (uint32(10) ** (j + 1)));

            _lotteries[idLotteryVal].countWinnersPerBracket[j] =
                _numberTicketsPerLotteryId[idLotteryVal][
                    transformedWinningNumber
                ] -
                numberAddressesInPreviousBracket;

            // A. If number of users for this bracket number is superior to 0
            if (
                (_numberTicketsPerLotteryId[idLotteryVal][
                    transformedWinningNumber
                ] - numberAddressesInPreviousBracket) != 0
            ) {
                // B. If rewards at this bracket are > 0, calculate, else, report the numberAddresses from previous bracket
                if (_lotteries[idLotteryVal].rewardsBreakdown[j] != 0) {
                    _lotteries[idLotteryVal].tokenPerBracket[j] =
                        (((_lotteries[idLotteryVal].rewardsBreakdown[j] *
                            totalAmountToShareToWinners) / 10000) /
                            (_numberTicketsPerLotteryId[idLotteryVal][
                                transformedWinningNumber
                            ] - numberAddressesInPreviousBracket)) /
                        10000;

                    // Update numberAddressesInPreviousBracket
                    numberAddressesInPreviousBracket = _numberTicketsPerLotteryId[
                        idLotteryVal
                    ][transformedWinningNumber];
                }
                // A. No TOKEN to distribute, they are added to the amount to withdraw to treasury address
            } else {
                _lotteries[idLotteryVal].tokenPerBracket[j] = 0;

                amountToWithdrawToTreasury +=
                    ((_lotteries[idLotteryVal].rewardsBreakdown[j] *
                        totalAmountToShareToWinners) / 10000) /
                    10000;
            }
        }

        // Update internal statuses for lottery
        _lotteries[idLotteryVal].finalNumber = finalNumber;
        _lotteries[idLotteryVal].status = Status.Claimable;

        if (autoInjection) {
            pendingInjectionNextLottery = amountToWithdrawToTreasury;
            amountToWithdrawToTreasury = 0;
        }

        amountToWithdrawToTreasury += (_lotteries[idLotteryVal]
            .amountCollectedInToken - amountToShareToWinners);

        // Transfer TOKEN to treasury address
        lotteryToken.safeTransfer(treasuryAddress, amountToWithdrawToTreasury);

        emit LotteryNumberDrawn(
            currentLotteryId,
            finalNumber,
            numberAddressesInPreviousBracket
        );
    }

    /**
     * @notice Change the random generator
     * @dev The calls to functions are used to verify the new generator implements them properly.
     * It is necessary to wait for the VRF response before starting a round.
     * Callable only by the contract owner
     * @param randomGeneratorAddress: address of the random generator
     */
    function changeRandomGenerator(
        address randomGeneratorAddress
    ) external onlyOwner {
        require(
            _lotteries[currentLotteryId].status == Status.Claimable,
            "Lottery not in claimable"
        );

        randomGenerator = IRandomNumberGenerator(randomGeneratorAddress);

        // Request a random number from the generator based on a seed
        IRandomNumberGenerator(randomGeneratorAddress).getRandomNumber(
            uint256(
                keccak256(abi.encodePacked(currentLotteryId, currentTicketId))
            )
        );

        // Calculate the finalNumber based on the randomResult generated by ChainLink's fallback
        uint32 finalNumber = IRandomNumberGenerator(randomGeneratorAddress)
            .viewRandomResult();

        emit NewRandomGenerator(randomGeneratorAddress, finalNumber);
    }

    /**
     * @notice Inject funds
     * @param idLottery: lottery id
     * @param amount: amount to inject in TOKEN token
     * @dev Callable by owner or injector address
     */
    function injectFunds(
        uint256 idLottery,
        uint256 amount
    ) external override onlyOwnerOrInjector {
        require(
            _lotteries[idLottery].status == Status.Open,
            "Lottery not open"
        );

        _lotteries[idLottery].amountCollectedInToken += amount;
        emit LotteryInjection(idLottery, amount);

        lotteryToken.safeTransferFrom(
            address(msg.sender),
            address(this),
            amount
        );
    }

    /**
     * @notice Start the lottery
     * @dev Callable by operator
     * @param timeEnd: endTime of the lottery
     * @param ticketPriceInToken: price of a ticket in TOKEN
     * @param divisorForDiscount: the divisor to calculate the discount magnitude for bulks
     * @param breakdownRewards: breakdown of rewards per bracket (must sum to 10,000)
     * @param feeTreasury: treasury fee (10,000 = 100%, 100 = 1%)
     */
    function startLottery(
        uint256 timeEnd,
        uint256 ticketPriceInToken,
        uint256 divisorForDiscount,
        uint256[6] memory breakdownRewards,
        uint256 feeTreasury
    ) external override onlyOperator {
        _startLottery(
            timeEnd,
            ticketPriceInToken,
            divisorForDiscount,
            breakdownRewards,
            feeTreasury
        );
    }

    function _startLottery(
        uint256 timeEnd,
        uint256 ticketPriceInToken,
        uint256 divisorForDiscount,
        uint256[6] memory breakdownRewards,
        uint256 feeTreasury
    ) internal {
        require(
            (currentLotteryId == 0) ||
                (_lotteries[currentLotteryId].status == Status.Claimable),
            "Not time to start Lottery"
        );

        require(
            ((timeEnd - block.timestamp) > MIN_LENGTH_LOTTERY) &&
                ((timeEnd - block.timestamp) < MAX_LENGTH_LOTTERY),
            "Lottery length outside of range"
        );

        require(
            (ticketPriceInToken >= minimumTicketPrice) &&
                (ticketPriceInToken <= maximumTicketPrice),
            "Outside of limits"
        );

        require(
            divisorForDiscount >= MIN_DISCOUNT_DIVISOR,
            "Discount divisor too low"
        );
        require(feeTreasury <= MAX_TREASURY_FEE, "Treasury fee too high");

        require(
            (breakdownRewards[0] +
                breakdownRewards[1] +
                breakdownRewards[2] +
                breakdownRewards[3] +
                breakdownRewards[4] +
                breakdownRewards[5]) == 10000,
            "Rewards must equal 10000"
        );

        currentLotteryId++;

        _lotteries[currentLotteryId] = Lottery({
            status: Status.Open,
            startTime: block.timestamp,
            endTime: timeEnd,
            priceTicketInToken: ticketPriceInToken,
            discountDivisor: divisorForDiscount,
            rewardsBreakdown: breakdownRewards,
            treasuryFee: feeTreasury,
            tokenPerBracket: [
                uint256(0),
                uint256(0),
                uint256(0),
                uint256(0),
                uint256(0),
                uint256(0)
            ],
            countWinnersPerBracket: [
                uint256(0),
                uint256(0),
                uint256(0),
                uint256(0),
                uint256(0),
                uint256(0)
            ],
            firstTicketId: currentTicketId,
            firstTicketIdNextLottery: currentTicketId,
            amountCollectedInToken: pendingInjectionNextLottery,
            finalNumber: 0
        });

        emit LotteryOpen(
            currentLotteryId,
            block.timestamp,
            timeEnd,
            ticketPriceInToken,
            currentTicketId,
            pendingInjectionNextLottery
        );

        pendingInjectionNextLottery = 0;
    }

    /**
     * @notice It allows the admin to recover wrong tokens sent to the contract
     * @param tokenAddress: the address of the token to withdraw
     * @param tokenAmount: the number of token amount to withdraw
     * @dev Only callable by owner.
     */
    function recoverWrongTokens(
        address tokenAddress,
        uint256 tokenAmount
    ) external onlyOwner {
        require(tokenAddress != address(lotteryToken), "Cannot be CBC token");

        emit AdminTokenRecovery(tokenAddress, tokenAmount);

        IERC20(tokenAddress).safeTransfer(address(msg.sender), tokenAmount);
    }

    /**
     * @notice Set TOKEN price ticket upper/lower limit
     * @dev Only callable by owner
     * @param minimumPrice: minimum price of a ticket in TOKEN
     * @param maximumPrice: maximum price of a ticket in TOKEN
     */
    function setMinAndMaxTicketPriceInToken(
        uint256 minimumPrice,
        uint256 maximumPrice
    ) external onlyOwner {
        require(minimumPrice <= maximumPrice, "minPrice must be < maxPrice");

        minimumTicketPrice = minimumPrice;
        maximumTicketPrice = maximumPrice;
    }

    /**
     * @notice Set max number of tickets
     * @dev Only callable by owner
     */
    function setMaxNumberTicketsPerBuy(
        uint256 maxNumberTicketsPerBuy
    ) external onlyOwner {
        require(maxNumberTicketsPerBuy != 0, "Must be > 0");
        maxNumberTicketsPerBuyOrClaim = maxNumberTicketsPerBuy;
    }

    /**
     * @notice Set operator, treasury, and injector addresses
     * @dev Only callable by owner
     * @param addressOperator: address of the operator
     * @param addressTreasury: address of the treasury
     * @param addressInjector: address of the injector
     */
    function setOperatorAndTreasuryAndInjectorAddresses(
        address addressOperator,
        address addressTreasury,
        address addressInjector
    ) external onlyOwner {
        require(addressOperator != address(0), "Cannot be zero address");
        require(addressTreasury != address(0), "Cannot be zero address");
        require(addressInjector != address(0), "Cannot be zero address");

        operatorAddress = addressOperator;
        treasuryAddress = addressTreasury;
        injectorAddress = addressInjector;

        emit NewOperatorAndTreasuryAndInjectorAddresses(
            addressOperator,
            addressTreasury,
            addressInjector
        );
    }

    /**
     * @notice Calculate price of a set of tickets
     * @param divisorForDiscount: divisor for the discount
     * @param priceTicket price of a ticket (in TOKEN)
     * @param numberTickets number of tickets to buy
     */
    function calculateTotalPriceForBulkTickets(
        uint256 divisorForDiscount,
        uint256 priceTicket,
        uint256 numberTickets
    ) external pure returns (uint256) {
        require(
            divisorForDiscount >= MIN_DISCOUNT_DIVISOR,
            "Must be >= MIN_DISCOUNT_DIVISOR"
        );
        require(numberTickets != 0, "Number of tickets must be > 0");

        return
            _calculateTotalPriceForBulkTickets(
                divisorForDiscount,
                priceTicket,
                numberTickets
            );
    }

    /**
     * @notice View current lottery id
     */
    function viewCurrentLotteryId() external view override returns (uint256) {
        return currentLotteryId;
    }

    /**
     * @notice View lottery information
     * @param idLottery: lottery id
     */
    function viewLottery(
        uint256 idLottery
    ) external view returns (Lottery memory) {
        return _lotteries[idLottery];
    }

    /**
     * @notice View ticker statuses and numbers for an array of ticket ids
     * @param idsTicket: array of idTicket
     */
    function viewNumbersAndStatusesForTicketIds(
        uint256[] calldata idsTicket
    ) external view returns (uint32[] memory, bool[] memory) {
        uint256 length = idsTicket.length;
        uint32[] memory ticketNumbers = new uint32[](length);
        bool[] memory ticketStatuses = new bool[](length);

        for (uint256 i = 0; i < length; i++) {
            ticketNumbers[i] = _tickets[idsTicket[i]].number;
            if (_tickets[idsTicket[i]].owner == address(0)) {
                ticketStatuses[i] = true;
            } else {
                ticketStatuses[i] = false;
            }
        }

        return (ticketNumbers, ticketStatuses);
    }

    /**
     * @notice View rewards for a given ticket, providing a bracket, and lottery id
     * @dev Computations are mostly offchain. This is used to verify a ticket!
     * @param idLottery: lottery id
     * @param idTicket: ticket id
     * @param bracket: bracket for the idTicket to verify the claim and calculate rewards
     */
    function viewRewardsForTicketId(
        uint256 idLottery,
        uint256 idTicket,
        uint32 bracket
    ) external view returns (uint256) {
        // Check lottery is in claimable status
        if (_lotteries[idLottery].status != Status.Claimable) {
            return 0;
        }

        // Check idTicket is within range
        if (
            (_lotteries[idLottery].firstTicketIdNextLottery < idTicket) &&
            (_lotteries[idLottery].firstTicketId >= idTicket)
        ) {
            return 0;
        }

        return _calculateRewardsForTicketId(idLottery, idTicket, bracket);
    }

    /**
     * @notice View user ticket ids, numbers, and statuses of user for a given lottery
     * @param user: user address
     * @param idLottery: lottery id
     * @param cursor: cursor to start where to retrieve the tickets
     * @param size: the number of tickets to retrieve
     */
    function viewUserInfoForLotteryId(
        address user,
        uint256 idLottery,
        uint256 cursor,
        uint256 size
    )
        external
        view
        returns (uint256[] memory, uint32[] memory, uint32[] memory, bool[] memory, bool[] memory, uint256)
    {
        uint256 length = size;
        uint256 numberTicketsBoughtAtLotteryId = _userTicketIdsPerLotteryId[
            user
        ][idLottery].length;

        if (length > (numberTicketsBoughtAtLotteryId - cursor)) {
            length = numberTicketsBoughtAtLotteryId - cursor;
        }

        address lotteryUser = user;
        uint256 lotteryID = idLottery;
        uint256 currentCursor = cursor;

        uint256[] memory lotteryTicketIds = new uint256[](length);
        uint32[] memory ticketNumbers = new uint32[](length);
        uint32[] memory ticketHighestBracket = new uint32[](length);
        bool[] memory ticketStatuses = new bool[](length);
        bool[] memory ticketWinnings = new bool[](length);

        for (uint256 i = 0; i < length; i++) {
            lotteryTicketIds[i] = _userTicketIdsPerLotteryId[lotteryUser][lotteryID][
                i + currentCursor
            ];
            ticketNumbers[i] = _tickets[lotteryTicketIds[i]].number;

            // True = ticket claimed
            if (_tickets[lotteryTicketIds[i]].owner == address(0)) {
                ticketStatuses[i] = true;
            } else {
                // ticket not claimed (includes the ones that cannot be claimed)
                ticketStatuses[i] = false;
            }

            (bool won, uint32 bracketToClaim) = _checkTicketHighestBracket(lotteryID, lotteryTicketIds[i]);
            ticketWinnings[i] = won;
            ticketHighestBracket[i] = bracketToClaim;
        }

        return (
            lotteryTicketIds,
            ticketNumbers,
            ticketHighestBracket,
            ticketStatuses,
            ticketWinnings,
            currentCursor + length
        );
    }

    /**
     * @notice Calculate rewards for a given ticket
     * @param idLottery: lottery id
     * @param idTicket: ticket id
     * @param bracket: bracket for the idTicket to verify the claim and calculate rewards
     */
    function _calculateRewardsForTicketId(
        uint256 idLottery,
        uint256 idTicket,
        uint32 bracket
    ) internal view returns (uint256) {
        // Retrieve the winning number combination
        uint32 userNumber = _lotteries[idLottery].finalNumber;

        // Retrieve the user number combination from the idTicket
        uint32 winningTicketNumber = _tickets[idTicket].number;

        // Apply transformation to verify the claim provided by the user is true
        uint32 transformedWinningNumber = _bracketCalculator[bracket] +
            (winningTicketNumber % (uint32(10) ** (bracket + 1)));

        uint32 transformedUserNumber = _bracketCalculator[bracket] +
            (userNumber % (uint32(10) ** (bracket + 1)));

        // Confirm that the two transformed numbers are the same, if not throw
        if (transformedWinningNumber != transformedUserNumber) {
            return 0;
        } else {
            return _lotteries[idLottery].tokenPerBracket[bracket];
        }
    }

    function _checkTicketHighestBracket(
        uint256 idLottery,
        uint256 idTicket
    ) internal view returns (bool, uint32) {
        // Retrieve the winning number combination
        uint32 userNumber = _lotteries[idLottery].finalNumber;

        // Retrieve the user number combination from the idTicket
        uint32 winningTicketNumber = _tickets[idTicket].number;

        uint32 bracket = 0;
        uint32 bracketToClaim = 0;
        bool highest = false;
        bool won = false;
        
        while (bracket < 6 && !highest) {
            // Apply transformation to verify the claim provided by the user is true
            uint32 transformedWinningNumber = _bracketCalculator[bracket] +
                (winningTicketNumber % (uint32(10) ** (bracket + 1)));

            uint32 transformedUserNumber = _bracketCalculator[bracket] +
                (userNumber % (uint32(10) ** (bracket + 1)));

            // Confirm that the two transformed numbers are the same, if not throw
            if (transformedWinningNumber != transformedUserNumber) {
                highest = true;
                bracket += 1;
            } else {
                bracketToClaim = bracket;
                won = true;
                bracket += 1;
            }
        }

        return (won, bracketToClaim);
    }

    /**
     * @notice Calculate final price for bulk of tickets
     * @param divisorForDiscount: divisor for the discount (the smaller it is, the greater the discount is)
     * @param _priceTicket: price of a ticket
     * @param _numberTickets: number of tickets purchased
     */
    function _calculateTotalPriceForBulkTickets(
        uint256 divisorForDiscount,
        uint256 _priceTicket,
        uint256 _numberTickets
    ) internal pure returns (uint256) {
        return
            (_priceTicket *
                _numberTickets *
                (divisorForDiscount + 1 - _numberTickets)) / divisorForDiscount;
    }

    /**
     * @notice Check if an address is a contract
     */
    function _isContract(address _addr) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(_addr)
        }
        return size > 0;
    }
}

/* RANDOM NUMBER GENERATOR */

contract RandomNumberGenerator is IRandomNumberGenerator, Ownable {
    using SafeERC20 for IERC20;

    address public lottery;
    bytes32 public keyHash;
    bytes32 public latestRequestId;
    uint32 public randomResult;
    uint256 public latestLotteryId;

    constructor(address lotteryAddress, address newOwner) {
        lottery = lotteryAddress;
        transferOwnership(newOwner);
    }

    /**
     * @notice Request randomness from a user-provided seed
     * @param theSeed: seed provided by the lottery
     */
    function getRandomNumber(uint256 theSeed) external {
        latestRequestId = keccak256(abi.encodePacked(keyHash, theSeed));
        randomResult = uint32(
            0.001 gwei + (uint256(latestRequestId) % 0.001 gwei)
        );
        latestLotteryId = ILottery(lottery).viewCurrentLotteryId();
    }

    /**
     * @notice Change the keyHash
     * @param hashKey: new keyHash
     */
    function setKeyHash(bytes32 hashKey) external onlyOwner {
        keyHash = hashKey;
    }

    /**
     * @notice Set the address for the lottery
     * @param lotteryAddress: address of the lottery
     */
    function setLotteryAddress(address lotteryAddress) external onlyOwner {
        require(
            lotteryAddress != address(0),
            "Cannot set Lottery address as null address"
        );
        lottery = lotteryAddress;
    }

    /**
     * @notice It allows the admin to withdraw tokens sent to the contract
     * @param addressToken: the address of the token to withdraw
     * @param amountToken: the number of token amount to withdraw
     * @dev Only callable by owner.
     */
    function withdrawTokens(
        address addressToken,
        uint256 amountToken
    ) external onlyOwner {
        IERC20(addressToken).safeTransfer(address(msg.sender), amountToken);
    }

    /**
     * @notice View latestLotteryId
     */
    function viewLatestLotteryId() external view returns (uint256) {
        return latestLotteryId;
    }

    /**
     * @notice View random result
     */
    function viewRandomResult() external view returns (uint32) {
        return randomResult;
    }
}