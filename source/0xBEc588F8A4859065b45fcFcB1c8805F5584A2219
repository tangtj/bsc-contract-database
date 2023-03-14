// SPDX-License-Identifier: agpl-3.0


pragma solidity >=0.4.24 <0.6.0;

/*
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with GSN meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
contract Context {
    // Empty internal constructor, to prevent people from mistakenly deploying
    // an instance of this contract, which should be used via inheritance.
    constructor () internal { }
    // solhint-disable-previous-line no-empty-blocks

    function _msgSender() internal view returns (address payable) {
        return msg.sender;
    }

    function _msgData() internal view returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor () internal {
        _owner = _msgSender();
        emit OwnershipTransferred(address(0), _owner);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(isOwner(), "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Returns true if the caller is the current owner.
     */
    function isOwner() public view returns (bool) {
        return _msgSender() == _owner;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     */
    function _transferOwnership(address newOwner) internal {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

/**
 * @title VersionedInitializable
 *
 * @dev Helper contract to support initializer functions. To use it, replace
 * the constructor with a function that has the `initializer` modifier.
 * WARNING: Unlike constructors, initializer functions must be manually
 * invoked. This applies both to deploying an Initializable contract, as well
 * as extending an Initializable contract via inheritance.
 * WARNING: When used with inheritance, manual care must be taken to not invoke
 * a parent initializer twice, or ensure that all initializers are idempotent,
 * because this is not dealt with automatically as with constructors.
 *
 @author Multiplier Finance, inspired by the OpenZeppelin Initializable contract
 */
contract VersionedInitializable {
    /**
     * @dev Indicates that the contract has been initialized.
     */
    uint256 private lastInitializedRevision = 0;

    /**
     * @dev Indicates that the contract is in the process of being initialized.
     */
    bool private initializing;

    /**
     * @dev Modifier to use in the initializer function of a contract.
     */
    modifier initializer() {
        uint256 revision = getRevision();
        require(
            initializing ||
                isConstructor() ||
                revision > lastInitializedRevision,
            "Contract instance has already been initialized"
        );

        bool isTopLevelCall = !initializing;
        if (isTopLevelCall) {
            initializing = true;
            lastInitializedRevision = revision;
        }

        _;

        if (isTopLevelCall) {
            initializing = false;
        }
    }

    /// @dev returns the revision number of the contract.
    /// Needs to be defined in the inherited class as a constant.
    function getRevision() internal pure returns (uint256);

    /// @dev Returns true if and only if the function is running in the
    // constructor
    function isConstructor() private view returns (bool) {
        // extcodesize checks the size of the code stored in an address, and
        // address returns the current address. Since the code is still not
        // deployed when running a constructor, any checks on its code size will
        // yield zero, making it an effective way to detect if a contract is
        // under construction or not.
        uint256 cs;
        //solium-disable-next-line
        assembly {
            cs := extcodesize(address)
        }
        return cs == 0;
    }

    // Reserved storage space to allow for layout changes in the future.
    uint256[50] private ______gap;
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
 */
contract ReentrancyGuard {
    // counter to allow mutex lock with only one SSTORE operation
    uint256 private _guardCounter;

    constructor () internal {
        // The counter starts at one to prevent changing it from zero to a non-zero
        // value, which is a more expensive operation.
        _guardCounter = 1;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and make it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        _guardCounter += 1;
        uint256 localCounter = _guardCounter;
        _;
        require(localCounter == _guardCounter, "ReentrancyGuard: reentrant call");
    }
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
     * - Subtraction cannot overflow.
     *
     * _Available since v2.4.0._
     */
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
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
     * - The divisor cannot be zero.
     *
     * _Available since v2.4.0._
     */
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
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
     * - The divisor cannot be zero.
     *
     * _Available since v2.4.0._
     */
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}


/**
 * @dev Interface of the ERC20 standard as defined in the EIP. Does not include
 * the optional functions; to access them see {ERC20Detailed}.
 */
interface IERC20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount) external returns (bool);

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
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

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
}


/**
 * @dev Collection of functions related to the address type
 */
library Address {
    /**
     * @dev Returns true if `account` is a contract.
     *
     * This test is non-exhaustive, and there may be false-negatives: during the
     * execution of a contract's constructor, its address will be reported as
     * not containing a contract.
     *
     * IMPORTANT: It is unsafe to assume that an address for which this
     * function returns false is an externally-owned account (EOA) and not a
     * contract.
     */
    function isContract(address account) internal view returns (bool) {
        // This method relies in extcodesize, which returns 0 for contracts in
        // construction, since the code is only stored at the end of the
        // constructor execution.

        // According to EIP-1052, 0x0 is the value returned for not-yet created accounts
        // and 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470 is returned
        // for accounts without code, i.e. `keccak256('')`
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly { codehash := extcodehash(account) }
        return (codehash != 0x0 && codehash != accountHash);
    }

    /**
     * @dev Converts an `address` into `address payable`. Note that this is
     * simply a type cast: the actual underlying value is not changed.
     *
     * _Available since v2.4.0._
     */
    function toPayable(address account) internal pure returns (address payable) {
        return address(uint160(account));
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
     *
     * _Available since v2.4.0._
     */
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-call-value
        (bool success, ) = recipient.call.value(amount)("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
}



/**
 * @dev Implementation of the {IERC20} interface.
 *
 * This implementation is agnostic to the way tokens are created. This means
 * that a supply mechanism has to be added in a derived contract using {_mint}.
 * For a generic mechanism see {ERC20Mintable}.
 *
 * TIP: For a detailed writeup see our guide
 * https://forum.zeppelin.solutions/t/how-to-implement-erc20-supply-mechanisms/226[How
 * to implement supply mechanisms].
 *
 * We have followed general OpenZeppelin guidelines: functions revert instead
 * of returning `false` on failure. This behavior is nonetheless conventional
 * and does not conflict with the expectations of ERC20 applications.
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
contract ERC20 is Context, IERC20 {
    using SafeMath for uint256;

    mapping (address => uint256) private _balances;

    mapping (address => mapping (address => uint256)) private _allowances;

    uint256 private _totalSupply;

    /**
     * @dev See {IERC20-totalSupply}.
     */
    function totalSupply() public view returns (uint256) {
        return _totalSupply;
    }

    /**
     * @dev See {IERC20-balanceOf}.
     */
    function balanceOf(address account) public view returns (uint256) {
        return _balances[account];
    }

    /**
     * @dev See {IERC20-transfer}.
     *
     * Requirements:
     *
     * - `recipient` cannot be the zero address.
     * - the caller must have a balance of at least `amount`.
     */
    function transfer(address recipient, uint256 amount) public returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    /**
     * @dev See {IERC20-allowance}.
     */
    function allowance(address owner, address spender) public view returns (uint256) {
        return _allowances[owner][spender];
    }

    /**
     * @dev See {IERC20-approve}.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 amount) public returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    /**
     * @dev See {IERC20-transferFrom}.
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {ERC20};
     *
     * Requirements:
     * - `sender` and `recipient` cannot be the zero address.
     * - `sender` must have a balance of at least `amount`.
     * - the caller must have allowance for `sender`'s tokens of at least
     * `amount`.
     */
    function transferFrom(address sender, address recipient, uint256 amount) public returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
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
    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
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
    function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }

    /**
     * @dev Moves tokens `amount` from `sender` to `recipient`.
     *
     * This is internal function is equivalent to {transfer}, and can be used to
     * e.g. implement automatic token fees, slashing mechanisms, etc.
     *
     * Emits a {Transfer} event.
     *
     * Requirements:
     *
     * - `sender` cannot be the zero address.
     * - `recipient` cannot be the zero address.
     * - `sender` must have a balance of at least `amount`.
     */
    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        _balances[sender] = _balances[sender].sub(amount, "ERC20: transfer amount exceeds balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }

    /** @dev Creates `amount` tokens and assigns them to `account`, increasing
     * the total supply.
     *
     * Emits a {Transfer} event with `from` set to the zero address.
     *
     * Requirements
     *
     * - `to` cannot be the zero address.
     */
    function _mint(address account, uint256 amount) internal {
        require(account != address(0), "ERC20: mint to the zero address");

        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
        emit Transfer(address(0), account, amount);
    }

     /**
     * @dev Destroys `amount` tokens from `account`, reducing the
     * total supply.
     *
     * Emits a {Transfer} event with `to` set to the zero address.
     *
     * Requirements
     *
     * - `account` cannot be the zero address.
     * - `account` must have at least `amount` tokens.
     */
    function _burn(address account, uint256 amount) internal {
        require(account != address(0), "ERC20: burn from the zero address");

        _balances[account] = _balances[account].sub(amount, "ERC20: burn amount exceeds balance");
        _totalSupply = _totalSupply.sub(amount);
        emit Transfer(account, address(0), amount);
    }

    /**
     * @dev Sets `amount` as the allowance of `spender` over the `owner`s tokens.
     *
     * This is internal function is equivalent to `approve`, and can be used to
     * e.g. set automatic allowances for certain subsystems, etc.
     *
     * Emits an {Approval} event.
     *
     * Requirements:
     *
     * - `owner` cannot be the zero address.
     * - `spender` cannot be the zero address.
     */
    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    /**
     * @dev Destroys `amount` tokens from `account`.`amount` is then deducted
     * from the caller's allowance.
     *
     * See {_burn} and {_approve}.
     */
    function _burnFrom(address account, uint256 amount) internal {
        _burn(account, amount);
        _approve(account, _msgSender(), _allowances[account][_msgSender()].sub(amount, "ERC20: burn amount exceeds allowance"));
    }
}


/**
 * @title SafeERC20
 * @dev Wrappers around ERC20 operations that throw on failure (when the token
 * contract returns false). Tokens that return no value (and instead revert or
 * throw on failure) are also supported, non-reverting calls are assumed to be
 * successful.
 * To use this library you can add a `using SafeERC20 for ERC20;` statement to your contract,
 * which allows you to call the safe operations as `token.safeTransfer(...)`, etc.
 */
library SafeERC20 {
    using SafeMath for uint256;
    using Address for address;

    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    function safeApprove(IERC20 token, address spender, uint256 value) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        // solhint-disable-next-line max-line-length
        require((value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).add(value);
        callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).sub(value, "SafeERC20: decreased allowance below zero");
        callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     */
    function callOptionalReturn(IERC20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves.

        // A Solidity high level call has three parts:
        //  1. The target address is checked to verify it contains contract code
        //  2. The call itself is made, and success asserted
        //  3. The return value is decoded, which in turn checks the size of the returned data.
        // solhint-disable-next-line max-line-length
        require(address(token).isContract(), "SafeERC20: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = address(token).call(data);
        require(success, "SafeERC20: low-level call failed");

        if (returndata.length > 0) { // Return data is optional
            // solhint-disable-next-line max-line-length
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}

/**
 * @dev Optional functions from the ERC20 standard.
 */
contract ERC20Detailed is IERC20 {
    string private _name;
    string private _symbol;
    uint8 private _decimals;

    /**
     * @dev Sets the values for `name`, `symbol`, and `decimals`. All three of
     * these values are immutable: they can only be set once during
     * construction.
     */
    constructor (string memory name, string memory symbol, uint8 decimals) public {
        _name = name;
        _symbol = symbol;
        _decimals = decimals;
    }

    /**
     * @dev Returns the name of the token.
     */
    function name() public view returns (string memory) {
        return _name;
    }

    /**
     * @dev Returns the symbol of the token, usually a shorter version of the
     * name.
     */
    function symbol() public view returns (string memory) {
        return _symbol;
    }

    /**
     * @dev Returns the number of decimals used to get its user representation.
     * For example, if `decimals` equals `2`, a balance of `505` tokens should
     * be displayed to a user as `5,05` (`505 / 10 ** 2`).
     *
     * Tokens usually opt for a value of 18, imitating the relationship between
     * Ether and Wei.
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * no way affects any of the arithmetic of the contract, including
     * {IERC20-balanceOf} and {IERC20-transfer}.
     */
    function decimals() public view returns (uint8) {
        return _decimals;
    }
}

library BscAddressLib {
    /**
     * @dev returns the address used within the protocol to identify ETH
     * @return the address assigned to ETH
     */
    function bnbAddress() internal pure returns (address) {
        return 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE;
    }
}


/**
 * @title IFlashLoanReceiver interface
 * @notice Interface for the Multiplier fee IFlashLoanReceiver.
 * @author Multiplier Finance
 * @dev implement this interface to develop a flashloan-compatible
 * flashLoanReceiver contract
 **/
interface IFlashLoanReceiver {
    function executeOperation(
        address _reserve,
        uint256 _amount,
        uint256 _fee,
        bytes calldata _params
    ) external;
}


/************
@title IFeeProvider interface
@notice Interface for the Multiplier fee provider.
*/

interface IFeeProvider {
    function calculateLoanOriginationFee(uint256 _amount)
        external
        view
        returns (uint256);

    function calculateRewards(uint256 _originationFee) external view returns (uint256, uint256, uint256);

    function getFeeRates() external view returns (uint256, uint256);

    function getFlashLoanFee() external view returns (uint256);
    
    function getRewardRates() external view returns (uint256, uint256, uint256);
}


/************
@title IPriceOracleGetter interface
@notice Interface for the Multiplier price oracle.*/
interface IPriceOracleGetter {
    /***********
    @dev returns the asset price in ETH
     */
    function getAssetPrice(address _asset) external view returns (uint256);
}


/**
 * @title IReserveInterestRateStrategyInterface interface
 * @notice Interface for the calculation of the interest rates.
 */

interface IReserveInterestRateStrategy {
    /**
     * @dev returns the base variable borrow rate, in rays
     */

    function getBaseVariableBorrowRate() external view returns (uint256);

    /**
     * @dev calculates the liquidity, stable, and variable rates depending on
     * the current utilization rate and the base parameters
     */
    function calculateInterestRates(
        address _reserve,
        uint256 _utilizationRate,
        uint256 _totalBorrowsStable,
        uint256 _totalBorrowsVariable,
        uint256 _averageStableBorrowRate
    )
        external
        view
        returns (
            uint256 liquidityRate,
            uint256 stableBorrowRate,
            uint256 variableBorrowRate
        );
}


/**
 * @title ILendingRateOracle interface
 * @notice Interface for the Multiplier borrow rate oracle. Provides the average
 * market borrow rate to be used as a base for the stable borrow rate
 * calculations
 **/

interface ILendingRateOracle {
    /**
    @dev returns the market borrow rate in ray
    **/
    function getMarketBorrowRate(address _asset)
        external
        view
        returns (uint256);

    /**
    @dev sets the market borrow rate. Rate value must be in ray
    **/
    function setMarketBorrowRate(address _asset, uint256 _rate) external;
}





/**
 * @title CoreLibrary library
 * @author Multiplier Finance
 * @notice Defines the data structures of the reserves and the user data
 **/
library CoreLibrary {
    using SafeMath for uint256;
    using WadRayMath for uint256;

    enum InterestRateMode {NONE, STABLE, VARIABLE}

    uint256 internal constant SECONDS_PER_YEAR = 365 days;

    struct UserReserveData {
        // principal amount borrowed by the user.
        uint256 principalBorrowBalance;
        // cumulated variable borrow index for the user. Expressed in ray
        uint256 lastVariableBorrowCumulativeIndex;
        // origination fee cumulated by the user
        uint256 originationFee;
        // stable borrow rate at which the user has borrowed. Expressed in ray
        uint256 stableBorrowRate;
        uint40 lastUpdateTimestamp;
        // defines if a specific deposit should or not be used as a collateral
        // in borrows
        bool useAsCollateral;
    }

    struct ReserveData {
        /**
         * @dev refer to the whitepaper, section 1.1 basic concepts for a formal
         * description of these properties.
         **/
        // the liquidity index. Expressed in ray
        uint256 lastLiquidityCumulativeIndex;
        // the current supply rate. Expressed in ray
        uint256 currentLiquidityRate;
        // the total borrows of the reserve at a stable rate. Expressed in the
        // currency decimals
        uint256 totalBorrowsStable;
        //the total borrows of the reserve at a variable rate. Expressed in the
        // currency decimals
        uint256 totalBorrowsVariable;
        //the current variable borrow rate. Expressed in ray
        uint256 currentVariableBorrowRate;
        //the current stable borrow rate. Expressed in ray
        uint256 currentStableBorrowRate;
        //the current average stable borrow rate (weighted average of all the
        // different stable rate loans). Expressed in ray
        uint256 currentAverageStableBorrowRate;
        //variable borrow index. Expressed in ray
        uint256 lastVariableBorrowCumulativeIndex;
        //the ltv of the reserve. Expressed in percentage (0-100)
        uint256 baseLTVasCollateral;
        //the liquidation threshold of the reserve. Expressed in percentage
        // (0-100)
        uint256 liquidationThreshold;
        //the liquidation bonus of the reserve. Expressed in percentage
        uint256 liquidationBonus;
        //the decimals of the reserve asset
        uint256 decimals;
        /**
         * @dev address of the MToken representing the asset
         **/
        address mTokenAddress;
        /**
         * @dev address of the interest rate strategy contract
         **/
        address interestRateStrategyAddress;
        uint40 lastUpdateTimestamp;
        // borrowingEnabled = true means users can borrow from this reserve
        bool borrowingEnabled;
        // usageAsCollateralEnabled = true means users can use this reserve as
        // collateral
        bool usageAsCollateralEnabled;
        // isStableBorrowRateEnabled = true means users can borrow at a stable
        // rate
        bool isStableBorrowRateEnabled;
        // isActive = true means the reserve has been activated and properly
        // configured
        bool isActive;
        // isFreezed = true means the reserve only allows repays and redeems,
        // but not deposits, new borrowings or rate swap
        bool isFreezed;
    }

    /**
     * @dev returns the ongoing normalized income for the reserve.
     * a value of 1e27 means there is no income. As time passes, the income is
     * accrued.
     * A value of 2*1e27 means that the income of the reserve is double the
     * initial amount.
     * @param _reserve the reserve object
     * @return the normalized income. expressed in ray
     **/
    function getNormalizedIncome(CoreLibrary.ReserveData storage _reserve)
        internal
        view
        returns (uint256)
    {
        uint256 cumulated =
            calculateLinearInterest(
                _reserve
                    .currentLiquidityRate,
                _reserve
                    .lastUpdateTimestamp
            )
                .rayMul(_reserve.lastLiquidityCumulativeIndex);

        return cumulated;
    }

    /**
     * @dev Updates the liquidity cumulative index Ci and variable borrow
     * cumulative index Bvc. Refer to the whitepaper for
     * a formal specification.
     * @param _self the reserve object
     **/
    function updateCumulativeIndexes(ReserveData storage _self) internal {
        uint256 totalBorrows = getTotalBorrows(_self);

        if (totalBorrows > 0) {
            //only cumulating if there is any income being produced
            uint256 cumulatedLiquidityInterest =
                calculateLinearInterest(
                    _self.currentLiquidityRate,
                    _self.lastUpdateTimestamp
                );

            _self.lastLiquidityCumulativeIndex = cumulatedLiquidityInterest
                .rayMul(_self.lastLiquidityCumulativeIndex);

            uint256 cumulatedVariableBorrowInterest =
                calculateCompoundedInterest(
                    _self.currentVariableBorrowRate,
                    _self.lastUpdateTimestamp
                );
            _self
                .lastVariableBorrowCumulativeIndex = cumulatedVariableBorrowInterest
                .rayMul(_self.lastVariableBorrowCumulativeIndex);
        }
    }

    /**
     * @dev accumulates a predefined amount of asset to the reserve as a fixed,
     * one time income. Used for example to accumulate
     * the flashloan fee to the reserve, and spread it through the depositors.
     * @param _self the reserve object
     * @param _totalLiquidity the total liquidity available in the reserve
     * @param _amount the amount to accomulate
     **/
    function cumulateToLiquidityIndex(
        ReserveData storage _self,
        uint256 _totalLiquidity,
        uint256 _amount
    ) internal {
        uint256 amountToLiquidityRatio =
            _amount.wadToRay().rayDiv(_totalLiquidity.wadToRay());

        uint256 cumulatedLiquidity =
            amountToLiquidityRatio.add(WadRayMath.ray());

        _self.lastLiquidityCumulativeIndex = cumulatedLiquidity.rayMul(
            _self.lastLiquidityCumulativeIndex
        );
    }

    /**
     * @dev initializes a reserve
     * @param _self the reserve object
     * @param _mTokenAddress the address of the overlying mToken contract
     * @param _decimals the number of decimals of the underlying asset
     * @param _interestRateStrategyAddress the address of the interest rate
     * strategy contract
     **/
    function init(
        ReserveData storage _self,
        address _mTokenAddress,
        uint256 _decimals,
        address _interestRateStrategyAddress
    ) external {
        require(
            _self.mTokenAddress == address(0),
            "Reserve has already been initialized"
        );

        if (_self.lastLiquidityCumulativeIndex == 0) {
            //if the reserve has not been initialized yet
            _self.lastLiquidityCumulativeIndex = WadRayMath.ray();
        }

        if (_self.lastVariableBorrowCumulativeIndex == 0) {
            _self.lastVariableBorrowCumulativeIndex = WadRayMath.ray();
        }

        _self.mTokenAddress = _mTokenAddress;
        _self.decimals = _decimals;

        _self.interestRateStrategyAddress = _interestRateStrategyAddress;
        _self.isActive = true;
        _self.isFreezed = false;
    }

    /**
     * @dev enables borrowing on a reserve
     * @param _self the reserve object
     * @param _stableBorrowRateEnabled true if the stable borrow rate must be
     * enabled by default, false otherwise
     **/
    function enableBorrowing(
        ReserveData storage _self,
        bool _stableBorrowRateEnabled
    ) external {
        require(_self.borrowingEnabled == false, "Reserve is already enabled");

        _self.borrowingEnabled = true;
        _self.isStableBorrowRateEnabled = _stableBorrowRateEnabled;
    }

    /**
     * @dev disables borrowing on a reserve
     * @param _self the reserve object
     **/
    function disableBorrowing(ReserveData storage _self) external {
        _self.borrowingEnabled = false;
    }

    /**
     * @dev enables a reserve to be used as collateral
     * @param _self the reserve object
     * @param _baseLTVasCollateral the loan to value of the asset when used as
     * collateral
     * @param _liquidationThreshold the threshold at which loans using this
     * asset as collateral will be considered undercollateralized
     * @param _liquidationBonus the bonus liquidators receive to liquidate this
     * asset
     **/
    function enableAsCollateral(
        ReserveData storage _self,
        uint256 _baseLTVasCollateral,
        uint256 _liquidationThreshold,
        uint256 _liquidationBonus
    ) external {
        require(
            _self.usageAsCollateralEnabled == false,
            "Reserve is already enabled as collateral"
        );

        _self.usageAsCollateralEnabled = true;
        _self.baseLTVasCollateral = _baseLTVasCollateral;
        _self.liquidationThreshold = _liquidationThreshold;
        _self.liquidationBonus = _liquidationBonus;

        if (_self.lastLiquidityCumulativeIndex == 0)
            _self.lastLiquidityCumulativeIndex = WadRayMath.ray();
    }

    /**
     * @dev disables a reserve as collateral
     * @param _self the reserve object
     **/
    function disableAsCollateral(ReserveData storage _self) external {
        _self.usageAsCollateralEnabled = false;
    }

    /**
     * @dev calculates the compounded borrow balance of a user
     * @param _self the userReserve object
     * @param _reserve the reserve object
     * @return the user compounded borrow balance
     **/
    function getCompoundedBorrowBalance(
        CoreLibrary.UserReserveData storage _self,
        CoreLibrary.ReserveData storage _reserve
    ) internal view returns (uint256) {
        if (_self.principalBorrowBalance == 0) return 0;

        uint256 principalBorrowBalanceRay =
            _self.principalBorrowBalance.wadToRay();
        uint256 compoundedBalance = 0;
        uint256 cumulatedInterest = 0;

        if (_self.stableBorrowRate > 0) {
            cumulatedInterest = calculateCompoundedInterest(
                _self.stableBorrowRate,
                _self.lastUpdateTimestamp
            );
        } else {
            //variable interest
            cumulatedInterest = calculateCompoundedInterest(
                _reserve
                    .currentVariableBorrowRate,
                _reserve
                    .lastUpdateTimestamp
            )
                .rayMul(_reserve.lastVariableBorrowCumulativeIndex)
                .rayDiv(_self.lastVariableBorrowCumulativeIndex);
        }

        compoundedBalance = principalBorrowBalanceRay
            .rayMul(cumulatedInterest)
            .rayToWad();

        if (compoundedBalance == _self.principalBorrowBalance) {
            //solium-disable-next-line
            if (_self.lastUpdateTimestamp != block.timestamp) {
                //no interest cumulation because of the rounding - we add 1 wei
                //as symbolic cumulated interest to avoid interest free loans.

                return _self.principalBorrowBalance.add(1 wei);
            }
        }

        return compoundedBalance;
    }

    /**
     * @dev increases the total borrows at a stable rate on a specific reserve
     * and updates the
     * average stable rate consequently
     * @param _reserve the reserve object
     * @param _amount the amount to add to the total borrows stable
     * @param _rate the rate at which the amount has been borrowed
     **/
    function increaseTotalBorrowsStableAndUpdateAverageRate(
        ReserveData storage _reserve,
        uint256 _amount,
        uint256 _rate
    ) internal {
        uint256 previousTotalBorrowStable = _reserve.totalBorrowsStable;
        //updating reserve borrows stable
        _reserve.totalBorrowsStable = _reserve.totalBorrowsStable.add(_amount);

        //update the average stable rate
        //weighted average of all the borrows
        uint256 weightedLastBorrow = _amount.wadToRay().rayMul(_rate);
        uint256 weightedPreviousTotalBorrows =
            previousTotalBorrowStable.wadToRay().rayMul(
                _reserve.currentAverageStableBorrowRate
            );

        _reserve.currentAverageStableBorrowRate = weightedLastBorrow
            .add(weightedPreviousTotalBorrows)
            .rayDiv(_reserve.totalBorrowsStable.wadToRay());
    }

    /**
     * @dev decreases the total borrows at a stable rate on a specific reserve and updates the
     * average stable rate consequently
     * @param _reserve the reserve object
     * @param _amount the amount to substract to the total borrows stable
     * @param _rate the rate at which the amount has been repaid
     **/
    function decreaseTotalBorrowsStableAndUpdateAverageRate(
        ReserveData storage _reserve,
        uint256 _amount,
        uint256 _rate
    ) internal {
        require(
            _reserve.totalBorrowsStable >= _amount,
            "Invalid amount to decrease"
        );

        uint256 previousTotalBorrowStable = _reserve.totalBorrowsStable;

        //updating reserve borrows stable
        _reserve.totalBorrowsStable = _reserve.totalBorrowsStable.sub(_amount);

        if (_reserve.totalBorrowsStable == 0) {
            _reserve.currentAverageStableBorrowRate = 0; //no income if there are no stable rate borrows
            return;
        }

        //update the average stable rate
        //weighted average of all the borrows
        uint256 weightedLastBorrow = _amount.wadToRay().rayMul(_rate);
        uint256 weightedPreviousTotalBorrows =
            previousTotalBorrowStable.wadToRay().rayMul(
                _reserve.currentAverageStableBorrowRate
            );

        require(
            weightedPreviousTotalBorrows >= weightedLastBorrow,
            "The amounts to subtract don't match"
        );

        _reserve.currentAverageStableBorrowRate = weightedPreviousTotalBorrows
            .sub(weightedLastBorrow)
            .rayDiv(_reserve.totalBorrowsStable.wadToRay());
    }

    /**
     * @dev increases the total borrows at a variable rate
     * @param _reserve the reserve object
     * @param _amount the amount to add to the total borrows variable
     **/
    function increaseTotalBorrowsVariable(
        ReserveData storage _reserve,
        uint256 _amount
    ) internal {
        _reserve.totalBorrowsVariable = _reserve.totalBorrowsVariable.add(
            _amount
        );
    }

    /**
     * @dev decreases the total borrows at a variable rate
     * @param _reserve the reserve object
     * @param _amount the amount to substract to the total borrows variable
     **/
    function decreaseTotalBorrowsVariable(
        ReserveData storage _reserve,
        uint256 _amount
    ) internal {
        require(
            _reserve.totalBorrowsVariable >= _amount,
            "The amount that is being subtracted from the variable total borrows is incorrect"
        );
        _reserve.totalBorrowsVariable = _reserve.totalBorrowsVariable.sub(
            _amount
        );
    }

    /**
     * @dev function to calculate the interest using a linear interest rate
     * formula
     * @param _rate the interest rate, in ray
     * @param _lastUpdateTimestamp the timestamp of the last update of the
     * interest
     * @return the interest rate linearly accumulated during the timeDelta, in
     * ray
     **/

    function calculateLinearInterest(uint256 _rate, uint40 _lastUpdateTimestamp)
        internal
        view
        returns (uint256)
    {
        //solium-disable-next-line
        uint256 timeDifference =
            block.timestamp.sub(uint256(_lastUpdateTimestamp));

        uint256 timeDelta =
            timeDifference.wadToRay().rayDiv(SECONDS_PER_YEAR.wadToRay());

        return _rate.rayMul(timeDelta).add(WadRayMath.ray());
    }

    /**
     * @dev function to calculate the interest using a compounded interest rate
     * formula
     * @param _rate the interest rate, in ray
     * @param _lastUpdateTimestamp the timestamp of the last update of the
     * interest
     * @return the interest rate compounded during the timeDelta, in ray
     **/
    function calculateCompoundedInterest(
        uint256 _rate,
        uint40 _lastUpdateTimestamp
    ) internal view returns (uint256) {
        //solium-disable-next-line
        uint256 timeDifference =
            block.timestamp.sub(uint256(_lastUpdateTimestamp));

        uint256 ratePerSecond = _rate.div(SECONDS_PER_YEAR);

        return ratePerSecond.add(WadRayMath.ray()).rayPow(timeDifference);
    }

    /**
     * @dev returns the total borrows on the reserve
     * @param _reserve the reserve object
     * @return the total borrows (stable + variable)
     **/
    function getTotalBorrows(CoreLibrary.ReserveData storage _reserve)
        internal
        view
        returns (uint256)
    {
        return _reserve.totalBorrowsStable.add(_reserve.totalBorrowsVariable);
    }

    function exceed99Percent(uint256 _value, uint256 _baseValue)
        internal
        pure
        returns (bool)
    {
        return (_value.wadDiv(_baseValue) > 0.99e18);
    }
}



interface IRewardVault {

    function withdraw(address _token, address payable _to, uint256 _amount)  external ;
}

















/**
 * @title FeeProvider contract
 * @notice Implements calculation for the fees applied by the protocol
 * @author Multiplier Finance
 **/
contract FeeProvider is IFeeProvider, VersionedInitializable {
    using SafeMath for uint256;
    using WadRayMath for uint256;

    // percentage of the fee to be calculated on the loan amount
    uint256 private constant originationFeeRate = 0.00001 * 1e18; // 100% = 1e18;
    uint256 private constant flashloanFeeRate = 0.0006 * 1e18;
    
    // 70/20/10% reward rates //
    uint256 private constant supplierRewardRate = 0.7 * 1e18;
    uint256 private constant governanceRewardRate = 0.2 * 1e18;
    // uint256 private safetyModuleRewardRate; // The remaining of 1 - supplierRewardRate - governanceRewardRate
    

    uint256 public constant FEE_PROVIDER_REVISION = 0x1;

    function getRevision() internal pure returns (uint256) {
        return FEE_PROVIDER_REVISION;
    }


    /**
     * @dev initializes the FeeProvider after it's added to the proxy
     * @param _addressesProvider the address of the LendingPoolAddressesProvider
     */
    function initialize(address _addressesProvider) public initializer {
    }

    /**
     * @dev calculates the origination fee for every loan executed on the
     * platform.
     * @param _amount the amount of the loan
     **/
    function calculateLoanOriginationFee(uint256 _amount)
        external
        view
        returns (uint256)
    {
        return _amount.wadMul(originationFeeRate);
    }

    /**
     * @dev returns the origination fee percentage
     **/
    function calculateRewards(uint256 _originationFee) external view returns (uint256, uint256, uint256) {
        
        // Checks to prevent improper percentages
        require (supplierRewardRate.add(governanceRewardRate) <= 1e18, "Invalid Fees configurations");

        uint256 supplierReward = _originationFee.wadMul(supplierRewardRate);
        uint256 govtReward = _originationFee.wadMul(governanceRewardRate);
        
        return (supplierReward, govtReward, _originationFee - supplierReward - govtReward);
        
    }
  
    function getFeeRates() external view returns (uint256, uint256) {
        return (originationFeeRate, flashloanFeeRate);
    }

    function getFlashLoanFee() external view returns (uint256) {
        return flashloanFeeRate;
    }
    
    function getRewardRates() external view returns (uint256, uint256, uint256) {
        uint256 safetyModuleRate = 1e18 - supplierRewardRate - governanceRewardRate;
        return (supplierRewardRate, governanceRewardRate, safetyModuleRate);
    }
    
}








/**
* @title LendingPoolCore contract
@author Multiplier Finance
* @notice Holds the state of the lending pool and all the funds deposited
* @dev NOTE: The core does not enforce security checks on the update of the
* state
* (eg, updateStateOnBorrow() does not enforce that borrowed is enabled on the
* reserve).
* The check that an action can be performed is a duty of the overlying
* LendingPool contract.
**/

contract LendingPoolCore is VersionedInitializable {
    using SafeMath for uint256;
    using WadRayMath for uint256;
    using CoreLibrary for CoreLibrary.ReserveData;
    using CoreLibrary for CoreLibrary.UserReserveData;
    using SafeERC20 for ERC20;
    using Address for address payable;

    /**
     * @dev Emitted when the state of a reserve is updated
     * @param reserve the address of the reserve
     * @param liquidityRate the new liquidity rate
     * @param stableBorrowRate the new stable borrow rate
     * @param variableBorrowRate the new variable borrow rate
     * @param liquidityIndex the new liquidity index
     * @param variableBorrowIndex the new variable borrow index
     **/
    event ReserveUpdated(
        address indexed reserve,
        uint256 liquidityRate,
        uint256 stableBorrowRate,
        uint256 variableBorrowRate,
        uint256 liquidityIndex,
        uint256 variableBorrowIndex
    );

    address public lendingPoolAddress;

    LendingPoolAddressesProvider public addressesProvider;
    LendingPoolParametersProvider public parametersProvider;
    IFeeProvider public feeProvider;
    RewardsManager public rewardManager;

    /**
     * @dev only lending pools can use functions affected by this modifier
     **/
    modifier onlyLendingPool {
        require(
            lendingPoolAddress == msg.sender,
            "The caller must be a lending pool contract"
        );
        _;
    }

    /**
     * @dev only lending pools configurator can use functions affected by this
     * modifier
     **/
    modifier onlyLendingPoolConfigurator {
        require(
            addressesProvider.getLendingPoolConfigurator() == msg.sender,
            "The caller must be a lending pool configurator contract"
        );
        _;
    }

    mapping(address => CoreLibrary.ReserveData) internal reserves;
    mapping(address => mapping(address => CoreLibrary.UserReserveData))
        internal usersReserveData;

    address[] public reservesList;

    uint256 public constant CORE_REVISION = 0x1;

    /**
     * @dev returns the revision number of the contract
     **/
    function getRevision() internal pure returns (uint256) {
        return CORE_REVISION;
    }

    /**
     * @dev initializes the Core contract, invoked upon registration on the
     * AddressesProvider
     * @param _addresessProvider the addressesProvider contract
     **/

    function initialize(LendingPoolAddressesProvider _addresessProvider)
        public
        initializer
    {
        addressesProvider = _addresessProvider;
        refreshConfigInternal();
    }

    /**
     * @dev updates the state of the core as a result of a deposit action
     * @param _reserve the address of the reserve in which the deposit is
     * happening
     * @param _user the address of the the user depositing
     * @param _amount the amount being deposited
     * @param _isFirstDeposit true if the user is depositing for the first time
     **/

    function updateStateOnDeposit(
        address _reserve,
        address _user,
        uint256 _amount,
        bool _isFirstDeposit
    ) external onlyLendingPool {
        reserves[_reserve].updateCumulativeIndexes();
        updateReserveInterestRatesAndTimestampInternal(_reserve, _amount, 0);

        if (_isFirstDeposit) {
            // if this is the first deposit of the user, we configure the
            // deposit as enabled to be used as collateral
            setUserUseReserveAsCollateral(_reserve, _user, true);
        }
    }

    /**
     * @dev updates the state of the core as a result of a redeem action
     * @param _reserve the address of the reserve in which the redeem is
     * happening
     * @param _user the address of the the user redeeming
     * @param _amountRedeemed the amount being redeemed
     * @param _userRedeemedEverything true if the user is redeeming everything
     **/
    function updateStateOnRedeem(
        address _reserve,
        address _user,
        uint256 _amountRedeemed,
        bool _userRedeemedEverything
    ) external onlyLendingPool {
        //compound liquidity and variable borrow interests
        reserves[_reserve].updateCumulativeIndexes();
        updateReserveInterestRatesAndTimestampInternal(
            _reserve,
            0,
            _amountRedeemed
        );

        //if user redeemed everything the useReserveAsCollateral flag is reset
        if (_userRedeemedEverything) {
            setUserUseReserveAsCollateral(_reserve, _user, false);
        }
    }

    /**
     * @dev updates the state of the core as a result of a flashloan action
     * @param _reserve the address of the reserve in which the flashloan is
     * happening
     **/

    function updateStateOnFlashLoan(address _reserve) external onlyLendingPool {
        //compounding the cumulated interest
        reserves[_reserve].updateCumulativeIndexes();

        // Note: Since all flashloan fees are distributed, the following aave codes are commented off
        //uint256 totalLiquidityBefore = _availableLiquidityBefore.add( getReserveTotalBorrows(_reserve));
        //compounding the received fee into the reserve
        //reserves[_reserve].cumulateToLiquidityIndex(totalLiquidityBefore, _income);
        //refresh interest rates
        // updateReserveInterestRatesAndTimestampInternal(_reserve, _income, 0);
    }

    /**
     * @dev updates the state of the core as a consequence of a borrow action.
     * @param _reserve the address of the reserve on which the user is borrowing
     * @param _user the address of the borrower
     * @param _amountBorrowed the new amount borrowed
     * @param _borrowFee the fee on the amount borrowed
     * @param _rateMode the borrow rate mode (stable, variable)
     * @return the new borrow rate for the user
     **/
    function updateStateOnBorrow(
        address _reserve,
        address _user,
        uint256 _amountBorrowed,
        uint256 _borrowFee,
        CoreLibrary.InterestRateMode _rateMode
    ) external onlyLendingPool returns (uint256, uint256) {
        // getting the previous borrow data of the user
        (uint256 principalBorrowBalance, , uint256 balanceIncrease) =
            getUserBorrowBalances(_reserve, _user);

        updateReserveStateOnBorrowInternal(
            _reserve,
            _user,
            principalBorrowBalance,
            balanceIncrease,
            _amountBorrowed,
            _rateMode
        );

        updateUserStateOnBorrowInternal(
            _reserve,
            _user,
            _amountBorrowed,
            balanceIncrease,
            _borrowFee,
            _rateMode
        );

        updateReserveInterestRatesAndTimestampInternal(
            _reserve,
            0,
            _amountBorrowed
        );

        return (getUserCurrentBorrowRate(_reserve, _user), balanceIncrease);
    }

    /**
     * @dev updates the state of the core as a consequence of a repay action.
     * @param _reserve the address of the reserve on which the user is repaying
     * @param _user the address of the borrower
     * @param _paybackAmountMinusFees the amount being paid back minus fees
     * @param _originationFeeRepaid the fee on the amount that is being repaid
     * @param _balanceIncrease the accrued interest on the borrowed amount
     * @param _repaidWholeLoan true if the user is repaying the whole loan
     **/

    function updateStateOnRepay(
        address _reserve,
        address _user,
        uint256 _paybackAmountMinusFees,
        uint256 _originationFeeRepaid,
        uint256 _balanceIncrease,
        bool _repaidWholeLoan
    ) external onlyLendingPool {
        updateReserveStateOnRepayInternal(
            _reserve,
            _user,
            _paybackAmountMinusFees,
            _balanceIncrease
        );
        updateUserStateOnRepayInternal(
            _reserve,
            _user,
            _paybackAmountMinusFees,
            _originationFeeRepaid,
            _balanceIncrease,
            _repaidWholeLoan
        );

        updateReserveInterestRatesAndTimestampInternal(
            _reserve,
            _paybackAmountMinusFees,
            0
        );
    }

    /**
     * @dev updates the state of the core as a consequence of a swap rate action.
     * @param _reserve the address of the reserve on which the user is repaying
     * @param _user the address of the borrower
     * @param _principalBorrowBalance the amount borrowed by the user
     * @param _compoundedBorrowBalance the amount borrowed plus accrued interest
     * @param _balanceIncrease the accrued interest on the borrowed amount
     * @param _currentRateMode the current interest rate mode for the user
     **/
    function updateStateOnSwapRate(
        address _reserve,
        address _user,
        uint256 _principalBorrowBalance,
        uint256 _compoundedBorrowBalance,
        uint256 _balanceIncrease,
        CoreLibrary.InterestRateMode _currentRateMode
    ) external onlyLendingPool returns (CoreLibrary.InterestRateMode, uint256) {
        updateReserveStateOnSwapRateInternal(
            _reserve,
            _user,
            _principalBorrowBalance,
            _compoundedBorrowBalance,
            _currentRateMode
        );

        CoreLibrary.InterestRateMode newRateMode =
            updateUserStateOnSwapRateInternal(
                _reserve,
                _user,
                _balanceIncrease,
                _currentRateMode
            );

        updateReserveInterestRatesAndTimestampInternal(_reserve, 0, 0);

        return (newRateMode, getUserCurrentBorrowRate(_reserve, _user));
    }

    /**
     * @dev updates the state of the core as a consequence of a liquidation
     * action.
     * @param _principalReserve the address of the principal reserve that is
     * being repaid
     * @param _collateralReserve the address of the collateral reserve that is
     * being liquidated
     * @param _user the address of the borrower
     * @param _amountToLiquidate the amount being repaid by the liquidator
     * @param _collateralToLiquidate the amount of collateral being liquidated
     * @param _feeLiquidated the amount of origination fee being liquidated
     * @param _liquidatedCollateralForFee the amount of collateral equivalent to
     * the origination fee + bonus
     * @param _balanceIncrease the accrued interest on the borrowed amount
     * @param _liquidatorReceivesMToken true if the liquidator will receive
     * Tokens, false otherwise
     **/
    function updateStateOnLiquidation(
        address _principalReserve,
        address _collateralReserve,
        address _user,
        uint256 _amountToLiquidate,
        uint256 _collateralToLiquidate,
        uint256 _feeLiquidated,
        uint256 _liquidatedCollateralForFee,
        uint256 _balanceIncrease,
        bool _liquidatorReceivesMToken
    ) external onlyLendingPool {
        updatePrincipalReserveStateOnLiquidationInternal(
            _principalReserve,
            _user,
            _amountToLiquidate,
            _balanceIncrease
        );

        updateCollateralReserveStateOnLiquidationInternal(_collateralReserve);

        updateUserStateOnLiquidationInternal(
            _principalReserve,
            _user,
            _amountToLiquidate,
            _feeLiquidated,
            _balanceIncrease
        );

        updateReserveInterestRatesAndTimestampInternal(
            _principalReserve,
            _amountToLiquidate,
            0
        );

        if (!_liquidatorReceivesMToken) {
            updateReserveInterestRatesAndTimestampInternal(
                _collateralReserve,
                0,
                _collateralToLiquidate.add(_liquidatedCollateralForFee)
            );
        }
    }

    /**
     * @dev updates the state of the core as a consequence of a stable rate
     * rebalance
     * @param _reserve the address of the principal reserve where the user
     * borrowed
     * @param _user the address of the borrower
     * @param _balanceIncrease the accrued interest on the borrowed amount
     * @return the new stable rate for the user
     **/
    function updateStateOnRebalance(
        address _reserve,
        address _user,
        uint256 _balanceIncrease
    ) external onlyLendingPool returns (uint256) {
        updateReserveStateOnRebalanceInternal(
            _reserve,
            _user,
            _balanceIncrease
        );

        //update user data and rebalance the rate
        updateUserStateOnRebalanceInternal(_reserve, _user, _balanceIncrease);
        updateReserveInterestRatesAndTimestampInternal(_reserve, 0, 0);
        return usersReserveData[_user][_reserve].stableBorrowRate;
    }

    /**
     * @dev enables or disables a reserve as collateral
     * @param _reserve the address of the principal reserve where the user
     * deposited
     * @param _user the address of the depositor
     * @param _useAsCollateral true if the depositor wants to use the reserve as
     * collateral
     **/
    function setUserUseReserveAsCollateral(
        address _reserve,
        address _user,
        bool _useAsCollateral
    ) public onlyLendingPool {
        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];
        user.useAsCollateral = _useAsCollateral;
    }

    /**
     * @notice BNB/token transfer functions
     **/

    /**
     * @dev fallback function enforces that the caller is a contract, to support
     * flashloan transfers
     **/
    function() external payable {
        //only contracts can send BNB to the core
        require(
            msg.sender.isContract(),
            "Only contracts can send bnb to the Lending pool core"
        );
    }

    /**
     * @dev transfers to the user a specific amount from the reserve.
     * @param _reserve the address of the reserve where the transfer is happening
     * @param _user the address of the user receiving the transfer
     * @param _amount the amount being transferred
     **/
    function transferToUser(
        address _reserve,
        address payable _user,
        uint256 _amount
    ) external onlyLendingPool {
        if (_reserve != BscAddressLib.bnbAddress()) {
            ERC20(_reserve).safeTransfer(_user, _amount);
        } else {
            //solium-disable-next-line
            (bool result, ) = _user.call.value(_amount).gas(50000)("");
            require(result, "Transfer of bnb failed");
        }
    }

    struct RewardsLocalVars {
        uint256 supplier;
        uint256 governance;
        uint256 safetyModule;
    }

    /**
     * @dev transfers the protocol fees to the fees collection address
     * @param _token the address of the token being transferred
     * @param _user the address of the user from where the transfer is happening
     * @param _amount the amount being transferred
     **/

    function transferToFeeCollectionAddress(
        address _token,
        address _user,
        uint256 _amount,
        bool _transferFromCore
    ) external payable onlyLendingPool {
        distributeFeestoVaults(_token, _user, _amount, _transferFromCore);
    }

    /**
     * @dev transfers the fees to the fees collection address in the case of
     * liquidation
     * @param _token the address of the token being transferred
     * @param _amount the amount being transferred
     **/

    function liquidateFee(address _token, uint256 _amount)
        external
        payable
        onlyLendingPool
    {
        require(
            msg.value == 0,
            "Fee liquidation does not require any transfer of value"
        );

        if (_amount == 0) {
            return;
        }

        // When liquidation happens and there is a part of the origination fee being sent here for reward distribution.
        (uint256 supplierAmt, uint256 govtAmt, ) =
            feeProvider.calculateRewards(_amount);

        // Create a Reward Item and send fees to vault //
        uint256 liquidity = getTotalmTokenSupply(_token);
        rewardManager.addRewardItem(_token, supplierAmt, liquidity, govtAmt);

        distributeFeestoVaults(_token, address(this), _amount, true);
    }

    function distributeFeestoVaults(
        address _token,
        address _user,
        uint256 _amount,
        bool _transferFromCore
    ) internal {
        RewardsLocalVars memory reward;
        (reward.supplier, reward.governance, reward.safetyModule) = feeProvider
            .calculateRewards(_amount);

        address payable toLpVault =
            address(uint160(addressesProvider.getLpRewardVault()));
        address payable toGovtVault =
            address(uint160(addressesProvider.getGovRewardVault()));
        address payable toSafetyVault =
            address(uint160(addressesProvider.getSafetyRewardVault()));

        // Transfer to reward vaults //
        if (_token != BscAddressLib.bnbAddress()) {
            require(
                msg.value == 0,
                "User is sending bnb along with the ERC20 transfer. Check the value attribute of the transaction"
            );

            ERC20 token = ERC20(_token);

            if (_transferFromCore) {
                if (reward.supplier != 0) {
                    token.safeTransfer(toLpVault, reward.supplier);
                }
                if (reward.governance != 0) {
                    token.safeTransfer(toGovtVault, reward.governance);
                }
                if (reward.safetyModule != 0) {
                    token.safeTransfer(toSafetyVault, reward.safetyModule);
                }
            } else {
                if (reward.supplier != 0) {
                    token.safeTransferFrom(_user, toLpVault, reward.supplier);
                }
                if (reward.governance != 0) {
                    token.safeTransferFrom(
                        _user,
                        toGovtVault,
                        reward.governance
                    );
                }
                if (reward.safetyModule != 0) {
                    token.safeTransferFrom(
                        _user,
                        toSafetyVault,
                        reward.safetyModule
                    );
                }
            }
        } else {
            // require(msg.value >= _amount, "The amount and the value sent do not match");

            if (reward.supplier != 0) {
                (bool result, ) =
                    toLpVault.call.value(reward.supplier).gas(50000)("");
                require(result, "Transfer of bnb failed");
            }

            if (reward.governance != 0) {
                (bool result, ) =
                    toGovtVault.call.value(reward.governance).gas(50000)("");
                require(result, "Transfer of bnb failed");
            }

            if (reward.safetyModule != 0) {
                (bool result, ) =
                    toSafetyVault.call.value(reward.safetyModule).gas(50000)(
                        ""
                    );
                require(result, "Transfer of bnb failed");
            }
        }
    }

    /**
     * @dev transfers an amount from a user to the destination reserve
     * @param _reserve the address of the reserve where the amount is being
     * transferred
     * @param _user the address of the user from where the transfer is happening
     * @param _amount the amount being transferred
     **/

    function transferToReserve(
        address _reserve,
        address payable _user,
        uint256 _amount
    ) external payable onlyLendingPool {
        if (_reserve != BscAddressLib.bnbAddress()) {
            require(
                msg.value == 0,
                "User is sending bnb along with the ERC20 transfer."
            );
            ERC20(_reserve).safeTransferFrom(_user, address(this), _amount);
        } else {
            require(
                msg.value >= _amount,
                "The amount and the value sent to deposit do not match"
            );

            if (msg.value > _amount) {
                //send back excess bnb
                uint256 excessAmount = msg.value.sub(_amount);
                //solium-disable-next-line
                (bool result, ) = _user.call.value(excessAmount).gas(50000)("");
                require(result, "Transfer of bnb failed");
            }
        }
    }

    /**
     * @notice data access functions
     **/

    /**
     * @dev returns the basic data (balances, fee accrued, reserve enabled/
     * disabled as collateral)
     * needed to calculate the global account data in the LendingPoolDataProvider
     * @param _reserve the address of the reserve
     * @param _user the address of the user
     * @return the user deposited balance, the principal borrow balance, the
     * fee, and if the reserve is enabled as collateral or not
     **/
    function getUserBasicReserveData(address _reserve, address _user)
        external
        view
        returns (
            uint256,
            uint256,
            uint256,
            bool
        )
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];

        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];

        uint256 underlyingBalance =
            getUserUnderlyingAssetBalance(_reserve, _user);

        if (user.principalBorrowBalance == 0) {
            return (underlyingBalance, 0, 0, user.useAsCollateral);
        }

        return (
            underlyingBalance,
            user.getCompoundedBorrowBalance(reserve),
            user.originationFee,
            user.useAsCollateral
        );
    }

    /**
     * @dev checks if a user is allowed to borrow at a stable rate
     * @param _reserve the reserve address
     * @param _user the user
     * @param _amount the amount the the user wants to borrow
     * @return true if the user is allowed to borrow at a stable rate, false
     * otherwise
     **/

    function isUserAllowedToBorrowAtStable(
        address _reserve,
        address _user,
        uint256 _amount
    ) external view returns (bool) {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];

        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];

        if (!reserve.isStableBorrowRateEnabled) return false;

        return
            !user.useAsCollateral ||
            !reserve.usageAsCollateralEnabled ||
            _amount > getUserUnderlyingAssetBalance(_reserve, _user);
    }

    /**
     * @dev gets the underlying asset balance of a user based on the
     * corresponding mToken balance.
     * @param _reserve the reserve address
     * @param _user the user address
     * @return the underlying deposit balance of the user
     **/

    function getUserUnderlyingAssetBalance(address _reserve, address _user)
        public
        view
        returns (uint256)
    {
        mToken mToken = mToken(reserves[_reserve].mTokenAddress);
        return mToken.balanceOf(_user);
    }

    function getUsermTokenBalance(address _reserve, address _user)
        public
        view
        returns (uint256)
    {
        mToken mToken = mToken(reserves[_reserve].mTokenAddress);
        return mToken.principalBalanceOf(_user);
    }

    function getUserStakedTokenBalance(address _user)
        public
        view
        returns (uint256)
    {
        address token = addressesProvider.getStakingToken();
        return ERC20(token).balanceOf(_user);
    }

    /**
     * @dev gets the interest rate strategy contract address for the reserve
     * @param _reserve the reserve address
     * @return the address of the interest rate strategy contract
     **/
    function getReserveInterestRateStrategyAddress(address _reserve)
        public
        view
        returns (address)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return reserve.interestRateStrategyAddress;
    }

    /**
     * @dev gets the mToken contract address for the reserve
     * @param _reserve the reserve address
     * @return the address of the mToken contract
     **/

    function getReservemTokenAddress(address _reserve)
        public
        view
        returns (address)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return reserve.mTokenAddress;
    }

    /**
     * @dev gets the available liquidity in the reserve. The available liquidity
     * is the balance of the core contract
     * @param _reserve the reserve address
     * @return the available liquidity
     **/
    function getReserveAvailableLiquidity(address _reserve)
        public
        view
        returns (uint256)
    {
        uint256 balance = 0;

        if (_reserve == BscAddressLib.bnbAddress()) {
            balance = address(this).balance;
        } else {
            balance = IERC20(_reserve).balanceOf(address(this));
        }
        return balance;
    }

    /**
     * @dev gets the total liquidity in the reserve. The total liquidity is the
     * balance of the core contract + total borrows
     * @param _reserve the reserve address
     * @return the total liquidity
     **/
    function getReserveTotalLiquidity(address _reserve)
        public
        view
        returns (uint256)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return
            getReserveAvailableLiquidity(_reserve).add(
                reserve.getTotalBorrows()
            );
    }

    /**
     * @dev gets the normalized income of the reserve. a value of 1e27 means
     * there is no income. A value of 2e27 means there
     * there has been 100% income.
     * @param _reserve the reserve address
     * @return the reserve normalized income
     **/
    function getReserveNormalizedIncome(address _reserve)
        external
        view
        returns (uint256)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return reserve.getNormalizedIncome();
    }

    /**
     * @dev gets the reserve total borrows
     * @param _reserve the reserve address
     * @return the total borrows (stable + variable)
     **/
    function getReserveTotalBorrows(address _reserve)
        public
        view
        returns (uint256)
    {
        return reserves[_reserve].getTotalBorrows();
    }

    /**
     * @dev gets the reserve total borrows stable
     * @param _reserve the reserve address
     * @return the total borrows stable
     **/
    function getReserveTotalBorrowsStable(address _reserve)
        external
        view
        returns (uint256)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return reserve.totalBorrowsStable;
    }

    /**
     * @dev gets the reserve total borrows variable
     * @param _reserve the reserve address
     * @return the total borrows variable
     **/

    function getReserveTotalBorrowsVariable(address _reserve)
        external
        view
        returns (uint256)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return reserve.totalBorrowsVariable;
    }

    /**
     * @dev gets the reserve liquidation threshold
     * @param _reserve the reserve address
     * @return the reserve liquidation threshold
     **/

    function getReserveLiquidationThreshold(address _reserve)
        external
        view
        returns (uint256)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return reserve.liquidationThreshold;
    }

    /**
     * @dev gets the reserve liquidation bonus
     * @param _reserve the reserve address
     * @return the reserve liquidation bonus
     **/

    function getReserveLiquidationBonus(address _reserve)
        external
        view
        returns (uint256)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return reserve.liquidationBonus;
    }

    /**
     * @dev gets the reserve current variable borrow rate. Is the base variable
     * borrow rate if the reserve is empty
     * @param _reserve the reserve address
     * @return the reserve current variable borrow rate
     **/

    function getReserveCurrentVariableBorrowRate(address _reserve)
        external
        view
        returns (uint256)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];

        if (reserve.currentVariableBorrowRate == 0) {
            return
                IReserveInterestRateStrategy(
                    reserve
                        .interestRateStrategyAddress
                )
                    .getBaseVariableBorrowRate();
        }
        return reserve.currentVariableBorrowRate;
    }

    /**
     * @dev gets the reserve current stable borrow rate. Is the market rate if
     * the reserve is empty
     * @param _reserve the reserve address
     * @return the reserve current stable borrow rate
     **/

    function getReserveCurrentStableBorrowRate(address _reserve)
        public
        view
        returns (uint256)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        ILendingRateOracle oracle =
            ILendingRateOracle(addressesProvider.getLendingRateOracle());

        if (reserve.currentStableBorrowRate == 0) {
            //no stable rate borrows yet
            return oracle.getMarketBorrowRate(_reserve);
        }

        return reserve.currentStableBorrowRate;
    }

    /**
     * @dev gets the reserve average stable borrow rate. The average stable rate
     * is the weighted average
     * of all the loans taken at stable rate.
     * @param _reserve the reserve address
     * @return the reserve current average borrow rate
     **/
    function getReserveCurrentAverageStableBorrowRate(address _reserve)
        external
        view
        returns (uint256)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return reserve.currentAverageStableBorrowRate;
    }

    /**
     * @dev gets the reserve liquidity rate
     * @param _reserve the reserve address
     * @return the reserve liquidity rate
     **/
    function getReserveCurrentLiquidityRate(address _reserve)
        external
        view
        returns (uint256)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return reserve.currentLiquidityRate;
    }

    /**
     * @dev gets the reserve liquidity cumulative index
     * @param _reserve the reserve address
     * @return the reserve liquidity cumulative index
     **/
    function getReserveLiquidityCumulativeIndex(address _reserve)
        external
        view
        returns (uint256)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return reserve.lastLiquidityCumulativeIndex;
    }

    /**
     * @dev gets the reserve variable borrow index
     * @param _reserve the reserve address
     * @return the reserve variable borrow index
     **/
    function getReserveVariableBorrowsCumulativeIndex(address _reserve)
        external
        view
        returns (uint256)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return reserve.lastVariableBorrowCumulativeIndex;
    }

    /**
     * @dev this function aggregates the configuration parameters of the reserve.
     * It's used in the LendingPoolDataProvider specifically to save gas, and
     * avoid multiple external contract calls to fetch the same data.
     * @param _reserve the reserve address
     * @return the reserve decimals
     * @return the base ltv as collateral
     * @return the liquidation threshold
     * @return if the reserve is used as collateral or not
     **/
    function getReserveConfiguration(address _reserve)
        external
        view
        returns (
            uint256,
            uint256,
            uint256,
            bool
        )
    {
        uint256 decimals;
        uint256 baseLTVasCollateral;
        uint256 liquidationThreshold;
        bool usageAsCollateralEnabled;

        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        decimals = reserve.decimals;
        baseLTVasCollateral = reserve.baseLTVasCollateral;
        liquidationThreshold = reserve.liquidationThreshold;
        usageAsCollateralEnabled = reserve.usageAsCollateralEnabled;

        return (
            decimals,
            baseLTVasCollateral,
            liquidationThreshold,
            usageAsCollateralEnabled
        );
    }

    /**
     * @dev returns the decimals of the reserve
     * @param _reserve the reserve address
     * @return the reserve decimals
     **/
    function getReserveDecimals(address _reserve)
        external
        view
        returns (uint256)
    {
        return reserves[_reserve].decimals;
    }

    /**
     * @dev returns true if the reserve is enabled for borrowing
     * @param _reserve the reserve address
     * @return true if the reserve is enabled for borrowing, false otherwise
     **/

    function isReserveBorrowingEnabled(address _reserve)
        external
        view
        returns (bool)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return reserve.borrowingEnabled;
    }

    /**
     * @dev returns true if the reserve is enabled as collateral
     * @param _reserve the reserve address
     * @return true if the reserve is enabled as collateral, false otherwise
     **/

    function isReserveUsageAsCollateralEnabled(address _reserve)
        external
        view
        returns (bool)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return reserve.usageAsCollateralEnabled;
    }

    /**
     * @dev returns true if the stable rate is enabled on reserve
     * @param _reserve the reserve address
     * @return true if the stable rate is enabled on reserve, false otherwise
     **/
    function getReserveIsStableBorrowRateEnabled(address _reserve)
        external
        view
        returns (bool)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return reserve.isStableBorrowRateEnabled;
    }

    /**
     * @dev returns true if the reserve is active
     * @param _reserve the reserve address
     * @return true if the reserve is active, false otherwise
     **/
    function getReserveIsActive(address _reserve) external view returns (bool) {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return reserve.isActive;
    }

    /**
     * @notice returns if a reserve is freezed
     * @param _reserve the reserve for which the information is needed
     * @return true if the reserve is freezed, false otherwise
     **/

    function getReserveIsFreezed(address _reserve)
        external
        view
        returns (bool)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        return reserve.isFreezed;
    }

    /**
     * @notice returns the timestamp of the last action on the reserve
     * @param _reserve the reserve for which the information is needed
     * @return the last updated timestamp of the reserve
     **/

    function getReserveLastUpdate(address _reserve)
        external
        view
        returns (uint40 timestamp)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        timestamp = reserve.lastUpdateTimestamp;
    }

    /**
     * @dev returns the utilization rate U of a specific reserve
     * @param _reserve the reserve for which the information is needed
     * @return the utilization rate in ray
     **/

    function getReserveUtilizationRate(address _reserve)
        public
        view
        returns (uint256)
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];

        uint256 totalBorrows = reserve.getTotalBorrows();

        if (totalBorrows == 0) {
            return 0;
        }

        uint256 availableLiquidity = getReserveAvailableLiquidity(_reserve);

        return totalBorrows.rayDiv(availableLiquidity.add(totalBorrows));
    }

    /**
     * @return the array of reserves configured on the core
     **/
    function getReserves() external view returns (address[] memory) {
        return reservesList;
    }

    /**
     * @param _reserve the address of the reserve for which the information is
     * needed
     * @param _user the address of the user for which the information is needed
     * @return true if the user has chosen to use the reserve as collateral,
     * false otherwise
     **/
    function isUserUseReserveAsCollateralEnabled(
        address _reserve,
        address _user
    ) external view returns (bool) {
        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];
        return user.useAsCollateral;
    }

    /**
     * @param _reserve the address of the reserve for which the information is
     * needed
     * @param _user the address of the user for which the information is needed
     * @return the origination fee for the user
     **/
    function getUserOriginationFee(address _reserve, address _user)
        external
        view
        returns (uint256)
    {
        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];
        return user.originationFee;
    }

    /**
     * @dev users with no loans in progress have NONE as borrow rate mode
     * @param _reserve the address of the reserve for which the information is
     * needed
     * @param _user the address of the user for which the information is needed
     * @return the borrow rate mode for the user,
     **/

    function getUserCurrentBorrowRateMode(address _reserve, address _user)
        public
        view
        returns (CoreLibrary.InterestRateMode)
    {
        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];

        if (user.principalBorrowBalance == 0) {
            return CoreLibrary.InterestRateMode.NONE;
        }

        return
            user.stableBorrowRate > 0
                ? CoreLibrary.InterestRateMode.STABLE
                : CoreLibrary.InterestRateMode.VARIABLE;
    }

    /**
     * @dev gets the current borrow rate of the user
     * @param _reserve the address of the reserve for which the information is
     * needed
     * @param _user the address of the user for which the information is needed
     * @return the borrow rate for the user,
     **/
    function getUserCurrentBorrowRate(address _reserve, address _user)
        internal
        view
        returns (uint256)
    {
        CoreLibrary.InterestRateMode rateMode =
            getUserCurrentBorrowRateMode(_reserve, _user);

        if (rateMode == CoreLibrary.InterestRateMode.NONE) {
            return 0;
        }

        return
            rateMode == CoreLibrary.InterestRateMode.STABLE
                ? usersReserveData[_user][_reserve].stableBorrowRate
                : reserves[_reserve].currentVariableBorrowRate;
    }

    /**
     * @dev the stable rate returned is 0 if the user is borrowing at variable
     * or not borrowing at all
     * @param _reserve the address of the reserve for which the information is
     * needed
     * @param _user the address of the user for which the information is needed
     * @return the user stable rate
     **/
    function getUserCurrentStableBorrowRate(address _reserve, address _user)
        external
        view
        returns (uint256)
    {
        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];
        return user.stableBorrowRate;
    }

    /**
     * @dev calculates and returns the borrow balances of the user
     * @param _reserve the address of the reserve
     * @param _user the address of the user
     * @return the principal borrow balance, the compounded balance and the
     * balance increase since the last borrow/repay/swap/rebalance
     **/

    function getUserBorrowBalances(address _reserve, address _user)
        public
        view
        returns (
            uint256,
            uint256,
            uint256
        )
    {
        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];
        if (user.principalBorrowBalance == 0) {
            return (0, 0, 0);
        }

        uint256 principal = user.principalBorrowBalance;
        uint256 compoundedBalance =
            CoreLibrary.getCompoundedBorrowBalance(user, reserves[_reserve]);
        return (principal, compoundedBalance, compoundedBalance.sub(principal));
    }

    function getTotalmTokenSupply(address _reserve)
        public
        view
        returns (uint256)
    {
        mToken mToken = mToken(reserves[_reserve].mTokenAddress);
        return mToken.principalTotalSupply();
    }

    /**
     * @dev the variable borrow index of the user is 0 if the user is not
     * borrowing or borrowing at stable
     * @param _reserve the address of the reserve for which the information is
     * needed
     * @param _user the address of the user for which the information is needed
     * @return the variable borrow index for the user
     **/

    function getUserVariableBorrowCumulativeIndex(
        address _reserve,
        address _user
    ) external view returns (uint256) {
        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];
        return user.lastVariableBorrowCumulativeIndex;
    }

    /**
     * @dev the variable borrow index of the user is 0 if the user is not
     * borrowing or borrowing at stable
     * @param _reserve the address of the reserve for which the information is
     * needed
     * @param _user the address of the user for which the information is needed
     * @return the variable borrow index for the user
     **/

    function getUserLastUpdate(address _reserve, address _user)
        external
        view
        returns (uint256 timestamp)
    {
        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];
        timestamp = user.lastUpdateTimestamp;
    }

    /**
     * @dev updates the lending pool core configuration
     **/
    function refreshConfiguration() external onlyLendingPoolConfigurator {
        refreshConfigInternal();
    }

    /**
     * @dev initializes a reserve
     * @param _reserve the address of the reserve
     * @param _mTokenAddress the address of the overlying mToken contract
     * @param _decimals the decimals of the reserve currency
     * @param _interestRateStrategyAddress the address of the interest rate
     * strategy contract
     **/
    function initReserve(
        address _reserve,
        address _mTokenAddress,
        uint256 _decimals,
        address _interestRateStrategyAddress
    ) external onlyLendingPoolConfigurator {
        reserves[_reserve].init(
            _mTokenAddress,
            _decimals,
            _interestRateStrategyAddress
        );
        addReserveToListInternal(_reserve);
    }

    /**
     * @dev removes the last added reserve in the reservesList array
     * @param _reserveToRemove the address of the reserve
     **/
    function removeLastAddedReserve(address _reserveToRemove)
        external
        onlyLendingPoolConfigurator
    {
        address lastReserve = reservesList[reservesList.length - 1];

        require(
            lastReserve == _reserveToRemove,
            "Reserve being removed is different than the reserve requested"
        );

        // as we can't check if totalLiquidity is 0 (since the reserve added
        // might not be an ERC20) we at least check that there is nothing
        // borrowed
        require(
            getReserveTotalBorrows(lastReserve) == 0,
            "Cannot remove a reserve with liquidity deposited"
        );

        reserves[lastReserve].isActive = false;
        reserves[lastReserve].mTokenAddress = address(0);
        reserves[lastReserve].decimals = 0;
        reserves[lastReserve].lastLiquidityCumulativeIndex = 0;
        reserves[lastReserve].lastVariableBorrowCumulativeIndex = 0;
        reserves[lastReserve].borrowingEnabled = false;
        reserves[lastReserve].usageAsCollateralEnabled = false;
        reserves[lastReserve].baseLTVasCollateral = 0;
        reserves[lastReserve].liquidationThreshold = 0;
        reserves[lastReserve].liquidationBonus = 0;
        reserves[lastReserve].interestRateStrategyAddress = address(0);

        reservesList.pop();
    }

    /**
     * @dev updates the address of the interest rate strategy contract
     * @param _reserve the address of the reserve
     * @param _rateStrategyAddress the address of the interest rate strategy
     * contract
     **/

    function setReserveInterestRateStrategyAddress(
        address _reserve,
        address _rateStrategyAddress
    ) external onlyLendingPoolConfigurator {
        reserves[_reserve].interestRateStrategyAddress = _rateStrategyAddress;
    }

    /**
     * @dev enables borrowing on a reserve. Also sets the stable rate borrowing
     * @param _reserve the address of the reserve
     * @param _stableBorrowRateEnabled true if the stable rate needs to be
     * enabled, false otherwise
     **/

    function enableBorrowingOnReserve(
        address _reserve,
        bool _stableBorrowRateEnabled
    ) external onlyLendingPoolConfigurator {
        reserves[_reserve].enableBorrowing(_stableBorrowRateEnabled);
    }

    /**
     * @dev disables borrowing on a reserve
     * @param _reserve the address of the reserve
     **/

    function disableBorrowingOnReserve(address _reserve)
        external
        onlyLendingPoolConfigurator
    {
        reserves[_reserve].disableBorrowing();
    }

    /**
     * @dev enables a reserve to be used as collateral
     * @param _reserve the address of the reserve
     **/
    function enableReserveAsCollateral(
        address _reserve,
        uint256 _baseLTVasCollateral,
        uint256 _liquidationThreshold,
        uint256 _liquidationBonus
    ) external onlyLendingPoolConfigurator {
        reserves[_reserve].enableAsCollateral(
            _baseLTVasCollateral,
            _liquidationThreshold,
            _liquidationBonus
        );
    }

    /**
     * @dev disables a reserve to be used as collateral
     * @param _reserve the address of the reserve
     **/
    function disableReserveAsCollateral(address _reserve)
        external
        onlyLendingPoolConfigurator
    {
        reserves[_reserve].disableAsCollateral();
    }

    /**
     * @dev enable the stable borrow rate mode on a reserve
     * @param _reserve the address of the reserve
     **/
    function enableReserveStableBorrowRate(address _reserve)
        external
        onlyLendingPoolConfigurator
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        reserve.isStableBorrowRateEnabled = true;
    }

    /**
     * @dev disable the stable borrow rate mode on a reserve
     * @param _reserve the address of the reserve
     **/
    function disableReserveStableBorrowRate(address _reserve)
        external
        onlyLendingPoolConfigurator
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        reserve.isStableBorrowRateEnabled = false;
    }

    /**
     * @dev activates a reserve
     * @param _reserve the address of the reserve
     **/
    function activateReserve(address _reserve)
        external
        onlyLendingPoolConfigurator
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];

        require(
            reserve.lastLiquidityCumulativeIndex > 0 &&
                reserve.lastVariableBorrowCumulativeIndex > 0,
            "Reserve has not been initialized yet"
        );
        reserve.isActive = true;
    }

    /**
     * @dev deactivates a reserve
     * @param _reserve the address of the reserve
     **/
    function deactivateReserve(address _reserve)
        external
        onlyLendingPoolConfigurator
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        reserve.isActive = false;
    }

    /**
     * @notice allows the configurator to freeze the reserve.
     * A freezed reserve does not allow any action apart from repay, redeem,
     * liquidationCall, rebalance.
     * @param _reserve the address of the reserve
     **/
    function freezeReserve(address _reserve)
        external
        onlyLendingPoolConfigurator
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        reserve.isFreezed = true;
    }

    /**
     * @notice allows the configurator to unfreeze the reserve. A unfreezed
     * reserve allows any action to be executed.
     * @param _reserve the address of the reserve
     **/
    function unfreezeReserve(address _reserve)
        external
        onlyLendingPoolConfigurator
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        reserve.isFreezed = false;
    }

    /**
     * @notice allows the configurator to update the loan to value of a reserve
     * @param _reserve the address of the reserve
     * @param _ltv the new loan to value
     **/
    function setReserveBaseLTVasCollateral(address _reserve, uint256 _ltv)
        external
        onlyLendingPoolConfigurator
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        reserve.baseLTVasCollateral = _ltv;
    }

    /**
     * @notice allows the configurator to update the liquidation threshold of a
     * reserve
     * @param _reserve the address of the reserve
     * @param _threshold the new liquidation threshold
     **/
    function setReserveLiquidationThreshold(
        address _reserve,
        uint256 _threshold
    ) external onlyLendingPoolConfigurator {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        reserve.liquidationThreshold = _threshold;
    }

    /**
     * @notice allows the configurator to update the liquidation bonus of a
     * reserve
     * @param _reserve the address of the reserve
     * @param _bonus the new liquidation bonus
     **/
    function setReserveLiquidationBonus(address _reserve, uint256 _bonus)
        external
        onlyLendingPoolConfigurator
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        reserve.liquidationBonus = _bonus;
    }

    /**
     * @notice allows the configurator to update the reserve decimals
     * @param _reserve the address of the reserve
     * @param _decimals the decimals of the reserve
     **/
    function setReserveDecimals(address _reserve, uint256 _decimals)
        external
        onlyLendingPoolConfigurator
    {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        reserve.decimals = _decimals;
    }

    /**
     * @notice internal functions
     **/

    /**
     * @dev updates the state of a reserve as a consequence of a borrow action.
     * @param _reserve the address of the reserve on which the user is borrowing
     * @param _user the address of the borrower
     * @param _principalBorrowBalance the previous borrow balance of the
     * borrower before the action
     * @param _balanceIncrease the accrued interest of the user on the previous
     * borrowed amount
     * @param _amountBorrowed the new amount borrowed
     * @param _rateMode the borrow rate mode (stable, variable)
     **/

    function updateReserveStateOnBorrowInternal(
        address _reserve,
        address _user,
        uint256 _principalBorrowBalance,
        uint256 _balanceIncrease,
        uint256 _amountBorrowed,
        CoreLibrary.InterestRateMode _rateMode
    ) internal {
        reserves[_reserve].updateCumulativeIndexes();

        // Increasing reserve total borrows to account for the new borrow
        // balance of the user
        // NOTE: Depending on the previous borrow mode, the borrows might need
        // to be switched from variable to stable or vice versa

        updateReserveTotalBorrowsByRateModeInternal(
            _reserve,
            _user,
            _principalBorrowBalance,
            _balanceIncrease,
            _amountBorrowed,
            _rateMode
        );
    }

    /**
     * @dev updates the state of a user as a consequence of a borrow action.
     * @param _reserve the address of the reserve on which the user is borrowing
     * @param _user the address of the borrower
     * @param _amountBorrowed the amount borrowed
     * @param _balanceIncrease the accrued interest of the user on the previous
     * borrowed amount
     * @param _rateMode the borrow rate mode (stable, variable)
     * @return the final borrow rate for the user. Emitted by the borrow() event
     **/

    function updateUserStateOnBorrowInternal(
        address _reserve,
        address _user,
        uint256 _amountBorrowed,
        uint256 _balanceIncrease,
        uint256 _fee,
        CoreLibrary.InterestRateMode _rateMode
    ) internal {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];

        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];

        if (_rateMode == CoreLibrary.InterestRateMode.STABLE) {
            //stable
            //reset the user variable index, and update the stable rate
            user.stableBorrowRate = reserve.currentStableBorrowRate;
            user.lastVariableBorrowCumulativeIndex = 0;
        } else if (_rateMode == CoreLibrary.InterestRateMode.VARIABLE) {
            //variable
            //reset the user stable rate, and store the new borrow index
            user.stableBorrowRate = 0;
            user.lastVariableBorrowCumulativeIndex = reserve
                .lastVariableBorrowCumulativeIndex;
        } else {
            revert("Invalid borrow rate mode");
        }
        //increase the principal borrows and the origination fee
        user.principalBorrowBalance = user
            .principalBorrowBalance
            .add(_amountBorrowed)
            .add(_balanceIncrease);
        user.originationFee = user.originationFee.add(_fee);

        //solium-disable-next-line
        user.lastUpdateTimestamp = uint40(block.timestamp);
    }

    /**
     * @dev updates the state of the reserve as a consequence of a repay action.
     * @param _reserve the address of the reserve on which the user is repaying
     * @param _user the address of the borrower
     * @param _paybackAmountMinusFees the amount being paid back minus fees
     * @param _balanceIncrease the accrued interest on the borrowed amount
     **/

    function updateReserveStateOnRepayInternal(
        address _reserve,
        address _user,
        uint256 _paybackAmountMinusFees,
        uint256 _balanceIncrease
    ) internal {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];

        CoreLibrary.UserReserveData storage user =
            usersReserveData[_reserve][_user];

        CoreLibrary.InterestRateMode borrowRateMode =
            getUserCurrentBorrowRateMode(_reserve, _user);

        //update the indexes
        reserves[_reserve].updateCumulativeIndexes();

        //compound the cumulated interest to the borrow balance and then
        // subtracting the payback amount
        if (borrowRateMode == CoreLibrary.InterestRateMode.STABLE) {
            reserve.increaseTotalBorrowsStableAndUpdateAverageRate(
                _balanceIncrease,
                user.stableBorrowRate
            );
            reserve.decreaseTotalBorrowsStableAndUpdateAverageRate(
                _paybackAmountMinusFees,
                user.stableBorrowRate
            );
        } else {
            reserve.increaseTotalBorrowsVariable(_balanceIncrease);
            reserve.decreaseTotalBorrowsVariable(_paybackAmountMinusFees);
        }
    }

    /**
     * @dev updates the state of the user as a consequence of a repay action.
     * @param _reserve the address of the reserve on which the user is repaying
     * @param _user the address of the borrower
     * @param _paybackAmountMinusFees the amount being paid back minus fees
     * @param _originationFeeRepaid the fee on the amount that is being repaid
     * @param _balanceIncrease the accrued interest on the borrowed amount
     * @param _repaidWholeLoan true if the user is repaying the whole loan
     **/
    function updateUserStateOnRepayInternal(
        address _reserve,
        address _user,
        uint256 _paybackAmountMinusFees,
        uint256 _originationFeeRepaid,
        uint256 _balanceIncrease,
        bool _repaidWholeLoan
    ) internal {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];

        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];

        // update the user principal borrow balance, adding the cumulated
        // interest and then subtracting the payback amount
        user.principalBorrowBalance = user
            .principalBorrowBalance
            .add(_balanceIncrease)
            .sub(_paybackAmountMinusFees);
        user.lastVariableBorrowCumulativeIndex = reserve
            .lastVariableBorrowCumulativeIndex;

        // if the balance decrease is equal to the previous principal (user is
        // repaying the whole loan)
        // and the rate mode is stable, we reset the interest rate mode of the
        // user
        if (_repaidWholeLoan) {
            user.stableBorrowRate = 0;
            user.lastVariableBorrowCumulativeIndex = 0;
        }
        user.originationFee = user.originationFee.sub(_originationFeeRepaid);

        //solium-disable-next-line
        user.lastUpdateTimestamp = uint40(block.timestamp);
    }

    /**
     * @dev updates the state of the user as a consequence of a swap rate action.
     * @param _reserve the address of the reserve on which the user is
     * performing the rate swap
     * @param _user the address of the borrower
     * @param _principalBorrowBalance the the principal amount borrowed by the
     * user
     * @param _compoundedBorrowBalance the principal amount plus the accrued
     * interest
     * @param _currentRateMode the rate mode at which the user borrowed
     **/
    function updateReserveStateOnSwapRateInternal(
        address _reserve,
        address _user,
        uint256 _principalBorrowBalance,
        uint256 _compoundedBorrowBalance,
        CoreLibrary.InterestRateMode _currentRateMode
    ) internal {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];

        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];

        //compounding reserve indexes
        reserve.updateCumulativeIndexes();

        if (_currentRateMode == CoreLibrary.InterestRateMode.STABLE) {
            uint256 userCurrentStableRate = user.stableBorrowRate;

            //swap to variable
            reserve.decreaseTotalBorrowsStableAndUpdateAverageRate(
                _principalBorrowBalance,
                userCurrentStableRate
            ); //decreasing stable from old principal balance
            reserve.increaseTotalBorrowsVariable(_compoundedBorrowBalance); //increase variable borrows
        } else if (_currentRateMode == CoreLibrary.InterestRateMode.VARIABLE) {
            //swap to stable
            uint256 currentStableRate = reserve.currentStableBorrowRate;
            reserve.decreaseTotalBorrowsVariable(_principalBorrowBalance);
            reserve.increaseTotalBorrowsStableAndUpdateAverageRate(
                _compoundedBorrowBalance,
                currentStableRate
            );
        } else {
            revert("Invalid rate mode received");
        }
    }

    /**
     * @dev updates the state of the user as a consequence of a swap rate action.
     * @param _reserve the address of the reserve on which the user is
     * performing the swap
     * @param _user the address of the borrower
     * @param _balanceIncrease the accrued interest on the borrowed amount
     * @param _currentRateMode the current rate mode of the user
     **/

    function updateUserStateOnSwapRateInternal(
        address _reserve,
        address _user,
        uint256 _balanceIncrease,
        CoreLibrary.InterestRateMode _currentRateMode
    ) internal returns (CoreLibrary.InterestRateMode) {
        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];

        CoreLibrary.InterestRateMode newMode =
            CoreLibrary.InterestRateMode.NONE;

        if (_currentRateMode == CoreLibrary.InterestRateMode.VARIABLE) {
            //switch to stable
            newMode = CoreLibrary.InterestRateMode.STABLE;
            user.stableBorrowRate = reserve.currentStableBorrowRate;
            user.lastVariableBorrowCumulativeIndex = 0;
        } else if (_currentRateMode == CoreLibrary.InterestRateMode.STABLE) {
            newMode = CoreLibrary.InterestRateMode.VARIABLE;
            user.stableBorrowRate = 0;
            user.lastVariableBorrowCumulativeIndex = reserve
                .lastVariableBorrowCumulativeIndex;
        } else {
            revert("Invalid interest rate mode received");
        }
        //compounding cumulated interest
        user.principalBorrowBalance = user.principalBorrowBalance.add(
            _balanceIncrease
        );
        //solium-disable-next-line
        user.lastUpdateTimestamp = uint40(block.timestamp);

        return newMode;
    }

    /**
     * @dev updates the state of the principal reserve as a consequence of a
     * liquidation action.
     * @param _principalReserve the address of the principal reserve that is
     * being repaid
     * @param _user the address of the borrower
     * @param _amountToLiquidate the amount being repaid by the liquidator
     * @param _balanceIncrease the accrued interest on the borrowed amount
     **/

    function updatePrincipalReserveStateOnLiquidationInternal(
        address _principalReserve,
        address _user,
        uint256 _amountToLiquidate,
        uint256 _balanceIncrease
    ) internal {
        CoreLibrary.ReserveData storage reserve = reserves[_principalReserve];

        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_principalReserve];

        //update principal reserve data
        reserve.updateCumulativeIndexes();

        CoreLibrary.InterestRateMode borrowRateMode =
            getUserCurrentBorrowRateMode(_principalReserve, _user);

        if (borrowRateMode == CoreLibrary.InterestRateMode.STABLE) {
            //increase the total borrows by the compounded interest
            reserve.increaseTotalBorrowsStableAndUpdateAverageRate(
                _balanceIncrease,
                user.stableBorrowRate
            );

            //decrease by the actual amount to liquidate
            reserve.decreaseTotalBorrowsStableAndUpdateAverageRate(
                _amountToLiquidate,
                user.stableBorrowRate
            );
        } else {
            //increase the total borrows by the compounded interest
            reserve.increaseTotalBorrowsVariable(_balanceIncrease);

            //decrease by the actual amount to liquidate
            reserve.decreaseTotalBorrowsVariable(_amountToLiquidate);
        }
    }

    /**
     * @dev updates the state of the collateral reserve as a consequence of a
     * liquidation action.
     * @param _collateralReserve the address of the collateral reserve that is
     * being liquidated
     **/
    function updateCollateralReserveStateOnLiquidationInternal(
        address _collateralReserve
    ) internal {
        //update collateral reserve
        reserves[_collateralReserve].updateCumulativeIndexes();
    }

    /**
     * @dev updates the state of the user being liquidated as a consequence of a
     * liquidation action.
     * @param _reserve the address of the principal reserve that is being repaid
     * @param _user the address of the borrower
     * @param _amountToLiquidate the amount being repaid by the liquidator
     * @param _feeLiquidated the amount of origination fee being liquidated
     * @param _balanceIncrease the accrued interest on the borrowed amount
     **/
    function updateUserStateOnLiquidationInternal(
        address _reserve,
        address _user,
        uint256 _amountToLiquidate,
        uint256 _feeLiquidated,
        uint256 _balanceIncrease
    ) internal {
        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        // first increase by the compounded interest, then decrease by the
        // liquidated amount
        user.principalBorrowBalance = user
            .principalBorrowBalance
            .add(_balanceIncrease)
            .sub(_amountToLiquidate);

        if (
            getUserCurrentBorrowRateMode(_reserve, _user) ==
            CoreLibrary.InterestRateMode.VARIABLE
        ) {
            user.lastVariableBorrowCumulativeIndex = reserve
                .lastVariableBorrowCumulativeIndex;
        }

        if (_feeLiquidated > 0) {
            user.originationFee = user.originationFee.sub(_feeLiquidated);
        }

        //solium-disable-next-line
        user.lastUpdateTimestamp = uint40(block.timestamp);
    }

    /**
     * @dev updates the state of the reserve as a consequence of a stable rate
     * rebalance
     * @param _reserve the address of the principal reserve where the user
     * borrowed
     * @param _user the address of the borrower
     * @param _balanceIncrease the accrued interest on the borrowed amount
     **/

    function updateReserveStateOnRebalanceInternal(
        address _reserve,
        address _user,
        uint256 _balanceIncrease
    ) internal {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];

        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];

        reserve.updateCumulativeIndexes();

        reserve.increaseTotalBorrowsStableAndUpdateAverageRate(
            _balanceIncrease,
            user.stableBorrowRate
        );
    }

    /**
     * @dev updates the state of the user as a consequence of a stable rate
     * rebalance
     * @param _reserve the address of the principal reserve where the user
     * borrowed
     * @param _user the address of the borrower
     * @param _balanceIncrease the accrued interest on the borrowed amount
     **/

    function updateUserStateOnRebalanceInternal(
        address _reserve,
        address _user,
        uint256 _balanceIncrease
    ) internal {
        CoreLibrary.UserReserveData storage user =
            usersReserveData[_user][_reserve];
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];

        user.principalBorrowBalance = user.principalBorrowBalance.add(
            _balanceIncrease
        );
        user.stableBorrowRate = reserve.currentStableBorrowRate;

        //solium-disable-next-line
        user.lastUpdateTimestamp = uint40(block.timestamp);
    }

    /**
     * @dev updates the state of the user as a consequence of a stable rate
     * rebalance
     * @param _reserve the address of the principal reserve where the user
     * borrowed
     * @param _user the address of the borrower
     * @param _balanceIncrease the accrued interest on the borrowed amount
     * @param _amountBorrowed the accrued interest on the borrowed amount
     **/
    function updateReserveTotalBorrowsByRateModeInternal(
        address _reserve,
        address _user,
        uint256 _principalBalance,
        uint256 _balanceIncrease,
        uint256 _amountBorrowed,
        CoreLibrary.InterestRateMode _newBorrowRateMode
    ) internal {
        CoreLibrary.InterestRateMode previousRateMode =
            getUserCurrentBorrowRateMode(_reserve, _user);
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];

        if (previousRateMode == CoreLibrary.InterestRateMode.STABLE) {
            CoreLibrary.UserReserveData storage user =
                usersReserveData[_user][_reserve];
            reserve.decreaseTotalBorrowsStableAndUpdateAverageRate(
                _principalBalance,
                user.stableBorrowRate
            );
        } else if (previousRateMode == CoreLibrary.InterestRateMode.VARIABLE) {
            reserve.decreaseTotalBorrowsVariable(_principalBalance);
        }

        uint256 newPrincipalAmount =
            _principalBalance.add(_balanceIncrease).add(_amountBorrowed);
        if (_newBorrowRateMode == CoreLibrary.InterestRateMode.STABLE) {
            reserve.increaseTotalBorrowsStableAndUpdateAverageRate(
                newPrincipalAmount,
                reserve.currentStableBorrowRate
            );
        } else if (
            _newBorrowRateMode == CoreLibrary.InterestRateMode.VARIABLE
        ) {
            reserve.increaseTotalBorrowsVariable(newPrincipalAmount);
        } else {
            revert("Invalid new borrow rate mode");
        }
    }

    /**
     * @dev Updates the reserve current stable borrow rate Rf, the current
     * variable borrow rate Rv and the current liquidity rate Rl.
     * Also updates the lastUpdateTimestamp value. Please refer to the
     * whitepaper for further information.
     * @param _reserve the address of the reserve to be updated
     * @param _liquidityAdded the amount of liquidity added to the protocol
     * (deposit or repay) in the previous action
     * @param _liquidityTaken the amount of liquidity taken from the protocol
     * (redeem or borrow)
     **/

    function updateReserveInterestRatesAndTimestampInternal(
        address _reserve,
        uint256 _liquidityAdded,
        uint256 _liquidityTaken
    ) internal {
        CoreLibrary.ReserveData storage reserve = reserves[_reserve];
        (
            uint256 newLiquidityRate,
            uint256 newStableRate,
            uint256 newVariableRate
        ) =
            IReserveInterestRateStrategy(reserve.interestRateStrategyAddress)
                .calculateInterestRates(
                _reserve,
                getReserveAvailableLiquidity(_reserve).add(_liquidityAdded).sub(
                    _liquidityTaken
                ),
                reserve.totalBorrowsStable,
                reserve.totalBorrowsVariable,
                reserve.currentAverageStableBorrowRate
            );

        reserve.currentLiquidityRate = newLiquidityRate;
        reserve.currentStableBorrowRate = newStableRate;
        reserve.currentVariableBorrowRate = newVariableRate;

        //solium-disable-next-line
        reserve.lastUpdateTimestamp = uint40(block.timestamp);

        emit ReserveUpdated(
            _reserve,
            newLiquidityRate,
            newStableRate,
            newVariableRate,
            reserve.lastLiquidityCumulativeIndex,
            reserve.lastVariableBorrowCumulativeIndex
        );
    }

    /**
     * @dev updates the internal configuration of the core
     **/
    function refreshConfigInternal() internal {
        lendingPoolAddress = addressesProvider.getLendingPool();
        parametersProvider = LendingPoolParametersProvider(
            addressesProvider.getLendingPoolParametersProvider()
        );
        feeProvider = IFeeProvider(addressesProvider.getFeeProvider());
        rewardManager = RewardsManager(addressesProvider.getRewardManager());
    }

    /**
     * @dev adds a reserve to the array of the reserves address
     **/
    function addReserveToListInternal(address _reserve) internal {
        bool reserveAlreadyAdded = false;
        for (uint256 i = 0; i < reservesList.length; i++)
            if (reservesList[i] == _reserve) {
                reserveAlreadyAdded = true;
            }
        if (!reserveAlreadyAdded) reservesList.push(_reserve);
    }
}














/**
 * @title LendingPoolDataProvider contract
 * @author Multiplier Finance
 * @notice Implements functions to fetch data from the core, and aggregate them
 * in order to allow computation
 * on the compounded balances and the account balances in BNB
 **/
contract LendingPoolDataProvider is VersionedInitializable {
    using SafeMath for uint256;
    using WadRayMath for uint256;

    LendingPoolCore public core;
    LendingPoolAddressesProvider public addressesProvider;

    /**
     * @dev specifies the health factor threshold at which the user position is
     * liquidated.
     * 1e18 by default, if the health factor drops below 1e18, the loan can be
     * liquidated.
     **/
    uint256 public constant HEALTH_FACTOR_LIQUIDATION_THRESHOLD = 1e18;

    uint256 public constant DATA_PROVIDER_REVISION = 0x1;

    function getRevision() internal pure returns (uint256) {
        return DATA_PROVIDER_REVISION;
    }

    /**
     * @dev initializer
     * @param _addressesProvider the address of the Address Provider
     **/

    function initialize(LendingPoolAddressesProvider _addressesProvider)
        public
        initializer
    {
        addressesProvider = _addressesProvider;
        core = LendingPoolCore(_addressesProvider.getLendingPoolCore());
    }

    /**
     * @dev struct to hold calculateUserGlobalData() local computations
     **/
    struct UserGlobalDataLocalVars {
        uint256 reserveUnitPrice;
        uint256 tokenUnit;
        uint256 compoundedLiquidityBalance;
        uint256 compoundedBorrowBalance;
        uint256 reserveDecimals;
        uint256 baseLtv;
        uint256 liquidationThreshold;
        uint256 originationFee;
        bool usageAsCollateralEnabled;
        bool userUsesReserveAsCollateral;
        address currentReserve;
    }

    /**
     * @dev calculates the user data across the reserves.
     * this includes the total liquidity/collateral/borrow balances in BNB,
     * the average Loan To Value, the average Liquidation Ratio, and the Health factor.
     * @param _user the address of the user
     * @return the total liquidity, total collateral, total borrow balances of
     * the user in BNB.
     * also the average Ltv, liquidation threshold, and the health factor
     **/
    function calculateUserGlobalData(address _user)
        public
        view
        returns (
            uint256 totalLiquidityBalanceBNB,
            uint256 totalCollateralBalanceBNB,
            uint256 totalBorrowBalanceBNB,
            uint256 totalFeesBNB,
            uint256 currentLtv,
            uint256 currentLiquidationThreshold,
            uint256 healthFactor,
            bool healthFactorBelowThreshold
        )
    {
        IPriceOracleGetter oracle =
            IPriceOracleGetter(addressesProvider.getPriceOracle());

        // Usage of a memory struct of vars to avoid "Stack too deep" errors due to local variables
        UserGlobalDataLocalVars memory vars;

        address[] memory reserves = core.getReserves();

        for (uint256 i = 0; i < reserves.length; i++) {
            vars.currentReserve = reserves[i];

            (
                vars.compoundedLiquidityBalance,
                vars.compoundedBorrowBalance,
                vars.originationFee,
                vars.userUsesReserveAsCollateral
            ) = core.getUserBasicReserveData(vars.currentReserve, _user);

            if (
                vars.compoundedLiquidityBalance == 0 &&
                vars.compoundedBorrowBalance == 0
            ) {
                continue;
            }

            //fetch reserve data
            (
                vars.reserveDecimals,
                vars.baseLtv,
                vars.liquidationThreshold,
                vars.usageAsCollateralEnabled
            ) = core.getReserveConfiguration(vars.currentReserve);

            vars.tokenUnit = 10**vars.reserveDecimals;
            vars.reserveUnitPrice = oracle.getAssetPrice(vars.currentReserve);

            //liquidity and collateral balance
            if (vars.compoundedLiquidityBalance > 0) {
                uint256 liquidityBalanceBNB =
                    vars
                        .reserveUnitPrice
                        .mul(vars.compoundedLiquidityBalance)
                        .div(vars.tokenUnit);
                totalLiquidityBalanceBNB = totalLiquidityBalanceBNB.add(
                    liquidityBalanceBNB
                );

                if (
                    vars.usageAsCollateralEnabled &&
                    vars.userUsesReserveAsCollateral
                ) {
                    totalCollateralBalanceBNB = totalCollateralBalanceBNB.add(
                        liquidityBalanceBNB
                    );
                    currentLtv = currentLtv.add(
                        liquidityBalanceBNB.mul(vars.baseLtv)
                    );
                    currentLiquidationThreshold = currentLiquidationThreshold
                        .add(
                        liquidityBalanceBNB.mul(vars.liquidationThreshold)
                    );
                }
            }

            if (vars.compoundedBorrowBalance > 0) {
                totalBorrowBalanceBNB = totalBorrowBalanceBNB.add(
                    vars.reserveUnitPrice.mul(vars.compoundedBorrowBalance).div(
                        vars.tokenUnit
                    )
                );
                totalFeesBNB = totalFeesBNB.add(
                    vars.originationFee.mul(vars.reserveUnitPrice).div(
                        vars.tokenUnit
                    )
                );
            }
        }

        currentLtv = totalCollateralBalanceBNB > 0
            ? currentLtv.div(totalCollateralBalanceBNB)
            : 0;
        currentLiquidationThreshold = totalCollateralBalanceBNB > 0
            ? currentLiquidationThreshold.div(totalCollateralBalanceBNB)
            : 0;

        healthFactor = calculateHealthFactorFromBalancesInternal(
            totalCollateralBalanceBNB,
            totalBorrowBalanceBNB,
            totalFeesBNB,
            currentLiquidationThreshold
        );
        healthFactorBelowThreshold =
            healthFactor < HEALTH_FACTOR_LIQUIDATION_THRESHOLD;
    }

    struct balanceDecreaseAllowedLocalVars {
        uint256 decimals;
        uint256 collateralBalanceBNB;
        uint256 borrowBalanceBNB;
        uint256 totalFeesBNB;
        uint256 currentLiquidationThreshold;
        uint256 reserveLiquidationThreshold;
        uint256 amountToDecreaseBNB;
        uint256 collateralBalancefterDecrease;
        uint256 liquidationThresholdAfterDecrease;
        uint256 healthFactorAfterDecrease;
        bool reserveUsageAsCollateralEnabled;
    }

    /**
     * @dev check if a specific balance decrease is allowed (i.e. doesn't bring * the user borrow position health factor under 1e18)
     * @param _reserve the address of the reserve
     * @param _user the address of the user
     * @param _amount the amount to decrease
     * @return true if the decrease of the balance is allowed
     **/

    function balanceDecreaseAllowed(
        address _reserve,
        address _user,
        uint256 _amount
    ) external view returns (bool) {
        // Usage of a memory struct of vars to avoid "Stack too deep" errors due to local variables
        balanceDecreaseAllowedLocalVars memory vars;

        (
            vars.decimals,
            ,
            vars.reserveLiquidationThreshold,
            vars.reserveUsageAsCollateralEnabled
        ) = core.getReserveConfiguration(_reserve);

        if (
            !vars.reserveUsageAsCollateralEnabled ||
            !core.isUserUseReserveAsCollateralEnabled(_reserve, _user)
        ) {
            return true; //if reserve is not used as collateral, no reasons to block the transfer
        }

        (
            ,
            vars.collateralBalanceBNB,
            vars.borrowBalanceBNB,
            vars.totalFeesBNB,
            ,
            vars.currentLiquidationThreshold,
            ,

        ) = calculateUserGlobalData(_user);

        if (vars.borrowBalanceBNB == 0) {
            return true; //no borrows - no reasons to block the transfer
        }

        IPriceOracleGetter oracle =
            IPriceOracleGetter(addressesProvider.getPriceOracle());

        vars.amountToDecreaseBNB = oracle
            .getAssetPrice(_reserve)
            .mul(_amount)
            .div(10**vars.decimals);

        vars.collateralBalancefterDecrease = vars.collateralBalanceBNB.sub(
            vars.amountToDecreaseBNB
        );

        //if there is a borrow, there can't be 0 collateral
        if (vars.collateralBalancefterDecrease == 0) {
            return false;
        }

        vars.liquidationThresholdAfterDecrease = vars
            .collateralBalanceBNB
            .mul(vars.currentLiquidationThreshold)
            .sub(vars.amountToDecreaseBNB.mul(vars.reserveLiquidationThreshold))
            .div(vars.collateralBalancefterDecrease);

        uint256 healthFactorAfterDecrease =
            calculateHealthFactorFromBalancesInternal(
                vars.collateralBalancefterDecrease,
                vars.borrowBalanceBNB,
                vars.totalFeesBNB,
                vars.liquidationThresholdAfterDecrease
            );

        return healthFactorAfterDecrease > HEALTH_FACTOR_LIQUIDATION_THRESHOLD;
    }

    /**
     * @notice calculates the amount of collateral needed in BNB to cover a new
     * borrow.
     * @param _reserve the reserve from which the user wants to borrow
     * @param _amount the amount the user wants to borrow
     * @param _fee the fee for the amount that the user needs to cover
     * @param _userCurrentBorrowBalanceTH the current borrow balance of the user
     * (before the borrow)
     * @param _userCurrentLtv the average ltv of the user given his current
     * collateral
     * @return the total amount of collateral in BNB to cover the current borrow
     * balance + the new amount + fee
     **/
    function calculateCollateralNeededInBNB(
        address _reserve,
        uint256 _amount,
        uint256 _fee,
        uint256 _userCurrentBorrowBalanceTH,
        uint256 _userCurrentFeesBNB,
        uint256 _userCurrentLtv
    ) external view returns (uint256) {
        uint256 reserveDecimals = core.getReserveDecimals(_reserve);

        IPriceOracleGetter oracle =
            IPriceOracleGetter(addressesProvider.getPriceOracle());

        uint256 requestedBorrowAmountBNB =
            oracle.getAssetPrice(_reserve).mul(_amount.add(_fee)).div(
                10**reserveDecimals
            ); //price is in bnb

        //add the current already borrowed amount to the amount requested to calculate the total collateral needed.
        uint256 collateralNeededInBNB =
            _userCurrentBorrowBalanceTH
                .add(_userCurrentFeesBNB)
                .add(requestedBorrowAmountBNB)
                .mul(100)
                .div(_userCurrentLtv); //LTV is calculated in percentage

        return collateralNeededInBNB;
    }

    /**
     * @dev calculates the equivalent amount in BNB that an user can borrow,
     * depending on the available collateral and the
     * average Loan To Value.
     * @param collateralBalanceBNB the total collateral balance
     * @param borrowBalanceBNB the total borrow balance
     * @param totalFeesBNB the total fees
     * @param ltv the average loan to value
     * @return the amount available to borrow in BNB for the user
     **/

    function calculateavailableBorrowsBNBInternal(
        uint256 collateralBalanceBNB,
        uint256 borrowBalanceBNB,
        uint256 totalFeesBNB,
        uint256 ltv
    ) internal view returns (uint256) {
        uint256 availableBorrowsBNB = collateralBalanceBNB.mul(ltv).div(100); //ltv is in percentage

        if (availableBorrowsBNB < borrowBalanceBNB) {
            return 0;
        }

        availableBorrowsBNB = availableBorrowsBNB.sub(
            borrowBalanceBNB.add(totalFeesBNB)
        );
        //calculate fee
        uint256 borrowFee =
            IFeeProvider(addressesProvider.getFeeProvider())
                .calculateLoanOriginationFee(availableBorrowsBNB);
        return availableBorrowsBNB.sub(borrowFee);
    }

    /**
     * @dev calculates the health factor from the corresponding balances
     * @param collateralBalanceBNB the total collateral balance in BNB
     * @param borrowBalanceBNB the total borrow balance in BNB
     * @param totalFeesBNB the total fees in BNB
     * @param liquidationThreshold the avg liquidation threshold
     **/
    function calculateHealthFactorFromBalancesInternal(
        uint256 collateralBalanceBNB,
        uint256 borrowBalanceBNB,
        uint256 totalFeesBNB,
        uint256 liquidationThreshold
    ) internal pure returns (uint256) {
        if (borrowBalanceBNB == 0) return uint256(-1);

        return
            (collateralBalanceBNB.mul(liquidationThreshold).div(100)).wadDiv(
                borrowBalanceBNB.add(totalFeesBNB)
            );
    }

    /**
     * @dev returns the health factor liquidation threshold
     **/
    function getHealthFactorLiquidationThreshold()
        public
        pure
        returns (uint256)
    {
        return HEALTH_FACTOR_LIQUIDATION_THRESHOLD;
    }

    /**
     * @dev accessory functions to fetch data from the lendingPoolCore
     **/
    function getReserveConfigurationData(address _reserve)
        external
        view
        returns (
            uint256 ltv,
            uint256 liquidationThreshold,
            uint256 liquidationBonus,
            address rateStrategyAddress,
            bool usageAsCollateralEnabled,
            bool borrowingEnabled,
            bool stableBorrowRateEnabled,
            bool isActive
        )
    {
        (, ltv, liquidationThreshold, usageAsCollateralEnabled) = core
            .getReserveConfiguration(_reserve);
        stableBorrowRateEnabled = core.getReserveIsStableBorrowRateEnabled(
            _reserve
        );
        borrowingEnabled = core.isReserveBorrowingEnabled(_reserve);
        isActive = core.getReserveIsActive(_reserve);
        liquidationBonus = core.getReserveLiquidationBonus(_reserve);

        rateStrategyAddress = core.getReserveInterestRateStrategyAddress(
            _reserve
        );
    }

    function getReserveData(address _reserve)
        external
        view
        returns (
            uint256 totalLiquidity,
            uint256 availableLiquidity,
            uint256 totalBorrowsStable,
            uint256 totalBorrowsVariable,
            uint256 liquidityRate,
            uint256 variableBorrowRate,
            uint256 stableBorrowRate,
            uint256 averageStableBorrowRate,
            uint256 utilizationRate,
            uint256 liquidityIndex,
            uint256 variableBorrowIndex,
            address mTokenAddress,
            uint40 lastUpdateTimestamp
        )
    {
        totalLiquidity = core.getReserveTotalLiquidity(_reserve);
        availableLiquidity = core.getReserveAvailableLiquidity(_reserve);
        totalBorrowsStable = core.getReserveTotalBorrowsStable(_reserve);
        totalBorrowsVariable = core.getReserveTotalBorrowsVariable(_reserve);
        liquidityRate = core.getReserveCurrentLiquidityRate(_reserve);
        variableBorrowRate = core.getReserveCurrentVariableBorrowRate(_reserve);
        stableBorrowRate = core.getReserveCurrentStableBorrowRate(_reserve);
        averageStableBorrowRate = core.getReserveCurrentAverageStableBorrowRate(
            _reserve
        );
        utilizationRate = core.getReserveUtilizationRate(_reserve);
        liquidityIndex = core.getReserveLiquidityCumulativeIndex(_reserve);
        variableBorrowIndex = core.getReserveVariableBorrowsCumulativeIndex(
            _reserve
        );
        mTokenAddress = core.getReservemTokenAddress(_reserve);
        lastUpdateTimestamp = core.getReserveLastUpdate(_reserve);
    }

    function getUserAccountData(address _user)
        external
        view
        returns (
            uint256 totalLiquidityBNB,
            uint256 totalCollateralBNB,
            uint256 totalBorrowsBNB,
            uint256 totalFeesBNB,
            uint256 availableBorrowsBNB,
            uint256 currentLiquidationThreshold,
            uint256 ltv,
            uint256 healthFactor
        )
    {
        (
            totalLiquidityBNB,
            totalCollateralBNB,
            totalBorrowsBNB,
            totalFeesBNB,
            ltv,
            currentLiquidationThreshold,
            healthFactor,

        ) = calculateUserGlobalData(_user);

        availableBorrowsBNB = calculateavailableBorrowsBNBInternal(
            totalCollateralBNB,
            totalBorrowsBNB,
            totalFeesBNB,
            ltv
        );
    }

    function getUserReserveData(address _reserve, address _user)
        external
        view
        returns (
            uint256 currentmTokenBalance,
            uint256 currentBorrowBalance,
            uint256 principalBorrowBalance,
            uint256 borrowRateMode,
            uint256 borrowRate,
            uint256 liquidityRate,
            uint256 originationFee,
            uint256 variableBorrowIndex,
            uint256 lastUpdateTimestamp,
            bool usageAsCollateralEnabled
        )
    {
        currentmTokenBalance = mToken(core.getReservemTokenAddress(_reserve))
            .balanceOf(_user);
        CoreLibrary.InterestRateMode mode =
            core.getUserCurrentBorrowRateMode(_reserve, _user);
        (principalBorrowBalance, currentBorrowBalance, ) = core
            .getUserBorrowBalances(_reserve, _user);

        //default is 0, if mode == CoreLibrary.InterestRateMode.NONE
        if (mode == CoreLibrary.InterestRateMode.STABLE) {
            borrowRate = core.getUserCurrentStableBorrowRate(_reserve, _user);
        } else if (mode == CoreLibrary.InterestRateMode.VARIABLE) {
            borrowRate = core.getReserveCurrentVariableBorrowRate(_reserve);
        }

        borrowRateMode = uint256(mode);
        liquidityRate = core.getReserveCurrentLiquidityRate(_reserve);
        originationFee = core.getUserOriginationFee(_reserve, _user);
        variableBorrowIndex = core.getUserVariableBorrowCumulativeIndex(
            _reserve,
            _user
        );
        lastUpdateTimestamp = core.getUserLastUpdate(_reserve, _user);
        usageAsCollateralEnabled = core.isUserUseReserveAsCollateralEnabled(
            _reserve,
            _user
        );
    }
}


/**
@title ILendingPoolAddressesProvider interface
@notice provides the interface to fetch the LendingPoolCore address
 */

contract ILendingPoolAddressesProvider {
    function getLendingPool() public view returns (address);

    function setLendingPoolImpl(address _pool) public;

    function getLendingPoolCore() public view returns (address payable);

    function setLendingPoolCoreImpl(address _lendingPoolCore) public;

    function getLendingPoolConfigurator() public view returns (address);

    function setLendingPoolConfiguratorImpl(address _configurator) public;

    function getLendingPoolDataProvider() public view returns (address);

    function setLendingPoolDataProviderImpl(address _provider) public;

    function getLendingPoolParametersProvider() public view returns (address);

    function setLendingPoolParametersProvider(address _parametersProvider) public;

    function getFeeProvider() public view returns (address);

    function setFeeProviderImpl(address _feeProvider) public;

    function getLendingPoolLiquidationManager() public view returns (address);

    function setLendingPoolLiquidationManager(address _manager) public;

    function getLendingPoolManager() public view returns (address);

    function setLendingPoolManager(address _lendingPoolManager) public;

    function getPriceOracle() public view returns (address);

    function setPriceOracle(address _priceOracle) public;

    function getLendingRateOracle() public view returns (address);

    function setLendingRateOracle(address _lendingRateOracle) public;

    function getRewardManager() public view returns (address);

    function setRewardManager(address _manager) public;

    function getLpRewardVault() public view returns (address);

    function setLpRewardVault(address _address) public;

    function getGovRewardVault() public view returns (address);

    function setGovRewardVault(address _address) public;

    function getSafetyRewardVault() public view returns (address);

    function setSafetyRewardVault(address _address) public;
    
    function getStakingToken() public view returns (address);

    function setStakingToken(address _address) public;
        
        
}


contract AddressStorage {
    mapping(bytes32 => address) private addresses;

    /**
     * @dev function to return the addresses associated with a given key value
     * @param _key the key value to be examined
     **/
    function getAddress(bytes32 _key) public view returns (address) {
        return addresses[_key];
    }

    /**
     * @dev function to set a key value with a provided key and value argument
     * @param _key The key to be set
     * @param _value The value to be set
     **/
    function _setAddress(bytes32 _key, address _value) internal {
        addresses[_key] = _value;
    }
}


contract UintStorage {
    mapping(bytes32 => uint256) private uints;

    /**
     * @dev function to return the uint associated with a given key value
     * @param _key the key value to be examined
     **/
    function getUint(bytes32 _key) public view returns (uint256) {
        return uints[_key];
    }

    /**
     * @dev function to set a key value with a provided key and value argument
     * @param _key The key to be set
     * @param _value The value to be set
     **/
    function _setUint(bytes32 _key, uint256 _value) internal {
        uints[_key] = _value;
    }
}




/******************
@title WadRayMath library
@author Multiplier Finance
@dev Provides mul and div function for wads (decimal numbers with 18 digits precision) and rays (decimals with 27 digits)
 */

library WadRayMath {
    using SafeMath for uint256;

    uint256 internal constant WAD = 1e18;
    uint256 internal constant halfWAD = WAD / 2;

    uint256 internal constant RAY = 1e27;
    uint256 internal constant halfRAY = RAY / 2;

    uint256 internal constant WAD_RAY_RATIO = 1e9;

    function ray() internal pure returns (uint256) {
        return RAY;
    }

    function wad() internal pure returns (uint256) {
        return WAD;
    }

    function halfRay() internal pure returns (uint256) {
        return halfRAY;
    }

    function halfWad() internal pure returns (uint256) {
        return halfWAD;
    }

    function wadMul(uint256 a, uint256 b) internal pure returns (uint256) {
        return halfWAD.add(a.mul(b)).div(WAD);
    }

    function wadDiv(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 halfB = b / 2;

        return halfB.add(a.mul(WAD)).div(b);
    }

    function rayMul(uint256 a, uint256 b) internal pure returns (uint256) {
        return halfRAY.add(a.mul(b)).div(RAY);
    }

    function rayDiv(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 halfB = b / 2;

        return halfB.add(a.mul(RAY)).div(b);
    }

    function rayToWad(uint256 a) internal pure returns (uint256) {
        uint256 halfRatio = WAD_RAY_RATIO / 2;

        return halfRatio.add(a).div(WAD_RAY_RATIO);
    }

    function wadToRay(uint256 a) internal pure returns (uint256) {
        return a.mul(WAD_RAY_RATIO);
    }

    /**
     * @dev calculates base^exp. The code uses the ModExp precompile
     * @return base^exp, in ray
     */
    //solium-disable-next-line
    function rayPow(uint256 x, uint256 n) internal pure returns (uint256 z) {
        z = n % 2 != 0 ? x : RAY;

        for (n /= 2; n != 0; n /= 2) {
            x = rayMul(x, x);

            if (n % 2 != 0) {
                z = rayMul(z, x);
            }
        }
    }
}




/**
* @title LendingPoolParametersProvider
* @author Multiplier Finance
* @notice stores the configuration parameters of the Lending Pool contract
**/


contract LendingPoolParametersProvider {
    
    uint256 private constant MAX_STABLE_RATE_BORROW_SIZE_PERCENT = 25;
    uint256 private constant REBALANCE_DOWN_RATE_DELTA = (1e27)/5;
    
    /**
    * @dev returns the maximum stable rate borrow size, in percentage of the
    * available liquidity.
    **/
    function getMaxStableRateBorrowSizePercent() external pure returns (uint256)  {
        return (MAX_STABLE_RATE_BORROW_SIZE_PERCENT);
    }

    /**
    * @dev returns the delta between the current stable rate and the user
    * stable rate at which the borrow position of the user will be rebalanced
    * (scaled down)
    **/
    function getRebalanceDownRateDelta() external pure returns (uint256) {
        return (REBALANCE_DOWN_RATE_DELTA);
    }
  
}





















/**
 * @title Proxy
 * @dev Implements delegation of calls to other contracts, with proper
 * forwarding of return values and bubbling of failures.
 * It defines a fallback function that delegates all calls to the address
 * returned by the abstract _implementation() internal function.
 */
contract Proxy {
    /**
     * @dev Fallback function.
     * Implemented entirely in `_fallback`.
     */
    function() external payable {
        _fallback();
    }

    /**
     * @return The Address of the implementation.
     */
    function _implementation() internal view returns (address);

    /**
     * @dev Delegates execution to an implementation contract.
     * This is a low level function that doesn't return to its internal call
     * site.
     * It will return to the external caller whatever the implementation
     * returns.
     * @param implementation Address to delegate.
     */
    function _delegate(address implementation) internal {
        //solium-disable-next-line
        assembly {
            // Copy msg.data. We take full control of memory in this inline
            // assembly
            // block because it will not return to Solidity code. We overwrite
            // the
            // Solidity scratch pad at memory position 0.
            calldatacopy(0, 0, calldatasize)

            // Call the implementation.
            // out and outsize are 0 because we don't know the size yet.
            let result := delegatecall(
                gas,
                implementation,
                0,
                calldatasize,
                0,
                0
            )

            // Copy the returned data.
            returndatacopy(0, 0, returndatasize)

            switch result
                // delegatecall returns 0 on error.
                case 0 {
                    revert(0, returndatasize)
                }
                default {
                    return(0, returndatasize)
                }
        }
    }

    /**
     * @dev Function that is run as the first thing in the fallback function.
     * Can be redefined in derived contracts to add functionality.
     * Redefinitions must call super._willFallback().
     */
    function _willFallback() internal {}

    /**
     * @dev fallback implementation.
     * Extracted to enable manual triggering.
     */
    function _fallback() internal {
        _willFallback();
        _delegate(_implementation());
    }
}



/**
 * @title BaseUpgradeabilityProxy
 * @dev This contract implements a proxy that allows to change the
 * implementation address to which it will delegate.
 * Such a change is called an implementation upgrade.
 */
contract BaseUpgradeabilityProxy is Proxy {
    /**
     * @dev Emitted when the implementation is upgraded.
     * @param implementation Address of the new implementation.
     */
    event Upgraded(address indexed implementation);

    /**
     * @dev Storage slot with the address of the current implementation.
     * This is the keccak-256 hash of "eip1967.proxy.implementation" subtracted
     * by 1, and is
     * validated in the constructor.
     */
    bytes32 internal constant IMPLEMENTATION_SLOT =
        0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc;

    /**
     * @dev Returns the current implementation.
     * @return Address of the current implementation
     */
    function _implementation() internal view returns (address impl) {
        bytes32 slot = IMPLEMENTATION_SLOT;
        //solium-disable-next-line
        assembly {
            impl := sload(slot)
        }
    }

    /**
     * @dev Upgrades the proxy to a new implementation.
     * @param newImplementation Address of the new implementation.
     */
    function _upgradeTo(address newImplementation) internal {
        _setImplementation(newImplementation);
        emit Upgraded(newImplementation);
    }

    /**
     * @dev Sets the implementation address of the proxy.
     * @param newImplementation Address of the new implementation.
     */
    function _setImplementation(address newImplementation) internal {
        require(
            Address.isContract(newImplementation),
            "Cannot set a proxy implementation to a non-contract address"
        );

        bytes32 slot = IMPLEMENTATION_SLOT;

        //solium-disable-next-line
        assembly {
            sstore(slot, newImplementation)
        }
    }
}


/**
 * @title UpgradeabilityProxy
 * @dev Extends BaseUpgradeabilityProxy with a constructor for initializing
 * implementation and init data.
 */
contract UpgradeabilityProxy is BaseUpgradeabilityProxy {
    /**
     * @dev Contract constructor.
     * @param _logic Address of the initial implementation.
     * @param _data Data to send as msg.data to the implementation to initialize
     * the proxied contract.
     * It should include the signature and the parameters of the function to be
     * called, as described in
     * https://solidity.readthedocs.io/en/v0.4.24/abi-spec.
     * html#function-selector-and-argument-encoding.
     * This parameter is optional, if no data is given the initialization call
     * to
     * proxied contract will be skipped.
     */
    constructor(address _logic, bytes memory _data) public payable {
        assert(
            IMPLEMENTATION_SLOT ==
                bytes32(uint256(keccak256("eip1967.proxy.implementation")) - 1)
        );
        _setImplementation(_logic);
        if (_data.length > 0) {
            (bool success, ) = _logic.delegatecall(_data);
            require(success);
        }
    }
}


/**
 * @title BaseAdminUpgradeabilityProxy
 * @dev This contract combines an upgradeability proxy with an authorization
 * mechanism for administrative tasks.
 * All external functions in this contract must be guarded by the
 * `ifAdmin` modifier. See ethereum/solidity#3864 for a Solidity
 * feature proposal that would enable this to be done automatically.
 */
contract BaseAdminUpgradeabilityProxy is BaseUpgradeabilityProxy {
    /**
     * @dev Emitted when the administration has been transferred.
     * @param previousAdmin Address of the previous admin.
     * @param newAdmin Address of the new admin.
     */
    event AdminChanged(address previousAdmin, address newAdmin);

    /**
     * @dev Storage slot with the admin of the contract.
     * This is the keccak-256 hash of "eip1967.proxy.admin" subtracted by 1,
     * and is
     * validated in the constructor.
     */

    bytes32
        internal constant ADMIN_SLOT = 0xb53127684a568b3173ae13b9f8a6016e243e63b6e8ee1178d6a717850b5d6103;

    /**
     * @dev Modifier to check whether the `msg.sender` is the admin.
     * If it is, it will run the function. Otherwise, it will delegate the call
     * to the implementation.
     */
    modifier ifAdmin() {
        if (msg.sender == _admin()) {
            _;
        } else {
            _fallback();
        }
    }

    /**
     * @return The address of the proxy admin.
     */
    function admin() external ifAdmin returns (address) {
        return _admin();
    }

    /**
     * @return The address of the implementation.
     */
    function implementation() external ifAdmin returns (address) {
        return _implementation();
    }

    /**
     * @dev Changes the admin of the proxy.
     * Only the current admin can call this function.
     * @param newAdmin Address to transfer proxy administration to.
     */
    function changeAdmin(address newAdmin) external ifAdmin {
        require(
            newAdmin != address(0),
            "Cannot change the admin of a proxy to the zero address"
        );
        emit AdminChanged(_admin(), newAdmin);
        _setAdmin(newAdmin);
    }

    /**
     * @dev Upgrade the backing implementation of the proxy.
     * Only the admin can call this function.
     * @param newImplementation Address of the new implementation.
     */
    function upgradeTo(address newImplementation) external ifAdmin {
        _upgradeTo(newImplementation);
    }

    /**
     * @dev Upgrade the backing implementation of the proxy and call a function
     * on the new implementation.
     * This is useful to initialize the proxied contract.
     * @param newImplementation Address of the new implementation.
     * @param data Data to send as msg.data in the low level call.
     * It should include the signature and the parameters of the function to be called, as described in
     * https://solidity.readthedocs.io/en/v0.4.24/abi-spec.html#function-selector-and-argument-encoding.
     */
    function upgradeToAndCall(address newImplementation, bytes calldata data)
        external
        payable
        ifAdmin
    {
        _upgradeTo(newImplementation);
        (bool success, ) = newImplementation.delegatecall(data);
        require(success);
    }

    /**
     * @return The admin slot.
     */
    function _admin() internal view returns (address adm) {
        bytes32 slot = ADMIN_SLOT;
        //solium-disable-next-line
        assembly {
            adm := sload(slot)
        }
    }

    /**
     * @dev Sets the address of the proxy admin.
     * @param newAdmin Address of the new proxy admin.
     */
    function _setAdmin(address newAdmin) internal {
        bytes32 slot = ADMIN_SLOT;
        //solium-disable-next-line
        assembly {
            sstore(slot, newAdmin)
        }
    }

    /**
     * @dev Only fall back when the sender is not the admin.
     */
    function _willFallback() internal {
        require(
            msg.sender != _admin(),
            "Cannot call fallback function from the proxy admin"
        );
        super._willFallback();
    }
}





/**
 * @title InitializableUpgradeabilityProxy
 * @dev Extends BaseUpgradeabilityProxy with an initializer for initializing
 * implementation and init data.
 */
contract InitializableUpgradeabilityProxy is BaseUpgradeabilityProxy {
    /**
     * @dev Contract initializer.
     * @param _logic Address of the initial implementation.
     * @param _data Data to send as msg.data to the implementation to initialize
     * the proxied contract.
     * It should include the signature and the parameters of the function to be
     * called, as described in
     * https://solidity.readthedocs.io/en/v0.4.24/abi-spec.
     * html#function-selector-and-argument-encoding.
     * This parameter is optional, if no data is given the initialization call
     * to proxied contract will be skipped.
     */
    function initialize(address _logic, bytes memory _data) public payable {
        require(_implementation() == address(0));
        assert(
            IMPLEMENTATION_SLOT ==
                bytes32(uint256(keccak256("eip1967.proxy.implementation")) - 1)
        );
        _setImplementation(_logic);
        if (_data.length > 0) {
            (bool success, ) = _logic.delegatecall(_data);
            require(success);
        }
    }
}


/**
 * @title InitializableAdminUpgradeabilityProxy
 * @dev Extends from BaseAdminUpgradeabilityProxy with an initializer for
 * initializing the implementation, admin, and init data.
 */
contract InitializableAdminUpgradeabilityProxy is
    BaseAdminUpgradeabilityProxy,
    InitializableUpgradeabilityProxy
{
    /**
     * Contract initializer.
     * @param _logic address of the initial implementation.
     * @param _admin Address of the proxy administrator.
     * @param _data Data to send as msg.data to the implementation to initialize
     * the proxied contract.
     * It should include the signature and the parameters of the function to be
     * called, as described in
     * https://solidity.readthedocs.io/en/v0.4.24/abi-spec.
     * html#function-selector-and-argument-encoding.
     * This parameter is optional, if no data is given the initialization call
     * to
     * proxied contract will be skipped.
     */
    function initialize(
        address _logic,
        address _admin,
        bytes memory _data
    ) public payable {
        require(_implementation() == address(0));
        InitializableUpgradeabilityProxy.initialize(_logic, _data);
        assert(
            ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1)
        );
        _setAdmin(_admin);
    }
}




/**
 * @title LendingPoolAddressesProvider contract
 * @notice Is the main registry of the protocol. All the different components of * the protocol are accessible
 * through the addresses provider.
 * @author Multiplier Finance
 **/

contract LendingPoolAddressesProvider is
    Ownable,
    ILendingPoolAddressesProvider,
    AddressStorage
{
    // Events to be emitted
    event LendingPoolUpdated(address indexed newAddress);
    event LendingPoolCoreUpdated(address indexed newAddress);
    event LendingPoolParametersProviderUpdated(address indexed newAddress);
    event LendingPoolManagerUpdated(address indexed newAddress);
    event LendingPoolConfiguratorUpdated(address indexed newAddress);
    event LendingPoolLiquidationManagerUpdated(address indexed newAddress);
    event LendingPoolDataProviderUpdated(address indexed newAddress);
    event PriceOracleUpdated(address indexed newAddress);
    event LendingRateOracleUpdated(address indexed newAddress);
    event FeeProviderUpdated(address indexed newAddress);
    event RewardManagerUpdated(address indexed newAddress);
    event LiquidityRewardVaultUpdated(address indexed newAddress);
    event GovRewardVaultUpdated(address indexed newAddress);
    event SafetyRewardVaultUpdated(address indexed newAddress);
    event StakingTokenUpdated(address indexed newAddress);

    event ProxyCreated(bytes32 id, address indexed newAddress);

    bytes32 private constant LENDING_POOL = "LENDING_POOL";
    bytes32 private constant LENDING_POOL_CORE = "LENDING_POOL_CORE";
    bytes32 private constant LENDING_POOL_CONFIGURATOR =
        "LENDING_POOL_CONFIGURATOR";
    bytes32 private constant LENDING_POOL_PARAMETERS_PROVIDER =
        "PARAMETERS_PROVIDER";
    bytes32 private constant LENDING_POOL_MANAGER = "LENDING_POOL_MANAGER";
    bytes32 private constant LENDING_POOL_LIQUIDATION_MANAGER =
        "LIQUIDATION_MANAGER";
    bytes32 private constant DATA_PROVIDER = "DATA_PROVIDER";
    bytes32 private constant PRICE_ORACLE = "PRICE_ORACLE";
    bytes32 private constant LENDING_RATE_ORACLE = "LENDING_RATE_ORACLE";
    bytes32 private constant FEE_PROVIDER = "FEE_PROVIDER";
    bytes32 private constant REWARD_MANAGER = "REWARD_MANAGER";
    bytes32 private constant LP_REWARD_VAULT = "LP_REWARD_VAULT";
    bytes32 private constant GOV_REWARD_VAULT = "GOV_REWARD_VAULT";
    bytes32 private constant SAFETY_REWARD_VAULT = "SAFETY_REWARD_VAULT";
    bytes32 private constant STAKING_TOKEN = "STAKING_TOKEN";

    /**
     * @dev returns the address of the LendingPool proxy
     * @return the lending pool proxy address
     **/
    function getLendingPool() public view returns (address) {
        return getAddress(LENDING_POOL);
    }

    /**
     * @dev updates the implementation of the lending pool
     * @param _pool the new lending pool implementation
     **/
    function setLendingPoolImpl(address _pool) public onlyOwner {
        updateImplInternal(LENDING_POOL, _pool);
        emit LendingPoolUpdated(_pool);
    }

    /**
     * @dev returns the address of the LendingPoolCore proxy
     * @return the lending pool core proxy address
     */
    function getLendingPoolCore() public view returns (address payable) {
        address payable core = address(uint160(getAddress(LENDING_POOL_CORE)));
        return core;
    }

    /**
     * @dev updates the implementation of the lending pool core
     * @param _lendingPoolCore the new lending pool core implementation
     **/
    function setLendingPoolCoreImpl(address _lendingPoolCore) public onlyOwner {
        updateImplInternal(LENDING_POOL_CORE, _lendingPoolCore);
        emit LendingPoolCoreUpdated(_lendingPoolCore);
    }

    /**
     * @dev returns the address of the LendingPoolConfigurator proxy
     * @return the lending pool configurator proxy address
     **/
    function getLendingPoolConfigurator() public view returns (address) {
        return getAddress(LENDING_POOL_CONFIGURATOR);
    }

    /**
     * @dev updates the implementation of the lending pool configurator
     * @param _configurator the new lending pool configurator implementation
     **/
    function setLendingPoolConfiguratorImpl(address _configurator)
        public
        onlyOwner
    {
        updateImplInternal(LENDING_POOL_CONFIGURATOR, _configurator);
        emit LendingPoolConfiguratorUpdated(_configurator);
    }

    /**
     * @dev returns the address of the LendingPoolDataProvider proxy
     * @return the lending pool data provider proxy address
     */
    function getLendingPoolDataProvider() public view returns (address) {
        return getAddress(DATA_PROVIDER);
    }

    /**
     * @dev updates the implementation of the lending pool data provider
     * @param _provider the new lending pool data provider implementation
     **/
    function setLendingPoolDataProviderImpl(address _provider)
        public
        onlyOwner
    {
        updateImplInternal(DATA_PROVIDER, _provider);
        emit LendingPoolDataProviderUpdated(_provider);
    }

    /**
     * @dev returns the address of the LendingPoolParametersProvider proxy
     * @return the address of the Lending pool parameters provider proxy
     **/
    function getLendingPoolParametersProvider() public view returns (address) {
        return getAddress(LENDING_POOL_PARAMETERS_PROVIDER);
    }

    /**
     * @dev updates the implementation of the lending pool parameters provider
     * @param _parametersProvider the new lending pool parameters provider implementation
     **/
    function setLendingPoolParametersProvider(address _parametersProvider)
        public
        onlyOwner
    {
        _setAddress(LENDING_POOL_PARAMETERS_PROVIDER, _parametersProvider);
        emit LendingPoolParametersProviderUpdated(_parametersProvider);
    }

    /**
     * @dev returns the address of the FeeProvider proxy
     * @return the address of the Fee provider proxy
     **/
    function getFeeProvider() public view returns (address) {
        return getAddress(FEE_PROVIDER);
    }

    /**
     * @dev updates the implementation of the FeeProvider proxy
     * @param _feeProvider the new lending pool fee provider implementation
     **/
    function setFeeProviderImpl(address _feeProvider) public onlyOwner {
        updateImplInternal(FEE_PROVIDER, _feeProvider);
        emit FeeProviderUpdated(_feeProvider);
    }

    /**
     * @dev returns the address of the LendingPoolLiquidationManager. Since the
     * manager is used
     * through delegateCall within the LendingPool contract, the proxy contract
     * pattern does not work properly hence
     * the addresses are changed directly.
     * @return the address of the Lending pool liquidation manager
     **/

    function getLendingPoolLiquidationManager() public view returns (address) {
        return getAddress(LENDING_POOL_LIQUIDATION_MANAGER);
    }

    /**
     * @dev updates the address of the Lending pool liquidation manager
     * @param _manager the new lending pool liquidation manager address
     **/
    function setLendingPoolLiquidationManager(address _manager)
        public
        onlyOwner
    {
        _setAddress(LENDING_POOL_LIQUIDATION_MANAGER, _manager);
        emit LendingPoolLiquidationManagerUpdated(_manager);
    }

    /**
     * @dev the functions below are storing specific addresses that are outside
     * the context of the protocol
     * hence the upgradable proxy pattern is not used
     **/

    function getLendingPoolManager() public view returns (address) {
        return getAddress(LENDING_POOL_MANAGER);
    }

    function setLendingPoolManager(address _lendingPoolManager)
        public
        onlyOwner
    {
        _setAddress(LENDING_POOL_MANAGER, _lendingPoolManager);
        emit LendingPoolManagerUpdated(_lendingPoolManager);
    }

    function getPriceOracle() public view returns (address) {
        return getAddress(PRICE_ORACLE);
    }

    function setPriceOracle(address _priceOracle) public onlyOwner {
        _setAddress(PRICE_ORACLE, _priceOracle);
        emit PriceOracleUpdated(_priceOracle);
    }

    function getLendingRateOracle() public view returns (address) {
        return getAddress(LENDING_RATE_ORACLE);
    }

    function setLendingRateOracle(address _lendingRateOracle) public onlyOwner {
        _setAddress(LENDING_RATE_ORACLE, _lendingRateOracle);
        emit LendingRateOracleUpdated(_lendingRateOracle);
    }

    function getRewardManager() public view returns (address) {
        return getAddress(REWARD_MANAGER);
    }

    function setRewardManager(address _manager) public onlyOwner {
        _setAddress(REWARD_MANAGER, _manager);
        emit RewardManagerUpdated(_manager);
    }

    function getLpRewardVault() public view returns (address) {
        return getAddress(LP_REWARD_VAULT);
    }

    function setLpRewardVault(address _address) public onlyOwner {
        _setAddress(LP_REWARD_VAULT, _address);
        emit LiquidityRewardVaultUpdated(_address);
    }

    function getGovRewardVault() public view returns (address) {
        return getAddress(GOV_REWARD_VAULT);
    }

    function setGovRewardVault(address _address) public onlyOwner {
        _setAddress(GOV_REWARD_VAULT, _address);
        emit GovRewardVaultUpdated(_address);
    }

    function getSafetyRewardVault() public view returns (address) {
        return getAddress(SAFETY_REWARD_VAULT);
    }

    function setSafetyRewardVault(address _address) public onlyOwner {
        _setAddress(SAFETY_REWARD_VAULT, _address);
        emit SafetyRewardVaultUpdated(_address);
    }

    function getStakingToken() public view returns (address) {
        return getAddress(STAKING_TOKEN);
    }

    function setStakingToken(address _address) public onlyOwner {
        _setAddress(STAKING_TOKEN, _address);
        emit StakingTokenUpdated(_address);
    }

    /**
     * @dev internal function to update the implementation of a specific component of the protocol
     * @param _id the id of the contract to be updated
     * @param _newAddress the address of the new implementation
     **/
    function updateImplInternal(bytes32 _id, address _newAddress) internal {
        address payable proxyAddress = address(uint160(getAddress(_id)));

        InitializableAdminUpgradeabilityProxy proxy =
            InitializableAdminUpgradeabilityProxy(proxyAddress);
        bytes memory params =
            abi.encodeWithSignature("initialize(address)", address(this));

        if (proxyAddress == address(0)) {
            proxy = new InitializableAdminUpgradeabilityProxy();
            proxy.initialize(_newAddress, address(this), params);
            _setAddress(_id, address(proxy));
            emit ProxyCreated(_id, address(proxy));
        } else {
            proxy.upgradeToAndCall(_newAddress, params);
        }
    }
}






























/**
 * @title Multiplier ERC20 mToken
 * @dev Implementation of the interest bearing token for the DLP protocol.
 * @author Multiplier Finance
 */
contract mToken is ERC20, ERC20Detailed {
    using WadRayMath for uint256;

    uint256 public constant UINT_MAX_VALUE = uint256(-1);

    /**
     * @dev emitted after the redeem action
     * @param _from the address performing the redeem
     * @param _value the amount to be redeemed
     * @param _fromBalanceIncrease the cumulated balance since the last update
     * of the user
     * @param _fromIndex the last index of the user
     **/
    event Redeem(
        address indexed _from,
        uint256 _value,
        uint256 _fromBalanceIncrease,
        uint256 _fromIndex
    );

    /**
     * @dev emitted after the mint action
     * @param _from the address performing the mint
     * @param _value the amount to be minted
     * @param _fromBalanceIncrease the cumulated balance since the last update
     * of the user
     * @param _fromIndex the last index of the user
     **/
    event MintOnDeposit(
        address indexed _from,
        uint256 _value,
        uint256 _fromBalanceIncrease,
        uint256 _fromIndex
    );

    /**
     * @dev emitted during the liquidation action, when the liquidator reclaims
     * the underlying asset
     * @param _from the address from which the tokens are being burned
     * @param _value the amount to be burned
     * @param _fromBalanceIncrease the cumulated balance since the last update
     * of the user
     * @param _fromIndex the last index of the user
     **/
    event BurnOnLiquidation(
        address indexed _from,
        uint256 _value,
        uint256 _fromBalanceIncrease,
        uint256 _fromIndex
    );

    /**
     * @dev emitted during the transfer action
     * @param _from the address from which the tokens are being transferred
     * @param _to the adress of the destination
     * @param _value the amount to be minted
     * @param _fromBalanceIncrease the cumulated balance since the last update
     * of the user
     * @param _toBalanceIncrease the cumulated balance since the last update of
     * the destination
     * @param _fromIndex the last index of the user
     * @param _toIndex the last index of the liquidator
     **/
    event BalanceTransfer(
        address indexed _from,
        address indexed _to,
        uint256 _value,
        uint256 _fromBalanceIncrease,
        uint256 _toBalanceIncrease,
        uint256 _fromIndex,
        uint256 _toIndex
    );

    /**
     * @dev emitted when the accumulation of the interest
     * by an user is redirected to another user
     * @param _from the address from which the interest is being redirected
     * @param _to the adress of the destination
     * @param _fromBalanceIncrease the cumulated balance since the last update
     * of the user
     * @param _fromIndex the last index of the user
     **/
    event InterestStreamRedirected(
        address indexed _from,
        address indexed _to,
        uint256 _redirectedBalance,
        uint256 _fromBalanceIncrease,
        uint256 _fromIndex
    );

    /**
     * @dev emitted when the redirected balance of an user is being updated
     * @param _targetAddress the address of which the balance is being updated
     * @param _targetBalanceIncrease the cumulated balance since the last update
     * of the target
     * @param _targetIndex the last index of the user
     * @param _redirectedBalanceAdded the redirected balance being added
     * @param _redirectedBalanceRemoved the redirected balance being removed
     **/
    event RedirectedBalanceUpdated(
        address indexed _targetAddress,
        uint256 _targetBalanceIncrease,
        uint256 _targetIndex,
        uint256 _redirectedBalanceAdded,
        uint256 _redirectedBalanceRemoved
    );

    event InterestRedirectionAllowanceChanged(
        address indexed _from,
        address indexed _to
    );

    address public underlyingAssetAddress;

    mapping(address => uint256) private userIndexes;
    mapping(address => address) private interestRedirectionAddresses;
    mapping(address => uint256) private redirectedBalances;
    mapping(address => address) private interestRedirectionAllowances;

    LendingPoolAddressesProvider private addressesProvider;
    LendingPoolCore private core;
    LendingPool private pool;
    LendingPoolDataProvider private dataProvider;

    modifier onlyLendingPool {
        require(
            msg.sender == address(pool),
            "The caller of this function must be a lending pool"
        );
        _;
    }

    modifier whenTransferAllowed(address _from, uint256 _amount) {
        require(
            isTransferAllowed(_from, _amount),
            "Transfer cannot be allowed."
        );
        _;
    }

    constructor(
        LendingPoolAddressesProvider _addressesProvider,
        address _underlyingAsset,
        uint8 _underlyingAssetDecimals,
        string memory _name,
        string memory _symbol
    ) public ERC20Detailed(_name, _symbol, _underlyingAssetDecimals) {
        addressesProvider = _addressesProvider;
        core = LendingPoolCore(addressesProvider.getLendingPoolCore());
        pool = LendingPool(addressesProvider.getLendingPool());
        dataProvider = LendingPoolDataProvider(
            addressesProvider.getLendingPoolDataProvider()
        );
        underlyingAssetAddress = _underlyingAsset;
    }

    /**
     * @notice ERC20 implementation internal function backing transfer() and
     * transferFrom()
     * @dev validates the transfer before allowing it. NOTE: This is not
     * standard ERC20 behavior
     **/
    function _transfer(
        address _from,
        address _to,
        uint256 _amount
    ) internal whenTransferAllowed(_from, _amount) {
        executeTransferInternal(_from, _to, _amount);
    }

    /**
     * @dev redirects the interest generated to a target address.
     * when the interest is redirected, the user balance is added to
     * the recepient redirected balance.
     * @param _to the address to which the interest will be redirected
     **/
    function redirectInterestStream(address _to) external {
        redirectInterestStreamInternal(msg.sender, _to);
    }

    /**
     * @dev redirects the interest generated by _from to a target address.
     * when the interest is redirected, the user balance is added to
     * the recepient redirected balance. The caller needs to have allowance on
     * the interest redirection to be able to execute the function.
     * @param _from the address of the user whom interest is being redirected
     * @param _to the address to which the interest will be redirected
     **/
    function redirectInterestStreamOf(address _from, address _to) external {
        require(
            msg.sender == interestRedirectionAllowances[_from],
            "Caller is not allowed to redirect the interest of the user"
        );
        redirectInterestStreamInternal(_from, _to);
    }

    /**
     * @dev gives allowance to an address to execute the interest redirection
     * on behalf of the caller.
     * @param _to the address to which the interest will be redirected. Pass
     * address(0) to reset
     * the allowance.
     **/
    function allowInterestRedirectionTo(address _to) external {
        require(_to != msg.sender, "User cannot give allowance to himself");
        interestRedirectionAllowances[msg.sender] = _to;
        emit InterestRedirectionAllowanceChanged(msg.sender, _to);
    }

    /**
     * @dev redeems mToken for the underlying asset
     * @param _amount the amount being redeemed
     **/
    function redeem(uint256 _amount) external {
        require(_amount > 0, "Amount to redeem needs to be > 0");

        // RewardManager: Update LP for this user and accumulate his rewards //
        updateLpReward(msg.sender);

        //cumulates the balance of the user
        (, uint256 currentBalance, uint256 balanceIncrease, uint256 index) =
            cumulateBalanceInternal(msg.sender);

        uint256 amountToRedeem = _amount;

        //if amount is equal to uint(-1), the user wants to redeem everything
        if (_amount == UINT_MAX_VALUE) {
            amountToRedeem = currentBalance;
        }

        require(
            amountToRedeem <= currentBalance,
            "User cannot redeem more than the available balance"
        );

        //check that the user is allowed to redeem the amount
        require(
            isTransferAllowed(msg.sender, amountToRedeem),
            "Transfer cannot be allowed."
        );

        // if the user is redirecting his interest towards someone else,
        // we update the redirected balance of the redirection address by adding
        // the accrued interest,
        // and removing the amount to redeem
        updateRedirectedBalanceOfRedirectionAddressInternal(
            msg.sender,
            balanceIncrease,
            amountToRedeem
        );

        // burns tokens equivalent to the amount requested
        _burn(msg.sender, amountToRedeem);

        bool userIndexReset = false;
        // reset the user data if the remaining balance is 0
        if (currentBalance.sub(amountToRedeem) == 0) {
            userIndexReset = resetDataOnZeroBalanceInternal(msg.sender);
        }

        // executes redeem of the underlying asset
        pool.redeemUnderlying(
            underlyingAssetAddress,
            msg.sender,
            amountToRedeem,
            currentBalance.sub(amountToRedeem)
        );

        emit Redeem(
            msg.sender,
            amountToRedeem,
            balanceIncrease,
            userIndexReset ? 0 : index
        );
    }

    /**
     * @dev mints token in the event of users depositing the underlying asset
     * into the lending pool
     * only lending pools can call this function
     * @param _account the address receiving the minted tokens
     * @param _amount the amount of tokens to mint
     */
    function mintOnDeposit(address _account, uint256 _amount)
        external
        onlyLendingPool
    {
        // RewardManager: Update LP for this user and accumulate his rewards //
        updateLpReward(_account);

        //cumulates the balance of the user
        (, , uint256 balanceIncrease, uint256 index) =
            cumulateBalanceInternal(_account);

        // if the user is redirecting his interest towards someone else,
        // we update the redirected balance of the redirection address by
        // adding the accrued interest and the amount deposited
        updateRedirectedBalanceOfRedirectionAddressInternal(
            _account,
            balanceIncrease.add(_amount),
            0
        );
        
        // mint an equivalent amount of tokens to cover the new deposit
        _mint(_account, _amount);

        emit MintOnDeposit(_account, _amount, balanceIncrease, index);
    }

    /**
     * @dev burns token in the event of a borrow being liquidated, in case the
     * liquidators reclaims the underlying asset
     * Transfer of the liquidated asset is executed by the lending pool
     * contract.
     * only lending pools can call this function
     * @param _account the address from which burn the mTokens
     * @param _value the amount to burn
     **/
    function burnOnLiquidation(address _account, uint256 _value)
        external
        onlyLendingPool
    {
        // RewardManager: Update LP for this user and accumulate his rewards before burning //
        updateLpReward(_account);

        // cumulates the balance of the user being liquidated
        (, uint256 accountBalance, uint256 balanceIncrease, uint256 index) =
            cumulateBalanceInternal(_account);

        // adds the accrued interest and substracts the burned amount to
        // the redirected balance
        updateRedirectedBalanceOfRedirectionAddressInternal(
            _account,
            balanceIncrease,
            _value
        );

        // burns the requested amount of tokens
        _burn(_account, _value);

        bool userIndexReset = false;
        // reset the user data if the remaining balance is 0
        if (accountBalance.sub(_value) == 0) {
            userIndexReset = resetDataOnZeroBalanceInternal(_account);
        }

        emit BurnOnLiquidation(
            _account,
            _value,
            balanceIncrease,
            userIndexReset ? 0 : index
        );
    }

    /**
     * @dev transfers tokens in the event of a borrow being liquidated, in case
     * the liquidators reclaims the mToken
     *      only lending pools can call this function
     * @param _from the address from which transfer the mTokens
     * @param _to the destination address
     * @param _value the amount to transfer
     **/
    function transferOnLiquidation(
        address _from,
        address _to,
        uint256 _value
    ) external onlyLendingPool {
        // being a normal transfer, the Transfer() and BalanceTransfer() are
        // emitted
        // so no need to emit a specific event here
        executeTransferInternal(_from, _to, _value);
    }

    /**
     * @dev calculates the balance of the user, which is the
     * principal balance + interest generated by the principal balance +
     * interest generated by the redirected balance
     * @param _user the user for which the balance is being calculated
     * @return the total balance of the user
     **/
    function balanceOf(address _user) public view returns (uint256) {
        // current principal balance of the user
        uint256 currentPrincipalBalance = super.balanceOf(_user);
        // balance redirected by other users to _user for interest rate accrual
        uint256 redirectedBalance = redirectedBalances[_user];

        if (currentPrincipalBalance == 0 && redirectedBalance == 0) {
            return 0;
        }
        // if the _user is not redirecting the interest to anybody, accrues
        // the interest for himself

        if (interestRedirectionAddresses[_user] == address(0)) {
            // accruing for himself means that both the principal balance and
            // the redirected balance partecipate in the interest
            return
                calculateCumulatedBalanceInternal(
                    _user,
                    currentPrincipalBalance.add(redirectedBalance)
                )
                    .sub(redirectedBalance);
        } else {
            // if the user redirected the interest, then only the redirected
            // balance generates interest. In that case, the interest generated
            // by the redirected balance is added to the current principal
            // balance.
            return
                currentPrincipalBalance.add(
                    calculateCumulatedBalanceInternal(_user, redirectedBalance)
                        .sub(redirectedBalance)
                );
        }
    }

    /**
     * @dev returns the principal balance of the user. The principal balance is
     * the last
     * updated stored balance, which does not consider the perpetually accruing
     * interest.
     * @param _user the address of the user
     * @return the principal balance of the user
     **/
    function principalBalanceOf(address _user) external view returns (uint256) {
        return super.balanceOf(_user);
    }

    /**
        @dev return total principal supply of mToken
        @return the current principal total supply
    */
    function principalTotalSupply() external view returns (uint256) {
        return super.totalSupply();
    }

    /**
     * @dev calculates the total supply of the specific mToken
     * since the balance of every single user increases over time, the total
     * supply
     * does that too.
     * @return the current total supply
     **/
    function totalSupply() public view returns (uint256) {
        uint256 currentSupplyPrincipal = super.totalSupply();

        if (currentSupplyPrincipal == 0) {
            return 0;
        }

        return
            currentSupplyPrincipal
                .wadToRay()
                .rayMul(core.getReserveNormalizedIncome(underlyingAssetAddress))
                .rayToWad();
    }

    /**
     * @dev Used to validate transfers before actually executing them.
     * @param _user address of the user to check
     * @param _amount the amount to check
     * @return true if the _user can transfer _amount, false otherwise
     **/
    function isTransferAllowed(address _user, uint256 _amount)
        public
        view
        returns (bool)
    {
        return
            dataProvider.balanceDecreaseAllowed(
                underlyingAssetAddress,
                _user,
                _amount
            );
    }

    /**
     * @dev returns the last index of the user, used to calculate the balance of
     * the user
     * @param _user address of the user
     * @return the last user index
     **/
    function getUserIndex(address _user) external view returns (uint256) {
        return userIndexes[_user];
    }

    /**
     * @dev returns the address to which the interest is redirected
     * @param _user address of the user
     * @return 0 if there is no redirection, an address otherwise
     **/
    function getInterestRedirectionAddress(address _user)
        external
        view
        returns (address)
    {
        return interestRedirectionAddresses[_user];
    }

    /**
     * @dev returns the redirected balance of the user. The redirected balance
     * is the balance
     * redirected by other accounts to the user, that is accrueing interest for
     * him.
     * @param _user address of the user
     * @return the total redirected balance
     **/
    function getRedirectedBalance(address _user)
        external
        view
        returns (uint256)
    {
        return redirectedBalances[_user];
    }

    /**
     * @dev accumulates the accrued interest of the user to the principal
     * balance
     * @param _user the address of the user for which the interest is being
     * accumulated
     * @return the previous principal balance, the new principal balance, the
     * balance increase
     * and the new user index
     **/
    function cumulateBalanceInternal(address _user)
        internal
        returns (
            uint256,
            uint256,
            uint256,
            uint256
        )
    {
        uint256 previousPrincipalBalance = super.balanceOf(_user);

        // calculate the accrued interest since the last accumulation
        uint256 balanceIncrease =
            balanceOf(_user).sub(previousPrincipalBalance);
        // mints an amount of tokens equivalent to the amount accumulated
        if (balanceIncrease != 0) {
            _mint(_user, balanceIncrease);
        }

        // updates the user index
        uint256 index =
            userIndexes[_user] = core.getReserveNormalizedIncome(
                underlyingAssetAddress
            );
        return (
            previousPrincipalBalance,
            previousPrincipalBalance.add(balanceIncrease),
            balanceIncrease,
            index
        );
    }

    /**
     * @dev updates the redirected balance of the user. If the user is not
     * redirecting his
     * interest, nothing is executed.
     * @param _user the address of the user for which the interest is being
     * accumulated
     * @param _balanceToAdd the amount to add to the redirected balance
     * @param _balanceToRemove the amount to remove from the redirected balance
     **/
    function updateRedirectedBalanceOfRedirectionAddressInternal(
        address _user,
        uint256 _balanceToAdd,
        uint256 _balanceToRemove
    ) internal {
        address redirectionAddress = interestRedirectionAddresses[_user];
        // if there isn't any redirection, nothing to be done
        if (redirectionAddress == address(0)) {
            return;
        }

        // compound balances of the redirected address
        (, , uint256 balanceIncrease, uint256 index) =
            cumulateBalanceInternal(redirectionAddress);

        // updating the redirected balance
        redirectedBalances[redirectionAddress] = redirectedBalances[
            redirectionAddress
        ]
            .add(_balanceToAdd)
            .sub(_balanceToRemove);

        // if the interest of redirectionAddress is also being redirected
        // we need to update
        // the redirected balance of the redirection target by adding the
        // balance increase

        address targetOfRedirectionAddress =
            interestRedirectionAddresses[redirectionAddress];

        if (targetOfRedirectionAddress != address(0)) {
            redirectedBalances[targetOfRedirectionAddress] = redirectedBalances[
                targetOfRedirectionAddress
            ]
                .add(balanceIncrease);
        }

        emit RedirectedBalanceUpdated(
            redirectionAddress,
            balanceIncrease,
            index,
            _balanceToAdd,
            _balanceToRemove
        );
    }

    /**
     * @dev calculate the interest accrued by _user on a specific balance
     * @param _user the address of the user for which the interest is being
     * accumulated
     * @param _balance the balance on which the interest is calculated
     * @return the interest rate accrued
     **/
    function calculateCumulatedBalanceInternal(address _user, uint256 _balance)
        internal
        view
        returns (uint256)
    {
        return
            _balance
                .wadToRay()
                .rayMul(core.getReserveNormalizedIncome(underlyingAssetAddress))
                .rayDiv(userIndexes[_user])
                .rayToWad();
    }

    /**
     * @dev executes the transfer of mTokens, invoked by both _transfer() and
     *      transferOnLiquidation()
     * @param _from the address from which transfer the mTokens
     * @param _to the destination address
     * @param _value the amount to transfer
     **/
    function executeTransferInternal(
        address _from,
        address _to,
        uint256 _value
    ) internal {
        require(_value > 0, "Transferred amount needs to be greater than zero");

        // RewardManager: Update the reward for these 2 users //
        updateLpReward(_from);
        updateLpReward(_to);

        // cumulate the balance of the sender
        (
            ,
            uint256 fromBalance,
            uint256 fromBalanceIncrease,
            uint256 fromIndex
        ) = cumulateBalanceInternal(_from);

        // cumulate the balance of the receiver
        (, , uint256 toBalanceIncrease, uint256 toIndex) =
            cumulateBalanceInternal(_to);

        // if the sender is redirecting his interest towards someone else,
        // adds to the redirected balance the accrued interest and removes the
        // amount
        // being transferred
        updateRedirectedBalanceOfRedirectionAddressInternal(
            _from,
            fromBalanceIncrease,
            _value
        );

        // if the receiver is redirecting his interest towards someone else,
        // adds to the redirected balance the accrued interest and the amount
        // being transferred
        updateRedirectedBalanceOfRedirectionAddressInternal(
            _to,
            toBalanceIncrease.add(_value),
            0
        );

        // performs the transfer
        super._transfer(_from, _to, _value);

        bool fromIndexReset = false;
        // reset the user data if the remaining balance is 0
        if (fromBalance.sub(_value) == 0) {
            fromIndexReset = resetDataOnZeroBalanceInternal(_from);
        }

        emit BalanceTransfer(
            _from,
            _to,
            _value,
            fromBalanceIncrease,
            toBalanceIncrease,
            fromIndexReset ? 0 : fromIndex,
            toIndex
        );
    }

    /**
     * @dev executes the redirection of the interest from one address to
     * another.
     * immediately after redirection, the destination address will start to
     * accrue interest.
     * @param _from the address from which transfer the mTokens
     * @param _to the destination address
     **/
    function redirectInterestStreamInternal(address _from, address _to)
        internal
    {
        address currentRedirectionAddress = interestRedirectionAddresses[_from];

        require(
            _to != currentRedirectionAddress,
            "Interest is already redirected to the user"
        );

        // accumulates the accrued interest to the principal
        (
            uint256 previousPrincipalBalance,
            uint256 fromBalance,
            uint256 balanceIncrease,
            uint256 fromIndex
        ) = cumulateBalanceInternal(_from);

        require(
            fromBalance > 0,
            "Interest stream can only be redirected if there is a valid balance"
        );

        // if the user is already redirecting the interest to someone, before
        // changing
        // the redirection address we substract the redirected balance of the
        // previous recipient
        if (currentRedirectionAddress != address(0)) {
            updateRedirectedBalanceOfRedirectionAddressInternal(
                _from,
                0,
                previousPrincipalBalance
            );
        }

        // if the user is redirecting the interest back to himself,
        // we simply set to 0 the interest redirection address
        if (_to == _from) {
            interestRedirectionAddresses[_from] = address(0);
            emit InterestStreamRedirected(
                _from,
                address(0),
                fromBalance,
                balanceIncrease,
                fromIndex
            );
            return;
        }

        // first set the redirection address to the new recipient
        interestRedirectionAddresses[_from] = _to;

        // adds the user balance to the redirected balance of the destination
        updateRedirectedBalanceOfRedirectionAddressInternal(
            _from,
            fromBalance,
            0
        );

        emit InterestStreamRedirected(
            _from,
            _to,
            fromBalance,
            balanceIncrease,
            fromIndex
        );
    }

    /**
     * @dev function to reset the interest stream redirection and the user
     * index, if the
     * user has no balance left.
     * @param _user the address of the user
     * @return true if the user index has also been reset, false otherwise.
     * useful to emit the proper user index value
     **/
    function resetDataOnZeroBalanceInternal(address _user)
        internal
        returns (bool)
    {
        //if the user has 0 principal balance, the interest stream redirection
        // gets reset
        interestRedirectionAddresses[_user] = address(0);

        //emits a InterestStreamRedirected event to notify that the redirection
        // has been reset
        emit InterestStreamRedirected(_user, address(0), 0, 0, 0);

        //if the redirected balance is also 0, we clear up the user index
        if (redirectedBalances[_user] == 0) {
            userIndexes[_user] = 0;
            return true;
        } else {
            return false;
        }
    }

    function updateLpReward(address _user) private {
        // RewardManager: Update LP for this user and accumulate his rewards //
        pool.updateLpReward(underlyingAssetAddress, _user);
    }
}













contract RewardsManager is Ownable, ReentrancyGuard {
    using SafeMath for uint256;
    using SafeERC20 for ERC20;

    address public lendingPoolAddress;
    address public coreAddress;
    address public stakingToken;
    LendingPoolAddressesProvider public addressesProvider;

    /**
     * @dev - An enum for the types of rewards
     */
    enum RewardTypes {Depositor, Governance, Safety}

    /**
     * @dev - A struct to store a user's accumulated reward and the pointer to the next claimable reward.
     */
    struct UserClaim {
        uint256 nextClaimablePtr;
        uint256 accumReward;
    }

    /**
     * @dev - A struct to store the RewardItem.
     * @notice amount - The amount of reward.
     * @notice paidSoFar - The amount that a user has accumulated or claimed from this reward item.
     * @notice totalSharingBase - For Depositor reward, this represent the total liquidity. For Governance reward, this represent the total bMxx staked.
     */
    struct RewardItem {
        uint256 amount;
        uint256 paidSoFar;
        uint256 totalSharingBase; // Either total LP or total bMxx Staking amount //
    }

    /**
     * @dev - A struct to store the individual RewardPools.
     * @notice claims - A mapping of user address to UserClaims.
     * @notice rewards - A mapping of RewardItems, retrieved by index. The index starts from 0.
     * @notice nextRewardPtr - Stores the index to the next reward item.
     * @notice valid - Determines whether a pool is valid or not.
     */
    struct RewardPool {
        mapping(address => UserClaim[2]) claims;
        mapping(uint256 => RewardItem[2]) rewards;
        uint256 nextRewardPtr;
        bool valid;
    }

    mapping(address => RewardPool) public rewardPools;

    /**
     * @dev only LendingPool can use functions affected by this modifier
     **/
    modifier onlyLendingPool {
        require(
            lendingPoolAddress == msg.sender,
            "The caller must be a lending pool contract"
        );
        _;
    }

    /**
     * @dev only LendingPool or Core can use functions affected by this modifier
     **/
    modifier onlyLendingPoolOrCore {
        require(
            lendingPoolAddress == msg.sender || coreAddress == msg.sender,
            "The caller must be a lending pool or core contract"
        );
        _;
    }

    constructor(LendingPoolAddressesProvider _addressProvider)
        public
        Ownable()
    {
        addressesProvider = _addressProvider;
        lendingPoolAddress = addressesProvider.getLendingPool();
        coreAddress = addressesProvider.getLendingPoolCore();
        stakingToken = addressesProvider.getStakingToken();
    }

    /**
     * @dev This function will register the reward pools based on an array of reserves
     * @param _reserves - Array of addresses
     * Access Control: Only Lending Pools
     */
    function registerPools(address[] memory _reserves)
        public
        onlyLendingPool
        nonReentrant
    {
        uint256 len = _reserves.length;
        for (uint256 i = 0; i < len; i++) {
            RewardPool memory pool = rewardPools[_reserves[i]];
            if (!pool.valid) {
                rewardPools[_reserves[i]] = RewardPool({
                    nextRewardPtr: 0,
                    valid: true
                });
            }
        }
    }

    /**
     * @dev This function will add a new reward item.
     * @param _reserve - The reserve of the lending pool.
     * @param _lpRewardAmt - The amount of reward for depositors.
     * @param lpBase - The total amount of the mToken supplied.
     * @param govRewardAmt - The amount of reward for the governance stakers.
     * Access Control: Only Lending Pools or Core
     */
    function addRewardItem(
        address _reserve,
        uint256 _lpRewardAmt,
        uint256 lpBase,
        uint256 govRewardAmt
    ) public onlyLendingPoolOrCore nonReentrant {
        // Check for any zero inputs//
        if (_lpRewardAmt == 0 || govRewardAmt == 0 || lpBase == 0) {
            return;
        }

        RewardPool storage pool = rewardPools[_reserve];
        require(pool.valid, "Unknown reserve pool in Reward Manager");

        pool.rewards[pool.nextRewardPtr][
            uint256(RewardTypes.Depositor)
        ] = RewardItem(_lpRewardAmt, 0, lpBase);

        uint256 stakedTokenSupply = IERC20(stakingToken).totalSupply();
        if (stakedTokenSupply != 0) {
            pool.rewards[pool.nextRewardPtr][
                uint256(RewardTypes.Governance)
            ] = RewardItem(govRewardAmt, 0, stakedTokenSupply);
        }
        pool.nextRewardPtr = pool.nextRewardPtr.add(1);
    }

    /**
     * @dev This function will update both the Depositor and Governance reward of the user.
     * @param _reserve - The reserve of the lending pool.
     * @param _user - The user's address.
     * @param _sharesLp - The amount of the mToken the user has.
     * @param _sharesGov - The amount of stBMXX token the user has.
     * @param _num - The number of reward items to update. Input 0 to update all available reward items.
     * Access Control: Only Lending Pools
     */
    function updateRewards(
        address _reserve,
        address _user,
        uint256 _sharesLp,
        uint256 _sharesGov,
        uint256 _num
    ) public onlyLendingPool {
        updateReward(_reserve, _user, RewardTypes.Depositor, _sharesLp, _num);
        updateReward(_reserve, _user, RewardTypes.Governance, _sharesGov, _num);
    }

    /**
     * @dev Called by LendingPool to accumulate and update the reward for a user.
     * @param _reserve The lending pool reserver.
     * @param _user The address of the user.
     * @param _type The type of reward to update.
     * @param _type The amount of reward.
     * @param _num The number of reward items to update. Input 0 to update all available reward items.
     * Access Control: Only Lending Pools
     **/
    function updateReward(
        address _reserve,
        address _user,
        RewardTypes _type,
        uint256 _shares,
        uint256 _num
    ) public onlyLendingPool nonReentrant {
        RewardPool storage pool = rewardPools[_reserve];
        require(pool.valid, "Unknown reserve pool in Reward Manager");

        // Pool has no reward yet ?
        if (pool.nextRewardPtr == 0) {
            return;
        }

        // For optimization, if _shares is 0, then we just update the next claimable pointer
        if (_shares == 0) {
            updateRange(pool, _user, uint256(_type), 0, 0, 0, false);
            return;
        }

        (uint256 start, uint256 end, bool hasNewReward) =
            getValidRange(pool, _user, uint256(_type), _num);
        updateRange(
            pool,
            _user,
            uint256(_type),
            start,
            end,
            _shares,
            hasNewReward
        );
    }

    /**
     * @dev This function will read the available reward of the user.
     * @param _reserve - The reserve of the lending pool.
     * @param _user - The user's address.
     * @param _type - The type of reward.
     * @param _share - The amount of mToken or stBMXX token, depending on the _type specified.
     */
    function readRewards(
        address _reserve,
        address _user,
        RewardTypes _type,
        uint256 _share
    ) public view returns (uint256) {
        RewardPool storage pool = rewardPools[_reserve];
        require(pool.valid, "Unknown reserve pool in Reward Manager");

        // Pool has no reward yet ?
        if (pool.nextRewardPtr == 0) {
            return 0;
        }

        uint256 rewardAmt = pool.claims[_user][uint256(_type)].accumReward;
        if (_share == 0) {
            return rewardAmt;
        }

        (uint256 start, uint256 end, bool hasNewReward) =
            getValidRange(pool, _user, uint256(_type), 0);
        if (hasNewReward) {
            uint256 accum = readRange(pool, uint256(_type), start, end, _share);
            rewardAmt = rewardAmt.add(accum);
        }
        return rewardAmt;
    }

    /**
     * @dev This function will retriev the start and end index of the reward item.
     * @param _pool - The reward pool.
     * @param _user - The user's address.
     * @param _type - The type of reward.
     * @param _num - The number of reward items to update. Input 0 to update all available reward items.
     */
    function getValidRange(
        RewardPool storage _pool,
        address _user,
        uint256 _type,
        uint256 _num
    )
        private
        view
        returns (
            uint256,
            uint256,
            bool
        )
    {
        UserClaim memory claim = _pool.claims[_user][_type];
        if (claim.nextClaimablePtr >= _pool.nextRewardPtr) {
            return (0, 0, false);
        }

        // Find the start and end //
        uint256 end = _pool.nextRewardPtr.sub(1);
        if (_num != 0) {
            uint256 temp = claim.nextClaimablePtr.add(_num - 1);
            if (temp < end) {
                end = temp;
            }
        }
        return (claim.nextClaimablePtr, end, true);
    }

    /**
     * @dev This function will update the accumulated reward of the user based on the range.
     * @param _pool - The reward pool.
     * @param _user - The user's address.
     * @param _type - The type of reward.
     * @param _start - The start of the range.
     * @param _end - The end of the range.
     * @param _share - The amount of mToken or stBMXX token, depending on the _type specified.
     * @param _hasNewReward - Specify whether a new reward is available.
     */
    function updateRange(
        RewardPool storage _pool,
        address _user,
        uint256 _type,
        uint256 _start,
        uint256 _end,
        uint256 _share,
        bool _hasNewReward
    ) private {
        // If there is no new claim, just point the nextClaimPtr to the latest
        if (!_hasNewReward) {
            UserClaim storage claim = _pool.claims[_user][_type];
            claim.nextClaimablePtr = _pool.nextRewardPtr;
            return;
        }

        uint256 accum;
        for (uint256 n = _start; n <= _end; n++) {
            RewardItem memory r = _pool.rewards[n][_type];
            if (r.amount == 0) {
                continue;
            }

            // Enough in RewardItem to pay ?
            uint256 amountAvailable = r.amount.sub(r.paidSoFar);
            if (amountAvailable == 0) {
                continue;
            }

            uint256 rewardAmt = _share.mul(r.amount).div(r.totalSharingBase);

            if (amountAvailable < rewardAmt) {
                rewardAmt = amountAvailable;
            }

            accum = accum.add(rewardAmt);

            // Update
            r.paidSoFar = r.paidSoFar.add(rewardAmt);
            _pool.rewards[n][_type] = r;
        }

        // Update
        UserClaim storage claim = _pool.claims[_user][_type];
        claim.nextClaimablePtr = _end.add(1); // set pointer to next reward in future //
        claim.accumReward = claim.accumReward.add(accum);
    }

    /**
     * @dev This function will read the accumulated reward from a range.
     * @param _pool - The reward pool.
     * @param _type - The type of reward.
     * @param _start - The start of the range.
     * @param _end - The end of the range.
     * @param _share - The amount of mToken or stBMXX token, depending on the _type specified.
     */
    function readRange(
        RewardPool storage _pool,
        uint256 _type,
        uint256 _start,
        uint256 _end,
        uint256 _share
    ) private view returns (uint256) {
        uint256 accum;
        for (uint256 n = _start; n <= _end; n++) {
            RewardItem memory r = _pool.rewards[n][_type];
            if (r.amount == 0) {
                continue;
            }

            // Enough in RewardItem to pay ?
            uint256 amountAvailable = r.amount.sub(r.paidSoFar);
            if (amountAvailable == 0) {
                continue;
            }

            uint256 rewardAmt = _share.mul(r.amount).div(r.totalSharingBase);

            if (amountAvailable < rewardAmt) {
                rewardAmt = amountAvailable;
            }

            accum = accum.add(rewardAmt);
        }
        return accum;
    }

    /**
     * @dev This function will reset the user's accumulated reward to 0 and set the pointer to the pool's next reward.
     * @param _reserve - The reserve of the lending pool.
     * @param _user - The user's address.
     * @param _type - The type of reward.
     * Access Control: Only Lending Pools
     */
    function resetReward(
        address _reserve,
        address _user,
        RewardTypes _type
    ) public onlyLendingPool {
        RewardPool storage pool = rewardPools[_reserve];
        require(pool.valid, "Unknown reserve pool in Reward Manager");

        UserClaim storage claim = pool.claims[_user][uint256(_type)];
        claim.nextClaimablePtr = pool.nextRewardPtr;
        claim.accumReward = 0;
    }

    /**
     * @dev This function will withdraw the reward and send to the user.
     * @param _type - The type of reward.
     * @param _token - The token to withdraw.
     * @param _to - The payee's address.
     * @param _amount - The amount to pay.
     * Access Control: Only Lending Pools
     */
    function withdrawFromVault(
        RewardTypes _type,
        address _token,
        address payable _to,
        uint256 _amount
    ) external onlyLendingPool {
        IRewardVault vault;
        if (_type == RewardTypes.Depositor) {
            vault = IRewardVault(addressesProvider.getLpRewardVault());
        } else if (_type == RewardTypes.Governance) {
            vault = IRewardVault(addressesProvider.getGovRewardVault());
        } else {
            vault = IRewardVault(addressesProvider.getSafetyRewardVault());
        }
        vault.withdraw(_token, _to, _amount);
    }
}




















/**
 * @title LendingPoolLiquidationManager contract
 * @author Multiplier Finance
 * @notice Implements the liquidation function.
 **/
contract LendingPoolLiquidationManager is
    ReentrancyGuard,
    VersionedInitializable
{
    using SafeMath for uint256;
    using WadRayMath for uint256;
    using Address for address;

    LendingPoolAddressesProvider public addressesProvider;
    LendingPoolCore public core;
    LendingPoolDataProvider public dataProvider;
    LendingPoolParametersProvider public parametersProvider;

    uint256 private constant LIQUIDATION_CLOSE_FACTOR_PERCENT = 50;

    /**
     * @dev emitted when a borrow fee is liquidated
     * @param _collateral the address of the collateral being liquidated
     * @param _reserve the address of the reserve
     * @param _user the address of the user being liquidated
     * @param _feeLiquidated the total fee liquidated
     * @param _liquidatedCollateralForFee the amount of collateral received by
     * the protocol in exchange for the fee
     * @param _timestamp the timestamp of the action
     **/
    event OriginationFeeLiquidated(
        address indexed _collateral,
        address indexed _reserve,
        address indexed _user,
        uint256 _feeLiquidated,
        uint256 _liquidatedCollateralForFee,
        uint256 _timestamp
    );

    /**
     * @dev emitted when a borrower is liquidated
     * @param _collateral the address of the collateral being liquidated
     * @param _reserve the address of the reserve
     * @param _user the address of the user being liquidated
     * @param _purchaseAmount the total amount liquidated
     * @param _liquidatedCollateralAmount the amount of collateral being
     * liquidated
     * @param _accruedBorrowInterest the amount of interest accrued by the
     * borrower since the last action
     * @param _liquidator the address of the liquidator
     * @param _receiveMToken true if the liquidator wants to receive
     * mTokens, false otherwise
     * @param _timestamp the timestamp of the action
     **/
    event LiquidationCall(
        address indexed _collateral,
        address indexed _reserve,
        address indexed _user,
        uint256 _purchaseAmount,
        uint256 _liquidatedCollateralAmount,
        uint256 _accruedBorrowInterest,
        address _liquidator,
        bool _receiveMToken,
        uint256 _timestamp
    );

    enum LiquidationErrors {
        NO_ERROR,
        NO_COLLATERAL_AVAILABLE,
        COLLATERAL_CANNOT_BE_LIQUIDATED,
        CURRRENCY_NOT_BORROWED,
        HEALTH_FACTOR_ABOVE_THRESHOLD,
        NOT_ENOUGH_LIQUIDITY
    }

    struct LiquidationCallLocalVars {
        uint256 userCollateralmTokenBalance;
        uint256 userCompoundedBorrowBalance;
        uint256 borrowBalanceIncrease;
        uint256 maxPrincipalAmountToLiquidate;
        uint256 actualAmountToLiquidate;
        uint256 liquidationRatio;
        uint256 collateralPrice;
        uint256 principalCurrencyPrice;
        uint256 maxAmountCollateralToLiquidate;
        uint256 originationFee;
        uint256 feeLiquidated;
        uint256 liquidatedCollateralForFee;
        CoreLibrary.InterestRateMode borrowRateMode;
        uint256 userStableRate;
        bool isCollateralEnabled;
        bool healthFactorBelowThreshold;
    }

    /**
     * @dev as the contract extends the VersionedInitializable contract to match
     * the state
     * of the LendingPool contract, the getRevision() function is needed.
     */
    function getRevision() internal pure returns (uint256) {
        return 0;
    }

    /**
     * @dev users can invoke this function to liquidate an undercollateralized
     * position.
     * @param _collateral the address of the collateral to liquidated
     * @param _reserve the address of the principal reserve
     * @param _user the address of the borrower
     * @param _purchaseAmount the amount of principal that the liquidator wants
     * to repay
     * @param _receiveMToken true if the liquidators wants to receive the
     * mTokens, false if
     * he wants to receive the underlying asset directly
     **/
    function liquidationCall(
        address _collateral,
        address _reserve,
        address _user,
        uint256 _purchaseAmount,
        bool _receiveMToken
    ) external payable returns (uint256, string memory) {
        // Usage of a memory struct of vars to avoid "Stack too deep" errors
        // due to local variables
        LiquidationCallLocalVars memory vars;

        (, , , , , , , vars.healthFactorBelowThreshold) = dataProvider
            .calculateUserGlobalData(_user);

        if (!vars.healthFactorBelowThreshold) {
            return (
                uint256(LiquidationErrors.HEALTH_FACTOR_ABOVE_THRESHOLD),
                "Health factor is not below the threshold"
            );
        }

        vars.userCollateralmTokenBalance = core.getUserUnderlyingAssetBalance(
            _collateral,
            _user
        );

        //if _user hasn't deposited this specific collateral, nothing can be
        // liquidated
        if (vars.userCollateralmTokenBalance == 0) {
            return (
                uint256(LiquidationErrors.NO_COLLATERAL_AVAILABLE),
                "Invalid collateral to liquidate"
            );
        }

        vars.isCollateralEnabled =
            core.isReserveUsageAsCollateralEnabled(_collateral) &&
            core.isUserUseReserveAsCollateralEnabled(_collateral, _user);

        // if _collateral isn't enabled as collateral by _user, it cannot be
        // liquidated
        if (!vars.isCollateralEnabled) {
            return (
                uint256(LiquidationErrors.COLLATERAL_CANNOT_BE_LIQUIDATED),
                "The collateral chosen cannot be liquidated"
            );
        }

        // if the user hasn't borrowed the specific currency defined by
        // _reserve, it cannot be liquidated
        (, vars.userCompoundedBorrowBalance, vars.borrowBalanceIncrease) = core
            .getUserBorrowBalances(_reserve, _user);

        if (vars.userCompoundedBorrowBalance == 0) {
            return (
                uint256(LiquidationErrors.CURRRENCY_NOT_BORROWED),
                "User did not borrow the specified currency"
            );
        }

        // all clear - calculate the max principal amount that can be liquidated
        vars.maxPrincipalAmountToLiquidate = vars
            .userCompoundedBorrowBalance
            .mul(LIQUIDATION_CLOSE_FACTOR_PERCENT)
            .div(100);

        vars.actualAmountToLiquidate = _purchaseAmount >
            vars.maxPrincipalAmountToLiquidate
            ? vars.maxPrincipalAmountToLiquidate
            : _purchaseAmount;

        (uint256 maxCollateralToLiquidate, uint256 principalAmountNeeded) =
            calculateAvailableCollateralToLiquidate(
                _collateral,
                _reserve,
                vars.actualAmountToLiquidate,
                vars.userCollateralmTokenBalance
            );

        vars.originationFee = core.getUserOriginationFee(_reserve, _user);

        // if there is a fee to liquidate, calculate the maximum amount of fee
        // that can be liquidated
        if (vars.originationFee > 0) {
            (
                vars.liquidatedCollateralForFee,
                vars.feeLiquidated
            ) = calculateAvailableCollateralToLiquidate(
                _collateral,
                _reserve,
                vars.originationFee,
                vars.userCollateralmTokenBalance.sub(maxCollateralToLiquidate)
            );
        }

        // If principalAmountNeeded < vars.ActualAmountToLiquidate,
        // there isn't enough
        // of _collateral to cover the actual amount that is being liquidated,
        // hence we liquidate a smaller amount

        if (principalAmountNeeded < vars.actualAmountToLiquidate) {
            vars.actualAmountToLiquidate = principalAmountNeeded;
        }

        // if liquidator reclaims the underlying asset, we make sure there is
        // enough available collateral in the reserve
        if (!_receiveMToken) {
            uint256 currentAvailableCollateral =
                core.getReserveAvailableLiquidity(_collateral);
            if (currentAvailableCollateral < maxCollateralToLiquidate) {
                return (
                    uint256(LiquidationErrors.NOT_ENOUGH_LIQUIDITY),
                    "There isn't enough liquidity available to liquidate"
                );
            }
        }

        core.updateStateOnLiquidation(
            _reserve,
            _collateral,
            _user,
            vars.actualAmountToLiquidate,
            maxCollateralToLiquidate,
            vars.feeLiquidated,
            vars.liquidatedCollateralForFee,
            vars.borrowBalanceIncrease,
            _receiveMToken
        );

        mToken collateralMToken =
            mToken(core.getReservemTokenAddress(_collateral));

        // if liquidator reclaims the mToken, he receives the equivalent
        // mToken amount
        if (_receiveMToken) {
            collateralMToken.transferOnLiquidation(
                _user,
                msg.sender,
                maxCollateralToLiquidate
            );
        } else {
            // otherwise receives the underlying asset
            // burn the equivalent amount of mToken
            collateralMToken.burnOnLiquidation(_user, maxCollateralToLiquidate);
            core.transferToUser(
                _collateral,
                msg.sender,
                maxCollateralToLiquidate
            );
        }

        // transfers the principal currency to the pool
        core.transferToReserve.value(msg.value)(
            _reserve,
            msg.sender,
            vars.actualAmountToLiquidate
        );

        if (vars.feeLiquidated > 0) {
            // if there is enough collateral to liquidate the fee, first
            // transfer burn an equivalent amount of
            // mTokens of the user
            collateralMToken.burnOnLiquidation(
                _user,
                vars.liquidatedCollateralForFee
            );

            // then liquidate the fee by transferring it to the fee collection
            // address
            core.liquidateFee(_collateral, vars.liquidatedCollateralForFee);

            emit OriginationFeeLiquidated(
                _collateral,
                _reserve,
                _user,
                vars.feeLiquidated,
                vars.liquidatedCollateralForFee,
                //solium-disable-next-line
                block.timestamp
            );
        }
        emit LiquidationCall(
            _collateral,
            _reserve,
            _user,
            vars.actualAmountToLiquidate,
            maxCollateralToLiquidate,
            vars.borrowBalanceIncrease,
            msg.sender,
            _receiveMToken,
            //solium-disable-next-line
            block.timestamp
        );

        return (uint256(LiquidationErrors.NO_ERROR), "No errors");
    }

    struct AvailableCollateralToLiquidateLocalVars {
        uint256 userCompoundedBorrowBalance;
        uint256 liquidationBonus;
        uint256 collateralPrice;
        uint256 principalCurrencyPrice;
        uint256 maxAmountCollateralToLiquidate;
        uint256 principalDecimals;
        uint256 collateralDecimals;
    }

    /**
     * @dev calculates how much of a specific collateral can be liquidated, given
     * a certain amount of principal currency. This function needs to be called after
     * all the checks to validate the liquidation have been performed, otherwise it might fail.
     * @param _collateral the collateral to be liquidated
     * @param _principal the principal currency to be liquidated
     * @param _purchaseAmount the amount of principal being liquidated
     * @param _userCollateralBalance the collatera balance for the specific _collateral asset of the user being liquidated
     * @return the maximum amount that is possible to liquidated given all the liquidation constraints (user balance, close factor) and
     * the purchase amount
     **/
    function calculateAvailableCollateralToLiquidate(
        address _collateral,
        address _principal,
        uint256 _purchaseAmount,
        uint256 _userCollateralBalance
    )
        internal
        view
        returns (uint256 collateralAmount, uint256 principalAmountNeeded)
    {
        collateralAmount = 0;
        principalAmountNeeded = 0;
        IPriceOracleGetter oracle =
            IPriceOracleGetter(addressesProvider.getPriceOracle());

        // Usage of a memory struct of vars to avoid "Stack too deep" errors due to local variables
        AvailableCollateralToLiquidateLocalVars memory vars;

        vars.collateralPrice = oracle.getAssetPrice(_collateral);
        vars.principalCurrencyPrice = oracle.getAssetPrice(_principal);
        vars.liquidationBonus = core.getReserveLiquidationBonus(_collateral);
        vars.principalDecimals = core.getReserveDecimals(_principal);
        vars.collateralDecimals = core.getReserveDecimals(_collateral);

        //this is the maximum possible amount of the selected collateral that can be liquidated, given the
        //max amount of principal currency that is available for liquidation.
        vars.maxAmountCollateralToLiquidate = vars
            .principalCurrencyPrice
            .mul(_purchaseAmount)
            .mul(10**vars.collateralDecimals)
            .div(vars.collateralPrice.mul(10**vars.principalDecimals))
            .mul(vars.liquidationBonus)
            .div(100);

        if (vars.maxAmountCollateralToLiquidate > _userCollateralBalance) {
            collateralAmount = _userCollateralBalance;
            principalAmountNeeded = vars
                .collateralPrice
                .mul(collateralAmount)
                .mul(10**vars.principalDecimals)
                .div(
                vars.principalCurrencyPrice.mul(10**vars.collateralDecimals)
            )
                .mul(100)
                .div(vars.liquidationBonus);
        } else {
            collateralAmount = vars.maxAmountCollateralToLiquidate;
            principalAmountNeeded = _purchaseAmount;
        }

        return (collateralAmount, principalAmountNeeded);
    }
}


/**
 * @title LendingPoolConfigurator contract
 * @author Multiplier Finance
 * @notice Executes configuration methods on the LendingPoolCore contract.
 * Allows to enable/disable reserves,
 * and set different protocol parameters.
 **/

contract LendingPoolConfigurator is VersionedInitializable {
    using SafeMath for uint256;

    /**
     * @dev emitted when a reserve is initialized.
     * @param _reserve the address of the reserve
     * @param _mToken the address of the overlying mToken contract
     * @param _interestRateStrategyAddress the address of the interest rate
     * strategy for the reserve
     **/
    event ReserveInitialized(
        address indexed _reserve,
        address indexed _mToken,
        address _interestRateStrategyAddress
    );

    /**
     * @dev emitted when a reserve is removed.
     * @param _reserve the address of the reserve
     **/
    event ReserveRemoved(address indexed _reserve);

    /**
     * @dev emitted when borrowing is enabled on a reserve
     * @param _reserve the address of the reserve
     * @param _stableRateEnabled true if stable rate borrowing is enabled, false
     * otherwise
     **/
    event BorrowingEnabledOnReserve(address _reserve, bool _stableRateEnabled);

    /**
     * @dev emitted when borrowing is disabled on a reserve
     * @param _reserve the address of the reserve
     **/
    event BorrowingDisabledOnReserve(address indexed _reserve);

    /**
     * @dev emitted when a reserve is enabled as collateral.
     * @param _reserve the address of the reserve
     * @param _ltv the loan to value of the asset when used as collateral
     * @param _liquidationThreshold the threshold at which loans using this
     * asset as collateral will be considered undercollateralized
     * @param _liquidationBonus the bonus liquidators receive to liquidate this
     * asset
     **/
    event ReserveEnabledAsCollateral(
        address indexed _reserve,
        uint256 _ltv,
        uint256 _liquidationThreshold,
        uint256 _liquidationBonus
    );

    /**
     * @dev emitted when a reserve is disabled as collateral
     * @param _reserve the address of the reserve
     **/
    event ReserveDisabledAsCollateral(address indexed _reserve);

    /**
     * @dev emitted when stable rate borrowing is enabled on a reserve
     * @param _reserve the address of the reserve
     **/
    event StableRateEnabledOnReserve(address indexed _reserve);

    /**
     * @dev emitted when stable rate borrowing is disabled on a reserve
     * @param _reserve the address of the reserve
     **/
    event StableRateDisabledOnReserve(address indexed _reserve);

    /**
     * @dev emitted when a reserve is activated
     * @param _reserve the address of the reserve
     **/
    event ReserveActivated(address indexed _reserve);

    /**
     * @dev emitted when a reserve is deactivated
     * @param _reserve the address of the reserve
     **/
    event ReserveDeactivated(address indexed _reserve);

    /**
     * @dev emitted when a reserve is freezed
     * @param _reserve the address of the reserve
     **/
    event ReserveFreezed(address indexed _reserve);

    /**
     * @dev emitted when a reserve is unfreezed
     * @param _reserve the address of the reserve
     **/
    event ReserveUnfreezed(address indexed _reserve);

    /**
     * @dev emitted when a reserve loan to value is updated
     * @param _reserve the address of the reserve
     * @param _ltv the new value for the loan to value
     **/
    event ReserveBaseLtvChanged(address _reserve, uint256 _ltv);

    /**
     * @dev emitted when a reserve liquidation threshold is updated
     * @param _reserve the address of the reserve
     * @param _threshold the new value for the liquidation threshold
     **/
    event ReserveLiquidationThresholdChanged(
        address _reserve,
        uint256 _threshold
    );

    /**
     * @dev emitted when a reserve liquidation bonus is updated
     * @param _reserve the address of the reserve
     * @param _bonus the new value for the liquidation bonus
     **/
    event ReserveLiquidationBonusChanged(address _reserve, uint256 _bonus);

    /**
     * @dev emitted when the reserve decimals are updated
     * @param _reserve the address of the reserve
     * @param _decimals the new decimals
     **/
    event ReserveDecimalsChanged(address _reserve, uint256 _decimals);

    /**
     * @dev emitted when a reserve interest strategy contract is updated
     * @param _reserve the address of the reserve
     * @param _strategy the new address of the interest strategy contract
     **/
    event ReserveInterestRateStrategyChanged(
        address _reserve,
        address _strategy
    );

    LendingPoolAddressesProvider public poolAddressesProvider;
    /**
     * @dev only the lending pool manager can call functions affected by this
     * modifier
     **/
    modifier onlyLendingPoolManager {
        require(
            poolAddressesProvider.getLendingPoolManager() == msg.sender,
            "The caller must be a lending pool manager"
        );
        _;
    }

    uint256 public constant CONFIGURATOR_REVISION = 0x1;

    function getRevision() internal pure returns (uint256) {
        return CONFIGURATOR_REVISION;
    }

    function initialize(LendingPoolAddressesProvider _poolAddressesProvider)
        public
        initializer
    {
        poolAddressesProvider = _poolAddressesProvider;
    }

    /**
     * @dev initializes a reserve
     * @param _reserve the address of the reserve to be initialized
     * @param _underlyingAssetDecimals the decimals of the reserve underlying
     * asset
     * @param _interestRateStrategyAddress the address of the interest rate
     * strategy contract for this reserve
     **/
    function initReserve(
        address _reserve,
        uint8 _underlyingAssetDecimals,
        address _interestRateStrategyAddress
    ) external onlyLendingPoolManager {
        ERC20Detailed asset = ERC20Detailed(_reserve);

        string memory mTokenName =
            string(
                abi.encodePacked("Multiplier Interest bearing ", asset.name())
            );
        string memory mTokenSymbol =
            string(abi.encodePacked("mx", asset.symbol()));

        initReserveWithData(
            _reserve,
            mTokenName,
            mTokenSymbol,
            _underlyingAssetDecimals,
            _interestRateStrategyAddress
        );
    }

    /**
     * @dev initializes a reserve using mTokenData provided externally
     * (useful if the underlying ERC20 contract doesn't expose name or decimals)
     * @param _reserve the address of the reserve to be initialized
     * @param _mTokenName the name of the mToken contract
     * @param _mTokenSymbol the symbol of the mToken contract
     * @param _underlyingAssetDecimals the decimals of the reserve underlying
     * asset
     * @param _interestRateStrategyAddress the address of the interest rate
     * strategy contract for this reserve
     **/
    function initReserveWithData(
        address _reserve,
        string memory _mTokenName,
        string memory _mTokenSymbol,
        uint8 _underlyingAssetDecimals,
        address _interestRateStrategyAddress
    ) public onlyLendingPoolManager {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());

        mToken mTokenInstance =
            new mToken(
                poolAddressesProvider,
                _reserve,
                _underlyingAssetDecimals,
                _mTokenName,
                _mTokenSymbol
            );
        core.initReserve(
            _reserve,
            address(mTokenInstance),
            _underlyingAssetDecimals,
            _interestRateStrategyAddress
        );

        emit ReserveInitialized(
            _reserve,
            address(mTokenInstance),
            _interestRateStrategyAddress
        );
    }

    /**
     * @dev removes the last added reserve in the list of the reserves
     * @param _reserveToRemove the address of the reserve
     **/
    function removeLastAddedReserve(address _reserveToRemove)
        external
        onlyLendingPoolManager
    {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        core.removeLastAddedReserve(_reserveToRemove);
        emit ReserveRemoved(_reserveToRemove);
    }

    /**
     * @dev enables borrowing on a reserve
     * @param _reserve the address of the reserve
     * @param _stableBorrowRateEnabled true if stable borrow rate needs to be
     * enabled by default on this reserve
     **/
    function enableBorrowingOnReserve(
        address _reserve,
        bool _stableBorrowRateEnabled
    ) external onlyLendingPoolManager {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        core.enableBorrowingOnReserve(_reserve, _stableBorrowRateEnabled);
        emit BorrowingEnabledOnReserve(_reserve, _stableBorrowRateEnabled);
    }

    /**
     * @dev disables borrowing on a reserve
     * @param _reserve the address of the reserve
     **/
    function disableBorrowingOnReserve(address _reserve)
        external
        onlyLendingPoolManager
    {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        core.disableBorrowingOnReserve(_reserve);

        emit BorrowingDisabledOnReserve(_reserve);
    }

    /**
     * @dev enables a reserve to be used as collateral
     * @param _reserve the address of the reserve
     * @param _baseLTVasCollateral the loan to value of the asset when used as
     * collateral
     * @param _liquidationThreshold the threshold at which loans using this
     * asset as collateral will be considered undercollateralized
     * @param _liquidationBonus the bonus liquidators receive to liquidate this
     * asset
     **/
    function enableReserveAsCollateral(
        address _reserve,
        uint256 _baseLTVasCollateral,
        uint256 _liquidationThreshold,
        uint256 _liquidationBonus
    ) external onlyLendingPoolManager {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        core.enableReserveAsCollateral(
            _reserve,
            _baseLTVasCollateral,
            _liquidationThreshold,
            _liquidationBonus
        );
        emit ReserveEnabledAsCollateral(
            _reserve,
            _baseLTVasCollateral,
            _liquidationThreshold,
            _liquidationBonus
        );
    }

    /**
     * @dev disables a reserve as collateral
     * @param _reserve the address of the reserve
     **/
    function disableReserveAsCollateral(address _reserve)
        external
        onlyLendingPoolManager
    {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        core.disableReserveAsCollateral(_reserve);

        emit ReserveDisabledAsCollateral(_reserve);
    }

    /**
     * @dev enable stable rate borrowing on a reserve
     * @param _reserve the address of the reserve
     **/
    function enableReserveStableBorrowRate(address _reserve)
        external
        onlyLendingPoolManager
    {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        core.enableReserveStableBorrowRate(_reserve);

        emit StableRateEnabledOnReserve(_reserve);
    }

    /**
     * @dev disable stable rate borrowing on a reserve
     * @param _reserve the address of the reserve
     **/
    function disableReserveStableBorrowRate(address _reserve)
        external
        onlyLendingPoolManager
    {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        core.disableReserveStableBorrowRate(_reserve);

        emit StableRateDisabledOnReserve(_reserve);
    }

    /**
     * @dev activates a reserve
     * @param _reserve the address of the reserve
     **/
    function activateReserve(address _reserve) external onlyLendingPoolManager {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        core.activateReserve(_reserve);

        emit ReserveActivated(_reserve);
    }

    /**
     * @dev deactivates a reserve
     * @param _reserve the address of the reserve
     **/
    function deactivateReserve(address _reserve)
        external
        onlyLendingPoolManager
    {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        require(
            core.getReserveTotalLiquidity(_reserve) == 0,
            "The liquidity of the reserve needs to be 0"
        );
        core.deactivateReserve(_reserve);

        emit ReserveDeactivated(_reserve);
    }

    /**
     * @dev freezes a reserve. A freezed reserve doesn't accept any new deposit,
     * borrow or rate swap, but can accept repayments, liquidations, rate
     * rebalances and redeems
     * @param _reserve the address of the reserve
     **/
    function freezeReserve(address _reserve) external onlyLendingPoolManager {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        core.freezeReserve(_reserve);

        emit ReserveFreezed(_reserve);
    }

    /**
     * @dev unfreezes a reserve
     * @param _reserve the address of the reserve
     **/
    function unfreezeReserve(address _reserve) external onlyLendingPoolManager {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        core.unfreezeReserve(_reserve);

        emit ReserveUnfreezed(_reserve);
    }

    /**
     * @dev emitted when a reserve loan to value is updated
     * @param _reserve the address of the reserve
     * @param _ltv the new value for the loan to value
     **/
    function setReserveBaseLTVasCollateral(address _reserve, uint256 _ltv)
        external
        onlyLendingPoolManager
    {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        core.setReserveBaseLTVasCollateral(_reserve, _ltv);
        emit ReserveBaseLtvChanged(_reserve, _ltv);
    }

    /**
     * @dev updates the liquidation threshold of a reserve.
     * @param _reserve the address of the reserve
     * @param _threshold the new value for the liquidation threshold
     **/
    function setReserveLiquidationThreshold(
        address _reserve,
        uint256 _threshold
    ) external onlyLendingPoolManager {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        core.setReserveLiquidationThreshold(_reserve, _threshold);
        emit ReserveLiquidationThresholdChanged(_reserve, _threshold);
    }

    /**
     * @dev updates the liquidation bonus of a reserve
     * @param _reserve the address of the reserve
     * @param _bonus the new value for the liquidation bonus
     **/
    function setReserveLiquidationBonus(address _reserve, uint256 _bonus)
        external
        onlyLendingPoolManager
    {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        core.setReserveLiquidationBonus(_reserve, _bonus);
        emit ReserveLiquidationBonusChanged(_reserve, _bonus);
    }

    /**
     * @dev updates the reserve decimals
     * @param _reserve the address of the reserve
     * @param _decimals the new number of decimals
     **/
    function setReserveDecimals(address _reserve, uint256 _decimals)
        external
        onlyLendingPoolManager
    {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        core.setReserveDecimals(_reserve, _decimals);
        emit ReserveDecimalsChanged(_reserve, _decimals);
    }

    /**
     * @dev sets the interest rate strategy of a reserve
     * @param _reserve the address of the reserve
     * @param _rateStrategyAddress the new address of the interest strategy
     * contract
     **/
    function setReserveInterestRateStrategyAddress(
        address _reserve,
        address _rateStrategyAddress
    ) external onlyLendingPoolManager {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        core.setReserveInterestRateStrategyAddress(
            _reserve,
            _rateStrategyAddress
        );
        emit ReserveInterestRateStrategyChanged(_reserve, _rateStrategyAddress);
    }

    /**
     * @dev refreshes the lending pool core configuration to update the cached
     * address
     **/
    function refreshLendingPoolCoreConfiguration()
        external
        onlyLendingPoolManager
    {
        LendingPoolCore core =
            LendingPoolCore(poolAddressesProvider.getLendingPoolCore());
        core.refreshConfiguration();
    }
}



/**
 * @title LendingPool contract
 * @notice Implements the actions of the LendingPool, and exposes accessory
 * methods to fetch the users and reserve data
 * @author Multiplier Finance
 **/

contract LendingPool is ReentrancyGuard, VersionedInitializable {
    using SafeMath for uint256;
    using WadRayMath for uint256;
    using Address for address;

    LendingPoolAddressesProvider public addressesProvider;
    LendingPoolCore public core;
    LendingPoolDataProvider public dataProvider;
    LendingPoolParametersProvider public parametersProvider;
    IFeeProvider public feeProvider;
    RewardsManager public rewardsMgr;

    /**
     * @dev emitted on deposit
     * @param _reserve the address of the reserve
     * @param _user the address of the user
     * @param _amount the amount to be deposited
     * @param _referral the referral number of the action
     * @param _timestamp the timestamp of the action
     **/
    event Deposit(
        address indexed _reserve,
        address indexed _user,
        uint256 _amount,
        uint16 indexed _referral,
        uint256 _timestamp
    );

    /**
     * @dev emitted during a redeem action.
     * @param _reserve the address of the reserve
     * @param _user the address of the user
     * @param _amount the amount to be deposited
     * @param _timestamp the timestamp of the action
     **/
    event RedeemUnderlying(
        address indexed _reserve,
        address indexed _user,
        uint256 _amount,
        uint256 _timestamp
    );

    /**
     * @dev emitted on borrow
     * @param _reserve the address of the reserve
     * @param _user the address of the user
     * @param _amount the amount to be deposited
     * @param _borrowRateMode the rate mode, can be either 1-stable or 2-variable
     * @param _borrowRate the rate at which the user has borrowed
     * @param _originationFee the origination fee to be paid by the user
     * @param _borrowBalanceIncrease the balance increase since the last borrow,
     * 0 if it's the first time borrowing
     * @param _referral the referral number of the action
     * @param _timestamp the timestamp of the action
     **/
    event Borrow(
        address indexed _reserve,
        address indexed _user,
        uint256 _amount,
        uint256 _borrowRateMode,
        uint256 _borrowRate,
        uint256 _originationFee,
        uint256 _borrowBalanceIncrease,
        uint16 indexed _referral,
        uint256 _timestamp
    );

    /**
     * @dev emitted on repay
     * @param _reserve the address of the reserve
     * @param _user the address of the user for which the repay has been executed
     * @param _repayer the address of the user that has performed the repay
     * action
     * @param _amountMinusFees the amount repaid minus fees
     * @param _fees the fees repaid
     * @param _borrowBalanceIncrease the balance increase since the last action
     * @param _timestamp the timestamp of the action
     **/
    event Repay(
        address indexed _reserve,
        address indexed _user,
        address indexed _repayer,
        uint256 _amountMinusFees,
        uint256 _fees,
        uint256 _borrowBalanceIncrease,
        uint256 _timestamp
    );

    /**
     * @dev emitted when a user performs a rate swap
     * @param _reserve the address of the reserve
     * @param _user the address of the user executing the swap
     * @param _newRateMode the new interest rate mode
     * @param _newRate the new borrow rate
     * @param _borrowBalanceIncrease the balance increase since the last action
     * @param _timestamp the timestamp of the action
     **/
    event Swap(
        address indexed _reserve,
        address indexed _user,
        uint256 _newRateMode,
        uint256 _newRate,
        uint256 _borrowBalanceIncrease,
        uint256 _timestamp
    );

    /**
     * @dev emitted when a user enables a reserve as collateral
     * @param _reserve the address of the reserve
     * @param _user the address of the user
     **/
    event ReserveUsedAsCollateralEnabled(
        address indexed _reserve,
        address indexed _user
    );

    /**
     * @dev emitted when a user disables a reserve as collateral
     * @param _reserve the address of the reserve
     * @param _user the address of the user
     **/
    event ReserveUsedAsCollateralDisabled(
        address indexed _reserve,
        address indexed _user
    );

    /**
     * @dev emitted when the stable rate of a user gets rebalanced
     * @param _reserve the address of the reserve
     * @param _user the address of the user for which the rebalance has been
     * executed
     * @param _newStableRate the new stable borrow rate after the rebalance
     * @param _borrowBalanceIncrease the balance increase since the last action
     * @param _timestamp the timestamp of the action
     **/
    event RebalanceStableBorrowRate(
        address indexed _reserve,
        address indexed _user,
        uint256 _newStableRate,
        uint256 _borrowBalanceIncrease,
        uint256 _timestamp
    );

    /**
     * @dev emitted when a flashloan is executed
     * @param _target the address of the flashLoanReceiver
     * @param _reserve the address of the reserve
     * @param _amount the amount requested
     * @param _timestamp the timestamp of the action
     **/
    event FlashLoan(
        address indexed _target,
        address indexed _reserve,
        uint256 _amount,
        uint256 _timestamp
    );

    /**
     * @dev these events are not emitted directly by the LendingPool
     * but they are declared here as the LendingPoolLiquidationManager
     * is executed using a delegateCall().
     * This allows to have the events in the generated ABI for LendingPool.
     **/

    /**
     * @dev emitted when a borrow fee is liquidated
     * @param _collateral the address of the collateral being liquidated
     * @param _reserve the address of the reserve
     * @param _user the address of the user being liquidated
     * @param _feeLiquidated the total fee liquidated
     * @param _liquidatedCollateralForFee the amount of collateral received by
     * the protocol in exchange for the fee
     * @param _timestamp the timestamp of the action
     **/
    event OriginationFeeLiquidated(
        address indexed _collateral,
        address indexed _reserve,
        address indexed _user,
        uint256 _feeLiquidated,
        uint256 _liquidatedCollateralForFee,
        uint256 _timestamp
    );

    /**
     * @dev emitted when a borrower is liquidated
     * @param _collateral the address of the collateral being liquidated
     * @param _reserve the address of the reserve
     * @param _user the address of the user being liquidated
     * @param _purchaseAmount the total amount liquidated
     * @param _liquidatedCollateralAmount the amount of collateral being
     * liquidated
     * @param _accruedBorrowInterest the amount of interest accrued by the
     * borrower since the last action
     * @param _liquidator the address of the liquidator
     * @param _receiveMToken true if the liquidator wants to receive
     * mTokens, false otherwise
     * @param _timestamp the timestamp of the action
     **/
    event LiquidationCall(
        address indexed _collateral,
        address indexed _reserve,
        address indexed _user,
        uint256 _purchaseAmount,
        uint256 _liquidatedCollateralAmount,
        uint256 _accruedBorrowInterest,
        address _liquidator,
        bool _receiveMToken,
        uint256 _timestamp
    );

    /**
     * @dev emitted when user claim reward.
     * @param _reserve the address of the reserve
     * @param _user the address of the user for which the reward was claimed to
     * @param _amount the amount of reward
     * @param _timestamp the timestamp of the action
     **/
    event ClaimReward(
        address indexed _reserve,
        address indexed _user,
        uint256 _amount,
        uint256 _timestamp
    );

    /**
     * @dev functions affected by this modifier can only be invoked by the
     * mToken.sol contract
     * @param _reserve the address of the reserve
     **/
    modifier onlyOverlyingmToken(address _reserve) {
        require(
            msg.sender == core.getReservemTokenAddress(_reserve),
            "The caller of this function can only be the mToken contract of this reserve"
        );
        _;
    }

    /**
     * @dev functions affected by this modifier can only be invoked if the
     * reserve is active
     * @param _reserve the address of the reserve
     **/
    modifier onlyActiveReserve(address _reserve) {
        requireReserveActiveInternal(_reserve);
        _;
    }

    /**
     * @dev functions affected by this modifier can only be invoked if the
     * reserve is not freezed.
     * A freezed reserve only allows redeems, repays, rebalances and
     * liquidations.
     * @param _reserve the address of the reserve
     **/
    modifier onlyUnfreezedReserve(address _reserve) {
        requireReserveNotFreezedInternal(_reserve);
        _;
    }

    /**
     * @dev functions affected by this modifier can only be invoked if the
     * provided _amount input parameter
     * is not zero.
     * @param _amount the amount provided
     **/
    modifier onlyAmountGreaterThanZero(uint256 _amount) {
        requireAmountGreaterThanZeroInternal(_amount);
        _;
    }

    uint256 public constant UINT_MAX_VALUE = uint256(-1);

    uint256 public constant LENDINGPOOL_REVISION = 0x1;

    function getRevision() internal pure returns (uint256) {
        return LENDINGPOOL_REVISION;
    }

    /**
     * @dev this function is invoked by the proxy contract when the LendingPool
     * contract is added to the
     * AddressesProvider.
     * @param _addressesProvider the address of the LendingPoolAddressesProvider
     * registry
     **/
    function initialize(LendingPoolAddressesProvider _addressesProvider)
        public
        initializer
    {
        addressesProvider = _addressesProvider;
        core = LendingPoolCore(addressesProvider.getLendingPoolCore());
        dataProvider = LendingPoolDataProvider(
            addressesProvider.getLendingPoolDataProvider()
        );
        parametersProvider = LendingPoolParametersProvider(
            addressesProvider.getLendingPoolParametersProvider()
        );
        feeProvider = IFeeProvider(addressesProvider.getFeeProvider());
    }

    /**
     * @dev deposits The underlying asset into the reserve. A corresponding
     * amount of the overlying asset (mTokens)
     * is minted.
     * @param _reserve the address of the reserve
     * @param _amount the amount to be deposited
     * @param _referralCode integrators are assigned a referral code and can
     * potentially receive rewards.
     **/
    function deposit(
        address _reserve,
        uint256 _amount,
        uint16 _referralCode
    )
        external
        payable
        nonReentrant
        onlyActiveReserve(_reserve)
        onlyUnfreezedReserve(_reserve)
        onlyAmountGreaterThanZero(_amount)
    {
        uint256 curBalance =
            core.getUserUnderlyingAssetBalance(_reserve, msg.sender);
        bool isFirstDeposit = (curBalance == 0);

        core.updateStateOnDeposit(
            _reserve,
            msg.sender,
            _amount,
            isFirstDeposit
        );

        //minting mToken to user 1:1 with the specific exchange rate
        mToken token = mToken(core.getReservemTokenAddress(_reserve));
        token.mintOnDeposit(msg.sender, _amount);

        //transfer to the core contract
        core.transferToReserve.value(msg.value)(_reserve, msg.sender, _amount);

        //solium-disable-next-line
        emit Deposit(
            _reserve,
            msg.sender,
            _amount,
            _referralCode,
            block.timestamp
        );
    }

    /**
     * @dev Redeems the underlying amount of assets requested by _user.
     * This function is executed by the overlying mToken contract in response
     * to a redeem action.
     * @param _reserve the address of the reserve
     * @param _user the address of the user performing the action
     * @param _amount the underlying amount to be redeemed
     **/
    function redeemUnderlying(
        address _reserve,
        address payable _user,
        uint256 _amount,
        uint256 _mTokenBalanceAfterRedeem
    )
        external
        nonReentrant
        onlyOverlyingmToken(_reserve)
        onlyActiveReserve(_reserve)
        onlyAmountGreaterThanZero(_amount)
    {
        uint256 currentAvailableLiquidity =
            core.getReserveAvailableLiquidity(_reserve);
        require(
            currentAvailableLiquidity >= _amount,
            "There is not enough liquidity available to redeem"
        );

        core.updateStateOnRedeem(
            _reserve,
            _user,
            _amount,
            _mTokenBalanceAfterRedeem == 0
        );

        core.transferToUser(_reserve, _user, _amount);

        //solium-disable-next-line
        emit RedeemUnderlying(_reserve, _user, _amount, block.timestamp);
    }

    /**
     * @dev data structures for local computations in the borrow() method.
     */

    struct BorrowLocalVars {
        uint256 principalBorrowBalance;
        uint256 currentLtv;
        uint256 currentLiquidationThreshold;
        uint256 borrowFee;
        uint256 requestedBorrowAmountBNB;
        uint256 amountOfCollateralNeededBNB;
        uint256 userCollateralBalanceBNB;
        uint256 userBorrowBalanceBNB;
        uint256 userTotalFeesBNB;
        uint256 borrowBalanceIncrease;
        uint256 currentReserveStableRate;
        uint256 availableLiquidity;
        uint256 reserveDecimals;
        uint256 finalUserBorrowRate;
        CoreLibrary.InterestRateMode rateMode;
        bool healthFactorBelowThreshold;
    }

    /**
     * @dev Allows users to borrow a specific amount of the reserve currency,
     * provided that the borrower
     * already deposited enough collateral.
     * @param _reserve the address of the reserve
     * @param _amount the amount to be borrowed
     * @param _interestRateMode the interest rate mode at which the user wants
     * to borrow. Can be 0 (STABLE) or 1 (VARIABLE)
     **/
    function borrow(
        address _reserve,
        uint256 _amount,
        uint256 _interestRateMode,
        uint16 _referralCode
    )
        external
        nonReentrant
        onlyActiveReserve(_reserve)
        onlyUnfreezedReserve(_reserve)
        onlyAmountGreaterThanZero(_amount)
    {
        // Usage of a memory struct of vars to avoid "Stack too deep" errors
        // due to local variables
        BorrowLocalVars memory vars;

        //check that the reserve is enabled for borrowing
        require(
            core.isReserveBorrowingEnabled(_reserve),
            "Reserve is not enabled for borrowing"
        );
        //validate interest rate mode
        require(
            uint256(CoreLibrary.InterestRateMode.VARIABLE) ==
                _interestRateMode ||
                uint256(CoreLibrary.InterestRateMode.STABLE) ==
                _interestRateMode,
            "Invalid interest rate mode selected"
        );

        //cast the rateMode to coreLibrary.interestRateMode
        vars.rateMode = CoreLibrary.InterestRateMode(_interestRateMode);

        //check that the amount is available in the reserve
        vars.availableLiquidity = core.getReserveAvailableLiquidity(_reserve);

        // Only allow the borrowing up to 99% of the availability liquidity
        require(
            !CoreLibrary.exceed99Percent(_amount, vars.availableLiquidity),
            "There is not enough liquidity available in the reserve"
        );

        (
            ,
            vars.userCollateralBalanceBNB,
            vars.userBorrowBalanceBNB,
            vars.userTotalFeesBNB,
            vars.currentLtv,
            vars.currentLiquidationThreshold,
            ,
            vars.healthFactorBelowThreshold
        ) = dataProvider.calculateUserGlobalData(msg.sender);

        require(
            vars.userCollateralBalanceBNB > 0,
            "The collateral balance is 0"
        );

        require(
            !vars.healthFactorBelowThreshold,
            "The borrower can already be liquidated so he cannot borrow more"
        );

        //calculating fees
        vars.borrowFee = feeProvider.calculateLoanOriginationFee(_amount);

        require(vars.borrowFee > 0, "The amount to borrow is too small");

        vars.amountOfCollateralNeededBNB = dataProvider
            .calculateCollateralNeededInBNB(
            _reserve,
            _amount,
            vars.borrowFee,
            vars.userBorrowBalanceBNB,
            vars.userTotalFeesBNB,
            vars.currentLtv
        );

        require(
            vars.amountOfCollateralNeededBNB <= vars.userCollateralBalanceBNB,
            "There is not enough collateral to cover a new borrow"
        );

        /**
         * Following conditions need to be met if the user is borrowing at a
         * stable rate:
         * 1. Reserve must be enabled for stable rate borrowing
         * 2. Users cannot borrow from the reserve if their collateral is
         * (mostly) the same currency
         *    they are borrowing, to prevent abuses.
         * 3. Users will be able to borrow only a relatively small, configurable
         * amount of the total
         *    liquidity
         **/

        if (vars.rateMode == CoreLibrary.InterestRateMode.STABLE) {
            //check if the borrow mode is stable and if stable rate borrowing is enabled on this reserve
            require(
                core.isUserAllowedToBorrowAtStable(
                    _reserve,
                    msg.sender,
                    _amount
                ),
                "User cannot borrow the selected amount with a stable rate"
            );

            //calculate the max available loan size in stable rate mode as a percentage of the
            //available liquidity
            uint256 maxLoanPercent =
                parametersProvider.getMaxStableRateBorrowSizePercent();
            uint256 maxLoanSizeStable =
                vars.availableLiquidity.mul(maxLoanPercent).div(100);

            require(
                _amount <= maxLoanSizeStable,
                "User is trying to borrow too much liquidity at a stable rate"
            );
        }

        //all conditions passed - borrow is accepted
        (vars.finalUserBorrowRate, vars.borrowBalanceIncrease) = core
            .updateStateOnBorrow(
            _reserve,
            msg.sender,
            _amount,
            vars.borrowFee,
            vars.rateMode
        );

        //if we reached this point, we can transfer
        core.transferToUser(_reserve, msg.sender, _amount);

        emit Borrow(
            _reserve,
            msg.sender,
            _amount,
            _interestRateMode,
            vars.finalUserBorrowRate,
            vars.borrowFee,
            vars.borrowBalanceIncrease,
            _referralCode,
            //solium-disable-next-line
            block.timestamp
        );
    }

    /**
     * @notice repays a borrow on the specific reserve, for the specified amount
     * (or for the whole amount, if uint256(-1) is specified).
     * @dev the target user is defined by _onBehalfOf. If there is no repayment
     * on behalf of another account,
     * _onBehalfOf must be equal to msg.sender.
     * @param _reserve the address of the reserve on which the user borrowed
     * @param _amount the amount to repay, or uint256(-1) if the user wants to
     * repay everything
     * @param _onBehalfOf the address for which msg.sender is repaying.
     **/

    struct RepayLocalVars {
        uint256 principalBorrowBalance;
        uint256 compoundedBorrowBalance;
        uint256 borrowBalanceIncrease;
        bool isBnb;
        uint256 paybackAmount;
        uint256 paybackAmountMinusFees;
        uint256 currentStableRate;
        uint256 originationFee;
        uint256 totalLiquidity;
    }

    struct RewardLocalVars {
        uint256 suppliers;
        uint256 governace;
    }

    function repay(
        address _reserve,
        uint256 _amount,
        address payable _onBehalfOf
    )
        external
        payable
        nonReentrant
        onlyActiveReserve(_reserve)
        onlyAmountGreaterThanZero(_amount)
    {
        // Usage of a memory struct of vars to avoid "Stack too deep" errors
        // due to local variables
        RepayLocalVars memory vars;

        (
            vars.principalBorrowBalance,
            vars.compoundedBorrowBalance,
            vars.borrowBalanceIncrease
        ) = core.getUserBorrowBalances(_reserve, _onBehalfOf);

        vars.originationFee = core.getUserOriginationFee(_reserve, _onBehalfOf);
        vars.isBnb = BscAddressLib.bnbAddress() == _reserve;

        require(
            vars.compoundedBorrowBalance > 0,
            "The user does not have any borrow pending"
        );

        require(
            _amount != UINT_MAX_VALUE || msg.sender == _onBehalfOf,
            "To repay on behalf of an user an explicit amount to repay is needed."
        );

        //default to max amount
        vars.paybackAmount = vars.compoundedBorrowBalance.add(
            vars.originationFee
        );

        if (_amount != UINT_MAX_VALUE && _amount < vars.paybackAmount) {
            vars.paybackAmount = _amount;
        }

        require(
            !vars.isBnb || msg.value >= vars.paybackAmount,
            "Invalid msg.value sent for the repayment"
        );

        // RewardManager : add reward item based on the fee paid back //
        //vars.totalLiquidity = core.getReserveTotalLiquidity(_reserve);
        uint256 feeReward = vars.originationFee;
        if (vars.paybackAmount <= vars.originationFee) {
            feeReward = vars.paybackAmount;
        }

        // Update and accumulate reward for this address.
        updateRewards(_reserve, _onBehalfOf);
        // RewardManager: Add new reward item. //
        addRewardItem(_reserve, feeReward);

        // if the amount is smaller than the origination fee, just transfer the
        // amount to the fee destination address
        if (vars.paybackAmount <= vars.originationFee) {
            core.updateStateOnRepay(
                _reserve,
                _onBehalfOf,
                0,
                vars.paybackAmount,
                vars.borrowBalanceIncrease,
                false
            );

            core.transferToFeeCollectionAddress.value(
                vars.isBnb ? vars.paybackAmount : 0
            )(_reserve, _onBehalfOf, vars.paybackAmount, false);

            emit Repay(
                _reserve,
                _onBehalfOf,
                msg.sender,
                0,
                vars.paybackAmount,
                vars.borrowBalanceIncrease,
                //solium-disable-next-line
                block.timestamp
            );
            return;
        }

        vars.paybackAmountMinusFees = vars.paybackAmount.sub(
            vars.originationFee
        );

        core.updateStateOnRepay(
            _reserve,
            _onBehalfOf,
            vars.paybackAmountMinusFees,
            vars.originationFee,
            vars.borrowBalanceIncrease,
            vars.compoundedBorrowBalance == vars.paybackAmountMinusFees
        );

        // if the user didn't repay the origination fee, transfer the fee to the
        // fee collection address
        if (vars.originationFee > 0) {
            core.transferToFeeCollectionAddress.value(
                vars.isBnb ? vars.originationFee : 0
            )(_reserve, msg.sender, vars.originationFee, false);
        }

        //sending the total msg.value if the transfer is BNB.
        //the transferToReserve() function will take care of sending the
        //excess BNB back to the caller
        core.transferToReserve.value(
            vars.isBnb ? msg.value.sub(vars.originationFee) : 0
        )(_reserve, msg.sender, vars.paybackAmountMinusFees);

        emit Repay(
            _reserve,
            _onBehalfOf,
            msg.sender,
            vars.paybackAmountMinusFees,
            vars.originationFee,
            vars.borrowBalanceIncrease,
            //solium-disable-next-line
            block.timestamp
        );
    }

    /**
     * @dev borrowers can user this function to swap between stable and variable
     * borrow rate modes.
     * @param _reserve the address of the reserve on which the user borrowed
     **/
    function swapBorrowRateMode(address _reserve)
        external
        nonReentrant
        onlyActiveReserve(_reserve)
        onlyUnfreezedReserve(_reserve)
    {
        (
            uint256 principalBorrowBalance,
            uint256 compoundedBorrowBalance,
            uint256 borrowBalanceIncrease
        ) = core.getUserBorrowBalances(_reserve, msg.sender);

        require(
            compoundedBorrowBalance > 0,
            "User does not have a borrow in progress on this reserve"
        );

        CoreLibrary.InterestRateMode currentRateMode =
            core.getUserCurrentBorrowRateMode(_reserve, msg.sender);

        if (currentRateMode == CoreLibrary.InterestRateMode.VARIABLE) {
            /**
             * user wants to swap to stable, before swapping we need to ensure
             * that
             * 1. stable borrow rate is enabled on the reserve
             * 2. user is not trying to abuse the reserve by depositing
             * more collateral than he is borrowing, artificially lowering
             * the interest rate, borrowing at variable, and switching to stable
             **/
            require(
                core.isUserAllowedToBorrowAtStable(
                    _reserve,
                    msg.sender,
                    compoundedBorrowBalance
                ),
                "User cannot borrow the selected amount at stable"
            );
        }

        (CoreLibrary.InterestRateMode newRateMode, uint256 newBorrowRate) =
            core.updateStateOnSwapRate(
                _reserve,
                msg.sender,
                principalBorrowBalance,
                compoundedBorrowBalance,
                borrowBalanceIncrease,
                currentRateMode
            );

        emit Swap(
            _reserve,
            msg.sender,
            uint256(newRateMode),
            newBorrowRate,
            borrowBalanceIncrease,
            //solium-disable-next-line
            block.timestamp
        );
    }

    /**
     * @dev rebalances the stable interest rate of a user if current liquidity
     * rate > user stable rate.
     * this is regulated by Multiplier to ensure that the protocol is not
     * abused, and the user is paying a fair
     * rate. Anyone can call this function though.
     * @param _reserve the address of the reserve
     * @param _user the address of the user to be rebalanced
     **/
    function rebalanceStableBorrowRate(address _reserve, address _user)
        external
        nonReentrant
        onlyActiveReserve(_reserve)
    {
        (, uint256 compoundedBalance, uint256 borrowBalanceIncrease) =
            core.getUserBorrowBalances(_reserve, _user);

        //step 1: user must be borrowing on _reserve at a stable rate
        require(
            compoundedBalance > 0,
            "User does not have any borrow for this reserve"
        );

        require(
            core.getUserCurrentBorrowRateMode(_reserve, _user) ==
                CoreLibrary.InterestRateMode.STABLE,
            "The user borrow is variable and cannot be rebalanced"
        );

        uint256 userCurrentStableRate =
            core.getUserCurrentStableBorrowRate(_reserve, _user);
        uint256 liquidityRate = core.getReserveCurrentLiquidityRate(_reserve);
        uint256 reserveCurrentStableRate =
            core.getReserveCurrentStableBorrowRate(_reserve);
        uint256 rebalanceDownRateThreshold =
            reserveCurrentStableRate.rayMul(
                WadRayMath.ray().add(
                    parametersProvider.getRebalanceDownRateDelta()
                )
            );

        // Step 2: we have two possible situations to rebalance:

        // 1. user stable borrow rate is below the current liquidity rate. The
        // loan needs to be rebalanced,
        // as this situation can be abused (user putting back the borrowed
        // liquidity in the same reserve to earn on it)
        // 2. user stable rate is above the market avg borrow rate of a certain
        // delta, and utilization rate is low.
        // In this case, the user is paying an interest that is too high, and
        // needs to be rescaled down.
        if (
            userCurrentStableRate < liquidityRate ||
            userCurrentStableRate > rebalanceDownRateThreshold
        ) {
            uint256 newStableRate =
                core.updateStateOnRebalance(
                    _reserve,
                    _user,
                    borrowBalanceIncrease
                );

            emit RebalanceStableBorrowRate(
                _reserve,
                _user,
                newStableRate,
                borrowBalanceIncrease,
                //solium-disable-next-line
                block.timestamp
            );

            return;
        }

        revert("Interest rate rebalance conditions were not met");
    }

    /**
     * @dev allows depositors to enable or disable a specific deposit as
     * collateral.
     * @param _reserve the address of the reserve
     * @param _useAsCollateral true if the user wants to user the deposit as
     * collateral, false otherwise.
     **/
    function setUserUseReserveAsCollateral(
        address _reserve,
        bool _useAsCollateral
    )
        external
        nonReentrant
        onlyActiveReserve(_reserve)
        onlyUnfreezedReserve(_reserve)
    {
        uint256 underlyingBalance =
            core.getUserUnderlyingAssetBalance(_reserve, msg.sender);

        require(
            underlyingBalance > 0,
            "User does not have any liquidity deposited"
        );

        require(
            dataProvider.balanceDecreaseAllowed(
                _reserve,
                msg.sender,
                underlyingBalance
            ),
            "User deposit is already being used as collateral"
        );

        core.setUserUseReserveAsCollateral(
            _reserve,
            msg.sender,
            _useAsCollateral
        );

        if (_useAsCollateral) {
            emit ReserveUsedAsCollateralEnabled(_reserve, msg.sender);
        } else {
            emit ReserveUsedAsCollateralDisabled(_reserve, msg.sender);
        }
    }

    /**
     * @dev users can invoke this function to liquidate an undercollateralized
     * position.
     * @param _collateral the address of the collateral to liquidated
     * @param _reserve the address of the principal reserve
     * @param _user the address of the borrower
     * @param _purchaseAmount the amount of principal that the liquidator wants
     * to repay
     * @param _receiveMToken true if the liquidators wants to receive the
     * mTokens, false if
     * he wants to receive the underlying asset directly
     **/
    function liquidationCall(
        address _collateral,
        address _reserve,
        address _user,
        uint256 _purchaseAmount,
        bool _receiveMToken
    )
        external
        payable
        nonReentrant
        onlyActiveReserve(_reserve)
        onlyActiveReserve(_collateral)
    {
        address liquidationManager =
            addressesProvider.getLendingPoolLiquidationManager();
        //solium-disable-next-line
        (bool success, bytes memory result) =
            liquidationManager.delegatecall(
                abi.encodeWithSignature(
                    "liquidationCall(address,address,address,uint256,bool)",
                    _collateral,
                    _reserve,
                    _user,
                    _purchaseAmount,
                    _receiveMToken
                )
            );

        require(success, "Liquidation call failed");

        (uint256 returnCode, string memory returnMessage) =
            abi.decode(result, (uint256, string));

        if (returnCode != 0) {
            //error found
            revert(
                string(abi.encodePacked("Liquidation failed: ", returnMessage))
            );
        }
    }

    /**
     * @dev allows smartcontracts to access the liquidity of the pool within one
     * transaction,
     * as long as the amount taken plus a fee is returned. NOTE There are
     * security concerns for developers of flashloan receiver contracts
     * that must be kept into consideration. For further details please visit
     * TODO: Multiplier Finance Documentation Link
     * @param _receiver The address of the contract receiving the funds. The
     * receiver should implement the IFlashLoanReceiver interface.
     * @param _reserve the address of the principal reserve
     * @param _amount the amount requested for this flashloan
     **/
    function flashLoan(
        address _receiver,
        address _reserve,
        uint256 _amount,
        bytes memory _params
    )
        public
        nonReentrant
        onlyActiveReserve(_reserve)
        onlyAmountGreaterThanZero(_amount)
    {
        //check that the reserve has enough available liquidity
        uint256 availableLiquidityBefore =
            _reserve == BscAddressLib.bnbAddress()
                ? address(core).balance
                : IERC20(_reserve).balanceOf(address(core));

        require(
            availableLiquidityBefore >= _amount,
            "There is not enough liquidity available to borrow"
        );

        //calculate amount fee
        uint256 protocolFeeRate = feeProvider.getFlashLoanFee();
        uint256 amountFee = _amount.wadMul(protocolFeeRate);
        require(
            amountFee > 0,
            "The requested amount is too small for a flashLoan."
        );

        //get the FlashLoanReceiver instance
        IFlashLoanReceiver receiver = IFlashLoanReceiver(_receiver);

        address payable userPayable = address(uint160(_receiver));

        //transfer funds to the receiver
        core.transferToUser(_reserve, userPayable, _amount);

        //execute action of the receiver
        receiver.executeOperation(_reserve, _amount, amountFee, _params);

        //check that the actual balance of the core contract includes the returned amount
        uint256 availableLiquidityAfter =
            _reserve == BscAddressLib.bnbAddress()
                ? address(core).balance
                : IERC20(_reserve).balanceOf(address(core));

        require(
            availableLiquidityAfter == availableLiquidityBefore.add(amountFee),
            "The actual balance of the protocol is inconsistent"
        );

        core.updateStateOnFlashLoan(_reserve);

        // Reward Manager : Add reward item
        addRewardItem(_reserve, amountFee);

        // At this point in time, the _amountFee is in the Core address.
        core.transferToFeeCollectionAddress(
            _reserve,
            address(core),
            amountFee,
            true
        );

        //solium-disable-next-line
        emit FlashLoan(_receiver, _reserve, _amount, block.timestamp);
    }

    /**
     * @dev accessory functions to fetch data from the core contract
     **/

    function getReserveConfigurationData(address _reserve)
        external
        view
        returns (
            uint256 ltv,
            uint256 liquidationThreshold,
            uint256 liquidationBonus,
            address interestRateStrategyAddress,
            bool usageAsCollateralEnabled,
            bool borrowingEnabled,
            bool stableBorrowRateEnabled,
            bool isActive
        )
    {
        return dataProvider.getReserveConfigurationData(_reserve);
    }

    function getReserveData(address _reserve)
        external
        view
        returns (
            uint256 totalLiquidity,
            uint256 availableLiquidity,
            uint256 totalBorrowsStable,
            uint256 totalBorrowsVariable,
            uint256 liquidityRate,
            uint256 variableBorrowRate,
            uint256 stableBorrowRate,
            uint256 averageStableBorrowRate,
            uint256 utilizationRate,
            uint256 liquidityIndex,
            uint256 variableBorrowIndex,
            address mTokenAddress,
            uint40 lastUpdateTimestamp
        )
    {
        return dataProvider.getReserveData(_reserve);
    }

    function getUserAccountData(address _user)
        external
        view
        returns (
            uint256 totalLiquidityBNB,
            uint256 totalCollateralBNB,
            uint256 totalBorrowsBNB,
            uint256 totalFeesBNB,
            uint256 availableBorrowsBNB,
            uint256 currentLiquidationThreshold,
            uint256 ltv,
            uint256 healthFactor
        )
    {
        return dataProvider.getUserAccountData(_user);
    }

    function getUserReserveData(address _reserve, address _user)
        external
        view
        returns (
            uint256 currentMTokenBalance,
            uint256 currentBorrowBalance,
            uint256 principalBorrowBalance,
            uint256 borrowRateMode,
            uint256 borrowRate,
            uint256 liquidityRate,
            uint256 originationFee,
            uint256 variableBorrowIndex,
            uint256 lastUpdateTimestamp,
            bool usageAsCollateralEnabled
        )
    {
        return dataProvider.getUserReserveData(_reserve, _user);
    }

    function getReserves() external view returns (address[] memory) {
        return core.getReserves();
    }

    /**
     * @dev internal function to save on code size for the onlyActiveReserve
     * modifier
     **/
    function requireReserveActiveInternal(address _reserve) internal view {
        require(
            core.getReserveIsActive(_reserve),
            "Action requires an active reserve"
        );
    }

    /**
     * @notice internal function to save on code size for the
     * onlyUnfreezedReserve modifier
     **/
    function requireReserveNotFreezedInternal(address _reserve) internal view {
        require(
            !core.getReserveIsFreezed(_reserve),
            "Action requires an unfreezed reserve"
        );
    }

    /**
     * @notice internal function to save on code size for the
     * onlyAmountGreaterThanZero modifier
     **/
    function requireAmountGreaterThanZeroInternal(uint256 _amount)
        internal
        pure
    {
        require(_amount > 0, "Amount must be greater than 0");
    }

    // Reward Manager
    // This need to be called before reward giving. If new reserve token is added, call this function again //

    function registerAllPoolsForReward() external {
        rewardsMgr = RewardsManager(addressesProvider.getRewardManager());
        rewardsMgr.registerPools(core.getReserves());
    }

    function readReward(address _reserve, RewardsManager.RewardTypes _type)
        external
        view
        returns (uint256)
    {
        uint256 share =
            _type == RewardsManager.RewardTypes.Depositor
                ? core.getUsermTokenBalance(_reserve, msg.sender)
                : core.getUserStakedTokenBalance(msg.sender);

        return rewardsMgr.readRewards(_reserve, msg.sender, _type, share);
    }

    function updateRewards(address _reserve, address _user) public {
        uint256 mTokenBalance = core.getUsermTokenBalance(_reserve, _user);
        uint256 stakedBalance = core.getUserStakedTokenBalance(_user);

        rewardsMgr.updateRewards(
            _reserve,
            _user,
            mTokenBalance,
            stakedBalance,
            0
        );
    }

    function updateGovernanceStakingRewards(address _user) external {
        uint256 stakedBalance = core.getUserStakedTokenBalance(_user);

        address[] memory reserves = core.getReserves();
        uint256 len = reserves.length;
        for (uint256 i = 0; i < len; i++) {
            rewardsMgr.updateReward(
                reserves[i],
                _user,
                RewardsManager.RewardTypes.Governance,
                stakedBalance,
                0
            );
        }
    }

    function updateLpReward(address _reserve, address _user) external {
        uint256 mTokenBalance = core.getUsermTokenBalance(_reserve, _user);
        rewardsMgr.updateReward(
            _reserve,
            _user,
            RewardsManager.RewardTypes.Depositor,
            mTokenBalance,
            0
        );
    }

    // In case a user has not claim for a very long time (exceed eVM max read/write), we can update it by batches //
    function updatePartialReward(
        address _reserve,
        address _user,
        RewardsManager.RewardTypes _type,
        uint256 num
    ) external {
        // Allow batch update only for Depositor and Governance rewards //
        if (_type == RewardsManager.RewardTypes.Safety) {
            return;
        }

        uint256 balance =
            _type == RewardsManager.RewardTypes.Depositor
                ? core.getUsermTokenBalance(_reserve, _user)
                : core.getUserStakedTokenBalance(_user);

        rewardsMgr.updateReward(_reserve, _user, _type, balance, num);
    }

    function addRewardItem(address _reserve, uint256 _amount) private {
        RewardLocalVars memory reward;
        (reward.suppliers, reward.governace, ) = feeProvider.calculateRewards(
            _amount
        );

        uint256 liquidity = core.getTotalmTokenSupply(_reserve);
        rewardsMgr.addRewardItem(
            _reserve,
            reward.suppliers,
            liquidity,
            reward.governace
        );
    }

    function claimReward(address _reserve, RewardsManager.RewardTypes _type)
        external
    {
        claimRewardInternal(_reserve, _type);
    }

    function claimAllReward(RewardsManager.RewardTypes _type) external {
        // Loop over all rewards and claims //
        address[] memory addresses = core.getReserves();
        uint256 len = addresses.length;
        for (uint256 i = 0; i < len; i++) {
            claimRewardInternal(addresses[i], _type);
        }
    }

    function claimRewardInternal(
        address _reserve,
        RewardsManager.RewardTypes _type
    ) internal {
        uint256 share =
            _type == RewardsManager.RewardTypes.Depositor
                ? core.getUsermTokenBalance(_reserve, msg.sender)
                : core.getUserStakedTokenBalance(msg.sender);

        uint256 amt =
            rewardsMgr.readRewards(_reserve, msg.sender, _type, share);

        if (amt != 0) {
            rewardsMgr.resetReward(_reserve, msg.sender, _type);
            rewardsMgr.withdrawFromVault(_type, _reserve, msg.sender, amt);

            //solium-disable-next-line
            emit ClaimReward(_reserve, msg.sender, amt, block.timestamp);
        }
    }
}

/**
 * @title DefaultReserveInterestRateStrategy contract
 * @notice implements the calculation of the interest rates depending on the
 * reserve parameters.
 * @dev if there is need to update the calculation of the interest rates for a
 * specific reserve,
 * a new version of this contract will be deployed.
 * @author Multiplier Finance
 **/
contract DefaultReserveInterestRateStrategy is IReserveInterestRateStrategy {
    using WadRayMath for uint256;
    using SafeMath for uint256;

    /**
     * @dev this value represents the utilization rate at which the pool aims
     * to obtain most competitive borrow rates
     * expressed in ray (e27)
     **/
    uint256 public  optimalUtilizationRate;

    /**
     * @dev this value represents the excess utilization rate above the
     * optimal. It's always equal to
     * 1-optimal utilization rate. Added as a constant here for gas optimizations
     * expressed in ray (e27)
     **/
    uint256 public  excessUtilizationRate;

    LendingPoolAddressesProvider public addressesProvider;

    //base variable borrow rate when Utilization rate = 0. Expressed in ray
    uint256 public baseVariableBorrowRate;

    //slope of the variable interest curve when utilization rate > 0 and <=
    // OPTIMAL_UTILIZATION_RATE. Expressed in ray
    uint256 public variableRateSlope1;

    //slope of the variable interest curve when utilization rate >
    // OPTIMAL_UTILIZATION_RATE. Expressed in ray
    uint256 public variableRateSlope2;

    //slope of the stable interest curve when utilization rate > 0 and <=
    // OPTIMAL_UTILIZATION_RATE. Expressed in ray
    uint256 public stableRateSlope1;

    //slope of the stable interest curve when utilization rate >
    // OPTIMAL_UTILIZATION_RATE. Expressed in ray
    uint256 public stableRateSlope2;
    address public reserve;

    constructor(
        address _reserve,
        LendingPoolAddressesProvider _provider,
        uint256 _baseVariableBorrowRate,
        uint256 _variableRateSlope1,
        uint256 _variableRateSlope2,
        uint256 _stableRateSlope1,
        uint256 _stableRateSlope2,
        uint256 _optimalUtilizationRate
    ) public {
        addressesProvider = _provider;
        baseVariableBorrowRate = _baseVariableBorrowRate;
        variableRateSlope1 = _variableRateSlope1;
        variableRateSlope2 = _variableRateSlope2;
        stableRateSlope1 = _stableRateSlope1;
        stableRateSlope2 = _stableRateSlope2;
        optimalUtilizationRate = _optimalUtilizationRate;
        excessUtilizationRate = uint256(1e27).sub(_optimalUtilizationRate);

        reserve = _reserve;
    }

    /**
    @dev accessors
     */

    function getBaseVariableBorrowRate() external view returns (uint256) {
        return baseVariableBorrowRate;
    }

    function getVariableRateSlope1() external view returns (uint256) {
        return variableRateSlope1;
    }

    function getVariableRateSlope2() external view returns (uint256) {
        return variableRateSlope2;
    }

    function getStableRateSlope1() external view returns (uint256) {
        return stableRateSlope1;
    }

    function getStableRateSlope2() external view returns (uint256) {
        return stableRateSlope2;
    }

    /**
     * @dev calculates the interest rates depending on the available liquidity
     * and the total borrowed.
     * @param _reserve the address of the reserve
     * @param _availableLiquidity the liquidity available in the reserve
     * @param _totalBorrowsStable the total borrowed from the reserve a stable
     * rate
     * @param _totalBorrowsVariable the total borrowed from the reserve at a
     * variable rate
     * @param _averageStableBorrowRate the weighted average of all the stable
     * rate borrows
     * @return the liquidity rate, stable borrow rate and variable borrow rate
     * calculated from the input parameters
     **/
    function calculateInterestRates(
        address _reserve,
        uint256 _availableLiquidity,
        uint256 _totalBorrowsStable,
        uint256 _totalBorrowsVariable,
        uint256 _averageStableBorrowRate
    )
        external
        view
        returns (
            uint256 currentLiquidityRate,
            uint256 currentStableBorrowRate,
            uint256 currentVariableBorrowRate
        )
    {
        uint256 totalBorrows = _totalBorrowsStable.add(_totalBorrowsVariable);

        uint256 utilizationRate =
            (totalBorrows == 0 && _availableLiquidity == 0)
                ? 0
                : totalBorrows.rayDiv(_availableLiquidity.add(totalBorrows));

        currentStableBorrowRate = ILendingRateOracle(
            addressesProvider.getLendingRateOracle()
        )
            .getMarketBorrowRate(_reserve);

        if (utilizationRate > optimalUtilizationRate) {
            uint256 excessUtilizationRateRatio =
                utilizationRate.sub(optimalUtilizationRate).rayDiv(
                    excessUtilizationRate
                );

            currentStableBorrowRate = currentStableBorrowRate
                .add(stableRateSlope1)
                .add(stableRateSlope2.rayMul(excessUtilizationRateRatio));

            currentVariableBorrowRate = baseVariableBorrowRate
                .add(variableRateSlope1)
                .add(variableRateSlope2.rayMul(excessUtilizationRateRatio));
        } else {
            currentStableBorrowRate = currentStableBorrowRate.add(
                stableRateSlope1.rayMul(
                    utilizationRate.rayDiv(optimalUtilizationRate)
                )
            );
            currentVariableBorrowRate = baseVariableBorrowRate.add(
                utilizationRate.rayDiv(optimalUtilizationRate).rayMul(
                    variableRateSlope1
                )
            );
        }

        currentLiquidityRate = getOverallBorrowRateInternal(
            _totalBorrowsStable,
            _totalBorrowsVariable,
            currentVariableBorrowRate,
            _averageStableBorrowRate
        )
            .rayMul(utilizationRate);
    }

    /**
     * @dev calculates the overall borrow rate as the weighted average between the total variable borrows and total stable borrows.
     * @param _totalBorrowsStable the total borrowed from the reserve a stable
     * rate
     * @param _totalBorrowsVariable the total borrowed from the reserve at a
     * variable rate
     * @param _currentVariableBorrowRate the current variable borrow rate
     * @param _currentAverageStableBorrowRate the weighted average of all the
     * stable rate borrows
     * @return the weighted averaged borrow rate
     **/
    function getOverallBorrowRateInternal(
        uint256 _totalBorrowsStable,
        uint256 _totalBorrowsVariable,
        uint256 _currentVariableBorrowRate,
        uint256 _currentAverageStableBorrowRate
    ) internal pure returns (uint256) {
        uint256 totalBorrows = _totalBorrowsStable.add(_totalBorrowsVariable);

        if (totalBorrows == 0) return 0;

        uint256 weightedVariableRate =
            _totalBorrowsVariable.wadToRay().rayMul(_currentVariableBorrowRate);

        uint256 weightedStableRate =
            _totalBorrowsStable.wadToRay().rayMul(
                _currentAverageStableBorrowRate
            );

        uint256 overallBorrowRate =
            weightedVariableRate.add(weightedStableRate).rayDiv(
                totalBorrows.wadToRay()
            );

        return overallBorrowRate;
    }
}