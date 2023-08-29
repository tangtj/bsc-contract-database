pragma solidity 0.8.4;


// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)
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

// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/extensions/IERC20Permit.sol)
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

// OpenZeppelin Contracts (last updated v4.9.0) (utils/Address.sol)
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
     * @dev Replacement for Solidity's `transfer`: sends `amount` wei to
     * `recipient`, forwarding all available gas and reverting on errors.
     *
     * https://eips.ethereum.org/EIPS/eip-1884[EIP1884] increases the gas cost
     * of certain opcodes, possibly making contracts go over the 2300 gas limit
     * imposed by `transfer`, making them unable to receive funds via
     * `transfer`. {sendValue} removes this limitation.
     *
     * https://consensys.net/diligence/blog/2019/09/stop-using-soliditys-transfer-now/[Learn more].
     *
     * IMPORTANT: because control is transferred to `recipient`, care must be
     * taken to not create reentrancy vulnerabilities. Consider using
     * {ReentrancyGuard} or the
     * https://solidity.readthedocs.io/en/v0.8.0/security-considerations.html#use-the-checks-effects-interactions-pattern[checks-effects-interactions pattern].
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
        return functionCallWithValue(target, data, 0, "Address: low-level call failed");
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
    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
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
        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
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
        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
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
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
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

    /**
     * @dev Tool to verify that a low level call was successful, and revert if it wasn't, either by bubbling the
     * revert reason or using the provided one.
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
            _revert(returndata, errorMessage);
        }
    }

    function _revert(bytes memory returndata, string memory errorMessage) private pure {
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

// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/utils/SafeERC20.sol)
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
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    /**
     * @dev Transfer `value` amount of `token` from `from` to `to`, spending the approval given by `from` to the
     * calling contract. If `token` returns no value, non-reverting calls are assumed to be successful.
     */
    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    /**
     * @dev Deprecated. This function has issues similar to the ones found in
     * {IERC20-approve}, and its usage is discouraged.
     *
     * Whenever possible, use {safeIncreaseAllowance} and
     * {safeDecreaseAllowance} instead.
     */
    function safeApprove(IERC20 token, address spender, uint256 value) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        require(
            (value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    /**
     * @dev Increase the calling contract's allowance toward `spender` by `value`. If `token` returns no value,
     * non-reverting calls are assumed to be successful.
     */
    function safeIncreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 oldAllowance = token.allowance(address(this), spender);
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, oldAllowance + value));
    }

    /**
     * @dev Decrease the calling contract's allowance toward `spender` by `value`. If `token` returns no value,
     * non-reverting calls are assumed to be successful.
     */
    function safeDecreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        unchecked {
            uint256 oldAllowance = token.allowance(address(this), spender);
            require(oldAllowance >= value, "SafeERC20: decreased allowance below zero");
            _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, oldAllowance - value));
        }
    }

    /**
     * @dev Set the calling contract's allowance toward `spender` to `value`. If `token` returns no value,
     * non-reverting calls are assumed to be successful. Compatible with tokens that require the approval to be set to
     * 0 before setting it to a non-zero value.
     */
    function forceApprove(IERC20 token, address spender, uint256 value) internal {
        bytes memory approvalCall = abi.encodeWithSelector(token.approve.selector, spender, value);

        if (!_callOptionalReturnBool(token, approvalCall)) {
            _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, 0));
            _callOptionalReturn(token, approvalCall);
        }
    }

    /**
     * @dev Use a ERC-2612 signature to set the `owner` approval toward `spender` on `token`.
     * Revert on invalid signature.
     */
    function safePermit(
        IERC20Permit token,
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) internal {
        uint256 nonceBefore = token.nonces(owner);
        token.permit(owner, spender, value, deadline, v, r, s);
        uint256 nonceAfter = token.nonces(owner);
        require(nonceAfter == nonceBefore + 1, "SafeERC20: permit did not succeed");
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

        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        require(returndata.length == 0 || abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     *
     * This is a variant of {_callOptionalReturn} that silents catches all reverts and returns a bool instead.
     */
    function _callOptionalReturnBool(IERC20 token, bytes memory data) private returns (bool) {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We cannot use {Address-functionCall} here since this should return false
        // and not revert is the subcall reverts.

        (bool success, bytes memory returndata) = address(token).call(data);
        return
            success && (returndata.length == 0 || abi.decode(returndata, (bool))) && Address.isContract(address(token));
    }
}

interface IOperatorAccessControl {
    event RoleGranted(
        bytes32 indexed role,
        address indexed account,
        address indexed sender
    );

    event RoleRevoked(
        bytes32 indexed role,
        address indexed account,
        address indexed sender
    );

    function hasRole(bytes32 role, address account)
        external
        view
        returns (bool);

    function isOperator(address account) external view returns (bool);

    function addOperator(address account) external;

    function revokeOperator(address account) external;
}

contract OperatorAccessControl is IOperatorAccessControl, Ownable {
    struct RoleData {
        mapping(address => bool) members;
        bytes32 adminRole;
    }

    mapping(bytes32 => RoleData) private _roles;

    bytes32 public constant OPERATOR_ROLE = keccak256("OPERATOR_ROLE");

    function hasRole(bytes32 role, address account)
        public
        view
        override
        returns (bool)
    {
        return _roles[role].members[account];
    }

    function _grantRole(bytes32 role, address account) private {
        if (!hasRole(role, account)) {
            _roles[role].members[account] = true;
            emit RoleGranted(role, account, _msgSender());
        }
    }

    function _setupRole(bytes32 role, address account) internal virtual {
        _grantRole(role, account);
    }

    function _revokeRole(bytes32 role, address account) private {
        if (hasRole(role, account)) {
            _roles[role].members[account] = false;
            emit RoleRevoked(role, account, _msgSender());
        }
    }

    modifier isOperatorOrOwner() {
        address _sender = _msgSender();
        require(
            isOperator(_sender) || owner() == _sender,
            "OperatorAccessControl: caller is not operator or owner"
        );
        _;
    }

    modifier onlyOperator() {
        require(
            isOperator(_msgSender()),
            "OperatorAccessControl: caller is not operator"
        );
        _;
    }

    function isOperator(address account) public view override returns (bool) {
        return hasRole(OPERATOR_ROLE, account);
    }

    function _addOperator(address account) internal virtual {
        _grantRole(OPERATOR_ROLE, account);
    }

    function addOperator(address account) public override onlyOperator {
        _grantRole(OPERATOR_ROLE, account);
    }

    function revokeOperator(address account) public override onlyOperator {
        _revokeRole(OPERATOR_ROLE, account);
    }
}

contract MarketV2 is ReentrancyGuard, OperatorAccessControl {
    using SafeERC20 for IERC20;

    address constant ETH_ADDRESS = 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE;

    bool public openAssetsVerify = false;

    address public systemIncomeAddress;

    mapping(uint256 => Partner) public partners;

    mapping(uint256 => Product) public products;

    mapping(address => mapping(uint256 => uint256)) public userProductMap;

    mapping(uint256 => mapping(uint256 => uint256)) public productDayPriceMap;

    mapping(uint256 => bool) public autoPayIdMap;

    uint256 constant RENEW_ADD_DAYS = 1000000;

    uint256 constant DAY_SECOND = 86400;

    uint256 constant DIVIDE_RATIO = 10000;

    mapping(address => mapping(uint256 => bool)) public renewMap;

    mapping(uint256 => bool) private offlineProductMap;

    constructor() {
        _addOperator(_msgSender());
    }

    struct Partner {
        uint256 id;
        uint256 ratio;
        address incomeAddress;
    }

    struct Product {
        uint256 partnerId;
        uint256 productId;
        address payContractAddress;
        uint256[] payAmounts;
        uint256[] purchaseDays;
    }

    function setOfflineProduct(
        uint256 productId_,
        bool offline_
    ) external onlyOperator {
        offlineProductMap[productId_] = offline_;
    }

    function setOpenAssetsVerify(bool openAssetsVerify_) external onlyOperator {
        openAssetsVerify = openAssetsVerify_;
    }

    function setIncomeAddress(
        address systemIncomeAddress_
    ) external onlyOperator {
        require(systemIncomeAddress_ != address(0), "cannot be zero address");
        systemIncomeAddress = systemIncomeAddress_;
    }

    function addPartner(
        uint256 id,
        uint256 ratio,
        address incomeAddress
    ) external onlyOperator {
        require(id > 0, "id must be greater than 0");
        require(partners[id].id == 0, "id already exists");
        require(incomeAddress != address(0), "cannot be zero address");
        partners[id].id = id;
        partners[id].ratio = ratio;
        partners[id].incomeAddress = incomeAddress;
    }

    function updatePartner(
        uint256 id,
        uint256 ratio,
        address incomeAddress
    ) external onlyOperator {
        require(id > 0, "id must be greater than 0");
        require(partners[id].id > 0, "id not exists");
        require(incomeAddress != address(0), "cannot be zero address");
        partners[id].id = id;
        partners[id].ratio = ratio;
        partners[id].incomeAddress = incomeAddress;
    }

    function addProduct(
        uint256 partnerId,
        uint256 productId,
        address payContractAddress,
        uint256[] memory payAmounts,
        uint256[] memory purchaseDays
    ) external onlyOperator {
        require(partnerId > 0, "partnerId must be greater than 0");
        require(productId > 0, "productId must be greater than 0");
        require(partners[partnerId].id > 0, "partner not exists");
        require(products[productId].productId == 0, "productId already exists");
        require(payContractAddress != address(0), "cannot be zero address");
        require(
            payAmounts.length == purchaseDays.length,
            "length does not match"
        );

        products[productId].partnerId = partnerId;
        products[productId].productId = productId;
        products[productId].payContractAddress = payContractAddress;
        products[productId].payAmounts = payAmounts;
        products[productId].purchaseDays = purchaseDays;

        uint256 _len = payAmounts.length;
        for (uint256 _i = 0; _i < _len; _i++) {
            productDayPriceMap[productId][payAmounts[_i]] = purchaseDays[_i];
        }
    }

    function updateProduct(
        uint256 partnerId,
        uint256 productId,
        address payContractAddress,
        uint256[] memory payAmounts,
        uint256[] memory purchaseDays
    ) external onlyOperator {
        require(partnerId > 0, "partnerId must be greater than 0");
        require(productId > 0, "productId must be greater than 0");
        require(partners[partnerId].id > 0, "partner not exists");
        require(products[productId].productId > 0, "productId not exists");
        require(payContractAddress != address(0), "cannot be zero address");
        require(
            payAmounts.length == purchaseDays.length,
            "length does not match"
        );

        products[productId].partnerId = partnerId;
        products[productId].productId = productId;
        products[productId].payContractAddress = payContractAddress;
        products[productId].payAmounts = payAmounts;
        products[productId].purchaseDays = purchaseDays;

        uint256 _len = payAmounts.length;
        for (uint256 _i = 0; _i < _len; _i++) {
            productDayPriceMap[productId][payAmounts[_i]] = purchaseDays[_i];
        }
    }

    // uint256s [partnerId,productId,payAmount,ratio,buyDays,expiresTime,autoPayId]
    // addresses [payContractAddress,buyUserAddress,partnerIncomeAddress,systemIncomeAddress]
    event BuyProductV2(uint256[] uint256s, address[] addresses, bool autoPay);

    function buy(
        uint256 productId,
        uint256 payAmount
    ) public payable nonReentrant {
        require(!offlineProductMap[productId], "Product offline");

        require(productId > 0, "productId must be greater than 0");

        require(products[productId].productId > 0, "productId not exists");

        require(
            systemIncomeAddress != address(0),
            "systemIncomeAddress cannot be zero address"
        );

        address buyAddress = _msgSender();

        address payContractAddress = products[productId].payContractAddress;

        uint256 buyDays = productDayPriceMap[productId][payAmount];

        require(buyDays > 0, "product not exists");

        uint256 partnerId = products[productId].partnerId;

        uint256 ratio = partners[partnerId].ratio;

        address incomeAddress = partners[partnerId].incomeAddress;

        uint256 partnerGetAmount = (payAmount * ratio) / DIVIDE_RATIO;

        uint256 systemGetAmount = payAmount - partnerGetAmount;

        bool autoPay = false;
        uint256 realBuyDays = buyDays;
        if (buyDays > RENEW_ADD_DAYS) {
            autoPay = true;
            realBuyDays = buyDays - RENEW_ADD_DAYS;
        }

        uint256 expiresTime = userProductMap[buyAddress][productId];

        if (autoPay && renewMap[buyAddress][productId]) {
            require(
                expiresTime < block.timestamp,
                "Renewal products cannot be purchased repeatedly"
            );
        }

        if (payContractAddress == ETH_ADDRESS) {
            require(msg.value == payAmount, "msg.value is incorrect");

            payable(incomeAddress).transfer(partnerGetAmount);

            payable(systemIncomeAddress).transfer(systemGetAmount);
        } else {
            IERC20 _erc20 = IERC20(payContractAddress);
            require(
                _erc20.balanceOf(buyAddress) >= payAmount,
                "erc20 payment is insufficient"
            );
            require(
                _erc20.allowance(buyAddress, address(this)) >= payAmount,
                "erc20 payment allowance is insufficient"
            );

            uint256 beforeAssets = _erc20.balanceOf(systemIncomeAddress);

            _erc20.safeTransferFrom(
                buyAddress,
                incomeAddress,
                partnerGetAmount
            );

            _erc20.safeTransferFrom(
                buyAddress,
                systemIncomeAddress,
                systemGetAmount
            );
            uint256 afterAssets = _erc20.balanceOf(systemIncomeAddress);

            if (openAssetsVerify) {
                require(
                    beforeAssets + systemGetAmount == afterAssets,
                    "Assets verify error"
                );
            }
        }

        if (expiresTime < block.timestamp) {
            expiresTime = block.timestamp + (realBuyDays * DAY_SECOND);
        } else {
            expiresTime = expiresTime + (realBuyDays * DAY_SECOND);
        }

        userProductMap[buyAddress][productId] = expiresTime;

        if (autoPay) {
            renewMap[buyAddress][productId] = true;
        }

        uint256[] memory uint256s = new uint256[](7);
        address[] memory addresses = new address[](4);
        uint256s[0] = partnerId;
        uint256s[1] = productId;
        uint256s[2] = payAmount;
        uint256s[3] = ratio;
        uint256s[4] = realBuyDays;
        uint256s[5] = expiresTime;
        uint256s[6] = 0;
        addresses[0] = payContractAddress;
        addresses[1] = buyAddress;
        addresses[2] = incomeAddress;
        addresses[3] = systemIncomeAddress;

        emit BuyProductV2(uint256s, addresses, autoPay);
    }

    function renew(
        uint256 productId,
        uint256 payAmount,
        uint256 autoPayId,
        address buyAddress
    ) external onlyOperator {
        require(products[productId].productId > 0, "product not exists");

        address payContractAddress = products[productId].payContractAddress;

        uint256 buyDays = productDayPriceMap[productId][payAmount];

        require(buyDays > 0, "product not exists");

        uint256 partnerId = products[productId].partnerId;

        uint256 partnerGetAmount = (payAmount * partners[partnerId].ratio) /
            DIVIDE_RATIO;

        uint256 systemGetAmount = payAmount - partnerGetAmount;

        uint256 expiresTime = userProductMap[buyAddress][productId];

        require(expiresTime > block.timestamp, "The product has expired");

        require(
            block.timestamp + DAY_SECOND >= expiresTime,
            "Renewal within 1 day of expiration"
        );

        IERC20 _erc20 = IERC20(payContractAddress);
        require(
            _erc20.balanceOf(buyAddress) >= payAmount,
            "erc20 payment is insufficient"
        );
        require(
            _erc20.allowance(buyAddress, address(this)) >= payAmount,
            "erc20 payment allowance is insufficient"
        );

        require(!autoPayIdMap[autoPayId], "Renewed");

        uint256 realBuyDays = buyDays - RENEW_ADD_DAYS;

        _erc20.safeTransferFrom(
            buyAddress,
            partners[partnerId].incomeAddress,
            partnerGetAmount
        );

        _erc20.safeTransferFrom(
            buyAddress,
            systemIncomeAddress,
            systemGetAmount
        );

        if (expiresTime < block.timestamp) {
            expiresTime = block.timestamp + (realBuyDays * DAY_SECOND);
        } else {
            expiresTime = expiresTime + (realBuyDays * DAY_SECOND);
        }

        userProductMap[buyAddress][productId] = expiresTime;

        autoPayIdMap[autoPayId] = true;

        uint256[] memory uint256s = new uint256[](7);
        address[] memory addresses = new address[](4);
        uint256s[0] = partnerId;
        uint256s[1] = productId;
        uint256s[2] = payAmount;
        uint256s[3] = partners[partnerId].ratio;
        uint256s[4] = realBuyDays;
        uint256s[5] = expiresTime;
        uint256s[6] = autoPayId;
        addresses[0] = payContractAddress;
        addresses[1] = buyAddress;
        addresses[2] = partners[partnerId].incomeAddress;
        addresses[3] = systemIncomeAddress;
        emit BuyProductV2(uint256s, addresses, true);
    }
}