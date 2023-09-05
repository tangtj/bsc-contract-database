// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
 

interface IERC165 {
    /**
     * @dev Returns true if this contract implements the interface defined by
     * `interfaceId`. See the corresponding
     * https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified[EIP section]
     * to learn more about how these ids are created.
     *
     * This function call must use less than 30 000 gas.
     */
    function supportsInterface(bytes4 interfaceId) external view returns (bool);
}



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
        // we're implementing it ourselves. We use {Address.functionCall} to perform this call, which verifies that
        // the target address contains contract code and also asserts for success in the low-level call.

        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        if (returndata.length > 0) {
            // Return data is optional
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}


interface IERC1155 is IERC165 {
    /**
     * @dev Emitted when `value` tokens of token type `id` are transferred from `from` to `to` by `operator`.
     */
    event TransferSingle(address indexed operator, address indexed from, address indexed to, uint256 id, uint256 value);

    /**
     * @dev Equivalent to multiple {TransferSingle} events, where `operator`, `from` and `to` are the same for all
     * transfers.
     */
    event TransferBatch(address indexed operator, address indexed from, address indexed to, uint256[] ids, uint256[] values);

    /**
     * @dev Emitted when `account` grants or revokes permission to `operator` to transfer their tokens, according to
     * `approved`.
     */
    event ApprovalForAll(address indexed account, address indexed operator, bool approved);

    /**
     * @dev Emitted when the URI for token type `id` changes to `value`, if it is a non-programmatic URI.
     *
     * If an {URI} event was emitted for `id`, the standard
     * https://eips.ethereum.org/EIPS/eip-1155#metadata-extensions[guarantees] that `value` will equal the value
     * returned by {IERC1155MetadataURI-uri}.
     */
    event URI(string value, uint256 indexed id);

    /**
     * @dev Returns the amount of tokens of token type `id` owned by `account`.
     *
     * Requirements:
     *
     * - `account` cannot be the zero address.
     */
    function balanceOf(address account, uint256 id) external view returns (uint256);

    /**
     * @dev xref:ROOT:erc1155.adoc#batch-operations[Batched] version of {balanceOf}.
     *
     * Requirements:
     *
     * - `accounts` and `ids` must have the same length.
     */
    function balanceOfBatch(address[] calldata accounts, uint256[] calldata ids) external view returns (uint256[] memory);

    /**
     * @dev Grants or revokes permission to `operator` to transfer the caller's tokens, according to `approved`,
     *
     * Emits an {ApprovalForAll} event.
     *
     * Requirements:
     *
     * - `operator` cannot be the caller.
     */
    function setApprovalForAll(address operator, bool approved) external;

    /**
     * @dev Returns true if `operator` is approved to transfer ``account``'s tokens.
     *
     * See {setApprovalForAll}.
     */
    function isApprovedForAll(address account, address operator) external view returns (bool);

    /**
     * @dev Transfers `amount` tokens of token type `id` from `from` to `to`.
     *
     * Emits a {TransferSingle} event.
     *
     * Requirements:
     *
     * - `to` cannot be the zero address.
     * - If the caller is not `from`, it must have been approved to spend ``from``'s tokens via {setApprovalForAll}.
     * - `from` must have a balance of tokens of type `id` of at least `amount`.
     * - If `to` refers to a smart contract, it must implement {IERC1155Receiver-onERC1155Received} and return the
     * acceptance magic value.
     */
    function safeTransferFrom(address from, address to, uint256 id, uint256 amount, bytes calldata data) external;

    /**
     * @dev xref:ROOT:erc1155.adoc#batch-operations[Batched] version of {safeTransferFrom}.
     *
     * Emits a {TransferBatch} event.
     *
     * Requirements:
     *
     * - `ids` and `amounts` must have the same length.
     * - If `to` refers to a smart contract, it must implement {IERC1155Receiver-onERC1155BatchReceived} and return the
     * acceptance magic value.
     */
    function safeBatchTransferFrom(address from, address to, uint256[] calldata ids, uint256[] calldata amounts, bytes calldata data) external;
}

interface IERC1155MetadataURI is IERC1155 {
    /**
     * @dev Returns the URI for token type `id`.
     *
     * If the `\{id\}` substring is present in the URI, it must be replaced by
     * clients with the actual token type ID.
     */
    function uri(uint256 id) external view returns (string memory);
}  

interface IERC1155SoulBond is IERC1155, IERC1155MetadataURI {
    event BoundSingle(address indexed operator, address indexed source, uint256 id, uint256 value);
    event BoundBatch(address indexed operator, address indexed source, uint256[] id, uint256[] value);

    function boundOf(address account, uint256 id) external view returns (uint256 amount);

    function boundOfBatch(address[] memory accounts, uint256[] memory ids) external view returns (uint256[] memory amounts);
}

interface IUnitag is IERC1155SoulBond {
    event CollectionCreated(address indexed operator, string collectionName);
    event CollectionURIChanged(address indexed operator, string collectionName, string newUri);
    event OwnershipTransferred(address indexed operator, address indexed newOwner, string collectionName);
    event SetupTag(address indexed operator, string collectionName, string tagName, uint256 value);
    event CollectionValueChanged(address indexed source, uint256 id, uint256 value);
    event OperatorAdded(address indexed operator, address indexed addedOperator, string collectionName);
    event OperatorRemoved(address indexed operator, address indexed removedOperator, string collectionName);

    function createCollection(string calldata collectionName, string calldata uri_, address callbackHandler) external returns (uint256 collectionId);

    function setupTag(string calldata collectionName, string calldata tagName, uint256 value) external;

    function transferOwner(string calldata collectionName, address newOwner) external;

    function addOperator(string calldata collectionName, address operator) external;

    function removeOperator(string calldata collectionName, address operator) external;

    function mint(address to, uint256 tagId, uint256 amount, bool bindImmediately) external;

    function bind(address source, uint256 tagId, uint256 value) external;

    function mintBatch(address to, uint256[] calldata tagIds, uint256[] calldata amounts, bool bindImmediately) external;

    function bindBatch(address source, uint256[] calldata tagIds, uint256[] calldata amounts) external;

    function available(string calldata collectionName) external view returns (bool);

    function collectionById(uint256 collectionId) external view returns (address owner, string memory name, string memory uri_);

    function collectionByName(string calldata collectionName) external view returns (uint256 collectionId, address owner, string memory name, string memory uri_);

    function operatorsById(uint256 collectionId) external view returns (address[] memory operators_);

    function operatorsByName(string calldata collectionName) external view returns (address[] memory operators_);

    function tagById(uint256 tagId) external view returns (uint256 collectionId, uint256 value, string memory name);

    function tagByName(string calldata collectionName, string calldata tagName) external view returns (uint256 tagId, uint256 collectionId, uint256 value, string memory name);

    function tagByFullName(string calldata tagFullName) external view returns (uint256 tagId, uint256 collectionId, uint256 value, string memory name);
}


interface IUnitagMinter {
    function mint(address to, uint256 tagId, uint256 amount, bool bindImmediately, bytes calldata signature) external; 
    function mintBatch(address to, uint256[] calldata tagIds, uint256[] calldata amounts, bool bindImmediately, bytes calldata signature) external;
}


interface IUnitagRelationRegistry {
    /**
     * @dev Set relationship through operators
     * @param collectionId the id of the collection
     * @param accounts  the accounts to set
     * @param signature the signatures of the accounts
     */
    function setParent(uint256 collectionId, address[] calldata accounts, bytes calldata signature) external;

    /**
     * @dev check if the account account has ancestor
     * @param collectionId the id of the collection
     * @param account the account to query
     */
    function hasAncestor(uint256 collectionId, address account) external view returns (bool);

    /**
     * @dev Get direct ancestor of one account(parent)
     * @param collectionId the id of the collection
     * @param account the account to query
     */
    function ancestor(uint256 collectionId, address account) external view returns (address);

    /**
     * @dev Get multi level ancestor of one account
     * @param collectionId the id of the collection
     * @param account the account to query
     * @param level the levels to query
     */
    function ancestors(uint256 collectionId, address account, uint256 level) external view returns (address[] memory _ancestors);
}


/**
 * @dev Library for managing
 * https://en.wikipedia.org/wiki/Set_(abstract_data_type)[sets] of primitive
 * types.
 *
 * Sets have the following properties:
 *
 * - Elements are added, removed, and checked for existence in constant time
 * (O(1)).
 * - Elements are enumerated in O(n). No guarantees are made on the ordering.
 *
 * ```
 * contract Example {
 *     // Add the library methods
 *     using EnumerableSet for EnumerableSet.AddressSet;
 *
 *     // Declare a set state variable
 *     EnumerableSet.AddressSet private mySet;
 * }
 * ```
 *
 * As of v3.3.0, sets of type `bytes32` (`Bytes32Set`), `address` (`AddressSet`)
 * and `uint256` (`UintSet`) are supported.
 *
 * [WARNING]
 * ====
 *  Trying to delete such a structure from storage will likely result in data corruption, rendering the structure unusable.
 *  See https://github.com/ethereum/solidity/pull/11843[ethereum/solidity#11843] for more info.
 *
 *  In order to clean an EnumerableSet, you can either remove all elements one by one or create a fresh instance using an array of EnumerableSet.
 * ====
 */
library EnumerableSet {
    // To implement this library for multiple types with as little code
    // repetition as possible, we write it in terms of a generic Set type with
    // bytes32 values.
    // The Set implementation uses private functions, and user-facing
    // implementations (such as AddressSet) are just wrappers around the
    // underlying Set.
    // This means that we can only create new EnumerableSets for types that fit
    // in bytes32.

    struct Set {
        // Storage of set values
        bytes32[] _values;
        // Position of the value in the `values` array, plus 1 because index 0
        // means a value is not in the set.
        mapping(bytes32 => uint256) _indexes;
    }

    /**
     * @dev Add a value to a set. O(1).
     *
     * Returns true if the value was added to the set, that is if it was not
     * already present.
     */
    function _add(Set storage set, bytes32 value) private returns (bool) {
        if (!_contains(set, value)) {
            set._values.push(value);
            // The value is stored at length-1, but we add 1 to all indexes
            // and use 0 as a sentinel value
            set._indexes[value] = set._values.length;
            return true;
        } else {
            return false;
        }
    }

    /**
     * @dev Removes a value from a set. O(1).
     *
     * Returns true if the value was removed from the set, that is if it was
     * present.
     */
    function _remove(Set storage set, bytes32 value) private returns (bool) {
        // We read and store the value's index to prevent multiple reads from the same storage slot
        uint256 valueIndex = set._indexes[value];

        if (valueIndex != 0) {
            // Equivalent to contains(set, value)
            // To delete an element from the _values array in O(1), we swap the element to delete with the last one in
            // the array, and then remove the last element (sometimes called as 'swap and pop').
            // This modifies the order of the array, as noted in {at}.

            uint256 toDeleteIndex = valueIndex - 1;
            uint256 lastIndex = set._values.length - 1;

            if (lastIndex != toDeleteIndex) {
                bytes32 lastValue = set._values[lastIndex];

                // Move the last value to the index where the value to delete is
                set._values[toDeleteIndex] = lastValue;
                // Update the index for the moved value
                set._indexes[lastValue] = valueIndex; // Replace lastValue's index to valueIndex
            }

            // Delete the slot where the moved value was stored
            set._values.pop();

            // Delete the index for the deleted slot
            delete set._indexes[value];

            return true;
        } else {
            return false;
        }
    }

    /**
     * @dev Returns true if the value is in the set. O(1).
     */
    function _contains(Set storage set, bytes32 value) private view returns (bool) {
        return set._indexes[value] != 0;
    }

    /**
     * @dev Returns the number of values on the set. O(1).
     */
    function _length(Set storage set) private view returns (uint256) {
        return set._values.length;
    }

    /**
     * @dev Returns the value stored at position `index` in the set. O(1).
     *
     * Note that there are no guarantees on the ordering of values inside the
     * array, and it may change when more values are added or removed.
     *
     * Requirements:
     *
     * - `index` must be strictly less than {length}.
     */
    function _at(Set storage set, uint256 index) private view returns (bytes32) {
        return set._values[index];
    }

    /**
     * @dev Return the entire set in an array
     *
     * WARNING: This operation will copy the entire storage to memory, which can be quite expensive. This is designed
     * to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that
     * this function has an unbounded cost, and using it as part of a state-changing function may render the function
     * uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.
     */
    function _values(Set storage set) private view returns (bytes32[] memory) {
        return set._values;
    }

    // Bytes32Set

    struct Bytes32Set {
        Set _inner;
    }

    /**
     * @dev Add a value to a set. O(1).
     *
     * Returns true if the value was added to the set, that is if it was not
     * already present.
     */
    function add(Bytes32Set storage set, bytes32 value) internal returns (bool) {
        return _add(set._inner, value);
    }

    /**
     * @dev Removes a value from a set. O(1).
     *
     * Returns true if the value was removed from the set, that is if it was
     * present.
     */
    function remove(Bytes32Set storage set, bytes32 value) internal returns (bool) {
        return _remove(set._inner, value);
    }

    /**
     * @dev Returns true if the value is in the set. O(1).
     */
    function contains(Bytes32Set storage set, bytes32 value) internal view returns (bool) {
        return _contains(set._inner, value);
    }

    /**
     * @dev Returns the number of values in the set. O(1).
     */
    function length(Bytes32Set storage set) internal view returns (uint256) {
        return _length(set._inner);
    }

    /**
     * @dev Returns the value stored at position `index` in the set. O(1).
     *
     * Note that there are no guarantees on the ordering of values inside the
     * array, and it may change when more values are added or removed.
     *
     * Requirements:
     *
     * - `index` must be strictly less than {length}.
     */
    function at(Bytes32Set storage set, uint256 index) internal view returns (bytes32) {
        return _at(set._inner, index);
    }

    /**
     * @dev Return the entire set in an array
     *
     * WARNING: This operation will copy the entire storage to memory, which can be quite expensive. This is designed
     * to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that
     * this function has an unbounded cost, and using it as part of a state-changing function may render the function
     * uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.
     */
    function values(Bytes32Set storage set) internal view returns (bytes32[] memory) {
        return _values(set._inner);
    }

    // AddressSet

    struct AddressSet {
        Set _inner;
    }

    /**
     * @dev Add a value to a set. O(1).
     *
     * Returns true if the value was added to the set, that is if it was not
     * already present.
     */
    function add(AddressSet storage set, address value) internal returns (bool) {
        return _add(set._inner, bytes32(uint256(uint160(value))));
    }

    /**
     * @dev Removes a value from a set. O(1).
     *
     * Returns true if the value was removed from the set, that is if it was
     * present.
     */
    function remove(AddressSet storage set, address value) internal returns (bool) {
        return _remove(set._inner, bytes32(uint256(uint160(value))));
    }

    /**
     * @dev Returns true if the value is in the set. O(1).
     */
    function contains(AddressSet storage set, address value) internal view returns (bool) {
        return _contains(set._inner, bytes32(uint256(uint160(value))));
    }

    /**
     * @dev Returns the number of values in the set. O(1).
     */
    function length(AddressSet storage set) internal view returns (uint256) {
        return _length(set._inner);
    }

    /**
     * @dev Returns the value stored at position `index` in the set. O(1).
     *
     * Note that there are no guarantees on the ordering of values inside the
     * array, and it may change when more values are added or removed.
     *
     * Requirements:
     *
     * - `index` must be strictly less than {length}.
     */
    function at(AddressSet storage set, uint256 index) internal view returns (address) {
        return address(uint160(uint256(_at(set._inner, index))));
    }

    /**
     * @dev Return the entire set in an array
     *
     * WARNING: This operation will copy the entire storage to memory, which can be quite expensive. This is designed
     * to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that
     * this function has an unbounded cost, and using it as part of a state-changing function may render the function
     * uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.
     */
    function values(AddressSet storage set) internal view returns (address[] memory) {
        bytes32[] memory store = _values(set._inner);
        address[] memory result;

        /// @solidity memory-safe-assembly
        assembly {
            result := store
        }

        return result;
    }

    // UintSet

    struct UintSet {
        Set _inner;
    }

    /**
     * @dev Add a value to a set. O(1).
     *
     * Returns true if the value was added to the set, that is if it was not
     * already present.
     */
    function add(UintSet storage set, uint256 value) internal returns (bool) {
        return _add(set._inner, bytes32(value));
    }

    /**
     * @dev Removes a value from a set. O(1).
     *
     * Returns true if the value was removed from the set, that is if it was
     * present.
     */
    function remove(UintSet storage set, uint256 value) internal returns (bool) {
        return _remove(set._inner, bytes32(value));
    }

    /**
     * @dev Returns true if the value is in the set. O(1).
     */
    function contains(UintSet storage set, uint256 value) internal view returns (bool) {
        return _contains(set._inner, bytes32(value));
    }

    /**
     * @dev Returns the number of values on the set. O(1).
     */
    function length(UintSet storage set) internal view returns (uint256) {
        return _length(set._inner);
    }

    /**
     * @dev Returns the value stored at position `index` in the set. O(1).
     *
     * Note that there are no guarantees on the ordering of values inside the
     * array, and it may change when more values are added or removed.
     *
     * Requirements:
     *
     * - `index` must be strictly less than {length}.
     */
    function at(UintSet storage set, uint256 index) internal view returns (uint256) {
        return uint256(_at(set._inner, index));
    }

    /**
     * @dev Return the entire set in an array
     *
     * WARNING: This operation will copy the entire storage to memory, which can be quite expensive. This is designed
     * to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that
     * this function has an unbounded cost, and using it as part of a state-changing function may render the function
     * uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.
     */
    function values(UintSet storage set) internal view returns (uint256[] memory) {
        bytes32[] memory store = _values(set._inner);
        uint256[] memory result;

        /// @solidity memory-safe-assembly
        assembly {
            result := store
        }

        return result;
    }
}


/**
 * @dev Provides a function to batch together multiple calls in a single external call.
 *
 * _Available since v4.1._
 */
abstract contract Multicall {
    /**
     * @dev Receives and executes a batch of function calls on this contract.
     */
    function multicall(bytes[] calldata data) external virtual returns (bytes[] memory results) {
        results = new bytes[](data.length);
        for (uint256 i = 0; i < data.length; i++) {
            results[i] = Address.functionDelegateCall(address(this), data[i]);
        }
        return results;
    }
}


abstract contract PrizeDepositStorageV2 is Multicall {
    using EnumerableSet for EnumerableSet.AddressSet;
    using SafeERC20 for IERC20;

    event DepositPrize(address indexed sender, uint256 indexed collectionId, address indexed payToken, uint256 amount);
    event WithdrawPrize(address indexed sender, uint256 indexed collectionId, address indexed payToken, uint256 amount);
    event SetupPrize(address indexed operator, uint256 tagId, address payToken, uint32 supply, uint256 unitShare);
    event TrimPrize(address indexed operator, uint256 tagId, address payToken);
    event ClaimPrize(address indexed recipient, uint256 tagId, uint256 units, address payToken, uint256 amount);
    event RefParamSet(address indexed operator, uint256 collectionId, uint256 feePercentage, uint256 level1, uint256 level2);
    event Referral(address indexed source, address indexed recipient, uint256 level, address payToken, uint256 amount);

    struct PrizePackage {
        uint32 supply; // 0 for unlimited
        uint32 rest;
        uint192 unitShare;
        address payToken;
    }

    address private constant nativeCurrency = address(0x0);
    address private constant zeroAddress = address(0x0);

    // tagId => prizeId[]
    mapping(uint256 => mapping(address => PrizePackage)) internal _tagPrizes;
    mapping(uint256 => EnumerableSet.AddressSet) internal _tagPrizeTokens;

    mapping(uint256 => uint256) internal _collectionRefParam; // collectionId=>fee

    // project=>token=>rest
    mapping(uint256 => mapping(address => uint256)) internal _prizePool;
    IUnitagRelationRegistry public immutable relationRegistry;

    uint256 public constant PERCENTAGE_BASE = 10000;

    constructor(address relationRegistry_) {
        relationRegistry = IUnitagRelationRegistry(relationRegistry_);
    }

    function _setRefParams(address operator, uint256 collectionId, uint256 feePercentage, uint256 level1, uint256 level2) internal {
        require(feePercentage <= PERCENTAGE_BASE, "UnitagPrizeDeposit: invalid ref level");
        require(level1 <= PERCENTAGE_BASE, "UnitagPrizeDeposit: invalid L1 percentage");
        require(level2 <= PERCENTAGE_BASE, "UnitagPrizeDeposit: invalid L2 percentage");
        require(level1 + level2 <= PERCENTAGE_BASE, "UnitagPrizeDeposit: invalid L1+L2 percentage");
        _collectionRefParam[collectionId] = (feePercentage << 64) | (level2 << 32) | level1;
        emit RefParamSet(operator, collectionId, feePercentage, level1, level2);
    }

    function _refParams(uint256 collectionId) internal view returns (uint256 feePercentage, uint256 level1, uint256 level2) {
        uint256 refParams = _collectionRefParam[collectionId];
        level1 = refParams & type(uint32).max;
        level2 = (refParams >> 32) & type(uint32).max;
        feePercentage = refParams >> 64;
    }

    function _prizePoolOf(uint256 collectionId, address token) internal view returns (uint256) {
        return _prizePool[collectionId][token];
    }

    function _prizeOf(uint256 tagId) internal view returns (PrizePackage[] memory prizes) {
        EnumerableSet.AddressSet storage prizeTokens = _tagPrizeTokens[tagId];
        uint256 prizeCount = prizeTokens.length();
        prizes = new PrizePackage[](prizeCount);
        mapping(address => PrizePackage) storage _prizes = _tagPrizes[tagId];
        for (uint256 index = 0; index < prizeCount; ++index) {
            address payToken = prizeTokens.at(index);
            prizes[index] = _prizes[payToken];
            prizes[index].payToken = payToken;
        }
    }

    function _depositPrize(address sender, uint256 collectionId, address payToken, uint256 amount) internal {
        uint256 balance = _transferInToken(sender, payToken, amount);
        _prizePool[collectionId][payToken] += balance;
        emit DepositPrize(sender, collectionId, payToken, balance);
    }

    function _withdrawPrize(address recipient, uint256 collectionId, address payToken, uint256 amount) internal {
        uint256 balance = _prizePool[collectionId][payToken];
        require(balance >= amount, "UnitagPrizeDeposit: not enought prize");
        _prizePool[collectionId][payToken] = balance - amount;
        _transferOutToken(recipient, payToken, amount);
        emit WithdrawPrize(recipient, collectionId, payToken, amount);
    }

    function _setupPrize(address operator, uint256 tagId, address payToken, uint32 supply, uint192 unitShare) internal {
        if (unitShare == 0) _tagPrizeTokens[tagId].remove(payToken);
        else {
            _tagPrizeTokens[tagId].add(payToken);
            PrizePackage storage prize = _tagPrizes[tagId][payToken];
            prize.supply = supply;
            prize.rest = supply;
            prize.unitShare = unitShare;
            prize.payToken = payToken;
        }
        emit SetupPrize(operator, tagId, payToken, supply, unitShare);
    }

    function _claimPrize(address recipient, uint256 collectionId, uint256 tagId, uint256 amount) internal {
        uint256 prizeCount = _tagPrizeTokens[tagId].length();
        address[] memory payTokens = new address[](prizeCount);
        uint256[] memory payAmounts = new uint256[](prizeCount);
        uint256[] memory units = new uint256[](prizeCount);
        uint256 rCount;
        for (uint256 index = 0; index < prizeCount; ++index) {
            address payToken = _tagPrizeTokens[tagId].at(index);
            {
                PrizePackage storage prize = _tagPrizes[tagId][payToken];
                uint256 balance = _prizePool[collectionId][payToken];
                {
                    uint256 _units = amount;
                    uint256 unitShare = prize.unitShare;
                    if (balance < unitShare) continue;
                    if (prize.supply > 0) {
                        uint256 rest = prize.rest;
                        _units = _min(rest, amount);
                        if (_units == 0) continue;
                        prize.rest = uint32(rest - _units);
                    }
                    _units = _min(balance / unitShare, _units);

                    uint256 sendAmount = _units * unitShare;
                    balance -= sendAmount;

                    payTokens[rCount] = payToken;
                    payAmounts[rCount] = sendAmount;
                    units[rCount] = _units;
                }
                _prizePool[collectionId][payToken] = balance;
                ++rCount;
            }
        }
        if (rCount > 0) {
            if (rCount != payTokens.length) {
                assembly {
                    mstore(payTokens, rCount)
                    mstore(payAmounts, rCount)
                    mstore(units, rCount)
                }
            }
            _transferOutTokenWithAncesors(collectionId, recipient, tagId, units, payTokens, payAmounts);
        }
    }

    function trimPrize(uint256 tagId) public {
        mapping(address => PrizePackage) storage prizes = _tagPrizes[tagId];
        EnumerableSet.AddressSet storage prizeTokens = _tagPrizeTokens[tagId];
        uint256 prizeCount = prizeTokens.length();
        for (uint256 index = 0; index < prizeCount; ) {
            address prizeToken = prizeTokens.at(index);
            if (prizes[prizeToken].supply > 0 && prizes[prizeToken].rest == 0) {
                --prizeCount;
                prizeTokens.remove(prizeToken);
                emit TrimPrize(msg.sender, tagId, prizeToken);
            } else ++index;
        }
    }

    /**
     * @param collectionId the id of the collection
     * @param accounts  the accounts to set
     * @param signature the signatures of the accounts
     */
    function setParent(uint256 collectionId, address[] calldata accounts, bytes calldata signature) public {
        relationRegistry.setParent(collectionId, accounts, signature);
    }

    function _transferOutToken(address recipient, address payToken, uint256 value) private {
        if (payToken == nativeCurrency) {
            (bool success, ) = recipient.call{value: value}("");
            require(success, "Address: unable to send value, recipient may have reverted");
        } else IERC20(payToken).safeTransfer(recipient, value);
    }

    function _transferOutTokenWithAncesors(uint256 collectionId, address recipient, uint256 tagId, uint256[] memory units, address[] memory payTokens, uint256[] memory values) private {
        address ancestors0;
        address ancestors1;
        (, uint256 level1, uint256 level2) = _refParams(collectionId);
        {
            address[] memory ancestors = relationRegistry.ancestors(collectionId, recipient, 2);
            if (ancestors.length == 2) {
                ancestors0 = ancestors[0];
                ancestors1 = ancestors[1];
            } else if (ancestors.length == 1) {
                ancestors0 = ancestors[0];
                level2 = 0;
            } else {
                level1 = 0;
                level2 = 0;
            }
        }

        uint256 tokenCount = payTokens.length;
        for (uint256 index = 0; index < tokenCount; ++index) {
            uint256 value = values[index];
            address payToken = payTokens[index];
            uint256 feeTotal;
            if (level1 != 0) {
                uint256 fee = (value * level1) / PERCENTAGE_BASE;
                feeTotal += fee;
                _transferOutToken(ancestors0, payToken, fee);
                emit Referral(recipient, ancestors0, 1, payToken, fee);
            }
            if (level2 != 0) {
                uint256 fee = (value * level2) / PERCENTAGE_BASE;
                feeTotal += fee;
                _transferOutToken(ancestors1, payToken, fee);
                emit Referral(recipient, ancestors1, 2, payToken, fee);
            }
            _transferOutToken(recipient, payToken, value - feeTotal);
            emit ClaimPrize(recipient, tagId, units[index], payToken, value - feeTotal);
        }
    }

    function _transferInToken(address spender, address payToken, uint256 value) private returns (uint256) {
        if (payToken == nativeCurrency) {
            return msg.value;
        } else {
            IERC20 erc20 = IERC20(payToken);
            uint256 balanceBefore = erc20.balanceOf(address(this));
            IERC20(payToken).safeTransferFrom(spender, address(this), value);
            return erc20.balanceOf(address(this)) - balanceBefore;
        }
    }

    function _min(uint256 a, uint256 b) private pure returns (uint256) {
        return a < b ? a : b;
    }
}



contract UnitagPrizeDepositMinter is PrizeDepositStorageV2 {
    IUnitag public immutable unitag;
    IUnitagMinter public immutable unitagMinter;

    constructor(address unitag_, address unitagMinter_, address relationRegistry_) PrizeDepositStorageV2(relationRegistry_) {
        unitag = IUnitag(unitag_);
        unitagMinter = IUnitagMinter(unitagMinter_);
    }

    function prizeOf(string calldata tagFullName) public view returns (PrizePackage[] memory prizes) {
        (uint256 tagId, , , ) = unitag.tagByFullName(tagFullName);
        prizes = _prizeOf(tagId);
    }

    function prizePoolOf(string calldata collectionName, address payToken) public view returns (uint256) {
        (uint256 collectionId, , , ) = unitag.collectionByName(collectionName);
        return _prizePoolOf(collectionId, payToken);
    }

    function setRefParams(string calldata collectionName, uint256 feePercentage, uint256 level1, uint256 level2) external {
        (uint256 collectionId, address owner, , ) = unitag.collectionByName(collectionName);
        require(owner == msg.sender, "Require collection owner");
        _setRefParams(msg.sender, collectionId, feePercentage, level1, level2);
    }

    function refParams(string calldata collectionName) public view returns (uint256 feePercentage, uint256 level1, uint256 level2) {
        (uint256 collectionId, , , ) = unitag.collectionByName(collectionName);
        (feePercentage, level1, level2) = _refParams(collectionId);
    }

    function depositPrize(string calldata collectionName, address payToken, uint256 amount) external payable {
        (uint256 collectionId, , , ) = unitag.collectionByName(collectionName);
        _depositPrize(msg.sender, collectionId, payToken, amount);
    }

    function withdrawPrize(string calldata collectionName, address payToken, uint256 amount, address recipient) external {
        (uint256 collectionId, address owner, , ) = unitag.collectionByName(collectionName);
        require(owner == msg.sender, "Require collection owner");
        _withdrawPrize(recipient, collectionId, payToken, amount);
    }

    function setupPrize(string calldata collectionName, string calldata tagName, address payToken, uint32 supply, uint192 unitShare) external {
        (, address owner, , ) = unitag.collectionByName(collectionName);
        require(owner == msg.sender, "Require collection owner");
        (uint256 tagId, , , ) = unitag.tagByName(collectionName, tagName);
        _setupPrize(msg.sender, tagId, payToken, supply, unitShare);
    }

    function mint(address to, uint256 tagId, uint256 amount, bool bindImmediately, bytes calldata signature) external {
        unitagMinter.mint(to, tagId, amount, bindImmediately, signature);
        if (bindImmediately) {
            (uint256 collectionId, , ) = unitag.tagById(tagId);
            _claimPrize(to, collectionId, tagId, amount);
        }
    }

    function mintBatch(address to, uint256[] calldata tagIds, uint256[] calldata amounts, bool bindImmediately, bytes calldata signature) external {
        unitagMinter.mintBatch(to, tagIds, amounts, bindImmediately, signature);
        if (bindImmediately) {
            uint256 tagLength = tagIds.length;
            if (tagLength > 0) {
                (uint256 collectionId, , ) = unitag.tagById(tagIds[0]);
                for (uint256 index = 0; index < tagLength; ++index) {
                    _claimPrize(to, collectionId, tagIds[index], amounts[index]);
                }
            }
        }
    }

    function validAsCollectionOwner(uint256 collectionId, address target) private view {
        (address owner, , ) = unitag.collectionById(collectionId);
        require(owner == target, "Require collection owner");
    }
}