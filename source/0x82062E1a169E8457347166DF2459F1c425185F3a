// File: @openzeppelin/contracts/utils/math/SafeMath.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/math/SafeMath.sol)

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
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
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
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}

// File: @openzeppelin/contracts/utils/Context.sol


// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)

pragma solidity ^0.8.0;

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

// File: @openzeppelin/contracts/access/Ownable.sol


// OpenZeppelin Contracts (last updated v4.9.0) (access/Ownable.sol)

pragma solidity ^0.8.0;


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
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
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

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.0;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
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
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

// File: Testsing.sol


pragma solidity ^0.8.18;




// Dex Factory contract interface
interface IDexFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

// Dex Router contract interface
interface IDexRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external payable returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

contract TESTING is Context, IERC20, Ownable {
    using SafeMath for uint256;

    string private _name = "TESTING";
    string private _symbol = "TST";
    uint8 private _decimals = 18;
    uint256 private _totalSupply = 20_000_000 * 1e18;
    uint256 private _maxSupply = 20_000_000 * 1e18; // Maximum supply that can be minted

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    // Fee-related mappings
    mapping(address => bool) public isExcludedFromFee;
    mapping(address => bool) public isExcludedFromMaxTxn;
    mapping(address => bool) public isExcludedFromMaxHolding;

    // Fee settings
    uint256 public minTokenToSwap = (_totalSupply * 5) / 10000; // Minimum tokens to trigger swap and distribute
    uint256 public maxHoldLimit = (_totalSupply * 2) / 100; // Maximum wallet holding limit
    uint256 public maxTxnLimit = (_totalSupply * 2) / 100; // Maximum transaction limit
    uint256 public percentDivider = 100;
    uint256 public launchedAt;

    uint256 private initialRevenueFee = 5; // Initial 5% revenue fee on buying and selling
    uint256 public revenueFeeOnBuying = initialRevenueFee; // 5% revenue fee on buying
    uint256 public revenueFeeOnSelling = initialRevenueFee; // 5% revenue fee on selling

    // Status flags
    bool public distributeAndLiquifyStatus; // Turn on to liquidate the pool
    bool public feesStatus = true; // Enable by default
    bool public trading;

    // Dex Router and Pair
    IDexRouter public dexRouter;
    address public dexPair;

    // Wallets
    address private marketingWallet;
    address private revWallet;
    address private teamWallet;

    event SwapAndLiquify(uint256 tokensSwapped, uint256 ethReceived, uint256 tokensIntoLiquidity);

    constructor() {
        _balances[owner()] = _totalSupply;

        marketingWallet = address(0x03ce2F4c356f058c705211f2640BBf4f2fD6ed6a);
        revWallet = address(0x03ce2F4c356f058c705211f2640BBf4f2fD6ed6a);
        teamWallet = address(0x03ce2F4c356f058c705211f2640BBf4f2fD6ed6a);

        dexRouter = IDexRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        isExcludedFromFee[address(dexRouter)] = true;
        isExcludedFromMaxTxn[address(dexRouter)] = true;
        isExcludedFromMaxHolding[address(dexRouter)] = true;

        dexPair = IDexFactory(dexRouter.factory()).createPair(address(this), dexRouter.WETH());
        isExcludedFromMaxHolding[dexPair] = true;

        isExcludedFromFee[owner()] = true; // Excluding owner from fees
        isExcludedFromFee[address(this)] = true;
        isExcludedFromFee[address(dexPair)] = true;
        isExcludedFromFee[address(0xdead)] = true;
        isExcludedFromFee[marketingWallet] = true; // Excluding marketingWallet from fees
        isExcludedFromFee[revWallet] = true; // Excluding revWallet from fees
        isExcludedFromFee[teamWallet] = true; // Excluding teamWallet from fees

        isExcludedFromMaxTxn[owner()] = true; // Excluding owner from max txn
        isExcludedFromMaxTxn[address(this)] = true;
        isExcludedFromMaxTxn[address(dexPair)] = true;
        isExcludedFromMaxTxn[address(0xdead)] = true;
        isExcludedFromMaxTxn[marketingWallet] = true; // Excluding marketingWallet from max txn
        isExcludedFromMaxTxn[revWallet] = true; // Excluding revWallet from max txn
        isExcludedFromMaxTxn[teamWallet] = true; // Excluding teamWallet from max txn

        isExcludedFromMaxHolding[owner()] = true; // Excluding owner from max holding
        isExcludedFromMaxHolding[address(this)] = true;
        isExcludedFromMaxHolding[address(dexPair)] = true;
        isExcludedFromMaxHolding[address(0xdead)] = true;
        isExcludedFromMaxHolding[marketingWallet] = true; // Excluding marketingWallet from max holding
        isExcludedFromMaxHolding[revWallet] = true; // Excluding revWallet from max holding
        isExcludedFromMaxHolding[teamWallet] = true; // Excluding teamWallet from max holding

        emit Transfer(address(0), owner(), _totalSupply);
    }

    receive() external payable {}

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function maxSupply() public view returns (uint256) {
        return _maxSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function circulationSupply() public view returns (uint256) {
        return _totalSupply.sub(_balances[address(0)]);
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount));
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue));
        return true;
    }

    function includeOrExcludeFromFee(address account, bool value) external onlyOwner {
        isExcludedFromFee[account] = value;
    }

    function includeOrExcludeFromMaxTxn(address account, bool value) external onlyOwner {
        isExcludedFromMaxTxn[account] = value;
    }

    function includeOrExcludeFromMaxHolding(address account, bool value) external onlyOwner {
        isExcludedFromMaxHolding[account] = value;
    }

    function setMinTokenToSwap(uint256 _amount) external onlyOwner {
        require(_amount <= (_totalSupply * 5) / 10000, "Amount too big");
        minTokenToSwap = _amount * 1e18;
    }

    function setMaxHoldLimit(uint256 _amount) external onlyOwner {
        require(_amount >= (_totalSupply * 2) / 100, "Amount too low");
        maxHoldLimit = _amount * 1e18;
    }

    function setMaxTxnLimit(uint256 _amount) external onlyOwner {
        require(_amount >= (_totalSupply * 2) / 100, "Amount too low");
        maxTxnLimit = _amount * 1e18;
    }


    function setRevenueFeeOnBuying(uint256 _revenueFee) external onlyOwner {
        require(_revenueFee <= 5, "Fee can't be set over 5%");
        revenueFeeOnBuying = _revenueFee;
    }

    function setRevenueFeeOnSelling(uint256 _revenueFee) external onlyOwner {
        require(_revenueFee <= 5, "Fee can't be set over 5%");
        revenueFeeOnSelling = _revenueFee;
    }

    function resetFeesToInitial() external onlyOwner {
        revenueFeeOnBuying = initialRevenueFee;
        revenueFeeOnSelling = initialRevenueFee;
    }

    function setDistributionStatus(bool _value) public onlyOwner {
        distributeAndLiquifyStatus = _value;
    }    

    function enableOrDisableFees(bool _value) external onlyOwner {
        feesStatus = _value;
    }

    function updateAddresses(address _marketingWallet, address _revWallet, address _teamWallet) external onlyOwner {
        marketingWallet = _marketingWallet;
        revWallet = _revWallet;
        teamWallet = _teamWallet;
    }

    function enableTrading() external onlyOwner {
        require(!trading, "Trading is already enabled");

        trading = true;
        feesStatus = true;
        distributeAndLiquifyStatus = true;
        launchedAt = block.timestamp;
    }

    function removeStuckEth(address _receiver) public onlyOwner {
        payable(_receiver).transfer(address(this).balance);
    }

    function totalBuyFeePerTx(uint256 amount) public view returns (uint256) {
        uint256 fee = (amount * revenueFeeOnBuying) / percentDivider;
        return fee;
    }

    function totalSellFeePerTx(uint256 amount) public view returns (uint256) {
        uint256 fee = (amount * revenueFeeOnSelling) / percentDivider;
        return fee;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(address from, address to, uint256 amount) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "ERC20: Amount must be greater than zero");

        if (!isExcludedFromMaxTxn[from] && !isExcludedFromMaxTxn[to]) {
            require(amount <= maxTxnLimit, "ERC20: Exceeds maximum transaction limit");

            // Disable trading until launch
            if (!trading) {
                require(dexPair != from && dexPair != to, "Trading is disabled");
            }
        }

        if (!isExcludedFromMaxHolding[to]) {
            require((_balances[to] + amount) <= maxHoldLimit, "Exceeds maximum holding limit");
        }

        // Swap and liquify
        distributeAndLiquify(from, to);

        // Indicates if fee should be deducted from transfer
        bool takeFee = true;

        // If any account belongs to isExcludedFromFee account then remove the fee
        if (isExcludedFromFee[from] || isExcludedFromFee[to] || !feesStatus) {
            takeFee = false;
        }

        // Transfer amount, it will take tax, burn, and liquidity fee
        _tokenTransfer(from, to, amount, takeFee);
    }

    // This method is responsible for taking all fees, if takeFee is true
    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 amount,
        bool takeFee
    ) private {
        if (dexPair == sender && takeFee) {
            uint256 allFee;
            uint256 tTransferAmount;
            allFee = totalBuyFeePerTx(amount);
            tTransferAmount = amount - allFee;
            _balances[sender] = _balances[sender] - amount;
            _balances[recipient] = _balances[recipient] + tTransferAmount;
            emit Transfer(sender, recipient, tTransferAmount);
            takeTokenFee(sender, allFee);
        } else if (dexPair == recipient && takeFee) {
            uint256 allFee = totalSellFeePerTx(amount);
            uint256 tTransferAmount = amount - allFee;
            _balances[sender] = _balances[sender] - amount;
            _balances[recipient] = _balances[recipient] + tTransferAmount;
            emit Transfer(sender, recipient, tTransferAmount);
            takeTokenFee(sender, allFee);
        } else {
            _balances[sender] = _balances[sender] - amount;
            _balances[recipient] = _balances[recipient] + (amount);
            emit Transfer(sender, recipient, amount);
        }
    }

    function takeTokenFee(address sender, uint256 amount) private {
        _balances[address(this)] = _balances[address(this)] + amount;
        emit Transfer(sender, address(this), amount);
    }

    function withdrawETH(uint256 _amount) external onlyOwner {
        require(address(this).balance >= _amount, "Insufficient ETH balance");
        payable(msg.sender).transfer(_amount);
    }

    function withdrawToken(IERC20 _token, uint256 _amount) external onlyOwner {
        require(_token.balanceOf(address(this)) >= _amount, "Insufficient token balance");
        _token.transfer(msg.sender, _amount);
    }

    function distributeAndLiquify(address from, address to) private {
        uint256 contractTokenBalance = balanceOf(address(this));

        bool shouldSell = contractTokenBalance >= minTokenToSwap;

        if (
            shouldSell &&
            from != dexPair &&
            distributeAndLiquifyStatus &&
            !(from == address(this) && to == dexPair)
        ) {
            // Approve contract
            _approve(address(this), address(dexRouter), minTokenToSwap);

            // Lock into liquidity pool
            Utils.swapTokensForEth(address(dexRouter), minTokenToSwap);
            uint256 ethFromFees = address(this).balance;

            // Distribute ETH to wallets
            if (ethFromFees > 0) {
                uint256 revShare = (ethFromFees * 40) / 100; // 40% for revenue wallet
                uint256 teamShare = (ethFromFees * 30) / 100; // 30% for team wallet
                uint256 marketingShare = (ethFromFees * 30) / 100; // 30% for marketing wallet

                payable(revWallet).transfer(revShare);
                payable(teamWallet).transfer(teamShare);
                payable(marketingWallet).transfer(marketingShare);
            }
        }
    }

    // Function to remove all limits
    function removeLimits() external onlyOwner {
        minTokenToSwap = 0;
        maxHoldLimit = _totalSupply;
        maxTxnLimit = _totalSupply;
    }
}

// Library for swapping on Dex
library Utils {
    function swapTokensForEth(address routerAddress, uint256 tokenAmount) internal {
        IDexRouter dexRouter = IDexRouter(routerAddress);

        // Generate the Dex pair path of token -> WETH
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = dexRouter.WETH();

        // Make the swap
        dexRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // Accept any amount of ETH
            path,
            address(this),
            block.timestamp + 300
        );
    }

    function addLiquidity(address routerAddress, address owner, uint256 tokenAmount, uint256 ethAmount) internal {
        IDexRouter dexRouter = IDexRouter(routerAddress);

        // Add the liquidity
        dexRouter.addLiquidityETH{value: ethAmount}(
            address(this),
            tokenAmount,
            0, // Slippage is unavailable
            0, // Slippage is unavailable
            owner,
            block.timestamp + 300
        );
    }
}