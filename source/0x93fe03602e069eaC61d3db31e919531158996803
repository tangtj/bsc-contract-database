// File @openzeppelin/contracts/utils/Context.sol@v4.9.0

// SPDX-License-Identifier: MIT
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

// File @openzeppelin/contracts/access/Ownable.sol@v4.9.0

// OpenZeppelin Contracts (last updated v4.9.0) (access/Ownable.sol)

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

// File @openzeppelin/contracts/token/ERC20/IERC20.sol@v4.9.0

// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

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
    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

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

// File contracts/libraries/pancakeswapV3/TransferHelper.sol

library TransferHelper {
    /// @notice Transfers tokens from the targeted address to the given destination
    /// @notice Errors with 'STF' if transfer fails
    /// @param token The contract address of the token to be transferred
    /// @param from The originating address from which the tokens will be transferred
    /// @param to The destination address of the transfer
    /// @param value The amount to be transferred
    function safeTransferFrom(
        address token,
        address from,
        address to,
        uint256 value
    ) internal {
        (bool success, bytes memory data) = token.call(
            abi.encodeWithSelector(
                IERC20.transferFrom.selector,
                from,
                to,
                value
            )
        );
        require(
            success && (data.length == 0 || abi.decode(data, (bool))),
            "STF"
        );
    }

    /// @notice Transfers tokens from msg.sender to a recipient
    /// @dev Errors with ST if transfer fails
    /// @param token The contract address of the token which will be transferred
    /// @param to The recipient of the transfer
    /// @param value The value of the transfer
    function safeTransfer(address token, address to, uint256 value) internal {
        (bool success, bytes memory data) = token.call(
            abi.encodeWithSelector(IERC20.transfer.selector, to, value)
        );
        require(
            success && (data.length == 0 || abi.decode(data, (bool))),
            "ST"
        );
    }

    /// @notice Approves the stipulated contract to spend the given allowance in the given token
    /// @dev Errors with 'SA' if transfer fails
    /// @param token The contract address of the token to be approved
    /// @param to The target of the approval
    /// @param value The amount of the given token the target will be allowed to spend
    function safeApprove(address token, address to, uint256 value) internal {
        (bool success, bytes memory data) = token.call(
            abi.encodeWithSelector(IERC20.approve.selector, to, value)
        );
        require(
            success && (data.length == 0 || abi.decode(data, (bool))),
            "SA"
        );
    }

    /// @notice Transfers ETH to the recipient address
    /// @dev Fails with `STE`
    /// @param to The destination of the transfer
    /// @param value The value to be transferred
    function safeTransferETH(address to, uint256 value) internal {
        (bool success, ) = to.call{value: value}(new bytes(0));
        require(success, "STE");
    }
}

// File @openzeppelin/contracts/utils/math/SafeMath.sol@v4.9.0

// OpenZeppelin Contracts (last updated v4.9.0) (utils/math/SafeMath.sol)

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
    function tryAdd(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
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
    function trySub(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
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
    function tryMul(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
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
    function tryDiv(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
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
    function tryMod(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
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

// File contracts/interfaces/external/IERC20Querier.sol

interface IERC20Querier {
    function decimals() external view returns (uint256);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function totalSupply() external view returns (uint256);
}

// File contracts/interfaces/pancakeswapV3/IPancakeV3Factory.sol

/// @title The interface for the PancakeSwap V3 Factory
/// @notice The PancakeSwap V3 Factory facilitates creation of PancakeSwap V3 pools and control over the protocol fees
interface IPancakeV3Factory {
    /// @notice Returns the pool address for a given pair of tokens and a fee, or address 0 if it does not exist
    /// @dev tokenA and tokenB may be passed in either token0/token1 or token1/token0 order
    /// @param tokenA The contract address of either token0 or token1
    /// @param tokenB The contract address of the other token
    /// @param fee The fee collected upon every swap in the pool, denominated in hundredths of a bip
    /// @return pool The pool address
    function getPool(
        address tokenA,
        address tokenB,
        uint24 fee
    ) external view returns (address pool);
}

// File contracts/interfaces/pancakeswapV3/IPancakeV3Pool.sol

/// @title The interface for a PancakeSwap V3 Pool
/// @notice A PancakeSwap pool facilitates swapping and automated market making between any two assets that strictly conform
/// to the ERC20 specification
/// @dev The pool interface is broken up into many smaller pieces
interface IPancakeV3Pool {
    /// @notice The 0th storage slot in the pool stores many values, and is exposed as a single method to save gas
    /// when accessed externally.
    /// @return sqrtPriceX96 The current price of the pool as a sqrt(token1/token0) Q64.96 value
    /// tick The current tick of the pool, i.e. according to the last tick transition that was run.
    /// This value may not always be equal to SqrtTickMath.getTickAtSqrtRatio(sqrtPriceX96) if the price is on a tick
    /// boundary.
    /// observationIndex The index of the last oracle observation that was written,
    /// observationCardinality The current maximum number of observations stored in the pool,
    /// observationCardinalityNext The next maximum number of observations, to be updated when the observation.
    /// feeProtocol The protocol fee for both tokens of the pool.
    /// Encoded as two 4 bit values, where the protocol fee of token1 is shifted 4 bits and the protocol fee of token0
    /// is the lower 4 bits. Used as the denominator of a fraction of the swap fee, e.g. 4 means 1/4th of the swap fee.
    /// unlocked Whether the pool is currently locked to reentrancy
    function slot0()
        external
        view
        returns (
            uint160 sqrtPriceX96,
            int24 tick,
            uint16 observationIndex,
            uint16 observationCardinality,
            uint16 observationCardinalityNext,
            uint32 feeProtocol,
            bool unlocked
        );

    /// @notice The first of the two tokens of the pool, sorted by address
    /// @return The token contract address
    function token0() external view returns (address);

    /// @notice The second of the two tokens of the pool, sorted by address
    /// @return The token contract address
    function token1() external view returns (address);

    /// @notice The pool's fee in hundredths of a bip, i.e. 1e-6
    /// @return The fee
    function fee() external view returns (uint24);

    /// @notice The pool tick spacing
    /// @dev Ticks can only be used at multiples of this value, minimum of 1 and always positive
    /// e.g.: a tickSpacing of 3 means ticks can be initialized every 3rd tick, i.e., ..., -6, -3, 0, 3, 6, ...
    /// This value is an int24 to avoid casting even though it is always positive.
    /// @return The tick spacing
    function tickSpacing() external view returns (int24);
}

// File contracts/libraries/PoolHelper.sol

library PoolHelper {
    using SafeMath for uint256;

    function getPoolAddress(
        address pancakeswapV3FactoryAddress,
        address tokenA,
        address tokenB,
        uint24 poolFee
    ) internal view returns (address poolAddress) {
        return
            IPancakeV3Factory(pancakeswapV3FactoryAddress).getPool(
                tokenA,
                tokenB,
                poolFee
            );
    }

    function getPoolInfo(
        address poolAddress
    )
        internal
        view
        returns (
            address token0,
            address token1,
            uint24 poolFee,
            int24 tick,
            uint160 sqrtPriceX96,
            uint256 decimal0,
            uint256 decimal1
        )
    {
        (sqrtPriceX96, tick, , , , , ) = IPancakeV3Pool(poolAddress).slot0();
        token0 = IPancakeV3Pool(poolAddress).token0();
        token1 = IPancakeV3Pool(poolAddress).token1();
        poolFee = IPancakeV3Pool(poolAddress).fee();
        decimal0 = IERC20Querier(token0).decimals();
        decimal1 = IERC20Querier(token1).decimals();
    }

    /// @dev formula explanation
    /*
    [Original formula (without decimal precision)]
    (token1 * (10^decimal1)) / (token0 * (10^decimal0)) = (sqrtPriceX96 / (2^96))^2   
    tokenPrice = token1/token0 = (sqrtPriceX96 / (2^96))^2 * (10^decimal0) / (10^decimal1)

    [Formula with decimal precision & decimal adjustment]
    tokenPriceWithDecimalAdj = tokenPrice * (10^decimalPrecision)
        = (sqrtPriceX96 * (10^decimalPrecision) / (2^96))^2 
            / 10^(decimalPrecision + decimal1 - decimal0)
    */
    function getTokenPriceWithDecimalsByPool(
        address poolAddress,
        uint256 decimalPrecision
    ) internal view returns (uint256 tokenPriceWithDecimals) {
        (
            ,
            ,
            ,
            ,
            uint160 sqrtPriceX96,
            uint256 decimal0,
            uint256 decimal1
        ) = getPoolInfo(poolAddress);

        // when decimalPrecision is 18,
        // calculation restriction: 79228162514264337594 <= sqrtPriceX96 <= type(uint160).max
        uint256 scaledPriceX96 = uint256(sqrtPriceX96)
            .mul(10 ** decimalPrecision)
            .div(2 ** 96);
        uint256 tokenPriceWithoutDecimalAdj = scaledPriceX96.mul(
            scaledPriceX96
        );
        uint256 decimalAdj = decimalPrecision.add(decimal1).sub(decimal0);
        uint256 result = tokenPriceWithoutDecimalAdj.div(10 ** decimalAdj);
        require(result > 0, "token price too small");
        tokenPriceWithDecimals = result;
    }

    function getTokenDecimalAdjustment(
        address token
    ) internal view returns (uint256 decimalAdjustment) {
        uint256 tokenDecimalStandard = 18;
        uint256 decimal = IERC20Querier(token).decimals();
        return tokenDecimalStandard.sub(decimal);
    }
}

// File contracts/interfaces/external/IWETH9.sol

/// @title Interface for WETH9
interface IWETH9 {
    /// @notice Deposit ether to get wrapped ether
    function deposit() external payable;

    /// @notice Withdraw wrapped ether to get ether
    function withdraw(uint256 amount) external;
}

// File contracts/interfaces/IZap.sol

interface IZap {
    /// @dev get zap data
    function slippageToleranceNumerator() external view returns (uint24);

    function getSwapInfo(
        address inputToken,
        address outputToken
    )
        external
        view
        returns (
            bool isPathDefined,
            address[] memory swapPathArray,
            uint24[] memory swapTradeFeeArray
        );

    function getTokenExchangeRate(
        address inputToken,
        address outputToken
    )
        external
        view
        returns (
            address token0,
            address token1,
            uint256 tokenPriceWith18Decimals
        );

    function getMinimumSwapOutAmount(
        address inputToken,
        address outputToken,
        uint256 inputAmount
    ) external view returns (uint256 minimumSwapOutAmount);

    /// @dev swapToken
    function swapToken(
        bool isBNB,
        address inputToken,
        address outputToken,
        uint256 inputAmount,
        address recipient
    ) external payable returns (uint256 outputAmount);

    function swapTokenWithMinimumOutput(
        bool isBNB,
        address inputToken,
        address outputToken,
        uint256 inputAmount,
        uint256 minimumSwapOutAmount,
        address recipient
    ) external payable returns (uint256 outputAmount);
}

// File contracts/interfaces/IZapEvent.sol

interface IZapEvent {
    event UpdateSlippageTolerance(uint24 slippageTolerance);

    event UpdateSwapTradeFee(
        address indexed inputToken,
        address indexed outputToken,
        uint24 swapTradeFee
    );

    event UpdateSwapPath(
        address indexed inputToken,
        address indexed outputToken,
        address[] newSwapPath
    );

    event SingleSwap(
        address indexed recipient,
        bool isBNB,
        address inputToken,
        uint256 inputAmount,
        address outputToken,
        uint256 outputAmount,
        address[] swapPath,
        uint24[] swapTradeFee
    );

    event MultiSwap(
        address indexed recipient,
        bool isBNB,
        address inputToken,
        uint256 inputAmount,
        address outputToken,
        uint256 outputAmount,
        address[] swapPath,
        uint24[] swapTradeFee
    );
}

// File contracts/interfaces/pancakeswapV3/ISmartRouter.sol

pragma abicoder v2;

interface ISmartRouter {
    struct ExactInputSingleParams {
        address tokenIn;
        address tokenOut;
        uint24 fee;
        address recipient;
        uint256 amountIn;
        uint256 amountOutMinimum;
        uint160 sqrtPriceLimitX96;
    }

    /// @notice Swaps `amountIn` of one token for as much as possible of another token
    /// @dev Setting `amountIn` to 0 will cause the contract to look up its own balance,
    /// and swap the entire amount, enabling contracts to send tokens before calling this function.
    /// @param params The parameters necessary for the swap, encoded as `ExactInputSingleParams` in calldata
    /// @return amountOut The amount of the received token
    function exactInputSingle(
        ExactInputSingleParams calldata params
    ) external payable returns (uint256 amountOut);

    struct ExactInputParams {
        bytes path;
        address recipient;
        uint256 amountIn;
        uint256 amountOutMinimum;
    }

    /// @notice Swaps `amountIn` of one token for as much as possible of another along the specified path
    /// @dev Setting `amountIn` to 0 will cause the contract to look up its own balance,
    /// and swap the entire amount, enabling contracts to send tokens before calling this function.
    /// @param params The parameters necessary for the multi-hop swap, encoded as `ExactInputParams` in calldata
    /// @return amountOut The amount of the received token
    function exactInput(
        ExactInputParams calldata params
    ) external payable returns (uint256 amountOut);

    /// @notice Swaps `amountIn` of one token for as much as possible of another token
    /// @dev Setting `amountIn` to 0 will cause the contract to look up its own balance,
    /// and swap the entire amount, enabling contracts to send tokens before calling this function.
    /// @param amountIn The amount of token to swap
    /// @param amountOutMin The minimum amount of output that must be received
    /// @param path The ordered list of tokens to swap through
    /// @param to The recipient address
    /// @return amountOut The amount of the received token
    function swapExactTokensForTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to
    ) external payable returns (uint256 amountOut);
}

// File contracts/libraries/constants/Constants.sol

library Constants {
    /// @dev mainnet pancakeswap V3
    address public constant PANCAKE_V3_FACTORY_ADDRESS =
        address(0x0BFbCF9fa4f9C56B0F40a671Ad40E0805A091865);
    address public constant MASTER_CHEF_V3_ADDRESS =
        address(0x556B9306565093C855AEA9AE92A594704c2Cd59e);
    address public constant NONFUNGIBLE_POSITION_MANAGER_ADDRESS =
        address(0x46A15B0b27311cedF172AB29E4f4766fbE7F4364);
    address public constant SMART_ROUTER_ADDRESS =
        address(0x13f4EA83D0bd40E75C8222255bc855a974568Dd4);

    /// @dev testnet pancakeswap V3
    address public constant TESTNET_PANCAKE_V3_FACTORY_ADDRESS =
        address(0x0BFbCF9fa4f9C56B0F40a671Ad40E0805A091865);
    address public constant TESTNET_MASTER_CHEF_V3_ADDRESS =
        address(0x4c650FB471fe4e0f476fD3437C3411B1122c4e3B);
    address public constant TESTNET_NONFUNGIBLE_POSITION_MANAGER_ADDRESS =
        address(0x427bF5b37357632377eCbEC9de3626C71A5396c1);
    address public constant TESTNET_SMART_ROUTER_ADDRESS =
        address(0x9a489505a00cE272eAa5e07Dba6491314CaE3796);

    /// @dev mainnet token address
    address public constant WBNB_ADDRESS =
        address(0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c);
    address public constant USDT_ADDRESS =
        address(0x55d398326f99059fF775485246999027B3197955);
    address public constant USDC_ADDRESS =
        address(0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d);
    address public constant BTCB_ADDRESS =
        address(0x7130d2A12B9BCbFAe4f2634d864A1Ee1Ce3Ead9c);
    address public constant BUSD_ADDRESS =
        address(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56);
    address public constant CAKE_ADDRESS =
        address(0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82);
    address public constant ETH_ADDRESS =
        address(0x2170Ed0880ac9A755fd29B2688956BD959F933F8);
    address public constant XRP_ADDRESS =
        address(0x1D2F0da169ceB9fC7B3144628dB156f3F6c60dBE);
    address public constant ADA_ADDRESS =
        address(0x3EE2200Efb3400fAbB9AacF31297cBdD1d435D47);
    address public constant DOT_ADDRESS =
        address(0x7083609fCE4d1d8Dc0C979AAb8c869Ea2C873402);
    address public constant RXD_ADDRESS =
        address(0x92cb26ca653a51bBf916E6c3d58599CDB12e3a9F);

    /// @dev testnet token address
    address public constant TESTNET_WBNB_ADDRESS =
        address(0xae13d989daC2f0dEbFf460aC112a837C89BAa7cd);
    address public constant TESTNET_USDT_ADDRESS =
        address(0x0fB5D7c73FA349A90392f873a4FA1eCf6a3d0a96);
    address public constant TESTNET_BUSD_ADDRESS =
        address(0xaB1a4d4f1D656d2450692D237fdD6C7f9146e814);
    address public constant TESTNET_CAKE_ADDRESS =
        address(0x8d008B313C1d6C7fE2982F62d32Da7507cF43551);
    address public constant TESTNET_RXD_ADDRESS =
        address(0x3f83fCC8eFC9EBB56362222dD7844226870a12Ea);

    /// @dev black hole address
    address public constant BLACK_HOLE_ADDRESS =
        address(0x000000000000000000000000000000000000dEaD);
}

// File contracts/libraries/constants/ZapConstants.sol

library ZapConstants {
    /// @dev decimal precision
    uint256 public constant DECIMALS_PRECISION = 18;

    /// @dev denominator
    uint24 public constant SLIPPAGE_TOLERANCE_DENOMINATOR = 1000000;
    uint24 public constant SWAP_TRADE_FEE_DENOMINATOR = 1000000;
}

// File contracts/libraries/ParameterVerificationHelper.sol

library ParameterVerificationHelper {
    function verifyNotZeroAddress(address inputAddress) internal pure {
        require(inputAddress != address(0), "input zero address");
    }

    function verifyGreaterThanZero(uint256 inputNumber) internal pure {
        require(inputNumber > 0, "input 0");
    }

    function verifyGreaterThanZero(int24 inputNumber) internal pure {
        require(inputNumber > 0, "input 0");
    }

    function verifyGreaterThanOrEqualToZero(int24 inputNumber) internal pure {
        require(inputNumber >= 0, "input less than 0");
    }

    function verifyPairTokensHaveWbnb(
        address token0Address,
        address token1Address,
        address wbnbAddress
    ) internal pure {
        require(
            token0Address == wbnbAddress || token1Address == wbnbAddress,
            "pair token not have WBNB"
        );
    }

    function verifyMsgValueEqualsInputAmount(
        uint256 inputAmount
    ) internal view {
        require(msg.value == inputAmount, "msg.value != inputAmount");
    }

    function verifyPairTokensHaveInputToken(
        address token0Address,
        address token1Address,
        address inputToken
    ) internal pure {
        require(
            token0Address == inputToken || token1Address == inputToken,
            "pair token not have inputToken"
        );
    }
}

// File contracts/ZapInitializer.sol

contract ZapInitializer {
    // inputToken => outputToken => swapPath
    mapping(address => mapping(address => address[])) internal swapPath;

    // inputToken => outputToken => swapTradeFeeNumerator
    mapping(address => mapping(address => uint24))
        internal swapTradeFeeNumerator;

    function initializeSwapTradeFeeNumerator(bool isTestnet) internal {
        if (isTestnet) {
            address WBNB = Constants.TESTNET_WBNB_ADDRESS;
            address USDT = Constants.TESTNET_USDT_ADDRESS;
            address BUSD = Constants.TESTNET_BUSD_ADDRESS;
            address CAKE = Constants.TESTNET_CAKE_ADDRESS;

            /// @dev testnet initilaization
            swapTradeFeeNumerator[USDT][WBNB] = 2500;
            swapTradeFeeNumerator[WBNB][USDT] = 2500;
            swapTradeFeeNumerator[CAKE][WBNB] = 2500;
            swapTradeFeeNumerator[WBNB][CAKE] = 2500;
            swapTradeFeeNumerator[CAKE][BUSD] = 2500;
            swapTradeFeeNumerator[BUSD][CAKE] = 2500;
        } else {
            address WBNB = Constants.WBNB_ADDRESS;
            address USDT = Constants.USDT_ADDRESS;
            address USDC = Constants.USDC_ADDRESS;
            address BTCB = Constants.BTCB_ADDRESS;
            address BUSD = Constants.BUSD_ADDRESS;
            address CAKE = Constants.CAKE_ADDRESS;
            address ETH = Constants.ETH_ADDRESS;
            address XRP = Constants.XRP_ADDRESS;
            address ADA = Constants.ADA_ADDRESS;
            address DOT = Constants.DOT_ADDRESS;

            /// @dev mainnet initialization trade fee 0.01%
            swapTradeFeeNumerator[USDT][BUSD] = 100;
            swapTradeFeeNumerator[BUSD][USDT] = 100;
            swapTradeFeeNumerator[BUSD][USDC] = 100;
            swapTradeFeeNumerator[USDC][BUSD] = 100;

            /// @dev mainnet initialization trade fee 0.05%
            swapTradeFeeNumerator[WBNB][USDT] = 500;
            swapTradeFeeNumerator[WBNB][BUSD] = 500;
            swapTradeFeeNumerator[USDT][WBNB] = 500;
            swapTradeFeeNumerator[USDT][BTCB] = 500;
            swapTradeFeeNumerator[USDT][USDC] = 500;
            swapTradeFeeNumerator[BTCB][USDT] = 500;
            swapTradeFeeNumerator[BTCB][BUSD] = 500;
            swapTradeFeeNumerator[ETH][USDC] = 500;
            swapTradeFeeNumerator[BUSD][WBNB] = 500;
            swapTradeFeeNumerator[BUSD][BTCB] = 500;
            swapTradeFeeNumerator[USDC][USDT] = 500;
            swapTradeFeeNumerator[USDC][ETH] = 500;

            /// @dev mainnet initialization trade fee 0.25%
            swapTradeFeeNumerator[WBNB][BTCB] = 2500;
            swapTradeFeeNumerator[WBNB][ETH] = 2500;
            swapTradeFeeNumerator[CAKE][WBNB] = 2500;
            swapTradeFeeNumerator[CAKE][USDT] = 2500;
            swapTradeFeeNumerator[CAKE][BUSD] = 2500;
            swapTradeFeeNumerator[BTCB][WBNB] = 2500;
            swapTradeFeeNumerator[BTCB][ETH] = 2500;
            swapTradeFeeNumerator[ETH][WBNB] = 2500;
            swapTradeFeeNumerator[ETH][BTCB] = 2500;
            swapTradeFeeNumerator[XRP][WBNB] = 2500;
            swapTradeFeeNumerator[ADA][WBNB] = 2500;
            swapTradeFeeNumerator[DOT][WBNB] = 2500;
        }
    }

    function initializeSwapPath(bool isTestnet) internal {
        if (isTestnet) {
            address WBNB = Constants.TESTNET_WBNB_ADDRESS;
            address USDT = Constants.TESTNET_USDT_ADDRESS;
            address BUSD = Constants.TESTNET_BUSD_ADDRESS;
            address CAKE = Constants.TESTNET_CAKE_ADDRESS;

            /// @dev testnet initialization
            swapPath[WBNB][USDT] = [WBNB, USDT];
            swapPath[WBNB][BUSD] = [WBNB, CAKE, BUSD];
            swapPath[WBNB][CAKE] = [WBNB, CAKE];

            swapPath[USDT][WBNB] = [USDT, WBNB];
            swapPath[USDT][BUSD] = [USDT, WBNB, CAKE, BUSD];
            swapPath[USDT][CAKE] = [USDT, WBNB, CAKE];

            swapPath[BUSD][WBNB] = [BUSD, CAKE, WBNB];
            swapPath[BUSD][USDT] = [BUSD, CAKE, WBNB, USDT];
            swapPath[BUSD][CAKE] = [BUSD, CAKE];

            swapPath[CAKE][WBNB] = [CAKE, WBNB];
            swapPath[CAKE][USDT] = [CAKE, WBNB, USDT];
            swapPath[CAKE][BUSD] = [CAKE, BUSD];
        } else {
            address WBNB = Constants.WBNB_ADDRESS;
            address USDT = Constants.USDT_ADDRESS;
            address USDC = Constants.USDC_ADDRESS;
            address BTCB = Constants.BTCB_ADDRESS;
            address BUSD = Constants.BUSD_ADDRESS;
            address CAKE = Constants.CAKE_ADDRESS;
            address ETH = Constants.ETH_ADDRESS;
            address XRP = Constants.XRP_ADDRESS;
            address ADA = Constants.ADA_ADDRESS;
            address DOT = Constants.DOT_ADDRESS;

            /// @dev mainnet initialization single swap
            // trade fee 0.01%
            swapPath[USDT][BUSD] = [USDT, BUSD];
            swapPath[BUSD][USDT] = [BUSD, USDT];
            swapPath[BUSD][USDC] = [BUSD, USDC];
            swapPath[USDC][BUSD] = [USDC, BUSD];

            /// @dev mainnet initialization single swap
            // trade fee 0.05%
            swapPath[WBNB][USDT] = [WBNB, USDT];
            swapPath[WBNB][BUSD] = [WBNB, BUSD];
            swapPath[USDT][WBNB] = [USDT, WBNB];
            swapPath[USDT][BTCB] = [USDT, BTCB];
            swapPath[USDT][USDC] = [USDT, USDC];
            swapPath[BTCB][USDT] = [BTCB, USDT];
            swapPath[BTCB][BUSD] = [BTCB, BUSD];
            swapPath[ETH][USDC] = [ETH, USDC];
            swapPath[BUSD][WBNB] = [BUSD, WBNB];
            swapPath[BUSD][BTCB] = [BUSD, BTCB];
            swapPath[USDC][USDT] = [USDC, USDT];
            swapPath[USDC][ETH] = [USDC, ETH];

            /// @dev mainnet initialization single swap
            // trade fee 0.25%
            swapPath[WBNB][BTCB] = [WBNB, BTCB];
            swapPath[WBNB][ETH] = [WBNB, ETH];
            swapPath[CAKE][WBNB] = [CAKE, WBNB];
            swapPath[CAKE][USDT] = [CAKE, USDT];
            swapPath[CAKE][BUSD] = [CAKE, BUSD];
            swapPath[BTCB][WBNB] = [BTCB, WBNB];
            swapPath[BTCB][ETH] = [BTCB, ETH];
            swapPath[ETH][WBNB] = [ETH, WBNB];
            swapPath[ETH][BTCB] = [ETH, BTCB];
            swapPath[XRP][WBNB] = [XRP, WBNB];
            swapPath[ADA][WBNB] = [ADA, WBNB];
            swapPath[DOT][WBNB] = [DOT, WBNB];

            /// @dev mainnet initialization multi swap
            swapPath[WBNB][USDC] = [WBNB, BUSD, USDC];
            swapPath[CAKE][BTCB] = [CAKE, USDT, BTCB];
            swapPath[CAKE][USDC] = [CAKE, BUSD, USDC];
            swapPath[CAKE][ETH] = [CAKE, WBNB, ETH];
            swapPath[USDT][ETH] = [USDT, USDC, ETH];
            swapPath[BTCB][USDC] = [BTCB, BUSD, USDC];
            swapPath[ETH][USDT] = [ETH, USDC, USDT];
            swapPath[ETH][BUSD] = [ETH, USDC, BUSD];
            swapPath[BUSD][ETH] = [BUSD, USDC, ETH];
            swapPath[XRP][USDT] = [XRP, WBNB, USDT];
            swapPath[XRP][BTCB] = [XRP, WBNB, BTCB];
            swapPath[XRP][BUSD] = [XRP, WBNB, BUSD];
            swapPath[XRP][USDC] = [XRP, WBNB, BUSD, USDC];
            swapPath[XRP][ETH] = [XRP, WBNB, ETH];
            swapPath[ADA][USDT] = [ADA, WBNB, USDT];
            swapPath[ADA][BTCB] = [ADA, WBNB, BTCB];
            swapPath[ADA][BUSD] = [ADA, WBNB, BUSD];
            swapPath[ADA][USDC] = [ADA, WBNB, BUSD, USDC];
            swapPath[ADA][ETH] = [ADA, WBNB, ETH];
            swapPath[DOT][USDT] = [DOT, WBNB, USDT];
            swapPath[DOT][BTCB] = [DOT, WBNB, BTCB];
            swapPath[DOT][BUSD] = [DOT, WBNB, BUSD];
            swapPath[DOT][USDC] = [DOT, WBNB, BUSD, USDC];
            swapPath[DOT][ETH] = [DOT, WBNB, ETH];
            swapPath[USDC][WBNB] = [USDC, BUSD, WBNB];
            swapPath[USDC][BTCB] = [USDC, BUSD, BTCB];
        }
    }
}

// File contracts/Zap.sol

/// @dev verified, public contract
/// @dev ceratin functions only owner callable
contract Zap is IZap, IZapEvent, Ownable, ZapInitializer {
    using SafeMath for uint256;
    uint24 public override slippageToleranceNumerator;

    address public PANCAKE_V3_FACTORY_ADDRESS;
    address public SMART_ROUTER_ADDRESS;
    address public WBNB;

    constructor(bool isTestnet, uint24 _slippageToleranceNumerator) {
        // initialize pre-defined swapPath and swapTradeFeeNumerator
        initializeSwapTradeFeeNumerator(isTestnet);
        initializeSwapPath(isTestnet);
        slippageToleranceNumerator = _slippageToleranceNumerator;
        if (isTestnet) {
            PANCAKE_V3_FACTORY_ADDRESS = Constants
                .TESTNET_PANCAKE_V3_FACTORY_ADDRESS;
            SMART_ROUTER_ADDRESS = Constants.TESTNET_SMART_ROUTER_ADDRESS;
            WBNB = Constants.TESTNET_WBNB_ADDRESS;
        } else {
            PANCAKE_V3_FACTORY_ADDRESS = Constants.PANCAKE_V3_FACTORY_ADDRESS;
            SMART_ROUTER_ADDRESS = Constants.SMART_ROUTER_ADDRESS;
            WBNB = Constants.WBNB_ADDRESS;
        }
    }

    function getSwapInfo(
        address inputToken,
        address outputToken
    )
        public
        view
        override
        returns (
            bool isPathDefined,
            address[] memory swapPathArray,
            uint24[] memory swapTradeFeeArray
        )
    {
        // parameter verification
        ParameterVerificationHelper.verifyNotZeroAddress(inputToken);
        ParameterVerificationHelper.verifyNotZeroAddress(outputToken);

        // verify inputToken is not outputToken
        require(inputToken != outputToken, "inputToken == outputToken");

        // get swapPath
        address[] memory _swapPathArray = swapPath[inputToken][outputToken];
        uint256 pathLength = _swapPathArray.length;

        if (pathLength >= 2) {
            // statement for "single swap path" & "multiple swap path"
            bool _isPathDefined = true;
            uint24[] memory _swapTradeFeeArray = new uint24[](pathLength - 1);

            for (uint i = 0; i < (pathLength - 1); i++) {
                uint24 tradeFee = swapTradeFeeNumerator[_swapPathArray[i]][
                    _swapPathArray[i + 1]
                ];
                if (tradeFee == 0) {
                    // path not defined if tradeFee is 0
                    _isPathDefined = false;
                }
                _swapTradeFeeArray[i] = tradeFee;
            }
            return (_isPathDefined, _swapPathArray, _swapTradeFeeArray);
        } else {
            // statement for "path is not defined"
            return (false, new address[](0), new uint24[](0));
        }
    }

    function setSlippageToleranceNumerator(
        uint24 slippageTolerance
    ) public onlyOwner {
        // parameter verification
        ParameterVerificationHelper.verifyGreaterThanZero(slippageTolerance);

        // verify slippageTolerance is less than SLIPPAGE_TOLERANCE_DENOMINATOR
        require(
            slippageTolerance < ZapConstants.SLIPPAGE_TOLERANCE_DENOMINATOR,
            "slippageTolerance too big"
        );

        // update slippageToleranceNumerator
        slippageToleranceNumerator = slippageTolerance;

        // emit UpdateSlippageTolerance event
        emit UpdateSlippageTolerance(slippageTolerance);
    }

    function setSwapTradeFeeNumerator(
        address inputToken,
        address outputToken,
        uint24 swapTradeFee
    ) public onlyOwner {
        // parameter verification
        ParameterVerificationHelper.verifyNotZeroAddress(inputToken);
        ParameterVerificationHelper.verifyNotZeroAddress(outputToken);
        ParameterVerificationHelper.verifyGreaterThanZero(swapTradeFee);

        // verify inputToken is not outputToken
        require(inputToken != outputToken, "inputToken == outputToken");

        // verify pool is exist
        address poolAddress = PoolHelper.getPoolAddress(
            PANCAKE_V3_FACTORY_ADDRESS,
            inputToken,
            outputToken,
            swapTradeFee
        );
        require(poolAddress != address(0), "pool not exist");

        // update swapTradeFeeNumerator
        swapTradeFeeNumerator[inputToken][outputToken] = swapTradeFee;

        // emit UpdateSwapTradeFee event
        emit UpdateSwapTradeFee(inputToken, outputToken, swapTradeFee);
    }

    function setSwapPath(
        address inputToken,
        address outputToken,
        address[] memory newSwapPath
    ) public onlyOwner {
        // parameter verification
        ParameterVerificationHelper.verifyNotZeroAddress(inputToken);
        ParameterVerificationHelper.verifyNotZeroAddress(outputToken);
        uint256 pathLength = newSwapPath.length;
        for (uint i = 0; i < pathLength; i++) {
            ParameterVerificationHelper.verifyNotZeroAddress(newSwapPath[i]);
        }

        // verify inputToken is not outputToken
        require(inputToken != outputToken, "inputToken == outputToken");

        // verify input path is valid swap path
        require(pathLength >= 2, "path too short");

        // verify first token in newSwapPath is inputToken
        require(newSwapPath[0] == inputToken, "path not start from inputToken");

        // verify last token in newSwapPath is outputToken
        require(
            newSwapPath[(pathLength - 1)] == outputToken,
            "path not end with outputToken"
        );

        // verify each swap? fee is defined
        for (uint i = 0; i < (pathLength - 1); i++) {
            uint24 tradeFee = swapTradeFeeNumerator[newSwapPath[i]][
                newSwapPath[i + 1]
            ];
            require(tradeFee != 0, "tradefee not defined");
        }

        // update Swap Path
        swapPath[inputToken][outputToken] = newSwapPath;

        // emit UpdateSwapPath event
        emit UpdateSwapPath(inputToken, outputToken, newSwapPath);
    }

    function getTokenExchangeRate(
        address inputToken,
        address outputToken
    )
        public
        view
        override
        returns (
            address token0,
            address token1,
            uint256 tokenPriceWith18Decimals
        )
    {
        // parameter verification
        ParameterVerificationHelper.verifyNotZeroAddress(inputToken);
        ParameterVerificationHelper.verifyNotZeroAddress(outputToken);

        // verify inputToken is not outputToken
        require(inputToken != outputToken, "inputToken == outputToken");

        // verify swap trade fee is defined
        uint24 tradeFee = swapTradeFeeNumerator[inputToken][outputToken];
        require(tradeFee != 0, "tradeFee not define");

        // verify pool is exist
        address poolAddress = PoolHelper.getPoolAddress(
            PANCAKE_V3_FACTORY_ADDRESS,
            inputToken,
            outputToken,
            tradeFee
        );
        require(poolAddress != address(0), "pool not exist");

        // query pool info
        (token0, token1, , , , , ) = PoolHelper.getPoolInfo(poolAddress);

        // calculate token price with 18 decimal precision
        tokenPriceWith18Decimals = PoolHelper.getTokenPriceWithDecimalsByPool(
            poolAddress,
            ZapConstants.DECIMALS_PRECISION
        );
    }

    function getMinimumSwapOutAmount(
        address inputToken,
        address outputToken,
        uint256 inputAmount
    ) public view override returns (uint256 minimumSwapOutAmount) {
        uint256 estimateSwapOutAmount = getEstimateSwapOutAmount(
            inputToken,
            outputToken,
            inputAmount
        );

        // calculate price include slippage tolerance
        uint256 _minimumSwapOutAmount = estimateSwapOutAmount
            .mul(
                uint256(ZapConstants.SLIPPAGE_TOLERANCE_DENOMINATOR).sub(
                    slippageToleranceNumerator
                )
            )
            .div(ZapConstants.SLIPPAGE_TOLERANCE_DENOMINATOR);

        minimumSwapOutAmount = _minimumSwapOutAmount;
    }

    function getEstimateSwapOutAmount(
        address inputToken,
        address outputToken,
        uint256 inputAmount
    ) public view returns (uint256 estimateSwapOutAmount) {
        // parameter verification
        ParameterVerificationHelper.verifyNotZeroAddress(inputToken);
        ParameterVerificationHelper.verifyNotZeroAddress(outputToken);
        ParameterVerificationHelper.verifyGreaterThanZero(inputAmount);

        // variable verification
        require(
            slippageToleranceNumerator > 0,
            "slippageToleranceNumerator is 0"
        );

        // verify inputToken is not outputToken
        require(inputToken != outputToken, "inputToken == outputToken");

        // verify swap info is defined
        (
            bool isPathDefined,
            address[] memory swapPathArray,
            uint24[] memory swapTradeFeeArray
        ) = getSwapInfo(inputToken, outputToken);
        require(isPathDefined == true, "path not define");

        // get swap path length as loop end index
        uint256 pathLength = swapPathArray.length;

        // intput token decimal adjustment
        uint256 calcAmount = inputAmount.mul(
            10 ** (PoolHelper.getTokenDecimalAdjustment(inputToken))
        );
        // Loop calculate minimum swap out amount
        for (uint i = 0; i < (pathLength - 1); i++) {
            address tokenIn = swapPathArray[i];
            address tokenOut = swapPathArray[i + 1];
            (
                address token0,
                address token1,
                uint256 tokenPriceWith18Decimals // (token1/token0) * 10**DECIMALS_PRECISION
            ) = getTokenExchangeRate(tokenIn, tokenOut);

            // deduct trade fee
            calcAmount = calcAmount
                .mul(
                    uint256(ZapConstants.SWAP_TRADE_FEE_DENOMINATOR).sub(
                        swapTradeFeeArray[i]
                    )
                )
                .div(ZapConstants.SWAP_TRADE_FEE_DENOMINATOR);

            // get swap out amount without slippage
            require(tokenIn == token0 || tokenIn == token1);
            if (tokenIn == token0) {
                calcAmount = calcAmount.mul(tokenPriceWith18Decimals).div(
                    10 ** ZapConstants.DECIMALS_PRECISION
                );
            } else {
                calcAmount = calcAmount
                    .mul(10 ** ZapConstants.DECIMALS_PRECISION)
                    .div(tokenPriceWith18Decimals);
            }
        }

        // output token decimal adjustment
        estimateSwapOutAmount = calcAmount.div(
            10 ** (PoolHelper.getTokenDecimalAdjustment(outputToken))
        );
    }

    /// @notice caller need to approve inputToken to Zap contract in inputAmount amount
    /// @notice be aware of sandwich attack, only for small amount swap
    function swapToken(
        bool isBNB,
        address inputToken,
        address outputToken,
        uint256 inputAmount,
        address recipient
    ) public payable override returns (uint256 outputAmount) {
        // get minimum swap out amount
        uint256 minimumSwapOutAmount = getMinimumSwapOutAmount(
            inputToken,
            outputToken,
            inputAmount
        );

        outputAmount = swapTokenWithMinimumOutput(
            isBNB,
            inputToken,
            outputToken,
            inputAmount,
            minimumSwapOutAmount,
            recipient
        );
    }

    /// @notice caller need to approve inputToken to Zap contract in inputAmount amount
    function swapTokenWithMinimumOutput(
        bool isBNB,
        address inputToken,
        address outputToken,
        uint256 inputAmount,
        uint256 minimumSwapOutAmount,
        address recipient
    ) public payable override returns (uint256 outputAmount) {
        // parameter verification
        ParameterVerificationHelper.verifyNotZeroAddress(inputToken);
        ParameterVerificationHelper.verifyNotZeroAddress(outputToken);
        ParameterVerificationHelper.verifyNotZeroAddress(recipient);
        ParameterVerificationHelper.verifyGreaterThanZero(inputAmount);

        // verify inputToken is not outputToken
        require(inputToken != outputToken, "inputToken == outputToken");

        // verify swap info is defined
        (
            bool isPathDefined,
            address[] memory swapPathArray,
            uint24[] memory swapTradeFeeArray
        ) = getSwapInfo(inputToken, outputToken);
        require(isPathDefined == true, "path not define");

        if (isBNB) {
            // verify input BNB is the same as inputAmount
            ParameterVerificationHelper.verifyMsgValueEqualsInputAmount(
                inputAmount
            );
            require(
                inputToken == WBNB,
                "input BNB must have swap path from WBNB"
            );

            // prepare WBNB for swap
            IWETH9(WBNB).deposit{value: inputAmount}();
        } else {
            // verify caller inputToken allowance is more or equal to inputAmount
            require(
                IERC20Querier(inputToken).allowance(
                    msg.sender,
                    address(this)
                ) >= inputAmount,
                "allowance insufficient"
            );

            // transfer inputToken in inputAmount from caller to Zap contract
            TransferHelper.safeTransferFrom(
                inputToken,
                msg.sender,
                address(this),
                inputAmount
            );
        }

        // approve inputToken to SmartRouter in inputAmount amount
        TransferHelper.safeApprove(
            inputToken,
            SMART_ROUTER_ADDRESS,
            inputAmount
        );

        uint256 pathLength = swapPathArray.length;
        if (pathLength == 2) {
            // statement for "single swap path", swap by exactInputSingle function
            outputAmount = ISmartRouter(SMART_ROUTER_ADDRESS).exactInputSingle(
                ISmartRouter.ExactInputSingleParams(
                    inputToken,
                    outputToken,
                    swapTradeFeeArray[0],
                    recipient,
                    inputAmount,
                    minimumSwapOutAmount,
                    0
                )
            );
            // emit SingleSwap event
            emit SingleSwap(
                recipient,
                isBNB,
                inputToken,
                inputAmount,
                outputToken,
                outputAmount,
                swapPathArray,
                swapTradeFeeArray
            );
        } else {
            // statement for "multiple swap path", swap by exactInput function
            bytes memory path = abi.encodePacked(swapPathArray[0]);
            for (uint i = 0; i < (pathLength - 1); i++) {
                path = abi.encodePacked(
                    path,
                    swapTradeFeeArray[i],
                    swapPathArray[i + 1]
                );
            }

            outputAmount = ISmartRouter(SMART_ROUTER_ADDRESS).exactInput(
                ISmartRouter.ExactInputParams(
                    path,
                    recipient,
                    inputAmount,
                    minimumSwapOutAmount
                )
            );
            // emit MultiSwap event
            emit MultiSwap(
                recipient,
                isBNB,
                inputToken,
                inputAmount,
                outputToken,
                outputAmount,
                swapPathArray,
                swapTradeFeeArray
            );
        }
    }
}