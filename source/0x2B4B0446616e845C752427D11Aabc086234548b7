// SPDX-License-Identifier: MIT

// The Mining Race NFT Smart Contract
// https://miningrace.com

// File @openzeppelin/contracts/utils/Context.sol@v4.9.2

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


// File @openzeppelin/contracts/access/Ownable.sol@v4.9.2

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


// File @openzeppelin/contracts/utils/introspection/IERC165.sol@v4.9.2

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


// File @openzeppelin/contracts/token/ERC721/IERC721.sol@v4.9.2

// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC721/IERC721.sol)

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
    function safeTransferFrom(address from, address to, uint256 tokenId, bytes calldata data) external;

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
    function safeTransferFrom(address from, address to, uint256 tokenId) external;

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
    function transferFrom(address from, address to, uint256 tokenId) external;

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
    function setApprovalForAll(address operator, bool approved) external;

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


// File @openzeppelin/contracts/token/ERC721/extensions/IERC721Metadata.sol@v4.9.2

// OpenZeppelin Contracts v4.4.1 (token/ERC721/extensions/IERC721Metadata.sol)

pragma solidity ^0.8.0;

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


// File @openzeppelin/contracts/utils/math/Math.sol@v4.9.2

// OpenZeppelin Contracts (last updated v4.9.0) (utils/math/Math.sol)

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
    function mulDiv(uint256 x, uint256 y, uint256 denominator) internal pure returns (uint256 result) {
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
                // Solidity will revert if denominator == 0, unlike the div opcode on its own.
                // The surrounding unchecked block does not change this fact.
                // See https://docs.soliditylang.org/en/latest/control-structures.html#checked-or-unchecked-arithmetic.
                return prod0 / denominator;
            }

            // Make sure the result is less than 2^256. Also prevents denominator == 0.
            require(denominator > prod1, "Math: mulDiv overflow");

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
    function mulDiv(uint256 x, uint256 y, uint256 denominator, Rounding rounding) internal pure returns (uint256) {
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
            if (value >= 10 ** 64) {
                value /= 10 ** 64;
                result += 64;
            }
            if (value >= 10 ** 32) {
                value /= 10 ** 32;
                result += 32;
            }
            if (value >= 10 ** 16) {
                value /= 10 ** 16;
                result += 16;
            }
            if (value >= 10 ** 8) {
                value /= 10 ** 8;
                result += 8;
            }
            if (value >= 10 ** 4) {
                value /= 10 ** 4;
                result += 4;
            }
            if (value >= 10 ** 2) {
                value /= 10 ** 2;
                result += 2;
            }
            if (value >= 10 ** 1) {
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
            return result + (rounding == Rounding.Up && 10 ** result < value ? 1 : 0);
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
     * @dev Return the log in base 256, following the selected rounding direction, of a positive value.
     * Returns 0 if given 0.
     */
    function log256(uint256 value, Rounding rounding) internal pure returns (uint256) {
        unchecked {
            uint256 result = log256(value);
            return result + (rounding == Rounding.Up && 1 << (result << 3) < value ? 1 : 0);
        }
    }
}


// File @openzeppelin/contracts/utils/math/SignedMath.sol@v4.9.2

// OpenZeppelin Contracts (last updated v4.8.0) (utils/math/SignedMath.sol)

pragma solidity ^0.8.0;

/**
 * @dev Standard signed math utilities missing in the Solidity language.
 */
library SignedMath {
    /**
     * @dev Returns the largest of two signed numbers.
     */
    function max(int256 a, int256 b) internal pure returns (int256) {
        return a > b ? a : b;
    }

    /**
     * @dev Returns the smallest of two signed numbers.
     */
    function min(int256 a, int256 b) internal pure returns (int256) {
        return a < b ? a : b;
    }

    /**
     * @dev Returns the average of two signed numbers without overflow.
     * The result is rounded towards zero.
     */
    function average(int256 a, int256 b) internal pure returns (int256) {
        // Formula from the book "Hacker's Delight"
        int256 x = (a & b) + ((a ^ b) >> 1);
        return x + (int256(uint256(x) >> 255) & (a ^ b));
    }

    /**
     * @dev Returns the absolute unsigned value of a signed value.
     */
    function abs(int256 n) internal pure returns (uint256) {
        unchecked {
            // must be unchecked in order to support `n = type(int256).min`
            return uint256(n >= 0 ? n : -n);
        }
    }
}


// File @openzeppelin/contracts/utils/Strings.sol@v4.9.2

// OpenZeppelin Contracts (last updated v4.9.0) (utils/Strings.sol)

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
     * @dev Converts a `int256` to its ASCII `string` decimal representation.
     */
    function toString(int256 value) internal pure returns (string memory) {
        return string(abi.encodePacked(value < 0 ? "-" : "", toString(SignedMath.abs(value))));
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

    /**
     * @dev Returns true if the two strings are equal.
     */
    function equal(string memory a, string memory b) internal pure returns (bool) {
        return keccak256(bytes(a)) == keccak256(bytes(b));
    }
}


// File @openzeppelin/contracts/token/ERC20/IERC20.sol@v4.9.2

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


// File contracts/extension/PaymentMethods.sol

pragma solidity ^0.8.0;


abstract contract PaymentMethods is Ownable {

    struct PricingOption {
        address paymentMethod;
        address paymentTo;
        uint256 price;
        uint256 extendDuration;
        uint256 paymentMethodId;
        bool    canBeUsedForPenalty;
    }

    //Key is paymentMethodId
    mapping(uint256 => PricingOption) private _paymentMethods;
    //Key is a custom ID => paymentMethodId => Option
    mapping(uint256 => mapping(uint256 => PricingOption)) private _specialLevelPaymentMethods;
    // Pricing IDs for levels and sequences are NOT in _paymentMethods
    mapping(uint256 => bool) private _levelsWithSpecialPayments;

    event PaymentMethodAdded(address indexed paymentMethodAddress, uint256 price, uint256 paymentMethodId, uint256 extendDuration);

    /**
     * @dev Allows owner to add/replace a payment method option for extending the NFT ownership
     *  while offering discounts on different durations
     *  owner can deactivate the payment method by setting price to 0
     */
    function setPaymentMethod(
        uint256 paymentMethodId,
        address paymentMethodAddress,
        uint256 nftPrice,
        uint256 extendDuration,
        address paymentReceiver,
        bool canBeUsedForPenalty
    ) public onlyOwner {
        PricingOption storage option = _paymentMethods[paymentMethodId];
        option.price = nftPrice;
        option.extendDuration = extendDuration;
        option.paymentTo = paymentReceiver;
        option.paymentMethod = paymentMethodAddress;
        option.paymentMethodId = paymentMethodId;
        option.canBeUsedForPenalty = canBeUsedForPenalty;

        emit PaymentMethodAdded(paymentMethodAddress, nftPrice, paymentMethodId, extendDuration);
    }

    /**
     * @dev Allows owner to add/replace a payment method option for special pricing for specific levels
     *  owner can deactivate the payment method by setting price to 0
     */
    function setPaymentMethodLevel(
        uint256 level,
        uint256 paymentMethodId,
        address paymentMethodAddress,
        uint256 nftPrice,
        uint256 extendDuration,
        address paymentReceiver,
        bool    canBeUsedForPenalty
    ) public onlyOwner {
        PricingOption storage option = _specialLevelPaymentMethods[level][paymentMethodId];
        option.price = nftPrice;
        option.extendDuration = extendDuration;
        option.paymentTo = paymentReceiver;
        option.paymentMethod = paymentMethodAddress;
        option.paymentMethodId = paymentMethodId;
        option.canBeUsedForPenalty = canBeUsedForPenalty;

        _levelsWithSpecialPayments[level] = true;
        emit PaymentMethodAdded(paymentMethodAddress, nftPrice, paymentMethodId, extendDuration);
    }

    /**
     * @dev Allows owner to disable mandatory requirement for level payment methods after adding them
     */
    function disableLevelPaymentMethods(uint256 level) public onlyOwner {
        _levelsWithSpecialPayments[level] = false;
    }

    function paymentMethod(uint256 paymentMethodId, uint256 levelId) public view returns (PricingOption memory, IERC20) {
        PricingOption memory paymentOption;
        if (_levelsWithSpecialPayments[levelId]) {
            paymentOption = _specialLevelPaymentMethods[levelId][paymentMethodId];
            require(paymentOption.price > 0 && paymentOption.paymentMethod != address(0), "Level Payment method is invalid");
        } else {
            paymentOption = _paymentMethods[paymentMethodId];
            require(paymentOption.price > 0 && paymentOption.paymentMethod != address(0), "Payment method is invalid");
        }
        IERC20 _paymentMethod = IERC20(paymentOption.paymentMethod);
        return (paymentOption, _paymentMethod);
    }

    /**
     * @dev used to get a payment method by ID
     *  Function checks if owner added a custom payment method for a specific tokenId, if not whether it implemented custom
     *  function to get depth(level) of tokenId and has a special payment for it
     */
    function _paymentMethodVerify(uint256 paymentMethodId, uint256 levelId) internal view returns (PricingOption memory, IERC20) {
        (PricingOption memory paymentOption, IERC20 _paymentMethod) = paymentMethod(paymentMethodId, levelId);
        require(_paymentMethod.balanceOf(_msgSender()) >= paymentOption.price, "Insufficient funds");
        require(_paymentMethod.allowance(_msgSender(), address(this)) >= paymentOption.price, "Currency activation/allowance required");
        return (paymentOption, _paymentMethod);
    }
}


// File @openzeppelin/contracts/security/ReentrancyGuard.sol@v4.9.2

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


// File @openzeppelin/contracts/utils/math/SafeMath.sol@v4.9.2

// OpenZeppelin Contracts (last updated v4.9.0) (utils/math/SafeMath.sol)

pragma solidity ^0.8.0;

// CAUTION
// This version of SafeMath should only be used with Solidity 0.8 or later,
// because it relies on the compiler's built in overflow checks.

/**
 * @dev Wrappers over Solidity's arithmetic operations.
 *
 * NOTE: `SafeMath` is generally not needed starting with Solidity 0.8, since the compiler
 * now has built in overflow checking.
 */
library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
            // benefit is lost if 'b' is also tested.
            // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the division of two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
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
        return a + b;
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
        return a * b;
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator.
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
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
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting with custom message on
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
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
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
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}


// File contracts/mrace/IMRNFT.sol

pragma solidity ^0.8.0;

interface IMRNFT {

    struct Spot {
        address owner;
        string parent;
        bytes32 payoutTo;
        uint256 sequence;
        uint256 level;
        uint256 childCount;
        uint256 expiresAt;
    }

    /**
     * @dev Emitted when `tokenId` token minted (reserved) by someone
     */
    event Mint(address indexed to, string indexed spotId, uint256 expiresAt);
    event Extended(string indexed tokenId, uint256 expiresAt);
    event PayoutChanged(string indexed spotId, bytes32 payoutHash);
    event Penalty(string indexed spotId, bool hasPenalty);

    function getSpot(string calldata spotId) external returns (Spot memory);

    /**
     * @dev Mints an NFT for a wallet
     */
    function mint(
        address owner,
        string calldata parentSpotId,
        string calldata newSpotId,
        uint256 _expiresAt
    )  external;

    /**
     * @dev Mints an NFT for a wallet and sets the payout hash
     */
    function mintWithPayoutHash(
        address owner,
        string calldata parentSpotId,
        string calldata newSpotId,
        uint256 _expiresAt,
        bytes32 _payoutHash
    ) external;

    /**
     * @dev Allows any user to buy a spot as long as they know the paymentMethodId and have an inviteCode
     *  restricted to one call per owner, and assumes owner's invite code will be randomly picked externally
     *  function restricts children of any spot to MAX_SPOTS_PER_CODE,
     *  this reverts if that is reached for the inviteCode provided
     * @param paymentMethodId ID of the payment option
     * @param invitedBySpotId ID of the spot that invited the user
     * @param newSpotId A randomized ID for the spot being bought, to have random function outside contract
     */
    function buy(
        uint256 paymentMethodId,
        string calldata invitedBySpotId,
        string calldata newSpotId
    ) external;

    function buyAndSetPayoutHash(
        uint256 paymentMethodId,
        string calldata invitedBySpotId,
        string calldata newSpotId,
        bytes32 _payoutHash
    ) external;

    /**
     * @dev Allows any wallet to extend a spot by pay its price, for as much as they want
     * @param paymentMethodId ID of the payment method
     * @param spotId ID of the spot being extended (even if not owned by sender)
     */
    function extend(uint256 paymentMethodId, string calldata spotId) external;

    /**
     * @dev Allows admin to extend specific spots when they pay in fiat
     * @param spotId ID of the spot being extended
     * @param newExpiresAt uint256 timestamp of the new value for expires at
     */
    function adminExtend(string calldata spotId, uint256 newExpiresAt) external;

    /**
     * @dev Allows owner of an NFT to set/change the payout hash of his spot
     */
    function setPayoutHash(string calldata spotId, bytes32 _payoutHash) external;

    /**
     * @dev Allows a user to calculate the value of a hash, so they can pass it to setPayoutHash function
     */
    function calculateHash(string calldata spotId, string calldata rewardAddress) external view returns (bytes32);

    /**
     * @dev Allows the owner to toggle on and off the never expires flag for a spot
     */
    function setNeverExpires(string calldata spotId, bool newValue) external;

    /**
     * @dev Returns the expiry time of a spot
     * @param spotId string The ID of the spot
     * @return timestamp in seconds
     */
    function expiresAt(string calldata spotId) external view returns(uint256);

    /**
     * @dev Allows admin to set a penalty on a spot, normally occurs when an off chain issue
     *  triggers this
     */
    function setPenalty(string calldata spotId, bool isPenalized) external;

    /**
     * @dev Checks if a spot has incurred a penalty either administratively or by not renewing
     */
    function hasPenalty(string calldata spotId) external view returns(bool);

    function payoutHash(string calldata spotId) external view returns (bytes32);

    /**
     * @dev Given a spotId and a reward address, checks if the reward address is valid by comparing it
     *  against the stored payoutHash (keccak256(spotId,rewardAddress))
     */
    function isPayoutHashValid(string calldata spotId, string calldata rewardAddress) external view returns(bool);

    function spotOf(address owner) external view returns (string memory);

    function ownerOfSpot(string calldata spotId) external view returns (address);
}


// File contracts/mrace/MRNFT.sol

pragma solidity ^0.8.0;








/**
 * @dev A custom NFT for The Mining Race, where NFTs are by invitation (parent NFTs)
 *  and each NFT is non transferable
 */
contract MRaceNFT is ReentrancyGuard, PaymentMethods, IMRNFT, IERC721, IERC721Metadata {
    using SafeMath for uint256;
    using Strings for uint256;
    using Strings for string;

    string private _name;
    string private _symbol;
    string public baseURI;

    uint256 public constant NEVER_EXPIRES = type(uint256).max;
    uint256 public constant MAX_SPOTS_PER_CODE = 10;
    uint256 public constant PENALTY_AFTER_EXPIRY = 90 * 86400;

    uint256 public _totalSupply = 1; //starts with root node
    uint256 public _cap;

    mapping(uint256 => Spot) private _sequences;
    mapping(address => string) private _spotOwnerships;
    mapping(string => Spot) private _communitySpots;
    mapping(string => Spot) private _spots;
    mapping(string => bool) private _neverExpireSpots;
    mapping(string => bool) private _penalizedSpots;

    modifier whenNotMaxed() {
        require(_cap > _totalSupply, "Token maxed out");
        _;
    }

    modifier whenSpotIdIsValid(string memory spotId) {
        require(bytes(spotId).length > 0, "Spot ID is invalid");
        _;
    }

    constructor(
        string memory tokenName,
        string memory tokenSymbol,
        uint256 maxSupply,
        string memory rootNodeId,
        address defaultPaymentMethod,
        uint256 defaultPrice,
        address ownerAddress,
        string memory tokenBaseUri
    ) whenSpotIdIsValid(rootNodeId) {
        _name = tokenName;
        _symbol = tokenSymbol;
        _cap = maxSupply;
        baseURI = tokenBaseUri;

        require(ownerAddress != address(0), "Token needs an owner");
        _transferOwnership(ownerAddress);

        require(defaultPaymentMethod != address(0), "Invalid default payment method");
        require(defaultPrice > 0, "Default price of token is required");

        setPaymentMethod(1, defaultPaymentMethod, defaultPrice, 31536000, ownerAddress, false);

        Spot memory newSpot;
        newSpot.sequence = 1;
        newSpot.childCount = 0;
        newSpot.level = 0;
        newSpot.expiresAt = NEVER_EXPIRES;
        newSpot.owner = address(this);

        _communitySpots[rootNodeId] = newSpot;
        _spots[rootNodeId] = newSpot;
        _sequences[1] = newSpot;
        // Community spots never expire
        _neverExpireSpots[rootNodeId] = true;

        // We treat it like a mint to self
        emit Mint(address(this), rootNodeId, 0);
        emit Transfer(address(0), address(this), 1);
    }

    function mint(
        address owner,
        string calldata parentSpotId,
        string calldata newSpotId,
        uint256 _expiresAt
    ) public override onlyOwner whenNotMaxed whenSpotIdIsValid(newSpotId) {
        bool isCommunity = owner == address(this);

        if (!isCommunity) {
            string storage ownedSpotId = _spotOwnerships[owner];
            require(ownedSpotId.equal(""), "Owner already owns a spot");
        }
        require(_spots[newSpotId].sequence == 0, "Spot ID is already in use");

        Spot storage parentSpot = _spots[parentSpotId];
        require(parentSpot.sequence > 0, "Parent spot is invalid");
        require(parentSpot.childCount < MAX_SPOTS_PER_CODE, "This invite code is maxed out");

        uint256 nextSequence = MAX_SPOTS_PER_CODE * parentSpot.sequence + parentSpot.childCount + 1;
        uint256 nextLevel = parentSpot.level + 1;

        Spot memory newSpot;
        newSpot.owner = owner;
        newSpot.sequence = nextSequence;
        newSpot.childCount = 0;
        newSpot.expiresAt = isCommunity ? NEVER_EXPIRES : _expiresAt;
        newSpot.parent = parentSpotId;
        newSpot.level = nextLevel;
        if (isCommunity) {
            //Community spot
            _communitySpots[newSpotId] = newSpot;
        }

        parentSpot.childCount = parentSpot.childCount + 1;
        _totalSupply = _totalSupply + 1;
        _spotOwnerships[owner] = newSpotId;
        _spots[newSpotId] = newSpot;
        _sequences[nextSequence] = newSpot;

        emit Mint(owner, newSpotId, newSpot.expiresAt);
        emit Transfer(address(0), owner, nextSequence);
    }

    function mintWithPayoutHash(
        address owner,
        string calldata parentSpotId,
        string calldata newSpotId,
        uint256 _expiresAt,
        bytes32 _payoutHash
    ) public override onlyOwner whenNotMaxed whenSpotIdIsValid(newSpotId) {
        mint(owner, parentSpotId, newSpotId, _expiresAt);
        Spot storage existing = _spots[newSpotId];
        existing.payoutTo = _payoutHash;
    }

    /**
     * @dev Allows any user to buy a spot as long as they know the paymentMethodId and have an inviteCode
     *  restricted to one call per owner, and assumes owner's invite code will be randomly picked externally
     *  function restricts children of any spot to MAX_SPOTS_PER_CODE,
     *  this reverts if that is reached for the inviteCode provided
     * @param paymentMethodId ID of the payment option
     * @param invitedBySpotId ID of the spot that invited the user
     * @param newSpotId A randomized ID for the spot being bought, to have random function outside contract
     */
    function buy(
        uint256 paymentMethodId,
        string calldata invitedBySpotId,
        string calldata newSpotId
    ) public override whenNotMaxed nonReentrant whenSpotIdIsValid(newSpotId) {
        address owner = _msgSender();
        string storage ownedSpotId = _spotOwnerships[owner];
        require(ownedSpotId.equal(""), "You already own a spot");
        require(_spots[newSpotId].sequence == 0, "Spot ID is already in use");

        Spot storage parentSpot = _spots[invitedBySpotId];
        require(parentSpot.sequence > 0, "Parent spot is invalid");
        require(parentSpot.childCount < MAX_SPOTS_PER_CODE, "This invite code is maxed out");

        uint256 nextSequence = MAX_SPOTS_PER_CODE * parentSpot.sequence + parentSpot.childCount + 1;
        uint256 nextLevel = parentSpot.level + 1;

        (PricingOption memory paymentOption, IERC20 paymentMethod) = _paymentMethodVerify(paymentMethodId, nextLevel);
        paymentMethod.transferFrom(_msgSender(), paymentOption.paymentTo, paymentOption.price);

        Spot memory newSpot;
        newSpot.owner = owner;
        newSpot.sequence = nextSequence;
        newSpot.childCount = 0;
        newSpot.expiresAt = paymentOption.extendDuration + block.timestamp;
        newSpot.parent = invitedBySpotId;
        newSpot.level = nextLevel;

        parentSpot.childCount = parentSpot.childCount + 1;
        _totalSupply = _totalSupply + 1;
        _spotOwnerships[owner] = newSpotId;
        _spots[newSpotId] = newSpot;
        _sequences[nextSequence] = newSpot;

        emit Mint(owner, newSpotId, newSpot.expiresAt);
        emit Transfer(address(0), owner, nextSequence);
    }

    function buyAndSetPayoutHash(
        uint256 paymentMethodId,
        string calldata invitedBySpotId,
        string calldata newSpotId,
        bytes32 _payoutHash
    ) public override whenNotMaxed {
        buy(paymentMethodId, invitedBySpotId, newSpotId);
        setPayoutHash(newSpotId, _payoutHash);
    }

    /**
     * @dev Allows any wallet to extend a spot by pay its price, for as much as they want
     * @param paymentMethodId ID of the payment method
     * @param spotId ID of the spot being extended (even if not owned by sender)
     */
    function extend(uint256 paymentMethodId, string calldata spotId) public override {
        Spot storage extended = _spots[spotId];
        require(_spots[spotId].sequence > 0, "Spot ID is invalid");
        // We don't care about level on renewal, and using level 0 implies admin can set special pricing for renwals
        // Which affects everyone
        (PricingOption memory paymentOption, IERC20 paymentMethod) = _paymentMethodVerify(paymentMethodId, 0);

        bool penalty = hasPenalty(spotId);
        require(!penalty || paymentOption.canBeUsedForPenalty, "This payment method cannot be used with a penalty");

        paymentMethod.transferFrom(_msgSender(), paymentOption.paymentTo, paymentOption.price);
        if (extended.expiresAt < block.timestamp) { // An expired spot, extend from now
            extended.expiresAt = block.timestamp + paymentOption.extendDuration;
        } else { // Extend the duration form future timestamp
            extended.expiresAt = extended.expiresAt + paymentOption.extendDuration;
        }

        emit Extended(spotId, extended.expiresAt);
    }

    /**
     * @dev Allows admin to extend specific spots when they pay in fiat
     * @param spotId ID of the spot being extended
     * @param newExpiresAt uint256 timestamp of the new value for expires at
     */
    function adminExtend(string calldata spotId, uint256 newExpiresAt) public override onlyOwner {
        Spot storage extended = _spots[spotId];
        require(_spots[spotId].sequence > 0, "Spot ID is invalid");
        extended.expiresAt = newExpiresAt;
        emit Extended(spotId, extended.expiresAt);
    }

    function setPayoutHash(string calldata spotId, bytes32 _payoutHash) public override {
        Spot storage existing = _spots[spotId];

        require(existing.sequence > 0, "Spot ID is invalid");
        string storage ownedSpot = _spotOwnerships[_msgSender()];
        require(spotId.equal(ownedSpot), "Only owner can set this");
        require(_payoutHash != bytes32(0), "Invalid payout hash");
        existing.payoutTo = _payoutHash;

        emit PayoutChanged(spotId, _payoutHash);
    }

    /**
     * @dev Allows the owner to toggle on and off the never expires flag for a spot
     */
    function setNeverExpires(string calldata spotId, bool newValue) public override onlyOwner {
        require(_spots[spotId].sequence > 0, "Spot ID is invalid");
        _neverExpireSpots[spotId] = newValue;
    }

    /**
     * @dev Returns the expiry time of a spot
     * @param spotId string The ID of the spot
     * @return timestamp in seconds
     */
    function expiresAt(string calldata spotId) public view override returns (uint256) {
        Spot storage existing = _spots[spotId];
        require(existing.sequence > 0, "Spot ID is invalid");
        if (_neverExpireSpots[spotId] || _communitySpots[spotId].sequence > 0) {
            return NEVER_EXPIRES;
        }
        return existing.expiresAt;
    }

    /**
     * @dev Allows admin to set a penalty on a spot, normally occurs when an off chain issue
     *  triggers this
     */
    function setPenalty(string calldata spotId, bool isPenalized) public override onlyOwner {
        _penalizedSpots[spotId] = isPenalized;
        emit Penalty(spotId, isPenalized);
    }

    /**
     * @dev Checks if a spot has incurred a penalty either administratively or by not renewing
     */
    function hasPenalty(string calldata spotId) public view override returns (bool) {
        return _penalizedSpots[spotId] ||
            ( // Not community and hasn't renewed in PENALTY_AFTER_EXPIRY
                _communitySpots[spotId].sequence == 0 &&
                _spots[spotId].expiresAt < block.timestamp + PENALTY_AFTER_EXPIRY
            );
    }

    function getSpot(string calldata spotId) public view override returns (Spot memory) {
        return _spots[spotId];
    }

    function totalSupply() external view returns (uint256)
    {
        return _totalSupply;
    }

    function cap() external view returns (uint256) {
        return _cap;
    }

    function payoutHash(string calldata spotId) public override view returns (bytes32) {
        return _spots[spotId].payoutTo;
    }

    function calculateHash(string calldata spotId, string calldata rewardAddress) public view returns (bytes32){
        return keccak256(abi.encodePacked(spotId, rewardAddress));
    }

    function isPayoutHashValid(string calldata spotId, string calldata rewardAddress) public override view returns(bool) {
        bytes32 hash = calculateHash(spotId, rewardAddress);
        return hash == _spots[spotId].payoutTo;
    }

    function spotOf(address owner) public override view returns (string memory) {
        return _spotOwnerships[owner];
    }

    function ownerOfSpot(string calldata spotId) public override view returns (address) {
        return _spots[spotId].owner;
    }

    function ownerOf(uint256 tokenId) public override view returns (address) {
        return _sequences[tokenId].owner;
    }

    /**
     * @dev ERC20/721 compatible balance of to check if a wallet has a spot
     */
    function balanceOf(address owner) public override view returns (uint256) {
        return _spotOwnerships[owner].equal("") ? 0 : 1;
    }

    /**
     * @dev See {IERC721Metadata-name}.
     */
    function name() public override view returns (string memory) {
        return _name;
    }

    /**
     * @dev See {IERC721Metadata-symbol}.
     */
    function symbol() public override view returns (string memory) {
        return _symbol;
    }

    function setBaseURI(string memory baseUri) public onlyOwner {
        baseURI = baseUri;
    }

    /**
     * @dev See {IERC721Metadata-tokenURI}.
     */
    function tokenURI(uint256 tokenId) public override view returns (string memory) {
        require(_sequences[tokenId].sequence > 0, "ERC721: URI query for nonexistent token");
        return bytes(baseURI).length > 0 ? string(abi.encodePacked(baseURI, tokenId.toString())) : "";
    }

    function uri(uint256 tokenId) public view returns (string memory) {
        return tokenURI(tokenId);
    }

    function supportsInterface(bytes4 interfaceId) external view returns (bool) {
        return
            interfaceId == type(IERC721).interfaceId ||
            interfaceId == type(IERC721Metadata).interfaceId ||
            interfaceId == type(IMRNFT).interfaceId;
    }

    // Blocked methods:
    function safeTransferFrom(address from, address to, uint256 tokenId, bytes calldata data) public override {
        revert("Non transferable tokens");
    }

    function safeTransferFrom(address from, address to, uint256 tokenId) public override {
        revert("Non transferable tokens");
    }

    function transferFrom(address from, address to, uint256 tokenId) public override {
        revert("Non transferable tokens");
    }

    function approve(address to, uint256 tokenId) public override {
        revert("Non transferable tokens");
    }

    function setApprovalForAll(address operator, bool approved) public override {
        revert("Non transferable tokens");
    }

    function getApproved(uint256 tokenId) public override view returns (address operator) {
        return address(this);
    }

    function isApprovedForAll(address owner, address operator) public override view returns (bool) {
        return false;
    }
}