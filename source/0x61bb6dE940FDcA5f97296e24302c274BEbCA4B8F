// SPDX-License-Identifier: MIT
pragma solidity >=0.6.0 <0.9.0;

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
abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this;
        // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
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
abstract contract Ownable is Context {
    address internal _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
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
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
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
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

/**
 * @dev Interface of the BEP20 standard as defined in the EIP.
 */
interface IBEP20 {
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
    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

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
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

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
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

/**
 * @dev Interface for the optional metadata functions from the BEP20 standard.
 *
 * _Available since v4.1._
 */
interface IBEP20Metadata {
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
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryAdd(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
        uint256 c = a + b;
        if (c < a) return (false, 0);
        return (true, c);
    }

    /**
     * @dev Returns the substraction of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function trySub(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
        if (b > a) return (false, 0);
        return (true, a - b);
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryMul(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) return (true, 0);
        uint256 c = a * b;
        if (c / a != b) return (false, 0);
        return (true, c);
    }

    /**
     * @dev Returns the division of two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryDiv(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
        if (b == 0) return (false, 0);
        return (true, a / b);
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryMod(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
        if (b == 0) return (false, 0);
        return (true, a % b);
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
        require(b <= a, "SafeMath: subtraction overflow");
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
        if (a == 0) return 0;
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting on
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
        require(b > 0, "SafeMath: division by zero");
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
        require(b > 0, "SafeMath: modulo by zero");
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
        require(b <= a, errorMessage);
        return a - b;
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting with custom message on
     * division by zero. The result is rounded towards zero.
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {tryDiv}.
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
        return a / b;
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
        require(b > 0, errorMessage);
        return a % b;
    }
}

contract BEP20 is Context, IBEP20, IBEP20Metadata {
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
    constructor(
        address platform,
        string memory name_,
        string memory symbol_,
        uint256 supply
    ) {
        _name = name_;
        _symbol = symbol_;

        _mint(platform, supply * 10**decimals());
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
     * @dev See {IERC20-transfer}.
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
     * @dev See {IERC20-allowance}.
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
     * @dev See {IERC20-approve}.
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
     * `subtractedValue`.
     */
    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        virtual
        returns (bool)
    {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(
            currentAllowance >= subtractedValue,
            "ERC20: decreased allowance below zero"
        );
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
        require(
            fromBalance >= amount,
            "ERC20: transfer amount exceeds balance"
        );
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
            require(
                currentAllowance >= amount,
                "ERC20: insufficient allowance"
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

interface IPair {
    function sync() external;

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves()
        external
        view
        returns (
            uint112 reserve0,
            uint112 reserve1,
            uint32 blockTimestampLast
        );

    function totalSupply() external view returns (uint256);
}

interface IFactory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);

    function getPair(address tokenA, address tokenB)
        external
        view
        returns (address pair);
}

abstract contract Excludes {
    mapping(address => bool) internal _Excludes;

    function setExclude(address _user, bool b) public {
        _authorizeExcludes();
        _Excludes[_user] = b;
    }

    function setExcludes(address[] memory _user, bool b) public {
        _authorizeExcludes();
        for (uint256 i = 0; i < _user.length; i++) {
            _Excludes[_user[i]] = b;
        }
    }

    function isExcludes(address _user) public view returns (bool) {
        return _Excludes[_user];
    }

    function _authorizeExcludes() internal virtual {}
}

abstract contract Managers {
    mapping(address => bool) internal _managers;

    modifier onlyManager() {
        require(isManager(msg.sender), "Managers: caller is not a manager");
        _;
    }

    function setManager(address user, bool b) public {
        _authorizeManagers();
        _managers[user] = b;
    }

    function setManagers(address[] memory user, bool b) public {
        _authorizeManagers();
        for (uint256 i = 0; i < user.length; i++) {
            _managers[user[i]] = b;
        }
    }

    function isManager(address user) public view returns (bool) {
        return _managers[user];
    }

    function _authorizeManagers() internal virtual {}
}

interface IRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

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

    function getAmountsOut(uint256 amountIn, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);

    function getAmountsIn(uint256 amountOut, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);
}

contract TokenStation {
    constructor(address token) {
        IBEP20(token).approve(msg.sender, type(uint256).max);
    }

    function getAddr() external view returns (address) {
        return address(this);
    }
}

abstract contract TokenBurnable is BEP20 {
    /**
     * @dev Destroys `amount` tokens from the caller.
     *
     * See {TRC20-_burn}.
     */
    function burn(uint256 amount) external {
        _burn(_msgSender(), amount);
    }

    /**
     * @dev Destroys `amount` tokens from `account`, deducting from the caller's
     * allowance.
     *
     * See {TRC20-_burn} and {TRC20-allowance}.
     *
     * Requirements:
     *
     * - the caller must have allowance for `accounts`'s tokens of at least
     * `amount`.
     */
    function burnFrom(address account, uint256 amount) external {
        address spender = _msgSender();
        _spendAllowance(account, spender, amount);
        _burn(account, amount);
    }
}


contract TokenABC is TokenBurnable, Excludes, Managers, Ownable {
    using SafeMath for uint256;

    address public pair;
    address[] private _buyPath;
    address[] private _sellPath;

    uint256 public constant calcBase = 10000;
    // 买：5%滑点，有上级，3%给上级，2%给基金会合约，无上级，5%全部给基金会合约。
    uint256 public feeRateOfBuy = 500;
    uint256 public feeRateOfBuy2Upper = 300;
    uint256 public feeRateOfBuy2Station = 200;

    uint256 public markedPrice;
    uint256 public markedTime;

    //uint256 public constant oneDay = 1 days;//正式
    uint256 public constant oneDay = 5 minutes; //测试
    uint256 public oneMonth = 30 days;

    address public blackHole = 0x000000000000000000000000000000000000dEaD;
    address public ZERO = address(0);
    IRouter public router = IRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E); //BSC
    IBEP20 public USDT = IBEP20(0xEDeE96891B0C3cf6589610A523852acA1f1e1fA1); //BSC Fake USDT

    uint256 listingTime; //上Pancake时间

    TokenStation private usdtStation;
    TokenStation private wpcStation;

    mapping(address => address) public uppers;
    mapping(address => mapping(address => bool)) public lowers;

    mapping(address => uint256) public accumulativeBuy;
    //每个地址累计最多购买500枚
    uint256 public maxWPCPerAddr = 500 ether;
    //单次最多购买500U的
    uint256 public maxUPerTransaction = 500 ether;

    //上中心化交易所前可以自由交易
    bool public freeTrade;
    //无涨停限制
    bool public freeSell;
    //无跌停限制
    bool public freeBuy;

    bool public swapTaxFeeAuto;

    bool public saveMarketAuto;
    //每次可处理折合U的上限，防止砸盘,如果是0则无限制。默认500U
    uint256 public swapTaxFeeLimitByUSDT;

    uint256 public saveMarketAt = 10 ether;
    uint256 public swapTaxFeeAt = 0.1 ether;

    address public platform;

    bool public initialized;

    event OnSaveMarket(address indexed initiator, uint256 amountOfU);
    event OnSwapTaxFee(address indexed initiator, uint256 amountOfWPC);

    mapping(address => bool) public primaries; //primary investors

    bool inSwap;
    modifier lockSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor(
        address _platform
    ) BEP20(_platform, "ABC Token", "ABC", 21 * 10 ** 6) {
        platform = _platform;
    }

    function initialize() external onlyOwner() {
        require(!initialized, "already initialized");
        address[] memory path = new address[](2);
        path[0] = address(USDT);
        path[1] = address(this);
        _buyPath = path;
        address[] memory path2 = new address[](2);
        path2[0] = address(this);
        path2[1] = address(USDT);
        _sellPath = path2;

        USDT.approve(address(router), type(uint256).max);
        _approve(address(this), address(router), type(uint256).max);

        listingTime = block.timestamp;
        usdtStation = new TokenStation(address(USDT));
        wpcStation = new TokenStation(address(this));

        _Excludes[address(this)] = true;
        _Excludes[platform] = true;
        _Excludes[blackHole] = true;
        _Excludes[wpcStation.getAddr()] = true;

        swapTaxFeeAuto = true;
        swapTaxFeeLimitByUSDT = 500 ether;

        primaries[address(this)] = true;
        primaries[platform] = true;
        primaries[blackHole] = true;

        emit OwnershipTransferred(_owner, platform);
        _owner = platform;
        _managers[platform] = true;

        initialized = true;
    }

    function monthSinceListing() public view returns (uint256) {
        return block.timestamp.sub(listingTime).div(oneMonth);
    }

    function daySinceListing() public view returns (uint256) {
        return block.timestamp.sub(listingTime).div(oneDay);
    }

    function stationBalanceOfWPC() public view returns (uint256) {
        return balanceOf(wpcStation.getAddr());
    }

    function stationBalanceOfUSDT() public view returns (uint256) {
        return USDT.balanceOf(usdtStation.getAddr());
    }

    function setFreeTrade(bool b) external onlyManager {
        freeTrade = b;
    }

    function setFreeBuy(bool b) external onlyManager {
        freeBuy = b;
    }

    function setFreeSell(bool b) external onlyManager {
        freeSell = b;
    }

    function setSwapTaxFeeAuto(bool b) external onlyManager {
        swapTaxFeeAuto = b;
    }

    function setSwapTaxFeeLimitByUSDT(uint256 limitU) external onlyManager {
        swapTaxFeeLimitByUSDT = limitU;
    }

    function setSaveMarketAt(uint256 at) external onlyManager {
        saveMarketAt = at;
    }

    function setSwapTaxFeeAt(uint256 at) external onlyManager {
        swapTaxFeeAt = at;
    }

    function setMaxWPCPerAddr(uint256 newVal) external onlyManager {
        maxWPCPerAddr = newVal;
    }

    function setMaxUPerTransaction(uint256 newVal) external onlyManager {
        maxUPerTransaction = newVal;
    }

    function setPrimaries(
        address[] memory accounts,
        bool b
    ) external onlyOwner {
        for (uint256 i = 0; i < accounts.length; i++) {
            primaries[accounts[i]] = b;
        }
    }

    function setListingTime(uint256 t) external onlyManager {
        if (t == 0) {
            listingTime = block.timestamp;
            markedTime = block.timestamp;
        } else {
            listingTime = t;
            markedTime = t;
        }
        markedPrice = getSellPrice4USDT(1 ether);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual override {
        if (freeTrade || isExcludes(from) || isExcludes(to)) {
            super._transfer(from, to, amount);
            return;
        }

        uint256 currentPrice = getSellPrice4USDT(1 ether); //就是1个WPC可以兑换出多少U
        if (markedPrice == 0 || (block.timestamp.sub(markedTime) >= oneDay)) {
            markedTime = block.timestamp;
            markedPrice = currentPrice;
        }

        if (_isPair(from)) {
            //Buy
            if (!freeBuy) {
                if (currentPrice <= 1 ether) {
                    require(primaries[to], "not primary investor");
                }

                if (currentPrice > markedPrice) {
                    //在涨
                    uint256 theDay = daySinceListing();
                    uint256 riseLimit;
                    //递减式：涨停机制改成第一天100%第二天50%第三天10%之后一直10%,自动停止交易，只能卖不能买
                    if (theDay == 0) {
                        //100%
                        riseLimit = calcBase;
                    } else if (theDay == 1) {
                        //50%
                        riseLimit = 5000;
                    } else {
                        //10%
                        riseLimit = 1000;
                    }

                    uint256 rised = currentPrice
                        .sub(markedPrice)
                        .mul(calcBase)
                        .div(markedPrice);

                    require(rised < riseLimit, "can not buy at limit up");
                }

                uint256 buyFee = _handBuyFee(from, to, amount);
                amount = amount.sub(buyFee);

                uint256 limitAmount = getBuyPrice4WPC(maxUPerTransaction);
                require(
                    amount <= limitAmount,
                    "exceeds the max limit per transaction"
                );

                if (accumulativeBuy[to].add(amount) <= maxWPCPerAddr) {
                    //每个地址只能累计买maxWPCPerAddr个WPC
                    accumulativeBuy[to] = accumulativeBuy[to].add(amount);
                } else {
                    revert("exceeds the allowance of this address");
                }
            }
        } else if (_isPair(to)) {
            if (!freeSell) {
                //Sell
                if (currentPrice < markedPrice) {
                    //在跌
                    uint256 fall = markedPrice
                        .sub(currentPrice)
                        .mul(calcBase)
                        .div(markedPrice);

                    require(fall < 1000, "can not sell at limit down");
                }

                uint256 sellFee = _handSellFee(from, amount);
                amount = amount.sub(sellFee);
            }
            if (swapTaxFeeAuto && !inSwap) {
                _swapTaxFee();
            }
        } else {
            //绑定上下级关系
            if (amount == 1 ether) {
                //上级发出绑定请求
                if (uppers[to] == ZERO) {
                    //如果受邀的人还没有上级才可以
                    lowers[from][to] = true;
                }
            } else if (amount == 0.5 ether) {
                //下级回应
                if (lowers[to][from]) {
                    //形成绑定关系
                    uppers[from] = to;
                }
            }

            //普通地址互转
            uint256 fee = _dynamicFeeByMonth(amount);
            amount = amount.sub(fee);
            //初始20%...第四个月5%，代币自动打入黑洞销毁
            super._transfer(from, blackHole, fee);

            if (swapTaxFeeAuto && !inSwap) {
                //自动出成U到基金会那里
                _swapTaxFee();
            }
        }

        super._transfer(from, to, amount);
    }

    function saveMarket() public onlyManager {
        _saveMarket();
    }

    function _saveMarket() private lockSwap {
        uint256 amount = USDT.balanceOf(usdtStation.getAddr());
        if (amount > saveMarketAt) {
            //金额太小没意义
            USDT.transferFrom(usdtStation.getAddr(), address(this), amount);

            router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
                amount,
                0,
                _buyPath,
                blackHole,
                block.timestamp + 9
            );

            emit OnSaveMarket(msg.sender, amount);
        }
    }

    function swapTaxFee() public onlyManager {
        _swapTaxFee();
    }

    function getSellPrice4USDT(
        uint256 amountWPC
    ) public view returns (uint256) {
        uint256[] memory amounts = router.getAmountsOut(amountWPC, _sellPath);
        if (amounts.length > 1) return amounts[1];
        return 0;
    }

    function getBuyPrice4WPC(uint256 amountUSDT) public view returns (uint256) {
        uint256[] memory amounts = router.getAmountsOut(amountUSDT, _buyPath);
        if (amounts.length > 1) return amounts[1];
        return 0;
    }

    function rescueLossToken(
        address token,
        address to,
        uint256 amount
    ) public onlyOwner {
        IBEP20(token).transfer(to, amount);
    }

    function _swapTaxFee() private lockSwap {
        uint256 totalFee = balanceOf(wpcStation.getAddr());
        if (totalFee > swapTaxFeeAt) {
            //金额太小没意义
            uint256 canSell;
            if (swapTaxFeeLimitByUSDT > 0) {
                canSell = getBuyPrice4WPC(swapTaxFeeLimitByUSDT);
                canSell = canSell > totalFee ? totalFee : canSell;
            } else {
                canSell = totalFee;
            }

            super._transfer(wpcStation.getAddr(), address(this), canSell);

            router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
                canSell,
                0,
                _sellPath,
                usdtStation.getAddr(),
                block.timestamp + 9
            );

            emit OnSwapTaxFee(msg.sender, canSell);
        }
    }

    function _ifLiquidityAdded() private view returns (bool isAddLP) {
        address token0 = IPair(pair).token0();
        address token1 = IPair(pair).token1();
        (uint256 r0, uint256 r1, ) = IPair(pair).getReserves();
        uint256 bal0 = IBEP20(token0).balanceOf(pair);
        uint256 bal1 = IBEP20(token1).balanceOf(pair);
        if (token0 == address(this)) return bal1.sub(r1) > 1000;
        else return bal0.sub(r0) > 1000;
    }

    function _ifLiquidityRemoved() private view returns (bool isRemoveLP) {
        address token0 = IPair(pair).token0();
        if (token0 == address(this)) return false;
        (uint256 r0, , ) = IPair(pair).getReserves();
        uint256 bal0 = IBEP20(token0).balanceOf(pair);
        return r0 > bal0.add(1000);
    }

    function _isPair(address p) private returns (bool) {
        if (pair == ZERO) {
            pair = IFactory(router.factory()).getPair(
                address(USDT),
                address(this)
            );
        }

        if (pair == ZERO) {
            pair = IFactory(router.factory()).createPair(
                address(USDT),
                address(this)
            );
            listingTime = block.timestamp;
        }

        return pair == p;
    }

    function _handBuyFee(
        //from是pair
        address from,
        address to,
        uint256 amount
    ) private returns (uint256 buyFee) {
        buyFee = amount.mul(feeRateOfBuy).div(calcBase); //5%
        uint256 toStation;
        address theUpper = uppers[to];
        if (theUpper != ZERO) {
            //有上级
            uint256 toUpper = amount.mul(feeRateOfBuy2Upper).div(calcBase); //3%
            bool oriValue = isExcludes(theUpper);
            if (!oriValue) {
                _Excludes[theUpper] = true;
            }
            //返现无滑点
            super._transfer(from, theUpper, toUpper);

            if (oriValue != isExcludes(theUpper)) {
                _Excludes[theUpper] = oriValue;
            }

            toStation = amount.mul(feeRateOfBuy2Station).div(calcBase); //2%

            buyFee = toUpper.add(toStation);
        } else {
            toStation = buyFee;
        }

        super._transfer(from, wpcStation.getAddr(), toStation);
    }

    function _handSellFee(
        address from,
        uint256 amount
    ) private returns (uint256 sellFee) {
        sellFee = _dynamicFeeByMonth(amount);
        super._transfer(from, wpcStation.getAddr(), sellFee);
    }

    function _dynamicFeeByMonth(
        uint256 amount
    ) private view returns (uint256 fee) {
        uint256 theMonth = monthSinceListing();

        if (theMonth == 0) {
            //第一个月20%
            fee = amount.mul(2000).div(calcBase);
        } else if (theMonth == 1) {
            //第二个月15%
            fee = amount.mul(1500).div(calcBase);
        } else if (theMonth == 2) {
            //第三个月10%
            fee = amount.mul(1000).div(calcBase);
        } else {
            //第四个月及以后5%
            fee = amount.mul(500).div(calcBase);
        }
    }

    function _authorizeExcludes() internal virtual override onlyOwner {}

    function _authorizeManagers() internal virtual override onlyOwner {}
}