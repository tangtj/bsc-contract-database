// SPDX-License-Identifier: MIT

pragma solidity ^0.8.14;

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

    /**
     * @dev Returns the decimals places of the token.
     */
    function decimals() external view returns (uint8);
}

contract Ownable is Context {
    address private _owner;

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
/**
 * @title SafeMathInt
 * @dev Math operations for int256 with overflow safety checks.
 */


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

contract TokenDistributor {
    constructor (address token) {
        IERC20(token).approve(msg.sender, uint(~uint256(0)));
    }
}

contract Atlanverse is ERC20 {

    using SafeMath for uint256;
    IUniswapV2Router02 public uniswapV2Router;
    address public  uniswapV2Pair;
    address _tokenOwner;

    uint public addLiquifyTime;//
    bool public isAddLiquify; //
    bool public swapState = false;//
    bool public bonusState = true;//
    bool public deflationState = true;//
    bool public inSwapAndLiquify;//
    uint256 public swapTokensAtAmount;//

    TokenDistributor public _tokenDistributor;//
    TokenDistributor public _bonusDistributor;//

    address public marketAddress = 0xc9C262E84B85D746f5417297332dDdc157387778;//
    address public pledgeAddress = 0x1aB1935Ab4DCE2e3ca1c3749822fB87Ec0D3Ec70;//
    address private _destroyAddress = address(0x000000000000000000000000000000000000dEaD);
    address private _usdtAddress = 0x55d398326f99059fF775485246999027B3197955;
    address private _pancakeRoute = 0x10ED43C718714eb63d5aA57B78B54704E256024E;


    // address private _usdtAddress = 0xFa60D973F7642B748046464e165A65B7323b0DEE;
    // address private _pancakeRoute = 0xD99D1c33F9fC3444f8101754aBC46c52416550D1;

    mapping(address => bool) private _isExcludedFromFees;//
    mapping(address => bool) public _swapPairList;//
    mapping (address => address) public _inviteeRecord;//

    uint256 public startTradeBlock;//

    uint256 transitionUnit = 10 ** 36;
    uint256 public protectionT;
    uint256 public protectionP;

    uint ratio = 1000;

    uint buyMarketingFee = 10;
    uint buyPoolFee = 10;
    uint buyBonusFee = 8;
    uint buyPledgeFee = 2;
    uint buyPromotionFee = 20;
    uint buyLimitMaxTime = 1800;

    uint marketingFee = 10;
    uint poolFee = 20;
    uint bonusFee = 6;
    uint pledgeFee = 4;
    uint destructionFee = 10;

    uint transferFee = 5;//

    uint public cutMaxTimes = 2400;
    uint public cutChunkTimes = 86400;
    mapping (address => uint) public cutLastAddLqTimes;

    event ExcludeFromFees(address indexed account, bool isExcluded);
    event ExcludeMultipleAccountsFromFees(address[] accounts, bool isExcluded);
    event SwapAndSendTo(
        address target,
        uint256 amount,
        string to
    );
    event SwapTokensForTokens(
        uint256 amountIn,
        address[] path
    );

    event SwapAndLiquify(
        uint256 tokensSwapped,
        uint256 ethReceived
    );

    modifier lockTheSwap {
        inSwapAndLiquify = true;
        _;
        inSwapAndLiquify = false;
    }
    constructor() ERC20("Atlanverse", "Atl") {
        address tokenOwner = _msgSender();
        _tokenOwner = tokenOwner;
        
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(_pancakeRoute);
        address _uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), _usdtAddress);
        uniswapV2Router = _uniswapV2Router;
        uniswapV2Pair = _uniswapV2Pair;
        _swapPairList[_uniswapV2Pair] = true;

        _approve(address(this), address(uniswapV2Router), ~uint256(0));
        IERC20(_usdtAddress).approve(address(uniswapV2Router), ~uint256(0));
        excludeFromFees(tokenOwner, true);
        excludeFromFees(address(this), true);
        uint256 total = 100000000000 * 10 ** decimals();
        swapTokensAtAmount = total.div(10000000);
        _mint(tokenOwner, total);
        _tokenDistributor = new TokenDistributor(address(_usdtAddress));
        _bonusDistributor = new TokenDistributor(address(_usdtAddress));

        _isExcludedFromFees[_tokenOwner] = true;
        addLiquifyTime = block.timestamp;
    }
    receive() external payable {
    }

    function balanceOf(address account) public view override returns (uint256) {
        if( _isExcludedFromFees[account] || isContract(account) || account == _destroyAddress || account == address(0)){
            return super.balanceOf(account);
        }
        if(addLiquifyTime == 0){
            return super.balanceOf(account);
        }
        if(!deflationState){
            return super.balanceOf(account);
        }
        uint time = block.timestamp;
        return _balanceOf(account,time);
    }
    function _balanceOf(address account,uint time)internal view returns(uint){
        uint bal = super.balanceOf(account);
        if( bal > 0 ){
            uint lastAddLqTime = cutLastAddLqTimes[account];
            if(addLiquifyTime > lastAddLqTime){
                lastAddLqTime = addLiquifyTime;
            }
            if( lastAddLqTime > 0 && time > lastAddLqTime ){
                uint i = (time - lastAddLqTime) / cutChunkTimes;
                i = i > cutMaxTimes ? cutMaxTimes : i;
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
    function getRate(uint a,uint n)private view returns(uint){
        for( uint i = 0; i < n; i++){
            a = a.mul(99).div(100);
        }
        return a;
    }


    function excludeFromFees(address account, bool excluded) public onlyOwner {
        _isExcludedFromFees[account] = excluded;
        emit ExcludeFromFees(account, excluded);
    }

    function excludeMultipleAccountsFromFees(address[] calldata accounts, bool excluded) public onlyOwner {
        for (uint256 i = 0; i < accounts.length; i++) {
            _isExcludedFromFees[accounts[i]] = excluded;
        }
        emit ExcludeMultipleAccountsFromFees(accounts, excluded);
    }

    function setSwapTokensAtAmount(uint256 _swapTokensAtAmount) public onlyOwner {
        swapTokensAtAmount = _swapTokensAtAmount;
    }
    function setCutChunkTimes(uint _chunkTimes)external onlyOwner{
        cutChunkTimes = _chunkTimes;
    }
    function setBuyLimitMaxTime(uint _limitMaxTime)external onlyOwner{
        buyLimitMaxTime = _limitMaxTime;
    }
    function changeSwapStats() public onlyOwner {
        swapState = !swapState;
    }

    function changeBonusState() public onlyOwner {
        bonusState = !bonusState;
    }
    function changeDeflationState() public onlyOwner {
        deflationState = !deflationState;
    }

    function setMarketAddress(address addr) external onlyOwner {
        marketAddress = addr;
        _isExcludedFromFees[addr] = true;
    }
    function setPledgeAddress(address addr) external onlyOwner {
        pledgeAddress = addr;
        _isExcludedFromFees[addr] = true;
    }
    function setBuyFee(uint _buyMarketingFee,uint _buyPoolFee,uint _buyBonusFee,uint _buyPledgeFee,uint _buyPromotionFee) external onlyOwner {
        buyMarketingFee = _buyMarketingFee;
        buyPoolFee = _buyPoolFee;
        buyBonusFee = _buyBonusFee;
        buyPledgeFee = _buyPledgeFee;
        buyPromotionFee = _buyPromotionFee;
    }
    function setSellFee(uint _marketingFee,uint _poolFee,uint _bonusFee,uint _pledgeFee,uint _destructionFee) external onlyOwner {
        marketingFee = _marketingFee;
        poolFee = _poolFee;
        bonusFee = _bonusFee;
        pledgeFee = _pledgeFee;
        destructionFee = _destructionFee;
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
        require(amount > 0, "amount need biger than zero");

        //
        if (from == address(this) || to == address(this) || inSwapAndLiquify) {
            return super._transfer(from, to, amount);
        }

        //
        if( !_isExcludedFromFees[from] && !isContract(from)){
            _updateBal(from,block.timestamp);
        }
        if( !_isExcludedFromFees[to] && !isContract(to)){
            _updateBal(to,block.timestamp);
        }

        //
        bool noPair = !_swapPairList[to] && !_swapPairList[from];
        bool noThis = from != address(this) || to != address(this);
        bool noDestroy = to != _destroyAddress;
        bool noExcluded = !_isExcludedFromFees[from] && !_isExcludedFromFees[to];
        if(noPair && noThis && noDestroy){
            //
            if(isContract(from) || isContract(to)){
                require(swapState || from == _tokenOwner, "!swapState");
            }
            //
            if(!isContract(from) && !isContract(to) && balanceOf(to) == 0 && _inviteeRecord[to] == address(0) && _inviteeRecord[from] != to){
                _inviteeRecord[to] = from;
            }
            uint256 transferLpAmount = 0;
            //
            if(isAddLiquify && noExcluded){
                transferLpAmount = amount.div(100).mul(transferFee);
                super._transfer(from, address(this), transferLpAmount);
            }
            return super._transfer(from, to, amount.sub(transferLpAmount));
        }

        require(swapState || from == _tokenOwner, "!swapState");
        //
        if (_swapPairList[from] || _swapPairList[to]) {
            if (false == isAddLiquify){
                isAddLiquify = true;
            }
            if (0 == startTradeBlock) {
                startTradeBlock = block.timestamp;
            }
            if (0 == addLiquifyTime){
                addLiquifyTime = block.timestamp;
            }
            if (0 == protectionT){
                protectionT = block.timestamp;
                //_resetProtection();
            }
            
            if (_swapPairList[from]) {
                addLpProvider(to);
            } else {
                addLpProvider(from);
            }
        }

        _tokenTransfer(from,to,amount);
        
        //
        if (bonusState && from != address(this)&& block.timestamp - startTradeBlock > 600) {
            if(block.timestamp.sub(protectionT) >= 86400){_resetProtection();}
            processLP(500000);
        }
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        uint recipientAmount = tAmount;
        if (_swapPairList[sender] && !_isExcludedFromFees[recipient] && balanceOf(uniswapV2Pair) > 0) {//Buy
            // Exceeding limit
            uint limit = 20000000 * 10 ** decimals();
            if(block.timestamp - addLiquifyTime <= buyLimitMaxTime && (balanceOf(address(recipient)) >= limit || tAmount > limit)){
                require(false, "Exceeding limit!");
            }
            //
            uint totalFee = ratio - buyMarketingFee - buyPoolFee - buyBonusFee - buyPledgeFee;
            if(_inviteeRecord[recipient] != address(0)){
                uint count = tAmount.div(ratio).mul(buyPromotionFee);
                address cur = _inviteeRecord[recipient];
                for(uint8 i = 0;i<5;i++){
                    if(cur == address(0)){
                        break;
                    }
                    uint amount = 0;
                    if(i == 0){
                        amount = count.div(buyPromotionFee).mul(8);
                    } else if(i == 1){
                        amount = count.div(buyPromotionFee).mul(5);
                    } else if(i == 2){
                        amount = count.div(buyPromotionFee).mul(3);
                    } else if(i == 3){
                        amount = count.div(buyPromotionFee).mul(2);
                    } else if(i == 4){
                        amount = count.div(buyPromotionFee).mul(2);
                    }
                    if(amount == 0){
                        break;
                    }
                    super._transfer(sender, cur, amount);
                    tAmount -= amount;
                    cur = _inviteeRecord[cur];
                }
            }
            uint pledgeAmount = tAmount.div(ratio).mul(buyPledgeFee);
            recipientAmount = tAmount.div(ratio).mul(totalFee);
            super._transfer(sender, address(pledgeAddress), pledgeAmount);
            super._transfer(sender, address(this), tAmount.sub(recipientAmount).sub(pledgeAmount));
            
        } else if (_swapPairList[recipient] && !_isExcludedFromFees[sender] && balanceOf(uniswapV2Pair) > 0){//Sell
            if(block.timestamp.sub(protectionT) >= 86400){_resetProtection();}
            //
            uint256 currentP = (IERC20(_usdtAddress).balanceOf(address(uniswapV2Pair))).mul(transitionUnit).div(balanceOf(address(uniswapV2Pair)));
            //
            if(protectionT > 0 && currentP < protectionP.div(100).mul(100 - 10)){
                poolFee = 140;
                bonusFee = 12;
                pledgeFee = 8;
                destructionFee = 30;
            }
            //
            else if(protectionP > 0 && currentP < protectionP.div(100).mul(100 - 30)){
                poolFee = 400;
                bonusFee = 25;
                pledgeFee = 15;
                destructionFee = 50;
            }else{
                poolFee = 20;
                bonusFee = 6;
                pledgeFee = 4;
                destructionFee = 10;
            }

            if(!inSwapAndLiquify){
                swapAndLiquify(tAmount);
            }

            //
            uint totalFee = ratio - marketingFee - bonusFee - destructionFee - poolFee - pledgeFee;
            recipientAmount = tAmount.div(ratio).mul(totalFee);

            uint destroyAmount = tAmount.div(ratio).mul(destructionFee);
            uint pledgeAmount = tAmount.div(ratio).mul(pledgeFee);
            super._transfer(sender, _destroyAddress, destroyAmount);
            super._transfer(sender, address(pledgeAddress), pledgeAmount);
            super._transfer(sender, address(this), tAmount.sub(destroyAmount).sub(recipientAmount).sub(pledgeAmount));
        }
        super._transfer(sender, recipient, recipientAmount);
    }
    function swapAndLiquify(uint amount) internal lockTheSwap {
        if(balanceOf(address(this)) == 0){
            return;
        }
        uint256 contractTokenBalance = balanceOf(address(this));
        if (contractTokenBalance < swapTokensAtAmount) {
            return;
        }
        //
        uint256 oldbalance = IERC20(_usdtAddress).balanceOf(address(_tokenDistributor));
        swapTokensForUSDT(contractTokenBalance,address(_tokenDistributor));
        uint256 newbalance = IERC20(_usdtAddress).balanceOf(address(_tokenDistributor));
        newbalance = newbalance.sub(oldbalance);
        if(newbalance <= 0){
            return;
        }
        uint totalFee = marketingFee + bonusFee + poolFee;
        IERC20(_usdtAddress).transferFrom(address(_tokenDistributor), marketAddress, newbalance.div(totalFee).mul(marketingFee));
        IERC20(_usdtAddress).transferFrom(address(_tokenDistributor), address(_bonusDistributor), newbalance.div(totalFee).mul(bonusFee));
        IERC20(_usdtAddress).transferFrom(address(_tokenDistributor), address(uniswapV2Pair), newbalance.div(totalFee).mul(poolFee));
    }

    function resetProtection() external onlyOwner {
        protectionT = block.timestamp;
        protectionP = (IERC20(_usdtAddress).balanceOf(address(uniswapV2Pair))).mul(transitionUnit).div(balanceOf(address(uniswapV2Pair)));
    }
    function setProtection(uint256 _protectionP, uint256 _protectionT) external onlyOwner {
        protectionP = _protectionP;
        protectionT = _protectionT;
    }
    function _resetProtection() private {
        uint256 time = block.timestamp;
        if (time.sub(protectionT) >= 86400) {
            protectionT = protectionT.add(86400);
            protectionP = (IERC20(_usdtAddress).balanceOf(address(uniswapV2Pair))).mul(transitionUnit).div(balanceOf(address(uniswapV2Pair)));
        }
    }
    function rescueToken(address tokenAddress, uint256 tokens) public returns (bool success)
    {
        require(msg.sender == _tokenOwner, "invalid address");
        return IERC20(tokenAddress).transfer(msg.sender, tokens);
    }


    function swapTokensForUSDT(uint256 tokenAmount,address recipient) private {
        if(tokenAmount == 0){
            return;
        }
        // generate the uniswap pair path of token -> udst
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _usdtAddress;

        // make the swap
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of ETH
            path,
            recipient,
            block.timestamp
        );
        emit SwapTokensForTokens(tokenAmount, path);
    }

    function _updateBal(address owner,uint time) internal{
        if(!deflationState){
            return;
        }
        uint bal = super.balanceOf(owner);
        if( bal > 0 ){
            uint updatedBal = _balanceOf(owner,time);

            if( bal > updatedBal){
                cutLastAddLqTimes[owner] = time;
                uint ba = bal - updatedBal;
                super._transfer(owner, _destroyAddress, ba);
            }
        }else{
            cutLastAddLqTimes[owner] = time;
        }
    }
    function isContract(address _address) public view returns (bool) {
        uint256 codeSize;
        assembly {
            codeSize := extcodesize(_address)
        }
        return codeSize > 0;
    }

    address[] private lpProviders;
    mapping(address => uint256) lpProviderIndex;
    mapping(address => bool) excludeLpProvider;
    uint256 private currentIndex;
    uint256 private lpRewardCondition = 1;
    uint256 private progressLPBlock;

    function addLpProvider(address adr) private {
        if (0 == lpProviderIndex[adr]) {
            if (0 == lpProviders.length || lpProviders[0] != adr) {
                lpProviderIndex[adr] = lpProviders.length;
                lpProviders.push(adr);
            }
        }
    }

    //
    function processLP(uint256 gas) private {
        //
        if (block.timestamp - progressLPBlock < 1800) {
            return;
        }
        //
        uint totalPair = IERC20(uniswapV2Pair).totalSupply();
        if (0 == totalPair) {
            return;
        }

        IERC20 USDT = IERC20(_usdtAddress);
        uint256 usdtBalance = USDT.balanceOf(address(_bonusDistributor));
        //
        // if (usdtBalance < lpRewardCondition) {
        //     return;
        // }
        if(usdtBalance <= 0){
            return;
        }

        address shareHolder;
        uint256 pairBalance;
        uint256 amount;

        uint256 shareholderCount = lpProviders.length;

        uint256 gasUsed = 0;
        uint256 iterations = 0;

        // 
        uint256 gasLeft = gasleft();

        //
        while (gasUsed < gas && iterations < shareholderCount) {
            //
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }
            shareHolder = lpProviders[currentIndex];
            //
            pairBalance = IERC20(uniswapV2Pair).balanceOf(shareHolder);
            //
            if (pairBalance > 0 && !excludeLpProvider[shareHolder]) {
                amount = usdtBalance * pairBalance / totalPair;
                //
                if (amount > 0) {
                    USDT.transferFrom(address(_bonusDistributor),shareHolder, amount);
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }

        progressLPBlock = block.timestamp;
    }
}