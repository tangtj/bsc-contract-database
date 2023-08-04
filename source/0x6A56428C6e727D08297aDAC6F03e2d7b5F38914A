// Sources flattened with hardhat v2.13.0 https://hardhat.org

// File @openzeppelin/contracts/utils/Context.sol@v4.8.2

// 
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


// File @openzeppelin/contracts/access/Ownable.sol@v4.8.2

// 
// OpenZeppelin Contracts (last updated v4.7.0) (access/Ownable.sol)

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


// File @openzeppelin/contracts/utils/introspection/IERC165.sol@v4.8.2

// 
// OpenZeppelin Contracts v4.4.1 (utils/introspection/IERC165.sol)

pragma solidity ^0.8.0;

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


// File @openzeppelin/contracts/token/ERC721/IERC721.sol@v4.8.2

// 
// OpenZeppelin Contracts (last updated v4.8.0) (token/ERC721/IERC721.sol)

pragma solidity ^0.8.0;

/**
 * @dev Required interface of an ERC721 compliant contract.
 */
interface IERC721 is IERC165 {
    /**
     * @dev Emitted when `tokenId` token is transferred from `from` to `to`.
     */
    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);

    /**
     * @dev Emitted when `owner` enables `approved` to manage the `tokenId` token.
     */
    event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);

    /**
     * @dev Emitted when `owner` enables or disables (`approved`) `operator` to manage all of its assets.
     */
    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

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
     * @dev Safely transfers `tokenId` token from `from` to `to`.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must exist and be owned by `from`.
     * - If the caller is not `from`, it must be approved to move this token by either {approve} or {setApprovalForAll}.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId,
        bytes calldata data
    ) external;

    /**
     * @dev Safely transfers `tokenId` token from `from` to `to`, checking first that contract recipients
     * are aware of the ERC721 protocol to prevent tokens from being forever locked.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must exist and be owned by `from`.
     * - If the caller is not `from`, it must have been allowed to move this token by either {approve} or {setApprovalForAll}.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

    /**
     * @dev Transfers `tokenId` token from `from` to `to`.
     *
     * WARNING: Note that the caller is responsible to confirm that the recipient is capable of receiving ERC721
     * or else they may be permanently lost. Usage of {safeTransferFrom} prevents loss, though the caller must
     * understand this adds an external call which potentially creates a reentrancy vulnerability.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must be owned by `from`.
     * - If the caller is not `from`, it must be approved to move this token by either {approve} or {setApprovalForAll}.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

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
     * @dev Returns the account approved for `tokenId` token.
     *
     * Requirements:
     *
     * - `tokenId` must exist.
     */
    function getApproved(uint256 tokenId) external view returns (address operator);

    /**
     * @dev Returns if the `operator` is allowed to manage all of the assets of `owner`.
     *
     * See {setApprovalForAll}
     */
    function isApprovedForAll(address owner, address operator) external view returns (bool);
}


// File @openzeppelin/contracts/utils/math/Math.sol@v4.8.2

// 
// OpenZeppelin Contracts (last updated v4.8.0) (utils/math/Math.sol)

pragma solidity ^0.8.0;

/**
 * @dev Standard math utilities missing in the Solidity language.
 */
library Math {
    enum Rounding {
        Down, // Toward negative infinity
        Up, // Toward infinity
        Zero // Toward zero
    }

    /**
     * @dev Returns the largest of two numbers.
     */
    function max(uint256 a, uint256 b) internal pure returns (uint256) {
        return a > b ? a : b;
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
        // (a + b) / 2 can overflow.
        return (a & b) + (a ^ b) / 2;
    }

    /**
     * @dev Returns the ceiling of the division of two numbers.
     *
     * This differs from standard division with `/` in that it rounds up instead
     * of rounding down.
     */
    function ceilDiv(uint256 a, uint256 b) internal pure returns (uint256) {
        // (a + b - 1) / b can overflow on addition, so we distribute.
        return a == 0 ? 0 : (a - 1) / b + 1;
    }

    /**
     * @notice Calculates floor(x * y / denominator) with full precision. Throws if result overflows a uint256 or denominator == 0
     * @dev Original credit to Remco Bloemen under MIT license (https://xn--2-umb.com/21/muldiv)
     * with further edits by Uniswap Labs also under MIT license.
     */
    function mulDiv(
        uint256 x,
        uint256 y,
        uint256 denominator
    ) internal pure returns (uint256 result) {
        unchecked {
            // 512-bit multiply [prod1 prod0] = x * y. Compute the product mod 2^256 and mod 2^256 - 1, then use
            // use the Chinese Remainder Theorem to reconstruct the 512 bit result. The result is stored in two 256
            // variables such that product = prod1 * 2^256 + prod0.
            uint256 prod0; // Least significant 256 bits of the product
            uint256 prod1; // Most significant 256 bits of the product
            assembly {
                let mm := mulmod(x, y, not(0))
                prod0 := mul(x, y)
                prod1 := sub(sub(mm, prod0), lt(mm, prod0))
            }

            // Handle non-overflow cases, 256 by 256 division.
            if (prod1 == 0) {
                return prod0 / denominator;
            }

            // Make sure the result is less than 2^256. Also prevents denominator == 0.
            require(denominator > prod1);

            ///////////////////////////////////////////////
            // 512 by 256 division.
            ///////////////////////////////////////////////

            // Make division exact by subtracting the remainder from [prod1 prod0].
            uint256 remainder;
            assembly {
                // Compute remainder using mulmod.
                remainder := mulmod(x, y, denominator)

                // Subtract 256 bit number from 512 bit number.
                prod1 := sub(prod1, gt(remainder, prod0))
                prod0 := sub(prod0, remainder)
            }

            // Factor powers of two out of denominator and compute largest power of two divisor of denominator. Always >= 1.
            // See https://cs.stackexchange.com/q/138556/92363.

            // Does not overflow because the denominator cannot be zero at this stage in the function.
            uint256 twos = denominator & (~denominator + 1);
            assembly {
                // Divide denominator by twos.
                denominator := div(denominator, twos)

                // Divide [prod1 prod0] by twos.
                prod0 := div(prod0, twos)

                // Flip twos such that it is 2^256 / twos. If twos is zero, then it becomes one.
                twos := add(div(sub(0, twos), twos), 1)
            }

            // Shift in bits from prod1 into prod0.
            prod0 |= prod1 * twos;

            // Invert denominator mod 2^256. Now that denominator is an odd number, it has an inverse modulo 2^256 such
            // that denominator * inv = 1 mod 2^256. Compute the inverse by starting with a seed that is correct for
            // four bits. That is, denominator * inv = 1 mod 2^4.
            uint256 inverse = (3 * denominator) ^ 2;

            // Use the Newton-Raphson iteration to improve the precision. Thanks to Hensel's lifting lemma, this also works
            // in modular arithmetic, doubling the correct bits in each step.
            inverse *= 2 - denominator * inverse; // inverse mod 2^8
            inverse *= 2 - denominator * inverse; // inverse mod 2^16
            inverse *= 2 - denominator * inverse; // inverse mod 2^32
            inverse *= 2 - denominator * inverse; // inverse mod 2^64
            inverse *= 2 - denominator * inverse; // inverse mod 2^128
            inverse *= 2 - denominator * inverse; // inverse mod 2^256

            // Because the division is now exact we can divide by multiplying with the modular inverse of denominator.
            // This will give us the correct result modulo 2^256. Since the preconditions guarantee that the outcome is
            // less than 2^256, this is the final result. We don't need to compute the high bits of the result and prod1
            // is no longer required.
            result = prod0 * inverse;
            return result;
        }
    }

    /**
     * @notice Calculates x * y / denominator with full precision, following the selected rounding direction.
     */
    function mulDiv(
        uint256 x,
        uint256 y,
        uint256 denominator,
        Rounding rounding
    ) internal pure returns (uint256) {
        uint256 result = mulDiv(x, y, denominator);
        if (rounding == Rounding.Up && mulmod(x, y, denominator) > 0) {
            result += 1;
        }
        return result;
    }

    /**
     * @dev Returns the square root of a number. If the number is not a perfect square, the value is rounded down.
     *
     * Inspired by Henry S. Warren, Jr.'s "Hacker's Delight" (Chapter 11).
     */
    function sqrt(uint256 a) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        // For our first guess, we get the biggest power of 2 which is smaller than the square root of the target.
        //
        // We know that the "msb" (most significant bit) of our target number `a` is a power of 2 such that we have
        // `msb(a) <= a < 2*msb(a)`. This value can be written `msb(a)=2**k` with `k=log2(a)`.
        //
        // This can be rewritten `2**log2(a) <= a < 2**(log2(a) + 1)`
        // → `sqrt(2**k) <= sqrt(a) < sqrt(2**(k+1))`
        // → `2**(k/2) <= sqrt(a) < 2**((k+1)/2) <= 2**(k/2 + 1)`
        //
        // Consequently, `2**(log2(a) / 2)` is a good first approximation of `sqrt(a)` with at least 1 correct bit.
        uint256 result = 1 << (log2(a) >> 1);

        // At this point `result` is an estimation with one bit of precision. We know the true value is a uint128,
        // since it is the square root of a uint256. Newton's method converges quadratically (precision doubles at
        // every iteration). We thus need at most 7 iteration to turn our partial result with one bit of precision
        // into the expected uint128 result.
        unchecked {
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            return min(result, a / result);
        }
    }

    /**
     * @notice Calculates sqrt(a), following the selected rounding direction.
     */
    function sqrt(uint256 a, Rounding rounding) internal pure returns (uint256) {
        unchecked {
            uint256 result = sqrt(a);
            return result + (rounding == Rounding.Up && result * result < a ? 1 : 0);
        }
    }

    /**
     * @dev Return the log in base 2, rounded down, of a positive value.
     * Returns 0 if given 0.
     */
    function log2(uint256 value) internal pure returns (uint256) {
        uint256 result = 0;
        unchecked {
            if (value >> 128 > 0) {
                value >>= 128;
                result += 128;
            }
            if (value >> 64 > 0) {
                value >>= 64;
                result += 64;
            }
            if (value >> 32 > 0) {
                value >>= 32;
                result += 32;
            }
            if (value >> 16 > 0) {
                value >>= 16;
                result += 16;
            }
            if (value >> 8 > 0) {
                value >>= 8;
                result += 8;
            }
            if (value >> 4 > 0) {
                value >>= 4;
                result += 4;
            }
            if (value >> 2 > 0) {
                value >>= 2;
                result += 2;
            }
            if (value >> 1 > 0) {
                result += 1;
            }
        }
        return result;
    }

    /**
     * @dev Return the log in base 2, following the selected rounding direction, of a positive value.
     * Returns 0 if given 0.
     */
    function log2(uint256 value, Rounding rounding) internal pure returns (uint256) {
        unchecked {
            uint256 result = log2(value);
            return result + (rounding == Rounding.Up && 1 << result < value ? 1 : 0);
        }
    }

    /**
     * @dev Return the log in base 10, rounded down, of a positive value.
     * Returns 0 if given 0.
     */
    function log10(uint256 value) internal pure returns (uint256) {
        uint256 result = 0;
        unchecked {
            if (value >= 10**64) {
                value /= 10**64;
                result += 64;
            }
            if (value >= 10**32) {
                value /= 10**32;
                result += 32;
            }
            if (value >= 10**16) {
                value /= 10**16;
                result += 16;
            }
            if (value >= 10**8) {
                value /= 10**8;
                result += 8;
            }
            if (value >= 10**4) {
                value /= 10**4;
                result += 4;
            }
            if (value >= 10**2) {
                value /= 10**2;
                result += 2;
            }
            if (value >= 10**1) {
                result += 1;
            }
        }
        return result;
    }

    /**
     * @dev Return the log in base 10, following the selected rounding direction, of a positive value.
     * Returns 0 if given 0.
     */
    function log10(uint256 value, Rounding rounding) internal pure returns (uint256) {
        unchecked {
            uint256 result = log10(value);
            return result + (rounding == Rounding.Up && 10**result < value ? 1 : 0);
        }
    }

    /**
     * @dev Return the log in base 256, rounded down, of a positive value.
     * Returns 0 if given 0.
     *
     * Adding one to the result gives the number of pairs of hex symbols needed to represent `value` as a hex string.
     */
    function log256(uint256 value) internal pure returns (uint256) {
        uint256 result = 0;
        unchecked {
            if (value >> 128 > 0) {
                value >>= 128;
                result += 16;
            }
            if (value >> 64 > 0) {
                value >>= 64;
                result += 8;
            }
            if (value >> 32 > 0) {
                value >>= 32;
                result += 4;
            }
            if (value >> 16 > 0) {
                value >>= 16;
                result += 2;
            }
            if (value >> 8 > 0) {
                result += 1;
            }
        }
        return result;
    }

    /**
     * @dev Return the log in base 10, following the selected rounding direction, of a positive value.
     * Returns 0 if given 0.
     */
    function log256(uint256 value, Rounding rounding) internal pure returns (uint256) {
        unchecked {
            uint256 result = log256(value);
            return result + (rounding == Rounding.Up && 1 << (result * 8) < value ? 1 : 0);
        }
    }
}


// File @openzeppelin/contracts/utils/Strings.sol@v4.8.2

// 
// OpenZeppelin Contracts (last updated v4.8.0) (utils/Strings.sol)

pragma solidity ^0.8.0;

/**
 * @dev String operations.
 */
library Strings {
    bytes16 private constant _SYMBOLS = "0123456789abcdef";
    uint8 private constant _ADDRESS_LENGTH = 20;

    /**
     * @dev Converts a `uint256` to its ASCII `string` decimal representation.
     */
    function toString(uint256 value) internal pure returns (string memory) {
        unchecked {
            uint256 length = Math.log10(value) + 1;
            string memory buffer = new string(length);
            uint256 ptr;
            /// @solidity memory-safe-assembly
            assembly {
                ptr := add(buffer, add(32, length))
            }
            while (true) {
                ptr--;
                /// @solidity memory-safe-assembly
                assembly {
                    mstore8(ptr, byte(mod(value, 10), _SYMBOLS))
                }
                value /= 10;
                if (value == 0) break;
            }
            return buffer;
        }
    }

    /**
     * @dev Converts a `uint256` to its ASCII `string` hexadecimal representation.
     */
    function toHexString(uint256 value) internal pure returns (string memory) {
        unchecked {
            return toHexString(value, Math.log256(value) + 1);
        }
    }

    /**
     * @dev Converts a `uint256` to its ASCII `string` hexadecimal representation with fixed length.
     */
    function toHexString(uint256 value, uint256 length) internal pure returns (string memory) {
        bytes memory buffer = new bytes(2 * length + 2);
        buffer[0] = "0";
        buffer[1] = "x";
        for (uint256 i = 2 * length + 1; i > 1; --i) {
            buffer[i] = _SYMBOLS[value & 0xf];
            value >>= 4;
        }
        require(value == 0, "Strings: hex length insufficient");
        return string(buffer);
    }

    /**
     * @dev Converts an `address` with fixed length of 20 bytes to its not checksummed ASCII `string` hexadecimal representation.
     */
    function toHexString(address addr) internal pure returns (string memory) {
        return toHexString(uint256(uint160(addr)), _ADDRESS_LENGTH);
    }
}


// File @openzeppelin/contracts/token/ERC20/IERC20.sol@v4.8.2

// 
// OpenZeppelin Contracts (last updated v4.6.0) (token/ERC20/IERC20.sol)

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
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}


// File contracts/LeagueOfThronesV2.sol

// 
pragma solidity ^0.8.0;




enum SeasonStatus { Invalid, WaitForNTF ,Pending, End }

enum MappingStatus { Invalid, Valid }


struct ExtraGeneralIds {
    uint256[] generalIds;
    MappingStatus status;
}

//union Record
struct UnionRecord{
    address[] playerAddresses;
    uint256 unionId;
    mapping(address => ExtraGeneralIds) playerExtraGeneralIds;
    MappingStatus status;
}


//season record
struct SeasonRecord{
    mapping(uint256 => UnionRecord) unionRecords;
    mapping(address => uint256) unionIdMapping;
    mapping(address => uint256) unionRewardRecord;
    mapping(address => uint256) gloryRewardRecord;
    mapping(address => uint256) rechargeRecord;
    MappingStatus rechargeStatus;
    address rechargeAddress;
    uint256 sumPlayers;
    address ntf1ContractAddress;
    address ntf2ContractAddress;
    address rewardAddress;
    uint256 playerLimit;
    uint256 reward1Amount;
    uint256 reward2Amount;
    uint256 sumRecharge;
    uint256[] rankConfigFromTo;
    uint256[] rankConfigValue;
    //reservation open ready end
    uint256[] seasonTimeConfig;
    SeasonStatus seasonStatus;
    uint256 maxUnionDiffNum;

}

struct SeaSonInfoResult{
    uint256 unionId;
    uint256[] generalIds;
}

struct SeasonStatusResult{
    uint256 sumPlayerNum;
    uint256[] unionsPlayerNum;
    uint256 maxUnionDiffNum;
}

contract LeagueOfThronesV2 is Ownable{

    event signUpInfo( string seasonId, address player, uint256 unionId, uint256[] extraGeneralIds,uint256 []originNFTIds,uint256 originUnionId);
    event startSeasonInfo( string seasonId, uint256 playerLimit, address rewardAddress, uint256 rewardAmount1, uint256 rewardAmount2, uint256[] rankConfigFromTo, uint256[] rankConfigValue, uint256[] seasonTimeConfig);
    event endSeasonInfo( string seasonId, uint256 unionId, address[] playerAddresses, uint256[] glorys, uint256 unionSumGlory);
    event sendRankRewardInfo( string seasonId, address player, uint256 rank, uint256 amount);
    event sendUnionRewardInfo( string seasonId, address player, uint256 glory, uint256 amount);
    event rechargeInfo( string seasonId, address player, uint256 rechargeId,uint256 amount, uint256 totalAmount);
    mapping( string => SeasonRecord) public seasonRecords;
    string public nowSeasonId;

    constructor() public onlyOwner{
        nowSeasonId = "";
    }

    //start season and transfer reward to contract
    function startSeason(
        string memory seasonId,
        uint256 playerLimit,
        address rewardAddress,
        uint256 rewardAmount1, 
        uint256 rewardAmount2, 
        uint256[] memory rankConfigFromTo,
        uint256[] memory rankConfigValue,
        uint256[] memory seasonTimeConfig,
        uint256 maxUnionDiffNum
        ) external onlyOwner payable {
        SeasonRecord storage sRecord = seasonRecords[seasonId];
        require(sRecord.seasonStatus == SeasonStatus.Invalid, "Season can not start repeat");
        require(seasonTimeConfig.length == 4, "time config length error" );
        require(maxUnionDiffNum > 0, "maxUnionDiffNum must above zero" );

        uint256 rewordAmount = rewardAmount1 + rewardAmount2;
        //BSC version modify ,no need to pre transfer
        if (false) {
            if(rewardAddress == address(0x0)){
                require( msg.value == rewordAmount, "Check the ETH amount" );
            }
            else{
                IERC20 token = IERC20(rewardAddress);
                uint256 allowance = token.allowance(msg.sender, address(this));
                require(allowance >= rewordAmount, "Check the token allowance");
                token.transferFrom(msg.sender, address(this), rewordAmount);
            }
        }


        sRecord.seasonStatus = SeasonStatus.WaitForNTF;
        sRecord.rewardAddress = rewardAddress;
        sRecord.reward1Amount = rewardAmount1;
        sRecord.reward2Amount = rewardAmount2;
        sRecord.playerLimit = playerLimit;
        sRecord.maxUnionDiffNum = maxUnionDiffNum;
        require(rankConfigFromTo.length == rankConfigValue.length * 2, "rewardConfig length error");
        uint256 sumReward = 0;
        bool indexRight = true;
        uint256 lastEnd = 0;
        for(uint256 i = 0; i < rankConfigValue.length; i++){
            if(rankConfigFromTo[i * 2] != lastEnd + 1){
                indexRight = false;
                break;
            }
            lastEnd = rankConfigFromTo[i * 2 + 1];
            sumReward += ((rankConfigFromTo[i * 2 + 1] - rankConfigFromTo[i * 2 ] + 1) * rankConfigValue[i]);
        }
        require(indexRight && sumReward == rewardAmount2, "reward config error");
        sRecord.rankConfigFromTo = rankConfigFromTo;
        sRecord.rankConfigValue = rankConfigValue;
        sRecord.seasonTimeConfig = seasonTimeConfig;
        emit startSeasonInfo(seasonId, playerLimit, rewardAddress, rewardAmount1, rewardAmount2, rankConfigFromTo, rankConfigValue, seasonTimeConfig);
    }

    //set nft address of season
    function setNFTAddress(string memory seasonId, address ntf1Address, address ntf2Address) external onlyOwner {
        SeasonRecord storage sRecord = seasonRecords[seasonId];
        require(sRecord.seasonStatus == SeasonStatus.WaitForNTF, "Season Haven't begin or NTF have set");
        sRecord.seasonStatus = SeasonStatus.Pending;
        sRecord.ntf1ContractAddress = ntf1Address;
        sRecord.ntf2ContractAddress = ntf2Address;
        sRecord.seasonStatus = SeasonStatus.Pending;
    }

    function getNFTAddresses(string memory seasonId ) public view returns (address[] memory){
        SeasonRecord storage sRecord = seasonRecords[seasonId];
        address[] memory addresses = new address[](2);
        addresses[0] = sRecord.ntf1ContractAddress;
        addresses[1] = sRecord.ntf2ContractAddress;
        return addresses;
    }

    function random(uint number) public view returns(uint) {
        return uint(keccak256(abi.encodePacked(block.timestamp,block.difficulty,  
            msg.sender))) % number;
    }


    // params: 
    //  unionId:  0 for random
    function signUpGame(string memory seasonId,uint256 unionId, uint256 ntf1TokenId, uint256 ntf2TokenId) external{
        SeasonRecord storage sRecord = seasonRecords[seasonId];
        require(sRecord.seasonStatus == SeasonStatus.Pending, "Season Status Error");
        require( block.timestamp >= sRecord.seasonTimeConfig[0] && block.timestamp <= sRecord.seasonTimeConfig[3], "It is not signUp time now");
        require( sRecord.sumPlayers < sRecord.playerLimit, "the number of players has reached the limit");
        require( unionId >= 0 && unionId <= 4, "unionId error");


        // Record original unionId to detect whether the player select random unionId
        uint256 orginUnionId = unionId;
        uint256[] memory originNFTIds = new uint256[](2);
        originNFTIds[0] = ntf1TokenId;
        originNFTIds[1] = ntf2TokenId;




        // find wheather player has signUp and get union's player number
        uint256[] memory unionPlayerNum = new uint256[](5);
        uint256 minumUnionPlayerNum = sRecord.playerLimit;

        bool hasSignUp = false;
        for( uint i = 1 ; i <= 4 ; i ++ ){
            UnionRecord storage unionRecord = sRecord.unionRecords[i];
            if(unionRecord.status != MappingStatus.Invalid ){
                unionPlayerNum[i] = unionRecord.playerAddresses.length;
                ExtraGeneralIds storage extraIds =  unionRecord.playerExtraGeneralIds[msg.sender];
                if(extraIds.status == MappingStatus.Valid){
                    hasSignUp = true;
                    break;
                }
            }

            if(unionPlayerNum[i] < minumUnionPlayerNum){
                minumUnionPlayerNum = unionPlayerNum[i];
            }
        }
        require(hasSignUp == false , "player has signUp");
        // random unionId
        if (unionId!=0){
            // unionId maxUnionDiffNum check
            require(unionPlayerNum[unionId] - minumUnionPlayerNum < sRecord.maxUnionDiffNum, "unionId maxUnionDiffNum check error");
        }else{
            // random unionId of which player number is not above maxUnionDiffNum + currentUnionPlayerNum
            uint256[] memory unionIdsList = new uint256[](4);
            uint256 unionIdsListLen = 0;
            for( uint i = 1 ; i <= 4 ; i ++ ){
                if(unionPlayerNum[i] - minumUnionPlayerNum < sRecord.maxUnionDiffNum){
                    // add to unionIdsList
                    unionIdsList[unionIdsListLen] = i;
                    unionIdsListLen ++ ;
                }
            }

            // random unionId by block.timestamp
            unionId = unionIdsList[uint(keccak256(abi.encodePacked(block.timestamp,block.difficulty, msg.sender))) % unionIdsListLen];
        }

        // update season record
        sRecord.sumPlayers ++ ;
        sRecord.unionIdMapping[msg.sender] = unionId;
        UnionRecord storage unionRecord = sRecord.unionRecords[unionId];
        if(unionRecord.status == MappingStatus.Invalid){
            //gen union record
            unionRecord.status = MappingStatus.Valid;
            unionRecord.playerAddresses = new address[](0);
        }
        unionRecord.playerAddresses.push(msg.sender);

        //Random extra general
        IERC721 ntf1Contract = IERC721(sRecord.ntf1ContractAddress);
        IERC721 ntf2Contract = IERC721(sRecord.ntf2ContractAddress);
        ExtraGeneralIds storage extraIds = unionRecord.playerExtraGeneralIds[msg.sender];
        extraIds.generalIds = new uint256[](0);
        extraIds.status = MappingStatus.Valid;
        try ntf1Contract.ownerOf(ntf1TokenId) returns(address owner){
            if(owner == msg.sender){
                extraIds.generalIds.push(random(4) + 7);
            }
        }
        catch{

        }
        try ntf2Contract.ownerOf(ntf2TokenId) returns(address owner){
            if(owner == msg.sender){
                extraIds.generalIds.push(random(4) + 11);
            }
        }
        catch{
            
        }
        emit signUpInfo(seasonId , msg.sender, unionId, extraIds.generalIds, originNFTIds, orginUnionId);
    }

    function setRechargeToken(string memory seasonId, address tokenAddress) public onlyOwner {
        SeasonRecord storage sRecord = seasonRecords[seasonId];
        require(sRecord.seasonStatus == SeasonStatus.Pending, "Season Status Error");
        require(sRecord.rechargeStatus == MappingStatus.Invalid, "recharge token have set");
        sRecord.rechargeStatus = MappingStatus.Valid;
        sRecord.rechargeAddress = tokenAddress;
    }

    function getRechargeToken(string memory seasonId) public view returns( address ) {
        SeasonRecord storage sRecord = seasonRecords[seasonId];
        require(sRecord.seasonStatus == SeasonStatus.Pending, "Season Status Error");
        require(sRecord.rechargeStatus == MappingStatus.Valid, "recharge token have not set");
        return sRecord.rechargeAddress;
    }

    function recharge(string memory seasonId, uint256 rechargeId ,uint256 amount) public payable {
        SeasonRecord storage sRecord = seasonRecords[seasonId];
        require(sRecord.seasonStatus == SeasonStatus.Pending, "Season Status Error");
        require(sRecord.rechargeStatus == MappingStatus.Valid, "recharge token have not set");
        // bool hasSignUp = false;
        // for( uint i = 1 ; i <= 4 ; i ++ ){
        //     UnionRecord storage unionRecord = sRecord.unionRecords[i];
        //     if(unionRecord.status == MappingStatus.Invalid ){
        //         continue;
        //     }
        //     else{
        //         ExtraGeneralIds storage extraIds =  unionRecord.playerExtraGeneralIds[msg.sender];
        //         if(extraIds.status == MappingStatus.Valid){
        //             hasSignUp = true;
        //             break;
        //         }
        //     }
        // }
        // require(hasSignUp == true , "player have not signUp");
        if(sRecord.rechargeAddress == address(0x0)){
            sRecord.rechargeRecord[msg.sender] += msg.value;
            sRecord.sumRecharge += msg.value;
            emit rechargeInfo(seasonId, msg.sender, rechargeId, msg.value, sRecord.rechargeRecord[msg.sender]);
        }
        else{
            IERC20 token = IERC20(sRecord.rechargeAddress);
            uint256 allowance = token.allowance(msg.sender, address(this));
            require(allowance >= amount, "Check the token allowance");
            token.transferFrom(msg.sender, address(this), amount);
            sRecord.rechargeRecord[msg.sender] += amount;
            sRecord.sumRecharge += amount;
            emit rechargeInfo(seasonId, msg.sender, rechargeId, amount, sRecord.rechargeRecord[msg.sender]);
        }
    }

    function getRechargeInfo( string memory seasonId, address player) public view returns (uint256, uint256){
        SeasonRecord storage sRecord = seasonRecords[seasonId];
        require(sRecord.seasonStatus != SeasonStatus.Invalid, "Season is not exist");
        return (sRecord.rechargeRecord[player], sRecord.sumRecharge);
    }


    function getSeasonStatus( string memory seasonId ) public view returns ( SeasonStatusResult memory ){
        SeasonRecord storage sRecord = seasonRecords[seasonId];
        require(sRecord.seasonStatus == SeasonStatus.Pending, "Season Status Error");
        SeasonStatusResult memory re = SeasonStatusResult( sRecord.sumPlayers , new uint256[](4),sRecord.maxUnionDiffNum);
        for( uint i = 1 ; i <= 4 ; i ++ ){
            UnionRecord storage unionRecord = sRecord.unionRecords[i];
            if(unionRecord.status == MappingStatus.Invalid ){
                re.unionsPlayerNum[i-1] = 0;
            }
            else{
                re.unionsPlayerNum[i-1] = unionRecord.playerAddresses.length;
            }
        }
        return re;
    } 

    function getSignUpInfo( string memory seasonId, address playerAddress) public view returns ( SeaSonInfoResult memory){
        SeasonRecord storage sRecord = seasonRecords[seasonId];
        require(sRecord.seasonStatus != SeasonStatus.Invalid, "Season Status Error");
        SeaSonInfoResult memory re = SeaSonInfoResult(0, new uint256[](0));
        for( uint i = 1 ; i <= 4 ; i ++ ){
            UnionRecord storage unionRecord = sRecord.unionRecords[i];
            if(unionRecord.status == MappingStatus.Invalid ){
                continue;
            }
            else{
                ExtraGeneralIds storage extraIds = unionRecord.playerExtraGeneralIds[playerAddress];
                if(extraIds.status == MappingStatus.Invalid){
                    continue;
                }
                re.unionId = i;
                re.generalIds = extraIds.generalIds;
            }
        }
        return re;
    }

    function endSeason(  string memory seasonId, uint256 unionId, address[] memory playerAddresses, uint256[] memory glorys, uint256 unionSumGlory) external onlyOwner {
        SeasonRecord storage sRecord = seasonRecords[seasonId];
        require(sRecord.seasonStatus == SeasonStatus.Pending,  "Season Status Error");
        require(playerAddresses.length == glorys.length, "input array length do not equal");
        uint fromToIndex = 0;
        uint rankMax = sRecord.rankConfigFromTo[sRecord.rankConfigFromTo.length - 1];
        //IERC20 token = IERC20(sRecord.rewardAddress);
        for(uint i = 0; i < playerAddresses.length; i++ ){  
            address playerAddress = playerAddresses[i];
            uint256 glory = glorys[i];
            if(sRecord.unionIdMapping[playerAddress] == unionId){
               uint256 amount = glory * sRecord.reward1Amount / unionSumGlory;
               sRecord.unionRewardRecord[playerAddress] = amount; 
               transferReward(sRecord.rewardAddress, playerAddress, amount);
               //if( token.transfer(playerAddress, amount)){
               emit sendUnionRewardInfo(seasonId, playerAddress, glory, amount);
               //}
            }
            if(i < rankMax){
               uint256 to = sRecord.rankConfigFromTo[fromToIndex * 2 + 1];
               if( i + 1 > to ){
                  fromToIndex += 1;
               }
               uint256 amount = sRecord.rankConfigValue[fromToIndex];
               sRecord.gloryRewardRecord[playerAddress] = amount;
               transferReward(sRecord.rewardAddress, playerAddress, amount);
               //if( token.transfer(playerAddress, amount)){
               emit sendRankRewardInfo(seasonId, playerAddress, i + 1, amount);
               //}
            }
        }
        emit endSeasonInfo( seasonId,  unionId,  playerAddresses, glorys, unionSumGlory);
    }

    function withdraw( address tokenAddress, uint256 amount) external  onlyOwner{
        if(tokenAddress == address(0x0)){
            require(address(this).balance >=  amount, "balance is not enough");
            payable(msg.sender).transfer(amount);
        }
        else{
            IERC20 token = IERC20(tokenAddress);
            uint256 balance = token.balanceOf(address(this));
            require(balance >= amount, "balance is not enough");
            token.transfer(msg.sender, amount);
        }
    }

    function transferReward(address rewardAddress, address toAddress, uint256 amount) internal {
        if(rewardAddress == address(0x0)){
            require(address(this).balance >=  amount, "balance is not enough");
            payable(toAddress).transfer(amount);
        }
        else{
            IERC20 token = IERC20(rewardAddress);
            uint256 balance = token.balanceOf(address(this));
            require(balance >= amount, "balance is not enough");
            token.transfer(toAddress, amount);
        }
    }
}