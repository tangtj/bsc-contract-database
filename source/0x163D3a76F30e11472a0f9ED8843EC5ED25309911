
// File: lib/openzeppelin-contracts-upgradeable/contracts/interfaces/IERC4626Upgradeable.sol
// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v4.8.0) (interfaces/IERC4626.sol)

pragma solidity ^0.8.0;

import "../token/ERC20/IERC20Upgradeable.sol";
import "../token/ERC20/extensions/IERC20MetadataUpgradeable.sol";

/**
 * @dev Interface of the ERC4626 "Tokenized Vault Standard", as defined in
 * https://eips.ethereum.org/EIPS/eip-4626[ERC-4626].
 *
 * _Available since v4.7._
 */
interface IERC4626Upgradeable is IERC20Upgradeable, IERC20MetadataUpgradeable {
    event Deposit(address indexed sender, address indexed owner, uint256 assets, uint256 shares);

    event Withdraw(
        address indexed sender, address indexed receiver, address indexed owner, uint256 assets, uint256 shares
    );

    /**
     * @dev Returns the address of the underlying token used for the Vault for accounting, depositing, and withdrawing.
     *
     * - MUST be an ERC-20 token contract.
     * - MUST NOT revert.
     */
    function asset() external view returns (address assetTokenAddress);

    /**
     * @dev Returns the total amount of the underlying asset that is “managed” by Vault.
     *
     * - SHOULD include any compounding that occurs from yield.
     * - MUST be inclusive of any fees that are charged against assets in the Vault.
     * - MUST NOT revert.
     */
    function totalAssets() external view returns (uint256 totalManagedAssets);

    /**
     * @dev Returns the amount of shares that the Vault would exchange for the amount of assets provided, in an ideal
     * scenario where all the conditions are met.
     *
     * - MUST NOT be inclusive of any fees that are charged against assets in the Vault.
     * - MUST NOT show any variations depending on the caller.
     * - MUST NOT reflect slippage or other on-chain conditions, when performing the actual exchange.
     * - MUST NOT revert.
     *
     * NOTE: This calculation MAY NOT reflect the “per-user” price-per-share, and instead should reflect the
     * “average-user’s” price-per-share, meaning what the average user should expect to see when exchanging to and
     * from.
     */
    function convertToShares(uint256 assets) external view returns (uint256 shares);

    /**
     * @dev Returns the amount of assets that the Vault would exchange for the amount of shares provided, in an ideal
     * scenario where all the conditions are met.
     *
     * - MUST NOT be inclusive of any fees that are charged against assets in the Vault.
     * - MUST NOT show any variations depending on the caller.
     * - MUST NOT reflect slippage or other on-chain conditions, when performing the actual exchange.
     * - MUST NOT revert.
     *
     * NOTE: This calculation MAY NOT reflect the “per-user” price-per-share, and instead should reflect the
     * “average-user’s” price-per-share, meaning what the average user should expect to see when exchanging to and
     * from.
     */
    function convertToAssets(uint256 shares) external view returns (uint256 assets);

    /**
     * @dev Returns the maximum amount of the underlying asset that can be deposited into the Vault for the receiver,
     * through a deposit call.
     *
     * - MUST return a limited value if receiver is subject to some deposit limit.
     * - MUST return 2 ** 256 - 1 if there is no limit on the maximum amount of assets that may be deposited.
     * - MUST NOT revert.
     */
    function maxDeposit(address receiver) external view returns (uint256 maxAssets);

    /**
     * @dev Allows an on-chain or off-chain user to simulate the effects of their deposit at the current block, given
     * current on-chain conditions.
     *
     * - MUST return as close to and no more than the exact amount of Vault shares that would be minted in a deposit
     *   call in the same transaction. I.e. deposit should return the same or more shares as previewDeposit if called
     *   in the same transaction.
     * - MUST NOT account for deposit limits like those returned from maxDeposit and should always act as though the
     *   deposit would be accepted, regardless if the user has enough tokens approved, etc.
     * - MUST be inclusive of deposit fees. Integrators should be aware of the existence of deposit fees.
     * - MUST NOT revert.
     *
     * NOTE: any unfavorable discrepancy between convertToShares and previewDeposit SHOULD be considered slippage in
     * share price or some other type of condition, meaning the depositor will lose assets by depositing.
     */
    function previewDeposit(uint256 assets) external view returns (uint256 shares);

    /**
     * @dev Mints shares Vault shares to receiver by depositing exactly amount of underlying tokens.
     *
     * - MUST emit the Deposit event.
     * - MAY support an additional flow in which the underlying tokens are owned by the Vault contract before the
     *   deposit execution, and are accounted for during deposit.
     * - MUST revert if all of assets cannot be deposited (due to deposit limit being reached, slippage, the user not
     *   approving enough underlying tokens to the Vault contract, etc).
     *
     * NOTE: most implementations will require pre-approval of the Vault with the Vault’s underlying asset token.
     */
    function deposit(uint256 assets, address receiver) external returns (uint256 shares);

    /**
     * @dev Returns the maximum amount of the Vault shares that can be minted for the receiver, through a mint call.
     * - MUST return a limited value if receiver is subject to some mint limit.
     * - MUST return 2 ** 256 - 1 if there is no limit on the maximum amount of shares that may be minted.
     * - MUST NOT revert.
     */
    function maxMint(address receiver) external view returns (uint256 maxShares);

    /**
     * @dev Allows an on-chain or off-chain user to simulate the effects of their mint at the current block, given
     * current on-chain conditions.
     *
     * - MUST return as close to and no fewer than the exact amount of assets that would be deposited in a mint call
     *   in the same transaction. I.e. mint should return the same or fewer assets as previewMint if called in the
     *   same transaction.
     * - MUST NOT account for mint limits like those returned from maxMint and should always act as though the mint
     *   would be accepted, regardless if the user has enough tokens approved, etc.
     * - MUST be inclusive of deposit fees. Integrators should be aware of the existence of deposit fees.
     * - MUST NOT revert.
     *
     * NOTE: any unfavorable discrepancy between convertToAssets and previewMint SHOULD be considered slippage in
     * share price or some other type of condition, meaning the depositor will lose assets by minting.
     */
    function previewMint(uint256 shares) external view returns (uint256 assets);

    /**
     * @dev Mints exactly shares Vault shares to receiver by depositing amount of underlying tokens.
     *
     * - MUST emit the Deposit event.
     * - MAY support an additional flow in which the underlying tokens are owned by the Vault contract before the mint
     *   execution, and are accounted for during mint.
     * - MUST revert if all of shares cannot be minted (due to deposit limit being reached, slippage, the user not
     *   approving enough underlying tokens to the Vault contract, etc).
     *
     * NOTE: most implementations will require pre-approval of the Vault with the Vault’s underlying asset token.
     */
    function mint(uint256 shares, address receiver) external returns (uint256 assets);

    /**
     * @dev Returns the maximum amount of the underlying asset that can be withdrawn from the owner balance in the
     * Vault, through a withdraw call.
     *
     * - MUST return a limited value if owner is subject to some withdrawal limit or timelock.
     * - MUST NOT revert.
     */
    function maxWithdraw(address owner) external view returns (uint256 maxAssets);

    /**
     * @dev Allows an on-chain or off-chain user to simulate the effects of their withdrawal at the current block,
     * given current on-chain conditions.
     *
     * - MUST return as close to and no fewer than the exact amount of Vault shares that would be burned in a withdraw
     *   call in the same transaction. I.e. withdraw should return the same or fewer shares as previewWithdraw if
     *   called
     *   in the same transaction.
     * - MUST NOT account for withdrawal limits like those returned from maxWithdraw and should always act as though
     *   the withdrawal would be accepted, regardless if the user has enough shares, etc.
     * - MUST be inclusive of withdrawal fees. Integrators should be aware of the existence of withdrawal fees.
     * - MUST NOT revert.
     *
     * NOTE: any unfavorable discrepancy between convertToShares and previewWithdraw SHOULD be considered slippage in
     * share price or some other type of condition, meaning the depositor will lose assets by depositing.
     */
    function previewWithdraw(uint256 assets) external view returns (uint256 shares);

    /**
     * @dev Burns shares from owner and sends exactly assets of underlying tokens to receiver.
     *
     * - MUST emit the Withdraw event.
     * - MAY support an additional flow in which the underlying tokens are owned by the Vault contract before the
     *   withdraw execution, and are accounted for during withdraw.
     * - MUST revert if all of assets cannot be withdrawn (due to withdrawal limit being reached, slippage, the owner
     *   not having enough shares, etc).
     *
     * Note that some implementations will require pre-requesting to the Vault before a withdrawal may be performed.
     * Those methods should be performed separately.
     */
    function withdraw(uint256 assets, address receiver, address owner) external returns (uint256 shares);

    /**
     * @dev Returns the maximum amount of Vault shares that can be redeemed from the owner balance in the Vault,
     * through a redeem call.
     *
     * - MUST return a limited value if owner is subject to some withdrawal limit or timelock.
     * - MUST return balanceOf(owner) if owner is not subject to any withdrawal limit or timelock.
     * - MUST NOT revert.
     */
    function maxRedeem(address owner) external view returns (uint256 maxShares);

    /**
     * @dev Allows an on-chain or off-chain user to simulate the effects of their redeemption at the current block,
     * given current on-chain conditions.
     *
     * - MUST return as close to and no more than the exact amount of assets that would be withdrawn in a redeem call
     *   in the same transaction. I.e. redeem should return the same or more assets as previewRedeem if called in the
     *   same transaction.
     * - MUST NOT account for redemption limits like those returned from maxRedeem and should always act as though the
     *   redemption would be accepted, regardless if the user has enough shares, etc.
     * - MUST be inclusive of withdrawal fees. Integrators should be aware of the existence of withdrawal fees.
     * - MUST NOT revert.
     *
     * NOTE: any unfavorable discrepancy between convertToAssets and previewRedeem SHOULD be considered slippage in
     * share price or some other type of condition, meaning the depositor will lose assets by redeeming.
     */
    function previewRedeem(uint256 shares) external view returns (uint256 assets);

    /**
     * @dev Burns exactly shares from owner and sends assets of underlying tokens to receiver.
     *
     * - MUST emit the Withdraw event.
     * - MAY support an additional flow in which the underlying tokens are owned by the Vault contract before the
     *   redeem execution, and are accounted for during redeem.
     * - MUST revert if all of shares cannot be redeemed (due to withdrawal limit being reached, slippage, the owner
     *   not having enough shares, etc).
     *
     * NOTE: some implementations will require pre-requesting to the Vault before a withdrawal may be performed.
     * Those methods should be performed separately.
     */
    function redeem(uint256 shares, address receiver, address owner) external returns (uint256 assets);
}


// File: lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/IERC20Upgradeable.sol
// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v4.6.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.0;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20Upgradeable {
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


// File: lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/IERC20MetadataUpgradeable.sol
// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts v4.4.1 (token/ERC20/extensions/IERC20Metadata.sol)

pragma solidity ^0.8.0;

import "../IERC20Upgradeable.sol";

/**
 * @dev Interface for the optional metadata functions from the ERC20 standard.
 *
 * _Available since v4.1._
 */
interface IERC20MetadataUpgradeable is IERC20Upgradeable {
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


// File: lib/openzeppelin-contracts/contracts/access/AccessControl.sol
// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v4.8.0) (access/AccessControl.sol)

pragma solidity ^0.8.0;

import "./IAccessControl.sol";
import "../utils/Context.sol";
import "../utils/Strings.sol";
import "../utils/introspection/ERC165.sol";

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
 * ```
 * bytes32 public constant MY_ROLE = keccak256("MY_ROLE");
 * ```
 *
 * Roles can be used to represent a set of permissions. To restrict access to a
 * function call, use {hasRole}:
 *
 * ```
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
 * accounts that have been granted it.
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


// File: lib/openzeppelin-contracts/contracts/access/IAccessControl.sol
// SPDX-License-Identifier: MIT
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


// File: lib/openzeppelin-contracts/contracts/security/Pausable.sol
// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v4.7.0) (security/Pausable.sol)

pragma solidity ^0.8.0;

import "../utils/Context.sol";

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
    constructor() {
        _paused = false;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        _requireNotPaused();
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
        _requirePaused();
        _;
    }

    /**
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Throws if the contract is paused.
     */
    function _requireNotPaused() internal view virtual {
        require(!paused(), "Pausable: paused");
    }

    /**
     * @dev Throws if the contract is not paused.
     */
    function _requirePaused() internal view virtual {
        require(paused(), "Pausable: not paused");
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


// File: lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol
// SPDX-License-Identifier: MIT
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
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}


// File: lib/openzeppelin-contracts/contracts/token/ERC721/IERC721.sol
// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v4.8.0) (token/ERC721/IERC721.sol)

pragma solidity ^0.8.0;

import "../../utils/introspection/IERC165.sol";

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
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon
     * a safe transfer.
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
     * - If the caller is not `from`, it must have been allowed to move this token by either {approve} or
     * {setApprovalForAll}.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon
     * a safe transfer.
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


// File: lib/openzeppelin-contracts/contracts/utils/Context.sol
// SPDX-License-Identifier: MIT
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


// File: lib/openzeppelin-contracts/contracts/utils/Strings.sol
// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v4.8.0) (utils/Strings.sol)

pragma solidity ^0.8.0;

import "./math/Math.sol";

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
     * @dev Converts an `address` with fixed length of 20 bytes to its not checksummed ASCII `string` hexadecimal
     * representation.
     */
    function toHexString(address addr) internal pure returns (string memory) {
        return toHexString(uint256(uint160(addr)), _ADDRESS_LENGTH);
    }
}


// File: lib/openzeppelin-contracts/contracts/utils/introspection/ERC165.sol
// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts v4.4.1 (utils/introspection/ERC165.sol)

pragma solidity ^0.8.0;

import "./IERC165.sol";

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


// File: lib/openzeppelin-contracts/contracts/utils/introspection/IERC165.sol
// SPDX-License-Identifier: MIT
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


// File: lib/openzeppelin-contracts/contracts/utils/math/Math.sol
// SPDX-License-Identifier: MIT
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
     * @notice Calculates floor(x * y / denominator) with full precision. Throws if result overflows a uint256 or
     * denominator == 0
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

            // Factor powers of two out of denominator and compute largest power of two divisor of denominator. Always
            // >= 1.
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

            // Use the Newton-Raphson iteration to improve the precision. Thanks to Hensel's lifting lemma, this also
            // works
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


// File: src/Base.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.18;

import { Pausable } from "openzeppelin-contracts/contracts/security/Pausable.sol";
import { AccessControl } from "openzeppelin-contracts/contracts/access/AccessControl.sol";

contract Base is AccessControl, Pausable {
    error NotManager();

    // keccak256("PAUSER_ROLE")
    bytes32 public constant PAUSER_ROLE = 0x65d7a28e3265b37a6474929f336521b332c1681b933f6cb9f3376673440d862a;

    // keccak256("MANAGER_ROLE")
    bytes32 public constant MANAGER_ROLE = 0x241ecf16d79d0f8dbfb92cbc07fe17840425976cf0667f022fe9877caa831b08;

    constructor(address _admin, address _pauser) {
        _grantRole(DEFAULT_ADMIN_ROLE, _admin);
        _grantRole(PAUSER_ROLE, _pauser);
    }

    function pause() external onlyRole(PAUSER_ROLE) {
        super._pause();
    }

    function unpause() external onlyRole(PAUSER_ROLE) {
        super._unpause();
    }

    function _grantManagerRole(address _manager) internal {
        _grantRole(MANAGER_ROLE, _manager);
    }

    function _requireManagerRole() internal view {
        if (!hasRole(MANAGER_ROLE, _msgSender())) {
            revert NotManager();
        }
    }
}


// File: src/OrderController.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.18;

import { IERC20 } from "openzeppelin-contracts/contracts/token/ERC20/IERC20.sol";
import { ITranchePool } from "./interfaces/ITranchePool.sol";
import { IOrderController, OrderType, Order } from "./interfaces/IOrderController.sol";
import { Base } from "./Base.sol";
import { IProtocolConfig } from "./interfaces/IProtocolConfig.sol";
import { IPortfolio, Status } from "./interfaces/IPortfolio.sol";

/// @title OrderController
/// @notice The OrderController is for handling deposit and withdrawal orders.
contract OrderController is IOrderController, Base {
    IPortfolio public portfolio;
    IERC20 public token;
    ITranchePool public tranche;
    uint256 public dust;

    uint256 public constant NULL = 0;
    uint256 public constant HEAD = 0;

    uint256 internal nextOrderId = 1;
    mapping(uint256 => Order) internal orders;

    constructor(
        ITranchePool tranche_,
        IProtocolConfig protocolConfig_,
        IPortfolio portfolio_,
        uint256 dust_,
        address manager_
    )
        Base(protocolConfig_.protocolAdmin(), protocolConfig_.pauser())
    {
        _grantManagerRole(manager_);
        _setToken(IERC20(tranche_.asset()));
        _setTranche(tranche_);
        _setPortfolio(portfolio_);
        _setDust(dust_);
    }

    /// @inheritdoc IOrderController
    function deposit(uint256 tokenAmount, uint256 iterationLimit) external whenNotPaused {
        _validateInputs(msg.sender, tokenAmount, OrderType.DEPOSIT);

        _cancelOrder(msg.sender);

        (uint256 remainingTokenAmount, uint256 iterations) = _processWithdrawOrder(tokenAmount, iterationLimit);

        if (iterations == iterationLimit) return;

        // double check for the case when dust = 0
        if (remainingTokenAmount > 0 && remainingTokenAmount >= dust) {
            _createOrder(msg.sender, remainingTokenAmount, OrderType.DEPOSIT);
        }
    }

    /// @inheritdoc IOrderController
    function withdraw(uint256 trancheAmount, uint256 iterationLimit) external whenNotPaused {
        _validateInputs(msg.sender, trancheAmount, OrderType.WITHDRAW);

        _cancelOrder(msg.sender);

        (uint256 remainingTrancheAmount, uint256 iterations) = _processDepositOrder(trancheAmount, iterationLimit);

        if (iterations == iterationLimit) return;

        // double check for the case when dust = 0
        if (remainingTrancheAmount > 0 && remainingTrancheAmount >= dust) {
            _createOrder(msg.sender, remainingTrancheAmount, OrderType.WITHDRAW);
        }
    }

    /// @inheritdoc IOrderController
    function cancelOrder() external {
        _cancelOrder(msg.sender);
    }

    /// @inheritdoc IOrderController
    function cancelDustOrder(uint256 orderId) external {
        Order storage order = orders[orderId];

        if (order.user == address(0)) {
            revert InvalidOrderId();
        }

        if (order.amount >= dust) {
            revert InvalidAmount();
        }

        _removeOrder(orderId);
    }

    function setDust(uint256 _dust) external {
        _requireManagerRole();
        _setDust(_dust);
    }

    /// @inheritdoc IOrderController
    function expectedTokenAmount(uint256 trancheAmount) public view virtual returns (uint256) {
        return tranche.convertToAssets(trancheAmount);
    }

    /// @inheritdoc IOrderController
    function expectedTrancheAmount(uint256 tokenAmount) public view virtual returns (uint256) {
        return tranche.convertToShares(tokenAmount);
    }

    function expectedTokenAmountCeil(uint256 trancheAmount) internal view virtual returns (uint256) {
        return tranche.convertToAssetsCeil(trancheAmount);
    }

    function expectedTrancheAmountCeil(uint256 tokenAmount) internal view virtual returns (uint256) {
        return tranche.convertToSharesCeil(tokenAmount);
    }

    /// @inheritdoc IOrderController
    function getUserOrder(address user) public view returns (Order memory) {
        if (_isStatus(Status.SeniorClosed) || _isStatus(Status.EquityClosed)) {
            return Order(0, 0, 0, 0, 0, address(0), OrderType.NONE);
        }

        uint256 currentOrderId = orders[HEAD].next;
        Order memory order = Order(0, 0, 0, 0, 0, address(0), OrderType.NONE);

        while (currentOrderId != NULL) {
            order = orders[currentOrderId];

            if (order.user == user) {
                return order;
            }

            currentOrderId = order.next;
        }

        // No order matched
        return Order(0, 0, 0, 0, 0, address(0), OrderType.NONE);
    }

    /// @inheritdoc IOrderController
    function getOrders() public view returns (Order[] memory) {
        if (_isStatus(Status.SeniorClosed) || _isStatus(Status.EquityClosed)) return new Order[](0);

        uint256 orderCount = getOrderCount();
        Order[] memory ordersArray = new Order[](orderCount);
        uint256 currentIndex = 0;
        uint256 currentOrderId = orders[HEAD].next;

        while (currentOrderId != NULL) {
            Order storage currentOrder = orders[currentOrderId];
            ordersArray[currentIndex] = currentOrder;
            currentOrderId = currentOrder.next;
            currentIndex++;
        }

        return ordersArray;
    }

    /// @inheritdoc IOrderController
    function getValidOrders() public view returns (Order[] memory, OrderType) {
        if (_isStatus(Status.SeniorClosed) || _isStatus(Status.EquityClosed)) return (new Order[](0), OrderType.NONE);

        (uint256 orderCount, OrderType orderType) = getValidOrderCount();
        Order[] memory ordersArray = new Order[](orderCount);
        uint256 currentIndex = 0;
        uint256 currentOrderId = orders[HEAD].next;

        while (currentOrderId != NULL) {
            Order storage currentOrder = orders[currentOrderId];

            if (_validateOrder(currentOrder, orderType)) {
                ordersArray[currentIndex] = currentOrder;
                currentIndex++;
            }

            currentOrderId = currentOrder.next;
        }

        return (ordersArray, orderType);
    }

    /// @inheritdoc IOrderController
    function getOrderCount() public view returns (uint256) {
        if (_isStatus(Status.SeniorClosed) || _isStatus(Status.EquityClosed)) return 0;

        uint256 count = 0;
        uint256 currentOrderId = orders[HEAD].next;

        while (currentOrderId != NULL) {
            count++;
            currentOrderId = orders[currentOrderId].next;
        }

        return count;
    }

    /// @inheritdoc IOrderController
    function getValidOrderCount() public view returns (uint256 count, OrderType orderType) {
        if (_isStatus(Status.SeniorClosed) || _isStatus(Status.EquityClosed)) return (0, OrderType.NONE);

        orderType = currentOrderType();
        uint256 currentOrderId = orders[HEAD].next;

        while (currentOrderId != NULL) {
            if (_validateOrder(orders[currentOrderId], orderType)) count++;
            currentOrderId = orders[currentOrderId].next;
        }

        return (count, orderType);
    }

    /// @inheritdoc IOrderController
    function currentOrderType() public view returns (OrderType) {
        uint256 currentOrderId = orders[HEAD].next;

        if (currentOrderId != NULL) {
            return orders[currentOrderId].orderType;
        } else {
            return OrderType.NONE;
        }
    }

    function _processWithdrawOrder(uint256 tokenAmount, uint256 iterationLimit) internal returns (uint256, uint256) {
        uint256 currentId = orders[HEAD].next;
        uint256 iterations = 0;

        while (currentId != NULL && tokenAmount > 0 && iterations < iterationLimit) {
            Order memory order = orders[currentId];

            if (order.orderType == OrderType.WITHDRAW) {
                if (_validateOrder(order, OrderType.WITHDRAW)) {
                    // round up to calculate the token amount for msg.sender to pay
                    uint256 orderTokenAmount = expectedTokenAmountCeil(order.amount);
                    if (orderTokenAmount <= tokenAmount) {
                        tokenAmount -= orderTokenAmount;
                        _executeWithdrawOrder(orderTokenAmount, order.amount, order.user);
                        _removeOrder(currentId);
                        currentId = order.next;
                    } else {
                        // round down to calculate the tranche amount for msg.sender to receive
                        uint256 trancheAmountToWithdraw = expectedTrancheAmount(tokenAmount);
                        orders[currentId].amount -= trancheAmountToWithdraw;
                        _executeWithdrawOrder(tokenAmount, trancheAmountToWithdraw, order.user);
                        tokenAmount = 0;
                    }
                } else {
                    _removeOrder(currentId);
                    currentId = order.next;
                }
                iterations++;
            } else {
                currentId = order.next;
            }
        }

        return (tokenAmount, iterations);
    }

    function _processDepositOrder(uint256 trancheAmount, uint256 iterationLimit) internal returns (uint256, uint256) {
        uint256 currentId = orders[HEAD].next;
        uint256 iterations = 0;

        while (currentId != NULL && trancheAmount > 0 && iterations < iterationLimit) {
            Order memory order = orders[currentId];

            if (order.orderType == OrderType.DEPOSIT) {
                if (_validateOrder(order, OrderType.DEPOSIT)) {
                    // round up to calculate the tranche amount for msg.sender to pay
                    uint256 orderTrancheAmount = expectedTrancheAmountCeil(order.amount);
                    if (orderTrancheAmount <= trancheAmount) {
                        trancheAmount -= orderTrancheAmount;
                        _executeDepositOrder(order.amount, orderTrancheAmount, order.user);
                        _removeOrder(currentId);
                        currentId = order.next;
                    } else {
                        // round down to calculate the tranche amount for msg.sender to receive
                        uint256 tokenAmountToDeposit = expectedTokenAmount(trancheAmount);
                        orders[currentId].amount -= tokenAmountToDeposit;
                        _executeDepositOrder(tokenAmountToDeposit, trancheAmount, order.user);
                        trancheAmount = 0;
                    }
                } else {
                    _removeOrder(currentId);
                    currentId = order.next;
                }
                iterations++;
            } else {
                currentId = order.next;
            }
        }

        return (trancheAmount, iterations);
    }

    function _createOrder(address user, uint256 amount, OrderType orderType) internal {
        uint256 orderId = nextOrderId++;
        uint256 prevId = orders[HEAD].prev;

        orders[orderId] = Order(orderId, amount, prevId, NULL, block.timestamp, user, orderType);
        orders[prevId].next = orderId;
        orders[HEAD].prev = orderId;
    }

    function _removeOrder(uint256 orderId) internal {
        Order storage order = orders[orderId];

        orders[order.prev].next = order.next;
        orders[order.next].prev = order.prev;

        delete orders[orderId];
    }

    function _cancelOrder(address user) internal {
        uint256 currentId = orders[HEAD].next;

        while (currentId != NULL) {
            Order memory order = orders[currentId];

            if (order.user == user) {
                _removeOrder(currentId);
                break;
            }

            currentId = order.next;
        }
    }

    // TO DEPRECATED: only used in tests
    function _executeDepositOrder(uint256 tokenAmount, address user) internal returns (uint256 trancheAmount) {
        trancheAmount = expectedTrancheAmount(tokenAmount);

        // Transfer USDT from depositor to the user who requested the withdrawal
        token.transferFrom(user, msg.sender, tokenAmount);

        // Transfer tranch from the user who requested the withdrawal to the depositor
        tranche.transferFrom(msg.sender, user, trancheAmount);
    }

    function _executeDepositOrder(uint256 tokenAmount, uint256 trancheAmount, address user) internal {
        // Transfer USDT from depositor to the user who requested the withdrawal
        token.transferFrom(user, msg.sender, tokenAmount);

        // Transfer tranch from the user who requested the withdrawal to the depositor
        tranche.transferFrom(msg.sender, user, trancheAmount);
    }

    function _executeWithdrawOrder(uint256 tokenAmount, uint256 trancheAmount, address user) internal {
        // Transfer USDT from the user who requested the deposit to the withdrawer
        token.transferFrom(msg.sender, user, tokenAmount);

        // Transfer tranch from the withdrawer to the user who requested the deposit
        tranche.transferFrom(user, msg.sender, trancheAmount);
    }

    // TO DEPRECATED: only used in tests
    function _executeWithdrawOrder(uint256 trancheAmount, address user) internal returns (uint256 tokenAmount) {
        tokenAmount = expectedTokenAmount(trancheAmount);

        // Transfer USDT from the user who requested the deposit to the withdrawer
        token.transferFrom(msg.sender, user, tokenAmount);

        // Transfer tranch from the withdrawer to the user who requested the deposit
        tranche.transferFrom(user, msg.sender, trancheAmount);
    }

    // set token
    function _setToken(IERC20 _token) internal {
        token = _token;
    }

    // set tranche
    function _setTranche(ITranchePool _tranche) internal {
        tranche = _tranche;
    }

    function _setPortfolio(IPortfolio _portfolio) internal {
        portfolio = _portfolio;
    }

    function _setDust(uint256 _dust) internal {
        dust = _dust;
        emit DustUpdated(_dust);
    }

    /**
     * @notice check user token allowance for corresponding order type
     */
    function _validateOrder(Order memory order, OrderType orderType) internal view returns (bool) {
        return
            _checkAllowance(order.user, order.amount, orderType) && _checkBalance(order.user, order.amount, orderType);
    }

    function _validateInputs(address user, uint256 amount, OrderType orderType) internal view {
        // double check for the case when dust = 0
        if (amount == 0 || amount < dust) revert InvalidAmount();
        if (!_checkBalance(user, amount, orderType)) revert InsufficientBalance();
        if (!_checkAllowance(user, amount, orderType)) revert InsufficientAllowance();
        if (_isStatus(Status.SeniorClosed) || _isStatus(Status.EquityClosed)) revert PortfolioClosed();
    }

    function _checkBalance(address user, uint256 amount, OrderType orderType) internal view returns (bool) {
        // check user balance
        if (orderType == OrderType.DEPOSIT) {
            return token.balanceOf(user) >= amount;
        } else {
            return tranche.balanceOf(user) >= amount;
        }
    }

    function _checkAllowance(address user, uint256 amount, OrderType orderType) internal view returns (bool) {
        // check user token allowance for corresponding order type
        if (orderType == OrderType.DEPOSIT) {
            return token.allowance(user, address(this)) >= amount;
        } else {
            return tranche.allowance(user, address(this)) >= amount;
        }
    }

    function _isStatus(Status allowedStatus) internal view virtual returns (bool) {
        Status status = portfolio.status();

        return status == allowedStatus;
    }
}


// File: src/interfaces/ICurrencyConverter.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.18;

interface ICurrencyConverter {
    error InvalidTargetDecimals();

    /// @notice set the exchange rate between WON and USD
    /// @dev the caller is contract keeper
    function setExchangeRate(uint256 _exchangeRate) external;

    /// @notice get the exchange rate between WON and USD
    function getExchangeRate() external returns (uint256, uint256);

    /// @param usdAmount the amount of USD received
    /// @dev constructor guarantees 18 = won decimals >= usd decimals
    /// @return wonAmount the amount of WON to give
    function convertFromUSD(uint256 usdAmount) external view returns (uint256 wonAmount);

    /// @param wonAmount the amount of WON received
    /// @dev constructor guarantees 18 = won decimals >= usd decimals
    /// @return usdAmount the amount of USD to give
    function convertToUSD(uint256 wonAmount) external view returns (uint256 usdAmount);
}


// File: src/interfaces/IDepositController.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.18;

interface IDepositController {
    /// @notice Handle deposit of assets and distribute shares and fees accordingly
    /// @param sender The address of the sender depositing assets
    /// @param assets The amount of assets being deposited
    /// @param receiver The address receiving the shares
    /// @return shares The amount of shares minted for the deposit
    /// @return fees The amount of fees collected during the deposit
    function onDeposit(
        address sender,
        uint256 assets,
        address receiver
    )
        external
        returns (uint256 shares, uint256 fees);

    /// @notice Handle minting of shares and distribute assets and fees accordingly
    /// @param sender The address of the sender minting shares
    /// @param shares The amount of shares being minted
    /// @param receiver The address receiving the assets
    /// @return assets The amount of assets corresponding to the minted shares
    /// @return fees The amount of fees collected during the minting
    function onMint(address sender, uint256 shares, address receiver) external returns (uint256 assets, uint256 fees);

    /// @notice Preview the number of shares that will be minted for a given amount of assets
    /// @param assets The amount of assets to be deposited
    /// @return shares The estimated number of shares to be minted
    function previewDeposit(uint256 assets) external view returns (uint256 shares);

    /// @notice Preview the total amount of assets (including fees) for a given number of shares
    /// @param shares The amount of shares to be minted
    /// @return assets The estimated total amount of assets (including fees) for the given shares
    function previewMint(uint256 shares) external view returns (uint256 assets);

    /// @notice Calculate the maximum deposit amount based on the given ceiling
    /// @param receiver The address receiving the shares
    /// @param ceiling The maximum allowed total assets
    /// @return assets The maximum deposit amount under the given ceiling
    function maxDeposit(address receiver, uint256 ceiling) external view returns (uint256 assets);

    /// @notice Calculate the maximum number of shares that can be minted based on the given ceiling
    /// @param receiver The address receiving the assets
    /// @param ceiling The maximum allowed total assets
    /// @return shares The maximum number of shares that can be minted under the given ceiling
    function maxMint(address receiver, uint256 ceiling) external view returns (uint256 shares);

    /// @notice Set the deposit fee rate
    /// @param _depositFeeRate The new deposit fee rate
    function setDepositFeeRate(uint256 _depositFeeRate) external;
}


// File: src/interfaces/IFixedInterestBulletLoans.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.18;

import { IPortfolio } from "./IPortfolio.sol";
import { IERC20 } from "openzeppelin-contracts/contracts/token/ERC20/IERC20.sol";
import { IERC721 } from "openzeppelin-contracts/contracts/token/ERC721/IERC721.sol";
import "openzeppelin-contracts/contracts/access/IAccessControl.sol";
import "./ICurrencyConverter.sol";

enum FixedInterestBulletLoanStatus {
    Created, // after create, before start
    Started, // after accept loan
    Repaid, // after repay loan
    Canceled, // after create and canceled
    Defaulted
}

interface IFixedInterestBulletLoans is IAccessControl {
    error NotPortfolio();
    error PortfolioAlreadySet();
    error NotSuitableLoanStatus();
    error NotEqualRepayAmount();
    /// @notice Emitted when a new loan is created

    event Created(uint256 indexed loanId);
    /// @notice Emitted when a loan is started
    event Started(uint256 indexed loanId);

    /// @notice Emitted when a loan is repaid
    event Repaid(uint256 indexed loanId, uint256 amount);

    /// @notice Emitted when a defaulted loan is repaid
    event RepayDefaulted(uint256 indexed loanId, uint256 amount);

    /// @notice Emitted when a loan is marked as defaulted
    event Defaulted(uint256 indexed loanId);

    /// @notice Emitted when a loan is canceled
    event Canceled(uint256 indexed loanId);

    /// @notice Emitted when a loan status is changed
    event LoanStatusChanged(uint256 indexed loanId, FixedInterestBulletLoanStatus newStatus);

    struct LoanMetadata {
        uint256 krwPrincipal;
        uint256 usdPrincipal;
        uint256 usdRepaid;
        uint256 interestRate; // Use basis point i.e. 10000 = 100%
        address recipient;
        address collateral;
        uint256 collateralId;
        uint256 startDate;
        uint256 duration;
        FixedInterestBulletLoanStatus status; // uint8
        IERC20 asset;
    }

    struct IssueLoanInputs {
        uint256 krwPrincipal;
        uint256 interestRate; // Use basis point i.e. 10000 = 100%
        address recipient;
        address collateral;
        uint256 collateralId;
        uint256 duration;
        IERC20 asset;
    }

    /// @notice Issue a new loan with the specified parameters
    /// @param loanInputs The input parameters for the new loan
    /// @return The ID of the newly created loan
    function issueLoan(IssueLoanInputs calldata loanInputs) external returns (uint256);

    /// @notice Start a loan with the specified ID
    /// @dev The loan must be in the Created status and the caller must be the portfolio contract
    /// @param loanId The ID of the loan to start
    /// @return principal The borrowed principal amount of the loan in USD
    function startLoan(uint256 loanId) external returns (uint256 principal);

    /// @notice Retrieve loan data for a specific loan ID
    /// @param loanId The ID of the loan
    /// @return The loan metadata as a LoanMetadata struct
    function loanData(uint256 loanId) external view returns (LoanMetadata memory);

    /// @notice Repay a loan with the specified ID and amount
    /// @dev The loan must be in the Started status and the caller must be the portfolio contract
    /// @param loanId The ID of the loan to repay
    /// @param usdAmount The amount to repay in USD
    function repayLoan(uint256 loanId, uint256 usdAmount) external;

    /// @notice Repay a defaulted loan with the specified ID and amount
    /// @dev The loan must be in the Defaulted status and the caller must be the portfolio contract
    /// @param loanId The ID of the defaulted loan to repay
    /// @param usdAmount The amount to repay in USD
    function repayDefaultedLoan(uint256 loanId, uint256 usdAmount) external;

    /// @notice Cancel a loan with the specified ID
    /// @dev The loan must be in the Created status and the caller must be the portfolio contract
    /// @param loanId The ID of the loan to cancel
    function cancelLoan(uint256 loanId) external;

    /// @notice Mark a loan as defaulted with the specified ID
    /// @dev The loan must be in the Started status and the caller must be the portfolio contract
    /// @param loanId The ID of the loan to mark as defaulted
    function markLoanAsDefaulted(uint256 loanId) external;

    /// @notice Get the recipient address of a loan with the specified ID
    /// @param loanId The ID of the loan
    /// @return The recipient address
    function getRecipient(uint256 loanId) external view returns (address);

    /// @notice Get the status of a loan with the specified ID
    /// @param loanId The ID of the loan
    /// @return The loan status as a FixedInterestBulletLoanStatus enum value
    function getStatus(uint256 loanId) external view returns (FixedInterestBulletLoanStatus);

    /// @notice Check if a loan with the specified ID is overdue
    /// @param loanId The ID of the loan
    /// @return A boolean value indicating if the loan is overdue
    function isOverdue(uint256 loanId) external view returns (bool);

    /// @notice Get the total number of loans
    /// @return The total number of loans
    function getLoansLength() external view returns (uint256);

    /// @notice Calculate the loan value at the current timestamp
    /// @param loanId The ID of the loan
    /// @return loan value. It remains the same after loan.endDate
    function currentUsdValue(uint256 loanId) external view returns (uint256);

    /// @notice Calculate the amount to repay. It is the same regardless of the repaying time.
    /// @param loanId The ID of the loan
    /// @return repayment amount
    function expectedUsdRepayAmount(uint256 loanId) external view returns (uint256);

    /// @notice Set the portfolio contract
    /// @dev The caller must have the manager role
    /// @param _portfolio The address of the portfolio contract
    function setPortfolio(IPortfolio _portfolio) external;

    /// @notice Set the currency converter contract
    /// @dev The caller must have the manager role
    /// @param _currencyConverter The address of the currency converter contract
    function setCurrencyConverter(ICurrencyConverter _currencyConverter) external;
}


// File: src/interfaces/ILoansManager.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.18;

import { IFixedInterestBulletLoans, FixedInterestBulletLoanStatus } from "./IFixedInterestBulletLoans.sol";

struct AddLoanParams {
    address recipient;
    uint256 krwPrincipal;
    uint256 interestRate;
    address collateral;
    uint256 collateralId;
    uint256 duration;
}

interface ILoansManager {
    /// @notice Emitted when a loan is added
    event LoanAdded(uint256 indexed loanId);

    /// @notice Emitted when a loan is funded
    event LoanFunded(uint256 indexed loanId);

    /// @notice Emitted when a loan is repaid
    event LoanRepaid(uint256 indexed loanId, uint256 amount);

    /// @notice Emitted when a loan is canceled
    event LoanCanceled(uint256 indexed loanId);

    /// @notice Emitted when a loan is defaulted
    event LoanDefaulted(uint256 indexed loanId);

    /// @notice Emitted when a active loan is removed
    event ActiveLoanRemoved(uint256 indexed loanId, FixedInterestBulletLoanStatus indexed status);

    /// @notice Thrown when loanId is not valid or doesn't exist in the issuedLoanIds mapping
    error InvalidLoanId();
    /// @notice Thrown when there are insufficient funds in the contract to fund a loan
    error InSufficientFund();
    /// @notice Thrown when the recipient address is invalid
    error InvalidRecipient();

    /// @param loanId Loan id
    /// @return Whether a loan with the given loanId was issued by this contract
    function issuedLoanIds(uint256 loanId) external view returns (bool);

    /// @return FixedInterestBulletLoans contract
    function fixedInterestBulletLoans() external view returns (IFixedInterestBulletLoans);
}


// File: src/interfaces/IOrderController.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.18;

enum OrderType {
    NONE,
    DEPOSIT,
    WITHDRAW
}

struct Order {
    uint256 id;
    uint256 amount;
    uint256 prev;
    uint256 next;
    uint256 createdAt;
    address user;
    OrderType orderType;
}

interface IOrderController {
    // Custom errors for _validateInputs
    error InvalidAmount();
    error InvalidOrderType();
    error InvalidOrderId();
    error InsufficientBalance();
    error InsufficientAllowance();
    error PortfolioClosed();

    event DustUpdated(uint256 newDust);

    /// @notice Allows a user to deposit tokens and create a DEPOSIT order. It doesn't gurantee deposit
    /// @dev The order can be executed if there's a matching withdrawal request. The caller should approve tokens prior
    /// to calling this function.
    /// @param tokenAmount The amount of tokens to deposit.
    /// @param iterationLimit The maximum number of orders to process in a single call.
    function deposit(uint256 tokenAmount, uint256 iterationLimit) external;

    /// @notice Allows a user to withdraw tranches and create a WITHDRAW order.
    /// @dev The order can be executed if there's a matching deposit request. The caller should approve tranches prior
    /// to calling this function.
    /// @param trancheAmount The amount of tranches to withdraw.
    /// @param iterationLimit The maximum number of orders to process in a single call.
    function withdraw(uint256 trancheAmount, uint256 iterationLimit) external;

    /// @notice Allows a user to cancel their pending order.
    /// @dev This can be called by the user who placed the order only.
    function cancelOrder() external;

    /// @notice Allows any users to cancel dust order.
    /// @param orderId The order id to cancel.
    function cancelDustOrder(uint256 orderId) external;

    /// @notice Calculate the expected token amount for a given tranche amount.
    /// @param trancheAmount The amount of tranches to convert.
    /// @return The expected token amount.
    function expectedTokenAmount(uint256 trancheAmount) external view returns (uint256);

    /// @notice Calculate the expected tranche amount for a given token amount.
    /// @param tokenAmount The amount of tokens to convert.
    /// @return The expected tranche amount.
    function expectedTrancheAmount(uint256 tokenAmount) external view returns (uint256);

    /// @notice Return the type of the current order in the linked list of orders
    function currentOrderType() external view returns (OrderType);

    /// @notice Return the count of valid orders and the current order type.
    /// @return count The count of valid orders.
    /// @return orderType The type of the current order.
    function getValidOrderCount() external view returns (uint256 count, OrderType orderType);

    /// @notice Return the valid orders and the current order type.
    /// @return orders The valid orders.
    /// @return orderType The type of the order.
    function getValidOrders() external view returns (Order[] memory, OrderType);

    /// @notice Return the count of all orders.
    /// @return The count of all orders.
    function getOrderCount() external view returns (uint256);

    /// @notice Return all orders.
    /// @return The orders.
    function getOrders() external view returns (Order[] memory);

    /// @notice Return the order of the given user.
    /// @param user The user address.
    /// @return The order.
    function getUserOrder(address user) external view returns (Order memory);

    /// @notice Return the min amount of an order.
    /// @return The minimum value.
    function dust() external view returns (uint256);
}


// File: src/interfaces/IPortfolio.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.18;

import { ITranchePool } from "./ITranchePool.sol";
import { IAccessControl } from "openzeppelin-contracts/contracts/access/IAccessControl.sol";
import { AddLoanParams, ILoansManager } from "./ILoansManager.sol";

enum Status {
    Preparation,
    Live,
    Stopped,
    SeniorClosed,
    EquityClosed
}

struct TrancheData {
    /// @dev
    uint256 initialAssets;
    /// @dev The APR expected to be granted at the end of the portfolio Live phase (in BPS)
    uint128 targetApr;
    /// @dev The minimum required ratio of the sum of subordinate tranches assets to the tranche assets (in BPS)
    /// It must be floored. e.g. 0.42857 -> 4285 (decimal 4)
    uint128 minSubordinateRatio;
}

struct TrancheInitData {
    /// @dev Address of the tranche vault
    ITranchePool tranche;
    /// @dev The APR expected to be granted at the end of the portfolio Live phase (in BPS)
    uint128 targetApr;
    /// @dev The minimum required ratio of the sum of subordinate tranches assets to the tranche assets (in BPS)
    uint128 minSubordinateRatio;
}

interface IPortfolio {
    error NotTranche();
    error NotGovernance();
    error NotManagerOrCollateralOwner();
    error EquityAprNotZero();
    error ActiveLoansExist();
    error AlreadyStarted();
    error NotReadyToCloseSenior();
    error NotFullyFunded();
    error NotReadyToCloseEquity();
    error AddLoanNotAllowed();
    error FundLoanNotAllowed();
    error RepayLoanNotAllowed();
    error RepayDefaultedLoanNotAllowed();

    error StopPortfolioWithInvalidStatus();
    error StopPortfolioWithInvalidValues();

    error RestartPortfolioWithInvalidStatus();
    error RestartPortfolioWithInvalidValues();
    error RestartPortfolioOverDuration();

    error DepositInvalidStatus();

    event PortfolioStatusChanged(Status status);

    /// @notice Returns the address of the tranche pool contract
    function tranches(uint256 index) external view returns (ITranchePool);

    /// @notice Returns current portfolio status
    function status() external view returns (Status);

    /// @notice Returns the timestamp when the portfolio started
    function startTimestamp() external view returns (uint40);

    /// @notice calculate each tranche values based only on the current assets.
    /// @dev It does not take account of loans value.
    /// @return Array of current tranche values
    function calculateWaterfall() external view returns (uint256[] memory);

    function calculateWaterfallForTranche(uint256 waterfallIndex) external view returns (uint256);

    /// @notice calculate each tranche values given the (current assets + loans value).
    function calculateWaterfallWithLoans() external view returns (uint256[] memory);

    function calculateWaterfallWithLoansForTranche(uint256 waterfallIndex) external view returns (uint256);

    /// @notice calculate the total value of all active loans in the contract.
    function loansValue() external view returns (uint256);

    /// @notice get token balance of this contract
    function getTokenBalance() external view returns (uint256);

    /// @notice get assumed current values
    /// @return equityValue The value of equity tranche
    /// @return fixedRatePoolValue The value of fixed rate pool tranches
    /// @return overdueValue The value of overdue loans
    function getAssumedCurrentValues()
        external
        view
        returns (uint256 equityValue, uint256 fixedRatePoolValue, uint256 overdueValue);

    /// @notice Starts the portfolio to issue loans.
    /// @dev
    /// - changes the state to Live
    /// - gathers assets to the portfolio from every tranche.
    /// @custom:role - manager
    function start() external;

    /// @notice Allow the senior tranche to withdraw.
    /// @dev
    /// - changes the state to SeniorClosed
    /// - Distribute the remaining assets to the senior tranche.
    /// @custom:role - manager
    function closeSenior() external;

    /// @notice Allow the equity tranche to withdraw.
    /// @dev
    /// - changes the state to EquityClosed
    /// - Distribute the remaining assets to the equity tranche.
    /// @custom:role - manager
    function closeEquity() external;

    /// @notice Create loan
    /// @param params Loan params
    /// @custom:status - Preparation, Live
    /// @custom:role - manager || collateral owner
    function addLoan(AddLoanParams calldata params) external returns (uint256 loanId);

    /// @notice Fund the loan
    /// @param loanId Loan id
    /// @custom:status - Live
    /// @custom:role - governance
    function fundLoan(uint256 loanId) external returns (uint256 principal);

    /// @notice Repay the loan
    /// @param loanId Loan id
    /// @custom:status - Live || SeniorClosed || Stopped
    /// @custom:role - all
    function repayLoan(uint256 loanId) external returns (uint256 amount);

    /// @notice Repay the loan
    /// @param loanId Loan id
    /// @param amount amount
    /// @custom:status - Live || SeniorClosed || Stopped
    /// @custom:role - manager
    function repayDefaultedLoan(uint256 loanId, uint256 amount) external;

    /// @notice Cancel the loan
    /// @param loanId Loan id
    /// @custom:role - manager
    function cancelLoan(uint256 loanId) external;

    /// @notice Cancel the loan
    /// @param loanId Loan id
    /// @custom:status - All (as long as the loan exists)
    /// @custom:role - manager
    function markLoanAsDefaulted(uint256 loanId) external;

    /// @notice Increase the current token balance of the portfolio
    /// @dev This function is used to track the token balance of the portfolio. Only the tranche pool contract can call
    /// @param amount Amount to increase
    /// @custom:role - tranchePool
    function increaseTokenBalance(uint256 amount) external;
}


// File: src/interfaces/IProtocolConfig.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.18;

interface IProtocolConfig {
    error NotProtocolAdmin();

    event ProtocolAdminChanged(address protocolAdmin);
    event PauserChanged(address pauser);

    function protocolAdmin() external view returns (address);

    function pauser() external view returns (address);
}


// File: src/interfaces/ITranchePool.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.18;

import { IPortfolio } from "./IPortfolio.sol";
import { IDepositController } from "./IDepositController.sol";
import { IWithdrawController } from "./IWithdrawController.sol";
import { IERC4626Upgradeable } from "openzeppelin-contracts-upgradeable/contracts/interfaces/IERC4626Upgradeable.sol";

interface ITranchePool is IERC4626Upgradeable {
    error PortfolioAlreadySet();
    error NotPortfolio();
    error PortfolioPaused();
    error MaxDepositExceeded();
    error MaxMintExceeded();
    error MaxWithdrawExceeded();
    error MaxRedeemExceeded();
    error InsufficientAllowance();

    event DepositControllerChanged(IDepositController indexed controller);
    event DepositFeePaid(address indexed receiver, uint256 fee);

    event WithdrawControllerChanged(IWithdrawController indexed controller);
    event WithdrawFeePaid(address indexed receiver, uint256 fee);

    event CeilingChanged(uint256 newCeiling);
    event FeeReceiverChanged(address newFeeReceiver);

    /// @notice the associated portfolio for this tranche pool
    /// @return portfolio The associated portfolio
    function portfolio() external view returns (IPortfolio portfolio);

    /// @notice the ceiling for this tranche pool
    /// @return ceiling The ceiling value
    function ceiling() external view returns (uint256 ceiling);

    /// @notice the fee receiver for this tranche pool
    /// @return The receiver address
    function feeReceiver() external view returns (address);

    /// @notice available liquidity tracked by the token balance tracker
    /// @return The available liquidity
    function availableLiquidity() external view returns (uint256);

    /// @notice the waterfall index for this tranche pool. 0 is equity and 1 is senior
    /// @return waterfallIndex The waterfall index
    function waterfallIndex() external view returns (uint256 waterfallIndex);

    /// @notice Converts a given amount of assets to shares (rounded up)
    /// @param assets The amount of assets to convert
    /// @return shares The equivalent amount of shares
    function convertToSharesCeil(uint256 assets) external view returns (uint256 shares);

    /// @notice Converts a given amount of shares to assets (rounded up)
    /// @param shares The amount of shares to convert
    /// @return assets The equivalent amount of assets
    function convertToAssetsCeil(uint256 shares) external view returns (uint256 assets);

    /// @notice Sets the portfolio for this tranche pool
    /// @param _portfolio The new portfolio to set
    /// @custom:role - manager
    function setPortfolio(IPortfolio _portfolio) external;

    /// @notice Sets the deposit controller for this tranche pool
    /// @param newController The new deposit controller to set
    /// @custom:role - manager
    function setDepositController(IDepositController newController) external;

    /// @notice Sets the withdraw controller for this tranche pool
    /// @param newController The new withdraw controller to set
    /// @custom:role - manager
    function setWithdrawController(IWithdrawController newController) external;

    /// @notice Sets the fee receiver for the tranche pool
    /// @param newFeeReceiver The new ceiling to set
    /// @custom:role - manager
    function setFeeReceiver(address newFeeReceiver) external;

    /// @notice Sets the ceiling for this tranche pool
    /// @param newCeiling The new ceiling to set
    /// @custom:role - manager
    function setCeiling(uint256 newCeiling) external;

    /// @notice Called when the portfolio starts
    /// @return The balance of the tranche pool
    function onPortfolioStart() external returns (uint256);

    /// @notice Increases the token balance of the tranche pool by the given amount
    /// @param amount The amount to increase the token balance by
    function increaseTokenBalance(uint256 amount) external;
}


// File: src/interfaces/IWithdrawController.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.18;

/// @title WithdrawController
/// @notice This contract manages the withdrawal and redemption fees for users in the tranches.
/// @dev The contract calculates fees, handles withdrawal, and allows setting of withdrawal fee rates.
interface IWithdrawController {
    // @return Rate of the withdraw fee in Basis point, i.e. decimal 4.
    function withdrawFeeRate() external view returns (uint256);

    /// @dev A user cannot withdraw in the live status.
    /// @return assets The max amount of assets that the user can withdraw.
    function maxWithdraw(address owner) external view returns (uint256 assets);

    /// @dev A user cannot redeem in the live status.
    /// @return shares The max amount of shares that the user can burn to withdraw assets.
    function maxRedeem(address owner) external view returns (uint256 shares);

    /// @notice Preview the amount of shares to burn to withdraw the given amount of assets.
    /// @dev    It always rounds up the result. e.g. 3/4 -> 1.
    /// @param  assets The amount of assets to withdraw.
    /// @return  shares The amount of shares to burn.
    function previewWithdraw(uint256 assets) external view returns (uint256 shares);

    /// @notice Preview the amount of assets to receive after burning the given amount of shares.
    /// @param  shares The amount of shares to burn.
    /// @return assets The amount of assets to receive.
    function previewRedeem(uint256 shares) external view returns (uint256 assets);

    /// @notice Executes the withdrawal process, returning the amount of shares to burn and fees.
    /// @dev The sender and owner parameters are not used in this implementation.
    /// @param assets The amount of assets to withdraw.
    /// @param receiver The address that will receive the assets.
    /// @param owner The address of the owner of the assets.
    /// @return shares The amount of shares to burn.
    /// @return fees The amount of fees to be paid.
    function onWithdraw(
        address sender,
        uint256 assets,
        address receiver,
        address owner
    )
        external
        returns (uint256 shares, uint256 fees);

    function onRedeem(
        address sender,
        uint256 shares,
        address receiver,
        address owner
    )
        external
        returns (uint256 assets, uint256 fees);
    function setWithdrawFeeRate(uint256 _withdrawFeeRate) external;
}

