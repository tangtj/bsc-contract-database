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
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}






// OpenZeppelin Contracts v4.4.1 (token/ERC20/extensions/IERC20Metadata.sol)





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


// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)



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


// OpenZeppelin Contracts v4.4.1 (token/ERC20/utils/SafeERC20.sol)





// OpenZeppelin Contracts (last updated v4.5.0) (utils/Address.sol)



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


// OpenZeppelin Contracts v4.4.1 (access/Ownable.sol)





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


interface IPancakeFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IPancakeRouter {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function getAmountsOut(uint amountIn, address[] memory path) external view returns (uint[] memory amounts);

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

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

}

interface IMain {
    function getInviter(address account) external view returns(address);
    function leaderBuyFee(uint256 amount) external;
    function leaderSellFee(uint256 amount) external;

}


interface IPancakePair {
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);

    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

  
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
}

interface INFTInterface {
    function balanceOf(address owner) external view returns (uint256);
}

contract PairTempAddress{
    address token;
    address target;
    constructor(address _token,address _target){
        token = _token;
        target = _target;
        IERC20(token).approve(target, type(uint256).max);
    }
        
    function reApprove() public{
        IERC20(token).approve(target, type(uint256).max);
    }
}

contract LeaderToken is Context,Ownable, IERC20, IERC20Metadata {

    using SafeERC20 for IERC20;

    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;
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
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
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


    IPancakeRouter public pancakeRouter;

    address public pancakePair;
    //银行NFT
    address public bankNFTAddress;
    //中国结NFT
    address public knotNFTAddress;

    address public marketingAddress;
    address public mainAddress;

    address public usdtAddress;
    address public gkAddress;

    //买入手续费
    uint256 public immutable buyRewardFee = 700;
    uint256 public immutable buyPowerFee = 100;   
    uint256 public immutable buyBurnFee = 50;



    //卖出手续费
    uint256 public immutable sellRewardFee = 750;
    uint256 public immutable sellPowerFee = 150;
    uint256 public immutable sellBurnFee = 50;
    uint256 public sellMarketingFee = 2050;

    
    uint256 public extraSupply;
    uint256 public SPY = 125000000000; //1.08% per day
    uint256 public SPYOne = 11574074074; // 0.1% per nft

    //交易锁
    bool private swapping;
    uint256 public swapPoolMax;
    uint256 public swapPool;

    address public pairTempAddress;

    mapping(address => uint256) public lastUpdateTime;
    mapping(address => bool) public rewardBlacklist;

    bool startSwap;

    bool startCompound;


    // exlcude from fees and max transaction amount
    mapping (address => bool) public _isExcludedFromFees;

    event UpdatePancakeRouter(address indexed newAddress, address indexed oldAddress);

    event ExcludeAccountsFromFees(address accounts, bool isExcluded);
    event CalculateReward(address indexed account,uint256 amount);

    event BuyRewardFee(address indexed account,uint256 amount);
    event SellRewardFee(address indexed account,uint256 amount);
    

    modifier calculateReward(address account) {
        if (account != address(0)) {
            uint256 cost = getCost(_balances[account]);
            if(cost >= 100e18){
                uint256 reward = getReward(account);
                if (reward > 0) {
                    _balances[account] = _balances[account] + reward;
                    emit CalculateReward(account,_balances[account]);
                    extraSupply = extraSupply + reward;
                }
            }
            lastUpdateTime[account] = block.timestamp;
        }
        _;
    }


    function updateReward(address account) public calculateReward(account){
    }
    
    constructor() {
         _name = "Leader Token";
        _symbol = "Leader";

        uint256 amount = 280000000e18;
        _totalSupply += amount;
        _balances[msg.sender] += amount;
        emit Transfer(address(0), msg.sender, amount);

        //init wallet
        if(block.chainid == 56){
            usdtAddress = 0x55d398326f99059fF775485246999027B3197955;
            updatePancakeRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        }else{
            usdtAddress = 0xC5A25d89470e873E8f37eA2B2E8d2e66181DDEF8;
            updatePancakeRouter(0xD99D1c33F9fC3444f8101754aBC46c52416550D1);
        }


        pairTempAddress =  address(new PairTempAddress(usdtAddress,address(this)));

        rewardBlacklist[address(this)] = true;

        excludeFromAllLimits(owner(), true);
        excludeFromAllLimits(address(this), true);
        
        swapPoolMax = 50000e18;
    }

    function updateSwapPoolMax(uint256 _newValue) public onlyOwner{
        swapPoolMax = _newValue;
    }

    function updateStartCompound(bool _value) public onlyOwner{
        startCompound = _value;
    }

    function updateMF(uint256 _value) public onlyOwner{
        sellMarketingFee = _value;
    }

    

    function updatePancakeRouter(address newAddress) public onlyOwner {
        require(newAddress != address(pancakeRouter), "MBank: The router already has that address");
        emit UpdatePancakeRouter(newAddress, address(pancakeRouter));
        pancakeRouter = IPancakeRouter(newAddress);
        address _pancakePair = IPancakeFactory(pancakeRouter.factory())
        .createPair(address(this), usdtAddress);
        pancakePair = _pancakePair;
        rewardBlacklist[pancakePair] = true;
        excludeFromAllLimits(newAddress, true);
    }

    function balanceOf(address account)
        public
        view
        virtual
        override
        returns (uint256)
    {
        uint256 cost = getCost(_balances[account]);
        if(cost >= 100e18){
            return _balances[account] + getReward(account);

        }
        return _balances[account];
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply + extraSupply;
    }

    function getCost(uint256 amount) public view returns(uint256){
        address t0 = IPancakePair(address(pancakePair)).token0();
        (uint r0,uint r1,) = IPancakePair(address(pancakePair)).getReserves();
        if( r0 > 0 && r1 > 0 ){
             if( t0 == address(this)){
                return  r1 * amount/ r0;
            }else{
                return r0 * amount / r1;
            }   
        }
        return 0;
    }

    function getReward(address account) public view returns (uint256) {
        if(!startCompound){
            return 0;
        }

        if (lastUpdateTime[account] == 0 || rewardBlacklist[account]) {
            return 0;
        }
        uint256 balanceCash = INFTInterface(bankNFTAddress).balanceOf(account);
        uint256 balanceKnot = INFTInterface(knotNFTAddress).balanceOf(account);

        uint256 perRate = balanceCash + 5 * balanceKnot; 

        uint256 count = perRate > 9 ? 9 : perRate;

        uint256 spy = SPY + count * SPYOne;
        
        return _balances[account] * spy * (block.timestamp - lastUpdateTime[account]) / 1e18;
    }

    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual calculateReward(from) calculateReward(to) {}

    function excludeFromAllLimits(address account, bool status) public onlyOwner {
        _isExcludedFromFees[account] = status;
        emit ExcludeAccountsFromFees(account,status);
    }

    function excludeMultipleFromAllLimits(address[] memory accounts, bool status) public onlyOwner {
        for(uint256 i = 0; i < accounts.length; i++) {
            excludeFromAllLimits(accounts[i], status);
        }
    }

    function setRewardBlacklist(address[] memory accounts, bool status) external onlyOwner {
        for(uint256 i = 0; i < accounts.length; i++) {
            rewardBlacklist[accounts[i]] = status;
        }
    }

    function setConfigAddress(
        address _bankNFTAddress,
        address _marketingAddress,
        address _knotNFTAddress,
        address _mainAddress,
        address _gkAddress
     ) external onlyOwner {
        bankNFTAddress = _bankNFTAddress;
        marketingAddress = _marketingAddress;
        knotNFTAddress = _knotNFTAddress;
        mainAddress = _mainAddress;
        gkAddress = _gkAddress;
        excludeFromAllLimits(marketingAddress, true);

    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal  {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

         if(!startSwap && _isExcludedFromFees[from] && pancakePair == to){
            startSwap = true;
        }

        require(startSwap || pancakePair != to,"not start swap");

        _beforeTokenTransfer(from,to,amount);

        if(amount == 0) {
            emit Transfer(from, to, amount);
            return;
        }

        uint256 fromBalance = _balances[from];

        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");

        if (!swapping && swapPool > swapPoolMax && pancakePair != from) {
            swapping = true;
            swapReward(swapPool);
            swapping = false;
        }


        if(_isExcludedFromFees[from] || _isExcludedFromFees[to]){
            _balances[from] -= amount;
            _balances[to] += amount;
            emit Transfer(from, to, amount);
            return;
        }

        if(pancakePair == from){
            //buy
            _balances[from] -= amount;

            uint256 buyRewardFeeAmount = amount * buyRewardFee / 10000;
            _balances[address(this)] += buyRewardFeeAmount;
            swapPool += buyRewardFeeAmount;
            emit Transfer(from, address(this), buyRewardFeeAmount);


            uint256 buyPowerFeeAmount = amount * buyPowerFee / 10000;
            _balances[mainAddress] += buyPowerFeeAmount;
            emit Transfer(from, mainAddress, buyPowerFeeAmount);


            uint256 buyBurnFeeAmount = amount * buyBurnFee / 10000;
            _balances[address(0x0)] += buyBurnFeeAmount;
            emit Transfer(from, address(0x0), buyBurnFeeAmount);

            _balances[to] += amount - buyRewardFeeAmount - buyPowerFeeAmount - buyBurnFeeAmount;
            emit Transfer(from, to, amount - buyRewardFeeAmount - buyPowerFeeAmount - buyBurnFeeAmount);

        }else if(pancakePair == to){
            //sell
            _balances[from] -= amount;

            uint256 sellRewardFeeAmount = amount * sellRewardFee / 10000;
            _balances[address(this)] += sellRewardFeeAmount;
            swapPool += sellRewardFeeAmount;
            emit Transfer(from, address(this), sellRewardFeeAmount);


            uint256 sellPowerFeeAmount = amount * sellPowerFee / 10000;
            _balances[mainAddress] += sellPowerFeeAmount;
            emit Transfer(from, mainAddress, sellPowerFeeAmount);


            uint256 sellBurnFeeAmount = amount * sellBurnFee / 10000;
            _balances[address(0x0)] += sellBurnFeeAmount;
            emit Transfer(from, address(0x0), sellBurnFeeAmount);

            uint256 sellMarketingFeeAmount = 0;
            if(sellMarketingFee > 0){
                sellMarketingFeeAmount = amount * sellMarketingFee / 10000;
                _balances[marketingAddress] += sellMarketingFeeAmount;
                emit Transfer(from, marketingAddress, sellMarketingFeeAmount);
            }

            _balances[to] += amount - sellRewardFeeAmount - sellMarketingFeeAmount - sellPowerFeeAmount - sellBurnFeeAmount;
            emit Transfer(from, to, amount - sellRewardFeeAmount - sellMarketingFeeAmount - sellPowerFeeAmount - sellBurnFeeAmount);

        }else{
            _balances[from] -= amount;
            _balances[to] = _balances[to] + amount * 95 / 100;
            emit Transfer(from, to, amount * 95 / 100);
            _balances[address(0x0)] =  _balances[address(0x0)] + amount * 5 / 100;
            emit Transfer(from, address(0x0), amount * 5 / 100);
        }
        if(_balances[from] == 0){
            _balances[from] += 1e13;
            emit Transfer(address(0x0), from, 1e13);
        }
    }

    function withdrawToken(address _tokenAddress,address _to,uint256 amount) public onlyOwner{
        IERC20(_tokenAddress).transfer(_to,amount);
    }


    event SwapReward(address indexed token,uint256 amount);

    function swapReward(uint256 amount) internal{
        _approve(address(this), address(pancakeRouter), amount);

        uint256 beforeU = IERC20(usdtAddress).balanceOf(address(this));
        sellLeaderToUsdt(amount);
        IERC20(usdtAddress).safeTransferFrom(pairTempAddress,address(this),IERC20(usdtAddress).balanceOf(pairTempAddress));
        uint256 afterU = IERC20(usdtAddress).balanceOf(address(this));  
        uint256 rewardU = afterU - beforeU;
        emit SwapReward(usdtAddress,rewardU);
        IERC20(usdtAddress).safeTransfer(mainAddress,rewardU * 15 / 100);
        uint256 sellGK = rewardU * 70 / 100;
        IERC20(usdtAddress).approve(address(pancakeRouter), sellGK);
        uint256 beforeGK = IERC20(gkAddress).balanceOf(mainAddress);
        sellUsdtToGK(sellGK);
        uint256 afterGk = IERC20(gkAddress).balanceOf(mainAddress);
        uint256 rewardGk = afterGk - beforeGK;
        emit SwapReward(gkAddress,rewardGk);
        swapPool = 0;
    }
    

    function sellLeaderToUsdt(uint256 tokenAmount) private{
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = usdtAddress;
        // make the swap
        pancakeRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            pairTempAddress,
            block.timestamp
        );
    }


    function sellUsdtToGK(uint256 tokenAmount) private{
        address[] memory path = new address[](2);
        path[0] = usdtAddress;
        path[1] = gkAddress;
        // make the swap
        pancakeRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            mainAddress,
            block.timestamp
        );
    }

}