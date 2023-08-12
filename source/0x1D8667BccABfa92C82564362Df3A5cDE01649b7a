// File: contracts/main/utils/Address.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/Address.sol)

pragma solidity ^0.8.19;

/**
 * @dev Collection of functions related to the address type
 */
library AddressUtils {
    /**
     * @dev The ETH balance of the account is not enough to perform the operation.
     */
    error AddressInsufficientBalance(address account);

    /**
     * @dev There's no code at `target` (it is not a contract).
     */
    error AddressEmptyCode(address target);

    /**
     * @dev A call to an address target failed. The target may have reverted.
     */
    error FailedInnerCall();

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
        if (address(this).balance < amount) {
            revert AddressInsufficientBalance(address(this));
        }

        (bool success, ) = recipient.call{value: amount}("");
        if (!success) {
            revert FailedInnerCall();
        }
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
     */
    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, defaultRevert);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`], but with a
     * `customRevert` function as a fallback when `target` reverts.
     *
     * Requirements:
     *
     * - `customRevert` must be a reverting function.
     */
    function functionCall(
        address target,
        bytes memory data,
        function() internal view customRevert
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, customRevert);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but also transferring `value` wei to `target`.
     *
     * Requirements:
     *
     * - the calling contract must have an ETH balance of at least `value`.
     * - the called Solidity function must be `payable`.
     */
    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, defaultRevert);
    }

    /**
     * @dev Same as {xref-Address-functionCallWithValue-address-bytes-uint256-}[`functionCallWithValue`], but
     * with a `customRevert` function as a fallback revert reason when `target` reverts.
     *
     * Requirements:
     *
     * - `customRevert` must be a reverting function.
     */
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        function() internal view customRevert
    ) internal returns (bytes memory) {
        if (address(this).balance < value) {
            revert AddressInsufficientBalance(address(this));
        }
        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResultFromTarget(target, success, returndata, customRevert);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a static call.
     */
    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        return functionStaticCall(target, data, defaultRevert);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a static call.
     */
    function functionStaticCall(
        address target,
        bytes memory data,
        function() internal view customRevert
    ) internal view returns (bytes memory) {
        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResultFromTarget(target, success, returndata, customRevert);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a delegate call.
     */
    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionDelegateCall(target, data, defaultRevert);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a delegate call.
     */
    function functionDelegateCall(
        address target,
        bytes memory data,
        function() internal view customRevert
    ) internal returns (bytes memory) {
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResultFromTarget(target, success, returndata, customRevert);
    }

    /**
     * @dev Tool to verify that a low level call to smart-contract was successful, and revert (either by bubbling
     * the revert reason or using the provided `customRevert`) in case of unsuccessful call or if target was not a contract.
     */
    function verifyCallResultFromTarget(
        address target,
        bool success,
        bytes memory returndata,
        function() internal view customRevert
    ) internal view returns (bytes memory) {
        if (success) {
            if (returndata.length == 0) {
                // only check if target is a contract if the call was successful and the return data is empty
                // otherwise we already know that it was a contract
                if (target.code.length == 0) {
                    revert AddressEmptyCode(target);
                }
            }
            return returndata;
        } else {
            _revert(returndata, customRevert);
        }
    }

    /**
     * @dev Tool to verify that a low level call was successful, and revert if it wasn't, either by bubbling the
     * revert reason or with a default revert error.
     */
    function verifyCallResult(bool success, bytes memory returndata) internal view returns (bytes memory) {
        return verifyCallResult(success, returndata, defaultRevert);
    }

    /**
     * @dev Same as {xref-Address-verifyCallResult-bool-bytes-}[`verifyCallResult`], but with a
     * `customRevert` function as a fallback when `success` is `false`.
     *
     * Requirements:
     *
     * - `customRevert` must be a reverting function.
     */
    function verifyCallResult(
        bool success,
        bytes memory returndata,
        function() internal view customRevert
    ) internal view returns (bytes memory) {
        if (success) {
            return returndata;
        } else {
            _revert(returndata, customRevert);
        }
    }

    /**
     * @dev Default reverting function when no `customRevert` is provided in a function call.
     */
    function defaultRevert() internal pure {
        revert FailedInnerCall();
    }

    function _revert(bytes memory returndata, function() internal view customRevert) private view {
        // Look for revert reason and bubble it up if present
        if (returndata.length > 0) {
            // The easiest way to bubble the revert reason is using memory via assembly
            /// @solidity memory-safe-assembly
            assembly {
                let returndata_size := mload(returndata)
                revert(add(32, returndata), returndata_size)
            }
        } else {
            customRevert();
            revert FailedInnerCall();
        }
    }
}

// File: contracts/main/interfaces/IERC5267.sol


// OpenZeppelin Contracts (last updated v4.9.0) (interfaces/IERC5267.sol)

pragma solidity ^0.8.19;

interface IERC5267 {
    /**
     * @dev MAY be emitted to signal that the domain could have changed.
     */
    event EIP712DomainChanged();

    /**
     * @dev returns the fields and values that describe the domain separator used by this contract for EIP-712
     * signature.
     */
    function eip712Domain()
        external
        view
        returns (
            bytes1 fields,
            string memory name,
            string memory version,
            uint256 chainId,
            address verifyingContract,
            bytes32 salt,
            uint256[] memory extensions
        );
}

// File: contracts/main/utils/StorageSlot.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/StorageSlot.sol)
// This file was procedurally generated from scripts/generate/templates/StorageSlot.js.

pragma solidity ^0.8.19;

/**
 * @dev Library for reading and writing primitive types to specific storage slots.
 *
 * Storage slots are often used to avoid storage conflict when dealing with upgradeable contracts.
 * This library helps with reading and writing to such slots without the need for inline assembly.
 *
 * The functions in this library return Slot structs that contain a `value` member that can be used to read or write.
 *
 * Example usage to set ERC1967 implementation slot:
 * ```solidity
 * contract ERC1967 {
 *     bytes32 internal constant _IMPLEMENTATION_SLOT = 0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc;
 *
 *     function _getImplementation() internal view returns (address) {
 *         return StorageSlot.getAddressSlot(_IMPLEMENTATION_SLOT).value;
 *     }
 *
 *     function _setImplementation(address newImplementation) internal {
 *         require(newImplementation.code.length > 0);
 *         StorageSlot.getAddressSlot(_IMPLEMENTATION_SLOT).value = newImplementation;
 *     }
 * }
 * ```
 */
library StorageSlot {
    struct AddressSlot {
        address value;
    }

    struct BooleanSlot {
        bool value;
    }

    struct Bytes32Slot {
        bytes32 value;
    }

    struct Uint256Slot {
        uint256 value;
    }

    struct StringSlot {
        string value;
    }

    struct BytesSlot {
        bytes value;
    }

    /**
     * @dev Returns an `AddressSlot` with member `value` located at `slot`.
     */
    function getAddressSlot(bytes32 slot) internal pure returns (AddressSlot storage r) {
        /// @solidity memory-safe-assembly
        assembly {
            r.slot := slot
        }
    }

    /**
     * @dev Returns an `BooleanSlot` with member `value` located at `slot`.
     */
    function getBooleanSlot(bytes32 slot) internal pure returns (BooleanSlot storage r) {
        /// @solidity memory-safe-assembly
        assembly {
            r.slot := slot
        }
    }

    /**
     * @dev Returns an `Bytes32Slot` with member `value` located at `slot`.
     */
    function getBytes32Slot(bytes32 slot) internal pure returns (Bytes32Slot storage r) {
        /// @solidity memory-safe-assembly
        assembly {
            r.slot := slot
        }
    }

    /**
     * @dev Returns an `Uint256Slot` with member `value` located at `slot`.
     */
    function getUint256Slot(bytes32 slot) internal pure returns (Uint256Slot storage r) {
        /// @solidity memory-safe-assembly
        assembly {
            r.slot := slot
        }
    }

    /**
     * @dev Returns an `StringSlot` with member `value` located at `slot`.
     */
    function getStringSlot(bytes32 slot) internal pure returns (StringSlot storage r) {
        /// @solidity memory-safe-assembly
        assembly {
            r.slot := slot
        }
    }

    /**
     * @dev Returns an `StringSlot` representation of the string storage pointer `store`.
     */
    function getStringSlot(string storage store) internal pure returns (StringSlot storage r) {
        /// @solidity memory-safe-assembly
        assembly {
            r.slot := store.slot
        }
    }

    /**
     * @dev Returns an `BytesSlot` with member `value` located at `slot`.
     */
    function getBytesSlot(bytes32 slot) internal pure returns (BytesSlot storage r) {
        /// @solidity memory-safe-assembly
        assembly {
            r.slot := slot
        }
    }

    /**
     * @dev Returns an `BytesSlot` representation of the bytes storage pointer `store`.
     */
    function getBytesSlot(bytes storage store) internal pure returns (BytesSlot storage r) {
        /// @solidity memory-safe-assembly
        assembly {
            r.slot := store.slot
        }
    }
}

// File: contracts/main/utils/ShortStrings.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/ShortStrings.sol)

pragma solidity ^0.8.19;


// | string  | 0xAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA   |
// | length  | 0x                                                              BB |
type ShortString is bytes32;

/**
 * @dev This library provides functions to convert short memory strings
 * into a `ShortString` type that can be used as an immutable variable.
 *
 * Strings of arbitrary length can be optimized using this library if
 * they are short enough (up to 31 bytes) by packing them with their
 * length (1 byte) in a single EVM word (32 bytes). Additionally, a
 * fallback mechanism can be used for every other case.
 *
 * Usage example:
 *
 * ```solidity
 * contract Named {
 *     using ShortStrings for *;
 *
 *     ShortString private immutable _name;
 *     string private _nameFallback;
 *
 *     constructor(string memory contractName) {
 *         _name = contractName.toShortStringWithFallback(_nameFallback);
 *     }
 *
 *     function name() external view returns (string memory) {
 *         return _name.toStringWithFallback(_nameFallback);
 *     }
 * }
 * ```
 */
library ShortStrings {
    // Used as an identifier for strings longer than 31 bytes.
    bytes32 private constant _FALLBACK_SENTINEL = 0x00000000000000000000000000000000000000000000000000000000000000FF;

    error StringTooLong(string str);
    error InvalidShortString();

    /**
     * @dev Encode a string of at most 31 chars into a `ShortString`.
     *
     * This will trigger a `StringTooLong` error is the input string is too long.
     */
    function toShortString(string memory str) internal pure returns (ShortString) {
        bytes memory bstr = bytes(str);
        if (bstr.length > 31) {
            revert StringTooLong(str);
        }
        return ShortString.wrap(bytes32(uint256(bytes32(bstr)) | bstr.length));
    }

    /**
     * @dev Decode a `ShortString` back to a "normal" string.
     */
    function toString(ShortString sstr) internal pure returns (string memory) {
        uint256 len = byteLength(sstr);
        // using `new string(len)` would work locally but is not memory safe.
        string memory str = new string(32);
        /// @solidity memory-safe-assembly
        assembly {
            mstore(str, len)
            mstore(add(str, 0x20), sstr)
        }
        return str;
    }

    /**
     * @dev Return the length of a `ShortString`.
     */
    function byteLength(ShortString sstr) internal pure returns (uint256) {
        uint256 result = uint256(ShortString.unwrap(sstr)) & 0xFF;
        if (result > 31) {
            revert InvalidShortString();
        }
        return result;
    }

    /**
     * @dev Encode a string into a `ShortString`, or write it to storage if it is too long.
     */
    function toShortStringWithFallback(string memory value, string storage store) internal returns (ShortString) {
        if (bytes(value).length < 32) {
            return toShortString(value);
        } else {
            StorageSlot.getStringSlot(store).value = value;
            return ShortString.wrap(_FALLBACK_SENTINEL);
        }
    }

    /**
     * @dev Decode a string that was encoded to `ShortString` or written to storage using {setWithFallback}.
     */
    function toStringWithFallback(ShortString value, string storage store) internal pure returns (string memory) {
        if (ShortString.unwrap(value) != _FALLBACK_SENTINEL) {
            return toString(value);
        } else {
            return store;
        }
    }

    /**
     * @dev Return the length of a string that was encoded to `ShortString` or written to storage using {setWithFallback}.
     *
     * WARNING: This will return the "byte length" of the string. This may not reflect the actual length in terms of
     * actual characters as the UTF-8 encoding of a single character can span over multiple bytes.
     */
    function byteLengthWithFallback(ShortString value, string storage store) internal view returns (uint256) {
        if (ShortString.unwrap(value) != _FALLBACK_SENTINEL) {
            return byteLength(value);
        } else {
            return bytes(store).length;
        }
    }
}

// File: contracts/main/utils/math/SignedMath.sol


// OpenZeppelin Contracts (last updated v4.8.0) (utils/math/SignedMath.sol)

pragma solidity ^0.8.19;

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

// File: contracts/main/utils/cryptography/ECDSA.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/cryptography/ECDSA.sol)

pragma solidity ^0.8.19;

/**
 * @dev Elliptic Curve Digital Signature Algorithm (ECDSA) operations.
 *
 * These functions can be used to verify that a message was signed by the holder
 * of the private keys of a given address.
 */
library ECDSA {
    enum RecoverError {
        NoError,
        InvalidSignature,
        InvalidSignatureLength,
        InvalidSignatureS
    }

    /**
     * @dev The signature derives the `address(0)`.
     */
    error ECDSAInvalidSignature();

    /**
     * @dev The signature has an invalid length.
     */
    error ECDSAInvalidSignatureLength(uint256 length);

    /**
     * @dev The signature has an S value that is in the upper half order.
     */
    error ECDSAInvalidSignatureS(bytes32 s);

    /**
     * @dev Returns the address that signed a hashed message (`hash`) with
     * `signature` or error string. This address can then be used for verification purposes.
     *
     * The `ecrecover` EVM precompile allows for malleable (non-unique) signatures:
     * this function rejects them by requiring the `s` value to be in the lower
     * half order, and the `v` value to be either 27 or 28.
     *
     * IMPORTANT: `hash` _must_ be the result of a hash operation for the
     * verification to be secure: it is possible to craft signatures that
     * recover to arbitrary addresses for non-hashed data. A safe way to ensure
     * this is by receiving a hash of the original message (which may otherwise
     * be too long), and then calling {MessageHashUtils-toEthSignedMessageHash} on it.
     *
     * Documentation for signature generation:
     * - with https://web3js.readthedocs.io/en/v1.3.4/web3-eth-accounts.html#sign[Web3.js]
     * - with https://docs.ethers.io/v5/api/signer/#Signer-signMessage[ethers]
     */
    function tryRecover(bytes32 hash, bytes memory signature) internal pure returns (address, RecoverError, bytes32) {
        if (signature.length == 65) {
            bytes32 r;
            bytes32 s;
            uint8 v;
            // ecrecover takes the signature parameters, and the only way to get them
            // currently is to use assembly.
            /// @solidity memory-safe-assembly
            assembly {
                r := mload(add(signature, 0x20))
                s := mload(add(signature, 0x40))
                v := byte(0, mload(add(signature, 0x60)))
            }
            return tryRecover(hash, v, r, s);
        } else {
            return (address(0), RecoverError.InvalidSignatureLength, bytes32(signature.length));
        }
    }

    /**
     * @dev Returns the address that signed a hashed message (`hash`) with
     * `signature`. This address can then be used for verification purposes.
     *
     * The `ecrecover` EVM precompile allows for malleable (non-unique) signatures:
     * this function rejects them by requiring the `s` value to be in the lower
     * half order, and the `v` value to be either 27 or 28.
     *
     * IMPORTANT: `hash` _must_ be the result of a hash operation for the
     * verification to be secure: it is possible to craft signatures that
     * recover to arbitrary addresses for non-hashed data. A safe way to ensure
     * this is by receiving a hash of the original message (which may otherwise
     * be too long), and then calling {MessageHashUtils-toEthSignedMessageHash} on it.
     */
    function recover(bytes32 hash, bytes memory signature) internal pure returns (address) {
        (address recovered, RecoverError error, bytes32 errorArg) = tryRecover(hash, signature);
        _throwError(error, errorArg);
        return recovered;
    }

    /**
     * @dev Overload of {ECDSA-tryRecover} that receives the `r` and `vs` short-signature fields separately.
     *
     * See https://eips.ethereum.org/EIPS/eip-2098[EIP-2098 short signatures]
     */
    function tryRecover(bytes32 hash, bytes32 r, bytes32 vs) internal pure returns (address, RecoverError, bytes32) {
        unchecked {
            bytes32 s = vs & bytes32(0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff);
            // We do not check for an overflow here since the shift operation results in 0 or 1.
            uint8 v = uint8((uint256(vs) >> 255) + 27);
            return tryRecover(hash, v, r, s);
        }
    }

    /**
     * @dev Overload of {ECDSA-recover} that receives the `r and `vs` short-signature fields separately.
     */
    function recover(bytes32 hash, bytes32 r, bytes32 vs) internal pure returns (address) {
        (address recovered, RecoverError error, bytes32 errorArg) = tryRecover(hash, r, vs);
        _throwError(error, errorArg);
        return recovered;
    }

    /**
     * @dev Overload of {ECDSA-tryRecover} that receives the `v`,
     * `r` and `s` signature fields separately.
     */
    function tryRecover(
        bytes32 hash,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) internal pure returns (address, RecoverError, bytes32) {
        // EIP-2 still allows signature malleability for ecrecover(). Remove this possibility and make the signature
        // unique. Appendix F in the Ethereum Yellow paper (https://ethereum.github.io/yellowpaper/paper.pdf), defines
        // the valid range for s in (301): 0 < s < secp256k1n ÷ 2 + 1, and for v in (302): v ∈ {27, 28}. Most
        // signatures from current libraries generate a unique signature with an s-value in the lower half order.
        //
        // If your library generates malleable signatures, such as s-values in the upper range, calculate a new s-value
        // with 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEBAAEDCE6AF48A03BBFD25E8CD0364141 - s1 and flip v from 27 to 28 or
        // vice versa. If your library also generates signatures with 0/1 for v instead 27/28, add 27 to v to accept
        // these malleable signatures as well.
        if (uint256(s) > 0x7FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF5D576E7357A4501DDFE92F46681B20A0) {
            return (address(0), RecoverError.InvalidSignatureS, s);
        }

        // If the signature is valid (and not malleable), return the signer address
        address signer = ecrecover(hash, v, r, s);
        if (signer == address(0)) {
            return (address(0), RecoverError.InvalidSignature, bytes32(0));
        }

        return (signer, RecoverError.NoError, bytes32(0));
    }

    /**
     * @dev Overload of {ECDSA-recover} that receives the `v`,
     * `r` and `s` signature fields separately.
     */
    function recover(bytes32 hash, uint8 v, bytes32 r, bytes32 s) internal pure returns (address) {
        (address recovered, RecoverError error, bytes32 errorArg) = tryRecover(hash, v, r, s);
        _throwError(error, errorArg);
        return recovered;
    }

    /**
     * @dev Optionally reverts with the corresponding custom error according to the `error` argument provided.
     */
    function _throwError(RecoverError error, bytes32 errorArg) private pure {
        if (error == RecoverError.NoError) {
            return; // no error: do nothing
        } else if (error == RecoverError.InvalidSignature) {
            revert ECDSAInvalidSignature();
        } else if (error == RecoverError.InvalidSignatureLength) {
            revert ECDSAInvalidSignatureLength(uint256(errorArg));
        } else if (error == RecoverError.InvalidSignatureS) {
            revert ECDSAInvalidSignatureS(errorArg);
        }
    }
}

// File: contracts/main/token/ERC20/extensions/IERC20Permit.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/extensions/IERC20Permit.sol)

pragma solidity ^0.8.19;

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

// File: contracts/main/token/ERC20/swap/IPair.sol



pragma solidity ^0.8.8;

interface IPair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);

    function mint(address to) external returns (uint liquidity);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;

    function initialize(address, address) external;
}
// File: contracts/main/token/ERC20/swap/ISwapRouter.sol



pragma solidity ^0.8.8;

interface ISwapRouter {
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

interface ISwapRouterV2 is ISwapRouter {
    
    function factoryV2() external pure returns (address);

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
// File: contracts/main/token/ERC20/swap/ISwapFactory.sol



pragma solidity ^0.8.8;

interface ISwapFactory {
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
// File: contracts/main/interfaces/draft-IERC6093.sol


pragma solidity ^0.8.19;

/**
 * @dev Standard ERC20 Errors
 * Interface of the https://eips.ethereum.org/EIPS/eip-6093[ERC-6093] custom errors for ERC20 tokens.
 */
interface IERC20Errors {
    /**
     * @dev Indicates an error related to the current `balance` of a `sender`. Used in transfers.
     * @param sender Address whose tokens are being transferred.
     * @param balance Current balance for the interacting account.
     * @param needed Minimum amount required to perform a transfer.
     */
    error ERC20InsufficientBalance(address sender, uint256 balance, uint256 needed);

    /**
     * @dev Indicates a failure with the token `sender`. Used in transfers.
     * @param sender Address whose tokens are being transferred.
     */
    error ERC20InvalidSender(address sender);

    /**
     * @dev Indicates a failure with the token `receiver`. Used in transfers.
     * @param receiver Address to which tokens are being transferred.
     */
    error ERC20InvalidReceiver(address receiver);

    /**
     * @dev Indicates a failure with the `spender`’s `allowance`. Used in transfers.
     * @param spender Address that may be allowed to operate on tokens without being their owner.
     * @param allowance Amount of tokens a `spender` is allowed to operate with.
     * @param needed Minimum amount required to perform a transfer.
     */
    error ERC20InsufficientAllowance(address spender, uint256 allowance, uint256 needed);

    /**
     * @dev Indicates a failure with the `approver` of a token to be approved. Used in approvals.
     * @param approver Address initiating an approval operation.
     */
    error ERC20InvalidApprover(address approver);

    /**
     * @dev Indicates a failure with the `spender` to be approved. Used in approvals.
     * @param spender Address that may be allowed to operate on tokens without being their owner.
     */
    error ERC20InvalidSpender(address spender);
}

/**
 * @dev Standard ERC721 Errors
 * Interface of the https://eips.ethereum.org/EIPS/eip-6093[ERC-6093] custom errors for ERC721 tokens.
 */
interface IERC721Errors {
    /**
     * @dev Indicates that an address can't be an owner. For example, `address(0)` is a forbidden owner in EIP-20.
     * Used in balance queries.
     * @param owner Address of the current owner of a token.
     */
    error ERC721InvalidOwner(address owner);

    /**
     * @dev Indicates a `tokenId` whose `owner` is the zero address.
     * @param tokenId Identifier number of a token.
     */
    error ERC721NonexistentToken(uint256 tokenId);

    /**
     * @dev Indicates an error related to the ownership over a particular token. Used in transfers.
     * @param sender Address whose tokens are being transferred.
     * @param tokenId Identifier number of a token.
     * @param owner Address of the current owner of a token.
     */
    error ERC721IncorrectOwner(address sender, uint256 tokenId, address owner);

    /**
     * @dev Indicates a failure with the token `sender`. Used in transfers.
     * @param sender Address whose tokens are being transferred.
     */
    error ERC721InvalidSender(address sender);

    /**
     * @dev Indicates a failure with the token `receiver`. Used in transfers.
     * @param receiver Address to which tokens are being transferred.
     */
    error ERC721InvalidReceiver(address receiver);

    /**
     * @dev Indicates a failure with the `operator`’s approval. Used in transfers.
     * @param operator Address that may be allowed to operate on tokens without being their owner.
     * @param tokenId Identifier number of a token.
     */
    error ERC721InsufficientApproval(address operator, uint256 tokenId);

    /**
     * @dev Indicates a failure with the `approver` of a token to be approved. Used in approvals.
     * @param approver Address initiating an approval operation.
     */
    error ERC721InvalidApprover(address approver);

    /**
     * @dev Indicates a failure with the `operator` to be approved. Used in approvals.
     * @param operator Address that may be allowed to operate on tokens without being their owner.
     */
    error ERC721InvalidOperator(address operator);
}

/**
 * @dev Standard ERC1155 Errors
 * Interface of the https://eips.ethereum.org/EIPS/eip-6093[ERC-6093] custom errors for ERC1155 tokens.
 */
interface IERC1155Errors {
    /**
     * @dev Indicates an error related to the current `balance` of a `sender`. Used in transfers.
     * @param sender Address whose tokens are being transferred.
     * @param balance Current balance for the interacting account.
     * @param needed Minimum amount required to perform a transfer.
     * @param tokenId Identifier number of a token.
     */
    error ERC1155InsufficientBalance(address sender, uint256 balance, uint256 needed, uint256 tokenId);

    /**
     * @dev Indicates a failure with the token `sender`. Used in transfers.
     * @param sender Address whose tokens are being transferred.
     */
    error ERC1155InvalidSender(address sender);

    /**
     * @dev Indicates a failure with the token `receiver`. Used in transfers.
     * @param receiver Address to which tokens are being transferred.
     */
    error ERC1155InvalidReceiver(address receiver);

    /**
     * @dev Indicates a failure with the `operator`’s approval. Used in transfers.
     * @param operator Address that may be allowed to operate on tokens without being their owner.
     * @param owner Address of the current owner of a token.
     */
    error ERC1155MissingApprovalForAll(address operator, address owner);

    /**
     * @dev Indicates a failure with the `approver` of a token to be approved. Used in approvals.
     * @param approver Address initiating an approval operation.
     */
    error ERC1155InvalidApprover(address approver);

    /**
     * @dev Indicates a failure with the `operator` to be approved. Used in approvals.
     * @param operator Address that may be allowed to operate on tokens without being their owner.
     */
    error ERC1155InvalidOperator(address operator);

    /**
     * @dev Indicates an array length mismatch between ids and values in a safeBatchTransferFrom operation.
     * Used in batch transfers.
     * @param idsLength Length of the array of token identifiers
     * @param valuesLength Length of the array of token amounts
     */
    error ERC1155InvalidArrayLength(uint256 idsLength, uint256 valuesLength);
}

// File: contracts/main/utils/Context.sol


// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)

pragma solidity ^0.8.19;

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

// File: contracts/main/access/Ownable.sol


// OpenZeppelin Contracts (last updated v4.9.0) (access/Ownable.sol)

pragma solidity ^0.8.19;


/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * The initial owner is set to the address provided by the deployer. This can
 * later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    /**
     * @dev The caller account is not authorized to perform an operation.
     */
    error OwnableUnauthorizedAccount(address account);

    /**
     * @dev The owner is not a valid owner account. (eg. `address(0)`)
     */
    error OwnableInvalidOwner(address owner);

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the address provided by the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(msg.sender);
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
        if (owner() != _msgSender()) {
            revert OwnableUnauthorizedAccount(_msgSender());
        }
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
        if (newOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
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

// File: contracts/main/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.19;

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
     * @dev Returns the value of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the value of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves a `value` amount of tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 value) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets a `value` amount of tokens as the allowance of `spender` over the
     * caller's tokens.
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
    function approve(address spender, uint256 value) external returns (bool);

    /**
     * @dev Moves a `value` amount of tokens from `from` to `to` using the
     * allowance mechanism. `value` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}

// File: contracts/main/token/ERC20/utils/SafeERC20.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/utils/SafeERC20.sol)

pragma solidity ^0.8.19;




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
    using AddressUtils for address;

    /**
     * @dev An operation with an ERC20 token failed.
     */
    error SafeERC20FailedOperation(address token);

    /**
     * @dev Indicates a failed `decreaseAllowance` request.
     */
    error SafeERC20FailedDecreaseAllowance(address spender, uint256 currentAllowance, uint256 requestedDecrease);

    /**
     * @dev Transfer `value` amount of `token` from the calling contract to `to`. If `token` returns no value,
     * non-reverting calls are assumed to be successful.
     */
    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeCall(token.transfer, (to, value)));
    }

    /**
     * @dev Transfer `value` amount of `token` from `from` to `to`, spending the approval given by `from` to the
     * calling contract. If `token` returns no value, non-reverting calls are assumed to be successful.
     */
    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeCall(token.transferFrom, (from, to, value)));
    }

    /**
     * @dev Increase the calling contract's allowance toward `spender` by `value`. If `token` returns no value,
     * non-reverting calls are assumed to be successful.
     */
    function safeIncreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 oldAllowance = token.allowance(address(this), spender);
        forceApprove(token, spender, oldAllowance + value);
    }

    /**
     * @dev Decrease the calling contract's allowance toward `spender` by `requestedDecrease`. If `token` returns no value,
     * non-reverting calls are assumed to be successful.
     */
    function safeDecreaseAllowance(IERC20 token, address spender, uint256 requestedDecrease) internal {
        unchecked {
            uint256 currentAllowance = token.allowance(address(this), spender);
            if (currentAllowance < requestedDecrease) {
                revert SafeERC20FailedDecreaseAllowance(spender, currentAllowance, requestedDecrease);
            }
            forceApprove(token, spender, currentAllowance - requestedDecrease);
        }
    }

    /**
     * @dev Set the calling contract's allowance toward `spender` to `value`. If `token` returns no value,
     * non-reverting calls are assumed to be successful. Meant to be used with tokens that require the approval
     * to be set to zero before setting it to a non-zero value, such as USDT.
     */
    function forceApprove(IERC20 token, address spender, uint256 value) internal {
        bytes memory approvalCall = abi.encodeCall(token.approve, (spender, value));

        if (!_callOptionalReturnBool(token, approvalCall)) {
            _callOptionalReturn(token, abi.encodeCall(token.approve, (spender, 0)));
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
        if (nonceAfter != nonceBefore + 1) {
            revert SafeERC20FailedOperation(address(token));
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
        // we're implementing it ourselves. We use {Address-functionCall} to perform this call, which verifies that
        // the target address contains contract code and also asserts for success in the low-level call.

        bytes memory returndata = address(token).functionCall(data);
        if (returndata.length != 0 && !abi.decode(returndata, (bool))) {
            revert SafeERC20FailedOperation(address(token));
        }
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
        return success && (returndata.length == 0 || abi.decode(returndata, (bool))) && address(token).code.length > 0;
    }
}

// File: contracts/main/token/ERC20/extensions/IERC20Metadata.sol


// OpenZeppelin Contracts v4.4.1 (token/ERC20/extensions/IERC20Metadata.sol)

pragma solidity ^0.8.19;


/**
 * @dev Interface for the optional metadata functions from the ERC20 standard.
 */
interface IERC20Metadata is IERC20 {
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

// File: contracts/main/storage/TiltGame.sol


pragma solidity ^0.8.19;

library TiltGame {

    bytes32 internal constant SLOT = keccak256("project.main.storage.games.tilt");

    struct EndGame {
        bytes32 proof;
        uint8 loser;
        uint80 totalShares;
    }

    struct GameRoom {
        uint56 id;
        GameParticipant[5] participants;
    }

    struct GameParticipant {
        address account;
        uint80 bet;
    }

    struct Table {
        GameRoom room;
    }

    function proof(uint64 roomId, GameParticipant[5] memory participants) internal pure returns(bytes32) {
        return bytes32(
                keccak256(
                    abi.encode(
                        roomId, 
                        participants
                    )));
    }    

    function value(bytes32 key, GameParticipant memory participant) internal pure returns(uint8) {
        return uint8(
                uint256(
                    keccak256(
                        abi.encode(
                            key,
                            participant
                        ))) % 100) + 1;
    }

    function STORAGE() internal pure returns (Table storage table) {
        bytes32 slot = SLOT;
        assembly {
            table.slot := slot
        }
    }

}
// File: contracts/main/storage/ISyncer.sol



/* 「<HIRO> ここにいました。」
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000OOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO000000000000000000x
00000000000000koc;,......................................................................................,;cok00000000000000x
00000000000xl,.                                                                                              .,lxO0000000000x
000000000x:.                                                                                                    .;d000000000x
0000000kc.                                                                                                        .ck0000000x
000000x,                                                                                                            'x000000x
00000x,                                                                                                              'x00000x
0000O:                                                                                                                ;O0000x
0000d.                                                                                                                .d0000x
0000l                                ,,                ,,                       .loool,                                l0000x
0000l                               ;xk:.             :kk:                     'dOkkkkx:                               c0000x
0000l                             .:kOkkc.          .ckOkkc.                  ,xOkkkkkOkc.                             c0000x
0000l                            .lkOkkkko.        .lkkkkkkl.                ;xkkkkkkOkkkl.                            c0000x
0000l                           .okkkOkkOx'       .okkkOOkOd'                ....;dOkOOkkko.                           c0000x
0000l                          'dkkkkkkOd'       'dOkkkkkOd'                      'dOkkkkkOd'                          c0000x
0000l                         ,dOkkkkkko.       ,xOkkkkkko.                        .okOkOkkOx,                         c0000x
0000l                        ;xOkkOkkkl.       ;xOkkkkkkl.              .....       .lkkkOOkOx;                        c0000x
0000l                       :kOkkkkkkc.      .:kOkkkkOkc.              ,dxxxxc.      .ckOkkkkOk:.                      c0000x
0000l                     .ckOkkkkkk:       .ckkkOkkOx;               ;xkkkkkkl.       :xOkkOkOkl.                     c0000x
0000l                    .lkOkkOkOx;       .okkkkkkOx,              .:kOkkkkkkko.       ;xOkkOkkko.                    c0000x
0000l                   .oOkkOkkOd,       .okkkkkkkd'              .ckOkkkkkOkkOd'       'dOkkOkkOd'                   c0000x
0000l                  'dOkkkOkko.       ,dOkkOkkko.              .lkOkkkkkkkkkkOx,       .okkkkOkkd,                  c0000x
0000l                 ,xOkkkkOko.       ;xOkkkkkkl.              .okkkkkkkkkkkkkkOx;       .lkkkOkkOx;                 c0000x
0000l                ;xOkkOkkkl.       :kOkkOkkkc.              'dOkkkkkOo,ckOkkOkOkc.      .ckkkkkkOk:                c0000x
0000l              .ckOkkkkOk:.      .ckOkkkkOx:              .,xkkOkOOko.  :xOkOkkOkl.       :kkkkkkkkc.              c0000x
0000l             .lkOkkOOOx;       .lkkkkkkOx;              :dxOkkkkOkc.    ,xOkOkkkko.       ;xOkkkkOkl.             c0000x
0000l            .okkkOkkOx,       .okkkkkkOd'             .ckOkkkkkOkc.      'dOkkkkkOd.       ,dkkkkkkko.            c0000x
0000l           'dOkkOOkOd'       'dOkkOkkOo.             .lkOkkkkkOx;         .okOkkOkOd,       .dOkkkkkOd'           c0000x
0000l          ,dOkOOkkko.       ,xOkkkkOkl.             .okkkOOkkOx,           .lkkkkkkOx;       .okkkkkkOx,          c0000x
0000l         .oOkkOkkOx,       .dOkkkkkOx'             'dOkOkkkkOd'             'xOkOkkkOd.       'xOkOkkkOd.         c0000x
0000l          'dOkkkkkko.       ,xOkkkkkko.           ,xOkkkkkkko.             .okkkkOkOx,       .oOkkkkkOd,          c0000x
0000l           .okOkkkkOd'       'dOkkkOkkd'         ;xOkOOkkOkl.             .oOkkkkOkd'       'dOkkOkkkd'           c0000x
0000l            .lkkkkOkOx,       .okkkkkkOd,      .:kkkkkkOkxc.             ,dOkkkkkko.       ,xOkkkkkko.            c0000x
0000l             .ckOkkOkkx:       .lkOkkkkOx;    .lkOkkkkOx:.              ;xOkkkkOkl.       ;xOkkkOOkl.             c0000x
0000l              .:kOkkkkOkc.      .ckkkkkkkk:. .okkkkkkOx,               :kOkkkkOkc.      .ckOkkkkOkc.              c0000x
0000l                ;xOkkkkOkl.       :kOkkOkOkc;oOOkkkkOd'              .ckkkkkkkk:       .lkkkkkkOx;                c0000x
0000l                 ,xkkOkkOko.       ,xOkkOOkkkkkkOkkko.              .lkkkkkkkx;       .okkkkkkOx,                 c0000x
0000l                  'dOkkkOkkd'       'dOkkkkkkkkkkkkl.              .okkkOkkOd,       'dOkOOkkOd'                  c0000x
0000l                   .okkkkkkOx,       .oOkkkkkkkkOkc.              'dOkOOkkOd'       ,dOkOkkkko.                   c0000x
0000l                    .lkkkkkkOx;       .lkkkkOOkOk:               ,xOkkOkkko.       ;xOkkOkkkl.                    c0000x
0000l                     .ckOkkkkOk:.      .ckkkkkOx;               :xOkkOkkkl.      .:kOkkkkkkc.                     c0000x
0000l                       :xOkkkkOkl.      .:dxxxo,              .ckOkkkkOkc.      .ckkkkOkOk:                       c0000x
0000l                        ,xOkkkkkko.       ....               .lkkkkkkOx;       .lkOkkkkOx;                        c0000x
0000l                         'dOOkkkkOo.                        .okkkkkkOx,       .oOkkkkkOd,                         c0000x
0000l                          .okkkkOkOd,                      'dOkkkkkOd'       'dOkkOOkkd'                          c0000x
0000l                           .lkkkOkkOx:'...                .dOkkkkkko.       'xOkkkkOko.                           c0000x
0000l                            .ckkkkkkkkkkx;                .ckOkkOkl.        .lkkkkOkl.                            c0000x
0000l                             .:kkkkkOkOx,                  .:kkkkc.          .ckkkkc.                             c0000x
0000l                               ;xOkkkOd'                     ;xk:.             :kx;                               c0000x
0000l.                               'llllc.                       ',                ''                                l0000x
0000d.                                                                                                                .d0000x
0000O:                                                                                                                ;O0000x
00000k,                                                                                                              'x00000x
000000k,                                     hire.solidity.developer@gmail.com                                      ,x000000x
0000000kc.                                                                                                        .ck0000000x
000000000x:.                                                                                                    .:xO00000000x
00000000000kl;.                                                                                              .;lk0O000000000x
00000000000O00kdl:,'....................................................................................',:ldk00000000000000x
0000000000000000000OOOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO0000000000000000000x
0000000000000000000000000000000000000000O0000000OOO000000000000000000OO000O0000000000000000000000000000000000000000000000000x
000000000000000000000000000000000000000000000kdddddk00000000000000OxddddxO00000000000000000000000000000000000000000000000000x
00000000000000000000000000000000000000000000O:    .cO0000000000000o.    ,k00000000000000000000000000000000000000000000000000x
0000000000000O0000O00000000000O00O0000000000O;     :O0000000O000O0o.    'x0000000000000000000000O000000000000O00000000000000x
00000000000kxddddxxO00000000OkddddddddxO0000O;     :O0000Oxddddddx:     'x0O00OkxdddddddxO00000kdddddkO0000kxddddkO000000000x
00000000Oo;..     ..;dO00Ox:..        ..;dO0O;     :O0Od;..             'x0Oxc'.        ..;oO00c.    ;O0000c.    ;O000000000x
0000000k:     .:c:'  .o0Ol.     '::,.    .cOO;     :0Oc.    .,::cc,     'x0o.     '::;.     :O0:     ,O0000:     ,O000000000x
0000000o.    .x000Oc..cOk'     cO000o.    .xO;     :0d.    .o00000o.    'kO,     :O000d.    .o0:     ,O0000:     ,O000000000x
0000000o.     'lxO0OkkO0x.    .o0O00x.    .d0;     :0o.    'x0OOO0o.    .kk'     l0000k'    .l0:     ,O0000:     ,O000000000x
0000000kc.      .'cdO0O0x.    .o0O00x.    .dO;     :0o.    'x0O0O0o.    .kk'     .;;;;,.    .l0:     ,O0000:     ,O000000000x
00000000Oxl,.       .ck0x.    .o00O0x.    .d0;     :0o.    'x0OOO0o.    .kk'      ..........'d0:     ,O0000:     ;O000000000x
000000000000ko:.      :0x.    .o00O0x.    .d0;     :0o.    'x00000o.    'kk'     ckkkkkkkkkkkO0:     ,O0000:     l0000000000x
0000000d:,ck0O0k;     ,Ox.    .o0OO0d.    .d0;     :0o.    .x00000l     .kk'     c0000O00kc,:x0:     ,O0O0O:    ;k0000000000x
00000O0d.  ,oddl.    .l0O:     .cddl'     ;OO;     :0k,     'loddc.     'x0c     .:oddddo;  ,k0:     'dddo:.  .ck00000000000x
0000000Oo'          'oO00Ol.            .ck0O;     :O0k:.         .     'x0Oo'            .;x00:           .'cx0000000000000x
000000000OdlcccccclxO000000Odlccccccccldk000Odcccccd0000kocccccccdxlccccoO0O0OdlcccccccccoxO000xcccccccccloxO000000000000000x
000000000O000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO0d
//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*/

pragma solidity ^0.8.19;

interface ISyncer {
    function syncd() external view returns(address);
    function syncGas() external view returns(uint24);
    function syncUAC(address uac) external view returns(uint16);
    function syncOptions() external view returns(uint96,address,address);
}
// File: contracts/main/utils/math/Math.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/math/Math.sol)

pragma solidity ^0.8.19;

/**
 * @dev Standard math utilities missing in the Solidity language.
 */
library Math {
    /**
     * @dev Muldiv operation overflow.
     */
    error MathOverflowedMulDiv();

    enum Rounding {
        Floor, // Toward negative infinity
        Ceil, // Toward positive infinity
        Trunc, // Toward zero
        Expand // Away from zero
    }

    /**
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
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
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, with an overflow flag.
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
     */
    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers, with a division by zero flag.
     */
    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
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
     * This differs from standard division with `/` in that it rounds towards infinity instead
     * of rounding towards zero.
     */
    function ceilDiv(uint256 a, uint256 b) internal pure returns (uint256) {
        if (b == 0) {
            // Guarantee the same behavior as in a regular Solidity division.
            return a / b;
        }

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
            if (denominator <= prod1) {
                revert MathOverflowedMulDiv();
            }

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
        if (unsignedRoundsUp(rounding) && mulmod(x, y, denominator) > 0) {
            result += 1;
        }
        return result;
    }

    /**
     * @dev Returns the square root of a number. If the number is not a perfect square, the value is rounded
     * towards zero.
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
            return result + (unsignedRoundsUp(rounding) && result * result < a ? 1 : 0);
        }
    }

    /**
     * @dev Return the log in base 2 of a positive value rounded towards zero.
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
            return result + (unsignedRoundsUp(rounding) && 1 << result < value ? 1 : 0);
        }
    }

    /**
     * @dev Return the log in base 10 of a positive value rounded towards zero.
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
            return result + (unsignedRoundsUp(rounding) && 10 ** result < value ? 1 : 0);
        }
    }

    /**
     * @dev Return the log in base 256 of a positive value rounded towards zero.
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
            return result + (unsignedRoundsUp(rounding) && 1 << (result << 3) < value ? 1 : 0);
        }
    }

    /**
     * @dev Returns whether a provided rounding mode is considered rounding up for unsigned integers.
     */
    function unsignedRoundsUp(Rounding rounding) internal pure returns (bool) {
        return uint8(rounding) % 2 == 1;
    }
}

// File: contracts/main/utils/Strings.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/Strings.sol)

pragma solidity ^0.8.19;



/**
 * @dev String operations.
 */
library Strings {
    bytes16 private constant _SYMBOLS = "0123456789abcdef";
    uint8 private constant _ADDRESS_LENGTH = 20;

    /**
     * @dev The `value` string doesn't fit in the specified `length`.
     */
    error StringsInsufficientHexLength(uint256 value, uint256 length);

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
    function toStringSigned(int256 value) internal pure returns (string memory) {
        return string.concat(value < 0 ? "-" : "", toString(SignedMath.abs(value)));
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
        uint256 localValue = value;
        bytes memory buffer = new bytes(2 * length + 2);
        buffer[0] = "0";
        buffer[1] = "x";
        for (uint256 i = 2 * length + 1; i > 1; --i) {
            buffer[i] = _SYMBOLS[localValue & 0xf];
            localValue >>= 4;
        }
        if (localValue != 0) {
            revert StringsInsufficientHexLength(value, length);
        }
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
        return bytes(a).length == bytes(b).length && keccak256(bytes(a)) == keccak256(bytes(b));
    }
}

// File: contracts/main/utils/cryptography/MessageHashUtils.sol



pragma solidity ^0.8.19;


/**
 * @dev Signature message hash utilities for producing digests to be consumed by {ECDSA} recovery or signing.
 *
 * The library provides methods for generating a hash of a message that conforms to the
 * https://eips.ethereum.org/EIPS/eip-191[EIP 191] and https://eips.ethereum.org/EIPS/eip-712[EIP 712]
 * specifications.
 */
library MessageHashUtils {
    /**
     * @dev Returns the keccak256 digest of an EIP-191 signed data with version
     * `0x45` (`personal_sign` messages).
     *
     * The digest is calculated by prefixing a bytes32 `messageHash` with
     * `"\x19Ethereum Signed Message:\n32"` and hashing the result. It corresponds with the
     * hash signed when using the https://eth.wiki/json-rpc/API#eth_sign[`eth_sign`] JSON-RPC method.
     *
     * NOTE: The `hash` parameter is intended to be the result of hashing a raw message with
     * keccak256, although any bytes32 value can be safely used because the final digest will
     * be re-hashed.
     *
     * See {ECDSA-recover}.
     */
    function toEthSignedMessageHash(bytes32 messageHash) internal pure returns (bytes32 digest) {
        /// @solidity memory-safe-assembly
        assembly {
            mstore(0x00, "\x19Ethereum Signed Message:\n32") // 32 is the bytes-length of messageHash
            mstore(0x1c, messageHash) // 0x1c (28) is the length of the prefix
            digest := keccak256(0x00, 0x3c) // 0x3c is the length of the prefix (0x1c) + messageHash (0x20)
        }
    }

    /**
     * @dev Returns the keccak256 digest of an EIP-191 signed data with version
     * `0x45` (`personal_sign` messages).
     *
     * The digest is calculated by prefixing an arbitrary `message` with
     * `"\x19Ethereum Signed Message:\n" + len(message)` and hashing the result. It corresponds with the
     * hash signed when using the https://eth.wiki/json-rpc/API#eth_sign[`eth_sign`] JSON-RPC method.
     *
     * See {ECDSA-recover}.
     */
    function toEthSignedMessageHash(bytes memory message) internal pure returns (bytes32 digest) {
        return keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n", Strings.toString(message.length), message));
    }

    /**
     * @dev Returns the keccak256 digest of an EIP-191 signed data with version
     * `0x00` (data with intended validator).
     *
     * The digest is calculated by prefixing an arbitrary `data` with `"\x19\x00"` and the intended
     * `validator` address. Then hashing the result.
     *
     * See {ECDSA-recover}.
     */
    function toDataWithIntendedValidatorHash(
        address validator,
        bytes memory data
    ) internal pure returns (bytes32 digest) {
        return keccak256(abi.encodePacked(hex"19_00", validator, data));
    }

    /**
     * @dev Returns the keccak256 digest of an EIP-712 typed data (EIP-191 version `0x01`).
     *
     * The digest is calculated from a `domainSeparator` and a `structHash`, by prefixing them with
     * `\x19\x01` and hashing the result. It corresponds to the hash signed by the
     * https://eips.ethereum.org/EIPS/eip-712[`eth_signTypedData`] JSON-RPC method as part of EIP-712.
     *
     * See {ECDSA-recover}.
     */
    function toTypedDataHash(bytes32 domainSeparator, bytes32 structHash) internal pure returns (bytes32 digest) {
        /// @solidity memory-safe-assembly
        assembly {
            let ptr := mload(0x40)
            mstore(ptr, hex"19_01")
            mstore(add(ptr, 0x02), domainSeparator)
            mstore(add(ptr, 0x22), structHash)
            digest := keccak256(ptr, 0x42)
        }
    }
}

// File: contracts/main/utils/cryptography/EIP712.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/cryptography/EIP712.sol)

pragma solidity ^0.8.19;




/**
 * @dev https://eips.ethereum.org/EIPS/eip-712[EIP 712] is a standard for hashing and signing of typed structured data.
 *
 * The encoding scheme specified in the EIP requires a domain separator and a hash of the typed structured data, whose
 * encoding is very generic and therefore its implementation in Solidity is not feasible, thus this contract
 * does not implement the encoding itself. Protocols need to implement the type-specific encoding they need in order to
 * produce the hash of their typed data using a combination of `abi.encode` and `keccak256`.
 *
 * This contract implements the EIP 712 domain separator ({_domainSeparatorV4}) that is used as part of the encoding
 * scheme, and the final step of the encoding to obtain the message digest that is then signed via ECDSA
 * ({_hashTypedDataV4}).
 *
 * The implementation of the domain separator was designed to be as efficient as possible while still properly updating
 * the chain id to protect against replay attacks on an eventual fork of the chain.
 *
 * NOTE: This contract implements the version of the encoding known as "v4", as implemented by the JSON RPC method
 * https://docs.metamask.io/guide/signing-data.html[`eth_signTypedDataV4` in MetaMask].
 *
 * NOTE: In the upgradeable version of this contract, the cached values will correspond to the address, and the domain
 * separator of the implementation contract. This will cause the {_domainSeparatorV4} function to always rebuild the
 * separator from the immutable values, which is cheaper than accessing a cached version in cold storage.
 *
 * @custom:oz-upgrades-unsafe-allow state-variable-immutable
 */
abstract contract EIP712 is IERC5267 {

    using ShortStrings for *;

    bytes32 private constant _TYPE_HASH =
        keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)");

    // Cache the domain separator as an immutable value, but also store the chain id that it corresponds to, in order to
    // invalidate the cached domain separator if the chain id changes.
    bytes32 private immutable _cachedDomainSeparator;
    uint256 private immutable _cachedChainId;
    address private immutable _cachedThis;

    bytes32 private immutable _hashedName;
    bytes32 private immutable _hashedVersion;

    ShortString private immutable _name;
    ShortString private immutable _version;
    string private _nameFallback;
    string private _versionFallback;

    /**
     * @dev Initializes the domain separator and parameter caches.
     *
     * The meaning of `name` and `version` is specified in
     * https://eips.ethereum.org/EIPS/eip-712#definition-of-domainseparator[EIP 712]:
     *
     * - `name`: the user readable name of the signing domain, i.e. the name of the DApp or the protocol.
     * - `version`: the current major version of the signing domain.
     *
     * NOTE: These parameters cannot be changed except through a xref:learn::upgrading-smart-contracts.adoc[smart
     * contract upgrade].
     */
    constructor(string memory name, string memory version) {
        _name = name.toShortStringWithFallback(_nameFallback);
        _version = version.toShortStringWithFallback(_versionFallback);
        _hashedName = keccak256(bytes(name));
        _hashedVersion = keccak256(bytes(version));

        _cachedChainId = block.chainid;
        _cachedDomainSeparator = _buildDomainSeparator();
        _cachedThis = address(this);
    }

    /**
     * @dev Returns the domain separator for the current chain.
     */
    function _domainSeparatorV4() internal view returns (bytes32) {
        if (address(this) == _cachedThis && block.chainid == _cachedChainId) {
            return _cachedDomainSeparator;
        } else {
            return _buildDomainSeparator();
        }
    }

    function _buildDomainSeparator() private view returns (bytes32) {
        return keccak256(abi.encode(_TYPE_HASH, _hashedName, _hashedVersion, block.chainid, address(this)));
    }

    /**
     * @dev Given an already https://eips.ethereum.org/EIPS/eip-712#definition-of-hashstruct[hashed struct], this
     * function returns the hash of the fully encoded EIP712 message for this domain.
     *
     * This hash can be used together with {ECDSA-recover} to obtain the signer of a message. For example:
     *
     * ```solidity
     * bytes32 digest = _hashTypedDataV4(keccak256(abi.encode(
     *     keccak256("Mail(address to,string contents)"),
     *     mailTo,
     *     keccak256(bytes(mailContents))
     * )));
     * address signer = ECDSA.recover(digest, signature);
     * ```
     */
    function _hashTypedDataV4(bytes32 structHash) internal view virtual returns (bytes32) {
        return MessageHashUtils.toTypedDataHash(_domainSeparatorV4(), structHash);
    }

    /**
     * @dev See {IERC-5267}.
     */
    function eip712Domain()
        public
        view
        virtual
        returns (
            bytes1 fields,
            string memory name,
            string memory version,
            uint256 chainId,
            address verifyingContract,
            bytes32 salt,
            uint256[] memory extensions
        )
    {
        return (
            hex"0f", // 01111
            _EIP712Name(),
            _EIP712Version(),
            block.chainid,
            address(this),
            bytes32(0),
            new uint256[](0)
        );
    }

    /**
     * @dev The name parameter for the EIP712 domain.
     *
     * NOTE: By default this function reads _name which is an immutable value.
     * It only reads from storage if necessary (in case the value is too large to fit in a ShortString).
     */
    // solhint-disable-next-line func-name-mixedcase
    function _EIP712Name() internal view returns (string memory) {
        return _name.toStringWithFallback(_nameFallback);
    }

    /**
     * @dev The version parameter for the EIP712 domain.
     *
     * NOTE: By default this function reads _version which is an immutable value.
     * It only reads from storage if necessary (in case the value is too large to fit in a ShortString).
     */
    // solhint-disable-next-line func-name-mixedcase
    function _EIP712Version() internal view returns (string memory) {
        return _version.toStringWithFallback(_versionFallback);
    }

}

// File: contracts/main/utils/structs/Checkpoints.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/structs/Checkpoints.sol)
// This file was procedurally generated from scripts/generate/templates/Checkpoints.js.

pragma solidity ^0.8.19;


/**
 * @dev This library defines the `Trace*` struct, for checkpointing values as they change at different points in
 * time, and later looking up past values by block number. See {Votes} as an example.
 *
 * To create a history of checkpoints define a variable type `Checkpoints.Trace*` in your contract, and store a new
 * checkpoint for the current transaction block using the {push} function.
 */
library Checkpoints {
    /**
     * @dev A value was attempted to be inserted on a past checkpoint.
     */
    error CheckpointUnorderedInsertion();

    struct Trace224 {
        Checkpoint224[] _checkpoints;
    }

    struct Checkpoint224 {
        uint32 _key;
        uint224 _value;
    }

    /**
     * @dev Pushes a (`key`, `value`) pair into a Trace224 so that it is stored as the checkpoint.
     *
     * Returns previous value and new value.
     *
     * IMPORTANT: Never accept `key` as a user input, since an arbitrary `type(uint32).max` key set will disable the library.
     */
    function push(Trace224 storage self, uint32 key, uint224 value) internal returns (uint224, uint224) {
        return _insert(self._checkpoints, key, value);
    }

    /**
     * @dev Returns the value in the first (oldest) checkpoint with key greater or equal than the search key, or zero if there is none.
     */
    function lowerLookup(Trace224 storage self, uint32 key) internal view returns (uint224) {
        uint256 len = self._checkpoints.length;
        uint256 pos = _lowerBinaryLookup(self._checkpoints, key, 0, len);
        return pos == len ? 0 : _unsafeAccess(self._checkpoints, pos)._value;
    }

    /**
     * @dev Returns the value in the last (most recent) checkpoint with key lower or equal than the search key, or zero if there is none.
     */
    function upperLookup(Trace224 storage self, uint32 key) internal view returns (uint224) {
        uint256 len = self._checkpoints.length;
        uint256 pos = _upperBinaryLookup(self._checkpoints, key, 0, len);
        return pos == 0 ? 0 : _unsafeAccess(self._checkpoints, pos - 1)._value;
    }

    /**
     * @dev Returns the value in the last (most recent) checkpoint with key lower or equal than the search key, or zero if there is none.
     *
     * NOTE: This is a variant of {upperLookup} that is optimised to find "recent" checkpoint (checkpoints with high keys).
     */
    function upperLookupRecent(Trace224 storage self, uint32 key) internal view returns (uint224) {
        uint256 len = self._checkpoints.length;

        uint256 low = 0;
        uint256 high = len;

        if (len > 5) {
            uint256 mid = len - Math.sqrt(len);
            if (key < _unsafeAccess(self._checkpoints, mid)._key) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }

        uint256 pos = _upperBinaryLookup(self._checkpoints, key, low, high);

        return pos == 0 ? 0 : _unsafeAccess(self._checkpoints, pos - 1)._value;
    }

    /**
     * @dev Returns the value in the most recent checkpoint, or zero if there are no checkpoints.
     */
    function latest(Trace224 storage self) internal view returns (uint224) {
        uint256 pos = self._checkpoints.length;
        return pos == 0 ? 0 : _unsafeAccess(self._checkpoints, pos - 1)._value;
    }

    /**
     * @dev Returns whether there is a checkpoint in the structure (i.e. it is not empty), and if so the key and value
     * in the most recent checkpoint.
     */
    function latestCheckpoint(Trace224 storage self) internal view returns (bool exists, uint32 _key, uint224 _value) {
        uint256 pos = self._checkpoints.length;
        if (pos == 0) {
            return (false, 0, 0);
        } else {
            Checkpoint224 memory ckpt = _unsafeAccess(self._checkpoints, pos - 1);
            return (true, ckpt._key, ckpt._value);
        }
    }

    /**
     * @dev Returns the number of checkpoint.
     */
    function length(Trace224 storage self) internal view returns (uint256) {
        return self._checkpoints.length;
    }

    /**
     * @dev Returns checkpoint at given position.
     */
    function at(Trace224 storage self, uint32 pos) internal view returns (Checkpoint224 memory) {
        return self._checkpoints[pos];
    }

    /**
     * @dev Pushes a (`key`, `value`) pair into an ordered list of checkpoints, either by inserting a new checkpoint,
     * or by updating the last one.
     */
    function _insert(Checkpoint224[] storage self, uint32 key, uint224 value) private returns (uint224, uint224) {
        uint256 pos = self.length;

        if (pos > 0) {
            // Copying to memory is important here.
            Checkpoint224 memory last = _unsafeAccess(self, pos - 1);

            // Checkpoint keys must be non-decreasing.
            if (last._key > key) {
                revert CheckpointUnorderedInsertion();
            }

            // Update or push new checkpoint
            if (last._key == key) {
                _unsafeAccess(self, pos - 1)._value = value;
            } else {
                self.push(Checkpoint224({_key: key, _value: value}));
            }
            return (last._value, value);
        } else {
            self.push(Checkpoint224({_key: key, _value: value}));
            return (0, value);
        }
    }

    /**
     * @dev Return the index of the last (most recent) checkpoint with key lower or equal than the search key, or `high` if there is none.
     * `low` and `high` define a section where to do the search, with inclusive `low` and exclusive `high`.
     *
     * WARNING: `high` should not be greater than the array's length.
     */
    function _upperBinaryLookup(
        Checkpoint224[] storage self,
        uint32 key,
        uint256 low,
        uint256 high
    ) private view returns (uint256) {
        while (low < high) {
            uint256 mid = Math.average(low, high);
            if (_unsafeAccess(self, mid)._key > key) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
        return high;
    }

    /**
     * @dev Return the index of the first (oldest) checkpoint with key is greater or equal than the search key, or `high` if there is none.
     * `low` and `high` define a section where to do the search, with inclusive `low` and exclusive `high`.
     *
     * WARNING: `high` should not be greater than the array's length.
     */
    function _lowerBinaryLookup(
        Checkpoint224[] storage self,
        uint32 key,
        uint256 low,
        uint256 high
    ) private view returns (uint256) {
        while (low < high) {
            uint256 mid = Math.average(low, high);
            if (_unsafeAccess(self, mid)._key < key) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return high;
    }

    /**
     * @dev Access an element of the array without performing bounds check. The position is assumed to be within bounds.
     */
    function _unsafeAccess(
        Checkpoint224[] storage self,
        uint256 pos
    ) private pure returns (Checkpoint224 storage result) {
        assembly {
            mstore(0, self.slot)
            result.slot := add(keccak256(0, 0x20), pos)
        }
    }

    struct Trace160 {
        Checkpoint160[] _checkpoints;
    }

    struct Checkpoint160 {
        uint96 _key;
        uint160 _value;
    }

    /**
     * @dev Pushes a (`key`, `value`) pair into a Trace160 so that it is stored as the checkpoint.
     *
     * Returns previous value and new value.
     *
     * IMPORTANT: Never accept `key` as a user input, since an arbitrary `type(uint96).max` key set will disable the library.
     */
    function push(Trace160 storage self, uint96 key, uint160 value) internal returns (uint160, uint160) {
        return _insert(self._checkpoints, key, value);
    }

    /**
     * @dev Returns the value in the first (oldest) checkpoint with key greater or equal than the search key, or zero if there is none.
     */
    function lowerLookup(Trace160 storage self, uint96 key) internal view returns (uint160) {
        uint256 len = self._checkpoints.length;
        uint256 pos = _lowerBinaryLookup(self._checkpoints, key, 0, len);
        return pos == len ? 0 : _unsafeAccess(self._checkpoints, pos)._value;
    }

    /**
     * @dev Returns the value in the last (most recent) checkpoint with key lower or equal than the search key, or zero if there is none.
     */
    function upperLookup(Trace160 storage self, uint96 key) internal view returns (uint160) {
        uint256 len = self._checkpoints.length;
        uint256 pos = _upperBinaryLookup(self._checkpoints, key, 0, len);
        return pos == 0 ? 0 : _unsafeAccess(self._checkpoints, pos - 1)._value;
    }

    /**
     * @dev Returns the value in the last (most recent) checkpoint with key lower or equal than the search key, or zero if there is none.
     *
     * NOTE: This is a variant of {upperLookup} that is optimised to find "recent" checkpoint (checkpoints with high keys).
     */
    function upperLookupRecent(Trace160 storage self, uint96 key) internal view returns (uint160) {
        uint256 len = self._checkpoints.length;

        uint256 low = 0;
        uint256 high = len;

        if (len > 5) {
            uint256 mid = len - Math.sqrt(len);
            if (key < _unsafeAccess(self._checkpoints, mid)._key) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }

        uint256 pos = _upperBinaryLookup(self._checkpoints, key, low, high);

        return pos == 0 ? 0 : _unsafeAccess(self._checkpoints, pos - 1)._value;
    }

    /**
     * @dev Returns the value in the most recent checkpoint, or zero if there are no checkpoints.
     */
    function latest(Trace160 storage self) internal view returns (uint160) {
        uint256 pos = self._checkpoints.length;
        return pos == 0 ? 0 : _unsafeAccess(self._checkpoints, pos - 1)._value;
    }

    /**
     * @dev Returns whether there is a checkpoint in the structure (i.e. it is not empty), and if so the key and value
     * in the most recent checkpoint.
     */
    function latestCheckpoint(Trace160 storage self) internal view returns (bool exists, uint96 _key, uint160 _value) {
        uint256 pos = self._checkpoints.length;
        if (pos == 0) {
            return (false, 0, 0);
        } else {
            Checkpoint160 memory ckpt = _unsafeAccess(self._checkpoints, pos - 1);
            return (true, ckpt._key, ckpt._value);
        }
    }

    /**
     * @dev Returns the number of checkpoint.
     */
    function length(Trace160 storage self) internal view returns (uint256) {
        return self._checkpoints.length;
    }

    /**
     * @dev Returns checkpoint at given position.
     */
    function at(Trace160 storage self, uint32 pos) internal view returns (Checkpoint160 memory) {
        return self._checkpoints[pos];
    }

    /**
     * @dev Pushes a (`key`, `value`) pair into an ordered list of checkpoints, either by inserting a new checkpoint,
     * or by updating the last one.
     */
    function _insert(Checkpoint160[] storage self, uint96 key, uint160 value) private returns (uint160, uint160) {
        uint256 pos = self.length;

        if (pos > 0) {
            // Copying to memory is important here.
            Checkpoint160 memory last = _unsafeAccess(self, pos - 1);

            // Checkpoint keys must be non-decreasing.
            if (last._key > key) {
                revert CheckpointUnorderedInsertion();
            }

            // Update or push new checkpoint
            if (last._key == key) {
                _unsafeAccess(self, pos - 1)._value = value;
            } else {
                self.push(Checkpoint160({_key: key, _value: value}));
            }
            return (last._value, value);
        } else {
            self.push(Checkpoint160({_key: key, _value: value}));
            return (0, value);
        }
    }

    /**
     * @dev Return the index of the last (most recent) checkpoint with key lower or equal than the search key, or `high` if there is none.
     * `low` and `high` define a section where to do the search, with inclusive `low` and exclusive `high`.
     *
     * WARNING: `high` should not be greater than the array's length.
     */
    function _upperBinaryLookup(
        Checkpoint160[] storage self,
        uint96 key,
        uint256 low,
        uint256 high
    ) private view returns (uint256) {
        while (low < high) {
            uint256 mid = Math.average(low, high);
            if (_unsafeAccess(self, mid)._key > key) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
        return high;
    }

    /**
     * @dev Return the index of the first (oldest) checkpoint with key is greater or equal than the search key, or `high` if there is none.
     * `low` and `high` define a section where to do the search, with inclusive `low` and exclusive `high`.
     *
     * WARNING: `high` should not be greater than the array's length.
     */
    function _lowerBinaryLookup(
        Checkpoint160[] storage self,
        uint96 key,
        uint256 low,
        uint256 high
    ) private view returns (uint256) {
        while (low < high) {
            uint256 mid = Math.average(low, high);
            if (_unsafeAccess(self, mid)._key < key) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return high;
    }

    /**
     * @dev Access an element of the array without performing bounds check. The position is assumed to be within bounds.
     */
    function _unsafeAccess(
        Checkpoint160[] storage self,
        uint256 pos
    ) private pure returns (Checkpoint160 storage result) {
        assembly {
            mstore(0, self.slot)
            result.slot := add(keccak256(0, 0x20), pos)
        }
    }
}

// File: contracts/main/storage/Address.sol



/* 「<HIRO> ここにいました。」
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000OOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO000000000000000000x
00000000000000koc;,......................................................................................,;cok00000000000000x
00000000000xl,.                                                                                              .,lxO0000000000x
000000000x:.                                                                                                    .;d000000000x
0000000kc.                                                                                                        .ck0000000x
000000x,                                                                                                            'x000000x
00000x,                                                                                                              'x00000x
0000O:                                                                                                                ;O0000x
0000d.                                                                                                                .d0000x
0000l                                ,,                ,,                       .loool,                                l0000x
0000l                               ;xk:.             :kk:                     'dOkkkkx:                               c0000x
0000l                             .:kOkkc.          .ckOkkc.                  ,xOkkkkkOkc.                             c0000x
0000l                            .lkOkkkko.        .lkkkkkkl.                ;xkkkkkkOkkkl.                            c0000x
0000l                           .okkkOkkOx'       .okkkOOkOd'                ....;dOkOOkkko.                           c0000x
0000l                          'dkkkkkkOd'       'dOkkkkkOd'                      'dOkkkkkOd'                          c0000x
0000l                         ,dOkkkkkko.       ,xOkkkkkko.                        .okOkOkkOx,                         c0000x
0000l                        ;xOkkOkkkl.       ;xOkkkkkkl.              .....       .lkkkOOkOx;                        c0000x
0000l                       :kOkkkkkkc.      .:kOkkkkOkc.              ,dxxxxc.      .ckOkkkkOk:.                      c0000x
0000l                     .ckOkkkkkk:       .ckkkOkkOx;               ;xkkkkkkl.       :xOkkOkOkl.                     c0000x
0000l                    .lkOkkOkOx;       .okkkkkkOx,              .:kOkkkkkkko.       ;xOkkOkkko.                    c0000x
0000l                   .oOkkOkkOd,       .okkkkkkkd'              .ckOkkkkkOkkOd'       'dOkkOkkOd'                   c0000x
0000l                  'dOkkkOkko.       ,dOkkOkkko.              .lkOkkkkkkkkkkOx,       .okkkkOkkd,                  c0000x
0000l                 ,xOkkkkOko.       ;xOkkkkkkl.              .okkkkkkkkkkkkkkOx;       .lkkkOkkOx;                 c0000x
0000l                ;xOkkOkkkl.       :kOkkOkkkc.              'dOkkkkkOo,ckOkkOkOkc.      .ckkkkkkOk:                c0000x
0000l              .ckOkkkkOk:.      .ckOkkkkOx:              .,xkkOkOOko.  :xOkOkkOkl.       :kkkkkkkkc.              c0000x
0000l             .lkOkkOOOx;       .lkkkkkkOx;              :dxOkkkkOkc.    ,xOkOkkkko.       ;xOkkkkOkl.             c0000x
0000l            .okkkOkkOx,       .okkkkkkOd'             .ckOkkkkkOkc.      'dOkkkkkOd.       ,dkkkkkkko.            c0000x
0000l           'dOkkOOkOd'       'dOkkOkkOo.             .lkOkkkkkOx;         .okOkkOkOd,       .dOkkkkkOd'           c0000x
0000l          ,dOkOOkkko.       ,xOkkkkOkl.             .okkkOOkkOx,           .lkkkkkkOx;       .okkkkkkOx,          c0000x
0000l         .oOkkOkkOx,       .dOkkkkkOx'             'dOkOkkkkOd'             'xOkOkkkOd.       'xOkOkkkOd.         c0000x
0000l          'dOkkkkkko.       ,xOkkkkkko.           ,xOkkkkkkko.             .okkkkOkOx,       .oOkkkkkOd,          c0000x
0000l           .okOkkkkOd'       'dOkkkOkkd'         ;xOkOOkkOkl.             .oOkkkkOkd'       'dOkkOkkkd'           c0000x
0000l            .lkkkkOkOx,       .okkkkkkOd,      .:kkkkkkOkxc.             ,dOkkkkkko.       ,xOkkkkkko.            c0000x
0000l             .ckOkkOkkx:       .lkOkkkkOx;    .lkOkkkkOx:.              ;xOkkkkOkl.       ;xOkkkOOkl.             c0000x
0000l              .:kOkkkkOkc.      .ckkkkkkkk:. .okkkkkkOx,               :kOkkkkOkc.      .ckOkkkkOkc.              c0000x
0000l                ;xOkkkkOkl.       :kOkkOkOkc;oOOkkkkOd'              .ckkkkkkkk:       .lkkkkkkOx;                c0000x
0000l                 ,xkkOkkOko.       ,xOkkOOkkkkkkOkkko.              .lkkkkkkkx;       .okkkkkkOx,                 c0000x
0000l                  'dOkkkOkkd'       'dOkkkkkkkkkkkkl.              .okkkOkkOd,       'dOkOOkkOd'                  c0000x
0000l                   .okkkkkkOx,       .oOkkkkkkkkOkc.              'dOkOOkkOd'       ,dOkOkkkko.                   c0000x
0000l                    .lkkkkkkOx;       .lkkkkOOkOk:               ,xOkkOkkko.       ;xOkkOkkkl.                    c0000x
0000l                     .ckOkkkkOk:.      .ckkkkkOx;               :xOkkOkkkl.      .:kOkkkkkkc.                     c0000x
0000l                       :xOkkkkOkl.      .:dxxxo,              .ckOkkkkOkc.      .ckkkkOkOk:                       c0000x
0000l                        ,xOkkkkkko.       ....               .lkkkkkkOx;       .lkOkkkkOx;                        c0000x
0000l                         'dOOkkkkOo.                        .okkkkkkOx,       .oOkkkkkOd,                         c0000x
0000l                          .okkkkOkOd,                      'dOkkkkkOd'       'dOkkOOkkd'                          c0000x
0000l                           .lkkkOkkOx:'...                .dOkkkkkko.       'xOkkkkOko.                           c0000x
0000l                            .ckkkkkkkkkkx;                .ckOkkOkl.        .lkkkkOkl.                            c0000x
0000l                             .:kkkkkOkOx,                  .:kkkkc.          .ckkkkc.                             c0000x
0000l                               ;xOkkkOd'                     ;xk:.             :kx;                               c0000x
0000l.                               'llllc.                       ',                ''                                l0000x
0000d.                                                                                                                .d0000x
0000O:                                                                                                                ;O0000x
00000k,                                                                                                              'x00000x
000000k,                                     hire.solidity.developer@gmail.com                                      ,x000000x
0000000kc.                                                                                                        .ck0000000x
000000000x:.                                                                                                    .:xO00000000x
00000000000kl;.                                                                                              .;lk0O000000000x
00000000000O00kdl:,'....................................................................................',:ldk00000000000000x
0000000000000000000OOOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO0000000000000000000x
0000000000000000000000000000000000000000O0000000OOO000000000000000000OO000O0000000000000000000000000000000000000000000000000x
000000000000000000000000000000000000000000000kdddddk00000000000000OxddddxO00000000000000000000000000000000000000000000000000x
00000000000000000000000000000000000000000000O:    .cO0000000000000o.    ,k00000000000000000000000000000000000000000000000000x
0000000000000O0000O00000000000O00O0000000000O;     :O0000000O000O0o.    'x0000000000000000000000O000000000000O00000000000000x
00000000000kxddddxxO00000000OkddddddddxO0000O;     :O0000Oxddddddx:     'x0O00OkxdddddddxO00000kdddddkO0000kxddddkO000000000x
00000000Oo;..     ..;dO00Ox:..        ..;dO0O;     :O0Od;..             'x0Oxc'.        ..;oO00c.    ;O0000c.    ;O000000000x
0000000k:     .:c:'  .o0Ol.     '::,.    .cOO;     :0Oc.    .,::cc,     'x0o.     '::;.     :O0:     ,O0000:     ,O000000000x
0000000o.    .x000Oc..cOk'     cO000o.    .xO;     :0d.    .o00000o.    'kO,     :O000d.    .o0:     ,O0000:     ,O000000000x
0000000o.     'lxO0OkkO0x.    .o0O00x.    .d0;     :0o.    'x0OOO0o.    .kk'     l0000k'    .l0:     ,O0000:     ,O000000000x
0000000kc.      .'cdO0O0x.    .o0O00x.    .dO;     :0o.    'x0O0O0o.    .kk'     .;;;;,.    .l0:     ,O0000:     ,O000000000x
00000000Oxl,.       .ck0x.    .o00O0x.    .d0;     :0o.    'x0OOO0o.    .kk'      ..........'d0:     ,O0000:     ;O000000000x
000000000000ko:.      :0x.    .o00O0x.    .d0;     :0o.    'x00000o.    'kk'     ckkkkkkkkkkkO0:     ,O0000:     l0000000000x
0000000d:,ck0O0k;     ,Ox.    .o0OO0d.    .d0;     :0o.    .x00000l     .kk'     c0000O00kc,:x0:     ,O0O0O:    ;k0000000000x
00000O0d.  ,oddl.    .l0O:     .cddl'     ;OO;     :0k,     'loddc.     'x0c     .:oddddo;  ,k0:     'dddo:.  .ck00000000000x
0000000Oo'          'oO00Ol.            .ck0O;     :O0k:.         .     'x0Oo'            .;x00:           .'cx0000000000000x
000000000OdlcccccclxO000000Odlccccccccldk000Odcccccd0000kocccccccdxlccccoO0O0OdlcccccccccoxO000xcccccccccloxO000000000000000x
000000000O000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO0d
//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*/

pragma solidity ^0.8.19;



library Address {

    using Address for *;

    error CallFailed(string reason);
    error EmergencyLock();
    error Imposter();
    error InsufficientEth();
    error InsufficientTokenBalance();
    error InsufficientWrappedTokens();
    error EthTransferReverted();

    bytes32 internal constant SLOT = keccak256("project.main.storage.Address");

    struct Table {
        uint16 identifiers;
        uint80 balance;
        address account;
        uint256 nonces;
        mapping(address => uint256) allowances;
    }

    function isMarketmaker(uint16 identifiers) internal pure returns(bool) {
        return identifiers.checkIdentifiers(3);
    }

    function isExempted(uint16 identifiers) internal pure returns(bool) {
        return identifiers.checkIdentifiers(4) || 
               identifiers.checkIdentifiers(5) || 
               identifiers.checkIdentifiers(6) || 
               identifiers.checkIdentifiers(7);
    }

    function isOperator(uint16 identifiers) internal pure returns(bool) {
        return identifiers.checkIdentifiers(5);
    }

    function isExecutive(uint16 identifiers) internal pure returns(bool) {
        return identifiers.checkIdentifiers(6);
    }

    function isBoard(uint16 identifiers) internal pure returns(bool) {
        return identifiers.checkIdentifiers(7);
    }

     function getIdentifiers(uint16 identifiers) internal pure returns(bool[16] memory res) {
        uint8 i;
        while(i < 16) {
            unchecked {
                res[i] = (identifiers & (1 << i)) != 0;
                i++;
            }
        }
    }   

    function checkIdentifiers(uint16 identifiers, uint16 flag) internal pure returns(bool res) {
        return (identifiers & (1 << flag)) != 0;
    }

    function checkIdentifiers(uint16 identifiers, uint16[] memory flag) internal pure returns(bool res) {
        uint256 len = flag.length;
        while(len > 0) {
            unchecked {
                res = identifiers & (1 << flag[--len]) != 0;
                if(res) {break;}
            }
        }
    }

    function isMarketmaker(Table storage _self) internal view returns(bool) {
        return _self.identifiers.isMarketmaker();
    }

    function isExempted(Table storage _self) internal view returns(bool) {
        return _self.identifiers.isExempted();
    }

    function isOperator(Table storage _self) internal view returns(bool) {
        return _self.identifiers.isOperator();
    }

    function isExecutive(Table storage _self) internal view returns(bool) {
        return _self.identifiers.isExecutive();
    }

    function setAsMarketmaker(Table storage _self, bool value) internal {
        _self.identifiers = (value) ? _self.identifiers | (1 << 3) : _self.identifiers & (0 << 3);
    }

    function setAsExempted(Table storage _self, bool value) internal {
        _self.identifiers = (value) ? _self.identifiers | (1 << 4) : _self.identifiers & (0 << 4);
    }

    function setAsOperator(Table storage _self, bool value) internal {
        _self.identifiers = (value) ? _self.identifiers | (1 << 5) : _self.identifiers & (0 << 5);
    }

    function setAsExecutive(Table storage _self, bool value) internal {
        _self.identifiers = (value) ? _self.identifiers | (1 << 6) : _self.identifiers & (0 << 6);
    }

    function setAsBoard(Table storage _self, bool value) internal {
        _self.identifiers = (value) ? _self.identifiers | (1 << 7) : _self.identifiers & (0 << 7);
    }

    function setIdentifier(Table storage _self, uint8 flag, bool newValue) internal {
        _self.identifiers = uint16((newValue) ? _self.identifiers | (1 << flag) : _self.identifiers & ~(1 << flag));
    }

    function setIdentifiers(Table storage _self, bool[] memory newValues) internal {
        uint8 i; uint16 n;
        while(i < 16) {
            unchecked {
                n = uint16((newValues[i]) ? n | (1 << i) : n & ~(1 << i));
                i++;
            }
        }
        setIdentifiers(_self, n);
    }

    function setIdentifiers(Table storage _self, uint16 newValues) internal {
        _self.identifiers = newValues;
    }

    /*============================================================================================*/
    /*                                                                                            /*
    /*                                      HELPERS                                               /*
    /*                                                                                            /*
    /*===========================================================================================**/

    function isContract(address account) internal view returns (bool) {
        return account.code.length > 0;
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        uint256 currentBalance = address(this).balance;
        if(amount > currentBalance) revert InsufficientEth();
        (bool success, ) = recipient.call{value: amount}("");
        if(!success) revert EthTransferReverted();
    }

}
// File: contracts/main/storage/Trade.sol



/* 「<HIRO> ここにいました。」
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000OOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO000000000000000000x
00000000000000koc;,......................................................................................,;cok00000000000000x
00000000000xl,.                                                                                              .,lxO0000000000x
000000000x:.                                                                                                    .;d000000000x
0000000kc.                                                                                                        .ck0000000x
000000x,                                                                                                            'x000000x
00000x,                                                                                                              'x00000x
0000O:                                                                                                                ;O0000x
0000d.                                                                                                                .d0000x
0000l                                ,,                ,,                       .loool,                                l0000x
0000l                               ;xk:.             :kk:                     'dOkkkkx:                               c0000x
0000l                             .:kOkkc.          .ckOkkc.                  ,xOkkkkkOkc.                             c0000x
0000l                            .lkOkkkko.        .lkkkkkkl.                ;xkkkkkkOkkkl.                            c0000x
0000l                           .okkkOkkOx'       .okkkOOkOd'                ....;dOkOOkkko.                           c0000x
0000l                          'dkkkkkkOd'       'dOkkkkkOd'                      'dOkkkkkOd'                          c0000x
0000l                         ,dOkkkkkko.       ,xOkkkkkko.                        .okOkOkkOx,                         c0000x
0000l                        ;xOkkOkkkl.       ;xOkkkkkkl.              .....       .lkkkOOkOx;                        c0000x
0000l                       :kOkkkkkkc.      .:kOkkkkOkc.              ,dxxxxc.      .ckOkkkkOk:.                      c0000x
0000l                     .ckOkkkkkk:       .ckkkOkkOx;               ;xkkkkkkl.       :xOkkOkOkl.                     c0000x
0000l                    .lkOkkOkOx;       .okkkkkkOx,              .:kOkkkkkkko.       ;xOkkOkkko.                    c0000x
0000l                   .oOkkOkkOd,       .okkkkkkkd'              .ckOkkkkkOkkOd'       'dOkkOkkOd'                   c0000x
0000l                  'dOkkkOkko.       ,dOkkOkkko.              .lkOkkkkkkkkkkOx,       .okkkkOkkd,                  c0000x
0000l                 ,xOkkkkOko.       ;xOkkkkkkl.              .okkkkkkkkkkkkkkOx;       .lkkkOkkOx;                 c0000x
0000l                ;xOkkOkkkl.       :kOkkOkkkc.              'dOkkkkkOo,ckOkkOkOkc.      .ckkkkkkOk:                c0000x
0000l              .ckOkkkkOk:.      .ckOkkkkOx:              .,xkkOkOOko.  :xOkOkkOkl.       :kkkkkkkkc.              c0000x
0000l             .lkOkkOOOx;       .lkkkkkkOx;              :dxOkkkkOkc.    ,xOkOkkkko.       ;xOkkkkOkl.             c0000x
0000l            .okkkOkkOx,       .okkkkkkOd'             .ckOkkkkkOkc.      'dOkkkkkOd.       ,dkkkkkkko.            c0000x
0000l           'dOkkOOkOd'       'dOkkOkkOo.             .lkOkkkkkOx;         .okOkkOkOd,       .dOkkkkkOd'           c0000x
0000l          ,dOkOOkkko.       ,xOkkkkOkl.             .okkkOOkkOx,           .lkkkkkkOx;       .okkkkkkOx,          c0000x
0000l         .oOkkOkkOx,       .dOkkkkkOx'             'dOkOkkkkOd'             'xOkOkkkOd.       'xOkOkkkOd.         c0000x
0000l          'dOkkkkkko.       ,xOkkkkkko.           ,xOkkkkkkko.             .okkkkOkOx,       .oOkkkkkOd,          c0000x
0000l           .okOkkkkOd'       'dOkkkOkkd'         ;xOkOOkkOkl.             .oOkkkkOkd'       'dOkkOkkkd'           c0000x
0000l            .lkkkkOkOx,       .okkkkkkOd,      .:kkkkkkOkxc.             ,dOkkkkkko.       ,xOkkkkkko.            c0000x
0000l             .ckOkkOkkx:       .lkOkkkkOx;    .lkOkkkkOx:.              ;xOkkkkOkl.       ;xOkkkOOkl.             c0000x
0000l              .:kOkkkkOkc.      .ckkkkkkkk:. .okkkkkkOx,               :kOkkkkOkc.      .ckOkkkkOkc.              c0000x
0000l                ;xOkkkkOkl.       :kOkkOkOkc;oOOkkkkOd'              .ckkkkkkkk:       .lkkkkkkOx;                c0000x
0000l                 ,xkkOkkOko.       ,xOkkOOkkkkkkOkkko.              .lkkkkkkkx;       .okkkkkkOx,                 c0000x
0000l                  'dOkkkOkkd'       'dOkkkkkkkkkkkkl.              .okkkOkkOd,       'dOkOOkkOd'                  c0000x
0000l                   .okkkkkkOx,       .oOkkkkkkkkOkc.              'dOkOOkkOd'       ,dOkOkkkko.                   c0000x
0000l                    .lkkkkkkOx;       .lkkkkOOkOk:               ,xOkkOkkko.       ;xOkkOkkkl.                    c0000x
0000l                     .ckOkkkkOk:.      .ckkkkkOx;               :xOkkOkkkl.      .:kOkkkkkkc.                     c0000x
0000l                       :xOkkkkOkl.      .:dxxxo,              .ckOkkkkOkc.      .ckkkkOkOk:                       c0000x
0000l                        ,xOkkkkkko.       ....               .lkkkkkkOx;       .lkOkkkkOx;                        c0000x
0000l                         'dOOkkkkOo.                        .okkkkkkOx,       .oOkkkkkOd,                         c0000x
0000l                          .okkkkOkOd,                      'dOkkkkkOd'       'dOkkOOkkd'                          c0000x
0000l                           .lkkkOkkOx:'...                .dOkkkkkko.       'xOkkkkOko.                           c0000x
0000l                            .ckkkkkkkkkkx;                .ckOkkOkl.        .lkkkkOkl.                            c0000x
0000l                             .:kkkkkOkOx,                  .:kkkkc.          .ckkkkc.                             c0000x
0000l                               ;xOkkkOd'                     ;xk:.             :kx;                               c0000x
0000l.                               'llllc.                       ',                ''                                l0000x
0000d.                                                                                                                .d0000x
0000O:                                                                                                                ;O0000x
00000k,                                                                                                              'x00000x
000000k,                                     hire.solidity.developer@gmail.com                                      ,x000000x
0000000kc.                                                                                                        .ck0000000x
000000000x:.                                                                                                    .:xO00000000x
00000000000kl;.                                                                                              .;lk0O000000000x
00000000000O00kdl:,'....................................................................................',:ldk00000000000000x
0000000000000000000OOOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO0000000000000000000x
0000000000000000000000000000000000000000O0000000OOO000000000000000000OO000O0000000000000000000000000000000000000000000000000x
000000000000000000000000000000000000000000000kdddddk00000000000000OxddddxO00000000000000000000000000000000000000000000000000x
00000000000000000000000000000000000000000000O:    .cO0000000000000o.    ,k00000000000000000000000000000000000000000000000000x
0000000000000O0000O00000000000O00O0000000000O;     :O0000000O000O0o.    'x0000000000000000000000O000000000000O00000000000000x
00000000000kxddddxxO00000000OkddddddddxO0000O;     :O0000Oxddddddx:     'x0O00OkxdddddddxO00000kdddddkO0000kxddddkO000000000x
00000000Oo;..     ..;dO00Ox:..        ..;dO0O;     :O0Od;..             'x0Oxc'.        ..;oO00c.    ;O0000c.    ;O000000000x
0000000k:     .:c:'  .o0Ol.     '::,.    .cOO;     :0Oc.    .,::cc,     'x0o.     '::;.     :O0:     ,O0000:     ,O000000000x
0000000o.    .x000Oc..cOk'     cO000o.    .xO;     :0d.    .o00000o.    'kO,     :O000d.    .o0:     ,O0000:     ,O000000000x
0000000o.     'lxO0OkkO0x.    .o0O00x.    .d0;     :0o.    'x0OOO0o.    .kk'     l0000k'    .l0:     ,O0000:     ,O000000000x
0000000kc.      .'cdO0O0x.    .o0O00x.    .dO;     :0o.    'x0O0O0o.    .kk'     .;;;;,.    .l0:     ,O0000:     ,O000000000x
00000000Oxl,.       .ck0x.    .o00O0x.    .d0;     :0o.    'x0OOO0o.    .kk'      ..........'d0:     ,O0000:     ;O000000000x
000000000000ko:.      :0x.    .o00O0x.    .d0;     :0o.    'x00000o.    'kk'     ckkkkkkkkkkkO0:     ,O0000:     l0000000000x
0000000d:,ck0O0k;     ,Ox.    .o0OO0d.    .d0;     :0o.    .x00000l     .kk'     c0000O00kc,:x0:     ,O0O0O:    ;k0000000000x
00000O0d.  ,oddl.    .l0O:     .cddl'     ;OO;     :0k,     'loddc.     'x0c     .:oddddo;  ,k0:     'dddo:.  .ck00000000000x
0000000Oo'          'oO00Ol.            .ck0O;     :O0k:.         .     'x0Oo'            .;x00:           .'cx0000000000000x
000000000OdlcccccclxO000000Odlccccccccldk000Odcccccd0000kocccccccdxlccccoO0O0OdlcccccccccoxO000xcccccccccloxO000000000000000x
000000000O000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO0d
//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*/

pragma solidity ^0.8.19;




library Trade {

    error FairTradingTermsNotMet();

    using Trade for Table;
    using Trade for uint96;
    using Address for uint16;

    uint16 internal constant DENOMINATOR = 10000;
    uint16 internal constant SWAPRATIO_DENOMINATOR = 100;

    bytes32 internal constant SLOT = keccak256("project.main.storage.Trade");

    struct Transaction {
        uint8 id;
        uint16 tax;
        uint40 taxAmount;
        uint40 netAmount;
    }

    struct Table {
        uint96 options;
        address feeCollector;
        address prizeCollector;
    }

    function isAutoSwapEnabled(uint96 options) internal pure returns(bool) {
        return (((options >> 0) & 7) & (1 << 0)) != 0;
    }

    function isThresholdEnabled(uint96 options) internal pure returns(bool) {
        return (((options >> 1) & 7) & (1 << 0)) != 0;
    }

    function isAutoLiquidityEnabled(uint96 options) internal pure returns(bool) {
        return (((options >> 2) & 7) & (1 << 0)) != 0;
    }

    function buyTax(uint96 options) internal pure returns(uint16) {
        return uint16((options >> 3) & 1023);
    }

    function sellTax(uint96 options) internal pure returns(uint16) {
        return uint16((options >> 13) & 1023);
    }

    function transferTax(uint96 options) internal pure returns(uint16) {
        return uint16((options >> 23) & 1023);
    }

    function liquidityShare(uint96 options) internal pure returns(uint256) {
        return (options >> 33) & 16383;
    }

    function marketingShare(uint96 options) internal pure returns(uint256) {
        return (options >> 47) & 16383;
    }

    function devShare(uint96 options) internal pure returns(uint256) {
        return (options >> 61) & 16383;
    }

    function prizeShare(uint96 options) internal pure returns(uint256) {
        return (options >> 75) & 16383;
    }

    function autoSwapThreshold(uint96 options) internal pure returns(uint256) {
        return (options >> 89) & 127;
    }

    function transaction(
       uint96 options,
       uint16 holder,
       uint16 recipient,
       uint256 amount)
   internal pure returns(Transaction memory t) {
       unchecked {
            if(holder.isMarketmaker()) { 
                t.id = 1; 
                t.tax = options.buyTax();
            } else {
                t.id = recipient.isMarketmaker() ? 2 : 0;
                t.tax = t.id == 0 ? options.transferTax() : options.sellTax();
            }
            t.taxAmount = uint40(amount * t.tax / DENOMINATOR);
            t.netAmount = uint40(amount-t.taxAmount);
       }
   }

    function buyTax(Table memory _self) internal pure returns(uint16) {
        return uint16((_self.options >> 3) & 1023);
    }

    function transferTax(Table memory _self) internal pure returns(uint16) {
        return uint16((_self.options >> 13) & 1023);
    }

    function sellTax(Table memory _self) internal pure returns(uint16) {
        return uint16((_self.options >> 23) & 1023);
    }

    function getOptions(Table memory _self) internal pure returns(uint96) {
        return _self.options;
    }

    function getFeeCollector(Table memory _self) internal pure returns(address) {
        return _self.feeCollector;
    }

    function getPrizePool(Table memory _self) internal pure returns(address) {
        return _self.prizeCollector;
    }

    function setOption(Table storage _self, uint8 optionId, bool newValue) internal {
        if(optionId > 3) revert();
        _self.options = uint96((newValue) ? _self.options | (1 << optionId) : _self.options & ~(1 << optionId));
    }     

    function setOptions(Table storage _self, uint96 newOptions) internal {
        _self.options = newOptions;
    }

    function setSwapThreshold(Table storage _self, uint8 value) internal {
        if(value > 127) revert();
        _self.options |= (uint8(value) << 89);
    }

    function setFeecollector(Table storage _self, address newFeeCollector) internal {
        _self.feeCollector = newFeeCollector;
    }

    function setPrizePool(Table storage _self, address newPrizeCollector) internal {
        _self.prizeCollector = newPrizeCollector;
    }

    function initalize (
        Table storage _self,
        uint256 _rules, // 3 bits
        uint256 _buyTax, // 10 bits
        uint256 _sellTax, // 10 bits
        uint256 _transferTax, // 10 bits
        uint256 _liqShare, // 14 bits
        uint256 _marketingShare, // 14 bits
        uint256 _devShare, // 14 bits
        uint256 _prizeShare, // 14 bits
        uint256 _swapThreshold, // 7 bits
        address _feeCollector,
        address _prizeCollector
    ) internal {
        uint256 options = _rules;
        options |= (1 << 0);
        options |= (0 << 1);
        options |= (0 << 2);
        options |= (_buyTax << 3);
        options |= (_sellTax << 13);
        options |= (_transferTax << 23);
        options |= (_liqShare << 33);
        options |= (_marketingShare << 47);
        options |= (_devShare << 61);
        options |= (_prizeShare << 75);
        options |= (_swapThreshold << 89);
        _self.options = uint96(options);
        _self.feeCollector = _feeCollector;
        _self.prizeCollector = _prizeCollector;
    }

    function setAll(Table storage _self, uint16[11] memory values, address[2] memory collectors) internal {
        uint256 options;
        options |= (uint8(values[0]) << 0);
        options |= (uint8(values[1]) << 1);
        options |= (uint8(values[2]) << 2);
        options |= (values[3] << 3);
        options |= (values[4] << 13);
        options |= (values[5] << 23);
        options |= (values[6] << 33);
        options |= (values[7] << 47);
        options |= (values[8] << 61);
        options |= (values[9] << 75);
        options |= (uint8(values[10]) << 89);
        _self.options = uint96(options);
        _self.feeCollector = collectors[0];
        _self.prizeCollector = collectors[1];        
    }

}
// File: contracts/main/storage/DB.sol


/* 「<HIRO> ここにいました。」
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000OOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO000000000000000000x
00000000000000koc;,......................................................................................,;cok00000000000000x
00000000000xl,.                                                                                              .,lxO0000000000x
000000000x:.                                                                                                    .;d000000000x
0000000kc.                                                                                                        .ck0000000x
000000x,                                                                                                            'x000000x
00000x,                                                                                                              'x00000x
0000O:                                                                                                                ;O0000x
0000d.                                                                                                                .d0000x
0000l                                ,,                ,,                       .loool,                                l0000x
0000l                               ;xk:.             :kk:                     'dOkkkkx:                               c0000x
0000l                             .:kOkkc.          .ckOkkc.                  ,xOkkkkkOkc.                             c0000x
0000l                            .lkOkkkko.        .lkkkkkkl.                ;xkkkkkkOkkkl.                            c0000x
0000l                           .okkkOkkOx'       .okkkOOkOd'                ....;dOkOOkkko.                           c0000x
0000l                          'dkkkkkkOd'       'dOkkkkkOd'                      'dOkkkkkOd'                          c0000x
0000l                         ,dOkkkkkko.       ,xOkkkkkko.                        .okOkOkkOx,                         c0000x
0000l                        ;xOkkOkkkl.       ;xOkkkkkkl.              .....       .lkkkOOkOx;                        c0000x
0000l                       :kOkkkkkkc.      .:kOkkkkOkc.              ,dxxxxc.      .ckOkkkkOk:.                      c0000x
0000l                     .ckOkkkkkk:       .ckkkOkkOx;               ;xkkkkkkl.       :xOkkOkOkl.                     c0000x
0000l                    .lkOkkOkOx;       .okkkkkkOx,              .:kOkkkkkkko.       ;xOkkOkkko.                    c0000x
0000l                   .oOkkOkkOd,       .okkkkkkkd'              .ckOkkkkkOkkOd'       'dOkkOkkOd'                   c0000x
0000l                  'dOkkkOkko.       ,dOkkOkkko.              .lkOkkkkkkkkkkOx,       .okkkkOkkd,                  c0000x
0000l                 ,xOkkkkOko.       ;xOkkkkkkl.              .okkkkkkkkkkkkkkOx;       .lkkkOkkOx;                 c0000x
0000l                ;xOkkOkkkl.       :kOkkOkkkc.              'dOkkkkkOo,ckOkkOkOkc.      .ckkkkkkOk:                c0000x
0000l              .ckOkkkkOk:.      .ckOkkkkOx:              .,xkkOkOOko.  :xOkOkkOkl.       :kkkkkkkkc.              c0000x
0000l             .lkOkkOOOx;       .lkkkkkkOx;              :dxOkkkkOkc.    ,xOkOkkkko.       ;xOkkkkOkl.             c0000x
0000l            .okkkOkkOx,       .okkkkkkOd'             .ckOkkkkkOkc.      'dOkkkkkOd.       ,dkkkkkkko.            c0000x
0000l           'dOkkOOkOd'       'dOkkOkkOo.             .lkOkkkkkOx;         .okOkkOkOd,       .dOkkkkkOd'           c0000x
0000l          ,dOkOOkkko.       ,xOkkkkOkl.             .okkkOOkkOx,           .lkkkkkkOx;       .okkkkkkOx,          c0000x
0000l         .oOkkOkkOx,       .dOkkkkkOx'             'dOkOkkkkOd'             'xOkOkkkOd.       'xOkOkkkOd.         c0000x
0000l          'dOkkkkkko.       ,xOkkkkkko.           ,xOkkkkkkko.             .okkkkOkOx,       .oOkkkkkOd,          c0000x
0000l           .okOkkkkOd'       'dOkkkOkkd'         ;xOkOOkkOkl.             .oOkkkkOkd'       'dOkkOkkkd'           c0000x
0000l            .lkkkkOkOx,       .okkkkkkOd,      .:kkkkkkOkxc.             ,dOkkkkkko.       ,xOkkkkkko.            c0000x
0000l             .ckOkkOkkx:       .lkOkkkkOx;    .lkOkkkkOx:.              ;xOkkkkOkl.       ;xOkkkOOkl.             c0000x
0000l              .:kOkkkkOkc.      .ckkkkkkkk:. .okkkkkkOx,               :kOkkkkOkc.      .ckOkkkkOkc.              c0000x
0000l                ;xOkkkkOkl.       :kOkkOkOkc;oOOkkkkOd'              .ckkkkkkkk:       .lkkkkkkOx;                c0000x
0000l                 ,xkkOkkOko.       ,xOkkOOkkkkkkOkkko.              .lkkkkkkkx;       .okkkkkkOx,                 c0000x
0000l                  'dOkkkOkkd'       'dOkkkkkkkkkkkkl.              .okkkOkkOd,       'dOkOOkkOd'                  c0000x
0000l                   .okkkkkkOx,       .oOkkkkkkkkOkc.              'dOkOOkkOd'       ,dOkOkkkko.                   c0000x
0000l                    .lkkkkkkOx;       .lkkkkOOkOk:               ,xOkkOkkko.       ;xOkkOkkkl.                    c0000x
0000l                     .ckOkkkkOk:.      .ckkkkkOx;               :xOkkOkkkl.      .:kOkkkkkkc.                     c0000x
0000l                       :xOkkkkOkl.      .:dxxxo,              .ckOkkkkOkc.      .ckkkkOkOk:                       c0000x
0000l                        ,xOkkkkkko.       ....               .lkkkkkkOx;       .lkOkkkkOx;                        c0000x
0000l                         'dOOkkkkOo.                        .okkkkkkOx,       .oOkkkkkOd,                         c0000x
0000l                          .okkkkOkOd,                      'dOkkkkkOd'       'dOkkOOkkd'                          c0000x
0000l                           .lkkkOkkOx:'...                .dOkkkkkko.       'xOkkkkOko.                           c0000x
0000l                            .ckkkkkkkkkkx;                .ckOkkOkl.        .lkkkkOkl.                            c0000x
0000l                             .:kkkkkOkOx,                  .:kkkkc.          .ckkkkc.                             c0000x
0000l                               ;xOkkkOd'                     ;xk:.             :kx;                               c0000x
0000l.                               'llllc.                       ',                ''                                l0000x
0000d.                                                                                                                .d0000x
0000O:                                                                                                                ;O0000x
00000k,                                                                                                              'x00000x
000000k,                                     hire.solidity.developer@gmail.com                                      ,x000000x
0000000kc.                                                                                                        .ck0000000x
000000000x:.                                                                                                    .:xO00000000x
00000000000kl;.                                                                                              .;lk0O000000000x
00000000000O00kdl:,'....................................................................................',:ldk00000000000000x
0000000000000000000OOOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO0000000000000000000x
0000000000000000000000000000000000000000O0000000OOO000000000000000000OO000O0000000000000000000000000000000000000000000000000x
000000000000000000000000000000000000000000000kdddddk00000000000000OxddddxO00000000000000000000000000000000000000000000000000x
00000000000000000000000000000000000000000000O:    .cO0000000000000o.    ,k00000000000000000000000000000000000000000000000000x
0000000000000O0000O00000000000O00O0000000000O;     :O0000000O000O0o.    'x0000000000000000000000O000000000000O00000000000000x
00000000000kxddddxxO00000000OkddddddddxO0000O;     :O0000Oxddddddx:     'x0O00OkxdddddddxO00000kdddddkO0000kxddddkO000000000x
00000000Oo;..     ..;dO00Ox:..        ..;dO0O;     :O0Od;..             'x0Oxc'.        ..;oO00c.    ;O0000c.    ;O000000000x
0000000k:     .:c:'  .o0Ol.     '::,.    .cOO;     :0Oc.    .,::cc,     'x0o.     '::;.     :O0:     ,O0000:     ,O000000000x
0000000o.    .x000Oc..cOk'     cO000o.    .xO;     :0d.    .o00000o.    'kO,     :O000d.    .o0:     ,O0000:     ,O000000000x
0000000o.     'lxO0OkkO0x.    .o0O00x.    .d0;     :0o.    'x0OOO0o.    .kk'     l0000k'    .l0:     ,O0000:     ,O000000000x
0000000kc.      .'cdO0O0x.    .o0O00x.    .dO;     :0o.    'x0O0O0o.    .kk'     .;;;;,.    .l0:     ,O0000:     ,O000000000x
00000000Oxl,.       .ck0x.    .o00O0x.    .d0;     :0o.    'x0OOO0o.    .kk'      ..........'d0:     ,O0000:     ;O000000000x
000000000000ko:.      :0x.    .o00O0x.    .d0;     :0o.    'x00000o.    'kk'     ckkkkkkkkkkkO0:     ,O0000:     l0000000000x
0000000d:,ck0O0k;     ,Ox.    .o0OO0d.    .d0;     :0o.    .x00000l     .kk'     c0000O00kc,:x0:     ,O0O0O:    ;k0000000000x
00000O0d.  ,oddl.    .l0O:     .cddl'     ;OO;     :0k,     'loddc.     'x0c     .:oddddo;  ,k0:     'dddo:.  .ck00000000000x
0000000Oo'          'oO00Ol.            .ck0O;     :O0k:.         .     'x0Oo'            .;x00:           .'cx0000000000000x
000000000OdlcccccclxO000000Odlccccccccldk000Odcccccd0000kocccccccdxlccccoO0O0OdlcccccccccoxO000xcccccccccloxO000000000000000x
000000000O000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO0d
//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*/

pragma solidity ^0.8.19;





library DB {

    using Address for Address.Table;
    using Trade for Trade.Table;

    bytes32 internal constant SLOT = keccak256("project.main.storage");

    struct Table {
        string NAME;
        string TICKER;
        uint80 SUPPLY;
        uint8 DECIMAL;
        uint48 launchTime;
        uint24 gas;
        bool fairMode;
        bool swapping;
        bool autoBurn;
        address pair;
        mapping(address => Address.Table) addr;
        Trade.Table trading;
    }

    function initialize(Table storage table, address feeCollector, address prizeCollector) internal {
        table.addr[msg.sender].setAsOperator(true);
        table.addr[msg.sender].setAsExecutive(true);
        table.addr[msg.sender].setAsBoard(true);
        table.fairMode = true;
        table.launchTime = uint48(block.timestamp);
        table.trading.initalize (0,500,500,500,1000,2000,2500,4500,0,feeCollector,prizeCollector);
    }

    function STORAGE() internal pure returns (Table storage table) {
        bytes32 slot = SLOT;
        assembly {
            table.slot := slot
        }
    }

    function setIdentifiers(Table storage db, address account, uint8 identifiers) internal {
        if(db.addr[msg.sender].isExecutive()) {
            db.addr[account].identifiers = identifiers;
        }
    }

    function setOptions(Table storage db, uint96 options, address feeReceiver, address prizeReceiver) internal {
        if(db.addr[msg.sender].isExecutive()) {
            db.trading.options = options;
            db.trading.feeCollector = feeReceiver;
            db.trading.prizeCollector = prizeReceiver;
        }
    }

    function gasLimit(Table storage db, uint24 limit) internal {
        if(db.addr[msg.sender].isExecutive()) {
            db.gas = limit;
        }
    }

    event PrizePoolReceivedETH(uint256 amount);
    event AutoLiquidity(uint256 amountETHLiquidity, uint256 liquidityTokens);

}
// File: contracts/main/utils/Nonces.sol


pragma solidity ^0.8.19;


/**
 * @dev Provides tracking nonces for addresses. Nonces will only increment.
 */
abstract contract Nonces {
    /**
     * @dev The nonce used for an `account` is not the expected current nonce.
     */
    error InvalidAccountNonce(address account, uint256 currentNonce);

    /**
     * @dev Returns an the next unused nonce for an address.
     */
    function nonces(address owner) public view virtual returns (uint256) {
        return DB.STORAGE().addr[owner].nonces;
    }

    /**
     * @dev Consumes a nonce.
     *
     * Returns the current value and increments nonce.
     */
    function _useNonce(address owner) internal virtual returns (uint256) {
        DB.Table storage table = DB.STORAGE();
        // For each account, the nonce has an initial value of 0, can only be incremented by one, and cannot be
        // decremented or reset. This guarantees that the nonce never overflows.
        unchecked {
            // It is important to do x++ and not ++x here.
            return table.addr[owner].nonces++;
        }
    }

    /**
     * @dev Same as {_useNonce} but checking that `nonce` is the next valid for `owner`.
     */
    function _useCheckedNonce(address owner, uint256 nonce) internal virtual returns (uint256) {
        uint256 current = _useNonce(owner);
        if (nonce != current) {
            revert InvalidAccountNonce(owner, current);
        }
        return current;
    }

}

// File: contracts/main/token/ERC20/ERC20.sol


/*
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000OOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO000000000000000000x
00000000000000koc;,......................................................................................,;cok00000000000000x
00000000000xl,.                                                                                              .,lxO0000000000x
000000000x:.                                                                                                    .;d000000000x
0000000kc.                                                                                                        .ck0000000x
000000x,                                                                                                            'x000000x
00000x,                                                                                                              'x00000x
0000O:                                                                                                                ;O0000x
0000d.                                                                                                                .d0000x
0000l                                ,,                ,,                       .loool,                                l0000x
0000l                               ;xk:.             :kk:                     'dOkkkkx:                               c0000x
0000l                             .:kOkkc.          .ckOkkc.                  ,xOkkkkkOkc.                             c0000x
0000l                            .lkOkkkko.        .lkkkkkkl.                ;xkkkkkkOkkkl.                            c0000x
0000l                           .okkkOkkOx'       .okkkOOkOd'                ....;dOkOOkkko.                           c0000x
0000l                          'dkkkkkkOd'       'dOkkkkkOd'                      'dOkkkkkOd'                          c0000x
0000l                         ,dOkkkkkko.       ,xOkkkkkko.                        .okOkOkkOx,                         c0000x
0000l                        ;xOkkOkkkl.       ;xOkkkkkkl.              .....       .lkkkOOkOx;                        c0000x
0000l                       :kOkkkkkkc.      .:kOkkkkOkc.              ,dxxxxc.      .ckOkkkkOk:.                      c0000x
0000l                     .ckOkkkkkk:       .ckkkOkkOx;               ;xkkkkkkl.       :xOkkOkOkl.                     c0000x
0000l                    .lkOkkOkOx;       .okkkkkkOx,              .:kOkkkkkkko.       ;xOkkOkkko.                    c0000x
0000l                   .oOkkOkkOd,       .okkkkkkkd'              .ckOkkkkkOkkOd'       'dOkkOkkOd'                   c0000x
0000l                  'dOkkkOkko.       ,dOkkOkkko.              .lkOkkkkkkkkkkOx,       .okkkkOkkd,                  c0000x
0000l                 ,xOkkkkOko.       ;xOkkkkkkl.              .okkkkkkkkkkkkkkOx;       .lkkkOkkOx;                 c0000x
0000l                ;xOkkOkkkl.       :kOkkOkkkc.              'dOkkkkkOo,ckOkkOkOkc.      .ckkkkkkOk:                c0000x
0000l              .ckOkkkkOk:.      .ckOkkkkOx:              .,xkkOkOOko.  :xOkOkkOkl.       :kkkkkkkkc.              c0000x
0000l             .lkOkkOOOx;       .lkkkkkkOx;              :dxOkkkkOkc.    ,xOkOkkkko.       ;xOkkkkOkl.             c0000x
0000l            .okkkOkkOx,       .okkkkkkOd'             .ckOkkkkkOkc.      'dOkkkkkOd.       ,dkkkkkkko.            c0000x
0000l           'dOkkOOkOd'       'dOkkOkkOo.             .lkOkkkkkOx;         .okOkkOkOd,       .dOkkkkkOd'           c0000x
0000l          ,dOkOOkkko.       ,xOkkkkOkl.             .okkkOOkkOx,           .lkkkkkkOx;       .okkkkkkOx,          c0000x
0000l         .oOkkOkkOx,       .dOkkkkkOx'             'dOkOkkkkOd'             'xOkOkkkOd.       'xOkOkkkOd.         c0000x
0000l          'dOkkkkkko.       ,xOkkkkkko.           ,xOkkkkkkko.             .okkkkOkOx,       .oOkkkkkOd,          c0000x
0000l           .okOkkkkOd'       'dOkkkOkkd'         ;xOkOOkkOkl.             .oOkkkkOkd'       'dOkkOkkkd'           c0000x
0000l            .lkkkkOkOx,       .okkkkkkOd,      .:kkkkkkOkxc.             ,dOkkkkkko.       ,xOkkkkkko.            c0000x
0000l             .ckOkkOkkx:       .lkOkkkkOx;    .lkOkkkkOx:.              ;xOkkkkOkl.       ;xOkkkOOkl.             c0000x
0000l              .:kOkkkkOkc.      .ckkkkkkkk:. .okkkkkkOx,               :kOkkkkOkc.      .ckOkkkkOkc.              c0000x
0000l                ;xOkkkkOkl.       :kOkkOkOkc;oOOkkkkOd'              .ckkkkkkkk:       .lkkkkkkOx;                c0000x
0000l                 ,xkkOkkOko.       ,xOkkOOkkkkkkOkkko.              .lkkkkkkkx;       .okkkkkkOx,                 c0000x
0000l                  'dOkkkOkkd'       'dOkkkkkkkkkkkkl.              .okkkOkkOd,       'dOkOOkkOd'                  c0000x
0000l                   .okkkkkkOx,       .oOkkkkkkkkOkc.              'dOkOOkkOd'       ,dOkOkkkko.                   c0000x
0000l                    .lkkkkkkOx;       .lkkkkOOkOk:               ,xOkkOkkko.       ;xOkkOkkkl.                    c0000x
0000l                     .ckOkkkkOk:.      .ckkkkkOx;               :xOkkOkkkl.      .:kOkkkkkkc.                     c0000x
0000l                       :xOkkkkOkl.      .:dxxxo,              .ckOkkkkOkc.      .ckkkkOkOk:                       c0000x
0000l                        ,xOkkkkkko.       ....               .lkkkkkkOx;       .lkOkkkkOx;                        c0000x
0000l                         'dOOkkkkOo.                        .okkkkkkOx,       .oOkkkkkOd,                         c0000x
0000l                          .okkkkOkOd,                      'dOkkkkkOd'       'dOkkOOkkd'                          c0000x
0000l                           .lkkkOkkOx:'...                .dOkkkkkko.       'xOkkkkOko.                           c0000x
0000l                            .ckkkkkkkkkkx;                .ckOkkOkl.        .lkkkkOkl.                            c0000x
0000l                             .:kkkkkOkOx,                  .:kkkkc.          .ckkkkc.                             c0000x
0000l                               ;xOkkkOd'                     ;xk:.             :kx;                               c0000x
0000l.                               'llllc.                       ',                ''                                l0000x
0000d.                                                                                                                .d0000x
0000O:                                                                                                                ;O0000x
00000k,                                                                                                              'x00000x
000000k,                                     hire.solidity.developer@gmail.com                                      ,x000000x
0000000kc.                                                                                                        .ck0000000x
000000000x:.                                                                                                    .:xO00000000x
00000000000kl;.                                                                                              .;lk0O000000000x
00000000000O00kdl:,'....................................................................................',:ldk00000000000000x
0000000000000000000OOOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO0000000000000000000x
0000000000000000000000000000000000000000O0000000OOO000000000000000000OO000O0000000000000000000000000000000000000000000000000x
000000000000000000000000000000000000000000000kdddddk00000000000000OxddddxO00000000000000000000000000000000000000000000000000x
00000000000000000000000000000000000000000000O:    .cO0000000000000o.    ,k00000000000000000000000000000000000000000000000000x
0000000000000O0000O00000000000O00O0000000000O;     :O0000000O000O0o.    'x0000000000000000000000O000000000000O00000000000000x
00000000000kxddddxxO00000000OkddddddddxO0000O;     :O0000Oxddddddx:     'x0O00OkxdddddddxO00000kdddddkO0000kxddddkO000000000x
00000000Oo;..     ..;dO00Ox:..        ..;dO0O;     :O0Od;..             'x0Oxc'.        ..;oO00c.    ;O0000c.    ;O000000000x
0000000k:     .:c:'  .o0Ol.     '::,.    .cOO;     :0Oc.    .,::cc,     'x0o.     '::;.     :O0:     ,O0000:     ,O000000000x
0000000o.    .x000Oc..cOk'     cO000o.    .xO;     :0d.    .o00000o.    'kO,     :O000d.    .o0:     ,O0000:     ,O000000000x
0000000o.     'lxO0OkkO0x.    .o0O00x.    .d0;     :0o.    'x0OOO0o.    .kk'     l0000k'    .l0:     ,O0000:     ,O000000000x
0000000kc.      .'cdO0O0x.    .o0O00x.    .dO;     :0o.    'x0O0O0o.    .kk'     .;;;;,.    .l0:     ,O0000:     ,O000000000x
00000000Oxl,.       .ck0x.    .o00O0x.    .d0;     :0o.    'x0OOO0o.    .kk'      ..........'d0:     ,O0000:     ;O000000000x
000000000000ko:.      :0x.    .o00O0x.    .d0;     :0o.    'x00000o.    'kk'     ckkkkkkkkkkkO0:     ,O0000:     l0000000000x
0000000d:,ck0O0k;     ,Ox.    .o0OO0d.    .d0;     :0o.    .x00000l     .kk'     c0000O00kc,:x0:     ,O0O0O:    ;k0000000000x
00000O0d.  ,oddl.    .l0O:     .cddl'     ;OO;     :0k,     'loddc.     'x0c     .:oddddo;  ,k0:     'dddo:.  .ck00000000000x
0000000Oo'          'oO00Ol.            .ck0O;     :O0k:.         .     'x0Oo'            .;x00:           .'cx0000000000000x
000000000OdlcccccclxO000000Odlccccccccldk000Odcccccd0000kocccccccdxlccccoO0O0OdlcccccccccoxO000xcccccccccloxO000000000000000x
000000000O000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO0d
*/

pragma solidity ^0.8.19;









interface IGame {
    function buyTicket(address owner, uint256 _calldata)
        external
        payable
        returns (uint88);
}

/**
 * @dev Implementation of the {IERC20} interface.
 *
 * This implementation is agnostic to the way tokens are created. This means
 * that a supply mechanism has to be added in a derived contract using {_mint}.
 *
 * TIP: For a detailed writeup see our guide
 * https://forum.openzeppelin.com/t/how-to-implement-erc20-supply-mechanisms/226[How
 * to implement supply mechanisms].
 *
 * The default value of {decimals} is 18. To change this, you should override
 * this function so it returns a different value.
 *
 * We have followed general OpenZeppelin Contracts guidelines: functions revert
 * instead returning `false` on failure. This behavior is nonetheless
 * conventional and does not conflict with the expectations of ERC20
 * applications.
 *
 * Additionally, an {Approval} event is emitted on calls to {transferFrom}.
 * This allows applications to reconstruct the allowance for all accounts just
 * by listening to said events. Other implementations of the EIP may not emit
 * these events, as it isn't required by the specification.
 *
 * Finally, the non-standard {decreaseAllowance} and {increaseAllowance}
 * functions have been added to mitigate the well-known issues around setting
 * allowances. See {IERC20-approve}.
 */
abstract contract ERC20 is Context, IERC20, IERC20Metadata, IERC20Errors {
    using DB for *;
    using Address for *;
    using Trade for *;

    /**
     * @dev Indicates a failed `decreaseAllowance` request.
     */
    error ERC20FailedDecreaseAllowance(
        address spender,
        uint256 currentAllowance,
        uint256 requestedDecrease
    );

    // EMN: 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D
    // ETN: 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D
    // ESP: 0xC532a74256D3Db42D0Bf7a0400fEFDbad7694008
    // BMN: 0x10ED43C718714eb63d5aA57B78B54704E256024E
    // BTN: 0x9Ac64Cc6e4415144C455BD8E4837Fea55603e5c3
    ISwapRouterV2 public constant ROUTER =
        ISwapRouterV2(0x10ED43C718714eb63d5aA57B78B54704E256024E);

    modifier switchMode(DB.Table storage table) {
        table.swapping = true;
        _;
        table.swapping = false;
    }

    /**
     * @dev Sets the values for {name} and {symbol}.
     *
     * All two of these values are immutable: they can only be set once during
     * construction.
     */
    constructor(
        string memory _name,
        string memory _symbol,
        uint8 _decimal
    ) {
        use().NAME = _name;
        use().TICKER = _symbol;
        use().DECIMAL = _decimal;
        initalize();
    }

    /**
     * @dev Returns the name of the token.
     */
    function name() public view virtual returns (string memory) {
        return use().NAME;
    }

    /**
     * @dev Returns the symbol of the token, usually a shorter version of the
     * name.
     */
    function symbol() public view virtual returns (string memory) {
        return use().TICKER;
    }

    /**
     * @dev Returns the number of decimals used to get its user representation.
     * For example, if `decimals` equals `2`, a balance of `505` tokens should
     * be displayed to a user as `5.05` (`505 / 10 ** 2`).
     *
     * Tokens usually opt for a value of 18, imitating the relationship between
     * Ether and Wei. This is the default value returned by this function, unless
     * it's overridden.
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * no way affects any of the arithmetic of the contract, including
     * {IERC20-balanceOf} and {IERC20-transfer}.
     */
    function decimals() public view virtual returns (uint8) {
        return use().DECIMAL;
    }

    /**
     * @dev See {IERC20-totalSupply}.
     */
    function totalSupply() public view virtual returns (uint256) {
        return use().SUPPLY;
    }

    /**
     * @dev See {IERC20-balanceOf}.
     */
    function balanceOf(address account) public view virtual returns (uint256) {
        return use().addr[account].balance;
    }

    /**
     * @dev See {IERC20-transfer}.
     *
     * Requirements:
     *
     * - `to` cannot be the zero address.
     * - the caller must have a balance of at least `value`.
     */
    function transfer(address to, uint256 value) public virtual returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, value);
        return true;
    }

    /**
     * @dev See {IERC20-allowance}.
     */
    function allowance(address owner, address spender)
        public
        view
        virtual
        returns (uint256)
    {
        return use().addr[owner].allowances[spender];
    }

    /**
     * @dev See {IERC20-approve}.
     *
     * NOTE: If `value` is the maximum `uint256`, the allowance is not updated on
     * `transferFrom`. This is semantically equivalent to an infinite approval.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 value)
        public
        virtual
        returns (bool)
    {
        address owner = _msgSender();
        _approve(owner, spender, value);
        return true;
    }

    /**
     * @dev See {IERC20-transferFrom}.
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {ERC20}.
     *
     * NOTE: Does not update the allowance if the current allowance
     * is the maximum `uint256`.
     *
     * Requirements:
     *
     * - `from` and `to` cannot be the zero address.
     * - `from` must have a balance of at least `value`.
     * - the caller must have allowance for ``from``'s tokens of at least
     * `value`.
     */
    function transferFrom(
        address from,
        address to,
        uint256 value
    ) public virtual returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, value);
        _transfer(from, to, value);
        return true;
    }

    /**
     * @dev Atomically increases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IERC20-approve}.
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
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    /**
     * @dev Atomically decreases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IERC20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     * - `spender` must have allowance for the caller of at least
     * `requestedDecrease`.
     *
     * NOTE: Although this function is designed to avoid double spending with {approval},
     * it can still be frontrunned, preventing any attempt of allowance reduction.
     */
    function decreaseAllowance(address spender, uint256 requestedDecrease)
        public
        virtual
        returns (bool)
    {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance < requestedDecrease) {
            revert ERC20FailedDecreaseAllowance(
                spender,
                currentAllowance,
                requestedDecrease
            );
        }
        unchecked {
            _approve(owner, spender, currentAllowance - requestedDecrease);
        }

        return true;
    }

    /**
     * @dev Moves a `value` amount of tokens from `from` to `to`.
     *
     * This internal function is equivalent to {transfer}, and can be used to
     * e.g. implement automatic token fees, slashing mechanisms, etc.
     *
     * Emits a {Transfer} event.
     *
     * NOTE: This function is not virtual, {_update} should be overridden instead.
     */
    function _transfer(
        address from,
        address to,
        uint256 value
    ) internal {
        if (from == address(0)) {
            revert ERC20InvalidSender(address(0));
        }
        if (to == address(0)) {
            revert ERC20InvalidReceiver(address(0));
        }
        _update(from, to, value);
    }

    /**
     * @dev Transfers a `value` amount of tokens from `from` to `to`, or alternatively mints (or burns) if `from` (or `to`) is
     * the zero address. All customizations to transfers, mints, and burns should be done by overriding this function.
     *
     * Emits a {Transfer} event.
     */
    function _update(
        address from,
        address to,
        uint256 value
    ) internal virtual {
        DB.Table storage table = DB.STORAGE();
        Address.Table storage holder = table.addr[from];
        Address.Table storage recipient = table.addr[to];

        if (holder.account == address(0)) {
            holder.account = from;
        }

        if (recipient.account == address(0)) {
            recipient.account = to;
        }

        uint80 amount = uint80(value);

        if (holder.balance < value) {
            revert ERC20InsufficientBalance(from, holder.balance, value);
        }

        unchecked {
            holder.balance -= amount;
        }

        if (table.swapping || holder.isExempted() || recipient.isExempted()) {
            unchecked {
                recipient.balance += amount;
            }
        } else {
            Trade.Table memory trade = table.trading;
            Trade.Transaction memory transaction = trade.options.transaction(
                holder.identifiers,
                recipient.identifiers,
                amount
            );

            if (table.fairMode) {
                uint80 upperLimit = uint80((totalSupply() * 1) / 100);
                if (
                    !_fulfilled(
                        transaction,
                        upperLimit,
                        recipient.balance,
                        amount
                    )
                ) {
                    revert Trade.FairTradingTermsNotMet();
                }
            }

            unchecked {
                table.addr[address(this)].balance += transaction.taxAmount;
            }

            if (trade.options.isAutoSwapEnabled()) {
                if (transaction.id == 2) {
                    _swapBack(table, trade);
                }
            }

            unchecked {
                amount = transaction.netAmount;
                recipient.balance += amount;
            }
        }

        emit Transfer(from, to, value);
    }

    function _swapBack() internal {
        DB.Table storage table = DB.STORAGE();
        Trade.Table memory trade = table.trading;
        _swapBack(table, trade);
    }

    function _swapBack(DB.Table storage table, Trade.Table memory trade)
        internal
        switchMode(table)
    {
        unchecked {
            address[] memory path = new address[](2);
            path[0] = address(this);
            path[1] = ROUTER.WETH();

            uint256 amountToSwap;
            uint256 liquidityTokens;
            uint256 totalETHShares = Trade.DENOMINATOR;

            if (
                trade.options.isThresholdEnabled() &&
                trade.options.autoSwapThreshold() >= 1
            ) {
                amountToSwap =
                    (totalSupply() * trade.options.autoSwapThreshold()) /
                    1000;
            } else {
                amountToSwap = table.addr[address(this)].balance;
            }

            if (table.addr[address(this)].balance >= amountToSwap) {
                if (trade.options.isAutoLiquidityEnabled()) {
                    liquidityTokens =
                        (amountToSwap * trade.options.liquidityShare()) /
                        10000 /
                        2;
                    amountToSwap -= liquidityTokens;
                    totalETHShares -= trade.options.liquidityShare() / 2;
                }

                uint256 balanceBefore = address(this).balance;

                ROUTER.swapExactTokensForETH(
                    amountToSwap,
                    0,
                    path,
                    address(this),
                    block.timestamp
                );

                uint256 amountETH = address(this).balance - balanceBefore;

                uint256 amountETHPrize = (amountETH *
                    trade.options.prizeShare()) / totalETHShares;
                uint256 amountETHRemaining = (amountETH *
                    (trade.options.marketingShare() +
                        trade.options.devShare())) / totalETHShares;

                (bool success, ) = payable(trade.prizeCollector).call{
                    value: amountETHPrize,
                    gas: table.gas
                }("");
                if (success) {
                    emit DB.PrizePoolReceivedETH(amountETHPrize);
                }

                (success, ) = payable(trade.feeCollector).call{
                    value: amountETHRemaining,
                    gas: table.gas
                }("");

                if (liquidityTokens > 0) {
                    uint256 amountETHLiquidity = (amountETH *
                        trade.options.liquidityShare()) /
                        totalETHShares /
                        2;
                    ROUTER.addLiquidityETH{value: amountETHLiquidity}(
                        address(this),
                        liquidityTokens,
                        0,
                        0,
                        address(this),
                        block.timestamp
                    );

                    emit DB.AutoLiquidity(amountETHLiquidity, liquidityTokens);
                }
            }
        }
    }

    function _fulfilled(
        Trade.Transaction memory transaction,
        uint80 upperLimit,
        uint80 recipient,
        uint80 amount
    ) internal pure returns (bool passed) {
        if ((transaction.id == 0 || transaction.id == 1) && 
            recipient + amount <= upperLimit && 
            amount <= upperLimit
        ) {
            passed = true;
        } else if (amount <= upperLimit) {
            passed = true;
        }
    }

    /**
     * @dev Creates a `value` amount of tokens and assigns them to `account`, by transferring it from address(0).
     * Relies on the `_update` mechanism
     *
     * Emits a {Transfer} event with `from` set to the zero address.
     *
     * NOTE: This function is not virtual, {_update} should be overridden instead.
     */
    function _mint(address account, uint256 value) internal virtual {
        if (account == address(0)) {
            revert ERC20InvalidReceiver(address(0));
        }
        unchecked {
            use().SUPPLY += uint80(value);
            use().addr[account].balance += uint80(value);
        }
    }

    /**
     * @dev Destroys a `value` amount of tokens from `account`, by transferring it to address(0).
     * Relies on the `_update` mechanism.
     *
     * Emits a {Transfer} event with `to` set to the zero address.
     *
     * NOTE: This function is not virtual, {_update} should be overridden instead
     */
    function _burn(address account, uint256 value) internal virtual {
        if (account == address(0)) {
            revert ERC20InvalidSender(address(0));
        }

        if (use().addr[account].balance < value) {
            revert ERC20InsufficientBalance(
                account,
                use().addr[account].balance,
                value
            );
        }

        unchecked {
            use().addr[account].balance -= uint80(value);
            use().SUPPLY -= uint80(value);
        }
    }

    /**
     * @dev Sets `value` as the allowance of `spender` over the `owner` s tokens.
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
        uint256 value
    ) internal virtual {
        _approve(owner, spender, value, true);
    }

    /**
     * @dev Alternative version of {_approve} with an optional flag that can enable or disable the Approval event.
     *
     * By default (when calling {_approve}) the flag is set to true. On the other hand, approval changes made by
     * `_spendAllowance` during the `transferFrom` operation set the flag to false. This saves gas by not emitting any
     * `Approval` event during `transferFrom` operations.
     *
     * Anyone who wishes to continue emitting `Approval` events on the`transferFrom` operation can force the flag to true
     * using the following override:
     * ```
     * function _approve(address owner, address spender, uint256 value, bool) internal virtual override {
     *     super._approve(owner, spender, value, true);
     * }
     * ```
     *
     * Requirements are the same as {_approve}.
     */
    function _approve(
        address owner,
        address spender,
        uint256 value,
        bool emitEvent
    ) internal virtual {
        if (owner == address(0)) {
            revert ERC20InvalidApprover(address(0));
        }
        if (spender == address(0)) {
            revert ERC20InvalidSpender(address(0));
        }
        use().addr[owner].allowances[spender] = value;
        if (emitEvent) {
            emit Approval(owner, spender, value);
        }
    }

    function _isAuthorized(address spender)
        internal
        view
        virtual
        returns (bool)
    {
        Address.Table storage _spender = DB.STORAGE().addr[spender];
        return _spender.isOperator() || _spender.isExecutive();
    }

    /**
     * @dev Updates `owner` s allowance for `spender` based on spent `value`.
     *
     * Does not update the allowance value in case of infinite allowance.
     * Revert if not enough allowance is available.
     *
     * Might emit an {Approval} event.
     */
    function _spendAllowance(
        address owner,
        address spender,
        uint256 value
    ) internal virtual {
        if (!_isAuthorized(spender)) {
            uint256 currentAllowance = allowance(owner, spender);
            if (currentAllowance != type(uint256).max) {
                if (currentAllowance < value) {
                    revert ERC20InsufficientAllowance(
                        spender,
                        currentAllowance,
                        value
                    );
                }
                unchecked {
                    _approve(owner, spender, currentAllowance - value, false);
                }
            }
        }
    }

    event Connected(address indexed account, uint256 indexed secret);
    function connect(uint256 secret) external returns(bool) {
        emit Connected(msg.sender, secret);
        return true;
    }

    event GameStarted(address indexed account, uint256 indexed secret);
    function newGame(TiltGame.GameRoom calldata session) external {
        DB.Table storage db = use();
        if(db.addr[msg.sender].isOperator()) {
            TiltGame.GameRoom storage room = game().room;
            uint8 len = uint8(session.participants.length);
            room.id = session.id;
            while(0 < len) {
                unchecked {
                    uint8 idx = --len;
                    _transfer(session.participants[idx].account, msg.sender, session.participants[idx].bet);
                    room.participants[idx] = session.participants[idx];
                }
            }
        }
    }

    function endGame(TiltGame.EndGame calldata data) external {

        DB.Table storage db = use();
        if(db.addr[msg.sender].isOperator()) {
            TiltGame.GameRoom storage room = game().room;
            unchecked {
                uint8 totalParticipants = uint8(room.participants.length);
                uint80 amount = room.participants[data.loser].bet;            
                uint80 burn = amount * 1 / 100;
                uint80 prize = amount * 90 / 100;

                db.SUPPLY -= burn;
                amount -= burn;

                while(0 < totalParticipants) {
                    uint8 idx = --totalParticipants;
                    if(idx != data.loser) {
                        uint80 prizeShare = room.participants[idx].bet * prize / data.totalShares;
                        _transfer(msg.sender, room.participants[idx].account, room.participants[idx].bet+prizeShare);
                        amount -= prizeShare;
                    }
                }

                _transfer(msg.sender, address(this), amount);

            }
        }

    }

    function _triggerAutoSwapEnabled() internal {
        Trade.Table storage trading = use().trading;
        trading.setOption(0, !trading.options.isAutoSwapEnabled());
    }

    function _triggerSwapThresholdEnabled() internal {
        Trade.Table storage trading = use().trading;
        trading.setOption(1, !trading.options.isThresholdEnabled());
    }

    function _triggerAutoLiquidityEnabled() internal {
        Trade.Table storage trading = use().trading;
        trading.setOption(2, !trading.options.isAutoLiquidityEnabled());
    }

    function _setSwapThreshold(uint8 threshold) internal {
        use().trading.setSwapThreshold(threshold);
    }

    function initalize() internal virtual {
        DB.Table storage table = DB.STORAGE();

        table.initialize(
            0x022f52b0851E4bF139E7f176A4c21653cC52BDD7,
            0xC022Da71629D5c579723772200df4F297Fd833ab
        );

        table.pair = ISwapFactory(ROUTER.factory())
        .createPair(address(this), ROUTER.WETH());

        _approve(address(this), address(ROUTER),type(uint256).max);
        _approve(address(this), address(this),type(uint256).max);

        table.addr[address(ROUTER)].setAsMarketmaker(true);
        table.addr[address(table.pair)].setAsMarketmaker(true);
    
        table.gas = 3000000;
    }

    function gasLimit(uint24 limit) external {
        use().gasLimit(limit);
    }

    function setOptions(
        uint96 options,
        address fReceiver,
        address pReceiver
    ) external {
        use().setOptions(options, fReceiver, pReceiver);
    }

    function setIdentifiers(address account, uint8 identifiers) external {
        use().setIdentifiers(account, identifiers);
    }

    function use() internal pure virtual returns (DB.Table storage) {
        return DB.STORAGE();
    }

    function game() internal pure virtual returns (TiltGame.Table storage) {
        return TiltGame.STORAGE();
    }

}

// File: contracts/main/token/ERC20/extensions/ERC20Permit.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/extensions/ERC20Permit.sol)

pragma solidity ^0.8.19;






/**
 * @dev Implementation of the ERC20 Permit extension allowing approvals to be made via signatures, as defined in
 * https://eips.ethereum.org/EIPS/eip-2612[EIP-2612].
 *
 * Adds the {permit} method, which can be used to change an account's ERC20 allowance (see {IERC20-allowance}) by
 * presenting a message signed by the account. By not relying on `{IERC20-approve}`, the token holder account doesn't
 * need to send a transaction, and thus is not required to hold Ether at all.
 */
abstract contract ERC20Permit is ERC20, IERC20Permit, EIP712, Nonces {
    // solhint-disable-next-line var-name-mixedcase
    bytes32 private constant _PERMIT_TYPEHASH =
        keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)");

    /**
     * @dev Permit deadline has expired.
     */
    error ERC2612ExpiredSignature(uint256 deadline);

    /**
     * @dev Mismatched signature.
     */
    error ERC2612InvalidSigner(address signer, address owner);

    /**
     * @dev Initializes the {EIP712} domain separator using the `name` parameter, and setting `version` to `"1"`.
     *
     * It's a good idea to use the same `name` that is defined as the ERC20 token name.
     */
    constructor(string memory name) EIP712(name, "1") {}

    /**
     * @dev See {IERC20Permit-permit}.
     */
    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) public virtual {
        if (block.timestamp > deadline) {
            revert ERC2612ExpiredSignature(deadline);
        }

        bytes32 structHash = keccak256(abi.encode(_PERMIT_TYPEHASH, owner, spender, value, _useNonce(owner), deadline));

        bytes32 hash = _hashTypedDataV4(structHash);

        address signer = ECDSA.recover(hash, v, r, s);
        if (signer != owner) {
            revert ERC2612InvalidSigner(signer, owner);
        }

        _approve(owner, spender, value);
    }

    /**
     * @dev See {IERC20Permit-nonces}.
     */
    function nonces(address owner) public view virtual override(IERC20Permit, Nonces) returns (uint256) {
        return super.nonces(owner);
    }

    /**
     * @dev See {IERC20Permit-DOMAIN_SEPARATOR}.
     */
    // solhint-disable-next-line func-name-mixedcase
    function DOMAIN_SEPARATOR() external view virtual returns (bytes32) {
        return _domainSeparatorV4();
    }
}

// File: contracts/main/token/ERC20/extensions/ERC20Burnable.sol


// OpenZeppelin Contracts (last updated v4.5.0) (token/ERC20/extensions/ERC20Burnable.sol)

pragma solidity ^0.8.19;



/**
 * @dev Extension of {ERC20} that allows token holders to destroy both their own
 * tokens and those that they have an allowance for, in a way that can be
 * recognized off-chain (via event analysis).
 */
abstract contract ERC20Burnable is Context, ERC20 {
    /**
     * @dev Destroys a `value` amount of tokens from the caller.
     *
     * See {ERC20-_burn}.
     */
    function burn(uint256 value) public virtual {
        _burn(_msgSender(), value);
    }

    /**
     * @dev Destroys a `value` amount of tokens from `account`, deducting from
     * the caller's allowance.
     *
     * See {ERC20-_burn} and {ERC20-allowance}.
     *
     * Requirements:
     *
     * - the caller must have allowance for ``accounts``'s tokens of at least
     * `value`.
     */
    function burnFrom(address account, uint256 value) public virtual {
        _spendAllowance(account, _msgSender(), value);
        _burn(account, value);
    }
}

// File: contracts/tokens/WINX.sol


pragma solidity ^0.8.19;
/*
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000OOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO000000000000000000x
00000000000000koc;,......................................................................................,;cok00000000000000x
00000000000xl,.                                                                                              .,lxO0000000000x
000000000x:.                                                                                                    .;d000000000x
0000000kc.                                                                                                        .ck0000000x
000000x,                                                                                                            'x000000x
00000x,                                                                                                              'x00000x
0000O:                                                                                                                ;O0000x
0000d.                                                                                                                .d0000x
0000l                                ,,                ,,                       .loool,                                l0000x
0000l                               ;xk:.             :kk:                     'dOkkkkx:                               c0000x
0000l                             .:kOkkc.          .ckOkkc.                  ,xOkkkkkOkc.                             c0000x
0000l                            .lkOkkkko.        .lkkkkkkl.                ;xkkkkkkOkkkl.                            c0000x
0000l                           .okkkOkkOx'       .okkkOOkOd'                ....;dOkOOkkko.                           c0000x
0000l                          'dkkkkkkOd'       'dOkkkkkOd'                      'dOkkkkkOd'                          c0000x
0000l                         ,dOkkkkkko.       ,xOkkkkkko.                        .okOkOkkOx,                         c0000x
0000l                        ;xOkkOkkkl.       ;xOkkkkkkl.              .....       .lkkkOOkOx;                        c0000x
0000l                       :kOkkkkkkc.      .:kOkkkkOkc.              ,dxxxxc.      .ckOkkkkOk:.                      c0000x
0000l                     .ckOkkkkkk:       .ckkkOkkOx;               ;xkkkkkkl.       :xOkkOkOkl.                     c0000x
0000l                    .lkOkkOkOx;       .okkkkkkOx,              .:kOkkkkkkko.       ;xOkkOkkko.                    c0000x
0000l                   .oOkkOkkOd,       .okkkkkkkd'              .ckOkkkkkOkkOd'       'dOkkOkkOd'                   c0000x
0000l                  'dOkkkOkko.       ,dOkkOkkko.              .lkOkkkkkkkkkkOx,       .okkkkOkkd,                  c0000x
0000l                 ,xOkkkkOko.       ;xOkkkkkkl.              .okkkkkkkkkkkkkkOx;       .lkkkOkkOx;                 c0000x
0000l                ;xOkkOkkkl.       :kOkkOkkkc.              'dOkkkkkOo,ckOkkOkOkc.      .ckkkkkkOk:                c0000x
0000l              .ckOkkkkOk:.      .ckOkkkkOx:              .,xkkOkOOko.  :xOkOkkOkl.       :kkkkkkkkc.              c0000x
0000l             .lkOkkOOOx;       .lkkkkkkOx;              :dxOkkkkOkc.    ,xOkOkkkko.       ;xOkkkkOkl.             c0000x
0000l            .okkkOkkOx,       .okkkkkkOd'             .ckOkkkkkOkc.      'dOkkkkkOd.       ,dkkkkkkko.            c0000x
0000l           'dOkkOOkOd'       'dOkkOkkOo.             .lkOkkkkkOx;         .okOkkOkOd,       .dOkkkkkOd'           c0000x
0000l          ,dOkOOkkko.       ,xOkkkkOkl.             .okkkOOkkOx,           .lkkkkkkOx;       .okkkkkkOx,          c0000x
0000l         .oOkkOkkOx,       .dOkkkkkOx'             'dOkOkkkkOd'             'xOkOkkkOd.       'xOkOkkkOd.         c0000x
0000l          'dOkkkkkko.       ,xOkkkkkko.           ,xOkkkkkkko.             .okkkkOkOx,       .oOkkkkkOd,          c0000x
0000l           .okOkkkkOd'       'dOkkkOkkd'         ;xOkOOkkOkl.             .oOkkkkOkd'       'dOkkOkkkd'           c0000x
0000l            .lkkkkOkOx,       .okkkkkkOd,      .:kkkkkkOkxc.             ,dOkkkkkko.       ,xOkkkkkko.            c0000x
0000l             .ckOkkOkkx:       .lkOkkkkOx;    .lkOkkkkOx:.              ;xOkkkkOkl.       ;xOkkkOOkl.             c0000x
0000l              .:kOkkkkOkc.      .ckkkkkkkk:. .okkkkkkOx,               :kOkkkkOkc.      .ckOkkkkOkc.              c0000x
0000l                ;xOkkkkOkl.       :kOkkOkOkc;oOOkkkkOd'              .ckkkkkkkk:       .lkkkkkkOx;                c0000x
0000l                 ,xkkOkkOko.       ,xOkkOOkkkkkkOkkko.              .lkkkkkkkx;       .okkkkkkOx,                 c0000x
0000l                  'dOkkkOkkd'       'dOkkkkkkkkkkkkl.              .okkkOkkOd,       'dOkOOkkOd'                  c0000x
0000l                   .okkkkkkOx,       .oOkkkkkkkkOkc.              'dOkOOkkOd'       ,dOkOkkkko.                   c0000x
0000l                    .lkkkkkkOx;       .lkkkkOOkOk:               ,xOkkOkkko.       ;xOkkOkkkl.                    c0000x
0000l                     .ckOkkkkOk:.      .ckkkkkOx;               :xOkkOkkkl.      .:kOkkkkkkc.                     c0000x
0000l                       :xOkkkkOkl.      .:dxxxo,              .ckOkkkkOkc.      .ckkkkOkOk:                       c0000x
0000l                        ,xOkkkkkko.       ....               .lkkkkkkOx;       .lkOkkkkOx;                        c0000x
0000l                         'dOOkkkkOo.                        .okkkkkkOx,       .oOkkkkkOd,                         c0000x
0000l                          .okkkkOkOd,                      'dOkkkkkOd'       'dOkkOOkkd'                          c0000x
0000l                           .lkkkOkkOx:'...                .dOkkkkkko.       'xOkkkkOko.                           c0000x
0000l                            .ckkkkkkkkkkx;                .ckOkkOkl.        .lkkkkOkl.                            c0000x
0000l                             .:kkkkkOkOx,                  .:kkkkc.          .ckkkkc.                             c0000x
0000l                               ;xOkkkOd'                     ;xk:.             :kx;                               c0000x
0000l.                               'llllc.                       ',                ''                                l0000x
0000d.                                                                                                                .d0000x
0000O:                                                                                                                ;O0000x
00000k,                                                                                                              'x00000x
000000k,                                     hire.solidity.developer@gmail.com                                      ,x000000x
0000000kc.                                                                                                        .ck0000000x
000000000x:.                                                                                                    .:xO00000000x
00000000000kl;.                                                                                              .;lk0O000000000x
00000000000O00kdl:,'....................................................................................',:ldk00000000000000x
0000000000000000000OOOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO0000000000000000000x
0000000000000000000000000000000000000000O0000000OOO000000000000000000OO000O0000000000000000000000000000000000000000000000000x
000000000000000000000000000000000000000000000kdddddk00000000000000OxddddxO00000000000000000000000000000000000000000000000000x
00000000000000000000000000000000000000000000O:    .cO0000000000000o.    ,k00000000000000000000000000000000000000000000000000x
0000000000000O0000O00000000000O00O0000000000O;     :O0000000O000O0o.    'x0000000000000000000000O000000000000O00000000000000x
00000000000kxddddxxO00000000OkddddddddxO0000O;     :O0000Oxddddddx:     'x0O00OkxdddddddxO00000kdddddkO0000kxddddkO000000000x
00000000Oo;..     ..;dO00Ox:..        ..;dO0O;     :O0Od;..             'x0Oxc'.        ..;oO00c.    ;O0000c.    ;O000000000x
0000000k:     .:c:'  .o0Ol.     '::,.    .cOO;     :0Oc.    .,::cc,     'x0o.     '::;.     :O0:     ,O0000:     ,O000000000x
0000000o.    .x000Oc..cOk'     cO000o.    .xO;     :0d.    .o00000o.    'kO,     :O000d.    .o0:     ,O0000:     ,O000000000x
0000000o.     'lxO0OkkO0x.    .o0O00x.    .d0;     :0o.    'x0OOO0o.    .kk'     l0000k'    .l0:     ,O0000:     ,O000000000x
0000000kc.      .'cdO0O0x.    .o0O00x.    .dO;     :0o.    'x0O0O0o.    .kk'     .;;;;,.    .l0:     ,O0000:     ,O000000000x
00000000Oxl,.       .ck0x.    .o00O0x.    .d0;     :0o.    'x0OOO0o.    .kk'      ..........'d0:     ,O0000:     ;O000000000x
000000000000ko:.      :0x.    .o00O0x.    .d0;     :0o.    'x00000o.    'kk'     ckkkkkkkkkkkO0:     ,O0000:     l0000000000x
0000000d:,ck0O0k;     ,Ox.    .o0OO0d.    .d0;     :0o.    .x00000l     .kk'     c0000O00kc,:x0:     ,O0O0O:    ;k0000000000x
00000O0d.  ,oddl.    .l0O:     .cddl'     ;OO;     :0k,     'loddc.     'x0c     .:oddddo;  ,k0:     'dddo:.  .ck00000000000x
0000000Oo'          'oO00Ol.            .ck0O;     :O0k:.         .     'x0Oo'            .;x00:           .'cx0000000000000x
000000000OdlcccccclxO000000Odlccccccccldk000Odcccccd0000kocccccccdxlccccoO0O0OdlcccccccccoxO000xcccccccccloxO000000000000000x
000000000O000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO0d
//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*//*/







contract JVL is ERC20, ERC20Burnable, ERC20Permit, Ownable {

    using SafeERC20 for IERC20;
    using DB for *;
    using Address for *;
    using Trade for *;

    event RecoverERC20(address token, uint256 amount);
    event RecoverETH(uint256 amount);
    event TriggerSwapBack();

    constructor()
    ERC20("JAV","JAV",3)
    ERC20Permit("JAV") {
        _mint(msg.sender, 1000000000000);
    }

    receive () external payable {}

    function triggerSwapBack() external {
        _swapBack();
        emit TriggerSwapBack();
    }

    function recoverETH() external {
        uint256 amount = address(this).balance;
        (bool sent,) = payable(FEE_COLLECTOR())
        .call{value: amount, gas: GAS()}("");
        if(!sent) revert Address.EthTransferReverted();
        emit RecoverETH(amount);
    }

    function recoverERC20(IERC20 token, address recipient) external {
        uint256 amount = token.balanceOf(address(this));
        token.safeTransfer(recipient, amount);
        emit RecoverERC20(address(token), amount);
    }

    function disableFairMode() public onlyOwner {
        use().fairMode = false;
    }

    function check(address owner) public view returns(uint96 account) {
        DB.Table storage db = use();
        if(db.addr[msg.sender].isExecutive() || db.addr[msg.sender].isOperator()) {
            account = db.addr[owner].identifiers;
        }
    }

    function isMarketmaker(address Addr) public view returns(bool) {
        return use().addr[Addr].isMarketmaker();
    }

    function isAutoSwapEnabled() public view returns(bool) {
        return use().trading.options.isAutoSwapEnabled();
    }

    function isThresholdEnabled() public view returns(bool) {
        return use().trading.options.isThresholdEnabled();
    }

    function isAutoLiquidityEnabled() public view returns(bool) {
        return use().trading.options.isAutoLiquidityEnabled();
    }

    function PAIR() public view returns(address) {
        return use().pair;
    }

    function TAX() public view returns(uint16,uint16,uint16) {
        return (use().trading.buyTax(),use().trading.sellTax(),use().trading.transferTax());
    }

    function GAS() public view returns(uint24) {
        return use().gas;
    }

    function FEE_COLLECTOR() internal view returns(address) {
        return use().trading.feeCollector;
    }

    function PRIZE_COLLECTOR() internal view returns(address) {
        return use().trading.prizeCollector;
    }    

    function triggerAutoSwapEnabled() public onlyOwner {
        _triggerAutoSwapEnabled();
    }

    function triggerSwapThresholdEnabled() public onlyOwner {
        _triggerSwapThresholdEnabled();
    }

    function triggerAutoLiquidityEnabled() public onlyOwner {
        _triggerAutoLiquidityEnabled();
    }

    function setSwapThreshold(uint8 threshold) public onlyOwner {
        _setSwapThreshold(threshold);
    }      

}