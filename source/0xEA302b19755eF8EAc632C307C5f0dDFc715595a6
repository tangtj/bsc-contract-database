// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

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
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
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

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
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
     */
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    /**
     * @dev Performs a Solidity function call using a low level `call`. A
     * plain `call` is an unsafe replacement for a function call: use this
     * function instead.
     *
     * If `target` reverts with a revert reason, it is bubbled up by this
     * function (like regular Solidity function calls).
     *
     * Returns the raw returned data. To convert to the expected return value,
     * use https://solidity.readthedocs.io/en/latest/units-and-global-variables.html?highlight=abi.decode#abi-encoding-and-decoding-functions[`abi.decode`].
     *
     * Requirements:
     *
     * - `target` must be a contract.
     * - calling `target` with `data` must not revert.
     *
     * _Available since v3.1._
     */
    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCall(target, data, "Address: low-level call failed");
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
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but also transferring `value` wei to `target`.
     *
     * Requirements:
     *
     * - the calling contract must have an ETH balance of at least `value`.
     * - the called Solidity function must be `payable`.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
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
        require(address(this).balance >= value, "Address: insufficient balance for call");
        require(isContract(target), "Address: call to non-contract");

        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a static call.
     *
     * _Available since v3.3._
     */
    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        return functionStaticCall(target, data, "Address: low-level static call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a static call.
     *
     * _Available since v3.3._
     */
    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        require(isContract(target), "Address: static call to non-contract");

        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a delegate call.
     *
     * _Available since v3.4._
     */
    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionDelegateCall(target, data, "Address: low-level delegate call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a delegate call.
     *
     * _Available since v3.4._
     */
    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(isContract(target), "Address: delegate call to non-contract");

        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    /**
     * @dev Tool to verifies that a low level call was successful, and revert if it wasn't, either by bubbling the
     * revert reason using the provided one.
     *
     * _Available since v4.3._
     */
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

    function safeTransfer(
        IERC20 token,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(
        IERC20 token,
        address from,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    /**
     * @dev Deprecated. This function has issues similar to the ones found in
     * {IERC20-approve}, and its usage is discouraged.
     *
     * Whenever possible, use {safeIncreaseAllowance} and
     * {safeDecreaseAllowance} instead.
     */
    function safeApprove(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        require(
            (value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        uint256 newAllowance = token.allowance(address(this), spender) + value;
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        unchecked {
            uint256 oldAllowance = token.allowance(address(this), spender);
            require(oldAllowance >= value, "SafeERC20: decreased allowance below zero");
            uint256 newAllowance = oldAllowance - value;
            _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
        }
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     */
    function _callOptionalReturn(IERC20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We use {Address.functionCall} to perform this call, which verifies that
        // the target address contains contract code and also asserts for success in the low-level call.

        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        if (returndata.length > 0) {
            // Return data is optional
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
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

/**
 * @dev Wrappers over Solidity's uintXX/intXX casting operators with added overflow
 * checks.
 *
 * Downcasting from uint256/int256 in Solidity does not revert on overflow. This can
 * easily result in undesired exploitation or bugs, since developers usually
 * assume that overflows raise errors. `SafeCast` restores this intuition by
 * reverting the transaction when such an operation overflows.
 *
 * Using this library instead of the unchecked operations eliminates an entire
 * class of bugs, so it's recommended to use it always.
 *
 * Can be combined with {SafeMath} and {SignedSafeMath} to extend it to smaller types, by performing
 * all math on `uint256` and `int256` and then downcasting.
 */
library SafeCast {
    /**
     * @dev Returns the downcasted uint224 from uint256, reverting on
     * overflow (when the input is greater than largest uint224).
     *
     * Counterpart to Solidity's `uint224` operator.
     *
     * Requirements:
     *
     * - input must fit into 224 bits
     */
    function toUint224(uint256 value) internal pure returns (uint224) {
        require(value <= type(uint224).max, "SafeCast: value doesn't fit in 224 bits");
        return uint224(value);
    }

    /**
     * @dev Returns the downcasted uint128 from uint256, reverting on
     * overflow (when the input is greater than largest uint128).
     *
     * Counterpart to Solidity's `uint128` operator.
     *
     * Requirements:
     *
     * - input must fit into 128 bits
     */
    function toUint128(uint256 value) internal pure returns (uint128) {
        require(value <= type(uint128).max, "SafeCast: value doesn't fit in 128 bits");
        return uint128(value);
    }

    /**
     * @dev Returns the downcasted uint96 from uint256, reverting on
     * overflow (when the input is greater than largest uint96).
     *
     * Counterpart to Solidity's `uint96` operator.
     *
     * Requirements:
     *
     * - input must fit into 96 bits
     */
    function toUint96(uint256 value) internal pure returns (uint96) {
        require(value <= type(uint96).max, "SafeCast: value doesn't fit in 96 bits");
        return uint96(value);
    }

    /**
     * @dev Returns the downcasted uint64 from uint256, reverting on
     * overflow (when the input is greater than largest uint64).
     *
     * Counterpart to Solidity's `uint64` operator.
     *
     * Requirements:
     *
     * - input must fit into 64 bits
     */
    function toUint64(uint256 value) internal pure returns (uint64) {
        require(value <= type(uint64).max, "SafeCast: value doesn't fit in 64 bits");
        return uint64(value);
    }

    /**
     * @dev Returns the downcasted uint32 from uint256, reverting on
     * overflow (when the input is greater than largest uint32).
     *
     * Counterpart to Solidity's `uint32` operator.
     *
     * Requirements:
     *
     * - input must fit into 32 bits
     */
    function toUint32(uint256 value) internal pure returns (uint32) {
        require(value <= type(uint32).max, "SafeCast: value doesn't fit in 32 bits");
        return uint32(value);
    }

    /**
     * @dev Returns the downcasted uint16 from uint256, reverting on
     * overflow (when the input is greater than largest uint16).
     *
     * Counterpart to Solidity's `uint16` operator.
     *
     * Requirements:
     *
     * - input must fit into 16 bits
     */
    function toUint16(uint256 value) internal pure returns (uint16) {
        require(value <= type(uint16).max, "SafeCast: value doesn't fit in 16 bits");
        return uint16(value);
    }

    /**
     * @dev Returns the downcasted uint8 from uint256, reverting on
     * overflow (when the input is greater than largest uint8).
     *
     * Counterpart to Solidity's `uint8` operator.
     *
     * Requirements:
     *
     * - input must fit into 8 bits.
     */
    function toUint8(uint256 value) internal pure returns (uint8) {
        require(value <= type(uint8).max, "SafeCast: value doesn't fit in 8 bits");
        return uint8(value);
    }

    /**
     * @dev Converts a signed int256 into an unsigned uint256.
     *
     * Requirements:
     *
     * - input must be greater than or equal to 0.
     */
    function toUint256(int256 value) internal pure returns (uint256) {
        require(value >= 0, "SafeCast: value must be positive");
        return uint256(value);
    }

    /**
     * @dev Returns the downcasted int128 from int256, reverting on
     * overflow (when the input is less than smallest int128 or
     * greater than largest int128).
     *
     * Counterpart to Solidity's `int128` operator.
     *
     * Requirements:
     *
     * - input must fit into 128 bits
     *
     * _Available since v3.1._
     */
    function toInt128(int256 value) internal pure returns (int128) {
        require(value >= type(int128).min && value <= type(int128).max, "SafeCast: value doesn't fit in 128 bits");
        return int128(value);
    }

    /**
     * @dev Returns the downcasted int64 from int256, reverting on
     * overflow (when the input is less than smallest int64 or
     * greater than largest int64).
     *
     * Counterpart to Solidity's `int64` operator.
     *
     * Requirements:
     *
     * - input must fit into 64 bits
     *
     * _Available since v3.1._
     */
    function toInt64(int256 value) internal pure returns (int64) {
        require(value >= type(int64).min && value <= type(int64).max, "SafeCast: value doesn't fit in 64 bits");
        return int64(value);
    }

    /**
     * @dev Returns the downcasted int32 from int256, reverting on
     * overflow (when the input is less than smallest int32 or
     * greater than largest int32).
     *
     * Counterpart to Solidity's `int32` operator.
     *
     * Requirements:
     *
     * - input must fit into 32 bits
     *
     * _Available since v3.1._
     */
    function toInt32(int256 value) internal pure returns (int32) {
        require(value >= type(int32).min && value <= type(int32).max, "SafeCast: value doesn't fit in 32 bits");
        return int32(value);
    }

    /**
     * @dev Returns the downcasted int16 from int256, reverting on
     * overflow (when the input is less than smallest int16 or
     * greater than largest int16).
     *
     * Counterpart to Solidity's `int16` operator.
     *
     * Requirements:
     *
     * - input must fit into 16 bits
     *
     * _Available since v3.1._
     */
    function toInt16(int256 value) internal pure returns (int16) {
        require(value >= type(int16).min && value <= type(int16).max, "SafeCast: value doesn't fit in 16 bits");
        return int16(value);
    }

    /**
     * @dev Returns the downcasted int8 from int256, reverting on
     * overflow (when the input is less than smallest int8 or
     * greater than largest int8).
     *
     * Counterpart to Solidity's `int8` operator.
     *
     * Requirements:
     *
     * - input must fit into 8 bits.
     *
     * _Available since v3.1._
     */
    function toInt8(int256 value) internal pure returns (int8) {
        require(value >= type(int8).min && value <= type(int8).max, "SafeCast: value doesn't fit in 8 bits");
        return int8(value);
    }

    /**
     * @dev Converts an unsigned uint256 into a signed int256.
     *
     * Requirements:
     *
     * - input must be less than or equal to maxInt256.
     */
    function toInt256(uint256 value) internal pure returns (int256) {
        // Note: Unsafe cast below is okay because `type(int256).max` is guaranteed to be positive
        require(value <= uint256(type(int256).max), "SafeCast: value doesn't fit in an int256");
        return int256(value);
    }
}

/**
 * @dev Standard math utilities missing in the Solidity language.
 */
library Math128 {
    /**
     * @dev Returns the largest of two numbers.
     */
    function max(int128 a, int128 b) internal pure returns (int128) {
        return a >= b ? a : b;
    }

    /**
     * @dev Returns the smallest of two numbers.
     */
    function min(int128 a, int128 b) internal pure returns (int128) {
        return a < b ? a : b;
    }

    /**
     * @dev Returns the smallest of two numbers.
     */
    function min(uint256 a, uint256 b) internal pure returns (uint256) {
        return a < b ? a : b;
    }
}

struct Point {
    int128 bias; // Voting weight
    int128 slope; // Multiplier factor to get voting weight at a given time
    uint256 timestamp;
    uint256 blockNumber;
}

interface IVSELF {
    function deposit(
        address _user,
        uint256 _amount,
        uint256 _lockDuration
    ) external;

    function withdraw(address _user) external;

    /// @dev Return the max epoch of the given "_user"
    function userPointEpoch(address _user) external view returns (uint256);

    /// @dev Return the max global epoch
    function epoch() external view returns (uint256);

    /// @dev Return the recorded point for _user at specific _epoch
    function userPointHistory(address _user, uint256 _epoch) external view returns (Point memory);

    /// @dev Return the recorded global point at specific _epoch
    function pointHistory(uint256 _epoch) external view returns (Point memory);

    /// @dev Trigger global check point
    function checkpoint() external;
}

interface IRevenueSharingPoolFactory {
    function parameters()
        external
        view
        returns (
            address VSELF,
            uint256 startTime,
            address rewardToken,
            address emergencyReturn,
            address owner
        );
}

contract RevenueSharingPool is Ownable, ReentrancyGuard {
    using SafeERC20 for IERC20;

    /// @dev Events
    event LogSetCanCheckpointToken(bool _toggleFlag);
    event LogFeed(uint256 _amount);
    event LogCheckpointToken(uint256 _timestamp, uint256 _tokens);
    event LogClaimed(address indexed _recipient, uint256 _amount, uint256 _claimEpoch, uint256 _maxEpoch);
    event LogKilled();
    event LogSetWhitelistedCheckpointCallers(address indexed _caller, address indexed _address, bool _ok);

    /// @dev Time-related constants
    uint256 public constant WEEK = 1 weeks;
    uint256 public constant TOKEN_CHECKPOINT_DEADLINE = 1 days;

    uint256 public startWeekCursor;
    uint256 public weekCursor;
    mapping(address => uint256) public weekCursorOf;
    mapping(address => uint256) public userEpochOf;

    uint256 public lastTokenTimestamp;
    mapping(uint256 => uint256) public tokensPerWeek;

    address public VSELF;
    IERC20 public rewardToken;
    uint256 public lastTokenBalance;

    /// @dev VSELF supply at week bounds
    mapping(uint256 => uint256) public totalSupplyAt;

    bool public canCheckpointToken;

    /// @dev address to get token when contract is emergency stop
    bool public isKilled;
    address public emergencyReturn;

    /// @dev list of whitelist checkpoint callers
    mapping(address => bool) public whitelistedCheckpointCallers;

    /// @notice constructor
    constructor() {
        (
            address _VSELF,
            uint256 _startTime,
            address _rewardToken,
            address _emergencyReturn,
            address owner
        ) = IRevenueSharingPoolFactory(msg.sender).parameters();
        uint256 _startTimeFloorWeek = _timestampToFloorWeek(_startTime);
        startWeekCursor = _startTimeFloorWeek;
        lastTokenTimestamp = _startTimeFloorWeek;
        weekCursor = _startTimeFloorWeek;
        rewardToken = IERC20(_rewardToken);
        VSELF = _VSELF;
        emergencyReturn = _emergencyReturn;

        _transferOwnership(owner);
    }

    modifier onlyLive() {
        require(!isKilled, "killed");
        _;
    }

    /// @notice Get VSELF balance of "_user" at "_timstamp"
    /// @param _user The user address
    /// @param _timestamp The timestamp to get user's balance
    function balanceOfAt(address _user, uint256 _timestamp) external view returns (uint256) {
        uint256 _maxUserEpoch = IVSELF(VSELF).userPointEpoch(_user);
        if (_maxUserEpoch == 0) {
            return 0;
        }

        uint256 _epoch = _findTimestampUserEpoch(_user, _timestamp, _maxUserEpoch);
        Point memory _point = IVSELF(VSELF).userPointHistory(_user, _epoch);
        int128 _bias = _point.bias - _point.slope * SafeCast.toInt128(int256(_timestamp - _point.timestamp));
        if (_bias < 0) {
            return 0;
        }
        return SafeCast.toUint256(_bias);
    }

    /// @notice Record token distribution checkpoint
    function _checkpointToken() internal {
        // Find out how many tokens to be distributed
        uint256 _rewardTokenBalance = rewardToken.balanceOf(address(this));
        uint256 _toDistribute = _rewardTokenBalance - lastTokenBalance;
        lastTokenBalance = _rewardTokenBalance;

        // Prepare and update time-related variables
        // 1. Setup _timeCursor to be the "lastTokenTimestamp"
        // 2. Find out how long from previous checkpoint
        // 3. Setup iterable cursor
        // 4. Update lastTokenTimestamp to be block.timestamp
        uint256 _timeCursor = lastTokenTimestamp;
        uint256 _deltaSinceLastTimestamp = block.timestamp - _timeCursor;
        uint256 _thisWeekCursor = _timestampToFloorWeek(_timeCursor);
        uint256 _nextWeekCursor = 0;
        lastTokenTimestamp = block.timestamp;

        // Iterate through weeks to filled out missing tokensPerWeek (if any)
        for (uint256 _i = 0; _i < 52; _i++) {
            _nextWeekCursor = _thisWeekCursor + WEEK;

            // if block.timestamp < _nextWeekCursor, means _nextWeekCursor goes
            // beyond the actual block.timestamp, hence it is the last iteration
            // to fill out tokensPerWeek
            if (block.timestamp < _nextWeekCursor) {
                if (_deltaSinceLastTimestamp == 0 && block.timestamp == _timeCursor) {
                    tokensPerWeek[_thisWeekCursor] = tokensPerWeek[_thisWeekCursor] + _toDistribute;
                } else {
                    tokensPerWeek[_thisWeekCursor] =
                        tokensPerWeek[_thisWeekCursor] +
                        ((_toDistribute * (block.timestamp - _timeCursor)) / _deltaSinceLastTimestamp);
                }
                break;
            } else {
                if (_deltaSinceLastTimestamp == 0 && _nextWeekCursor == _timeCursor) {
                    tokensPerWeek[_thisWeekCursor] = tokensPerWeek[_thisWeekCursor] + _toDistribute;
                } else {
                    tokensPerWeek[_thisWeekCursor] =
                        tokensPerWeek[_thisWeekCursor] +
                        ((_toDistribute * (_nextWeekCursor - _timeCursor)) / _deltaSinceLastTimestamp);
                }
            }
            _timeCursor = _nextWeekCursor;
            _thisWeekCursor = _nextWeekCursor;
        }

        emit LogCheckpointToken(block.timestamp, _toDistribute);
    }

    /// @notice Update token checkpoint
    /// @dev Calculate the total token to be distributed in a given week.
    /// At launch can only be called by owner, after launch can be called
    /// by anyone if block.timestamp > lastTokenTime + TOKEN_CHECKPOINT_DEADLINE
    function checkpointToken() external nonReentrant {
        require(
            msg.sender == owner() ||
                whitelistedCheckpointCallers[msg.sender] ||
                (canCheckpointToken && (block.timestamp > lastTokenTimestamp + TOKEN_CHECKPOINT_DEADLINE)),
            "!allow"
        );
        _checkpointToken();
    }

    /// @notice Record VSELF total supply for each week
    function _checkpointTotalSupply() internal {
        uint256 _weekCursor = weekCursor;
        uint256 _roundedTimestamp = _timestampToFloorWeek(block.timestamp);

        IVSELF(VSELF).checkpoint();

        for (uint256 _i = 0; _i < 52; _i++) {
            if (_weekCursor > _roundedTimestamp) {
                break;
            } else {
                uint256 _epoch = _findTimestampEpoch(_weekCursor);
                Point memory _point = IVSELF(VSELF).pointHistory(_epoch);
                int128 _timeDelta = 0;
                if (_weekCursor > _point.timestamp) {
                    _timeDelta = SafeCast.toInt128(int256(_weekCursor - _point.timestamp));
                }
                int128 _bias = _point.bias - _point.slope * _timeDelta;
                if (_bias < 0) {
                    totalSupplyAt[_weekCursor] = 0;
                } else {
                    totalSupplyAt[_weekCursor] = SafeCast.toUint256(_bias);
                }
            }
            _weekCursor = _weekCursor + WEEK;
        }

        weekCursor = _weekCursor;
    }

    /// @notice Update VSELF total supply checkpoint
    /// @dev This function can be called independently or at the first claim of
    /// the new epoch week.
    function checkpointTotalSupply() external nonReentrant {
        _checkpointTotalSupply();
    }

    /// @notice Claim rewardToken
    /// @dev Perform claim rewardToken
    function _claim(address _user, uint256 _maxClaimTimestamp) internal returns (uint256) {
        uint256 _userEpoch = 0;
        uint256 _toDistribute = 0;

        uint256 _maxUserEpoch = IVSELF(VSELF).userPointEpoch(_user);
        uint256 _startWeekCursor = startWeekCursor;

        // _maxUserEpoch = 0, meaning no lock.
        // Hence, no yield for _user
        if (_maxUserEpoch == 0) {
            return 0;
        }

        uint256 _userWeekCursor = weekCursorOf[_user];
        if (_userWeekCursor == 0) {
            // if _user has no _userWeekCursor with GrassHouse yet
            // then we need to perform binary search
            _userEpoch = _findTimestampUserEpoch(_user, _startWeekCursor, _maxUserEpoch);
        } else {
            // else, _user must has epoch with GrassHouse already
            _userEpoch = userEpochOf[_user];
        }

        if (_userEpoch == 0) {
            _userEpoch = 1;
        }

        Point memory _userPoint = IVSELF(VSELF).userPointHistory(_user, _userEpoch);

        if (_userWeekCursor == 0) {
            _userWeekCursor = ((_userPoint.timestamp + WEEK - 1) / WEEK) * WEEK;
        }

        // _userWeekCursor is already at/beyond _maxClaimTimestamp
        // meaning nothing to be claimed for this user.
        // This can be:
        // 1) User just lock their SELF less than 1 week
        // 2) User already claimed their rewards
        if (_userWeekCursor >= _maxClaimTimestamp) {
            return 0;
        }

        // Handle when user lock SELF before Grasshouse started
        // by assign _userWeekCursor to Grasshouse's _startWeekCursor
        if (_userWeekCursor < _startWeekCursor) {
            _userWeekCursor = _startWeekCursor;
        }

        Point memory _prevUserPoint = Point({bias: 0, slope: 0, timestamp: 0, blockNumber: 0});

        // Go through weeks
        for (uint256 _i = 0; _i < 52; _i++) {
            // If _userWeekCursor is iterated to be at/beyond _maxClaimTimestamp
            // This means we went through all weeks that user subject to claim rewards already
            if (_userWeekCursor >= _maxClaimTimestamp) {
                break;
            }
            // Move to the new epoch if need to,
            // else calculate rewards that user should get.
            if (_userWeekCursor >= _userPoint.timestamp && _userEpoch <= _maxUserEpoch) {
                _userEpoch = _userEpoch + 1;
                _prevUserPoint = Point({
                    bias: _userPoint.bias,
                    slope: _userPoint.slope,
                    timestamp: _userPoint.timestamp,
                    blockNumber: _userPoint.blockNumber
                });
                // When _userEpoch goes beyond _maxUserEpoch then there is no more Point,
                // else take _userEpoch as a new Point
                if (_userEpoch > _maxUserEpoch) {
                    _userPoint = Point({bias: 0, slope: 0, timestamp: 0, blockNumber: 0});
                } else {
                    _userPoint = IVSELF(VSELF).userPointHistory(_user, _userEpoch);
                }
            } else {
                int128 _timeDelta = SafeCast.toInt128(int256(_userWeekCursor - _prevUserPoint.timestamp));
                uint256 _balanceOf = SafeCast.toUint256(
                    Math128.max(_prevUserPoint.bias - _timeDelta * _prevUserPoint.slope, 0)
                );
                if (_balanceOf == 0 && _userEpoch > _maxUserEpoch) {
                    break;
                }
                if (_balanceOf > 0) {
                    _toDistribute =
                        _toDistribute +
                        (_balanceOf * tokensPerWeek[_userWeekCursor]) /
                        totalSupplyAt[_userWeekCursor];
                }
                _userWeekCursor = _userWeekCursor + WEEK;
            }
        }

        _userEpoch = Math128.min(_maxUserEpoch, _userEpoch - 1);
        userEpochOf[_user] = _userEpoch;
        weekCursorOf[_user] = _userWeekCursor;

        emit LogClaimed(_user, _toDistribute, _userEpoch, _maxUserEpoch);

        return _toDistribute;
    }

    /// @notice Claim rewardToken for "_user"
    /// @param _user The address to claim rewards for
    function claim(address _user) external nonReentrant onlyLive returns (uint256) {
        if (block.timestamp >= weekCursor) _checkpointTotalSupply();
        uint256 _lastTokenTimestamp = lastTokenTimestamp;

        if (canCheckpointToken && (block.timestamp > _lastTokenTimestamp + TOKEN_CHECKPOINT_DEADLINE)) {
            _checkpointToken();
            _lastTokenTimestamp = block.timestamp;
        }

        _lastTokenTimestamp = _timestampToFloorWeek(_lastTokenTimestamp);

        uint256 _amount = _claim(_user, _lastTokenTimestamp);
        if (_amount != 0) {
            lastTokenBalance = lastTokenBalance - _amount;
            rewardToken.safeTransfer(_user, _amount);
        }

        return _amount;
    }

    /// @notice Claim rewardToken for multiple users
    /// @param _users The array of addresses to claim reward for
    function claimMany(address[] calldata _users) external nonReentrant onlyLive returns (bool) {
        require(_users.length <= 20, "!over 20 users");

        if (block.timestamp >= weekCursor) _checkpointTotalSupply();

        uint256 _lastTokenTimestamp = lastTokenTimestamp;

        if (canCheckpointToken && (block.timestamp > _lastTokenTimestamp + TOKEN_CHECKPOINT_DEADLINE)) {
            _checkpointToken();
            _lastTokenTimestamp = block.timestamp;
        }

        _lastTokenTimestamp = _timestampToFloorWeek(_lastTokenTimestamp);
        uint256 _total = 0;

        for (uint256 i = 0; i < _users.length; i++) {
            require(_users[i] != address(0), "bad user");

            uint256 _amount = _claim(_users[i], _lastTokenTimestamp);
            if (_amount != 0) {
                rewardToken.safeTransfer(_users[i], _amount);
                _total = _total + _amount;
            }
        }

        if (_total != 0) {
            lastTokenBalance = lastTokenBalance - _total;
        }

        return true;
    }

    /// @notice Receive rewardTokens into the contract and trigger token checkpoint
    function feed(uint256 _amount) external nonReentrant onlyLive returns (bool) {
        rewardToken.safeTransferFrom(msg.sender, address(this), _amount);

        if (canCheckpointToken && (block.timestamp > lastTokenTimestamp + TOKEN_CHECKPOINT_DEADLINE)) {
            _checkpointToken();
        }

        emit LogFeed(_amount);

        return true;
    }

    /// @notice Do Binary Search to find out epoch from timestamp
    /// @param _timestamp Timestamp to find epoch
    function _findTimestampEpoch(uint256 _timestamp) internal view returns (uint256) {
        uint256 _min = 0;
        uint256 _max = IVSELF(VSELF).epoch();
        // Loop for 128 times -> enough for 128-bit numbers
        for (uint256 i = 0; i < 128; i++) {
            if (_min >= _max) {
                break;
            }
            uint256 _mid = (_min + _max + 1) / 2;
            Point memory _point = IVSELF(VSELF).pointHistory(_mid);
            if (_point.timestamp <= _timestamp) {
                _min = _mid;
            } else {
                _max = _mid - 1;
            }
        }
        return _min;
    }

    /// @notice Perform binary search to find out user's epoch from the given timestamp
    /// @param _user The user address
    /// @param _timestamp The timestamp that you wish to find out epoch
    /// @param _maxUserEpoch Max epoch to find out the timestamp
    function _findTimestampUserEpoch(
        address _user,
        uint256 _timestamp,
        uint256 _maxUserEpoch
    ) internal view returns (uint256) {
        uint256 _min = 0;
        uint256 _max = _maxUserEpoch;
        for (uint256 i = 0; i < 128; i++) {
            if (_min >= _max) {
                break;
            }
            uint256 _mid = (_min + _max + 1) / 2;
            Point memory _point = IVSELF(VSELF).userPointHistory(_user, _mid);
            if (_point.timestamp <= _timestamp) {
                _min = _mid;
            } else {
                _max = _mid - 1;
            }
        }
        return _min;
    }

    function kill() external onlyOwner {
        isKilled = true;
        rewardToken.safeTransfer(emergencyReturn, rewardToken.balanceOf(address(this)));

        emit LogKilled();
    }

    /// @notice Set canCheckpointToken to allow random callers to call checkpointToken
    /// @param _newCanCheckpointToken The new canCheckpointToken flag
    function setCanCheckpointToken(bool _newCanCheckpointToken) external onlyOwner {
        canCheckpointToken = _newCanCheckpointToken;
        emit LogSetCanCheckpointToken(_newCanCheckpointToken);
    }

    /// @notice Round off random timestamp to week
    /// @param _timestamp The timestamp to be rounded off
    function _timestampToFloorWeek(uint256 _timestamp) internal pure returns (uint256) {
        return (_timestamp / WEEK) * WEEK;
    }

    /// @notice Inject rewardToken into the contract
    /// @param _timestamp The timestamp of the rewardToken to be distributed
    /// @param _amount The amount of rewardToken to be distributed
    function injectReward(uint256 _timestamp, uint256 _amount) external onlyOwner {
        rewardToken.safeTransferFrom(msg.sender, address(this), _amount);
        lastTokenBalance += _amount;
        tokensPerWeek[_timestampToFloorWeek(_timestamp)] = _amount;
    }

    /// @notice Set whitelisted checkpoint callers.
    /// @dev Must only be called by owner.
    /// @param _callers addresses to be whitelisted.
    /// @param _ok The new ok flag for callers.
    function setWhitelistedCheckpointCallers(address[] calldata _callers, bool _ok) external onlyOwner {
        for (uint256 _idx = 0; _idx < _callers.length; _idx++) {
            whitelistedCheckpointCallers[_callers[_idx]] = _ok;
            emit LogSetWhitelistedCheckpointCallers(msg.sender, _callers[_idx], _ok);
        }
    }
}

contract RevenueSharingPoolFactory is Ownable {
    struct Parameters {
        address VSELF;
        uint256 startTime;
        address rewardToken;
        address emergencyReturn;
        address owner;
    }

    Parameters public parameters;

    address public VSELF;

    uint256 public poolLength;

    mapping(uint256 => address) public pools;

    mapping(address => address) public rewardTokenPools;

    uint256 public constant MAX_STARTTIME_DURATION = 4 weeks;

    event NewRevenueSharingPool(address indexed pool, address indexed rewardToken, uint256 startTime);

    constructor(address _VSELF) {
        VSELF = _VSELF;
    }

    /// @dev Deploys a LmPool
    /// @param _startTime Time to be started
    /// @param _rewardToken The token to be distributed
    /// @param _emergencyReturn The address to return token when emergency stop
    function deploy(
        uint256 _startTime,
        address _rewardToken,
        address _emergencyReturn
    ) external onlyOwner returns (address pool) {
        require(rewardTokenPools[_rewardToken] == address(0), "Already created pool");
        require(_startTime <= block.timestamp + MAX_STARTTIME_DURATION, "Invalid startTime");
        parameters = Parameters({
            VSELF: VSELF,
            startTime: _startTime,
            rewardToken: _rewardToken,
            emergencyReturn: _emergencyReturn,
            owner: msg.sender
        });

        pool = address(
            new RevenueSharingPool{
                salt: keccak256(abi.encode(_rewardToken, _emergencyReturn, _startTime, block.timestamp))
            }()
        );

        delete parameters;

        pools[poolLength] = pool;
        poolLength++;
        rewardTokenPools[_rewardToken] = pool;

        emit NewRevenueSharingPool(pool, _rewardToken, _startTime);
    }
}