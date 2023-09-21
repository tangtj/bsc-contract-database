// SPDX-License-Identifier: MIT
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

// File: @openzeppelin/contracts/utils/math/Math.sol


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

// File: @openzeppelin/contracts/utils/Strings.sol


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

// File: @openzeppelin/contracts/utils/Address.sol


// OpenZeppelin Contracts (last updated v4.8.0) (utils/Address.sol)

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

// File: @openzeppelin/contracts/token/ERC20/extensions/draft-IERC20Permit.sol


// OpenZeppelin Contracts v4.4.1 (token/ERC20/extensions/draft-IERC20Permit.sol)

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

// File: @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol


// OpenZeppelin Contracts (last updated v4.8.0) (token/ERC20/utils/SafeERC20.sol)

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
        // we're implementing it ourselves. We use {Address-functionCall} to perform this call, which verifies that
        // the target address contains contract code and also asserts for success in the low-level call.

        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        if (returndata.length > 0) {
            // Return data is optional
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}

// File: contracts/SmartGoldBullion.sol


pragma solidity ^0.8.18;





interface TeamData {
    function getUserReferrer(address _userAddr) external view returns (address);
    function getUserRank(address _userAddr) external view returns (uint256);
    function getTeamUsers(address _userAddr, uint256 _generation) external view returns (address[] memory);
    function updateRank(address _userAddr, uint256 _newRank) external;
}

contract SGB is Ownable {

    using Strings for uint256;
    using SafeERC20 for IERC20;

    uint256 private startTime;
    address private teamDataContract;

    address private dev;

    address private immutable usdt = 0x55d398326f99059fF775485246999027B3197955;
    address private immutable sgc = 0x7E197abf80487299A1375bca225f6155890bc28A;

    uint256 private immutable timeStep = 1 days;

    uint256 public baseROI = 3;
    uint256 public divider = 100;
    uint256 public maxEarnMultiplier = 3;
    
    uint256 public referDepth = 300;
    uint256 public dynamicBonus = 5;
    uint256 public dynamicBonus1 = 350;
    uint256 public dynamicBonus2 = 300;
    uint256 public vipBonus = 20;
    uint256 public bonusDivider = 1000;
    uint256 public exchangeSGCTxn = 1;

    uint256 private sgcPrice = 1000;

    uint256 public investUSDTWeight = 90;
    uint256 public investCreditWeight = 10;

    uint256 public reinvestCreditWeight = 50;
    uint256 public reinvestSGCWeight = 50;
    uint256 public reinvestMultiplier = 150;

    uint256[] public package = [150e18, 1500e18, 4500e18];
    uint256[] public salesRequired = [0, 0, 5000e18, 10000e18, 20000e18, 60000e18, 240000e18];
    uint256[] public requiredVIP = [0, 0, 0, 2, 2, 3, 4];

    struct TokenBalance {
        uint256 sgc;
        uint256 usdt;
        uint256 credit;
        uint256 reinvest;
        uint256 transfer;
    }
    mapping(address=>TokenBalance) public tokenBalance;

    struct InvestmentInfo {
        uint256 totalInvestment;
        uint256 activeInvestment;
        uint256 lastClaim;
        uint256 maxEarning;
        uint256 withdrew;
        uint256 totalSales;
    }
    mapping(address=>InvestmentInfo) public investmentInfo;

    struct PayoutInfo {
        uint256 txnID;
        address receiver;
        uint256 amount;
        bool completed;
    }
    mapping(uint256=>PayoutInfo[]) private payoutInfos;

    mapping(uint256=>uint256) public dayTotalInvest;
    mapping(uint256=>address[]) public dayInvestors;
    mapping(uint256=>uint256) public dayTotalReinvest;
    mapping(uint256=>address[]) public dayReinvestors;
    mapping(address=>mapping(uint256=>uint256)) private userDayEarning;
    mapping(uint256=>uint256) public roiByDay;
    mapping(address=>bool) private admin;
    mapping(address=>string) private adminName;

    constructor (
        address _dev,
        address _teamContract,
        uint256 _startTime
    ) {
        dev = _dev;
        teamDataContract = _teamContract;
        startTime = _startTime;
    }

    function invest(uint256 _package, uint256 _gen, bool _toggle) external {

        uint256 usdtNeed = package[_package]*investUSDTWeight/divider;
        uint256 usdtBalance = IERC20(usdt).balanceOf(msg.sender);
        require(usdtBalance >= usdtNeed, "Insufficient USDT balance.");

        uint256 csgcNeed = (package[_package]*investCreditWeight/divider);
        require(tokenBalance[msg.sender].credit >= csgcNeed, "Insufficient credit-sgc.");

        tokenBalance[msg.sender].credit -= csgcNeed;
        IERC20(usdt).safeTransferFrom(msg.sender, address(this), usdtNeed);

        if (investmentInfo[msg.sender].totalInvestment == 0) {
            investmentInfo[msg.sender].lastClaim = block.timestamp;
        }
        investmentInfo[msg.sender].totalInvestment += package[_package];
        investmentInfo[msg.sender].activeInvestment += package[_package];
        investmentInfo[msg.sender].maxEarning += (package[_package]*maxEarnMultiplier);

        uint256 curDay = getCurDay();
        dayTotalInvest[curDay] += package[_package];
        dayInvestors[curDay].push(msg.sender);

        if (_toggle == true) {
            updateUserRanking(msg.sender, _gen);
            calculateUplineBonus(msg.sender, package[_package]);
        }

    }

    function airdrop(address _userAddr, uint256 _package, uint256 _gen, bool _toggle) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        if (investmentInfo[_userAddr].totalInvestment == 0) {
            investmentInfo[_userAddr].lastClaim = block.timestamp;
        }
        investmentInfo[_userAddr].totalInvestment += package[_package];
        investmentInfo[_userAddr].activeInvestment += package[_package];
        investmentInfo[_userAddr].maxEarning += (package[_package]*maxEarnMultiplier);
        uint256 curDay = getCurDay();
        dayTotalInvest[curDay] += package[_package];
        dayInvestors[curDay].push(_userAddr);
        if (_toggle == true) {
            updateUserRanking(_userAddr, _gen);
            calculateUplineBonus(_userAddr, package[_package]);
        }
    }

    function claimDailyReturn() external {
        uint256 today = getCurDay();
        require(block.timestamp - investmentInfo[msg.sender].lastClaim >= timeStep, "Not yet one day.");
        require(userDayEarning[msg.sender][today] == 0, "Already claimed earning for today.");

        uint256 todayROI = baseROI;
        if (roiByDay[today] > 0) {
            todayROI = roiByDay[today];
        }

        uint256 dailyEarning = investmentInfo[msg.sender].activeInvestment*todayROI/divider;
        investmentInfo[msg.sender].activeInvestment -= dailyEarning;
        userDayEarning[msg.sender][today] = dailyEarning;

        tokenBalance[msg.sender].transfer += (dailyEarning*dynamicBonus1/bonusDivider);
        tokenBalance[msg.sender].reinvest += (dailyEarning*dynamicBonus1/bonusDivider);
        tokenBalance[msg.sender].usdt += (dailyEarning*dynamicBonus2/bonusDivider);

        investmentInfo[msg.sender].lastClaim = block.timestamp;

    }

    function withdrawUSDT(address _userAddr, uint256 _amount) external {
        uint256 balance = IERC20(usdt).balanceOf(address(this));
        require(balance >= _amount, "Contract insufficient balance.");
        require(tokenBalance[_userAddr].usdt >= _amount, "User insufficient USDT.");
        tokenBalance[_userAddr].usdt -= _amount;
        IERC20(usdt).safeTransfer(_userAddr, _amount);
        investmentInfo[_userAddr].withdrew += _amount;
    }

    function withdrawSGC(address _userAddr, uint256 _amount) external {
        uint256 balance = IERC20(sgc).balanceOf(address(this));
        require(balance >= _amount, "Contract insufficient balance.");
        require(tokenBalance[_userAddr].sgc >= _amount, "User insufficient USDT.");
        tokenBalance[_userAddr].sgc -= _amount;
        IERC20(sgc).safeTransfer(_userAddr, _amount);
    }

    function reinvest(uint256 _amount, uint256 _gen, bool _toggle) external {

        uint256 rsgcNeed = _amount*reinvestCreditWeight/divider;
        require(tokenBalance[msg.sender].reinvest >= rsgcNeed, "Insufficient reinvest-sgc.");

        uint256 sgcNeed = (_amount*reinvestSGCWeight/divider);
        uint256 sgcBalance = IERC20(sgc).balanceOf(msg.sender);
        require(sgcBalance >= sgcNeed, "Insufficient SGC balance.");

        tokenBalance[msg.sender].reinvest -= rsgcNeed;
        IERC20(sgc).safeTransferFrom(msg.sender, address(this), sgcNeed);

        investmentInfo[msg.sender].activeInvestment += (_amount*reinvestMultiplier/divider);

        if (_toggle == true) {
            updateUserRanking(msg.sender, _gen);
            calculateUplineBonus(msg.sender, _amount);
        }

        uint256 curDay = getCurDay();
        dayTotalReinvest[curDay] += _amount;
        dayReinvestors[curDay].push(msg.sender);

    }

    function updateUserRanking(address _userAddr, uint256 _gen) public {
        uint256 rank = TeamData(teamDataContract).getUserRank(_userAddr);
        uint256 vipMembers = checkDownlineVIP(_userAddr, _gen);
        uint256 eligibleRank;
        for (uint256 i = rank; i < salesRequired.length; i++) {
            if (investmentInfo[_userAddr].activeInvestment > 0) {
                if (investmentInfo[_userAddr].totalSales >= salesRequired[i] && vipMembers >= requiredVIP[i]) {
                    eligibleRank = i;
                }
            }
        }
        if (eligibleRank > rank) {
            TeamData(teamDataContract).updateRank(_userAddr, eligibleRank);
        }
    }

    function checkDownlineVIP(address _userAddr, uint256 _generation) public view returns (uint256) {
        address[] memory teamUsers = TeamData(teamDataContract).getTeamUsers(_userAddr, _generation);
        uint256 userRank = TeamData(teamDataContract).getUserRank(_userAddr);
        uint256 VIPcount = 0;
        for (uint256 i = 0; i < teamUsers.length; i++) {
            uint256 teamUserRank = TeamData(teamDataContract).getUserRank(teamUsers[i]);
            if (userRank > 1) {
                if (teamUserRank >= userRank-1) {
                    VIPcount++;
                }
            }
        }
        return VIPcount;
    }

    function calculateUplineBonus(address _userAddr, uint256 _amount) private {
        address upline = TeamData(teamDataContract).getUserReferrer(_userAddr);
        uint256 downlineRank = TeamData(teamDataContract).getUserRank(_userAddr);

        for (uint256 i = 0; i < referDepth; i++) {
            if (upline != address(0)) {
                uint256 uplineRank = TeamData(teamDataContract).getUserRank(upline);
                if (investmentInfo[upline].activeInvestment > 0 && uplineRank > 0) {
                    if (uplineRank == 1) {
                        uint256 reward = _amount*dynamicBonus/bonusDivider;
                        if (i < 5) {
                            tokenBalance[upline].sgc += (reward*dynamicBonus1/bonusDivider);
                            tokenBalance[upline].reinvest += (reward*dynamicBonus1/bonusDivider);
                            tokenBalance[upline].usdt += (reward*dynamicBonus2/bonusDivider);
                        }
                    } else if (uplineRank > 1 && uplineRank == downlineRank) {
                        uint256 reward = _amount*dynamicBonus/bonusDivider;
                        if (i < 10) {
                            tokenBalance[upline].sgc += (reward*dynamicBonus1/bonusDivider);
                            tokenBalance[upline].reinvest += (reward*dynamicBonus1/bonusDivider);
                            tokenBalance[upline].usdt += (reward*dynamicBonus2/bonusDivider);
                        }
                    } else if (uplineRank > 1) {
                        uint256 reward = _amount*vipBonus/bonusDivider;
                        tokenBalance[upline].sgc += (reward*dynamicBonus1/bonusDivider);
                        tokenBalance[upline].reinvest += (reward*dynamicBonus1/bonusDivider);
                        tokenBalance[upline].usdt += (reward*dynamicBonus2/bonusDivider);
                    }
                }
                investmentInfo[upline].totalSales += _amount;
                if (upline == dev) break;
                downlineRank = uplineRank;
                upline = TeamData(teamDataContract).getUserReferrer(upline);
            } else {
                break;
            }
        }
    }

    function setROIByDay(uint256 _day, uint256 _roi) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        roiByDay[_day] = _roi;
    }

    function setReinvestCreditWeight(uint256 _newValue) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        reinvestCreditWeight = _newValue;
    }

    function setReInvestSGCWeight(uint256 _newValue) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        reinvestSGCWeight = _newValue;
    }

    function setReinvestMultiplier(uint256 _newValue) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        reinvestMultiplier = _newValue;
    }

    function setSalesRequired(uint256[] memory _newValues) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        salesRequired = _newValues;
    }

    function setRequiredVIP(uint256[] memory _newValues) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        requiredVIP = _newValues;
    }

    // Token Balance
    function getTokenBalanceByAddr(address _userAddr) external view returns (TokenBalance memory) {
        return tokenBalance[_userAddr];
    }

    // USDT
    function setUserUSDT(address _userAddr, uint256 _value) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        tokenBalance[_userAddr].usdt = _value;
    }

    function increaseUserUSDT(address _receiver, uint256 _increaseAmount) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        tokenBalance[_receiver].usdt += _increaseAmount;
    }

    function decreaseUserUSDT(address _spender, uint256 _decreaseAmount) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        require(tokenBalance[_spender].usdt >= _decreaseAmount, "User insufficient credit points.");
        tokenBalance[_spender].usdt -= _decreaseAmount;
    }

    function getUserUSDT(address _userAddr) external view returns (uint256) {
        return tokenBalance[_userAddr].usdt;
    }

    // SGC
    function buySGC(uint256 _amount) external {
        uint256 buyPrice = _amount*sgcPrice/1000;
        IERC20(usdt).safeTransferFrom(msg.sender, address(this), buyPrice);
        IERC20(sgc).safeTransfer(msg.sender, _amount*1e18);
    }

    function setUserSGC(address _userAddr, uint256 _value) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        tokenBalance[_userAddr].sgc = _value;
    }
    
    function increaseUserSGC(address _receiver, uint256 _increaseAmount) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        tokenBalance[_receiver].sgc += _increaseAmount;
    }

    function decreaseUserSGC(address _spender, uint256 _decreaseAmount) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        require(tokenBalance[_spender].sgc >= _decreaseAmount, "User insufficient credit points.");
        tokenBalance[_spender].sgc -= _decreaseAmount;
    }

    function getUserSGC(address _userAddr) external view returns (uint256) {
        return tokenBalance[_userAddr].sgc;
    }

    // C-SGC
    function buyCredit(uint256 _amount) external {
        uint256 buyPrice = _amount*sgcPrice/1000;
        IERC20(usdt).safeTransferFrom(msg.sender, address(this), buyPrice);
        tokenBalance[msg.sender].credit += _amount;
    }

    function sellCredit(uint256 _amount) external {
        require(tokenBalance[msg.sender].credit >= _amount, "Insufficient credit to sell.");
        uint256 sellPrice = _amount*sgcPrice;
        tokenBalance[msg.sender].credit -= _amount;
        tokenBalance[msg.sender].usdt += sellPrice;
    }

    function transferCredit(address _receiver, uint256 _amount) external {
        require(tokenBalance[msg.sender].credit >= _amount, "Insufficient credit for transfer.");
        tokenBalance[msg.sender].credit -= _amount;
        tokenBalance[_receiver].credit += _amount;
    }

    function getUserCredit(address _userAddr) external view returns (uint256) {
        return tokenBalance[_userAddr].credit;
    }

    function increaseCredit(address _receiver, uint256 _increaseAmount) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        tokenBalance[_receiver].credit += _increaseAmount;
    }

    function decreaseCredit(address _spender, uint256 _decreaseAmount) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        require(tokenBalance[_spender].credit >= _decreaseAmount, "User insufficient credit points.");
        tokenBalance[_spender].credit -= _decreaseAmount;
    }

    function setUserCredit(address _userAddr, uint256 _value) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        tokenBalance[_userAddr].credit = _value;
    }

    // R-SGC
    function getUserReinvest(address _userAddr) external view returns (uint256) {
        return tokenBalance[_userAddr].reinvest;
    }

    function increaseReinvest(address _receiver, uint256 _increaseAmount) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        tokenBalance[_receiver].reinvest += _increaseAmount;
    }

    function decreaseReinvest(address _spender, uint256 _decreaseAmount) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        require(tokenBalance[_spender].reinvest >= _decreaseAmount, "User insufficient reinvest points.");
        tokenBalance[_spender].reinvest -= _decreaseAmount;
    }

    function setUserReinvest(address _userAddr, uint256 _value) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        tokenBalance[_userAddr].reinvest = _value;
    }

    // T-SGC
    function getUserTransfer(address _userAddr) external view returns(uint256) {
        return tokenBalance[_userAddr].transfer;
    }

    function increaseTransfer(address _receiver, uint256 _increaseAmount) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        tokenBalance[_receiver].transfer += _increaseAmount;
    }

    function decreaseTransfer(address _spender, uint256 _decreaseAmount) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        require(tokenBalance[_spender].transfer >= _decreaseAmount, "User insufficient transfer points.");
        tokenBalance[_spender].transfer -= _decreaseAmount;
    }

    function setUserTransfer(address _userAddr, uint256 _value) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        tokenBalance[_userAddr].transfer = _value;
    }

    function swapToCredit(uint256 _exchangeAmount) external {
        require(tokenBalance[msg.sender].transfer >= _exchangeAmount, "User insufficient transfer points.");
        tokenBalance[msg.sender].transfer -= _exchangeAmount;
        tokenBalance[msg.sender].credit += _exchangeAmount;
    }

    function swapToSGC(uint256 _exchangeAmount) external {
        require(tokenBalance[msg.sender].transfer >= _exchangeAmount, "User insufficient transfer points.");
        tokenBalance[msg.sender].transfer -= _exchangeAmount;

        uint256 dailyRelease = _exchangeAmount*10/100;
        uint256 today = getCurDay();
        for (uint256 i = 0; i < 10; i++) {
            payoutInfos[today+i].push(PayoutInfo(exchangeSGCTxn, msg.sender, dailyRelease, false));
        }
        exchangeSGCTxn++;
    }

    function distributeSGCPayout(uint256 _day) external {
        require(getCurDay() >= _day, "Invalid Day.");
        for (uint256 i = 0; i < payoutInfos[_day].length; i++) {
            if (payoutInfos[_day][i].completed == false) {
                address sgcReceiver = payoutInfos[_day][i].receiver;
                tokenBalance[sgcReceiver].sgc += payoutInfos[_day][i].amount;
                payoutInfos[_day][i].completed = true;
            }
        }
    }

    function getPayoutDetails(uint256 _day) external view returns(PayoutInfo[] memory) {
        return payoutInfos[_day];
    }

    function getTokenBalance(address wallet, address token) public view returns(uint256) {
        return IERC20(token).balanceOf(wallet);
    }

    function withdrawTokenToAddress(address withdrawTokenAddr, address receiver, uint256 amount) external onlyOwner {
        uint256 balance = IERC20(withdrawTokenAddr).balanceOf(address(this));
        require(balance >= amount, "Contract insufficient balance.");
        IERC20(withdrawTokenAddr).safeTransfer(receiver, amount);
    }

    // Investment Management
    function setUserTotalInvestment(address _userAddr, uint256 _value) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        investmentInfo[_userAddr].totalInvestment = _value;
    }

    function getUserTotalInvestment(address _userAddr) external view returns (uint256) {
        return investmentInfo[_userAddr].totalInvestment;
    }

    function setUserActiveInvestment(address _userAddr, uint256 _value) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        investmentInfo[_userAddr].activeInvestment = _value;
    }

    function getUserActiveInvestment(address _userAddr) external view returns (uint256) {
        return investmentInfo[_userAddr].activeInvestment;
    }

    function setUserMaxEarning(address _userAddr, uint256 _value) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        investmentInfo[_userAddr].maxEarning = _value;
    }

    function getUserMaxEarning(address _userAddr) external view returns (uint256) {
        return investmentInfo[_userAddr].maxEarning;
    }

    function setUserWithdrew(address _userAddr, uint256 _value) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        investmentInfo[_userAddr].withdrew = _value;
    }

    function getUserWithdrew(address _userAddr) external view returns (uint256) {
        return investmentInfo[_userAddr].withdrew;
    }

    function setUserLastClaim(address _userAddr, uint256 _value) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        investmentInfo[_userAddr].lastClaim = _value;
    }

    function getUserLastClaim(address _userAddr) external view returns (uint256) {
        return investmentInfo[_userAddr].lastClaim;
    }

    function setUserTotalSales(address _userAddr, uint256 _value) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        investmentInfo[_userAddr].totalSales = _value;
    }

    function getUserTotalSales(address _userAddr) external view returns (uint256) {
        return investmentInfo[_userAddr].totalSales;
    }

    function setPackage(uint256[] memory _newValues) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        package = _newValues;
    }

    function setInvestUSDTWeight(uint256 _newValue) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        investUSDTWeight = _newValue;
    }
    
    function setInvestCreditWeight(uint256 _newValue) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        investCreditWeight = _newValue;
    }

    function setReferDepth(uint256 _newValue) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        referDepth = _newValue;
    }

    function setDynamicBonus(uint256 _newValue) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        dynamicBonus = _newValue;
    }

    function setVIPBonus(uint256 _newValue) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        vipBonus = _newValue;
    }

    function setBonusDivider(uint256 _newValue) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        bonusDivider = _newValue;
    }

    function setDynamicBonus1(uint256 _newValue) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        dynamicBonus1 = _newValue;
    }

    function setDynamicBonus2(uint256 _newValue) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        dynamicBonus2 = _newValue;
    }

    function setBaseROI(uint256 _newValue) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        baseROI = _newValue;
    }

    function setDivider(uint256 _newValue) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        divider = _newValue;
    }

    function setMaxEarnMultiplier(uint256 _newValue) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        maxEarnMultiplier = _newValue;
    }

    function setTeamDataContract(address _contractAddr) external onlyOwner {
        teamDataContract = _contractAddr;
    }

    function setDev(address _Addr) external onlyOwner {
        dev = _Addr;
    }

    function manageAdmin(address _wallet, string memory _name, bool _state) external onlyOwner {
        admin[_wallet] = _state;
        adminName[_wallet] = _name;
    }

    function checkAdmin(address _wallet) external view returns (bool) {
        return admin[_wallet];
    }

    function getAdminName(address _wallet) external view returns (string memory) {
        return adminName[_wallet];
    }
    
    function setSGCPrice(uint256 _newValue) external {
        require(admin[msg.sender] || msg.sender == owner(), "Only admin or owner can execute.");
        sgcPrice = _newValue;
    }

    function getUserEarningByDay(address _userAddr, uint256 _day) external view returns (uint256) {
        return userDayEarning[_userAddr][_day];
    }

    function getSGCPrice() external view returns (uint256) {
        return sgcPrice;
    }

    function getCurDay() public view returns(uint256) {
        return (block.timestamp - startTime) / timeStep;
    }

    function withdrawSales() external onlyOwner {
        uint256 balance = IERC20(usdt).balanceOf(address(this));
        require(balance > 0, "Contract insufficient balance.");
        IERC20(usdt).safeTransfer(owner(), balance);
    }

}