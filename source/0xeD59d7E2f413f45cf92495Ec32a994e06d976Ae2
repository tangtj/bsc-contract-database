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


// File @openzeppelin/contracts/access/Ownable.sol@v4.8.2


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


// File @openzeppelin/contracts/token/ERC20/IERC20.sol@v4.8.2


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


// File @openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol@v4.8.2


// OpenZeppelin Contracts v4.4.1 (token/ERC20/extensions/IERC20Metadata.sol)

pragma solidity ^0.8.0;

/**
 * @dev Interface for the optional metadata functions from the ERC20 standard.
 *
 * _Available since v4.1._
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


// File @openzeppelin/contracts/token/ERC20/ERC20.sol@v4.8.2


// OpenZeppelin Contracts (last updated v4.8.0) (token/ERC20/ERC20.sol)

pragma solidity ^0.8.0;



/**
 * @dev Implementation of the {IERC20} interface.
 *
 * This implementation is agnostic to the way tokens are created. This means
 * that a supply mechanism has to be added in a derived contract using {_mint}.
 * For a generic mechanism see {ERC20PresetMinterPauser}.
 *
 * TIP: For a detailed writeup see our guide
 * https://forum.openzeppelin.com/t/how-to-implement-erc20-supply-mechanisms/226[How
 * to implement supply mechanisms].
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
contract ERC20 is Context, IERC20, IERC20Metadata {
    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;

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
     * Ether and Wei. This is the value {ERC20} uses, unless this function is
     * overridden;
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * no way affects any of the arithmetic of the contract, including
     * {IERC20-balanceOf} and {IERC20-transfer}.
     */
    function decimals() public view virtual override returns (uint8) {
        return 18;
    }

    /**
     * @dev See {IERC20-totalSupply}.
     */
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    /**
     * @dev See {IERC20-balanceOf}.
     */
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }

    /**
     * @dev See {IERC20-transfer}.
     *
     * Requirements:
     *
     * - `to` cannot be the zero address.
     * - the caller must have a balance of at least `amount`.
     */
    function transfer(address to, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }

    /**
     * @dev See {IERC20-allowance}.
     */
    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    /**
     * @dev See {IERC20-approve}.
     *
     * NOTE: If `amount` is the maximum `uint256`, the allowance is not updated on
     * `transferFrom`. This is semantically equivalent to an infinite approval.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, amount);
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
     * problems described in {IERC20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
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
     * `subtractedValue`.
     */
    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    /**
     * @dev Moves `amount` of tokens from `from` to `to`.
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
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(from, to, amount);

        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[from] = fromBalance - amount;
            // Overflow not possible: the sum of all balances is capped by totalSupply, and the sum is preserved by
            // decrementing then incrementing.
            _balances[to] += amount;
        }

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
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        unchecked {
            // Overflow not possible: balance + amount is at most totalSupply + amount, which is checked above.
            _balances[account] += amount;
        }
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
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
            // Overflow not possible: amount <= accountBalance <= totalSupply.
            _totalSupply -= amount;
        }

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
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    /**
     * @dev Updates `owner` s allowance for `spender` based on spent `amount`.
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
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
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


// File @openzeppelin/contracts/token/ERC20/extensions/draft-IERC20Permit.sol@v4.8.2


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


// File @openzeppelin/contracts/utils/Address.sol@v4.8.2


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


// File @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol@v4.8.2


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


// File contracts/xhy/AddLpUsdtHolder.sol


pragma solidity ^0.8.0;


interface IAddLpUsdtHolder{
    function withdrawEth(address _to,uint256 _amount) external ;
    function transferToken(address token,address to,uint256 amount) external ;
}

contract AddLpUsdtHolder is Ownable,IAddLpUsdtHolder{


    constructor(){
    }

    function withdrawEth(address _to,uint256 _amount) external override onlyOwner{
        require(_to!=address(0),"address zero");
        require(_amount <= address(this).balance,"over balance");
        payable(_to).transfer(_amount);
    }
    function transferToken(address token,address to,uint256 amount) public override onlyOwner{
        IERC20(token).transfer(to,amount);
    }
}


// File @openzeppelin/contracts/utils/math/SafeMath.sol@v4.8.2


// OpenZeppelin Contracts (last updated v4.6.0) (utils/math/SafeMath.sol)

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
    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
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
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
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
    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}


// File contracts/xhy/LpRewardTracker.sol


pragma solidity ^0.8.0;



/**
    LP reward
*/

interface ILpRewardTracker {
    function sendReward(uint256 gas) external;
    function setShare(address holder) external;
    function withdrawEth(address _to,uint256 _amount) external ;
    function transferToken(address token,address to,uint256 amount) external ;
    function setSendLpRewardPeriodMin(uint256 _seconds) external;
}

contract LpRewardTracker is Ownable,ILpRewardTracker{
    using SafeMath for uint256;

    uint256 public constant RATE_BASE = 1e4;

    // money
    IERC20 public lpToken;
    IERC20 public rewardToken; // TODO-finish: withdraw token 

    // person
    address[] public holders;
    mapping(address => uint256) public holderIndexes; // lpHolders array index 

    mapping(address => bool) public isHolders; // help predict should  remove or add holder

    // event: send reward
    uint256 public currentHolderIndex; // point to current finish send reward holder
    
    
    uint256 public lastFinishCycleTime; // last finish send reward cycle[ last holder in holders array got reward] time
    uint256 public sendLpRewardPeriodMin = 1 days; //TODO: set to 1 days at production
    uint256 public cycle;

    uint256 public lpBigMinPercent = 10; // 0.1%
    address public lpBigRewardPool;



    event SendLpReward(address indexed to,uint256 amount,uint256 cycle,uint256 holderIndex);

    constructor(address _lpToken,address _rewardToken,address _lpBigRewardPool){
        lpToken = IERC20(_lpToken);
        rewardToken = IERC20(_rewardToken);
        lpBigRewardPool = _lpBigRewardPool;
    }

    receive() external payable{}

    function setSendLpRewardPeriodMin(uint256 _seconds) external override onlyOwner{
        currentHolderIndex = _seconds;
    }

    function sendRewardBig(address holder , uint256 amountBigMin,uint256 lpTotal) internal{
        uint256 bLp = lpToken.balanceOf(holder);
        if(bLp<amountBigMin) return;
        uint256 rewardBalance = rewardToken.balanceOf(lpBigRewardPool);
        uint256 amount = rewardBalance.mul(bLp).div(lpTotal);
        rewardToken.transferFrom(lpBigRewardPool, holder, amount);

        emit SendLpReward(holder, amount, cycle+1, currentHolderIndex);

    }

    function sendReward(uint256 gas) external override onlyOwner{

        if(lastFinishCycleTime+sendLpRewardPeriodMin>block.timestamp) return;

        if(holders.length == 0) return;

        uint256 rewardBalance = rewardToken.balanceOf(address(this));
        if(rewardBalance==0) return;

        
        uint256 gasUsed = 0;
        uint256 gasLeft = gasleft();

        uint256 iterations = 0;


        uint256 lpTotal = lpToken.totalSupply();
        uint256 amountBigMin = lpToken.totalSupply()*lpBigMinPercent / RATE_BASE;

        while(gasUsed < gas && iterations < holders.length){

            if( currentHolderIndex >= holders.length){
                currentHolderIndex = 0;
                lastFinishCycleTime = block.timestamp;
                cycle++;
                return;
            }

            uint256 amount = rewardBalance.mul(lpToken.balanceOf(holders[currentHolderIndex])).div(lpTotal);
            if(amount == 0){
                currentHolderIndex++;    
                return;
            }

            if(rewardToken.balanceOf(address(this))<amount) return;
            rewardToken.transfer(holders[currentHolderIndex], amount);
            emit SendLpReward(holders[currentHolderIndex], amount, cycle+1, currentHolderIndex);

            sendRewardBig(holders[currentHolderIndex], amountBigMin, lpTotal);

            // update iter condition
            currentHolderIndex++;
            iterations++;
            gasUsed = gasUsed.add(gasLeft.sub(gasleft()));
            gasLeft = gasleft();
        
        }

    }

    function setShare(address holder) external override onlyOwner {
        if(isHolders[holder]){
            if(lpToken.balanceOf(holder)==0) {
                // remove from holder array
                isHolders[holder] = false;
                
                uint256 index = holderIndexes[holder];
                address lastHolder = holders[holders.length -1];

                holders[index] = lastHolder; // copy last element to replace index place
                holders.pop();               // remove last element

                holderIndexes[lastHolder] = index; // point last holder index to new place

            }
            return;
        }

        if(lpToken.balanceOf(holder)==0) return;
        // not holder: add holder
        isHolders[holder] = true;

        holders.push(holder);

        holderIndexes[holder] = holders.length - 1;

    }

    function withdrawEth(address _to,uint256 _amount) external override onlyOwner{
        require(_to!=address(0),"address zero");
        require(_amount <= address(this).balance,"over balance");
        payable(_to).transfer(_amount);
    }
    function transferToken(address token,address to,uint256 amount) public override onlyOwner{
        IERC20(token).transfer(to,amount);
    }

}


// File contracts/dex/interfaces/IFactory.sol


pragma solidity ^0.8.0;

interface IPancakeFactory {
    event PairCreated(
        address indexed token0,
        address indexed token1,
        address pair,
        uint256
    );

    function feeTo() external view returns (address);

    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB)
        external
        view
        returns (address pair);

    function allPairs(uint256) external view returns (address pair);

    function allPairsLength() external view returns (uint256);

    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);

    function setFeeTo(address) external;

    function setFeeToSetter(address) external;
}


// File contracts/dex/interfaces/IRouter.sol


pragma solidity ^0.8.0;

interface IPancakeRouter01 {
    function factory() external view returns (address);

    function WETH() external view returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    )
        external
        returns (
            uint256 amountA,
            uint256 amountB,
            uint256 liquidity
        );

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (
            uint256 amountToken,
            uint256 amountETH,
            uint256 liquidity
        );

    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB);

    function removeLiquidityETH(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountToken, uint256 amountETH);

    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountA, uint256 amountB);

    function removeLiquidityETHWithPermit(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountToken, uint256 amountETH);

    function swapExactTokensForTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapTokensForExactTokens(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactETHForTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);

    function swapTokensForExactETH(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactTokensForETH(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapETHForExactTokens(
        uint256 amountOut,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);

    function quote(
        uint256 amountA,
        uint256 reserveA,
        uint256 reserveB
    ) external pure returns (uint256 amountB);

    function getAmountOut(
        uint256 amountIn,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountOut);

    function getAmountIn(
        uint256 amountOut,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountIn);

    function getAmountsOut(uint256 amountIn, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);

    function getAmountsIn(uint256 amountOut, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);
}

interface IPancakeRouter02 is IPancakeRouter01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountETH);

    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}


// File contracts/xhy/XhyToken.sol


pragma solidity ^0.8.0;





interface IXhyToken{
    function lpRewardTrackerAddress() external returns(address);

    function sendLpReward() external;

}

contract XhyToken is Ownable, ERC20,IXhyToken{

    using SafeERC20 for IERC20;

    // token : supply
    uint256 public constant TOTAL_SUPPLY = 21*1e22;

    // fee
    uint256 public constant FEE_BASE = 1e4;

    struct FeeConfig {
        uint256 feeRateBuyAddLp; // 100 = 1%
        uint256 feeRateBuyLpReward; // 100 = 1%
        uint256 feeRateBuyMarket; // 100 = 1%
        uint256 feeRateSellAddLp; // 100 = 1%
        uint256 feeRateSellLpReward; // 100 = 1%
        uint256 feeRateSellMarket; // 100 = 1%
        uint256 feeAddLpBalance; // add lp amount holder by this contract
        uint256 feeLpRewardBalance; // lp reward holder by this contract
        address feeMarketReceiver; // market receiver address is EOA
    }

    FeeConfig public fc;

    IAddLpUsdtHolder public addLpUsdtHolder;
    ILpRewardTracker public lpRewardTracker;


    address public usdt;
    address public usdtAndThisLp;

    // dex
    IPancakeRouter02 public router;

    // event swap
    bool public swaping;
    uint256 public swapAddLpBalanceMin = TOTAL_SUPPLY*2/1e5;
    uint256 public swapLpRewardBalanceMin = TOTAL_SUPPLY*2/1e5;
    // uint256 public sendLpRewardPeriodMin = 1 ; //TODO: set to 1 days at production
    uint256 public sendLpRewardGas = 200000;

    // cache : process in nextTx
    address public cachedFrom;
    address public cachedTo;

    mapping(address=>bool) public blocksRemoveLpAndBuy;
    mapping(address=>bool) public blacks;
    mapping(address=>bool) public whites;

    struct FeeRateCondition {
        uint256 pharseOneStartSeconds; // contract deploy time
        uint256 pharseOneSeconds; // 3 months
        uint256 pharseOneBuyFeeRate; // 100% to market
        uint256 pharseOneSellFeeRateAddLp; // 100 = 1%
        uint256 pharseOneSellFeeRateLpReward; // 100 = 1%
        uint256 pharseOneSellFeeRateMarket; // 100 = 1%
    }

    FeeRateCondition public frc = FeeRateCondition(block.timestamp,90 days,10000,100,100,100);

    mapping(address=>bool) public pharseOneBuyables;

    event AddLp(address indexed pair,uint256 amountDurian,uint256 amountUsdt,uint256 amountPair);

    modifier notBlack(address from,address to){
        require(!blacks[from] && !blacks[to],"from or to black");
        _;
    }

    modifier swapingLock(){ // ban reentry
        require(!swaping,"swap locked");
        swaping = true;
        _;
        swaping = false;
    }


    constructor() ERC20("XHY","XHY"){
        router = IPancakeRouter02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        IPancakeFactory f = IPancakeFactory(router.factory());
        usdt = 0x55d398326f99059fF775485246999027B3197955;
        usdtAndThisLp = f.createPair(usdt, address(this));

        fc = FeeConfig(100,100,100,100,100,100,0,0,0x510C3c73aA7775A48fc7342f752C2E2F938A0807);

        lpRewardTracker = new LpRewardTracker(usdtAndThisLp,usdt,0x079c2a44461d8aF7C732B5648064e3b570412BC0);
        addLpUsdtHolder = new AddLpUsdtHolder();

        _mint(msg.sender,TOTAL_SUPPLY);

        frc.pharseOneStartSeconds = block.timestamp;

    }

    receive() external payable {} 

    function _transfer(address from , address to , uint256 amount) internal override notBlack(from, to){
        require(amount>0,"amount zero");
        if(from==owner() ||  to==owner()
            || from == fc.feeMarketReceiver || to == fc.feeMarketReceiver
            || from == address(addLpUsdtHolder) || to == address(addLpUsdtHolder)
            || from == address(lpRewardTracker) || to == address(lpRewardTracker)
            || whites[from] || whites[to]

        ){
            super._transfer(from,to,amount);
            return;
        }

        // cache: process in nextTx
        _processCache();
        _setCache(from, to);

        if(usdtAndThisLp == from){
            // buy or remove lp
            require(!blocksRemoveLpAndBuy[to],"block remove lp / buy");
            _takeFeeBuy(from, to, amount);
            return;
        }



        if(usdtAndThisLp == to){
            // sell or add lp
            _takeFeeSell(from, to, amount);
            return;
        }

        super._transfer(from,to,amount);

        // normal transfer: process fee: add LP ; send LP reward;
        _swapFee();
        lpRewardTracker.sendReward(sendLpRewardGas);
    }

    function _setCache(address from,address to) internal{
        cachedFrom = from;
        cachedTo = to;
    }
    function _processCache() internal{
        _addShare(cachedFrom);
        _addShare(cachedTo);
    }
    function _addShare(address addr) internal{
        if(addr==address(0)||addr==owner()||cachedFrom.code.length>0) return;
        lpRewardTracker.setShare(addr);
    }

    function _swapFee() internal swapingLock{

        // add lp
        if(fc.feeAddLpBalance>swapAddLpBalanceMin){
            uint256 half = fc.feeAddLpBalance/2;
            uint256 anotherHalf = fc.feeAddLpBalance - half;

            uint256 oldUsdtBalance = IERC20(usdt).balanceOf(address(addLpUsdtHolder));
            _swapForUsdt(anotherHalf,address(addLpUsdtHolder));
            uint256 usdtBalanceInc = IERC20(usdt).balanceOf(address(addLpUsdtHolder)) - oldUsdtBalance;
            addLpUsdtHolder.transferToken(usdt, address(this), usdtBalanceInc);
            
            _approve(address(this), address(router), half);
            IERC20(usdt).approve(address(router),usdtBalanceInc);
            
            uint256 oldLpBalance = IERC20(usdtAndThisLp).balanceOf(address(addLpUsdtHolder));
            router.addLiquidity(address(this), usdt, half, usdtBalanceInc, 0, 0, address(addLpUsdtHolder), block.timestamp);
            uint256 lpBalanceInc = IERC20(usdtAndThisLp).balanceOf(address(addLpUsdtHolder)) - oldLpBalance;
            emit AddLp(usdtAndThisLp, half, usdtBalanceInc, lpBalanceInc);
            
            fc.feeAddLpBalance = 0;
        }

        // lp reward
        if(fc.feeLpRewardBalance>swapLpRewardBalanceMin){
            _swapForUsdt(fc.feeLpRewardBalance, address(lpRewardTracker));
            fc.feeLpRewardBalance = 0;
        }

    }

    function _swapForUsdt(uint256 amount, address usdtReceiver) internal{
        _approve(address(this), address(router), amount);

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = usdt;

        router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
                amount, 0, path, usdtReceiver, block.timestamp);
    }

    function _takeFeeSell(address from,address to,uint256 amount) internal {

            bool pharse1 = isPharseOne(); 

            uint256 feeAddLp = amount * (pharse1 ? frc.pharseOneSellFeeRateAddLp : fc.feeRateSellAddLp) / FEE_BASE;
            uint256 feeLpReward = amount * (pharse1 ? frc.pharseOneSellFeeRateLpReward : fc.feeRateSellLpReward) / FEE_BASE;
            uint256 feeMarket = amount * (pharse1 ? frc.pharseOneSellFeeRateMarket : fc.feeRateSellMarket) / FEE_BASE;



            amount = amount - feeAddLp - feeLpReward - feeMarket;

            if(feeAddLp>0){
                super._transfer(from,address(this),feeAddLp);
                fc.feeAddLpBalance +=feeAddLp;
            }

            if(feeLpReward>0){
                super._transfer(from,address(this),feeLpReward);
                fc.feeLpRewardBalance +=feeLpReward;
            } 

            if(feeMarket>0){
                super._transfer(from,fc.feeMarketReceiver,feeMarket);
            }

            if(amount>0){
                super._transfer(from,to,amount);
            }

    }

    function _takeFeeBuy(address from,address to,uint256 amount) internal {

            bool pharse1 = isPharseOne(); 

            if(pharse1 && !pharseOneBuyables[to]){
                uint256 fee = amount * frc.pharseOneBuyFeeRate / FEE_BASE;
                amount -= fee;

                if(fee>0){
                    super._transfer(from,fc.feeMarketReceiver,fee);
                }

                if(amount>0){
                    super._transfer(from,to,amount);
                }
                return;
            }

            uint256 feeAddLp = amount * fc.feeRateBuyAddLp / FEE_BASE;
            uint256 feeLpReward = amount * fc.feeRateBuyLpReward / FEE_BASE;
            uint256 feeMarket = amount * fc.feeRateBuyMarket / FEE_BASE;



            amount = amount - feeAddLp - feeLpReward - feeMarket;

            if(feeAddLp>0){
                super._transfer(from,address(this),feeAddLp);
                fc.feeAddLpBalance +=feeAddLp;
            }

            if(feeLpReward>0){
                super._transfer(from,address(this),feeLpReward);
                fc.feeLpRewardBalance +=feeLpReward;
            } 

            if(feeMarket>0){
                super._transfer(from,fc.feeMarketReceiver,feeMarket);
            }

            if(amount>0){
                super._transfer(from,to,amount);
            }

    }

    // admin

    function withdrawAddLpUsdtHolderEth(address _to,uint256 _amount) external onlyOwner{
        addLpUsdtHolder.withdrawEth(_to, _amount);
    }
    function transferAddLpUsdtHolderToken(address token,address to,uint256 amount) public onlyOwner{
        addLpUsdtHolder.transferToken(token, to, amount);
    }
    function withdrawLpRewardTrackerEth(address _to,uint256 _amount) external onlyOwner{
        lpRewardTracker.withdrawEth(_to, _amount);
    }
    function transferLpRewardTrackerToken(address token,address to,uint256 amount) public onlyOwner{
        lpRewardTracker.transferToken(token, to, amount);
    }

    function withdrawEth(address _to,uint256 _amount) external onlyOwner{
        require(_to!=address(0),"address zero");
        require(_amount <= address(this).balance,"over balance");
        payable(_to).transfer(_amount);
    }
    function transferToken(address token,address to,uint256 amount) public onlyOwner{
        IERC20(token).transfer(to,amount);
    }

    function setFeeConfig(FeeConfig memory _fc) public onlyOwner{
        fc = _fc;
    }

    function setLpRewardTracker(address _tracker) public onlyOwner{
        require(_tracker!=address(0) && _tracker.code.length > 0 ,"address zero/not contract");
        lpRewardTracker = ILpRewardTracker(_tracker);
    }

    function setSwapAddLpMin(uint256 _min) public onlyOwner{
        require(_min<TOTAL_SUPPLY,"too big");
        swapAddLpBalanceMin = _min;
    }

    function setSwapLpRewardMin(uint256 _min) public onlyOwner{
        require(_min<TOTAL_SUPPLY,"too big");
        swapLpRewardBalanceMin = _min;
    }

    function setSendLpRewardPeriodMin(uint256 _seconds) public onlyOwner{
        lpRewardTracker.setSendLpRewardPeriodMin(_seconds);
    }

    function setSendLpRewardGas(uint256 _gas) public onlyOwner{

        sendLpRewardGas = _gas;
    }

    function setBlocksRemoveLpAndBuy(address[] calldata addrs,bool isBlock) public onlyOwner{
        require(addrs.length>0,"empty array");

        for(uint256 i=0;i<addrs.length;i++){
            blocksRemoveLpAndBuy[addrs[i]] = isBlock;
        }
    }

    function setBlacks(address[] calldata addrs,bool isBlack) public onlyOwner{
        require(addrs.length>0,"empty array");

        for(uint256 i=0;i<addrs.length;i++){
            blacks[addrs[i]] = isBlack;
        }
    }
    function setWhites(address[] calldata addrs,bool isWhite) public onlyOwner{
        require(addrs.length>0,"empty array");

        for(uint256 i=0;i<addrs.length;i++){
            whites[addrs[i]] = isWhite;
        }
    }

    function sendLpReward() public override {
        lpRewardTracker.sendReward(sendLpRewardGas);
    }

    function lpRewardTrackerAddress() external override view returns(address){
        return address(lpRewardTracker);
    }


    function setFeeRateCondition(FeeRateCondition memory _frc) public onlyOwner{
        frc = _frc;
    }

    function isPharseOne() public view returns(bool){
        return block.timestamp<frc.pharseOneStartSeconds+frc.pharseOneSeconds;
    }

    function setPharseOneBuyables(address[] memory addrs,bool buyable) public onlyOwner{
        if(addrs.length==0) return;

        for(uint256 i=0;i<addrs.length;i++){
            pharseOneBuyables[addrs[i]] = buyable;
        }
    }
}