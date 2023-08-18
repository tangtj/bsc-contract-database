// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

/**
 * @dev Interface of the ERC165 standard, as defined in the
 * https://eips.ethereum.org/EIPS/eip-165[EIP].
 *
 * Implementers can declare support of contract interfaces, which can then be
 * queried by others ({ERC165Checker}).
 *
 * For an implementation, see {ERC165}.
 */
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

// File: @openzeppelin/contracts/token/ERC721/IERC721.sol
/**
 * @dev Required interface of an ERC721 compliant contract.
 */
interface IERC721 is IERC165 {
    /**
     * @dev Emitted when `tokenId` token is transferred from `from` to `to`.
     */
    event Transfer(
        address indexed from,
        address indexed to,
        uint256 indexed tokenId
    );

    /**
     * @dev Emitted when `owner` enables `approved` to manage the `tokenId` token.
     */
    event Approval(
        address indexed owner,
        address indexed approved,
        uint256 indexed tokenId
    );

    /**
     * @dev Emitted when `owner` enables or disables (`approved`) `operator` to manage all of its assets.
     */
    event ApprovalForAll(
        address indexed owner,
        address indexed operator,
        bool approved
    );

    /**
     * @dev Returns the number of tokens in ``owner``'s account.
     */
    function balanceOf(address owner) external view returns (uint256 balance);

    /**
     * @dev Returns the owner of the `tokenId` token.
     *
     * Requirements:
     *
     * - `tokenId` must exist.
     */
    function ownerOf(uint256 tokenId) external view returns (address owner);

    /**
     * @dev Gives permission to `to` to transfer `tokenId` token to another account.
     * The approval is cleared when the token is transferred.
     *
     * Only a single account can be approved at a time, so approving the zero address clears previous approvals.
     *
     * Requirements:
     *
     * - The caller must own the token or be an approved operator.
     * - `tokenId` must exist.
     *
     * Emits an {Approval} event.
     */
    function approve(address to, uint256 tokenId) external;

    /**
     * @dev Returns the account approved for `tokenId` token.
     *
     * Requirements:
     *
     * - `tokenId` must exist.
     */
    function getApproved(uint256 tokenId)
        external
        view
        returns (address operator);

    /**
     * @dev Approve or remove `operator` as an operator for the caller.
     * Operators can call {transferFrom} or {safeTransferFrom} for any token owned by the caller.
     *
     * Requirements:
     *
     * - The `operator` cannot be the caller.
     *
     * Emits an {ApprovalForAll} event.
     */
    function setApprovalForAll(address operator, bool _approved) external;

    /**
     * @dev Returns if the `operator` is allowed to manage all of the assets of `owner`.
     *
     * See {setApprovalForAll}
     */
    function isApprovedForAll(address owner, address operator)
        external
        view
        returns (bool);
}

/**
 * @title ERC-721 Non-Fungible Token Standard, optional enumeration extension
 * @dev See https://eips.ethereum.org/EIPS/eip-721
 */
interface IERC721Enumerable is IERC721 {
    /**
     * @dev Returns the total amount of tokens stored by the contract.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns a token ID owned by `owner` at a given `index` of its token list.
     * Use along with {balanceOf} to enumerate all of ``owner``'s tokens.
     */
    function tokenOfOwnerByIndex(address owner, uint256 index)
        external
        view
        returns (uint256 tokenId);

    /**
     * @dev Returns a token ID at a given `index` of all the tokens stored by the contract.
     * Use along with {totalSupply} to enumerate all tokens.
     */
    function tokenByIndex(uint256 index) external view returns (uint256);
}

/**
 * @dev Implementation of the {IERC165} interface.
 *
 * Contracts that want to implement ERC165 should inherit from this contract and override {supportsInterface} to check
 * for the additional interface id that will be supported. For example:
 *
 * ```solidity
 * function supportsInterface(bytes4 interfaceId) public view virtual override returns (bool) {
 *     return interfaceId == type(MyInterface).interfaceId || super.supportsInterface(interfaceId);
 * }
 * ```
 *
 * Alternatively, {ERC165Storage} provides an easier to use but more expensive implementation.
 */
abstract contract ERC165 is IERC165 {
    /**
     * @dev See {IERC165-supportsInterface}.
     */
    function supportsInterface(bytes4 interfaceId)
        public
        view
        virtual
        override
        returns (bool)
    {
        return interfaceId == type(IERC165).interfaceId;
    }
}

/**
 * @dev String operations.
 */
library Strings {
    bytes16 private constant _HEX_SYMBOLS = "0123456789abcdef";

    /**
     * @dev Converts a `uint256` to its ASCII `string` decimal representation.
     */
    function toString(uint256 value) internal pure returns (string memory) {
        // Inspired by OraclizeAPI's implementation - MIT licence
        // https://github.com/oraclize/ethereum-api/blob/b42146b063c7d6ee1358846c198246239e9360e8/oraclizeAPI_0.4.25.sol
        if (value == 0) {
            return "0";
        }
        uint256 temp = value;
        uint256 digits;
        while (temp != 0) {
            digits++;
            temp /= 10;
        }
        bytes memory buffer = new bytes(digits);
        while (value != 0) {
            digits -= 1;
            buffer[digits] = bytes1(uint8(48 + uint256(value % 10)));
            value /= 10;
        }
        return string(buffer);
    }

    /**
     * @dev Converts a `uint256` to its ASCII `string` hexadecimal representation.
     */
    function toHexString(uint256 value) internal pure returns (string memory) {
        if (value == 0) {
            return "0x00";
        }
        uint256 temp = value;
        uint256 length = 0;
        while (temp != 0) {
            length++;
            temp >>= 8;
        }
        return toHexString(value, length);
    }

    /**
     * @dev Converts a `uint256` to its ASCII `string` hexadecimal representation with fixed length.
     */
    function toHexString(uint256 value, uint256 length)
        internal
        pure
        returns (string memory)
    {
        bytes memory buffer = new bytes(2 * length + 2);
        buffer[0] = "0";
        buffer[1] = "x";
        for (uint256 i = 2 * length + 1; i > 1; --i) {
            buffer[i] = _HEX_SYMBOLS[value & 0xf];
            value >>= 4;
        }
        require(value == 0, "Strings: hex length insufficient");
        return string(buffer);
    }
}

/**
 * @title ERC-721 Non-Fungible Token Standard, optional metadata extension
 * @dev See https://eips.ethereum.org/EIPS/eip-721
 */
interface IERC721Metadata is IERC721 {
    /**
     * @dev Returns the token collection name.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the token collection symbol.
     */
    function symbol() external view returns (string memory);

    /**
     * @dev Returns the Uniform Resource Identifier (URI) for `tokenId` token.
     */
    function tokenURI(uint256 tokenId) external view returns (string memory);
}

/**
 * @title ERC721 token receiver interface
 * @dev Interface for any contract that wants to support safeTransfers
 * from ERC721 asset contracts.
 */
interface IERC721Receiver {
    /**
     * @dev Whenever an {IERC721} `tokenId` token is transferred to this contract via {IERC721-safeTransferFrom}
     * by `operator` from `from`, this function is called.
     *
     * It must return its Solidity selector to confirm the token transfer.
     * If any other value is returned or the interface is not implemented by the recipient, the transfer will be reverted.
     *
     * The selector can be obtained in Solidity with `IERC721.onERC721Received.selector`.
     */
    function onERC721Received(
        address operator,
        address from,
        uint256 tokenId,
        bytes calldata data
    ) external returns (bytes4);
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
contract Ownable is Context {
    address public _owner;

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

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );
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
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
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
     *
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
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
     *
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
     *
     * - The divisor cannot be zero.
     */
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
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
     *
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
     *
     * - The divisor cannot be zero.
     */
    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
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
        // According to EIP-1052, 0x0 is the value returned for not-yet created accounts
        // and 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470 is returned
        // for accounts without code, i.e. `keccak256('')`
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly {
            codehash := extcodehash(account)
        }
        return (codehash != accountHash && codehash != 0x0);
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
        require(
            address(this).balance >= amount,
            "Address: insufficient balance"
        );

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{value: amount}("");
        require(
            success,
            "Address: unable to send value, recipient may have reverted"
        );
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
    function functionCall(address target, bytes memory data)
        internal
        returns (bytes memory)
    {
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
        return _functionCallWithValue(target, data, 0, errorMessage);
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
        return
            functionCallWithValue(
                target,
                data,
                value,
                "Address: low-level call with value failed"
            );
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
        return _functionCallWithValue(target, data, value, errorMessage);
    }

    function _functionCallWithValue(
        address target,
        bytes memory data,
        uint256 weiValue,
        string memory errorMessage
    ) private returns (bytes memory) {
        require(isContract(target), "Address: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{value: weiValue}(
            data
        );
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
 * @dev Implementation of https://eips.ethereum.org/EIPS/eip-721[ERC721] Non-Fungible Token Standard, including
 * the Metadata extension, but not including the Enumerable extension, which is available separately as
 * {ERC721Enumerable}.
 */
contract ERC721 is Context, ERC165, IERC721, IERC721Metadata {
    using Address for address;
    using Strings for uint256;

    // Token name
    string private _name;
    // Token symbol
    string private _symbol;

    // Mapping from token ID to owner address
    mapping(uint256 => address) private _owners;
    // Mapping owner address to token count
    mapping(address => uint256) private _balances;
    // Mapping from token ID to approved address
    mapping(uint256 => address) private _tokenApprovals;
    // Mapping from owner to operator approvals
    mapping(address => mapping(address => bool)) private _operatorApprovals;

    /**
     * @dev Initializes the contract by setting a `name` and a `symbol` to the token collection.
     */
    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    /**
     * @dev See {IERC165-supportsInterface}.
     */
    function supportsInterface(bytes4 interfaceId)
        public
        view
        virtual
        override(ERC165, IERC165)
        returns (bool)
    {
        return
            interfaceId == type(IERC721).interfaceId ||
            interfaceId == type(IERC721Metadata).interfaceId ||
            super.supportsInterface(interfaceId);
    }

    /**
     * @dev See {IERC721-balanceOf}.
     */
    function balanceOf(address owner)
        public
        view
        virtual
        override
        returns (uint256)
    {
        require(
            owner != address(0),
            "ERC721: balance query for the zero address"
        );
        return _balances[owner];
    }

    /**
     * @dev See {IERC721-ownerOf}.
     */
    function ownerOf(uint256 tokenId)
        public
        view
        virtual
        override
        returns (address)
    {
        address owner = _owners[tokenId];
        require(
            owner != address(0),
            "ERC721: owner query for nonexistent token"
        );
        return owner;
    }

    /**
     * @dev See {IERC721Metadata-name}.
     */
    function name() public view virtual override returns (string memory) {
        return _name;
    }

    /**
     * @dev See {IERC721Metadata-symbol}.
     */
    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    /**
     * @dev See {IERC721Metadata-tokenURI}.
     */
    function tokenURI(uint256 tokenId)
        public
        view
        virtual
        override
        returns (string memory)
    {
        require(
            _exists(tokenId),
            "ERC721Metadata: URI query for nonexistent token"
        );

        string memory baseURI = _baseURI();
        return
            bytes(baseURI).length > 0
                ? string(abi.encodePacked(baseURI, tokenId.toString()))
                : "";
    }

    /**
     * @dev Base URI for computing {tokenURI}. If set, the resulting URI for each
     * token will be the concatenation of the `baseURI` and the `tokenId`. Empty
     * by default, can be overriden in child contracts.
     */
    function _baseURI() internal view virtual returns (string memory) {
        return "";
    }

    /**
     * @dev See {IERC721-approve}.
     */
    function approve(address to, uint256 tokenId) public virtual override {
        address owner = ERC721.ownerOf(tokenId);
        require(to != owner, "ERC721: approval to current owner");

        require(
            _msgSender() == owner || isApprovedForAll(owner, _msgSender()),
            "ERC721: approve caller is not owner nor approved for all"
        );

        _approve(to, tokenId);
    }

    /**
     * @dev See {IERC721-getApproved}.
     */
    function getApproved(uint256 tokenId)
        public
        view
        virtual
        override
        returns (address)
    {
        require(
            _exists(tokenId),
            "ERC721: approved query for nonexistent token"
        );

        return _tokenApprovals[tokenId];
    }

    /**
     * @dev See {IERC721-setApprovalForAll}.
     */
    function setApprovalForAll(address operator, bool approved)
        public
        virtual
        override
    {
        require(operator != _msgSender(), "ERC721: approve to caller");

        _operatorApprovals[_msgSender()][operator] = approved;
        emit ApprovalForAll(_msgSender(), operator, approved);
    }

    /**
     * @dev See {IERC721-isApprovedForAll}.
     */
    function isApprovedForAll(address owner, address operator)
        public
        view
        virtual
        override
        returns (bool)
    {
        return _operatorApprovals[owner][operator];
    }

    /**
     * @dev Safely transfers `tokenId` token from `from` to `to`, checking first that contract recipients
     * are aware of the ERC721 protocol to prevent tokens from being forever locked.
     *
     * `_data` is additional data, it has no specified format and it is sent in call to `to`.
     *
     * This internal function is equivalent to {safeTransferFrom}, and can be used to e.g.
     * implement alternative mechanisms to perform token transfer, such as signature-based.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must exist and be owned by `from`.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function _safeTransfer(
        address from,
        address to,
        uint256 tokenId,
        bytes memory _data
    ) internal virtual {
        _transfer(from, to, tokenId);
        require(
            _checkOnERC721Received(from, to, tokenId, _data),
            "ERC721: transfer to non ERC721Receiver implementer"
        );
    }

    /**
     * @dev Returns whether `tokenId` exists.
     *
     * Tokens can be managed by their owner or approved accounts via {approve} or {setApprovalForAll}.
     *
     * Tokens start existing when they are minted (`_mint`),
     * and stop existing when they are burned (`_burn`).
     */
    function _exists(uint256 tokenId) internal view virtual returns (bool) {
        return _owners[tokenId] != address(0);
    }

    /**
     * @dev Returns whether `spender` is allowed to manage `tokenId`.
     *
     * Requirements:
     *
     * - `tokenId` must exist.
     */
    function _isApprovedOrOwner(address spender, uint256 tokenId)
        internal
        view
        virtual
        returns (bool)
    {
        require(
            _exists(tokenId),
            "ERC721: operator query for nonexistent token"
        );
        address owner = ERC721.ownerOf(tokenId);
        return (spender == owner ||
            getApproved(tokenId) == spender ||
            isApprovedForAll(owner, spender));
    }

    /**
     * @dev Safely mints `tokenId` and transfers it to `to`.
     *
     * Requirements:
     *
     * - `tokenId` must not exist.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function _safeMint(address to, uint256 tokenId) internal virtual {
        _safeMint(to, tokenId, "");
    }

    /**
     * @dev Same as {xref-ERC721-_safeMint-address-uint256-}[`_safeMint`], with an additional `data` parameter which is
     * forwarded in {IERC721Receiver-onERC721Received} to contract recipients.
     */
    function _safeMint(
        address to,
        uint256 tokenId,
        bytes memory _data
    ) internal virtual {
        _mint(to, tokenId);
        require(
            _checkOnERC721Received(address(0), to, tokenId, _data),
            "ERC721: transfer to non ERC721Receiver implementer"
        );
    }

    /**
     * @dev Mints `tokenId` and transfers it to `to`.
     *
     * WARNING: Usage of this method is discouraged, use {_safeMint} whenever possible
     *
     * Requirements:
     *
     * - `tokenId` must not exist.
     * - `to` cannot be the zero address.
     *
     * Emits a {Transfer} event.
     */
    function _mint(address to, uint256 tokenId) internal virtual {
        require(to != address(0), "ERC721: mint to the zero address");
        require(!_exists(tokenId), "ERC721: token already minted");

        _beforeTokenTransfer(address(0), to, tokenId);

        _balances[to] += 1;
        _owners[tokenId] = to;

        emit Transfer(address(0), to, tokenId);
    }

    /**
     * @dev Destroys `tokenId`.
     * The approval is cleared when the token is burned.
     *
     * Requirements:
     *
     * - `tokenId` must exist.
     *
     * Emits a {Transfer} event.
     */
    function _burn(uint256 tokenId) internal virtual {
        address owner = ERC721.ownerOf(tokenId);

        _beforeTokenTransfer(owner, address(0), tokenId);

        // Clear approvals
        _approve(address(0), tokenId);

        _balances[owner] -= 1;
        delete _owners[tokenId];

        emit Transfer(owner, address(0), tokenId);
    }

    /**
     * @dev Transfers `tokenId` from `from` to `to`.
     *  As opposed to {transferFrom}, this imposes no restrictions on msg.sender.
     *
     * Requirements:
     *
     * - `to` cannot be the zero address.
     * - `tokenId` token must be owned by `from`.
     *
     * Emits a {Transfer} event.
     */
    function _transfer(
        address from,
        address to,
        uint256 tokenId
    ) internal virtual {
        require(
            ERC721.ownerOf(tokenId) == from,
            "ERC721: transfer of token that is not own"
        );
        require(to != address(0), "ERC721: transfer to the zero address");

        _beforeTokenTransfer(from, to, tokenId);

        // Clear approvals from the previous owner
        _approve(address(0), tokenId);

        _balances[from] -= 1;
        _balances[to] += 1;
        _owners[tokenId] = to;

        emit Transfer(from, to, tokenId);
    }

    function _transferMarketNft(
        address from,
        address to,
        uint256 tokenId
    ) internal virtual {
        require(
            ERC721.ownerOf(tokenId) == from,
            "ERC721: transfer of token that is not own"
        );
        require(_exists(tokenId), "ERC721: token already minted");
        require(to != address(0), "ERC721: transfer to the zero address");

        _beforeTokenTransfer(from, to, tokenId);

        // Clear approvals from the previous owner
        _approve(address(0), tokenId);

        _balances[from] -= 1;
        _balances[to] += 1;
        _owners[tokenId] = to;
    }

    /**
     * @dev Approve `to` to operate on `tokenId`
     *
     * Emits a {Approval} event.
     */
    function _approve(address to, uint256 tokenId) internal virtual {
        _tokenApprovals[tokenId] = to;
        emit Approval(ERC721.ownerOf(tokenId), to, tokenId);
    }

    /**
     * @dev Internal function to invoke {IERC721Receiver-onERC721Received} on a target address.
     * The call is not executed if the target address is not a contract.
     *
     * @param from address representing the previous owner of the given token ID
     * @param to target address that will receive the tokens
     * @param tokenId uint256 ID of the token to be transferred
     * @param _data bytes optional data to send along with the call
     * @return bool whether the call correctly returned the expected magic value
     */
    function _checkOnERC721Received(
        address from,
        address to,
        uint256 tokenId,
        bytes memory _data
    ) private returns (bool) {
        if (to.isContract()) {
            try
                IERC721Receiver(to).onERC721Received(
                    _msgSender(),
                    from,
                    tokenId,
                    _data
                )
            returns (bytes4 retval) {
                return retval == IERC721Receiver.onERC721Received.selector;
            } catch (bytes memory reason) {
                if (reason.length == 0) {
                    revert(
                        "ERC721: transfer to non ERC721Receiver implementer"
                    );
                } else {
                    assembly {
                        revert(add(32, reason), mload(reason))
                    }
                }
            }
        } else {
            return true;
        }
    }

    /**
     * @dev Hook that is called before any token transfer. This includes minting
     * and burning.
     *
     * Calling conditions:
     *
     * - When `from` and `to` are both non-zero, ``from``'s `tokenId` will be
     * transferred to `to`.
     * - When `from` is zero, `tokenId` will be minted for `to`.
     * - When `to` is zero, ``from``'s `tokenId` will be burned.
     * - `from` and `to` are never both zero.
     *
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     */
    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 tokenId
    ) internal virtual {}
}

/**
 * @dev This implements an optional extension of {ERC721} defined in the EIP that adds
 * enumerability of all the token ids in the contract as well as all token ids owned by each
 * account.
 */
abstract contract ERC721Enumerable is ERC721, IERC721Enumerable {
    // Mapping from owner to list of owned token IDs
    mapping(address => mapping(uint256 => uint256)) private _ownedTokens;

    // Mapping from token ID to index of the owner tokens list
    mapping(uint256 => uint256) private _ownedTokensIndex;

    // Array with all token ids, used for enumeration
    uint256[] private _allTokens;

    // Mapping from token id to position in the allTokens array
    mapping(uint256 => uint256) private _allTokensIndex;

    /**
     * @dev See {IERC165-supportsInterface}.
     */
    function supportsInterface(bytes4 interfaceId)
        public
        view
        virtual
        override(IERC165, ERC721)
        returns (bool)
    {
        return
            interfaceId == type(IERC721Enumerable).interfaceId ||
            super.supportsInterface(interfaceId);
    }

    /**
     * @dev See {IERC721Enumerable-tokenOfOwnerByIndex}.
     */
    function tokenOfOwnerByIndex(address owner, uint256 index)
        public
        view
        virtual
        override
        returns (uint256)
    {
        require(
            index < ERC721.balanceOf(owner),
            "ERC721Enumerable: owner index out of bounds"
        );
        return _ownedTokens[owner][index];
    }

    /**
     * @dev See {IERC721Enumerable-totalSupply}.
     */
    function totalSupply() public view virtual override returns (uint256) {
        return _allTokens.length;
    }

    /**
     * @dev See {IERC721Enumerable-tokenByIndex}.
     */
    function tokenByIndex(uint256 index)
        public
        view
        virtual
        override
        returns (uint256)
    {
        require(
            index < ERC721Enumerable.totalSupply(),
            "ERC721Enumerable: global index out of bounds"
        );
        return _allTokens[index];
    }

    /**
     * @dev Hook that is called before any token transfer. This includes minting
     * and burning.
     *
     * Calling conditions:
     *
     * - When `from` and `to` are both non-zero, ``from``'s `tokenId` will be
     * transferred to `to`.
     * - When `from` is zero, `tokenId` will be minted for `to`.
     * - When `to` is zero, ``from``'s `tokenId` will be burned.
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     *
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     */
    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 tokenId
    ) internal virtual override {
        super._beforeTokenTransfer(from, to, tokenId);

        if (from == address(0)) {
            _addTokenToAllTokensEnumeration(tokenId);
        } else if (from != to) {
            _removeTokenFromOwnerEnumeration(from, tokenId);
        }
        if (to == address(0)) {
            _removeTokenFromAllTokensEnumeration(tokenId);
        } else if (to != from) {
            _addTokenToOwnerEnumeration(to, tokenId);
        }
    }

    /**
     * @dev Private function to add a token to this extension's ownership-tracking data structures.
     * @param to address representing the new owner of the given token ID
     * @param tokenId uint256 ID of the token to be added to the tokens list of the given address
     */
    function _addTokenToOwnerEnumeration(address to, uint256 tokenId) private {
        uint256 length = ERC721.balanceOf(to);
        _ownedTokens[to][length] = tokenId;
        _ownedTokensIndex[tokenId] = length;
    }

    /**
     * @dev Private function to add a token to this extension's token tracking data structures.
     * @param tokenId uint256 ID of the token to be added to the tokens list
     */
    function _addTokenToAllTokensEnumeration(uint256 tokenId) private {
        _allTokensIndex[tokenId] = _allTokens.length;
        _allTokens.push(tokenId);
    }

    /**
     * @dev Private function to remove a token from this extension's ownership-tracking data structures. Note that
     * while the token is not assigned a new owner, the `_ownedTokensIndex` mapping is _not_ updated: this allows for
     * gas optimizations e.g. when performing a transfer operation (avoiding double writes).
     * This has O(1) time complexity, but alters the order of the _ownedTokens array.
     * @param from address representing the previous owner of the given token ID
     * @param tokenId uint256 ID of the token to be removed from the tokens list of the given address
     */
    function _removeTokenFromOwnerEnumeration(address from, uint256 tokenId)
        private
    {
        // To prevent a gap in from's tokens array, we store the last token in the index of the token to delete, and
        // then delete the last slot (swap and pop).

        uint256 lastTokenIndex = ERC721.balanceOf(from) - 1;
        uint256 tokenIndex = _ownedTokensIndex[tokenId];

        // When the token to delete is the last token, the swap operation is unnecessary
        if (tokenIndex != lastTokenIndex) {
            uint256 lastTokenId = _ownedTokens[from][lastTokenIndex];

            _ownedTokens[from][tokenIndex] = lastTokenId; // Move the last token to the slot of the to-delete token
            _ownedTokensIndex[lastTokenId] = tokenIndex; // Update the moved token's index
        }

        // This also deletes the contents at the last position of the array
        delete _ownedTokensIndex[tokenId];
        delete _ownedTokens[from][lastTokenIndex];
    }

    /**
     * @dev Private function to remove a token from this extension's token tracking data structures.
     * This has O(1) time complexity, but alters the order of the _allTokens array.
     * @param tokenId uint256 ID of the token to be removed from the tokens list
     */
    function _removeTokenFromAllTokensEnumeration(uint256 tokenId) private {
        // To prevent a gap in the tokens array, we store the last token in the index of the token to delete, and
        // then delete the last slot (swap and pop).

        uint256 lastTokenIndex = _allTokens.length - 1;
        uint256 tokenIndex = _allTokensIndex[tokenId];

        // When the token to delete is the last token, the swap operation is unnecessary. However, since this occurs so
        // rarely (when the last minted token is burnt) that we still do the swap here to avoid the gas cost of adding
        // an 'if' statement (like in _removeTokenFromOwnerEnumeration)
        uint256 lastTokenId = _allTokens[lastTokenIndex];

        _allTokens[tokenIndex] = lastTokenId; // Move the last token to the slot of the to-delete token
        _allTokensIndex[lastTokenId] = tokenIndex; // Update the moved token's index

        // This also deletes the contents at the last position of the array
        delete _allTokensIndex[tokenId];
        _allTokens.pop();
    }
}

/**
 * @title BEP20 interface
 * @dev see https://eips.ethereum.org/EIPS/eip-20
 */
interface IBEP20 {
    function transfer(address to, uint256 value) external returns (bool);

    function approve(address spender, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    function totalSupply() external view returns (uint256);

    function balanceOf(address who) external view returns (uint256);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

interface IBEP20Metadata is IBEP20 {
    /**
     * @dev Returns the name of the token.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the symbol of the token.
     */
    function symbol() external view returns (string memory);

    /**
     * @dev Returns the decimals places of the token.
     */
    function decimals() external view returns (uint8);
}

/**
 * @dev Implementation of the {IBEP20} interface.
 *
 * This implementation is agnostic to the way tokens are created. This means
 * that a supply mechanism has to be added in a derived contract using {_mint}.
 * For a generic mechanism see {BEP20PresetMinterPauser}.
 *
 * instead returning `false` on failure. This behavior is nonetheless
 * conventional and does not conflict with the expectations of BEP20
 * applications.
 *
 * Additionally, an {Approval} event is emitted on calls to {transferFrom}.
 * This allows applications to reconstruct the allowance for all accounts just
 * by listening to said events. Other implementations of the EIP may not emit
 * these events, as it isn't required by the specification.
 *
 * Finally, the non-standard {decreaseAllowance} and {increaseAllowance}
 * functions have been added to mitigate the well-known issues around setting
 * allowances. See {IBEP20-approve}.
 */
abstract contract BEP20 is Context, IBEP20Metadata {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    string private _name;
    string private _symbol;
    uint256 private _totalSupply;

    /**
     * @dev Sets the values for {name} and {symbol}.
     *
     * The default value of {decimals} is 18. To select a different value for
     * {decimals} you should overload it.
     *
     * All two of these values are immutable: they can only be set once during
     * construction.
     */
    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    /**
     * @dev Returns the name of the token.
     */
    function name() public view virtual override returns (string memory) {
        return _name;
    }

    /**
     * @dev Returns the symbol of the token, usually a shorter version of the
     * name.
     */
    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    /**
     * @dev Returns the number of decimals used to get its user representation.
     * For example, if `decimals` equals `2`, a balance of `505` tokens should
     * be displayed to a user as `5.05` (`505 / 10 ** 2`).
     *
     * Tokens usually opt for a value of 18, imitating the relationship between
     * Ether and Wei. This is the value {BEP20} uses, unless this function is
     * overridden;
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * no way affects any of the arithmetic of the contract, including
     * {IBEP20-balanceOf} and {IBEP20-transfer}.
     */
    function decimals() public view virtual override returns (uint8) {
        return 18;
    }

    /**
     * @dev See {IBEP20-totalSupply}.
     */
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    /**
     * @dev See {IBEP20-balanceOf}.
     */
    function balanceOf(address account)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _balances[account];
    }

    /**
     * @dev See {IBEP20-transfer}.
     *
     * Requirements:
     *
     * - `to` cannot be the zero address.
     * - the caller must have a balance of at least `amount`.
     */
    function transfer(address to, uint256 amount)
        public
        virtual
        override
        returns (bool)
    {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }

    /**
     * @dev See {IBEP20-allowance}.
     */
    function allowance(address owner, address spender)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    /**
     * @dev See {IBEP20-approve}.
     *
     * NOTE: If `amount` is the maximum `uint256`, the allowance is not updated on
     * `transferFrom`. This is semantically equivalent to an infinite approval.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 amount)
        public
        virtual
        override
        returns (bool)
    {
        address owner = _msgSender();
        _approve(owner, spender, amount);
        return true;
    }

    /**
     * @dev See {IBEP20-transferFrom}.
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {BEP20}.
     *
     * NOTE: Does not update the allowance if the current allowance
     * is the maximum `uint256`.
     *
     * Requirements:
     *
     * - `from` and `to` cannot be the zero address.
     * - `from` must have a balance of at least `amount`.
     * - the caller must have allowance for ``from``'s tokens of at least
     * `amount`.
     */
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    /**
     * @dev Atomically increases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IBEP20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function increaseAllowance(address spender, uint256 addedValue)
        public
        virtual
        returns (bool)
    {
        address owner = _msgSender();
        _approve(owner, spender, _allowances[owner][spender] + addedValue);
        return true;
    }

    /**
     * @dev Atomically decreases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IBEP20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     * - `spender` must have allowance for the caller of at least
     * `subtractedValue`.
     */
    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        virtual
        returns (bool)
    {
        address owner = _msgSender();
        uint256 currentAllowance = _allowances[owner][spender];
        require(
            currentAllowance >= subtractedValue,
            "BEP20: decreased allowance below zero"
        );
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    /**
     * @dev Moves `amount` of tokens from `sender` to `recipient`.
     *
     * This internal function is equivalent to {transfer}, and can be used to
     * e.g. implement automatic token fees, slashing mechanisms, etc.
     *
     * Emits a {Transfer} event.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `from` must have a balance of at least `amount`.
     */
    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        require(from != address(0), "BEP20: transfer from the zero address");
        require(to != address(0), "BEP20: transfer to the zero address");

        _beforeTokenTransfer(from, to, amount);

        uint256 fromBalance = _balances[from];
        require(
            fromBalance >= amount,
            "BEP20: transfer amount exceeds balance"
        );
        unchecked {
            _balances[from] = fromBalance - amount;
        }
        _balances[to] += amount;

        emit Transfer(from, to, amount);

        _afterTokenTransfer(from, to, amount);
    }

    /** @dev Creates `amount` tokens and assigns them to `account`, increasing
     * the total supply.
     *
     * Emits a {Transfer} event with `from` set to the zero address.
     *
     * Requirements:
     *
     * - `account` cannot be the zero address.
     */
    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "BEP20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(address(0), account, amount);
    }

    /**
     * @dev Destroys `amount` tokens from `account`, reducing the
     * total supply.
     *
     * Emits a {Transfer} event with `to` set to the zero address.
     *
     * Requirements:
     *
     * - `account` cannot be the zero address.
     * - `account` must have at least `amount` tokens.
     */
    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "BEP20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "BEP20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
        }
        _totalSupply -= amount;

        emit Transfer(account, address(0), amount);

        _afterTokenTransfer(account, address(0), amount);
    }

    /**
     * @dev Sets `amount` as the allowance of `spender` over the `owner` s tokens.
     *
     * This internal function is equivalent to `approve`, and can be used to
     * e.g. set automatic allowances for certain subsystems, etc.
     *
     * Emits an {Approval} event.
     *
     * Requirements:
     *
     * - `owner` cannot be the zero address.
     * - `spender` cannot be the zero address.
     */
    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    /**
     * @dev Spend `amount` form the allowance of `owner` toward `spender`.
     *
     * Does not update the allowance amount in case of infinite allowance.
     * Revert if not enough allowance is available.
     *
     * Might emit an {Approval} event.
     */
    function _spendAllowance(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(
                currentAllowance >= amount,
                "BEP20: insufficient allowance"
            );
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }

    /**
     * @dev Hook that is called before any transfer of tokens. This includes
     * minting and burning.
     *
     * Calling conditions:
     *
     * - when `from` and `to` are both non-zero, `amount` of ``from``'s tokens
     * will be transferred to `to`.
     * - when `from` is zero, `amount` tokens will be minted for `to`.
     * - when `to` is zero, `amount` of ``from``'s tokens will be burned.
     * - `from` and `to` are never both zero.
     *
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     */
    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}

    /**
     * @dev Hook that is called after any transfer of tokens. This includes
     * minting and burning.
     *
     * Calling conditions:
     *
     * - when `from` and `to` are both non-zero, `amount` of ``from``'s tokens
     * has been transferred to `to`.
     * - when `from` is zero, `amount` tokens have been minted for `to`.
     * - when `to` is zero, `amount` of ``from``'s tokens have been burned.
     * - `from` and `to` are never both zero.
     *
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     */
    function _afterTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
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
    // amount. Since refunds are capped to a pBEPentage of the total
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

    modifier isHuman() {
        require(tx.origin == msg.sender, "sorry humans only");
        _;
    }
}

contract MerketNFT is ERC721Enumerable, Ownable, ReentrancyGuard {
    using SafeMath for uint256;
    using Strings for uint256;
    using Address for address;

    IBEP20 public _stakingToken;
    string public baseURI;
    uint256 private _costMintValue;
    uint256 private _transaction_fee;
    uint256 private _transfer_cost;
    uint256 private _unstake_percent;
    uint256 private _DECIMAL_PERCENT = 100;
    uint256 private _FACTOR_PERCENT = 10000;
    uint256 public maxMintAmount = 10;
    uint256 public maxMintSupply = 20000;    

    //TimeLock mapping to TimeLock information
    struct TimeLockInfo {
        uint256 timeLock;
        uint256 yield;
        bool exists;
    }
    mapping(uint256 => TimeLockInfo) private timeLocks;
    uint256[] private timeLockKeys;

    //NFTs Base with Benefits
    uint256 lastItemId = 1;
    struct NFTBenefitItem {
        uint256 itemId;
        string name;
        string rarity; //comum, raro, epic, lendário, ruby
        uint256 price;
        uint256 percentage;
        string tokenURI;
        bool status;
    }
    mapping(uint256 => NFTBenefitItem) public NFTBeneftList;

    //Staking NFTs
    struct NFTStake {
        address ownerNftAddr;
        uint256 yieldPercent;
        uint256 dailyRate;
        uint256 totalLocked;
        uint256 totalStaked;
        uint256 totalNFTs;
        uint256 startDate;
        uint256 lastDate;
        uint256 timeLock;
        bool staked;
    }
    mapping(address => mapping(uint256 => NFTStake)) public NFTStakeInfo;
    
    //Mapping NFTs in staking for user
    mapping(address => uint256[]) private userStakedNfts;
    //This mapping tracks whether a specific NFT is staked
    mapping(address => mapping(uint256 => bool)) private nftIsStaked;

    //Multple Mint
    struct ReceiversMint {
        address Wallet;
        string UriToken;
    }
    mapping(uint256 => string) private _UriTokens;
    mapping(uint256 => uint256) private _itemIds;

    //Buy Fee List
    struct marketNft {
        uint256 tokenId;
        address ownerNftAddr;
        uint256 price;
        bool exist;
    }
    mapping(uint256 => marketNft) private marketNfts;

    bool private inTransfer;
    bool public paused = false;
    modifier isPausable() {
        require(!paused, "The Contract is paused. Mintable is paused");
        _;
    }
    mapping(address => bool) public whitelisted;
    address _companyAddress;

    constructor(
        string memory name,
        string memory symbol,
        address stakingToken,
        uint256 costOfMint,
        uint256 transferCost,
        uint256 transactFee,
        uint256 unstake_percent,
        address companyAddress,
        string memory _initBaseURI
    ) ERC721(name, symbol) {
        _owner = msg.sender;
        _stakingToken = IBEP20(stakingToken);
        _costMintValue = costOfMint;
        _transfer_cost = transferCost;
        _transaction_fee = transactFee;
        _unstake_percent = unstake_percent;
        _companyAddress = companyAddress;
        setBaseURI(_initBaseURI);
        mint(
            msg.sender,
            "bafybeihy3wuwxpldh27shubfvl4klzbjioyhgv6hn3gmqyvud6pr6symiq"
        );
    }

    /**
     * @dev Return fee amounts. Market Sale on NFT!
     */
    function getMintCostValue() public view returns (uint256) {
        return _costMintValue;
    }

    /**
     * @dev Return fee amounts. Market Sale on NFT!
     */
    function getTransactionFee() public view returns (uint256) {
        return _transaction_fee;
    }

    /**
     * @dev Transfer rate of nfts between users.
     */
    function getTransferFee() public view returns (uint256) {
        return _transfer_cost;
    }

    /**
     * @dev Return fee percentage of Unstake fee.
     */
    function getUnstakeFeePercent() public view returns (uint256) {
        return _unstake_percent;
    }

    /**
     * @dev Withdrall address by company.!
     */
    function getWithdrawAddress() public view returns (address) {
        return _companyAddress;
    }

    /**
     * @dev Get one or All NFTs in staking of user!
     */
    function getUserStakedNfts(address user)
        public view returns (uint256[] memory)
    {
        return userStakedNfts[user];
    }

    /*get token URI of NFT*/
    function tokenURI(uint256 tokenId)
        public
        view
        virtual
        override
        returns (string memory)
    {
        require(_exists(tokenId), "ERC721Metadata: URI query for nonexistent token");
        string memory currentBaseURI = _baseURI();
        string memory _tokenURI = _UriTokens[tokenId];
        return bytes(currentBaseURI).length > 0 ? string(abi.encodePacked(currentBaseURI, _tokenURI)) : "";
    }

    /*get Item Id of NFT*/
    function getItemId(uint256 tokenId) public view returns (uint256) {
        require(_exists(tokenId), "ERC721Metadata: itemId query for nonexistent token");
        return _itemIds[tokenId];
    }

    /*get getNFTDetails*/
    function getNftSaleDetailsForId(uint256 tokenId)
        public
        view
        returns (
            address ownerNftAddr,
            uint256 price,
            bool exist
        )
    {
        return (
            marketNfts[tokenId].ownerNftAddr,
            marketNfts[tokenId].price,
            marketNfts[tokenId].exist
        );
    }

    /*get list of ids Nfts User*/
    function getNftsUser(address _owner)
        public view returns (uint256[] memory)
    {
        uint256 ownerTokenCount = balanceOf(_owner);
        uint256[] memory tokenIds = new uint256[](ownerTokenCount);
        for (uint256 i; i < ownerTokenCount; i++) {
            tokenIds[i] = tokenOfOwnerByIndex(_owner, i);
        }
        return tokenIds;
    }

    /*Function to get the timeLock information*/
    function getTimeLock(uint256 _timeLock) public view returns (uint256, uint256) {
        if(!timeLocks[_timeLock].exists){return (0,0);}
        return (timeLocks[_timeLock].timeLock, timeLocks[_timeLock].yield);
    }

    /*get all TimeLocks registered*/
    function getTimeLockKeys()
        public view returns (uint256[] memory)
    {
        return timeLockKeys;
    }

    /*Base Url of get NFT IPFS*/
    function _baseURI() internal view virtual override returns (string memory) {
        return baseURI;
    }

    /**
     * @dev Enables the contract to receive BNB.
     */
    receive() external payable {}
    fallback() external payable {}

    //Global Mint NFT
    function mint(address _to, string memory _uriToken)
        public
        payable
        isPausable
    {
        uint256 supply = totalSupply();
        uint256 newId = supply + 1;
        
        if (msg.sender != owner() && !whitelisted[msg.sender]) {
            require(msg.value >= _costMintValue, "Insufficient value for the mintage fee");
        }

        _safeMint(_to, newId);
        _setItemID(newId, 0);
        _setUriToken(newId, _uriToken);
    }

    // Multiples Mintable NFTs
    function multMint(ReceiversMint[] memory _mintAmount)
        public
        payable
        isPausable
    {
        uint256 supply = totalSupply();
        require(_mintAmount.length > 0);
        require(_mintAmount.length <= maxMintAmount);
        require(supply + _mintAmount.length <= maxMintSupply);        
        
        if (msg.sender != owner() && !whitelisted[msg.sender]) {
            require(msg.value >= _costMintValue, "Insufficient value for the mintage fee");
        }

        for (uint256 i = 0; i < _mintAmount.length; i++) {
            uint256 newId = supply + i;
            _safeMint(_mintAmount[i].Wallet, newId);
            _setItemID(newId, 0);
            _setUriToken(newId, _mintAmount[i].UriToken);
        }
    }

    //Benefits Mint NFT
    function mintBenefit(address _to, uint256 itemId, string memory _uriToken)
        public onlyOwner isPausable
    {
        require(NFTBeneftList[itemId].status, "Unable to locate the benefit");
        uint256 supply = totalSupply();
        uint256 newId = supply + 1;

        _safeMint(_to, newId);
        _setItemID(newId, itemId);
        _setUriToken(newId, _uriToken);
    }

    function buyWithBenefit(uint256 tokenId) public isPausable {
        NFTBenefitItem storage itemBeneft = NFTBeneftList[getItemId(tokenId)];
        require(_exists(tokenId), "ERC721: token already minted");
        if (msg.sender != owner() && !whitelisted[msg.sender]) {
            uint256 dexBalanceToken = _stakingToken.balanceOf(msg.sender);                
            require(dexBalanceToken >= itemBeneft.price, "Insufficient value for the transaction fee");
            _stakingToken.transferFrom(msg.sender, _companyAddress, itemBeneft.price);
        }

        if (!inTransfer) {
            inTransfer = true;
            _transferMarketNft(_owner, msg.sender, tokenId);
            emit TransferMerktNft(_owner, msg.sender, tokenId);
            inTransfer = false;
        }
    }

    // User Add NFT on Sale
    function addNftOnSale(uint256 tokenId, uint256 _price) public isPausable {
        address _OwnerAddr = msg.sender;
        require(
            _OwnerAddr == ownerOf(tokenId),
            "It is not possible to sell an NFT that you do not own."
        );
        require(_exists(tokenId), "Could not find this Nft Token");
        require(
            !marketNfts[tokenId].exist,
            "A NFT already exists, created for sale"
        );
        require(!nftIsStaked[msg.sender][tokenId], "NFT is already staked");

        _approve(address(this), tokenId);
        marketNfts[tokenId].tokenId = tokenId;
        marketNfts[tokenId].ownerNftAddr = _OwnerAddr;
        marketNfts[tokenId].price = _price;
        marketNfts[tokenId].exist = true;
    }

    // User Edit NFT on Sale
    function editNftOnSale(uint256 tokenId, uint256 _price) public isPausable {
        address _OwnerAddr = msg.sender;
        require(
            _OwnerAddr == ownerOf(tokenId),
            "It is not possible to edit an NFT that you do not own."
        );
        require(_exists(tokenId), "Could not find this Nft Token");
        require(_price > 0, "You need to enter a valid value");
        require(
            marketNfts[tokenId].exist,
            "The NFT was not found. Check the ID entered"
        );
        marketNfts[tokenId].price = _price;
    }

    // User Delete NFT on Sale
    function deleteNftOnSale(uint256 tokenId) public isPausable {
        address _OwnerAddr = msg.sender;
        require(
            _OwnerAddr == ownerOf(tokenId),
            "It is not possible to delete an NFT that you do not own."
        );
        require(_exists(tokenId), "Could not find this Nft Token");
        require(
            marketNfts[tokenId].exist,
            "The NFT was not found. Check the ID entered"
        );

        // Clear approvals from the previous owner
        _approve(address(0), tokenId);

        delete marketNfts[tokenId];
    }

    function sendStake(uint256 tokenId, uint256 timeLock) external isPausable {
        NFTBenefitItem storage itemBeneft = NFTBeneftList[getItemId(tokenId)];
        require(
            msg.sender == ownerOf(tokenId),
            "It is not possible to staking an NFT that you do not own."
        );
        require(_exists(tokenId), "NFT does not exist");
        require(timeLocks[timeLock].exists, "Invalid time lock");
        require(!nftIsStaked[msg.sender][tokenId], "NFT is already staked");
        require(itemBeneft.status, "Unable to locate the benefit");

        uint256 benefit = itemBeneft.percentage;
        uint256 yield = timeLocks[timeLock].yield;
        uint256 yieldPercent = yield.mul(benefit).div(_FACTOR_PERCENT);

        if(!NFTStakeInfo[msg.sender][timeLock].staked){
            NFTStakeInfo[msg.sender][timeLock].startDate = block.timestamp;
            yieldPercent += yield;
        }

        uint256 dailyRate = DailyRate(yieldPercent);
        uint256 daysStaked = (block.timestamp - NFTStakeInfo[msg.sender][timeLock].startDate) / 1 days;
        if (daysStaked >= 1) {
            uint256 renderBonus = recalculateBonus(timeLock);
            NFTStakeInfo[msg.sender][timeLock].totalStaked += renderBonus;
            NFTStakeInfo[msg.sender][timeLock].lastDate = block.timestamp;
        }

        //Update NFT's Staked Status
        NFTStakeInfo[msg.sender][timeLock].ownerNftAddr = msg.sender;
        NFTStakeInfo[msg.sender][timeLock].yieldPercent += yieldPercent;
        NFTStakeInfo[msg.sender][timeLock].dailyRate = dailyRate;
        NFTStakeInfo[msg.sender][timeLock].totalLocked += itemBeneft.price;
        NFTStakeInfo[msg.sender][timeLock].timeLock = timeLock;
        NFTStakeInfo[msg.sender][timeLock].totalNFTs += 1;
        NFTStakeInfo[msg.sender][timeLock].staked = true;
        // Add the (NFT) to the user's list of staked
        userStakedNfts[msg.sender].push(tokenId);
        nftIsStaked[msg.sender][tokenId] = true;

        emit NFTStaked(msg.sender, tokenId, block.timestamp, yieldPercent);
    }

    function unstake(uint256 timeLock) external isPausable {
        NFTStake memory StkInfo = NFTStakeInfo[msg.sender][timeLock];
        uint256 totalBonus = calculateBonus(msg.sender, timeLock);
        uint256 daysStaked = (block.timestamp - StkInfo.startDate) / 1 days;
        if (daysStaked < timeLock) {
            uint256 unlockFee = totalBonus.mul(_unstake_percent).div(100);
            totalBonus = totalBonus.sub(unlockFee);
        }

        //Gets the matrix of NFTs that the user has bet on
        uint256[] storage userNfts = userStakedNfts[msg.sender];
        //Scrolls through the matrix and removes the bets
        for (uint256 i = 0; i < userNfts.length; i++) {            
            //Set the value in the nftIsStaked mapping to false
            nftIsStaked[msg.sender][userNfts[i]] = false;
        }

        //Remove NFT's Staked Status
        delete NFTStakeInfo[msg.sender][timeLock];
        delete userStakedNfts[msg.sender];
        nftIsStaked[msg.sender][timeLock] = false;

        require(
            _stakingToken.transfer(msg.sender, totalBonus),
            "A transaction error has occurred. Please try again."
        );
        emit NftUnstaked(msg.sender, totalBonus);
    }

    function claimStake(uint256 timeLock) external isPausable {
        NFTStake memory StkInfo = NFTStakeInfo[msg.sender][timeLock];
        uint256 totalBonus = calculateBonus(msg.sender, timeLock);
        uint256 daysStaked = (block.timestamp - StkInfo.startDate) / 1 days;
        if (daysStaked < timeLock) {
            uint256 unlockFee = totalBonus.mul(_unstake_percent).div(100);
            totalBonus = totalBonus.sub(unlockFee);
        }
        require(totalBonus > 0, "There is not enough balance for this claim");
        require(
            _stakingToken.transfer(msg.sender, totalBonus),
            "A transaction error has occurred. Please try again."
        );

        NFTStakeInfo[msg.sender][timeLock].totalStaked = 0;
        NFTStakeInfo[msg.sender][timeLock].startDate = block.timestamp;
        NFTStakeInfo[msg.sender][timeLock].lastDate = 0;

        emit ClaimStaked(msg.sender, totalBonus);
    }
    
    function calculateBonus(address account, uint256 timeLock) public view returns (uint256) {
        NFTStake memory StkInfo = NFTStakeInfo[account][timeLock];
        uint256 startStaked = (block.timestamp - StkInfo.startDate) / 1 days;
        uint256 daysStaked = startStaked;
        if(!StkInfo.staked || daysStaked == 0){
            return 0;
        }

        if(StkInfo.lastDate > 0){
            daysStaked = (block.timestamp - StkInfo.lastDate) / 1 days;
        }
        
        if (startStaked >= timeLock) {
            daysStaked = timeLock;
        }

        //Get the daily rate of return
        uint256 dailyStakeValue = (StkInfo.totalLocked).mul(StkInfo.yieldPercent).div(_FACTOR_PERCENT);
        uint256 dailyRateValue = (dailyStakeValue).div(12).div(30);
        uint256 bonus = dailyRateValue.mul(daysStaked);
        //Adds the bonus to the bonus total
        return bonus + StkInfo.totalStaked;
    }

    function recalculateBonus(uint256 timeLock) internal view returns (uint256) {
        NFTStake memory StkInfo = NFTStakeInfo[msg.sender][timeLock];
        uint256 startStaked = (block.timestamp - StkInfo.startDate) / 1 days;
        uint256 lastStaked = StkInfo.lastDate == 0 ? 0 : (block.timestamp - StkInfo.lastDate) / 1 days;
        uint256 daysStaked = 0;
        if(StkInfo.lastDate <= 0 && startStaked > 0){
            daysStaked = startStaked;
        } else {            
            daysStaked = lastStaked;
        }

        if (daysStaked > 0) {
            //Calculates the bonus since the last addition of NFT
            //Get the daily rate of return
            uint256 dailyStakeValue = (StkInfo.totalLocked).mul(StkInfo.yieldPercent).div(_FACTOR_PERCENT);
            uint256 dailyRateValue = (dailyStakeValue).div(12).div(30);
            uint256 bonus = dailyRateValue.mul(daysStaked);
            //Adds the bonus to the bonus total
            return bonus;
        }

        return 0;
    }

    /*get Percentage of Rate Daily stake*/
    function DailyRate(uint256 yieldPercent) internal view returns (uint256) {
        uint256 ratePercent = yieldPercent.mul(_FACTOR_PERCENT);
        //Calculates the monthly APR
        uint256 monthlyRate = ratePercent.div(12);
        //Calculates the daily APR
        uint256 dailyRate = monthlyRate.div(30);

        return dailyRate;
    }

    function updateStartDate(uint256 timeLock, uint256 _startDate) public onlyOwner {
        NFTStakeInfo[msg.sender][timeLock].startDate = _startDate;
    }

    function transferFrom(
        address from,
        address to,
        uint256 tokenId
    ) public payable isPausable {
        require(
            !marketNfts[tokenId].exist && !inTransfer,
            "This NFT is for sale, cancel to transfer it"
        );
        require(!nftIsStaked[msg.sender][tokenId], "NFT is already staked");
        require(
            _isApprovedOrOwner(_msgSender(), tokenId),
            "ERC721: transfer caller is not owner nor approved"
        );
        require(
            ERC721.ownerOf(tokenId) == from,
            "ERC721: transfer of token that is not own"
        );
        require(to != address(0), "ERC721: transfer to the zero address");

        if (msg.sender != owner() && !inTransfer) {
            if (!whitelisted[msg.sender]) {
                require(
                    msg.value >= _transfer_cost,
                    "it is necessary to add the shipping fee."
                );
                payable(_companyAddress).transfer(msg.value);
            }
        }

        _transfer(from, to, tokenId);
    }

    function transfer(address to, uint256 tokenId) public payable isPausable {
        address from = msg.sender;
        require(
            !marketNfts[tokenId].exist && !inTransfer,
            "This NFT is for sale, cancel to transfer it"
        );
        require(!nftIsStaked[msg.sender][tokenId], "NFT is already staked");
        require(
            _isApprovedOrOwner(_msgSender(), tokenId),
            "ERC721: transfer caller is not owner nor approved"
        );
        require(
            ERC721.ownerOf(tokenId) == from,
            "ERC721: transfer of token that is not own"
        );

        if (msg.sender != owner() && !inTransfer) {
            if (!whitelisted[msg.sender]) {
                require(
                    msg.value >= _transfer_cost,
                    "it is necessary to add the shipping fee."
                );
                payable(_companyAddress).transfer(msg.value);
            }
        }
        _transfer(from, to, tokenId);
    }

    function transferMarketNft(uint256 tokenId) public payable isPausable {
        uint256 amount = msg.value;
        address sender = marketNfts[tokenId].ownerNftAddr;
        uint256 fee = amount.mul(_transaction_fee).div(100);
        require(_exists(tokenId), "ERC721: token already minted");
        require(
            marketNfts[tokenId].exist,
            "This NFT is not for sale, check the id entered."
        );
        require(
            amount > 0 && amount >= marketNfts[tokenId].price,
            "Value does not match that of the NFT"
        );

        if (!inTransfer) {
            inTransfer = true;
            payable(_companyAddress).transfer(fee);
            payable(sender).transfer(amount.sub(fee));
            _transferMarketNft(sender, msg.sender, tokenId);
            delete marketNfts[tokenId];
            emit TransferMerktNft(sender, msg.sender, tokenId);
            inTransfer = false;
        }
    }

    /*Create Item for NFTs Benefit*/
    function createItemBenefit(
        string memory name,
        string memory rarity,
        uint256 percentage,
        uint256 price
    ) public onlyOwner {
        require(percentage <= 100, "Percentage Cannot Exceed 100%");

        NFTBeneftList[lastItemId].itemId = lastItemId;
        NFTBeneftList[lastItemId].name = name;
        NFTBeneftList[lastItemId].rarity = rarity;
        NFTBeneftList[lastItemId].percentage = percentage.mul(_DECIMAL_PERCENT);
        NFTBeneftList[lastItemId].price = price;
        NFTBeneftList[lastItemId].status = true;
        lastItemId += 1;
    }

    /*Update Item for NFTs Benefit*/
    function updateItemBenefit(
        uint256 _itemId,
        uint256 percentage,
        uint256 price,
        bool status
    ) public onlyOwner {
        require(percentage <= 100, "Percentage Cannot Exceed 100%");
        NFTBeneftList[_itemId].percentage = percentage.mul(_DECIMAL_PERCENT);
        NFTBeneftList[_itemId].price = price;
        NFTBeneftList[_itemId].status = status;
    }

    /*Create yieldId of percentage in staking*/
    function setTimeLock(uint256 _timeLock, uint256 _percentage) public onlyOwner {
        require(!timeLocks[_timeLock].exists, "TimeLock already registered");

        TimeLockInfo memory newTimeLockInfo = TimeLockInfo({
            timeLock: _timeLock, yield: _percentage.mul(_DECIMAL_PERCENT), exists: true
        });
        timeLocks[_timeLock] = newTimeLockInfo;
        timeLockKeys.push(_timeLock);
    }

    /*Update yieldId of percentage in staking*/
    function editTimeLock(uint256 _timeLock, uint256 _newPercentage, bool _status) public onlyOwner {
        require(timeLocks[_timeLock].exists, "TimeLock not registered");

        //Update timelock
        timeLocks[_timeLock].yield = _newPercentage.mul(_DECIMAL_PERCENT);
        timeLocks[_timeLock].exists = _status;
    }

    //Cost of Mint - only owner
    function setCostMint(uint256 _newCost) public onlyOwner {
        _costMintValue = _newCost;
    }

    //Set Base Url of get NFT IPFS
    function setBaseURI(string memory _newBaseURI) public onlyOwner {
        baseURI = _newBaseURI;
    }

    //Set URI to tokenID (private)
    function _setUriToken(uint256 tokenId, string memory _UriToken) internal virtual {
        require(_exists(tokenId), "ERC721Metadata: URI set of nonexistent token");
        _UriTokens[tokenId] = _UriToken;
    }

    //Set Id to tokenID (private)
    function _setItemID(uint256 tokenId, uint256 _itemId) internal virtual {
        require(_exists(tokenId), "ERC721Metadata: itemId set of nonexistent token");
        _itemIds[tokenId] = _itemId;
    }

    /**
     * @dev Update fee Percentage for withdrawals before unlocking.
     */
    function updateUnstakeFee(uint256 unstakePercent) public onlyOwner {
        require(
            unstakePercent <= 100,
            "The fee percentage cannot be more than 100%"
        );
        _unstake_percent = unstakePercent;
    }

    /**
     * @dev Update fee amounts. Market Sale on NFT! Enter only the entire fee amount.
     */
    function updateFee(uint256 _transactionFee) public onlyOwner {
        require(
            _transactionFee <= 100,
            "The fee percentage cannot be more than 100%"
        );
        _transaction_fee = _transactionFee;
    }

    /**
     * @dev Update fee amounts of Transactions. Enter only the entire fee amount Decimal.
     */
    function updateTransferCost(uint256 _transferCost) public onlyOwner {
        _transfer_cost = _transferCost;
    }

    function whitelistUser(address _user) public onlyOwner {
        if (whitelisted[_user]) {
            whitelisted[_user] = false;
        } else {
            whitelisted[_user] = true;
        }
    }

    function setCompanyAddress(address payable CompanyAddress)
        public onlyOwner
    {
        _companyAddress = CompanyAddress;
    }

    /*
     * @dev Function of compare strings in project.
     */
    function _compareString(string memory s1, string memory s2)
        private pure returns (bool)
    {
        return (keccak256(bytes(s1)) == keccak256(bytes(s2)));
    }

    function withdToBNB() public onlyOwner {
        require(
            msg.sender != address(0),
            "To make the withdrawal, you need to register a valid address."
        );
        require(
            address(this).balance > 0,
            "You do not have enough balance for this withdrawal"
        );
        payable(_companyAddress).transfer(address(this).balance);
    }

    function pause() public onlyOwner {
        if (paused) {
            paused = false;
        } else {
            paused = true;
        }
    }

    // Events
    event TransferMerktNft(address indexed from, address indexed to, uint256 tokenId);
    event NftMinted(address indexed owner, uint256 tokenId, uint256 rarity);
    event NFTStaked(address indexed user, uint256 tokenId, uint256 timestamp, uint256 yieldPercent);
    event NftUnstaked(address indexed user, uint256 bonus);
    event ClaimStaked(address indexed user, uint256 bonus);
    event BonusTokenWithdrawn(uint256 amount);
}