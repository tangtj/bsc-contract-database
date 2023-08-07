// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts v4.4.1 (utils/math/SafeMath.sol)

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
    function tryAdd(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
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
    function tryMul(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
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
    function tryDiv(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
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
    function tryMod(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
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

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

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
        _approve(owner, spender, _allowances[owner][spender] + addedValue);
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
        uint256 currentAllowance = _allowances[owner][spender];
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
     * @dev Moves `amount` of tokens from `sender` to `recipient`.
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
        }
        _balances[to] += amount;

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
     * @dev Spend `amount` form the allowance of `owner` toward `spender`.
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

contract FoisonCoin is ERC20 {
    using SafeMath for uint256;

    struct Record {
        uint256 blockId;
        uint256 amount;
    }

    struct ExchangeRate {
        uint256 blockId;
        int256 rate;
    }

    // constants
    uint256 private constant E18 = 1e18;
    uint256 private constant E = 1e8;
    uint256 private constant E20 = 20 * E * E18;
    uint256 private constant E45 = 45 * E * E18;
    uint256 private constant E60 = 60 * E * E18;
    uint256 private constant E70 = 70 * E * E18;
    uint256 private constant E15 = 15 * E * E18;
    uint256[2][5] private rates = [
        [uint256(66), uint256(100000)],
        [uint256(33), uint256(100000)],
        [uint256(165), uint256(10000000)],
        [uint256(825), uint256(100000000)],
        [uint256(4125), uint256(1000000000)]
    ];

    // account
    address private constant USDTADR =
        0xF0fa82Bc9Bc443adD29Ef454B679fb938C56D7AF;

    address private _owner;
    address private _firstAdr;
    address private _usdtAdr;
    address private _bustAdr;
    // rasie pool
    bool private _isRaising = true;
    uint256 private _rasieTotal = 5 * E * E18;
    uint256[2][3] private exchangeRates = [
        [uint256(57), 5 * E * E18],
        [uint256(64), 5 * E * E18],
        [uint256(72), 5 * E * E18]
    ];
    uint256 private cur_rate_idx = 0;

    uint256 private _mintTotal = 0;
    mapping(address => Record) private records;
    uint256[] private breakChangeIds; // 0->20->45->60->70
    event Pledge(address indexed _from, uint256 _value);
    event Withdraw(
        address indexed _from,
        uint256 _value,
        uint256 _totalMint,
        address _recommender,
        uint256 _recommenderValue
    );
    event UpdateMintMount(uint256 blockId, uint256 _value);
    event RaiseSucc(
        address indexed _from,
        uint256 _value,
        uint256 _rasieTotal,
        address _recommender,
        uint256 _recommenderValue,
        address _usdtFrom
    );

    constructor(
        address authUser,
        address firstCtr,
        address usdtCtr,
        address busdCtr
    ) ERC20("Big Foison Coin", "BFC") {
        _mint(authUser, 15 * E * E18);
        _owner = msg.sender;
        _firstAdr = firstCtr;
        _usdtAdr = usdtCtr;
        _bustAdr = busdCtr;
    }

    fallback() external {}

    receive() external payable {}

    function raise(
        uint256 amount,
        address usdtFrom,
        address recommender
    ) public returns (uint256) {
        require(checkIsValidUsdt(usdtFrom), "invalid usdt contracts address");

        uint256 curRate = usdtFrom == address(_firstAdr)
            ? 1
            : exchangeRates[cur_rate_idx][0];
        uint256 curUsdtAmount = usdtFrom == address(_firstAdr) ? amount : (curRate * amount) / uint256(10000);

        require(
            ERC20(usdtFrom).allowance(msg.sender, address(this)) >=
                curUsdtAmount,
            "token need allowance > amount"
        );
        require(
            ERC20(usdtFrom).balanceOf(msg.sender) >= curUsdtAmount,
            "rasie amount must less than balance"
        );
        require(amount >= 0, "amount must greater than 0");

        if (usdtFrom != address(_firstAdr)) {
            require(_isRaising, "raise is over!!");
            require(amount >= 100 * E18, "amount should be larger than 100");
        }

        require(E15 >= amount, "rasie pool must greater than amount ");

        ERC20(usdtFrom).transferFrom(
            msg.sender,
            address(USDTADR),
            curUsdtAmount
        );
        mint(msg.sender, amount);

        if (recommender != address(0) && usdtFrom != address(_firstAdr)) {
            uint256 toRecAmount = amount.div(10);
            mint(recommender, toRecAmount);
            _rasieTotal -= toRecAmount;
        }

        if (usdtFrom != address(_firstAdr)) {
            _rasieTotal -= amount;
        }

        emit RaiseSucc(
            msg.sender,
            amount,
            _rasieTotal,
            recommender,
            amount.div(10),
            usdtFrom
        );
        return amount;
    }

    function totalSupply() public override pure returns (uint256) {
        return 100 * E * E18;
    }

    function checkIsValidUsdt(address adr) private view returns (bool) {
        return adr == _firstAdr || adr == _usdtAdr || adr == _bustAdr;
    }

    function isRaiseOver() public view returns (bool) {
        return _isRaising == false;
    }

    function getCurrentRaiseRate() public view returns (uint256) {
        return exchangeRates[cur_rate_idx][0];
    }

    function startNextTerm() public returns (uint256) {
        require(msg.sender == _owner, "permission refuse");

        require(isRaiseOver(), "current raise is not over!");

        if (cur_rate_idx < 2) {
            cur_rate_idx++;
        }

        _rasieTotal = exchangeRates[cur_rate_idx][1];
        _isRaising = true;
        return exchangeRates[cur_rate_idx][0];
    }

    function rasieOver(address promoteUser) public {
        require(msg.sender == _owner, "permission refuse");

        // transfer to rasie
        if (_rasieTotal > 0) {
            mint(promoteUser, _rasieTotal);
        }

        _rasieTotal = 0;
        _isRaising = false;
    }

    function getCurrentRasieMount() public view returns (uint256) {
        return _rasieTotal;
    }

    function mint(address recipient, uint256 amount) private {
        _mint(recipient, amount);
    }

    function pledge(uint256 amount) public {
        require(
            allowance(msg.sender, address(this)) >= amount,
            "token need allowance > amount"
        );
        require(balanceOf(msg.sender) >= amount, "user balance need > amount");

        require(
            amount > 100 * 1000 * E18,
            "pledge amount must be larger than 100w"
        );

        require(!checkIsPledge(msg.sender), "tokenAdr is in pledge");

        _transfer(msg.sender, address(this), amount);

        records[msg.sender].blockId = block.number;
        records[msg.sender].amount = amount;
        emit Pledge(msg.sender, amount);
    }

    function checkIsPledge(address tokenAdr) public view returns (bool) {
        return records[tokenAdr].blockId != 0;
    }

    function withdraw(address recommender) public returns (uint256[2] memory) {
        require(records[msg.sender].amount >= 0, "user has not pledge yet");

        uint256 interest = getInterest(
            records[msg.sender].amount,
            block.number,
            records[msg.sender].blockId
        );
        uint256 amount = records[msg.sender].amount;
        // uint256 amountWithInterest = amount + interest;

        pay(interest, amount);
        updateMintAmount(interest);
        if (recommender != address(0)) {
            mint(recommender, (interest * 5) / 10);
            updateMintAmount((interest * 5) / 10);
        }

        emit Withdraw(
            msg.sender,
            amount,
            interest,
            recommender,
            (interest * 5) / 10
        );

        records[msg.sender].blockId = 0;
        records[msg.sender].amount = 0;
        return [amount, interest];
    }

    function pay(uint256 interest, uint256 amount) private {
        mint(msg.sender, interest);
        _transfer(address(this), msg.sender, amount);
    }

    function updateMintAmount(uint256 mount) private {
        uint256 nextMount = _mintTotal + mount;

        uint256 startIdx = getChangeIdx(_mintTotal);
        uint256 endIdx = getChangeIdx(nextMount);

        if (endIdx > startIdx) {
            updateExchangeIds(block.number, [startIdx, endIdx]);
        }

        _mintTotal = nextMount;
        emit UpdateMintMount(block.number, _mintTotal);
    }

    function updateExchangeIds(uint256 blockId, uint256[2] memory startAndEnd)
        private
    {
        uint256 start = startAndEnd[0];
        uint256 end = startAndEnd[1];

        for (uint256 i = start; i < end; i++) {
            breakChangeIds[i] = blockId;
        }
    }

    function getInterest(
        uint256 amount,
        uint256 endId,
        uint256 startId
    ) public view returns (uint256) {
        uint256 changeTimes = breakChangeIds.length;
        uint256 intersts = 0;
        uint256 curId = startId;
        uint256 i = 0;
        for (i = 0; i < changeTimes; i++) {
            if (breakChangeIds[i] > curId) {
                intersts +=
                    (amount * getDay(breakChangeIds[i], curId) * rates[i][0]) /
                    rates[i][1] /
                    100000;
            }
            curId = breakChangeIds[i];
        }

        intersts +=
            (amount * getDay(endId, curId) * rates[i][0]) /
            rates[i][1] /
            100000;
        return intersts;
    }

    function getCurrentInterest(address tokenAdr)
        public
        view
        returns (uint256[2] memory)
    {
        require(checkIsPledge(tokenAdr), "token needs in pledge");

        uint256 amount = records[tokenAdr].amount;

        return [
            amount,
            getInterest(amount, block.number, records[tokenAdr].blockId)
        ];
    }

    function getDay(uint256 endId, uint256 startId)
        public
        pure
        returns (uint256)
    {
        require(endId >= startId, "endId must be larger than startId");
        return ((endId - startId) * 100000) / 28800;
    }

    function getChangeIdx(uint256 mount) private pure returns (uint256) {
        if (mount < E20) return 0;
        if (mount >= E20 && mount < E45) return 1;
        if (mount >= E45 && mount < E60) return 2;
        if (mount >= E60 && mount < E70) return 3;

        return 4;
    }

    function getMintTotalAmount() public view returns (uint256) {
        return _mintTotal;
    }
}