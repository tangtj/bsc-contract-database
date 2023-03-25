// SPDX-License-Identifier: MIT

pragma solidity 0.6.12;

/*
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with GSN meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
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
    constructor() internal {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
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
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
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
    function transferFrom(
        address sender,
        address recipient,
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
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        uint256 c = a + b;
        if (c < a) return (false, 0);
        return (true, c);
    }

    /**
     * @dev Returns the substraction of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        if (b > a) return (false, 0);
        return (true, a - b);
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) return (true, 0);
        uint256 c = a * b;
        if (c / a != b) return (false, 0);
        return (true, c);
    }

    /**
     * @dev Returns the division of two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        if (b == 0) return (false, 0);
        return (true, a / b);
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        if (b == 0) return (false, 0);
        return (true, a % b);
    }

    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     *
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
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        return a - b;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     *
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) return 0;
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        return a / b;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: modulo by zero");
        return a % b;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {trySub}.
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        return a - b;
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting with custom message on
     * division by zero. The result is rounded towards zero.
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {tryDiv}.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        return a / b;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting with custom message when dividing by zero.
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {tryMod}.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        return a % b;
    }
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
     */
    function isContract(address account) internal view returns (bool) {
        // This method relies on extcodesize, which returns 0 for contracts in
        // construction, since the code is only stored at the end of the
        // constructor execution.

        uint256 size;
        // solhint-disable-next-line no-inline-assembly
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
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

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    /**
     * @dev Performs a Solidity function call using a low level `call`. A
     * plain`call` is an unsafe replacement for a function call: use this
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

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return _verifyCallResult(success, returndata, errorMessage);
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

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.staticcall(data);
        return _verifyCallResult(success, returndata, errorMessage);
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

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return _verifyCallResult(success, returndata, errorMessage);
    }

    function _verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) private pure returns (bytes memory) {
        if (success) {
            return returndata;
        } else {
            // Look for revert reason and bubble it up if present
            if (returndata.length > 0) {
                // The easiest way to bubble the revert reason is using memory via assembly

                // solhint-disable-next-line no-inline-assembly
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
    using SafeMath for uint256;
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
        // solhint-disable-next-line max-line-length
        require((value == 0) || (token.allowance(address(this), spender) == 0), "SafeERC20: approve from non-zero to non-zero allowance");
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        uint256 newAllowance = token.allowance(address(this), spender).add(value);
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        uint256 newAllowance = token.allowance(address(this), spender).sub(value, "SafeERC20: decreased allowance below zero");
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
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
            // solhint-disable-next-line max-line-length
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

    constructor() internal {
        _status = _NOT_ENTERED;
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
 * @dev Contract module which allows children to implement an emergency stop
 * mechanism that can be triggered by an authorized account.
 *
 * This module is used through inheritance. It will make available the
 * modifiers `whenNotPaused` and `whenPaused`, which can be applied to
 * the functions of your contract. Note that they will not be pausable by
 * simply including this module, only once the modifiers are put in place.
 */
abstract contract Pausable is Context {
    /**
     * @dev Emitted when the pause is triggered by `account`.
     */
    event Paused(address account);

    /**
     * @dev Emitted when the pause is lifted by `account`.
     */
    event Unpaused(address account);

    bool private _paused;

    /**
     * @dev Initializes the contract in unpaused state.
     */
    constructor() internal {
        _paused = false;
    }

    /**
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        require(!paused(), "Pausable: paused");
        _;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is paused.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    modifier whenPaused() {
        require(paused(), "Pausable: not paused");
        _;
    }

    /**
     * @dev Triggers stopped state.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }

    /**
     * @dev Returns to normal state.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
    }
}

interface IPancakeswapFarm {
    function poolLength() external view returns (uint256);

    function userInfo() external view returns (uint256);

    // Return reward multiplier over the given _from to _to block.
    function getMultiplier(uint256 _from, uint256 _to) external view returns (uint256);

    // View function to see pending CAKEs on frontend.
    function pendingCake(uint256 _pid, address _user) external view returns (uint256);

    function pendingMDO(uint256 _pid, address _user) external view returns (uint256);

    function pendingShare(uint256 _pid, address _user) external view returns (uint256);

    function pendingBDO(uint256 _pid, address _user) external view returns (uint256);

    function pendingRewards(uint256 _pid, address _user) external view returns (uint256);

    function pendingReward(uint256 _pid, address _user) external view returns (uint256);

    function pendingBELT(uint256 _pid, address _user) external view returns (uint256);

    function pendingBusd(uint256 _pid, address _user) external view returns (uint256);

    // Deposit LP tokens to MasterChef for CAKE allocation.
    function deposit(uint256 _pid, uint256 _amount) external;

    // Withdraw LP tokens from MasterChef.
    function withdraw(uint256 _pid, uint256 _amount) external;

    // Stake CAKE tokens to MasterChef
    function enterStaking(uint256 _amount) external;

    // Withdraw CAKE tokens from STAKING.
    function leaveStaking(uint256 _amount) external;

    // Withdraw without caring about rewards. EMERGENCY ONLY.
    function emergencyWithdraw(uint256 _pid) external;
}

interface ILockedBdex {
    function canUnlockAmount(address _account) external view returns (uint256);

    function claimUnlocked() external;

    function unlockAll() external;
}

interface IValueLiquidFormula {
    function getAmountsOut(
        address tokenIn,
        address tokenOut,
        uint256 amountIn,
        address[] calldata path
    ) external view returns (uint256[] memory amounts);
}

interface IValueLiquidRouter {
    function swapExactTokensForTokens(
        address tokenIn,
        address tokenOut,
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function addLiquidity(
        address pair,
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    )
        external
        returns (
            uint256 amountA,
            uint256 amountB,
            uint256 liquidity
        );
}

interface ITreasury {
    function notifyExternalReward(uint256 _amount) external;
}

interface IStrategy {
    // Total want tokens managed by strategy
    function wantLockedTotal() external view returns (uint256);

    // Sum of all shares of users to wantLockedTotal
    function sharesTotal() external view returns (uint256);

    function wantAddress() external view returns (address);

    function token0Address() external view returns (address);

    function token1Address() external view returns (address);

    function earnedAddress() external view returns (address);

    function ratio0() external view returns (uint256);

    function ratio1() external view returns (uint256);

    function getPricePerFullShare() external view returns (uint256);

    // Main want token compounding function
    function earn() external;

    // Transfer want tokens autoFarm -> strategy
    function deposit(address _userAddress, uint256 _wantAmt) external returns (uint256);

    // Transfer want tokens strategy -> autoFarm
    function withdraw(address _userAddress, uint256 _wantAmt) external returns (uint256);

    function migrateFrom(
        address _oldStrategy,
        uint256 _oldWantLockedTotal,
        uint256 _oldSharesTotal
    ) external;

    function inCaseTokensGetStuck(
        address _token,
        uint256 _amount,
        address _to
    ) external;
}

contract BvaultsStrategyMigratable is Ownable, ReentrancyGuard, Pausable {
    // Maximises yields in pancakeswap

    using SafeMath for uint256;
    using SafeERC20 for IERC20;

    bool public isAutoComp; // this vault is purely for staking. eg. WBNB-BDO staking vault.

    address public farmContractAddress; // address of farm, eg, PCS, Thugs etc.
    uint256 public pid; // pid of pool in farmContractAddress
    address public bdex = address(0x7E0F01918D92b2750bbb18fcebeEDD5B94ebB867);
    address public xbdex = address(0x4CF73a6F72B8093b96264a794064372edb15508e);
    uint256 public startReleaseBlock = 9864000;
    bool public checkForUnlockReward = true;
    address public wantAddress;
    address public token0Address;
    address public token1Address;
    address public earnedAddress;
    uint256 public ratio0 = 5000; // 50%
    uint256 public ratio1 = 5000; // 50%

    address public vswapRouterAddress = address(0xC6747954a9B3A074d8E4168B444d7F397FeE76AA); // vSwap Router
    address public vswapFormula = address(0xCB9f345c32e2216e5F13E1A816059C6435C92038); // vSwap Formula
    mapping(address => mapping(address => address[])) public vswapPaths;

    address public constant wbnbAddress = address(0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c);
    address public constant busdAddress = address(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56);

    address public operator;
    address public strategist;
    address public timelock = address(0x2ad335E18C97d18561dd1a0C1447b1018b81f213); // 6h timelock
    bool public notPublic = false; // allow public to call earn() function

    uint256 public lastEarnBlock = 0;
    uint256 public wantLockedTotal = 0;
    uint256 public sharesTotal = 0;

    uint256 public controllerFee = 300;
    uint256 public constant controllerFeeMax = 10000; // 100 = 1%
    uint256 public constant controllerFeeUL = 300;

    uint256 public constant buyBackRateMax = 10000; // 100 = 1%
    uint256 public constant buyBackRateUL = 800; // 8%

    uint256 public buyBackRate1 = 300; // 3%
    address public buyBackToken1 = address(0x81859801b01764D4f0Fa5E64729f5a6C3b91435b); // BFI
    address public buyBackAddress1 = address(0xaaaf5d9923Be358Ea0b6205Ef2A3B929D460Ac7A); // to BFI Staking Pool

    uint256 public buyBackRate2 = 0; // 2%
    address public buyBackToken2 = address(0x190b589cf9Fb8DDEabBFeae36a813FFb2A702454); // BDO
    address public buyBackAddress2 = address(0x5D42dc503763dD1e7A1B510b055150Cc5754656B); // to TreasuryProxy (to split the profit)

    uint256 public entranceFeeFactor = 10000; // 0% entrance fee (goes to pool + prevents front-running)
    uint256 public constant entranceFeeFactorMax = 10000; // 100 = 1%
    uint256 public constant entranceFeeFactorLL = 9950; // 0.5% is the max entrance fee settable. LL = lowerlimit

    event Deposit(uint256 amount);
    event Withdraw(uint256 amount);
    event Farm(uint256 amount);
    event Compound(address token0Address, uint256 token0Amt, address token1Address, uint256 token1Amt);
    event Earned(address earnedAddress, uint256 earnedAmt);
    event BuyBack(address earnedAddress, address buyBackToken, uint256 earnedAmt, uint256 buyBackAmt, address receiver);
    event DistributeFee(address earnedAddress, uint256 fee, address receiver);
    event ConvertDustToEarned(address tokenAddress, address earnedAddress, uint256 tokenAmt);
    event InCaseTokensGetStuck(address tokenAddress, uint256 tokenAmt, address receiver);
    event ExecuteTransaction(address indexed target, uint256 value, string signature, bytes data);

    // _controller:  BvaultsBank
    // _buyBackToken1Info[]: buyBackToken1, buyBackAddress1
    // _buyBackToken2Info[]: buyBackToken2, buyBackAddress2
    constructor(
        address _controller,
        bool _isAutoComp,
        address _farmContractAddress,
        uint256 _pid,
        address _wantAddress,
        address _earnedAddress,
        address _vswapRouterAddress,
        address _vswapFormula,
        address[] memory _buyBackToken1Info,
        address[] memory _buyBackToken2Info,
        address _token0,
        address _token1,
        uint256 _ratio0
    ) public {
        operator = msg.sender;
        strategist = msg.sender;
        // to call earn if public not allowed

        isAutoComp = _isAutoComp;
        wantAddress = _wantAddress;

        if (_vswapRouterAddress != address(0)) vswapRouterAddress = _vswapRouterAddress;
        if (_vswapFormula != address(0)) vswapFormula = _vswapFormula;

        buyBackToken1 = _buyBackToken1Info[0];
        buyBackAddress1 = _buyBackToken1Info[1];
        buyBackToken2 = _buyBackToken2Info[0];
        buyBackAddress2 = _buyBackToken2Info[1];

        if (isAutoComp) {
            token0Address = _token0;
            token1Address = _token1;
            if (_ratio0 != 0) {
                ratio0 = _ratio0;
                ratio1 = uint256(10000).sub(ratio0);
            }

            farmContractAddress = _farmContractAddress;
            pid = _pid;
            earnedAddress = _earnedAddress;
        }

        transferOwnership(_controller);
    }

    modifier onlyOperator() {
        require(operator == msg.sender, "BvaultsStrategy: caller is not the operator");
        _;
    }

    modifier onlyStrategist() {
        require(strategist == msg.sender || operator == msg.sender, "BvaultsStrategy: caller is not the strategist");
        _;
    }

    modifier onlyTimelock() {
        require(timelock == msg.sender, "BvaultsStrategy: caller is not timelock");
        _;
    }

    function isAuthorised(address _account) public view returns (bool) {
        return (_account == operator) || (msg.sender == strategist) || (msg.sender == timelock);
    }

    function getPricePerFullShare() external view returns (uint256) {
        return (sharesTotal == 0) ? 1e18 : wantLockedTotal.mul(1e18).div(sharesTotal);
    }

    // Receives new deposits from user
    function deposit(address, uint256 _wantAmt) public onlyOwner whenNotPaused returns (uint256) {
        IERC20(wantAddress).safeTransferFrom(address(msg.sender), address(this), _wantAmt);

        uint256 sharesAdded = _wantAmt;
        if (wantLockedTotal > 0 && sharesTotal > 0) {
            sharesAdded = _wantAmt.mul(sharesTotal).mul(entranceFeeFactor).div(wantLockedTotal).div(entranceFeeFactorMax);
        }
        sharesTotal = sharesTotal.add(sharesAdded);

        if (isAutoComp) {
            _farm();
        } else {
            wantLockedTotal = wantLockedTotal.add(_wantAmt);
        }

        emit Deposit(_wantAmt);

        return sharesAdded;
    }

    function farm() public nonReentrant {
        _farm();
    }

    function _farm() internal {
        uint256 wantAmt = IERC20(wantAddress).balanceOf(address(this));
        wantLockedTotal = wantLockedTotal.add(wantAmt);
        IERC20(wantAddress).safeIncreaseAllowance(farmContractAddress, wantAmt);

        IPancakeswapFarm(farmContractAddress).deposit(pid, wantAmt);

        emit Farm(wantAmt);
    }

    function withdraw(address, uint256 _wantAmt) public onlyOwner nonReentrant returns (uint256) {
        require(_wantAmt > 0, "BvaultsStrategy: !_wantAmt");

        if (isAutoComp) {
            IPancakeswapFarm(farmContractAddress).withdraw(pid, _wantAmt);
        }

        uint256 wantAmt = IERC20(wantAddress).balanceOf(address(this));
        if (_wantAmt > wantAmt) {
            _wantAmt = wantAmt;
        }

        if (wantLockedTotal < _wantAmt) {
            _wantAmt = wantLockedTotal;
        }

        uint256 sharesRemoved = _wantAmt.mul(sharesTotal).div(wantLockedTotal);
        if (sharesRemoved > sharesTotal) {
            sharesRemoved = sharesTotal;
        }
        sharesTotal = sharesTotal.sub(sharesRemoved);
        wantLockedTotal = wantLockedTotal.sub(_wantAmt);

        IERC20(wantAddress).safeTransfer(address(msg.sender), _wantAmt);

        emit Withdraw(_wantAmt);

        return sharesRemoved;
    }

    // 1. Harvest farm tokens
    // 2. Converts farm tokens into want tokens
    // 3. Deposits want tokens

    function earn() public whenNotPaused {
        require(isAutoComp, "BvaultsStrategy: !isAutoComp");
        require(!notPublic || isAuthorised(msg.sender), "BvaultsStrategy: !authorised");

        // Harvest farm tokens
        IPancakeswapFarm(farmContractAddress).deposit(pid, 0);

        // Check if there is any unlocked amount
        if (checkForUnlockReward) {
            if (block.number > startReleaseBlock) {
                if (IERC20(xbdex).balanceOf(address(this)) > 0) {
                    ILockedBdex(xbdex).unlockAll();
                }
                if (ILockedBdex(xbdex).canUnlockAmount(address(this)) > 0) {
                    ILockedBdex(xbdex).claimUnlocked();
                }
            }
        }

        // Converts farm tokens into want tokens
        uint256 earnedAmt = IERC20(earnedAddress).balanceOf(address(this));

        emit Earned(earnedAddress, earnedAmt);

        uint256 _distributeFee = distributeFees(earnedAmt);
        uint256 _buyBackAmt1 = buyBack1(earnedAmt);
        uint256 _buyBackAmt2 = buyBack2(earnedAmt);

        earnedAmt = earnedAmt.sub(_distributeFee).sub(_buyBackAmt1).sub(_buyBackAmt2);

        IERC20(earnedAddress).safeIncreaseAllowance(vswapRouterAddress, earnedAmt);

        if (earnedAddress != token0Address) {
            // Swap earned to token0 by ratio0
            _vswapSwapToken(earnedAddress, token0Address, earnedAmt.mul(ratio0).div(10000), address(this));
        }

        if (earnedAddress != token1Address) {
            // Swap earned to token1 by ratio1
            _vswapSwapToken(earnedAddress, token1Address, earnedAmt.mul(ratio1).div(10000), address(this));
        }

        // Get want tokens, ie. add liquidity
        uint256 token0Amt = IERC20(token0Address).balanceOf(address(this));
        uint256 token1Amt = IERC20(token1Address).balanceOf(address(this));
        if (token0Amt > 0 && token1Amt > 0) {
            _vswapAddLiquidity(token0Address, token1Address, token0Amt, token1Amt);
            emit Compound(token0Address, token0Amt, token1Address, token1Amt);
        }

        lastEarnBlock = block.number;

        _farm();
    }

    function buyBack1(uint256 _earnedAmt) internal returns (uint256) {
        if (buyBackRate1 <= 0 || buyBackAddress1 == address(0)) {
            return 0;
        }

        uint256 _buyBackAmt = _earnedAmt.mul(buyBackRate1).div(buyBackRateMax);

        uint256 _pathLength = vswapPaths[earnedAddress][buyBackToken1].length;
        uint256 _before = IERC20(buyBackToken1).balanceOf(buyBackAddress1);
        if (_pathLength > 0 && earnedAddress != buyBackToken1) {
            _vswapSwapToken(earnedAddress, buyBackToken1, _buyBackAmt, buyBackAddress1);
        } else {
            IERC20(earnedAddress).safeTransfer(buyBackAddress1, _buyBackAmt);
        }
        uint256 _after = IERC20(buyBackToken1).balanceOf(buyBackAddress1);
        uint256 _newReward = _after.sub(_before);

        emit BuyBack(earnedAddress, buyBackToken1, _buyBackAmt, _newReward, buyBackAddress1);
        return _buyBackAmt;
    }

    function buyBack2(uint256 _earnedAmt) internal returns (uint256) {
        if (buyBackRate2 <= 0 || buyBackAddress2 == address(0)) {
            return 0;
        }

        uint256 _buyBackAmt = _earnedAmt.mul(buyBackRate2).div(buyBackRateMax);

        uint256 _pathLength = vswapPaths[earnedAddress][buyBackToken2].length;
        uint256 _before = IERC20(buyBackToken2).balanceOf(buyBackAddress2);
        if (_pathLength > 0 && earnedAddress != buyBackToken2) {
            _vswapSwapToken(earnedAddress, buyBackToken2, _buyBackAmt, buyBackAddress2);
        } else {
            IERC20(earnedAddress).safeTransfer(buyBackAddress2, _buyBackAmt);
        }
        uint256 _after = IERC20(buyBackToken2).balanceOf(buyBackAddress2);
        uint256 _newReward = _after.sub(_before);
        if (_newReward > 0) {
            ITreasury(buyBackAddress2).notifyExternalReward(_newReward);
            emit BuyBack(earnedAddress, buyBackToken2, _buyBackAmt, _newReward, buyBackAddress2);
        }

        return _buyBackAmt;
    }

    function distributeFees(uint256 _earnedAmt) internal returns (uint256 _fee) {
        if (_earnedAmt > 0) {
            // Performance fee
            if (controllerFee > 0) {
                _fee = _earnedAmt.mul(controllerFee).div(controllerFeeMax);
                IERC20(earnedAddress).safeTransfer(operator, _fee);
                emit DistributeFee(earnedAddress, _fee, operator);
            }
        }
    }

    function convertDustToEarned() public whenNotPaused {
        require(isAutoComp, "!isAutoComp");

        // Converts dust tokens into earned tokens, which will be reinvested on the next earn().

        // Converts token0 dust (if any) to earned tokens
        uint256 _token0Amt = IERC20(token0Address).balanceOf(address(this));
        if (token0Address != earnedAddress && _token0Amt > 0) {
            _vswapSwapToken(token0Address, earnedAddress, _token0Amt, address(this));
        }

        // Converts token1 dust (if any) to earned tokens
        uint256 _token1Amt = IERC20(token1Address).balanceOf(address(this));
        if (token1Address != earnedAddress && _token1Amt > 0) {
            _vswapSwapToken(token1Address, earnedAddress, _token1Amt, address(this));
        }
    }

    function exchangeRate(
        address _inputToken,
        address _outputToken,
        uint256 _tokenAmount
    ) public view returns (uint256) {
        uint256[] memory amounts = IValueLiquidFormula(vswapFormula).getAmountsOut(_inputToken, _outputToken, _tokenAmount, vswapPaths[_inputToken][_outputToken]);
        return amounts[amounts.length - 1];
    }

    function pendingHarvest() public view returns (uint256) {
        uint256 _earnedBal = IERC20(earnedAddress).balanceOf(address(this));
        return IPancakeswapFarm(farmContractAddress).pendingReward(pid, address(this)).add(_earnedBal);
    }

    function pendingHarvestDollarValue() public view returns (uint256) {
        uint256 _pending = pendingHarvest();
        return (_pending == 0) ? 0 : exchangeRate(earnedAddress, busdAddress, _pending);
    }

    function pause() external onlyOperator {
        _pause();
    }

    function unpause() external onlyOperator {
        _unpause();
    }

    function setOperator(address _operator) external onlyOperator {
        operator = _operator;
    }

    function setStrategist(address _strategist) external onlyOperator {
        strategist = _strategist;
    }

    function setEntranceFeeFactor(uint256 _entranceFeeFactor) external onlyOperator {
        require(_entranceFeeFactor > entranceFeeFactorLL, "BvaultsStrategy: !safe - too low");
        require(_entranceFeeFactor <= entranceFeeFactorMax, "BvaultsStrategy: !safe - too high");
        entranceFeeFactor = _entranceFeeFactor;
    }

    function setControllerFee(uint256 _controllerFee) external onlyOperator {
        require(_controllerFee <= controllerFeeUL, "BvaultsStrategy: too high");
        controllerFee = _controllerFee;
    }

    function setBuyBackRate1(uint256 _buyBackRate1) external onlyOperator {
        require(buyBackRate1 <= buyBackRateUL, "BvaultsStrategy: too high");
        buyBackRate1 = _buyBackRate1;
    }

    function setBuyBackRate2(uint256 _buyBackRate2) external onlyOperator {
        require(buyBackRate2 <= buyBackRateUL, "BvaultsStrategy: too high");
        buyBackRate2 = _buyBackRate2;
    }

    function setBuyBackAddress1(address _buyBackAddress1) external onlyOperator {
        require(buyBackAddress1 != address(0), "zero");
        buyBackAddress1 = _buyBackAddress1;
    }

    function setBuyBackAddress2(address _buyBackAddress2) external onlyOperator {
        require(_buyBackAddress2 != address(0), "zero");
        buyBackAddress2 = _buyBackAddress2;
    }

    function setNotPublic(bool _notPublic) external onlyOperator {
        notPublic = _notPublic;
    }

    /* ========== Pancakeswap & ValueLiquidRouter ========== */

    function setMainPaths(
        address[] memory _earnedToBuyBackToken1Path,
        address[] memory _earnedToBuyBackToken2Path,
        address[] memory _earnedToToken0Path,
        address[] memory _earnedToToken1Path,
        address[] memory _earnedToBusdPath,
        address[] memory _token0ToEarnedPath,
        address[] memory _token1ToEarnedPath
    ) external onlyOperator {
        vswapPaths[earnedAddress][buyBackToken1] = _earnedToBuyBackToken1Path;
        vswapPaths[earnedAddress][buyBackToken2] = _earnedToBuyBackToken2Path;
        vswapPaths[earnedAddress][token0Address] = _earnedToToken0Path;
        vswapPaths[earnedAddress][token1Address] = _earnedToToken1Path;
        vswapPaths[earnedAddress][busdAddress] = _earnedToBusdPath;
        vswapPaths[token0Address][earnedAddress] = _token0ToEarnedPath;
        vswapPaths[token1Address][earnedAddress] = _token1ToEarnedPath;
    }

    function setVswapPaths(
        address _inputToken,
        address _outputToken,
        address[] memory _path
    ) external onlyOperator {
        vswapPaths[_inputToken][_outputToken] = _path;
    }

    function _vswapSwapToken(
        address _inputToken,
        address _outputToken,
        uint256 _amount,
        address to
    ) internal {
        IERC20(_inputToken).safeIncreaseAllowance(vswapRouterAddress, _amount);
        IValueLiquidRouter(vswapRouterAddress).swapExactTokensForTokens(_inputToken, _outputToken, _amount, 1, vswapPaths[_inputToken][_outputToken], to, now.add(1800));
    }

    function _vswapAddLiquidity(
        address _tokenA,
        address _tokenB,
        uint256 _amountADesired,
        uint256 _amountBDesired
    ) internal {
        IERC20(_tokenA).safeIncreaseAllowance(vswapRouterAddress, _amountADesired);
        IERC20(_tokenB).safeIncreaseAllowance(vswapRouterAddress, _amountBDesired);
        IValueLiquidRouter(vswapRouterAddress).addLiquidity(wantAddress, _tokenA, _tokenB, _amountADesired, _amountBDesired, 0, 0, address(this), now.add(1800));
    }

    function setCheckForUnlockReward(bool _checkForUnlockReward) external onlyOperator {
        checkForUnlockReward = _checkForUnlockReward;
    }

    function setStartReleaseBlock(uint256 _startReleaseBlock) external onlyOperator {
        startReleaseBlock = _startReleaseBlock;
    }

    /* ========== EMERGENCY ========== */

    function migrateFrom(
        address _oldStrategy,
        uint256 _oldWantLockedTotal,
        uint256 _oldSharesTotal
    ) external onlyOwner {
        require(wantLockedTotal == 0 && sharesTotal == 0 && _oldSharesTotal > 0, "strategy is not empty");
        require(wantAddress == IStrategy(_oldStrategy).wantAddress(), "!wantAddress");
        require(token0Address == IStrategy(_oldStrategy).token0Address(), "!token0Address");
        require(token1Address == IStrategy(_oldStrategy).token1Address(), "!token1Address");
        require(IERC20(wantAddress).balanceOf(address(this)) == _oldWantLockedTotal, "short of wantLockedTotal");

        sharesTotal = _oldSharesTotal;
        _farm();
    }

    function inCaseTokensGetStuck(
        address _token,
        uint256 _amount,
        address _to
    ) external onlyOperator {
        require(_token != earnedAddress, "earned");
        require(_token != wantAddress, "want");
        IERC20(_token).safeTransfer(_to, _amount);
        emit InCaseTokensGetStuck(_token, _amount, _to);
    }

    function setTimelock(address _timelock) external onlyTimelock {
        timelock = _timelock;
    }

    /**
     * @dev This is from Timelock contract.
     */
    function executeTransaction(
        address target,
        uint256 value,
        string memory signature,
        bytes memory data
    ) external onlyTimelock returns (bytes memory) {
        bytes memory callData;

        if (bytes(signature).length == 0) {
            callData = data;
        } else {
            callData = abi.encodePacked(bytes4(keccak256(bytes(signature))), data);
        }

        // solium-disable-next-line security/no-call-value
        (bool success, bytes memory returnData) = target.call{value: value}(callData);
        require(success, "BvaultsStrategy::executeTransaction: Transaction execution reverted.");

        emit ExecuteTransaction(target, value, signature, data);

        return returnData;
    }
}