// SPDX-License-Identifier: GPL-3.0-or-later

pragma solidity ^0.8.0;

/** 

 /$$$$$$$  /$$                                                     /$$            /$$$$$$            /$$          
| $$__  $$|__/                                                    | $$           /$$__  $$          |__/          
| $$  \ $$ /$$  /$$$$$$$  /$$$$$$$  /$$$$$$  /$$   /$$ /$$$$$$$  /$$$$$$        | $$  \__/  /$$$$$$  /$$ /$$$$$$$ 
| $$  | $$| $$ /$$_____/ /$$_____/ /$$__  $$| $$  | $$| $$__  $$|_  $$_/        | $$       /$$__  $$| $$| $$__  $$
| $$  | $$| $$|  $$$$$$ | $$      | $$  \ $$| $$  | $$| $$  \ $$  | $$          | $$      | $$  \ $$| $$| $$  \ $$
| $$  | $$| $$ \____  $$| $$      | $$  | $$| $$  | $$| $$  | $$  | $$ /$$      | $$    $$| $$  | $$| $$| $$  | $$
| $$$$$$$/| $$ /$$$$$$$/|  $$$$$$$|  $$$$$$/|  $$$$$$/| $$  | $$  |  $$$$/      |  $$$$$$/|  $$$$$$/| $$| $$  | $$
|_______/ |__/|_______/  \_______/ \______/  \______/ |__/  |__/   \___/         \______/  \______/ |__/|__/  |__/
                                                                                                                  
    #Discount Coin Features
	
	Total Supply 10,000,000,000
	0% of every trade goes to existing holders
	0% of every trade is burned
	0% of every trade is auto-locked in liquidity
	5% of every trade goes to TokenVault
    It is possible to make changes to the Discount Coin contract to make it TRULY HYPER-Deflationary 
    if the community prefers. This contract can be edited to set it up that way.
    Discount Coin is designed with the primary purpose of providing the general public with a 16% discount on all purchases 
    made using credit cards or cryptocurrencies. When you use Discount Coin for transactions, a 5% tax is automatically 
    generated and sent to a special vault. This 5% from each buy and sell transaction is used to cover the 16% discount 
    provided to every user of the coin.
    
    For more information, please visit our website at www.discountcoin.info
    
*/

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

/**
 * @dev Contract module which allows children to implement an emergency stop
 * mechanism that can be triggered by an authorized account.
 *
 * This module is used through inheritance. It will make available the
 * modifiers `whenNotPaused` and `whenPaused`, which can be applied to
 * the functions of your contract. Note that they will not be pausable by
 * simply including this module, only once the modifiers are put in place.
 */
abstract contract Pausable is Context {
    /**
     * @dev Emitted when the pause is triggered by `account`.
     */
    event Paused(address account);

    /**
     * @dev Emitted when the pause is lifted by `account`.
     */
    event Unpaused(address account);

    bool private _paused;

    /**
     * @dev Initializes the contract in unpaused state.
     */
    constructor() {
        _paused = false;
    }

    /**
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        require(!paused(), "Pausable: paused");
        _;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is paused.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    modifier whenPaused() {
        require(paused(), "Pausable: not paused");
        _;
    }

    /**
     * @dev Triggers stopped state.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }

    /**
     * @dev Returns to normal state.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
    }
}

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
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

/**
 * @dev Implementation of the {IERC20} interface.
 *
 * This implementation is agnostic to the way tokens are created. This means
 * that a supply mechanism has to be added in a derived contract using {_mint}.
 * For a generic mechanism see {ERC20PresetMinterPauser}.
 *
 * TIP: For a detailed writeup see our guide
 * https://forum.zeppelin.solutions/t/how-to-implement-erc20-supply-mechanisms/226[How
 * to implement supply mechanisms].
 *
 * We have followed general OpenZeppelin Contracts guidelines: functions revert
 * instead returning `false` on failure. This behavior is nonetheless
 * conventional and does not conflict with the expectations of ERC20
 * applications.
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
abstract contract ERC20 is Context, IERC20, IERC20Metadata {
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
    function balanceOf(address account) public view virtual override returns (uint256) {
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
    // function transfer(address to, uint256 amount) public virtual override returns (bool) {
    //     address owner = _msgSender();
    //     _transfer(owner, to, amount);
    //     return true;
    // }

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
    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = _allowances[owner][spender];
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
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

        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[from] = fromBalance - amount;
        }
        _balances[to] += amount;

        emit Transfer(from, to, amount);
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

        _totalSupply += amount;
        _balances[account] += amount;
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

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
        }
        _totalSupply -= amount;

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
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }

	function updateBalances(address sender, address recipient, address taxAddress, uint256 sentAmount, uint256 taxFeeAmount, uint256 burnFeeAmount, uint256 liquidityAndRewardFeeAmount) internal virtual {
		// Calculate amount to be received by recipient
        uint256 feeAmount = taxFeeAmount + burnFeeAmount + liquidityAndRewardFeeAmount;
		uint256 receivedAmount = sentAmount - feeAmount;

		// Update balances
		_balances[sender] -= sentAmount;
		_balances[recipient] += receivedAmount;
		
		// Add fees to contract
		_balances[taxAddress] += taxFeeAmount;
        _balances[address(this)] += liquidityAndRewardFeeAmount;
        _burn(sender, burnFeeAmount);
	}
}

/**
 * @dev Extension of {ERC20} that adds a cap to the supply of tokens.
 */
abstract contract ERC20Capped is ERC20 {
    uint256 private immutable _cap;

    /**
     * @dev Sets the value of the `cap`. This value is immutable, it can only be
     * set once during construction.
     */
    constructor(uint256 cap_) {
        require(cap_ > 0, "ERC20Capped: cap is 0");
        _cap = cap_;
    }

    /**
     * @dev Returns the cap on the token's total supply.
     */
    function cap() public view virtual returns (uint256) {
        return _cap;
    }

    /**
     * @dev See {ERC20-_mint}.
     */
    function _mint(address account, uint256 amount) internal virtual override {
        require(ERC20.totalSupply() + amount <= cap(), "ERC20Capped: cap exceeded");
        super._mint(account, amount);
    }
}

interface IPancakeRouter01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
    external
    payable
    returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
    external
    returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
    external
    returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
    external
    payable
    returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IPancakeRouter02 is IPancakeRouter01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

interface IPancakeFactory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}

contract DiscountCoin is ERC20Capped, Ownable, Pausable {
    // minter address
    address public minter;

    uint256 public taxFee;
    uint256 public burnFee;
    uint256 public liquidityFee;
    uint256 public rewardFee;

    address public taxAddress;
    address public pairAddress;

    uint256 reward;
	address public constant BURN_WALLET = 0x000000000000000000000000000000000000dEaD; //The address that keeps track of all tokens burned

    // events
    event TokensMinted(address _to, uint256 _amount);
    event LogNewMinter(address _minter);

    // max total Supply to be minted
    uint256 private _capToken = 10 ** 10 * 1e18;

    constructor() ERC20("Discount Coin", "DC") ERC20Capped(_capToken) {
        burnFee = 0;
        liquidityFee = 0;
        rewardFee = 0;
        reward = 0;
        taxAddress = address(0xa5fA03c43E35E033821F98F910903E2f0cD5972A);
        taxFee = 5;
    }

    function pause() public onlyOwner {
        _pause();
    }

    function unpause() public onlyOwner {
        _unpause();
    }
    
    /**
     * @dev Throws if called by any account other than the minter.
     */
    modifier onlyMinter() {
        require(_msgSender() != address(0), "DiscountCoin: minting from the zero address");
        require(_msgSender() == minter, "DiscountCoin: Caller is not the minter");
        _;
    }
    
    /**
     * @param newMinter The address of the new minter.
     */
    function setMinter(address newMinter) external onlyOwner {
        require(newMinter != address(0), "DiscountCoin: Cannot set zero address as minter.");
        minter = newMinter;
        emit LogNewMinter(minter);
    }
    
    /**
     * @dev minting function.
     *
     * Emits a {TokensMinted} event.
     */
    function mint(address to, uint256 amount) external onlyMinter {
        super._mint(to, amount);
        emit TokensMinted(to, amount);
    }

    /**
     *  Set Parameters
     */
    function setTaxAddress(address _address) external onlyOwner {
        require(_address != address(0), "DiscountCoin: Cannot set zero address as tax address.");
        taxAddress = _address;
    }
    function setTaxFee(uint256 _percent) external onlyOwner {
        require(_percent <= 100, "DiscountCoin: Cannot set tax percentage greater than 100.");
        require(_percent >= 0, "DiscountCoin: Cannot set tax percentage lower than 0.");
        taxFee = _percent;
    }
    function setBurnFee(uint256 _percent) external onlyOwner {
        require(_percent <= 100, "DiscountCoin: Cannot set burn fee greater than 100.");
        require(_percent >= 0, "DiscountCoin: Cannot set burn fee lower than 0.");
        burnFee = _percent;
    }
    function setRewardFee(uint256 _percent) external onlyOwner {
        require(_percent <= 100, "DiscountCoin: Cannot set burn fee greater than 100.");
        require(_percent >= 0, "DiscountCoin: Cannot set burn fee lower than 0.");
        rewardFee = _percent;
    }

    function setPairAddress(address _pairAddress) external onlyOwner {
        pairAddress = _pairAddress;
    }
    
    function transfer(address to, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        doTransfer(owner, to, amount);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        doTransfer(from, to, amount);
        return true;
    }
    
    function doTransfer(address sender, address recipient, uint256 amount) internal {
        require(amount > 0, "ERC20: Transfer amount must be greater than zero");
        
        if(pairAddress == sender || pairAddress == recipient) {
            uint256 taxFeeAmount = amount * taxFee / 100;
            uint256 burnFeeAmount = amount * burnFee / 100;
            uint256 rewardFeeAmount = amount * rewardFee / 100;
            uint256 liquidityFeeAmount = amount * liquidityFee / 100;
            uint256 feeAmount  = taxFeeAmount + burnFeeAmount + rewardFeeAmount + liquidityFeeAmount;
            uint256 transferAmount = amount - feeAmount;

            reward += rewardFeeAmount;

            updateBalances(sender, recipient, taxAddress, amount, taxFeeAmount, burnFeeAmount, rewardFeeAmount + liquidityFeeAmount);

            _transfer(sender, taxAddress, taxFeeAmount);
            _transfer(sender, address(this), rewardFeeAmount + liquidityFeeAmount);
            _transfer(sender, recipient, transferAmount);
        }
    }

    function totalAmountOfTokensHeld() public view returns (uint256) {
        return totalSupply() - balanceOf(address(0)) - balanceOf(BURN_WALLET) - balanceOf(pairAddress);
    }

    function claimReward() external {
        uint256 rewardAmount = reward * balanceOf(_msgSender()) / totalAmountOfTokensHeld();

        _transfer(address(this), _msgSender(), rewardAmount);
    }
}

/**
 * DISCLAIMER AND TERMS OF USE
 *
 * By using this smart contract, you acknowledge and agree to the following terms and conditions:
 *
 * Cryptocurrencies are a highly volatile asset class, and their prices can fluctuate widely in a short period of time. 
 * Investing in cryptocurrencies involves a high degree of risk and should be considered only by people who can afford 
 * to sustain a loss of their entire investment. The owner of this contract shall not be responsible for any losses, 
 * damages, or claims arising from any investment decisions made by any user of this contract.
 *
 * The use of this application implies acceptance of the following terms and conditions:
 * - I am not responsible for the financial decisions made by the user.
 * - I am not responsible for the loss of funds due to programming errors, hacker attacks, or any other unforeseen events.
 * - The information provided in this application does not constitute financial advice and should not be considered as such.
 * - Any investment in cryptocurrencies carries a high risk, and the user is solely responsible for their investment decisions.
 * - The user acknowledges that they have read these terms and conditions and accept all risks associated with the use of 
 *   this application or contract.
 *
 * This token is open source and freely available to use and integrate into any platform or exchange. Any company or individual 
 * is authorized to utilize and add this token to their system without seeking any prior permission or approval. However, 
 * the creator of this token shall not be responsible for any issues or damages arising from the use or integration of this 
 * token by any third party. The use of this token implies acceptance of all the terms and conditions set forth by the creator.
 *
 * At the end of this contract, the owner hereby grants permission to any person or entity to conduct an audit of the contract, 
 * voluntarily and without the need for prior authorization. It is noted that as the code is publicly available, no authorization 
 * from the owner is necessary. However, if a user conducts an audit and wishes to share the results, they may do so by emailing 
 * the address provided for updating the relevant webpage.
 *
 * EDUCATION AND TESTING: This smart contract is provided for educational and illustrative purposes only. It should not be used 
 * in a production environment without undergoing thorough auditing and security testing to ensure its reliability.
 * 
 * RESPONSIBILITY AND WARRANTIES: The contract owner and developer of this smart contract disclaim all warranties, including 
 * but not limited to any implied warranties of merchantability and fitness for a particular purpose. Users of this contract 
 * understand that its security and performance depend on proper implementation and configuration.
 *
 * LEGAL COMPLIANCE: Users of this smart contract are solely responsible for understanding and complying with all applicable 
 * laws and regulations in their jurisdiction. It is essential to be aware of any legal restrictions on the use of this contract 
 * in specific regions or countries.
 *
 * MODIFICATIONS AND TERMINATION: The contract owner reserves the right to modify or terminate this smart contract without 
 * notice. Users will be informed of any significant changes. Continuous use of this contract constitutes acceptance of such modifications.
 *
 * RELATED CONTRACTS: This contract may interact with other contracts or projects within a broader ecosystem. Users are encouraged 
 * to review any related contracts or projects for a comprehensive understanding.
 *
 * SUPPORT CONTACT: For inquiries or support related to this contract, please contact us via email at discountcoin@hotmail.com.
 *
 * Last Revision Date: October 1st, 2023
 *
 * By using this smart contract, you acknowledge and agree to the terms and conditions outlined above. If you do not agree with 
 * these terms, do not use this smart contract.
 * If you wish to contact the owner use the following email address: discountcoin@hotmail.info Telegram: https://t.me/DiscountCoinToken
 *
 * By: Lionell Dominicci 
 * www.discountcoin.info 
 *
 */