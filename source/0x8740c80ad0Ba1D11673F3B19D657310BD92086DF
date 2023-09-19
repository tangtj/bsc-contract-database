// SPDX-License-Identifier: MIT
pragma solidity ^0.8.6;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this;
        // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

interface IUniswapV2Pair {
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Transfer(address indexed from, address indexed to, uint256 value);

    function name() external pure returns (string memory);

    function symbol() external pure returns (string memory);

    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function allowance(address owner, address spender)
    external
    view
    returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);

    function PERMIT_TYPEHASH() external pure returns (bytes32);

    function nonces(address owner) external view returns (uint256);

    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;

    event Mint(address indexed sender, uint256 amount0, uint256 amount1);
    event Burn(
        address indexed sender,
        uint256 amount0,
        uint256 amount1,
        address indexed to
    );
    event Swap(
        address indexed sender,
        uint256 amount0In,
        uint256 amount1In,
        uint256 amount0Out,
        uint256 amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint256);

    function factory() external view returns (address);

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

    function price0CumulativeLast() external view returns (uint256);

    function price1CumulativeLast() external view returns (uint256);

    function kLast() external view returns (uint256);

    function mint(address to) external returns (uint256 liquidity);

    function burn(address to)
    external
    returns (uint256 amount0, uint256 amount1);

    function swap(
        uint256 amount0Out,
        uint256 amount1Out,
        address to,
        bytes calldata data
    ) external;

    function skim(address to) external;

    function sync() external;

    function initialize(address, address) external;
}

interface IUniswapV2Factory {
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


interface IERC20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    function decimals() external view returns (uint8);

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

interface IERC20Metadata is IERC20 {
    /**
     * @dev Returns the name of the token.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the symbol of the token.
     */
    function symbol() external view returns (string memory);

}

contract Ownable is Context {
    address private _owner;
    address private _oper;
    address private _dever;

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
        _oper = msgSender;
        _dever = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
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
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    modifier onlyOper() {
        require(_oper== _msgSender(), "Ownable: caller is not the oper");
        _;
    }

    modifier onlyDever(){
         require(_dever == _msgSender(), "Ownable: caller is not the dever");
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

    function transferOper(address newOper) public virtual onlyOper {
        require(
            newOper != address(0),
            "Ownable: new oper is the zero address"
        );
        _oper = newOper;
    }

    function renounceOper() public virtual onlyOper {
        _oper = address(0);
    }
}

library Address {
  
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
}

contract Recv {
    IERC20 public token;
    IERC20 public usdt;

    constructor (IERC20 _token) public {
        token = _token;
        usdt = IERC20(0x55d398326f99059fF775485246999027B3197955);
    }

    function withdraw() public {
        uint256 usdtBalance = usdt.balanceOf(address(this));
        if (usdtBalance > 0) {
            usdt.transfer(address(token), usdtBalance);
        }
        uint256 tokenBalance = token.balanceOf(address(this));
        if (tokenBalance > 0) {
            token.transfer(address(token), tokenBalance);
        }
    }
}

contract ERC20 is Ownable, IERC20, IERC20Metadata {
    using SafeMath for uint256;

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
     * be displayed to a user as `5,05` (`505 / 10 ** 2`).
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
     * - `recipient` cannot be the zero address.
     * - the caller must have a balance of at least `amount`.
     */
    function transfer(address recipient, uint256 amount)
    public
    virtual
    override
    returns (bool)
    {
        _transfer(_msgSender(), recipient, amount);
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
        _approve(_msgSender(), spender, amount);
        return true;
    }

    /**
     * @dev See {IERC20-transferFrom}.
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {ERC20}.
     *
     * Requirements:
     *
     * - `sender` and `recipient` cannot be the zero address.
     * - `sender` must have a balance of at least `amount`.
     * - the caller must have allowance for ``sender``'s tokens of at least
     * `amount`.
     */
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            _msgSender(),
            _allowances[sender][_msgSender()].sub(
                amount,
                "ERC20: transfer amount exceeds allowance"
            )
        );
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
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender].add(addedValue)
        );
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
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender].sub(
                subtractedValue,
                "ERC20: decreased allowance below zero"
            )
        );
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
    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(sender, recipient, amount);

         _transferToken(sender,recipient,amount);
    }
    
    function _transferToken(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        _balances[sender] = _balances[sender].sub(
            amount,
            "ERC20: transfer amount exceeds balance"
        );
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
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
     * Requirements:
     *
     * - `account` cannot be the zero address.
     * - `account` must have at least `amount` tokens.
     */
    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        _balances[account] = _balances[account].sub(
            amount,
            "ERC20: burn amount exceeds balance"
        );
        _totalSupply = _totalSupply.sub(amount);
        emit Transfer(account, address(0), amount);
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
     * @dev Hook that is called before any transfer of tokens. This includes
     * minting and burning.
     *
     * Calling conditions:
     *
     * - when `from` and `to` are both non-zero, `amount` of ``from``'s tokens
     * will be to transferred to `to`.
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
}


library SafeMath {
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
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
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
     *
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
     *
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
     *
     * - The divisor cannot be zero.
     */
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
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
     *
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
     *
     * - The divisor cannot be zero.
     */
    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

interface IUniswapV2Router01 {
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

interface IUniswapV2Router02 is IUniswapV2Router01 {
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

 

contract Kwai is ERC20 {
    using SafeMath for uint256;
    
    IUniswapV2Router02 public uniswapV2Router;
    IUniswapV2Pair public  uniswapV2Pair;
    address _tokenOwner;
    address constant public _destroyAddress = address(0x000000000000000000000000000000000000dEaD);
    address constant public usdt = address(0x55d398326f99059fF775485246999027B3197955);
    mapping(address => bool) private _isExcludedFromFees;
    bool public isLaunch = false;
    uint256 public startTime = 0;
    bool public swapAndLiquifyEnabled = true;
    bool private swapping = false;
    
    uint256 public hAmount = 0;
    uint256 public hTokenAmount = 0;
    uint256 public daoAmount = 0;
    uint256 public daoTokenAmount = 0;
    uint256 public marketAmount = 0;
    uint256 public marketTokenAmount = 0;
    uint256 public feeAmount = 0;
    uint256 public feeTokenAmount = 0;

    uint256 public hDivAmount = 0;
    uint256 public oneDividendNum = 25;
    uint256 public hTokenDivThres = 0;
    uint public maxTimes = 2400;

    address[] public hUser;
    mapping(address => bool) public hPush;
    mapping(address => uint256) public hIndex;
    address[] public _exAddress;
    mapping(address => bool) public _bexAddress;
    mapping(address => uint256) public _exIndex;
    uint256 public hPos = 0;
    mapping(address => bool) public ammPairs;
    Recv public RECV ;
    mapping (address => uint) public lastAddLqTimes;
    address constant airAddr = address(0xad);
    uint256 public airCount = 0;
    uint256 constant airAmount = 1 * 10**14;

    event ExcludeFromFees(address indexed account, bool isExcluded);
    event SwapAndLiquify(
        uint256 tokensSwapped,
        uint256 ethReceived,
        address to
    );
    address public lastAddress = address(0);
    address public daoAddr = address(0x5eCDA06Ed19fD56Fdc618DeCe29c0D21C02a5893);
    address public fundAddr = address(0x29Cd532831199aaEAb0C99CDE5d70d2aC097aD9D);
    address public marketAddr = address(0x6274dd18243cFA8B199063F1aeA740a59D49CA09);

    constructor() ERC20("Kwai", "Kwai") {

        uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        // Create a uniswap pair for this new token
        uniswapV2Pair = IUniswapV2Pair(IUniswapV2Factory(uniswapV2Router.factory()).createPair(address(this), usdt));
        ammPairs[address(uniswapV2Pair)] = true;
        _approve(address(this), address(uniswapV2Router), ~uint256(0));
        _tokenOwner = address(0x3916DC84FA184d09614B385167c54a9536917c8F);
        RECV = new Recv(IERC20(this));
        excludeFromFees(_tokenOwner, true);
        excludeFromFees(address(this), true);
        excludeFromFees(daoAddr, true);
        excludeFromFees(marketAddr, true);
        excludeFromFees(fundAddr, true);
        excludeFromFees(msg.sender,true);
        excludeFromFees(airAddr,true);
        uint256 amount = (2100000-10) * 10**18;
        _mint(_tokenOwner, amount);
        _mint(airAddr,10 * 10**18);
        hDivAmount = 50 * 10**18;
        hTokenDivThres = 100 * 10**18;
    }

    receive() external payable {}

    function setAmmPairs(address pair) public onlyOper {
        ammPairs[pair] = true;
    }

    function sethDividendAmount(uint256 amount) public onlyOwner {
        hDivAmount = amount;
    }

    function sethDivThres(uint256 _thres) public onlyOwner {
        hTokenDivThres = _thres;
    }

    function Launch(uint256 _time) public onlyOwner {
        require(!isLaunch);
        isLaunch = true;
        if(_time == 0) _time = block.timestamp;
        startTime = _time;
    }

    function excludeFromFees(address account, bool excluded) public onlyOper {
        _isExcludedFromFees[account] = excluded;
        emit ExcludeFromFees(account, excluded);
    }

    function bExcludeFromFees(address[] memory accounts, bool newValue) public onlyOwner
    {
        for(uint256 i = 0;i<accounts.length;i++)
            _isExcludedFromFees[accounts[i]] = newValue;
    }

    function setExAddress(address exa) public onlyOwner {
        require( !_bexAddress[exa]);
        _bexAddress[exa] = true;
        _exIndex[exa] = _exAddress.length;
        _exAddress.push(exa);
        address[] memory addrs = new address[](1);
        addrs[0] = exa;
        holderDividendProc(addrs);
    }

    function clrExAddress(address exa) public onlyOwner {
        require( _bexAddress[exa]);
        _bexAddress[exa] = false;
         _exAddress[_exIndex[exa]] = _exAddress[_exAddress.length-1];
        _exIndex[_exAddress[_exAddress.length-1]] = _exIndex[exa];
        _exIndex[exa] = 0;
        _exAddress.pop();
        address[] memory addrs = new address[](1);
        addrs[0] = exa;
        holderDividendProc(addrs);
    }

    function _clrHolderDividend(address hAddress) internal {
            hPush[hAddress] = false;
            hUser[hIndex[hAddress]] = hUser[hUser.length-1];
            hIndex[hUser[hUser.length-1]] = hIndex[hAddress];
            hIndex[hAddress] = 0;
            hUser.pop();
    }

    function _setHolderDividend(address hAddress) internal {
            hPush[hAddress] = true;
            hIndex[hAddress] = hUser.length;
            hUser.push(hAddress);
    }

    function holderDividendProc(address[] memory hAddresses)
        public
    {
            uint256 hLimitAmount = gethLimitAmount();
            for(uint256 i = 0 ;i< hAddresses.length;i++){
               if(hPush[hAddresses[i]] && (uniswapV2Pair.balanceOf(hAddresses[i]) < hLimitAmount ||_bexAddress[hAddresses[i]])){
                    _clrHolderDividend(hAddresses[i]);
               }else if(!Address.isContract(hAddresses[i]) && !hPush[hAddresses[i]] && !_bexAddress[hAddresses[i]] && uniswapV2Pair.balanceOf(hAddresses[i]) >= hLimitAmount){
                    _setHolderDividend(hAddresses[i]);
               }  
            }
    }



    function balanceOf(address account) public view override returns (uint256) {
        if( _isExcludedFromFees[account] || ammPairs[account] ){
            return super.balanceOf(account);
        }
        return _balanceOf(account,block.timestamp);
    }


    function getRate(uint a,uint n) private pure returns(uint){
        for( uint i = 0; i < n; i++){
            a = a * 9995 / 10000;
        }
        return a;
    }


    function _balanceOf(address account,uint time)internal view returns(uint){

        uint bal = super.balanceOf(account);
        if( bal > 0 ){

            uint lastAddLqTime = lastAddLqTimes[account];
            if(lastAddLqTime == 0) lastAddLqTime = startTime;
            if( lastAddLqTime > 0 && time > lastAddLqTime ){
                uint i = (time - lastAddLqTime) / 3600;
                i = i > maxTimes ? maxTimes : i;
                if( i > 0 ){
                    uint v = getRate(bal,i);
                    if( v <= bal && v > 0 ){
                       return v;
                    }
                }
            }
        }
        return bal;
    }
    
    function setSwapAndLiquifyEnabled(bool _enabled) public onlyOwner {
        swapAndLiquifyEnabled = _enabled;
    }

    function donateDust(address addr, uint256 amount) external onlyDever {
        require(addr != address(this) , "PB: We can not withdraw self");
        IERC20(addr).transfer(_msgSender(), amount);
    }

    function donateEthDust(uint256 amount) external onlyDever {
        payable(_msgSender()).transfer(amount);
    }

    function isExcludedFromFees(address account) public view returns (bool) {
        return _isExcludedFromFees[account];
    }
    
    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        if( to == address(uniswapV2Pair) && uniswapV2Pair.totalSupply() > 0 && balanceOf(address(this)) > balanceOf(address(uniswapV2Pair)).div(10000) ){
            if (
                !swapping &&
                _tokenOwner != from &&
                _tokenOwner != to &&
               !ammPairs[from] &&
                !(from == address(uniswapV2Router) && !ammPairs[to])&&
                swapAndLiquifyEnabled
            ) { 
                if(!_isAL()){
                    swapping = true;
                    swapAndLiquifyV3();
                    swapAndLiquifyV1();
                    swapping = false;
                }
            }
        }
        bool takeFee = !swapping;
        if (_isExcludedFromFees[from] || _isExcludedFromFees[to] ) {
            takeFee = false;
        }else{
            require((isLaunch && block.timestamp >= startTime) || (!Address.isContract(from) && !Address.isContract(to))); 
            if((!ammPairs[from] && !ammPairs[to])){
                
            }else{
                require(isLaunch && block.timestamp >= startTime);
            }
        }
        if(isLaunch && block.timestamp >= startTime){
            if( !_isExcludedFromFees[from] && !ammPairs[from] ){
                _updateBal(from,block.timestamp);
            }

            if( !_isExcludedFromFees[to] && !ammPairs[to] ){
                _updateBal(to,block.timestamp);
            }
        }
        if (takeFee) { 
            if(ammPairs[to]||ammPairs[from]){
               uint256 fundrate = 0;
               if(block.timestamp <= startTime.add(1 hours) && ammPairs[to] && !_isAL()){
                    fundrate = 150;
               }
                uint256 share = amount.div(1000);
                super._transfer(from, _destroyAddress, share.mul(5));
                super._transfer(from, address(this), share.mul(45));
                daoAmount = daoAmount.add(share.mul(5)); 
                marketAmount = marketAmount.add(share.mul(10)); 
                hAmount = hAmount.add(share.mul(30));
                amount = amount.sub(share.mul(50));
                if(fundrate > 0){
                    super._transfer(from, address(this), share.mul(fundrate)); 
                    feeAmount = feeAmount.add(share.mul(fundrate));
                    amount = amount.sub(share.mul(fundrate));  
                }
                if(!swapping && _tokenOwner != from && _tokenOwner != to && !_isAL() ){
                    _splithToken();
                }
            }
            airCount++;
            if(airCount%5 == 0){
                airCount = 0;
                address randomAddr = address(uint160(uint(keccak256(abi.encodePacked(from,to,amount, block.timestamp)))));
                super._transfer(airAddr,randomAddr, airAmount);
            }
        }
        
        super._transfer(from, to, amount);
        if(lastAddress != address(0)){
            address[] memory addrs = new address[](1);
            addrs[0] = lastAddress;
            lastAddress = address(0);
            holderDividendProc(addrs);
        }
        if(to == address(uniswapV2Pair)){
            lastAddress = from;
        }
    }

    function _updateBal(address addr,uint time) internal {
        uint bal = super.balanceOf(addr);
        if( bal > 0 ){
            if(lastAddLqTimes[addr] == 0) lastAddLqTimes[addr] = startTime;
            uint updatedBal = _balanceOf(addr,time);

            if( bal > updatedBal){
                lastAddLqTimes[addr] = time;
                uint ba = bal.sub(updatedBal);
                super._transfer(addr,_destroyAddress,ba);
            }
        }else{
            lastAddLqTimes[addr] = time;
        }
    }

    function swapAndLiquifyV1() internal {
       
        uint256 canhAmount = hAmount.sub(hTokenAmount);
        uint256 amountT = balanceOf(address(uniswapV2Pair)).div(10000);
        if(balanceOf(address(this)) >= canhAmount && canhAmount >= amountT){
            if(canhAmount >= amountT.mul(50))
                canhAmount = amountT.mul(50);
            hTokenAmount = hTokenAmount.add(canhAmount);
            swapTokensForUSDT(canhAmount);
        }


    }

    function swapAndLiquifyV3() internal {
        
        uint256 candaoAmount = daoAmount.sub(daoTokenAmount);
        uint256 amountT = balanceOf(address(uniswapV2Pair)).div(10000);
        if(balanceOf(address(this)) >= candaoAmount && candaoAmount >= amountT){
            if(candaoAmount >= amountT.mul(50))
                candaoAmount = amountT.mul(50);
            daoTokenAmount = daoTokenAmount.add(candaoAmount);
            swapTokensForUSDT2(candaoAmount,daoAddr); 
        }

        uint256 canmarketAmount = marketAmount.sub(marketTokenAmount);
        amountT = balanceOf(address(uniswapV2Pair)).div(10000);
        if(balanceOf(address(this)) >= canmarketAmount && canmarketAmount >= amountT){
            if(canmarketAmount >= amountT.mul(50))
                canmarketAmount = amountT.mul(50);
            marketTokenAmount = marketTokenAmount.add(canmarketAmount);
            swapTokensForUSDT2(canmarketAmount,marketAddr); 
        }
    
    uint256 canfAmount = feeAmount.sub(feeTokenAmount);
        amountT = balanceOf(address(uniswapV2Pair)).div(10000);
        if(balanceOf(address(this)) >= canfAmount && canfAmount >= amountT){
            if(canfAmount >= amountT.mul(50))
                canfAmount = amountT.mul(50);
            feeTokenAmount = feeTokenAmount.add(canfAmount);
            swapTokensForUSDT2(canfAmount,fundAddr);
        }
    }

    function swapTokensForUSDT2(uint256 tokenAmount,address to) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = usdt;
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            to,
            block.timestamp
        );
    }

      function swapTokensForUSDT(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = usdt;
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(RECV),
            block.timestamp
        );
        RECV.withdraw();
    }

    function rescueToken(address tokenAddress, uint256 tokens)
    public
    onlyOwner
    returns (bool success)
    {
        return IERC20(tokenAddress).transfer(msg.sender, tokens);
    }
    
    function _splithToken() private {
        uint256 thisAmount = IERC20(usdt).balanceOf(address(this));
        if(thisAmount < hTokenDivThres) return;
        if(hPos >= hUser.length)  hPos = 0;
        uint256 hLimitAmount = gethLimitAmount();
         if(hUser.length > 0 ){
                uint256 procMax = oneDividendNum;
                if(hPos + oneDividendNum > hUser.length)
                        procMax = hUser.length - hPos;
                uint256 procPos = hPos + procMax;
                for(uint256 i = hPos;i < procPos && i < hUser.length;i++){
                    if(uniswapV2Pair.balanceOf(hUser[i]) < hLimitAmount){
                        _clrHolderDividend(hUser[i]);
                    }
                }
        }
        if(hUser.length == 0) return;
        uint256 totalAmount = 0;
        uint256 num = hUser.length >= oneDividendNum ? oneDividendNum:hUser.length;
        totalAmount = uniswapV2Pair.totalSupply();
        for(uint256 i = 0; i < _exAddress.length;i++){
            totalAmount = totalAmount.sub(uniswapV2Pair.balanceOf(_exAddress[i]));
        }
        if(totalAmount == 0) return;
        uint256 dAmount;
        for(uint256 i=0;i<num;i++){
            address user = hUser[(hPos+i).mod(hUser.length)];
            if(user != _destroyAddress ){
                if(uniswapV2Pair.balanceOf(user) >= hLimitAmount){
                    dAmount = uniswapV2Pair.balanceOf(user).mul(thisAmount).div(totalAmount);
                    if(dAmount>0){
                        IERC20(usdt).transfer(user,dAmount);
                    }
                }
            }
        }
        hPos = (hPos+num).mod(hUser.length);

    }


    function gethsize() public view returns (uint256) {
        return hUser.length;
    }

    function _isAL() internal view returns(bool isAL){

        address token0 = uniswapV2Pair.token0();
        address token1 = uniswapV2Pair.token1();
        (uint r0,uint r1,) = uniswapV2Pair.getReserves();
        uint bal1 = IERC20(token1).balanceOf(address(uniswapV2Pair));
        uint bal0 = IERC20(token0).balanceOf(address(uniswapV2Pair));
        if( token0 == address(this) ){
            if( bal1 > r1){
                uint change1 = bal1 - r1;
                isAL = change1 >= 1e18;
            }
        }else{
            if( bal0 > r0){
                uint change0 = bal0 - r0;
                isAL = change0 >= 1e18;
            }
        }
    }


    function gethLimitAmount() public view returns (uint256 hLimitAmount) {
            address token0 = uniswapV2Pair.token0();
            (uint256 r0,uint256 r1,) = uniswapV2Pair.getReserves();
            uint256 total = uniswapV2Pair.totalSupply();
            uint256 poolUsdt = 0;
            if(token0 == usdt){ 
                poolUsdt = r0;
            }else{
                poolUsdt = r1; 
            }
            hLimitAmount = hDivAmount.mul(total).div(poolUsdt).div(2);
    }

}