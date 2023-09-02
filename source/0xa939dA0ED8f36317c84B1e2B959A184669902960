/**
 *Submitted for verification at BscScan.com on 2023-08-24
*/

// Sources flattened with hardhat v2.13.0 https://hardhat.org

// File contracts/RPAXSuper.sol

// SPDX-License-Identifier: MIT
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

// File @openzeppelin/contracts/access/Ownable.sol@v4.8.2

// OpenZeppelin Contracts (last updated v4.7.0) (access/Ownable.sol)

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

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

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
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
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

// File @uniswap/v2-core/contracts/interfaces/IUniswapV2Pair.sol@v1.0.1

pragma solidity >=0.5.0;

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

// File contracts/math/SafeMath.sol

pragma solidity >=0.4.0;

/**
 * @dev Wrappers over Solidity's arithmetic operations with added overflow
 * checks.
 *
 * Arithmetic operations in Solidity wrap on overflow. This can easily result
 * in bugs, because programmers usually assume that an overflow raises an
 * error, which is the standard behavior in high level programming languages.
 * `SafeMath` restores this intuition by reverting the transaction when an
 * operation overflows.
 *
 * Using this library instead of the unchecked operations eliminates an entire
 * class of bugs, so it's recommended to use it always.
 */
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

    function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
        z = x < y ? x : y;
    }

    // babylonian method (https://en.wikipedia.org/wiki/Methods_of_computing_square_roots#Babylonian_method)
    function sqrt(uint256 y) internal pure returns (uint256 z) {
        if (y > 3) {
            z = y;
            uint256 x = y / 2 + 1;
            while (x < z) {
                z = x;
                x = (y / x + x) / 2;
            }
        } else if (y != 0) {
            z = 1;
        }
    }
}

// File contracts/libraries/UniswapV2Library.sol

pragma solidity >=0.5.0;

library UniswapV2Library {
    using SafeMath for uint256;

    // returns sorted token addresses, used to handle return values from pairs sorted in this order
    function sortTokens(address tokenA, address tokenB)
        internal
        pure
        returns (address token0, address token1)
    {
        require(tokenA != tokenB, "UniswapV2Library: IDENTICAL_ADDRESSES");
        (token0, token1) = tokenA < tokenB
            ? (tokenA, tokenB)
            : (tokenB, tokenA);
        require(token0 != address(0), "UniswapV2Library: ZERO_ADDRESS");
    }

    // calculates the CREATE2 address for a pair without making any external calls
    function pairFor(
        address factory,
        address tokenA,
        address tokenB
    ) internal pure returns (address pair) {
        (address token0, address token1) = sortTokens(tokenA, tokenB);
        pair = address(
            uint160(
                uint256(
                    keccak256(
                        abi.encodePacked(
                            hex"ff",
                            factory,
                            keccak256(abi.encodePacked(token0, token1)),
                            hex"00fb7f630766e6a796048ea87d01acd3068e8ff67d078148a3fa3f4a84f69bd5" // init code hash
                        )
                    )
                )
            )
        );
    }

    // fetches and sorts the reserves for a pair
    function getReserves(
        address factory,
        address tokenA,
        address tokenB
    ) internal view returns (uint256 reserveA, uint256 reserveB) {
        (address token0, ) = sortTokens(tokenA, tokenB);
        (uint256 reserve0, uint256 reserve1, ) = IUniswapV2Pair(
            pairFor(factory, tokenA, tokenB)
        ).getReserves();
        (reserveA, reserveB) = tokenA == token0
            ? (reserve0, reserve1)
            : (reserve1, reserve0);
    }

    // given some amount of an asset and pair reserves, returns an equivalent amount of the other asset
    function quote(
        uint256 amountA,
        uint256 reserveA,
        uint256 reserveB
    ) internal pure returns (uint256 amountB) {
        require(amountA > 0, "UniswapV2Library: INSUFFICIENT_AMOUNT");
        require(
            reserveA > 0 && reserveB > 0,
            "UniswapV2Library: INSUFFICIENT_LIQUIDITY"
        );
        amountB = amountA.mul(reserveB) / reserveA;
    }

    // given an input amount of an asset and pair reserves, returns the maximum output amount of the other asset
    function getAmountOut(
        uint256 amountIn,
        uint256 reserveIn,
        uint256 reserveOut
    ) internal pure returns (uint256 amountOut) {
        require(amountIn > 0, "UniswapV2Library: INSUFFICIENT_INPUT_AMOUNT");
        require(
            reserveIn > 0 && reserveOut > 0,
            "UniswapV2Library: INSUFFICIENT_LIQUIDITY"
        );
        uint256 amountInWithFee = amountIn.mul(997);
        uint256 numerator = amountInWithFee.mul(reserveOut);
        uint256 denominator = reserveIn.mul(1000).add(amountInWithFee);
        amountOut = numerator / denominator;
    }

    // given an output amount of an asset and pair reserves, returns a required input amount of the other asset
    function getAmountIn(
        uint256 amountOut,
        uint256 reserveIn,
        uint256 reserveOut
    ) internal pure returns (uint256 amountIn) {
        require(amountOut > 0, "UniswapV2Library: INSUFFICIENT_OUTPUT_AMOUNT");
        require(
            reserveIn > 0 && reserveOut > 0,
            "UniswapV2Library: INSUFFICIENT_LIQUIDITY"
        );
        uint256 numerator = reserveIn.mul(amountOut).mul(1000);
        uint256 denominator = reserveOut.sub(amountOut).mul(997);
        amountIn = (numerator / denominator).add(1);
    }

    // performs chained getAmountOut calculations on any number of pairs
    function getAmountsOut(
        address factory,
        uint256 amountIn,
        address[] memory path
    ) internal view returns (uint256[] memory amounts) {
        require(path.length >= 2, "UniswapV2Library: INVALID_PATH");
        amounts = new uint256[](path.length);
        amounts[0] = amountIn;
        for (uint256 i; i < path.length - 1; i++) {
            (uint256 reserveIn, uint256 reserveOut) = getReserves(
                factory,
                path[i],
                path[i + 1]
            );
            amounts[i + 1] = getAmountOut(amounts[i], reserveIn, reserveOut);
        }
    }

    // performs chained getAmountIn calculations on any number of pairs
    function getAmountsIn(
        address factory,
        uint256 amountOut,
        address[] memory path
    ) internal view returns (uint256[] memory amounts) {
        require(path.length >= 2, "UniswapV2Library: INVALID_PATH");
        amounts = new uint256[](path.length);
        amounts[amounts.length - 1] = amountOut;
        for (uint256 i = path.length - 1; i > 0; i--) {
            (uint256 reserveIn, uint256 reserveOut) = getReserves(
                factory,
                path[i - 1],
                path[i]
            );
            amounts[i - 1] = getAmountIn(amounts[i], reserveIn, reserveOut);
        }
    }
}

// File @openzeppelin/contracts/security/ReentrancyGuard.sol@v4.8.2

// OpenZeppelin Contracts (last updated v4.8.0) (security/ReentrancyGuard.sol)

pragma solidity ^0.8.0;

/**
 * @dev Contract module that helps prevent reentrant calls to a function.
 *
 * Inheriting from `ReentrancyGuard` will make the {nonReentrant} modifier
 * available, which can be applied to functions to make sure there are no nested
 * (reentrant) calls to them.
 *
 * Note that because there is a single `nonReentrant` guard, functions marked as
 * `nonReentrant` may not call one another. This can be worked around by making
 * those functions `private`, and then adding `external` `nonReentrant` entry
 * points to them.
 *
 * TIP: If you would like to learn more about reentrancy and alternative ways
 * to protect against it, check out our blog post
 * https://blog.openzeppelin.com/reentrancy-after-istanbul/[Reentrancy After Istanbul].
 */
abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _status will be _NOT_ENTERED
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}

// File @openzeppelin/contracts/token/ERC20/IERC20.sol@v4.8.2

// OpenZeppelin Contracts (last updated v4.6.0) (token/ERC20/IERC20.sol)

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
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

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
}

// File contracts/interface/IPancakePair.sol

pragma solidity >=0.5.0;

interface IPancakePair {
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

// File contracts/interface/IRPAX.sol

pragma solidity ^0.8.4;

interface IRPAX {
    function burn(uint256 amount) external;

    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    function balanceOf(address account) external view returns (uint256);

    function uniswapV2PairUSDT() external view returns (address);
}

// File contracts/RPAXSuper.sol

pragma solidity 0.8.19;

contract RPAXSuper is ReentrancyGuard, Ownable {
    using SafeMath for uint256;
    IRPAX public immutable rpax;
    uint256 private constant LOCK_UP_AMOUNT = 24 * 10**21;
    uint256 private constant MAX_SUPER = 1001;
    uint256 private constant BUY_SUPER_AMOUNT_USDT = 3 * 10**21;
    uint256 private constant RPAX_RATIO = 1 * 10**18;
    uint256 private constant LIQUID_PLEDGE = 5 * 10**20;
    uint256 private constant RELEASE_PER_WEEK = 10;
    uint256 private constant RELEASE_SUPER_COUNT = 99;
    uint256 private constant WEEK = 7 days;
    uint256 private constant DAY = 1 days;
    address public constant USDT = 0x55d398326f99059fF775485246999027B3197955;
    address private constant DESTROY =
        0x000000000000000000000000000000000000dEaD;
    uint256 private constant USDT_PRICE = 5 * 10**20;
    uint256 private constant ONE_SUPERNUM = 200;
    uint256 private constant ONE_PLEDGENUM = 50;
    uint256 private constant ONE_FREENUM = 200;

    struct SuperNodeInfo {
        uint256 startTime;
        bool isSuperNode;
    }
    mapping(address => SuperNodeInfo) public superNode;
    address[] public superNodes;
    mapping(address => uint256) public releaseCount;
    uint256 private rewardPos = 0;
    uint256 public lastTimeOfSuper;
    mapping(address => uint256) public rewardOfUser;
    mapping(address => uint256) public rewardOfUserTotal;

    struct LiquidPledge {
        uint256 startTime;
        uint256 pledgeAmount;
    }
    mapping(address => uint256) public validRecommendPledge;
    mapping(address => LiquidPledge[]) public liquidPledgeNode;
    address[] public liquidPledgeNodes;
    mapping(address => bool) public isLiquidPledge;
    mapping(address => uint256) withdrawAmount;
    uint256 private liquidPos;
    uint256 public onePledgeNum = 50;
    mapping(address => uint256) public withdrawAlreayLiquidPledge;
    mapping(address => address[]) public toSon;
    mapping(address => address) public toFather;
    uint256 public lastTimeOfPledge;
    mapping(address => uint256) public pledgeRewardOfUser;
    mapping(address => uint256) public pledgeRewardOfTotal;

    struct Info {
        address users;
        uint256 amount;
        uint256 flag;
        uint256 time;
    }
    mapping(address => Info[]) public pledgeRewardOfInfo;
    mapping(address => uint256) public pledgeRatioOfUser;
    mapping(address => address[]) public toSonOfFree;
    mapping(address => address) public toFatherOfFree;
    address[] public freeAddresses;
    mapping(address => bool) public isFreeNode;
    uint256 private freePos;
    uint256 public lastTimeOfFree;
    mapping(address => uint256) public freeRewardOfUser;
    mapping(address => uint256) public freeRewardOfUserTotal;
    mapping(address => uint256) public freeRatioOfUser;
    address public fundAddress =
        address(0x2783AC2553d5445D60C43dDBfa76ADeb377101df);
    mapping(address => uint256) public userAlreadyWithdraw;
    mapping(address => uint256) public extraReward;
    uint256 public lastTxTime = 43200;

    address public ownerOne =
        address(0x40861979afbD623Bf7c73c0257BBF916dca42BCc);
    address public ownerTwo =
        address(0x12865ea44E4C2FB075b73581ad049e5a150AEc11);
    address public ownerThree =
        address(0x4eFC84059C74cC8EFA7D9933ea5cEeD88fDC68Cb);
    address public owners = address(0x2c4889bB4399137dF2A19e4703Fd920B32FFb8Ae);

    event BuyLiquidPledge(uint256 indexed rpaxAmount);

    constructor(address _rpax) {
        rpax = IRPAX(_rpax);
    }

    function getSuperNodes() external view returns (uint256) {
        return superNodes.length;
    }

    function getLiquidPledge() external view returns (uint256) {
        return liquidPledgeNodes.length;
    }

    function getLiquidPledgeAmount(address sender) external view returns(bool){
        LiquidPledge[] memory info = liquidPledgeNode[sender];
        uint256 totalAmount;
        uint256 len = info.length;
        for(uint256 i=0; i < len; i++){
            totalAmount += info[i].pledgeAmount;
        }
        if(totalAmount +  withdrawAmount[sender] > 0) return true;
        return false;
    }

    function buySuperNode(address token) external nonReentrant {
        require(tx.origin == msg.sender, "Contract cannot be called");
        require(superNodes.length < MAX_SUPER, "Over max super amount");
        require(!superNode[msg.sender].isSuperNode, "Have already purchased");
        uint256 rpaxRatio = getPrice(RPAX_RATIO);
        uint256 rpaxAmount = rpaxRatio.mul(3000);
        uint256 beforeBalanceOfRPAX = rpax.balanceOf(address(this));
        uint256 beforeBalanceOfUSDT = IERC20(USDT).balanceOf(address(this));
        if (token == address(rpax)) {
            IERC20(token).transferFrom(
                msg.sender,
                address(this),
                rpaxAmount
            );
        } else if (token == USDT) {
            IERC20(token).transferFrom(
                msg.sender,
                address(this),
                BUY_SUPER_AMOUNT_USDT
            );
        }
        uint256 afterBalanceOfRPAX = rpax.balanceOf(address(this));
        uint256 afterBalanceOfUSDT = IERC20(USDT).balanceOf(address(this));
        require(
            ((afterBalanceOfRPAX - beforeBalanceOfRPAX) >=
                rpaxAmount) ||
                ((afterBalanceOfUSDT - beforeBalanceOfUSDT) >=
                    BUY_SUPER_AMOUNT_USDT),
            "Token was sent incorrectly"
        );
        SuperNodeInfo storage info = superNode[msg.sender];
        info.startTime = block.timestamp + WEEK;
        info.isSuperNode = true;
        superNodes.push(msg.sender);
    }

    function setSuperNode(address _superNode) external nonReentrant{
        require(msg.sender == owners,"Invalid owner");
        require(
            !superNode[_superNode].isSuperNode,
            "Super node already existed"
        );
        SuperNodeInfo storage info = superNode[_superNode];
        info.startTime = block.timestamp + WEEK;
        info.isSuperNode = true;
        superNodes.push(_superNode);
    }

    function releaseSuperReward() external nonReentrant {
        require(msg.sender == ownerOne, "Invalid owner");
        uint256 num = (rewardPos + ONE_SUPERNUM > superNodes.length)
            ? superNodes.length - rewardPos
            : ONE_SUPERNUM;
        address[] memory supers = superNodes;
        uint256 usdtAmount = LOCK_UP_AMOUNT.mul(RELEASE_PER_WEEK).div(1000);
       
        uint256 rpaxRatio = getPrice(RPAX_RATIO);
        uint256 usdtAmountRatio = usdtAmount.div(1 ether);
        uint256 rpaxAmount = usdtAmountRatio.mul(rpaxRatio);

        address user;
        for (uint256 i = rewardPos; i < rewardPos + num; i++) {
            user = supers[i];
            SuperNodeInfo memory info = superNode[user];
            if (
                block.timestamp >= info.startTime &&
                releaseCount[user] < RELEASE_SUPER_COUNT
            ) {
                rewardOfUser[user] = rpaxAmount;
                rewardOfUserTotal[user] = rewardOfUserTotal[user] + rpaxAmount;
                releaseCount[user]++;
                SuperNodeInfo storage information = superNode[user];
                information.startTime = information.startTime + WEEK;
            }
        }
        rewardPos += num;
        if (rewardPos >= superNodes.length) {
            rewardPos = 0;
            lastTimeOfSuper = block.timestamp;
        }
    }

    function getRewardOfUser(address[] calldata addrs)
        external
        view
        returns (uint256[] memory, uint256)
    {
        uint256[] memory rewardAmounts = new uint256[](addrs.length);
        for (uint256 i = 0; i < addrs.length; i++) {
            rewardAmounts[i] = rewardOfUser[addrs[i]];
        }
        return (rewardAmounts, lastTimeOfSuper);
    }

    function buyLiquidPledge(
        address recommend,
        address recommended,
        uint256 usdtAmount,
        bool flag
    ) external nonReentrant {
        require(recommended == msg.sender, "Recommended are equal to callers");
        require(tx.origin == msg.sender, "Contract cannot be called");
        require(usdtAmount > 0, "An illegal amount");
        uint256 rpaxRatio = getPrice(RPAX_RATIO);
        uint256 rpaxAmount = rpaxRatio.mul(usdtAmount.div(1 ether));
        LiquidPledge[] storage info = liquidPledgeNode[msg.sender];
        rpax.transferFrom(msg.sender, address(this), rpaxAmount);
        LiquidPledge memory lp = LiquidPledge({
            startTime: block.timestamp,
            pledgeAmount: rpaxAmount
        });
        info.push(lp);
        if (flag && !isLiquidPledge[msg.sender]) {
            if (recommend != address(0)) {
                address[] memory _recommended = toSon[recommend];
                for (uint256 i = 0; i < _recommended.length; i++) {
                    require(
                        _recommended[i] != recommended,
                        "Have been recommended"
                    );
                }
                require(
                    toFather[recommended] == address(0),
                    "Have been recommended"
                );
                toSon[recommend].push(recommended);
                toFather[recommended] = recommend;
            }
            require(usdtAmount >= LIQUID_PLEDGE, "Insufficient amount");
            liquidPledgeNodes.push(msg.sender);
            isLiquidPledge[msg.sender] = true;
        }
        emit BuyLiquidPledge(rpaxAmount);
    }

    function setLiquidPledgeandRecommend(
        address recommend,
        address recommended,
        uint256 rpaxAmount,
        bool flag
    ) external {
        require(msg.sender == owners,"Invalid owners");
        LiquidPledge[] storage info = liquidPledgeNode[recommended];
        LiquidPledge memory lp = LiquidPledge({
            startTime: block.timestamp,
            pledgeAmount: rpaxAmount
        });
        info.push(lp);
        if (flag && !isLiquidPledge[recommended]) {
            liquidPledgeNodes.push(recommended);
            isLiquidPledge[recommended] = true;
        }
        if (recommend == address(0)) return;
        toSon[recommend].push(recommended);
        toFather[recommended] = recommend;
    }

    function liquidPledgeReward() external nonReentrant {
        require(msg.sender == ownerTwo, "Invalid owner");
        uint256 num = (liquidPos + onePledgeNum > liquidPledgeNodes.length)
            ? liquidPledgeNodes.length - liquidPos
            : onePledgeNum;
        address[] memory pledges = liquidPledgeNodes;
        address user;
        uint256 ratio;
        for (uint256 i = liquidPos; i < liquidPos + num; i++) {
            user = pledges[i];
            LiquidPledge[] memory info = liquidPledgeNode[user];
            ratio = _getRatio(user);

            uint256 totalReward;
            uint256 lastTotalReward;
            uint256 totalAmount;
            uint256 superAmount;
            uint256 superOneAmount;
            uint256 twoAmount;
            for (uint256 j = 0; j < info.length; j++) {
                if (block.timestamp - info[j].startTime >= DAY) {
                    totalAmount = totalAmount.add(info[j].pledgeAmount);
                }
            }
            totalReward = totalAmount.add(withdrawAmount[user]);

            if (totalReward == 0) {
                continue;
            }
            lastTotalReward = totalReward.mul(ratio).div(1000);
            superAmount = lastTotalReward.mul(25).div(100);
            (address superNodeOne, address nodeTwo) = _getSuperNode(user);
            superOneAmount = lastTotalReward.mul(20).div(100);

            twoAmount = lastTotalReward.mul(5).div(100);
            if (superNodeOne == address(0)) {
                rpax.transfer(DESTROY, superAmount);
            } else if (superNodeOne != address(0) && nodeTwo == address(0)) {
                Info memory information = Info({
                    users: superNodeOne,
                    amount: superOneAmount,
                    flag: 2,
                    time: block.timestamp
                });
                _setReward(superNodeOne, information);
                pledgeRewardOfTotal[superNodeOne] =
                    pledgeRewardOfTotal[superNodeOne] +
                    superOneAmount;

                rpax.transfer(DESTROY, twoAmount);
            } else if (superNodeOne != address(0) && nodeTwo != address(0)) {
                Info memory information = Info({
                    users: superNodeOne,
                    amount: superOneAmount,
                    flag: 2,
                    time: block.timestamp
                });
                _setReward(superNodeOne, information);
                pledgeRewardOfTotal[superNodeOne] =
                    pledgeRewardOfTotal[superNodeOne] +
                    superOneAmount;

                Info memory infoSuper = Info({
                    users: nodeTwo,
                    amount: twoAmount,
                    flag: 2,
                    time: block.timestamp
                });
                _setReward(nodeTwo, infoSuper);
                pledgeRewardOfTotal[nodeTwo] =
                    pledgeRewardOfTotal[nodeTwo] +
                    twoAmount;
            }

            rpax.transfer(DESTROY, lastTotalReward.mul(15).div(100));
            Info memory infoSupers = Info({
                users: user,
                amount: lastTotalReward.mul(60).div(100),
                flag: 1,
                time: block.timestamp
            });
            _setReward(user, infoSupers);

            pledgeRewardOfTotal[user] =
                pledgeRewardOfTotal[user] +
                lastTotalReward.mul(60).div(100);
            pledgeRatioOfUser[user] = ratio;
        }
        liquidPos += num;
        if (liquidPos >= liquidPledgeNodes.length) {
            liquidPos = 0;
        }
    }

    function _setReward(address node, Info memory info) private {
        uint256 _time = block.timestamp;
        Info[] storage infos = pledgeRewardOfInfo[node];
        if (infos.length == 0) {
            infos.push(info);
            return;
        }
        bool _flag;
        for (uint256 i = 0; i < infos.length; i++) {
            if (_time - infos[i].time > lastTxTime) {
                infos[i] = info;
                _flag = true;
                break;
            }
        }
        if (!_flag) infos.push(info);
    }

    function getPledgeRewardOfUser(address[] calldata addrs)
        external
        view
        returns (
            address[] memory,
            uint256[] memory,
            uint256[] memory,
            uint256[] memory
        )
    {
        uint256 len = addrs.length;

        uint256 totalSize = 0;
        for (uint256 i = 0; i < len; i++) {
            totalSize += pledgeRewardOfInfo[addrs[i]].length;
        }

        uint256[] memory amounts = new uint256[](totalSize);
        uint256[] memory flags = new uint256[](totalSize);
        address[] memory user = new address[](totalSize);

        uint256 index;
        for (uint256 i = 0; i < len; i++) {
            Info[] memory flattened = new Info[](
                pledgeRewardOfInfo[addrs[i]].length
            );
            flattened = pledgeRewardOfInfo[addrs[i]];
            for (uint256 j = 0; j < flattened.length; j++) {
                user[index] = flattened[j].users;
                amounts[index] = flattened[j].amount;
                flags[index] = flattened[j].flag;
                index++;
            }
        }
        uint256[] memory ratios = _getRatios(addrs, totalSize);
        return (user, amounts, flags, ratios);
    }

    function _getRatios(address[] calldata addrs, uint256 _totalSize)
        private
        view
        returns (uint256[] memory)
    {
        uint256[] memory ratios = new uint256[](_totalSize);
        uint256 index;
        for (uint256 i = 0; i < addrs.length; i++) {
            for (uint256 j = 0; j < pledgeRewardOfInfo[addrs[i]].length; j++) {
                ratios[index] = pledgeRatioOfUser[addrs[i]];
                index++;
            }
        }
        return ratios;
    }

    function _getRatio(address user) private returns (uint256) {
        address[] memory sons = toSon[user];
        uint256 recommendAmount;
        uint256 ratio;
        uint256 i;
        for (i = 0; i < sons.length; i++) {
            LiquidPledge[] memory info = liquidPledgeNode[sons[i]];
            if (info.length == 0) continue;
            if (
                withdrawAmount[sons[i]] != 0 ||
                info[info.length - 1].pledgeAmount > 0
            ) {
                recommendAmount++;
            }
        }
        validRecommendPledge[user] = recommendAmount;
        if (recommendAmount == 0) {
            ratio = 15;
        } else if (recommendAmount <= 2) {
            ratio = 16;
        } else if (recommendAmount <= 4) {
            ratio = 17;
        } else if (recommendAmount <= 9) {
            ratio = 18;
        } else if (recommendAmount >= 10) {
            ratio = 20;
        }
        return ratio;
    }

    function _getSuperNode(address user)
        private
        view
        returns (address, address)
    {
        address father = toFather[user];
        address superNodeOne;
        address nodeTwo;
        uint256 i;
        uint256 count;
        while (true) {
            if (father == address(0)) break;
            if (superNode[father].isSuperNode) {
                if (count == 1) {
                    nodeTwo = father;
                    break;
                }
                superNodeOne = father;
                count++;
            } else if (count == 1) {
                break;
            }

            father = toFather[father];
            i++;
        }
        return (superNodeOne, nodeTwo);
    }

    function withdrawLiquidPledge(uint256 _withdrawAmount)
        external
        nonReentrant
    {
        LiquidPledge[] storage info = liquidPledgeNode[msg.sender];
        if (withdrawAmount[msg.sender] < _withdrawAmount) {
            uint256 totalAmount;
            for (uint256 i = 0; i < info.length; i++) {
                if (block.timestamp - info[i].startTime >= DAY) {
                    totalAmount = totalAmount.add(info[i].pledgeAmount);
                    delete info[i];
                }
            }
            withdrawAmount[msg.sender] = withdrawAmount[msg.sender].add(
                totalAmount
            );
            require(
                withdrawAmount[msg.sender] >= _withdrawAmount,
                "Not sufficient funds"
            );
        }
        withdrawAmount[msg.sender] = withdrawAmount[msg.sender].sub(
            _withdrawAmount
        );
        withdrawAlreayLiquidPledge[msg.sender] += _withdrawAmount;
    }

    function canWithdrawOfLiquidPledge(address user)
        public
        view
        returns (uint256)
    {
        uint256 totalAmount;
        LiquidPledge[] memory info = liquidPledgeNode[user];
        for (uint256 i = 0; i < info.length; i++) {
            if (block.timestamp - info[i].startTime >= DAY) {
                totalAmount = totalAmount.add(info[i].pledgeAmount);
            }
        }
        return withdrawAmount[user] + totalAmount;
    }

    function setFreeRecommendandFree(address recommend, address recommended) external{
        for (uint256 i = 0; i < freeAddresses.length; i++) {
            require(freeAddresses[i] != recommended, "FreeNode already exist");
        }
        freeAddresses.push(recommended);
        isFreeNode[recommended] = true;

        if (recommend == address(0)) return;
        require(
            toFatherOfFree[recommended] == address(0),
            "Have been recommended"
        );
        address[] memory _recommended = toSonOfFree[recommend];
        for (uint256 i = 0; i < _recommended.length; i++) {
            require(_recommended[i] != recommended, "Have been recommended");
        }
        toSonOfFree[recommend].push(recommended);
        toFatherOfFree[recommended] = recommend;
    }

    function freePledgeReward() external {
        require(msg.sender == ownerThree, "Invalid owner");
        address[] memory freeNode = freeAddresses;
        uint256 recommendCount;
        address user;
        uint256 ratio;
        uint256 rpaxAmount;
        uint256 rewardAmount;
        uint256 num = (freePos + ONE_FREENUM > freeAddresses.length)
            ? freeAddresses.length - freePos
            : ONE_FREENUM;
        uint256 rpaxRatio = getPrice(RPAX_RATIO);
        rpaxAmount = rpaxRatio.mul(100);
        for (uint256 i = freePos; i < freePos + num; i++) {
            user = freeNode[i];
            recommendCount = getFreeRecommendCount(user);
            ratio = _getFreeRatio(recommendCount);
            rewardAmount = rpaxAmount.mul(ratio).div(1000);
            freeRewardOfUser[user] = rewardAmount;
            freeRewardOfUserTotal[user] =
                freeRewardOfUserTotal[user] +
                rewardAmount;
            freeRatioOfUser[user] = ratio;
        }
        freePos += num;
        if (freePos >= freeAddresses.length) {
            freePos = 0;
            lastTimeOfFree = block.timestamp;
        }
    }

    function getfreeRewardOfUser(address[] calldata addrs)
        external
        view
        returns (
            uint256[] memory,
            uint256[] memory,
            uint256
        )
    {
        uint256[] memory rewardAmounts = new uint256[](addrs.length);
        uint256[] memory ratios = new uint256[](addrs.length);
        for (uint256 i = 0; i < addrs.length; i++) {
            rewardAmounts[i] = freeRewardOfUser[addrs[i]];
            ratios[i] = freeRatioOfUser[addrs[i]];
        }
        return (rewardAmounts, ratios, lastTimeOfFree);
    }

    function getFreeRecommendCount(address _freeNode)
        public
        view
        returns (uint256)
    {
        return toSonOfFree[_freeNode].length;
    }

    function _getFreeRatio(uint256 recommendCount)
        private
        pure
        returns (uint256)
    {
        uint256 ratio;
        if (recommendCount == 0) {
            ratio = 5;
        } else if (recommendCount <= 2) {
            ratio = 6;
        } else if (recommendCount <= 4) {
            ratio = 7;
        } else if (recommendCount <= 9) {
            ratio = 8;
        } else if (recommendCount >= 10) {
            ratio = 10;
        }
        return ratio;
    }

    function withdrawToken(address receiver, uint256 amount)
        public
        returns (bool)
    {
        require(msg.sender == receiver, "Not oneself");
        uint256 totalAmount = extraReward[receiver] +
            freeRewardOfUserTotal[receiver] +
            pledgeRewardOfTotal[receiver] +
            rewardOfUserTotal[receiver];

        userAlreadyWithdraw[receiver] = userAlreadyWithdraw[receiver] + amount;

        require(
            totalAmount - userAlreadyWithdraw[receiver] > 0,
            "Not sufficient funds"
        );
        uint256 trueCanWithdraw = totalAmount - userAlreadyWithdraw[receiver];
        require(trueCanWithdraw >= amount, "Not sufficient funds");

        return IERC20(address(rpax)).transfer(receiver, amount);
    }

    function canWithdrawTotal(address user) external view returns (uint256) {
        uint256 _canWithdrawAmount = extraReward[user] +
            freeRewardOfUserTotal[user] +
            pledgeRewardOfTotal[user] +
            rewardOfUserTotal[user] -
            userAlreadyWithdraw[user] +
            withdrawAlreayLiquidPledge[msg.sender];
        return _canWithdrawAmount;
    }

    function setExtraReward(
        address[] memory _user,
        uint256[] memory _rewardAmount
    ) external {
        require(msg.sender == owners, "Invalid owner");
        for (uint256 i = 0; i < _user.length; i++) {
            extraReward[_user[i]] = extraReward[_user[i]] + _rewardAmount[i];
        }
    }

    function setSubReward(address _user, uint256 _rewardAmount) external {
        require(msg.sender == owners, "Invalid owner");
        extraReward[_user] = extraReward[_user] - _rewardAmount;
    }

    function rescueToken(
        address tokenAddress,
        address receiver,
        uint256 amount
    ) public returns (bool success) {
        require(msg.sender == fundAddress, "Need fundAddress");
        return IERC20(tokenAddress).transfer(receiver, amount);
    }

    function setFundAddress(address _fundAddress) external onlyOwner {
        fundAddress = _fundAddress;
    }

    function setOwners(address _owners) external onlyOwner {
        owners = _owners;
    }

    function setOwner(
        address _ownerOne,
        address _ownerTwo,
        address _ownerThree
    ) external onlyOwner {
        require(_ownerOne != address(0), "Invalid address");
        require(_ownerTwo != address(0), "Invalid address");
        require(_ownerThree != address(0), "Invalid address");
        ownerOne = _ownerOne;
        ownerTwo = _ownerTwo;
        ownerThree = _ownerThree;
    }

    function setLiquidPledgeTime(uint256 _time) public onlyOwner {
        lastTxTime = _time;
    }

    function getPrice(uint256 amount) public view returns (uint256) {
        (uint256 reserve0, uint256 reserve1, ) = IPancakePair(
            rpax.uniswapV2PairUSDT()
        ).getReserves();
        return UniswapV2Library.getAmountOut(amount, reserve0, reserve1);
    }
}