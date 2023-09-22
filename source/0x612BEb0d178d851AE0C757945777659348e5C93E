// File: @openzeppelin/contracts/utils/Address.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/Address.sol)

pragma solidity ^0.8.1;

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

// File: @openzeppelin/contracts/token/ERC20/extensions/IERC20Permit.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/extensions/IERC20Permit.sol)

pragma solidity ^0.8.0;

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

// File: @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol


// OpenZeppelin Contracts (last updated v4.9.3) (token/ERC20/utils/SafeERC20.sol)

pragma solidity ^0.8.0;




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
     * non-reverting calls are assumed to be successful. Meant to be used with tokens that require the approval
     * to be set to zero before setting it to a non-zero value, such as USDT.
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

// File: contracts/Web3SlotsUSDT.sol


// Name: Web-3 Slots
// https://web3smc.net/

pragma solidity 0.8.16;




interface iRewardPool {
    function makeCalculations(uint _amount) external;
    function setUserWeight(address user, uint256 weight) external;
}

interface IWeb3 {
    function registerInTheMatrix(address newuser, address sponsor) external returns (bool);
    function User(uint _activeMatrix, address _user) external view returns (
        uint id,
        uint sID,
        address sAddress,
        uint pID,
        address pAddress,
        uint invitedByCnt,
        uint cnt
    );
    function activeMatrix(address _user) external view returns (uint);
    function addressToId(address _user) external view returns (uint);
}

contract Web3Slots_USDToken is Ownable {
   
    /* SETTINGS */
    IWeb3 private web3;
    iRewardPool rewardPool;
    IERC20 private usdToken;
    using SafeERC20 for IERC20;
    uint private constant initialCost = 15 * 10**18;  // Assuming that USDToken has 18 decimals
    uint stage = 0;

    /* MAPPINGS */
    mapping(uint => bool) public isLevelActive;
    mapping(address => mapping(uint => bool)) public levelBoughtByUser;
    mapping(address => uint256) public directSponsorEarnings;
    mapping(address => uint256) public levelSponsorEarnings;
    mapping(address => bool) whitelisted;

    /* EVENTS */
    event activateSlot(address indexed user, uint indexed level);

    constructor(IWeb3 _web3, IERC20 _usdToken) {
        web3 = _web3; //0x8F1B56F559e7e2c2258859dB29FD7836647B02AD Web3Slots
        usdToken = _usdToken;
        //0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56 - BUSD - deprecated
        //0x55d398326f99059ff775485246999027b3197955 - BSC-USD (USDT)
        //0x8ac76a51cc950d9822d68b83fe1ad97b32cd580d - USDC
        whitelisted[msg.sender] = true;
        for(uint i = 1; i <= 10; i++) {
            levelBoughtByUser[msg.sender][i] = true;
        }
        for(uint i = 1; i <= 10; i++) {
            isLevelActive[i] = false;
        }
        isLevelActive[1] = true;
    }
    
    function buyInitSlot(address sponsor) public {
        uint level = 1;
        web3.registerInTheMatrix(msg.sender, sponsor);
        if(stage == 0) require(whitelisted[msg.sender] == true, "Not whitelisted");
        require(web3.addressToId(msg.sender) > 0, "User is not registered");
        require(level == 1, "Invalid level");
        require(!levelBoughtByUser[msg.sender][level], "Level already bought by user");

        // Calculate the cost for the level
        uint costForLevel = initialCost * (2 ** (level - 1));

        // Check if the allowance is enough
        require(usdToken.allowance(msg.sender, address(this)) >= costForLevel, "Check the token allowance");

        // Transfer the tokens from the sender to this contract
        usdToken.safeTransferFrom(msg.sender, address(this), costForLevel);

        // Update that the level has been bought by the user
        levelBoughtByUser[msg.sender][level] = true;

        // Distribution of the funds
        (,,address directSponsor,,,,) = web3.User(1, msg.sender);
        distributeFunds(level, costForLevel / 3, directSponsor);
    }

    function buySlot(uint level) public {
        if(stage == 0) require(whitelisted[msg.sender] == true, "Not whitelisted");
        require(web3.addressToId(msg.sender) > 0, "User is not registered");
        require(level >= 1 && level <= 10, "Invalid slot");
        if(level > 1) require(levelBoughtByUser[msg.sender][level-1], "You should buy previous slot first");
        require(!levelBoughtByUser[msg.sender][level], "Slot already bought by user");
        require(isLevelActive[level], "This slot is currently inactive");

        // Calculate the cost for the level
        uint costForLevel = initialCost * (2 ** (level - 1));

        // Check if the allowance is enough
        require(usdToken.allowance(msg.sender, address(this)) >= costForLevel, "Check the token allowance");

        // Transfer the tokens from the sender to this contract
        usdToken.safeTransferFrom(msg.sender, address(this), costForLevel);

        // Update that the level has been bought by the user
        levelBoughtByUser[msg.sender][level] = true;

        // Distribution of the funds
        (,,address directSponsor,,,,) = web3.User(1, msg.sender);
        distributeFunds(level, costForLevel / 3, directSponsor);
    }

    function distributeFunds(uint level, uint sponsorAmount, address directSponsor) internal {
        uint activeMatrix = web3.activeMatrix(msg.sender);
        address sAddress = directSponsor;
        (,,,,address pAddress,,) = web3.User(activeMatrix, msg.sender);
        
        // Service fee is only for the first two levels
        if(level <= 2) {
            // Transfer service fee to the owner
            usdToken.safeTransfer(owner(), sponsorAmount);
        } else {
            usdToken.safeTransfer(address(rewardPool), sponsorAmount);
            if (stage != 2) {
                rewardPool.makeCalculations(sponsorAmount);
                rewardPool.setUserWeight(msg.sender, level * 10);
            }
        }

        // Send to 5 level sponsors
        address sponsor = pAddress;
        for(uint i = 0; i < 5; i++) {
            while(!levelBoughtByUser[sponsor][level]) {
                // Check if the sponsor's sponsor has bought the level
                activeMatrix = web3.activeMatrix(sponsor);
                (,,,,sponsor,,) = web3.User(activeMatrix, sponsor);
            }
            uint amount = sponsorAmount / 5; // 20% of the sponsor amount
            usdToken.safeTransfer(sponsor, amount);
            // Update the earnings for the sponsor
            levelSponsorEarnings[sponsor] += amount;
            activeMatrix = web3.activeMatrix(sponsor);
            (,,,,sponsor,,) = web3.User(activeMatrix, sponsor);
        }

        // Distribute funds to the direct sponsor
        if(sAddress != address(0)) {
            usdToken.transfer(directSponsor, sponsorAmount);
            // Update the earnings for the direct sponsor
            directSponsorEarnings[directSponsor] += sponsorAmount;
        }
    }

    function distributeFundsLog(address user, uint level, uint sponsorAmount, address directSponsor) internal {
        uint activeMatrix = web3.activeMatrix(user);
        address sAddress = directSponsor;
        (,,,,address pAddress,,) = web3.User(activeMatrix, user);        
        // Send to 5 level sponsors
        address sponsor = pAddress;
        for(uint i = 0; i < 5; i++) {
            while(!levelBoughtByUser[sponsor][level]) {
                // Check if the sponsor's sponsor has bought the level
                activeMatrix = web3.activeMatrix(sponsor);
                (,,,,sponsor,,) = web3.User(activeMatrix, sponsor);
            }
            uint amount = sponsorAmount / 5; // 20% of the sponsor amount
            // Update the earnings for the sponsor
            levelSponsorEarnings[sponsor] += amount;
            activeMatrix = web3.activeMatrix(sponsor);
            (,,,,sponsor,,) = web3.User(activeMatrix, sponsor);
        }
        // Distribute funds to the direct sponsor
        if(sAddress != address(0)) {
            directSponsorEarnings[directSponsor] += sponsorAmount;
        }
    }

    function getUserData(address _user) public view returns (bool[10] memory activeLevels, uint256 directEarnings, uint256 levelEarnings, uint256 totalEarnings) {        
        for (uint i = 0; i < 10; i++) 
            activeLevels[i] = levelBoughtByUser[_user][i+1];

        directEarnings = directSponsorEarnings[_user];
        levelEarnings = levelSponsorEarnings[_user];
        totalEarnings = directEarnings + levelEarnings;

        return (activeLevels, directEarnings, levelEarnings, totalEarnings);
    }

    // Function that allows owner to set the status of a level
    function setSlotStatus(uint level, bool isActive) public onlyOwner {
        require(level >= 1 && level <= 10, "Invalid level");
        isLevelActive[level] = isActive;
    }

    function setRewardPool(address _contractAddress) public onlyOwner returns (bool) {
        rewardPool = iRewardPool(_contractAddress);
        return true;
    }

    function withdrawTokensFromBalance(address _token) public onlyOwner {
        require(_token != address(0), "withdrawTokensFromBalance: Token is the zero address");
        IERC20 withdrawToken = IERC20(_token);
        uint256 tokenBalance = withdrawToken.balanceOf(address(this));
        if(tokenBalance > 0) withdrawToken.safeTransfer(msg.sender, tokenBalance);
    }

    function recoveryFunds() public onlyOwner {
        address payable _owner = payable(owner());
        _owner.transfer(address(this).balance);
    }

    function setStage(uint _stage) public onlyOwner returns (bool) {
        stage = _stage;
        return true;
    }

    function modifyWhitelist(address[] memory _list) public onlyOwner returns(uint count) {
        uint _count = 0;
        for (uint256 i = 0; i < _list.length; i++) {
            if(whitelisted[_list[i]] != true){
                whitelisted[_list[i]] = true;
                _count++;
            }
        }
        return _count;
    }

    function setUSDToken(IERC20 _usdToken) public onlyOwner returns(IERC20 usdToken_) {
        usdToken = _usdToken;
        return usdToken;
    }

    function activateUserSlot(address[] memory users, uint level) public onlyOwner {
        require(level >= 1 && level <= 10, "Invalid slot");        
        require(isLevelActive[level], "This slot is currently inactive");

        uint costForLevel = initialCost * (2 ** (level - 1));
        for (uint256 i = 0; i < users.length; i++) {            
            if(levelBoughtByUser[users[i]][level]) continue;
            if(web3.addressToId(users[i]) > 0) {
                levelBoughtByUser[users[i]][level] = true;
                (,,address directSponsor,,,,) = web3.User(1, users[i]);
                distributeFundsLog(users[i], level, costForLevel / 3, directSponsor);
                levelBoughtByUser[users[i]][level] = true;
            }
        }
        
    }

}