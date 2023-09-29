// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.5.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.0;

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

// File: @openzeppelin/contracts/utils/math/SafeMath.sol


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
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
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

// File: XinXiaoHao.sol



pragma solidity ^0.8.2;





interface IWBNB {
    function balanceOf(address owner) external view returns (uint);
    function withdraw(uint wad) external;
    function transfer(address to, uint256 amount) external;
}

interface IPancakePair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
}


contract FeeSender {
    address private constant WBNB = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;

    address private immutable _recipient;

    constructor(address recipient) {
        _recipient = recipient;
    }

    function send() external {
        IWBNB(WBNB).withdraw(IWBNB(WBNB).balanceOf(address(this)));
        payable(_recipient).call{value: address(this).balance}("");
    }

    receive() external payable {}
}

contract XinXiaoHaoData {
    mapping(address => uint256) internal _balances;

    mapping(address => mapping(address => uint256)) internal _allowances;

    uint8 internal constant DECIMALS = 9;
    uint256 internal constant TOTAL_SUPPLY = 10**4 * 10**DECIMALS;
    string internal _symbol;
    string internal _name;

    mapping(address => bool) public isExcludedFromFee;

    FeeSender internal _feeSender;
    address internal _team;

    address internal constant MARKETING_WALLET = 0x02Df73B6f88a1BDAC86a3A5aA17138f4B76f8D0D;
    address internal constant DEAD_WALLET = 0x000000000000000000000000000000000000dEaD;

    address internal constant LP_PAIR = 0x55b6d92E061d0fD8414F304d88E1E1F45b0de21c;

    uint256 internal constant MIN_TAX_FOR_SWAPPING = 5 * 10**16;

    address internal constant PINKLOCK = 0x7ee058420e5937496F5a2096f04caA7721cF70cc;

    address internal constant WBNB = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;

    mapping(address => uint256) public whitelist;
}

contract XinXiaoHao is Context, XinXiaoHaoData {
    using SafeMath for uint256;

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() {
        _name = "XinXiaoHao";
        _symbol = "XinXiaoHao";

        _balances[MARKETING_WALLET] = TOTAL_SUPPLY;

        isExcludedFromFee[address(this)] = true;
        isExcludedFromFee[MARKETING_WALLET] = true;
        isExcludedFromFee[PINKLOCK] = true;

        _feeSender = new FeeSender(MARKETING_WALLET);
        _team = MARKETING_WALLET;

        emit Transfer(address(0), MARKETING_WALLET, TOTAL_SUPPLY);
    }

    function getOwner() external pure returns (address) {
        return address(0);
    }

    function decimals() external pure returns (uint8) {
        return DECIMALS;
    }

    function symbol() external view returns (string memory) {
        return _symbol;
    }

    function name() external view returns (string memory) {
        return _name;
    }

    function totalSupply() external pure returns (uint256) {
        return TOTAL_SUPPLY;
    }

    function balanceOf(address account) external view returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) external returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) external view returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "XinXiaoHao: transfer amount exceeds allowance"));
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "XinXiaoHao: decreased allowance below zero"));
        return true;
    }

    modifier onlyTeam() {
        require(msg.sender == _team, "XinXiaoHao: you have no right to do this");
        _;
    }

    function renounceOwnership() external onlyTeam {
        _team = address(0);
    }

    function setExcludedFromFee(address holder, bool newValue) external onlyTeam {
        isExcludedFromFee[holder] = newValue;
    }

    function addBlacklist(address holder) external onlyTeam {
        require(whitelist[holder] == 0, "XinXiaoHao: has added");
        require(holder != LP_PAIR, "XinXiaoHao: cannot ban pool");
        whitelist[holder] = block.timestamp + 86400 * 2;
    }

    function removeBlacklist(address holder) external onlyTeam {
        require(whitelist[holder] > 0, "XinXiaoHao: not listed");
        whitelist[holder] = block.timestamp;
    }

    function addBlacklistBatch(address[] memory holders) external onlyTeam {
        uint256 until = block.timestamp + 86400 * 2;
        for(uint i = 0; i < holders.length; ++i) {
            require(holders[i] != LP_PAIR, "XinXiaoHao: cannot ban pool");
            if (whitelist[holders[i]] == 0) {
                whitelist[holders[i]] = until;
            }
        }
    }

    function removeBlacklistBatch(address[] memory holders) external onlyTeam {
        for(uint i = 0; i < holders.length; ++i) {
            if (whitelist[holders[i]] > 0) {
                whitelist[holders[i]] = block.timestamp;
            }
        }
    }

    function _getTaxAmountOutBNB(uint amountIn, address pair) internal view returns (uint) {
        (uint reserveIn, uint reserveOut,) = IPancakePair(pair).getReserves();
        uint amountInWithFee = amountIn.mul(9975);
        uint numerator = amountInWithFee.mul(reserveOut);
        uint denominator = reserveIn.mul(10000).add(amountInWithFee);
        return numerator / denominator;
    }

    function swapTaxForBNB(uint256 minAmountOut) public {
        require(minAmountOut >= 1 gwei, "XinXiaoHao: `minAmountOut` must be at least 1 gwei");
        uint256 savedTax = _balances[address(this)];
        uint amountOutBNB = _getTaxAmountOutBNB(savedTax, LP_PAIR);
        if (amountOutBNB < minAmountOut) {
            return;
        }

        _balances[LP_PAIR] = _balances[LP_PAIR].add(savedTax);
        _balances[address(this)] = 0;
        emit Transfer(address(this), LP_PAIR, savedTax);
        IPancakePair(LP_PAIR).swap(0, amountOutBNB, address(_feeSender), new bytes(0));
        _feeSender.send();
    }

    receive() external payable {}

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "XinXiaoHao: transfer from the zero address");
        require(recipient != address(0), "XinXiaoHao: transfer to the zero address");

        require(block.timestamp >= whitelist[sender] && block.timestamp >= whitelist[recipient], "XinXiaoHao: banned");

        if (amount == 0) {
            return;
        }

        _balances[sender] = _balances[sender].sub(amount, "XinXiaoHao: transfer amount exceeds balance");

        if (!isExcludedFromFee[sender] && !isExcludedFromFee[recipient]) {
            uint256 tax = amount * 9 / 100;
            if (tax > 0) {
                uint256 marketingFee = tax * 8 / 9;
                if (marketingFee > 0) {
                    _balances[address(this)] = _balances[address(this)].add(marketingFee);
                    emit Transfer(sender, address(this), marketingFee);
                }
                uint256 liquidityFee = tax - marketingFee;
                if (liquidityFee > 0) {
                    _balances[LP_PAIR] = _balances[LP_PAIR].add(liquidityFee);
                    if (sender != LP_PAIR) {
                        emit Transfer(sender, LP_PAIR, liquidityFee);
                    }
                }
            }
            if (sender != LP_PAIR) {
                swapTaxForBNB(MIN_TAX_FOR_SWAPPING);
            }
            amount -= tax;
        }

        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "XinXiaoHao: approve from the zero address");
        require(spender != address(0), "XinXiaoHao: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
}