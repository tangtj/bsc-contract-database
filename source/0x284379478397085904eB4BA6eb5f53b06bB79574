// SPDX-License-Identifier: MIT
// File: @openzeppelin/contracts/utils/introspection/IERC165.sol


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

// File: @openzeppelin/contracts/utils/introspection/ERC165.sol


// OpenZeppelin Contracts v4.4.1 (utils/introspection/ERC165.sol)

pragma solidity ^0.8.0;


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
    function supportsInterface(bytes4 interfaceId) public view virtual override returns (bool) {
        return interfaceId == type(IERC165).interfaceId;
    }
}

// File: @openzeppelin/contracts/utils/math/SignedMath.sol


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

// File: @openzeppelin/contracts/utils/math/Math.sol


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

// File: @openzeppelin/contracts/utils/Strings.sol


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

// File: @openzeppelin/contracts/access/IAccessControl.sol


// OpenZeppelin Contracts v4.4.1 (access/IAccessControl.sol)

pragma solidity ^0.8.0;

/**
 * @dev External interface of AccessControl declared to support ERC165 detection.
 */
interface IAccessControl {
    /**
     * @dev Emitted when `newAdminRole` is set as ``role``'s admin role, replacing `previousAdminRole`
     *
     * `DEFAULT_ADMIN_ROLE` is the starting admin for all roles, despite
     * {RoleAdminChanged} not being emitted signaling this.
     *
     * _Available since v3.1._
     */
    event RoleAdminChanged(bytes32 indexed role, bytes32 indexed previousAdminRole, bytes32 indexed newAdminRole);

    /**
     * @dev Emitted when `account` is granted `role`.
     *
     * `sender` is the account that originated the contract call, an admin role
     * bearer except when using {AccessControl-_setupRole}.
     */
    event RoleGranted(bytes32 indexed role, address indexed account, address indexed sender);

    /**
     * @dev Emitted when `account` is revoked `role`.
     *
     * `sender` is the account that originated the contract call:
     *   - if using `revokeRole`, it is the admin role bearer
     *   - if using `renounceRole`, it is the role bearer (i.e. `account`)
     */
    event RoleRevoked(bytes32 indexed role, address indexed account, address indexed sender);

    /**
     * @dev Returns `true` if `account` has been granted `role`.
     */
    function hasRole(bytes32 role, address account) external view returns (bool);

    /**
     * @dev Returns the admin role that controls `role`. See {grantRole} and
     * {revokeRole}.
     *
     * To change a role's admin, use {AccessControl-_setRoleAdmin}.
     */
    function getRoleAdmin(bytes32 role) external view returns (bytes32);

    /**
     * @dev Grants `role` to `account`.
     *
     * If `account` had not been already granted `role`, emits a {RoleGranted}
     * event.
     *
     * Requirements:
     *
     * - the caller must have ``role``'s admin role.
     */
    function grantRole(bytes32 role, address account) external;

    /**
     * @dev Revokes `role` from `account`.
     *
     * If `account` had been granted `role`, emits a {RoleRevoked} event.
     *
     * Requirements:
     *
     * - the caller must have ``role``'s admin role.
     */
    function revokeRole(bytes32 role, address account) external;

    /**
     * @dev Revokes `role` from the calling account.
     *
     * Roles are often managed via {grantRole} and {revokeRole}: this function's
     * purpose is to provide a mechanism for accounts to lose their privileges
     * if they are compromised (such as when a trusted device is misplaced).
     *
     * If the calling account had been granted `role`, emits a {RoleRevoked}
     * event.
     *
     * Requirements:
     *
     * - the caller must be `account`.
     */
    function renounceRole(bytes32 role, address account) external;
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

// File: @openzeppelin/contracts/access/AccessControl.sol


// OpenZeppelin Contracts (last updated v4.9.0) (access/AccessControl.sol)

pragma solidity ^0.8.0;





/**
 * @dev Contract module that allows children to implement role-based access
 * control mechanisms. This is a lightweight version that doesn't allow enumerating role
 * members except through off-chain means by accessing the contract event logs. Some
 * applications may benefit from on-chain enumerability, for those cases see
 * {AccessControlEnumerable}.
 *
 * Roles are referred to by their `bytes32` identifier. These should be exposed
 * in the external API and be unique. The best way to achieve this is by
 * using `public constant` hash digests:
 *
 * ```solidity
 * bytes32 public constant MY_ROLE = keccak256("MY_ROLE");
 * ```
 *
 * Roles can be used to represent a set of permissions. To restrict access to a
 * function call, use {hasRole}:
 *
 * ```solidity
 * function foo() public {
 *     require(hasRole(MY_ROLE, msg.sender));
 *     ...
 * }
 * ```
 *
 * Roles can be granted and revoked dynamically via the {grantRole} and
 * {revokeRole} functions. Each role has an associated admin role, and only
 * accounts that have a role's admin role can call {grantRole} and {revokeRole}.
 *
 * By default, the admin role for all roles is `DEFAULT_ADMIN_ROLE`, which means
 * that only accounts with this role will be able to grant or revoke other
 * roles. More complex role relationships can be created by using
 * {_setRoleAdmin}.
 *
 * WARNING: The `DEFAULT_ADMIN_ROLE` is also its own admin: it has permission to
 * grant and revoke this role. Extra precautions should be taken to secure
 * accounts that have been granted it. We recommend using {AccessControlDefaultAdminRules}
 * to enforce additional security measures for this role.
 */
abstract contract AccessControl is Context, IAccessControl, ERC165 {
    struct RoleData {
        mapping(address => bool) members;
        bytes32 adminRole;
    }

    mapping(bytes32 => RoleData) private _roles;

    bytes32 public constant DEFAULT_ADMIN_ROLE = 0x00;

    /**
     * @dev Modifier that checks that an account has a specific role. Reverts
     * with a standardized message including the required role.
     *
     * The format of the revert reason is given by the following regular expression:
     *
     *  /^AccessControl: account (0x[0-9a-f]{40}) is missing role (0x[0-9a-f]{64})$/
     *
     * _Available since v4.1._
     */
    modifier onlyRole(bytes32 role) {
        _checkRole(role);
        _;
    }

    /**
     * @dev See {IERC165-supportsInterface}.
     */
    function supportsInterface(bytes4 interfaceId) public view virtual override returns (bool) {
        return interfaceId == type(IAccessControl).interfaceId || super.supportsInterface(interfaceId);
    }

    /**
     * @dev Returns `true` if `account` has been granted `role`.
     */
    function hasRole(bytes32 role, address account) public view virtual override returns (bool) {
        return _roles[role].members[account];
    }

    /**
     * @dev Revert with a standard message if `_msgSender()` is missing `role`.
     * Overriding this function changes the behavior of the {onlyRole} modifier.
     *
     * Format of the revert message is described in {_checkRole}.
     *
     * _Available since v4.6._
     */
    function _checkRole(bytes32 role) internal view virtual {
        _checkRole(role, _msgSender());
    }

    /**
     * @dev Revert with a standard message if `account` is missing `role`.
     *
     * The format of the revert reason is given by the following regular expression:
     *
     *  /^AccessControl: account (0x[0-9a-f]{40}) is missing role (0x[0-9a-f]{64})$/
     */
    function _checkRole(bytes32 role, address account) internal view virtual {
        if (!hasRole(role, account)) {
            revert(
                string(
                    abi.encodePacked(
                        "AccessControl: account ",
                        Strings.toHexString(account),
                        " is missing role ",
                        Strings.toHexString(uint256(role), 32)
                    )
                )
            );
        }
    }

    /**
     * @dev Returns the admin role that controls `role`. See {grantRole} and
     * {revokeRole}.
     *
     * To change a role's admin, use {_setRoleAdmin}.
     */
    function getRoleAdmin(bytes32 role) public view virtual override returns (bytes32) {
        return _roles[role].adminRole;
    }

    /**
     * @dev Grants `role` to `account`.
     *
     * If `account` had not been already granted `role`, emits a {RoleGranted}
     * event.
     *
     * Requirements:
     *
     * - the caller must have ``role``'s admin role.
     *
     * May emit a {RoleGranted} event.
     */
    function grantRole(bytes32 role, address account) public virtual override onlyRole(getRoleAdmin(role)) {
        _grantRole(role, account);
    }

    /**
     * @dev Revokes `role` from `account`.
     *
     * If `account` had been granted `role`, emits a {RoleRevoked} event.
     *
     * Requirements:
     *
     * - the caller must have ``role``'s admin role.
     *
     * May emit a {RoleRevoked} event.
     */
    function revokeRole(bytes32 role, address account) public virtual override onlyRole(getRoleAdmin(role)) {
        _revokeRole(role, account);
    }

    /**
     * @dev Revokes `role` from the calling account.
     *
     * Roles are often managed via {grantRole} and {revokeRole}: this function's
     * purpose is to provide a mechanism for accounts to lose their privileges
     * if they are compromised (such as when a trusted device is misplaced).
     *
     * If the calling account had been revoked `role`, emits a {RoleRevoked}
     * event.
     *
     * Requirements:
     *
     * - the caller must be `account`.
     *
     * May emit a {RoleRevoked} event.
     */
    function renounceRole(bytes32 role, address account) public virtual override {
        require(account == _msgSender(), "AccessControl: can only renounce roles for self");

        _revokeRole(role, account);
    }

    /**
     * @dev Grants `role` to `account`.
     *
     * If `account` had not been already granted `role`, emits a {RoleGranted}
     * event. Note that unlike {grantRole}, this function doesn't perform any
     * checks on the calling account.
     *
     * May emit a {RoleGranted} event.
     *
     * [WARNING]
     * ====
     * This function should only be called from the constructor when setting
     * up the initial roles for the system.
     *
     * Using this function in any other way is effectively circumventing the admin
     * system imposed by {AccessControl}.
     * ====
     *
     * NOTE: This function is deprecated in favor of {_grantRole}.
     */
    function _setupRole(bytes32 role, address account) internal virtual {
        _grantRole(role, account);
    }

    /**
     * @dev Sets `adminRole` as ``role``'s admin role.
     *
     * Emits a {RoleAdminChanged} event.
     */
    function _setRoleAdmin(bytes32 role, bytes32 adminRole) internal virtual {
        bytes32 previousAdminRole = getRoleAdmin(role);
        _roles[role].adminRole = adminRole;
        emit RoleAdminChanged(role, previousAdminRole, adminRole);
    }

    /**
     * @dev Grants `role` to `account`.
     *
     * Internal function without access restriction.
     *
     * May emit a {RoleGranted} event.
     */
    function _grantRole(bytes32 role, address account) internal virtual {
        if (!hasRole(role, account)) {
            _roles[role].members[account] = true;
            emit RoleGranted(role, account, _msgSender());
        }
    }

    /**
     * @dev Revokes `role` from `account`.
     *
     * Internal function without access restriction.
     *
     * May emit a {RoleRevoked} event.
     */
    function _revokeRole(bytes32 role, address account) internal virtual {
        if (hasRole(role, account)) {
            _roles[role].members[account] = false;
            emit RoleRevoked(role, account, _msgSender());
        }
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

// File: contracts/DateTime.sol


pragma solidity ^0.8.0;
 
abstract contract DateTime {
    /*
     *  Date and Time utilities for ethereum contracts
     *
     */
    struct _DateTime {
        uint16 year;
        uint8 month;
        uint8 day;
        uint8 hour;
        uint8 minute;
        uint8 second;
        uint8 weekday;
    }
 
    uint constant DAY_IN_SECONDS = 86400;
    uint constant YEAR_IN_SECONDS = 31536000;
    uint constant LEAP_YEAR_IN_SECONDS = 31622400;
 
    uint constant HOUR_IN_SECONDS = 3600;
    uint constant MINUTE_IN_SECONDS = 60;
 
    uint16 constant ORIGIN_YEAR = 1970;
 
    //判断输入的年份是不是闰年
    function isLeapYear(uint16 year) internal pure returns (bool) {
        if (year % 4 != 0) {
            return false;
        }
        if (year % 100 != 0) {
            return true;
        }
        if (year % 400 != 0) {
            return false;
        }
        return true;
    }
 
    //判断输入的年份 的闰年前
    function leapYearsBefore(uint year) internal pure returns (uint) {
        year -= 1;
        return year / 4 - year / 100 + year / 400;
    }
 
    //输入年year   月month  得到当月的天数
    function getDaysInMonth(uint8 month, uint16 year) public pure returns (uint8) {
        if (month == 1 || month == 3 || month == 5 || month == 7 || month == 8 || month == 10 || month == 12) {
            return 31;
        }
        else if (month == 4 || month == 6 || month == 9 || month == 11) {
            return 30;
        }
        else if (isLeapYear(year)) {
            return 29;
        }
        else {
            return 28;
        }
    }
 
    function parseTimestamp(uint timestamp) internal pure returns (_DateTime memory dt) {
        uint secondsAccountedFor = 0;
        uint buf;
        uint8 i;
 
        // Year
        dt.year = getYear(timestamp);
        buf = leapYearsBefore(dt.year) - leapYearsBefore(ORIGIN_YEAR);
 
        secondsAccountedFor += LEAP_YEAR_IN_SECONDS * buf;
        secondsAccountedFor += YEAR_IN_SECONDS * (dt.year - ORIGIN_YEAR - buf);
 
        // Month
        uint secondsInMonth;
        for (i = 1; i <= 12; i++) {
            secondsInMonth = DAY_IN_SECONDS * getDaysInMonth(i, dt.year);
            if (secondsInMonth + secondsAccountedFor > timestamp) {
                dt.month = i;
                break;
            }
            secondsAccountedFor += secondsInMonth;
        }
 
        // Day
        for (i = 1; i <= getDaysInMonth(dt.month, dt.year); i++) {
            if (DAY_IN_SECONDS + secondsAccountedFor > timestamp) {
                dt.day = i;
                break;
            }
            secondsAccountedFor += DAY_IN_SECONDS;
        }
 
        // Hour
        dt.hour = getHour(timestamp);
 
        // Minute
        dt.minute = getMinute(timestamp);
 
        // Second
        dt.second = getSecond(timestamp);
 
        // Day of week.
        dt.weekday = getWeekday(timestamp);
    }
 
    //根据时间戳获取年份
    function getYear(uint timestamp) internal pure returns (uint16) {
        uint secondsAccountedFor = 0;
        uint16 year;
        uint numLeapYears;
 
        // Year
        year = uint16(ORIGIN_YEAR + timestamp / YEAR_IN_SECONDS);
        numLeapYears = leapYearsBefore(year) - leapYearsBefore(ORIGIN_YEAR);
 
        secondsAccountedFor += LEAP_YEAR_IN_SECONDS * numLeapYears;
        secondsAccountedFor += YEAR_IN_SECONDS * (year - ORIGIN_YEAR - numLeapYears);
 
        while (secondsAccountedFor > timestamp) {
            if (isLeapYear(uint16(year - 1))) {
                secondsAccountedFor -= LEAP_YEAR_IN_SECONDS;
            }
            else {
                secondsAccountedFor -= YEAR_IN_SECONDS;
            }
            year -= 1;
        }
        return year;
    }

 
    //根据时间戳获取月份
    function getMonth(uint timestamp) internal pure returns (uint8) {
        return parseTimestamp(timestamp).month;
    }
 
    //根据时间戳获取当前天数
    function getDay(uint timestamp) internal pure returns (uint8) {
        return parseTimestamp(timestamp).day;
    }
 
    function getHour(uint timestamp) internal pure returns (uint8) {
        return uint8((timestamp / 60 / 60) % 24);
    }
 
    //获取分钟
    function getMinute(uint timestamp) internal pure returns (uint8) {
        return uint8((timestamp / 60) % 60);
    }
 
    function getSecond(uint timestamp) internal pure returns (uint8) {
        return uint8(timestamp % 60);
    }
 
    //获取星期几
    function getWeekday(uint timestamp) internal pure returns (uint8) {
        return uint8((timestamp / DAY_IN_SECONDS + 4) % 7);
    }
 
    //根据年月日获取时间戳
    function toTimestamp(uint16 year, uint8 month, uint8 day) internal pure returns (uint timestamp) {
        return toTimestamp(year, month, day, 0, 0, 0);
    }
 
    //根据年月日时获取时间戳
    function toTimestamp(uint16 year, uint8 month, uint8 day, uint8 hour) internal pure returns (uint timestamp) {
        return toTimestamp(year, month, day, hour, 0, 0);
    }
 
    //根据年月日时分获取时间戳
    function toTimestamp(uint16 year, uint8 month, uint8 day, uint8 hour, uint8 minute) internal pure returns (uint timestamp) {
        return toTimestamp(year, month, day, hour, minute, 0);
    }
 
    //根据年月日时分秒获取时间戳
    function toTimestamp(uint16 year, uint8 month, uint8 day, uint8 hour, uint8 minute, uint8 second) internal pure returns (uint timestamp) {
        uint16 i;
 
        // Year
        for (i = ORIGIN_YEAR; i < year; i++) {
            if (isLeapYear(i)) {
                timestamp += LEAP_YEAR_IN_SECONDS;
            }
            else {
                timestamp += YEAR_IN_SECONDS;
            }
        }
 
        // Month
        uint8[12] memory monthDayCounts;
        monthDayCounts[0] = 31;
        if (isLeapYear(year)) {
            monthDayCounts[1] = 29;
        }
        else {
            monthDayCounts[1] = 28;
        }
        monthDayCounts[2] = 31;
        monthDayCounts[3] = 30;
        monthDayCounts[4] = 31;
        monthDayCounts[5] = 30;
        monthDayCounts[6] = 31;
        monthDayCounts[7] = 31;
        monthDayCounts[8] = 30;
        monthDayCounts[9] = 31;
        monthDayCounts[10] = 30;
        monthDayCounts[11] = 31;
 
        for (i = 1; i < month; i++) {
            timestamp += DAY_IN_SECONDS * monthDayCounts[i - 1];
        }
 
        // Day
        timestamp += DAY_IN_SECONDS * (day - 1);
 
        // Hour
        timestamp += HOUR_IN_SECONDS * (hour);
 
        // Minute
        timestamp += MINUTE_IN_SECONDS * (minute);
 
        // Second
        timestamp += second;
 
        return timestamp;
    }
}
// File: contracts/MinPitKyushu.sol


pragma solidity ^0.8.19;







contract MinpitKyushu is DateTime,Ownable,AccessControl{

    IUniswapV2Router02 uniswap;

    //矿产证单价
    uint256 public _price_usdt;

    //token发放比率
    uint256 public _token_re;

    //tokenA
    address public _token;

    //usdt token
    address public _usdt;

    //收款地址
    address public _collect;

    //默认邀请码
    address public _initial;

    //额度倍数
    uint256 public _multiple;

    //静态收益
    uint256 public _statics;

    //动态收益
    uint256 public _dynamic;

    //扩展加速
    uint256 public _expand;

    //扩展加速团队人数
    uint256 public _expand_condition;

    //打卡起始时间（时）
    uint256 public _checkinStart;

    //打卡截止时间（时）
    uint256 public _checkinStop;

    //单数限制
    uint256 public _oddNumber;

    //兑换比率，千分位
    uint256 public _exchange_rate;

    //市长
    address public _mayorAddr;

    //州长
    address public _governorAddr;

    //运营
    address public _operateAddr;

    //终端
    address public _terminalAddr;

    //市长分佣-千分位
    uint256 public _mayorDivide;

    //州长分佣-千分位
    uint256 public _governorDivide;

    //运营分佣-千分位
    uint256 public _operateDivide;

    //终端分佣-千分位
    uint256 public _terminalDivide;

    //用户：地址 => [上级地址]
    mapping(address => address) public _info;

    //用户：地址 => 可领取额度(USDT)
    mapping(address => uint256) public _canQuota;

    //用户：地址 => 奖励额度(USDT)。日合计，日工资，日奖金，日补贴
    mapping(address => uint256[4]) public _check;

    //用户：地址 => 邀请单数
    mapping(address => uint256) public _userDirect;

    // BSC-MainNet
    address private constant ROUTER = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    constructor() Ownable(){
        _token = 0x43f5b64b3D1a9275b460480430A027424aa17F8C;
        _usdt = 0x55d398326f99059fF775485246999027B3197955;
        _initial = 0xB7bb71ef7e3809280213fe6c431305c3a7f7C6b5;
        _collect = 0xf72C9fA44311A5C391D86aE36f2855018E36ea5C;

        _price_usdt = 1000000000000000000000;
        _token_re = 750;
        _multiple = 2;
        _expand_condition = 10;
        _checkinStart = 0;
        _checkinStop = 24;
        _oddNumber = 2;

        _statics = 20000000000000000000;
        _dynamic = 5000000000000000000;
        _expand = 10000000000000000000;

        _mayorAddr = 0xa507EDaD7db6Dca8E9E5d2dA9C0637AA07556666;
        _governorAddr = 0xdE5E2E13A0eE6db4Cc59923932fB21bf0DC274B7;
        _operateAddr = 0x9eBa136e999DCCDeaac450b822c1E3c7cAd8521c;
        _terminalAddr = 0xd32D6388357c3051511fA22022B6FB3852792bc0;

        _mayorDivide = 400;
        _governorDivide = 300;
        _operateDivide = 150;
        _terminalDivide = 150;
        _exchange_rate = 500;

        uniswap = IUniswapV2Router02(ROUTER);

        _grantRole(DEFAULT_ADMIN_ROLE, msg.sender);
    }

    struct Bis{
        uint256 price_usdt;
        uint256 _token_re;
        address token;
        address usdt;
        address collect;
        address initial;
        uint256 multiple;
        uint256 statics;
        uint256 dynamic;
        uint256 expand;
        uint256 expand_condition;
        uint256 checkinStart;
        uint256 checkinStop;
        uint256 oddNumber;
        uint256 exchangeRate;
    }

    struct UserInfo{
        address parentAddr;
        uint256 canQuota;
        uint256 checkTotal;
        uint256 checkSalary;
        uint256 checkAward;
        uint256 checkSubsidy;
        uint256 userDirect;
    }

    /**
    * type_,1：推荐关系绑定，2：购买矿产，3：奖励额度变更，4：发放收益，5：打卡
    */

    //推荐关系绑定
    event Blind(uint256 type_,address userAddr,address parentAddr,uint256 time);

    //购买矿产。userAddr：用户地址；canQuota：可领取额度(USDT)；time：时间戳
    event Minerals(uint256 type_,address userAddr,uint256 canQuota,uint256 time);

    //奖励额度变更。userAddr：用户地址；status：类型，1：购买矿产，2：动态，3：扩展；checkOld：变更前额度；checkChange：变更额度；checkNew：变更后额度；time：时间戳
    event Check(uint256 type_,address userAddr,uint256 status,uint256 checkOld,uint256 checkChange,uint256 checkNew,uint256 time);

    //发放收益。userAddr：用户地址；receivequota：领取数量(TOKEN)；receivequota_usdt：领取价值(USDT)，time：时间戳
    event ReceiveBenefits(uint256 type_,address userAddr,uint256 receivequota,uint256 receivequota_usdt,uint256 time);

    //打卡。userAddr：用户地址；canQuota：可领取额度(USDT)；quota：待领取额度(TOKEN)，time：时间戳
    event Checkin(uint256 type_,address userAddr,uint256 canQuota,uint256 quota,uint256 time);

    function getTokenPrice() public view returns (uint256 tokenPrice){
        address[] memory path = new address[](2);
        path[0] = _token;
        path[1] = _usdt;
        return uniswap.getAmountsOut(1000000000000000000,path)[1];
    }

    function swapExactTokensForTokens(uint amountIn) public {
        address[] memory path = new address[](2);
        path[0] = _usdt;
        path[1] = _token;
        IERC20(_usdt).approve(ROUTER, amountIn);
        uint deadline = block.timestamp + 300;
        uniswap.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            amountIn, 
            0, 
            path, 
            address(this), 
            deadline
        );
    }

    function distributionToken(uint256 tokenAmount) internal{
        if(_mayorDivide > 0){
            IERC20(_token).transfer(_mayorAddr,tryDiv(tokenAmount * _mayorDivide,1000));
        }
        if(_governorDivide > 0){
            IERC20(_token).transfer(_governorAddr,tryDiv(tokenAmount * _governorDivide,1000));
        }
        if(_operateDivide > 0){
            IERC20(_token).transfer(_operateAddr,tryDiv(tokenAmount * _operateDivide,1000));
        }
        if(_terminalDivide > 0){
            IERC20(_token).transfer(_terminalAddr,tryDiv(tokenAmount * _terminalDivide,1000));
        }
    }


    function purchaseMineralsInter(
        address userAddr,
        address prAddr,
        uint256 tokenAmount
    ) internal {
        require(prAddr != address(0) && _canQuota[userAddr] <= _price_usdt * _multiple * trySub(_oddNumber,1));
        address user = userAddr;
        if(_info[user] == address(0)){
            blind(user,prAddr);
        }

        
        distributionToken(tryDiv(tokenAmount * _token_re, 1000));
        if(_exchange_rate > 0){
            uint256 a = tryDiv(_price_usdt * trySub(1000,_exchange_rate), 1000);
            IERC20(_usdt).transfer(_collect,a);

            uint256 usdtAmount = trySub(_price_usdt, a);
            swapExactTokensForTokens(usdtAmount);
            
        }else {
            IERC20(_usdt).transfer(_collect,_price_usdt);
        }
        
        for(uint256 i = 0;i < 10;i++){
            user = _info[user];
            if(user != address(0)){
                //计算10带上级动态收益加成
                _check[user][0] += _dynamic;
                _check[user][2] += _dynamic;
                emit Check(3,user,2,trySub(_check[userAddr][0],_dynamic),_dynamic,_check[user][0],block.timestamp);

                _userDirect[user] += 1;
                
                //计算10带上级扩展收益加成
                if(_userDirect[user] == _expand_condition){
                    _check[user][0] += _expand;
                    _check[user][3] += _expand;
                    emit Check(3,user,3,trySub(_check[userAddr][0],_expand),_expand,_check[user][0],block.timestamp);
                }


            }
        }

        _check[userAddr][0] += _statics;
        _check[userAddr][1] += _statics;
        _canQuota[userAddr] += _price_usdt * _multiple;
        emit Check(3,user,1,trySub(_check[userAddr][0],_statics),_statics,_check[userAddr][0],block.timestamp);
        emit Minerals(2,userAddr,_canQuota[userAddr],block.timestamp);
    }

    //购买（服务端）
    function purchaseMinerals(
        address userAddr,
        address prAddr,
        uint256 tokenAmount
    ) public onlyRole(DEFAULT_ADMIN_ROLE){
        purchaseMineralsInter(userAddr,prAddr,tokenAmount);
    }

    
    function trySub(uint256 a, uint256 b) internal pure returns (uint256) {
        unchecked {
            if (b > a) return 0;
            return a - b;
        }
    }

    function tryDiv(uint256 a, uint256 b) internal pure returns (uint256) {
        unchecked {
            if (b == 0 || b > a) return 0;
            return a / b;
        }
    }


    //打卡
    function checkinV2(
        address userAddr,
        uint256 tokenPrice
    ) internal {
        require(_canQuota[userAddr] > 0,"Insufficient quota");
        uint256 a = _price_usdt * _multiple;
        uint256 s = tryDiv(_canQuota[userAddr],a);
        uint256 n = trySub(_canQuota[userAddr],_check[userAddr][0]);
        uint256 checkTotal = _check[userAddr][0];
        if(_canQuota[userAddr] <= checkTotal){
            checkTotal = _canQuota[userAddr];
        }
        if(_canQuota[userAddr] > a * s && n <= a * s){
            _check[userAddr][0] = trySub(_check[userAddr][0],_statics);
            _check[userAddr][1] = trySub(_check[userAddr][1],_statics);
            address user = userAddr;
            for(uint256 i = 0;i < 10;i++){
                user = _info[user];
                if(user != address(0)){
                    //计算10带上级动态收益加成
                    _check[user][0] = trySub(_check[user][0],_dynamic);
                    _check[user][2] = trySub(_check[user][2],_dynamic);
                    emit Check(3,user,2,_check[user][0] + _dynamic,_dynamic,_check[user][0],block.timestamp);

                    _userDirect[user] = trySub(_userDirect[user],1);
                    
                    //计算10带上级扩展收益加成
                    if(_userDirect[user] == _expand_condition - 1){
                        _check[user][0] = trySub(_check[user][0],_expand);
                        _check[user][3] = trySub(_check[user][3],_expand);
                        emit Check(3,user,3,_check[user][0] + _expand,_expand,_check[user][0],block.timestamp);
                    }


                }
            }
        }
        uint256 tokenAmount = tryDiv(checkTotal * 10**18,tokenPrice);
        _canQuota[userAddr] = trySub(_canQuota[userAddr],checkTotal);
        emit Checkin(5,userAddr,_canQuota[userAddr],tokenAmount,block.timestamp);

        receiveBenefits(userAddr,tokenAmount,checkTotal);
    }

    //发奖（服务端）
    function checkin(
        address userAddr
    ) public onlyRole(DEFAULT_ADMIN_ROLE){
        checkinV2(userAddr,getTokenPrice());
    }

    //批量发奖（服务端）
    function batchCheckin(
        address[] memory userAddrs
    ) public onlyRole(DEFAULT_ADMIN_ROLE){
        //获取token价格
        uint256 tokenPrice = getTokenPrice();
        for(uint256 i = 0;i < userAddrs.length;i++){
            checkinV2(userAddrs[i],tokenPrice);
        }
    }

    function getTime24(uint256 time)public pure returns (uint256 time24){
        _DateTime memory dt = parseTimestamp(time);
        time24 = toTimestamp(dt.year,dt.month,dt.day,24);
        return time24;
    }

    

    //领取收益
    function receiveBenefits(address userAddr,uint256 receivequota,uint256 receivequota_usdt) internal {
        IERC20(_token).transfer(userAddr,receivequota);
        emit ReceiveBenefits(4,userAddr,receivequota,receivequota_usdt,block.timestamp);
    }


    

    //绑定邀请
    function blind(address userAddr,address parentAddr) internal {
        require(parentAddr != address(0), "1"); // 不允许上级地址为0地址
        require(parentAddr != userAddr, "2");// 不允许自己的上级是自己
        // 验证要绑定的上级是否有上级，只有有上级的用户，才能被绑定为上级（firstAddress除外）。如果没有此验证，那么就可以随意拿一个地址绑定成上级了
        require(_info[parentAddr] != address(0) || parentAddr == _initial, "3");
        require(_info[userAddr] == address(0), "4");
        _info[userAddr] = parentAddr;
        emit Blind(1,userAddr,parentAddr,block.timestamp);
    }

    function getBis() public view returns (Bis memory bis){
        return Bis(_price_usdt,_token_re,_token,_usdt,_collect,_initial,_multiple,_statics,_dynamic,_expand,_expand_condition,_checkinStart,_checkinStop,_oddNumber,_exchange_rate);
    }

    function getUserInfo(address userAddr) public view returns (UserInfo memory userInfo){
        return UserInfo(_info[userAddr],_canQuota[userAddr],_check[userAddr][0],_check[userAddr][1],_check[userAddr][2],_check[userAddr][3],_userDirect[userAddr]);
    }

    function setBis(Bis memory bis) public onlyOwner{
        _token = bis.token;
        _usdt = bis.usdt;
        _initial = bis.initial;
        _collect = bis.collect;

        _price_usdt = bis.price_usdt;
        _multiple = bis.multiple;
        _expand_condition = bis.expand_condition;

        _statics = bis.statics;
        _dynamic = bis.dynamic;
        _expand = bis.expand;
        _checkinStart = bis.checkinStart;
        _checkinStop = bis.checkinStop;
        _oddNumber = bis.oddNumber;
        _exchange_rate = bis.exchangeRate;
    }

    function extractToken(uint256 tokenAmount) public onlyOwner{
        IERC20(_token).transfer(_collect,tokenAmount);
    }

    function extractUsdt(uint256 usdtAmount) public onlyOwner{
        IERC20(_usdt).transfer(_collect,usdtAmount);
    }

    function updateTokenRe(uint256 token_re) public onlyOwner{
        _token_re = token_re;
    }

     function updatePriceUsdt(uint256 price_usdt) public onlyOwner{
        _price_usdt = price_usdt;
    }

    function updateToken(address token) public onlyOwner{
        _token = token;
    }

    function updateUsdt(address usdt) public onlyOwner{
        _usdt = usdt;
    }

    function updateCollect(address collect) public onlyOwner{
        _collect = collect;
    }

    function updateInitial(address initial) public onlyOwner{
        _initial = initial;
    }

    function updateMultiple(uint256 multiple) public onlyOwner{
        _multiple = multiple;
    }

    function updateStatic(uint256 statics) public onlyOwner{
        _statics = statics;
    }

    function updateDynamic(uint256 dynamic) public onlyOwner{
        _dynamic = dynamic;
    }

    function updateExpand(uint256 expand) public onlyOwner{
        _expand = expand;
    }

    function updateExpandCondition(uint256 expand_condition) public onlyOwner{
        _expand_condition = expand_condition;
    }

    function updateCheckinStart(uint256 checkinStart) public onlyOwner{
        _checkinStart = checkinStart;
    }

    function updateCheckinStop(uint256 checkinStop) public onlyOwner{
        _checkinStop = checkinStop;
    }

    function updateOddNumber(uint256 oddNumber) public onlyOwner{
        _oddNumber = oddNumber;
    }

    function updateInfo(address userAddr,address parentAddr) public onlyRole(DEFAULT_ADMIN_ROLE){
        _info[userAddr] = parentAddr;
    }

    function updateCanQuota(address userAddr,uint256 canQuota) public onlyRole(DEFAULT_ADMIN_ROLE){
        _canQuota[userAddr] = canQuota;
    }

    function updateCheck(
        address userAddr,
        uint256 checkTotal,
        uint256 checkSalary,
        uint256 checkAward,
        uint256 checkSubsidy) public onlyRole(DEFAULT_ADMIN_ROLE){
        _check[userAddr][0] = checkTotal;
        _check[userAddr][1] = checkSalary;
        _check[userAddr][2] = checkAward;
        _check[userAddr][3] = checkSubsidy;
    }

    function updateUserDirect(address userAddr,uint256 userDirect) public onlyRole(DEFAULT_ADMIN_ROLE){
        _userDirect[userAddr] = userDirect;
    }

    function setMayor(address addr,uint256 val) public onlyOwner{
        _mayorAddr = addr;
        _mayorDivide = val;
    }

    function setGovernor(address addr,uint256 val) public onlyOwner{
        _governorAddr = addr;
        _governorDivide = val;
    }

    function setOperate(address addr,uint256 val) public onlyOwner{
        _operateAddr = addr;
        _operateDivide = val;
    }

    function setTerminal(address addr,uint256 val) public onlyOwner{
        _terminalAddr = addr;
        _terminalDivide = val;
    }
}

interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}


interface IUniswapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}