//					$$\ $$\ $$\    $$\                    $$\   $$\     
//					\__|\__|$$ |   $$ |                   $$ |  $$ |    
//					$$\ $$\ $$ |   $$ |$$$$$$\  $$\   $$\ $$ |$$$$$$\   
//					$$ |$$ |\$$\  $$  |\____$$\ $$ |  $$ |$$ |\_$$  _|  
//					$$ |$$ | \$$\$$  / $$$$$$$ |$$ |  $$ |$$ |  $$ |    
//					$$ |$$ |  \$$$  / $$  __$$ |$$ |  $$ |$$ |  $$ |$$\ 
//					$$ |$$ |   \$  /  \$$$$$$$ |\$$$$$$  |$$ |  \$$$$  |
//					\__|\__|    \_/    \_______| \______/ \__|   \____/ 
//		                                                         
//		
//		 $$$$$$\                                                                  
//		$$  __$$\                                                                 
//		$$ /  \__|$$\  $$\  $$\  $$$$$$\   $$$$$$\   $$$$$$\   $$$$$$\   $$$$$$\  
//		\$$$$$$\  $$ | $$ | $$ | \____$$\ $$  __$$\ $$  __$$\ $$  __$$\ $$  __$$\ 
//		 \____$$\ $$ | $$ | $$ | $$$$$$$ |$$ /  $$ |$$ /  $$ |$$$$$$$$ |$$ |  \__|
//		$$\   $$ |$$ | $$ | $$ |$$  __$$ |$$ |  $$ |$$ |  $$ |$$   ____|$$ |      
//		\$$$$$$  |\$$$$$\$$$$  |\$$$$$$$ |$$$$$$$  |$$$$$$$  |\$$$$$$$\ $$ |      
//		 \______/  \_____\____/  \_______|$$  ____/ $$  ____/  \_______|\__|      
//		                                  $$ |      $$ |                          
//		                                  $$ |      $$ |                          
//		                                  \__|      \__|                                                                       

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.11;

// -------------------------------------- Babylonian -------------------------------------------
library Babylonian {
    // computes square roots using the babylonian method
	// https://en.wikipedia.org/wiki/Methods_of_computing_square_roots#Babylonian_method
	
	// credit for this implementation goes to
    // https://github.com/abdk-consulting/abdk-libraries-solidity/blob/master/ABDKMath64x64.sol#L687
    function sqrt(uint256 x) internal pure returns (uint256) {
        if (x == 0) return 0;
        // this block is equivalent to r = uint256(1) << (BitMath.mostSignificantBit(x) / 2);
        // however that code costs significantly more gas
        uint256 xx = x;
        uint256 r = 1;
        if (xx >= 0x100000000000000000000000000000000) {
            xx >>= 128;
            r <<= 64;
        }
        if (xx >= 0x10000000000000000) {
            xx >>= 64;
            r <<= 32;
        }
        if (xx >= 0x100000000) {
            xx >>= 32;
            r <<= 16;
        }
        if (xx >= 0x10000) {
            xx >>= 16;
            r <<= 8;
        }
        if (xx >= 0x100) {
            xx >>= 8;
            r <<= 4;
        }
        if (xx >= 0x10) {
            xx >>= 4;
            r <<= 2;
        }
        if (xx >= 0x8) {
            r <<= 1;
        }
        r = (r + x / r) >> 1;
        r = (r + x / r) >> 1;
        r = (r + x / r) >> 1;
        r = (r + x / r) >> 1;
        r = (r + x / r) >> 1;
        r = (r + x / r) >> 1;
        r = (r + x / r) >> 1; // Seven iterations should be enough
        uint256 r1 = x / r;
        return (r < r1 ? r : r1);
    }
}
// -------------------------------------- IERC20 -------------------------------------------
interface IERC20 {
    function totalSupply() external view returns (uint256);
	function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}
// -------------------------------------- IERC20 -------------------------------------------
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
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}
// -------------------------------------- Address -------------------------------------------
library Address {
    function isContract(address account) internal view returns (bool) {
        // According to EIP-1052, 0x0 is the value returned for not-yet created accounts
        // and 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470 is returned
        // for accounts without code, i.e. `keccak256('')`
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly {
            codehash := extcodehash(account)
        }
        return (codehash != accountHash && codehash != 0x0);
    }
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCall(target, data, "Address: low-level call failed");
    }
    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        require(isContract(target), "Address: call to non-contract");

        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResult(success, returndata, errorMessage);
    }
    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        return functionStaticCall(target, data, "Address: low-level static call failed");
    }
    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        require(isContract(target), "Address: static call to non-contract");

        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }
    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionDelegateCall(target, data, "Address: low-level delegate call failed");
    }
    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(isContract(target), "Address: delegate call to non-contract");

        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }
    function verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal pure returns (bytes memory) {
        if (success) {
            return returndata;
        } else {
            // Look for revert reason and bubble it up if present
            if (returndata.length > 0) {
                // The easiest way to bubble the revert reason is using memory via assembly

                assembly {
                    let returndata_size := mload(returndata)
                    revert(add(32, returndata), returndata_size)
                }
            } else {
                revert(errorMessage);
            }
        }
    }
}
// -------------------------------------- SafeERC20 -------------------------------------------
library SafeERC20 {
    using SafeMath for uint256;
    using Address for address;

    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    /**
     * @dev Deprecated. This function has issues similar to the ones found in
     * {IERC20-approve}, and its usage is discouraged.
     *
     * Whenever possible, use {safeIncreaseAllowance} and
     * {safeDecreaseAllowance} instead.
     */
    function safeApprove(IERC20 token, address spender, uint256 value) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        // solhint-disable-next-line max-line-length
        require((value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).add(value);
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).sub(value, "SafeERC20: decreased allowance below zero");
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     */
    function _callOptionalReturn(IERC20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We use {Address.functionCall} to perform this call, which verifies that
        // the target address contains contract code and also asserts for success in the low-level call.

        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        if (returndata.length > 0) { // Return data is optional
            // solhint-disable-next-line max-line-length
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}
// -------------------------------------- LowGasSafeMath -------------------------------------------
library LowGasSafeMath {	
	/// @title Optimized overflow and underflow safe math operations
	/// @notice Contains methods for doing math operations that revert on overflow or underflow for minimal gas cost

    /// @notice Returns x + y, reverts if sum overflows uint256
    /// @param x The augend
    /// @param y The addend
    /// @return z The sum of x and y
    function add(uint256 x, uint256 y) internal pure returns (uint256 z) {
        require((z = x + y) >= x);
    }

    /// @notice Returns x - y, reverts if underflows
    /// @param x The minuend
    /// @param y The subtrahend
    /// @return z The difference of x and y
    function sub(uint256 x, uint256 y) internal pure returns (uint256 z) {
        require((z = x - y) <= x);
    }

    /// @notice Returns x * y, reverts if overflows
    /// @param x The multiplicand
    /// @param y The multiplier
    /// @return z The product of x and y
    function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {
        require(x == 0 || (z = x * y) / x == y);
    }

    /// @notice Returns x + y, reverts if overflows or underflows
    /// @param x The augend
    /// @param y The addend
    /// @return z The sum of x and y
    function add(int256 x, int256 y) internal pure returns (int256 z) {
        require((z = x + y) >= x == (y >= 0));
    }

    /// @notice Returns x - y, reverts if overflows or underflows
    /// @param x The minuend
    /// @param y The subtrahend
    /// @return z The difference of x and y
    function sub(int256 x, int256 y) internal pure returns (int256 z) {
        require((z = x - y) <= x == (y >= 0));
    }
}
// -------------------------------------- IUniswapV2Pair -------------------------------------------
interface IUniswapV2Pair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);

    function mint(address to) external returns (uint liquidity);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;

    function initialize(address, address) external;
}
// -------------------------------------- IUniswapV2Router01 -------------------------------------------
interface IUniswapV2Router01 {
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
// -------------------------------------- IUniswapV2Router02 -------------------------------------------
interface IUniswapV2Router02 is IUniswapV2Router01 {
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
// -------------------------------------- IUniswapV2Factory -------------------------------------------
interface IUniswapV2Factory {
    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);
    function getPair(address tokenA, address tokenB) external view returns (address pair);
}
// -------------------------------------- IWETH -------------------------------------------
interface IWETH is IERC20 {
    function deposit() external payable;
    function withdraw(uint256 wad) external;
}
// -------------------------------------- IiiVault -------------------------------------------
interface IiiVault is IERC20 {
    function deposit(uint256 amount) external;
    function withdraw(uint256 shares) external;
    function want() external pure returns (address);
}

// -------------------------------------------------------------------------------------------------
// ---------------------------------- iiVaultPanV2Swapper ------------------------------------------
// -------------------------------------------------------------------------------------------------
contract A_Swapper_iiVaultPanV2 {
    using LowGasSafeMath for uint256;
    using SafeERC20 for IERC20;
    using SafeERC20 for IiiVault;

    IUniswapV2Router02 public immutable router;
    IUniswapV2Factory public immutable factory;
    address public immutable WETH;
    uint256 public constant minimumAmount = 1000;
    uint256 MAX_INT = 2**256 - 1;
    uint256 public constant MAX_SLIPPAGE = 1000;
    

    constructor() {        
        router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E); // mainnet        
        //router = IUniswapV2Router02(0x9Ac64Cc6e4415144C455BD8E4837Fea55603e5c3); // testnet
        
        WETH = router.WETH();
        factory = IUniswapV2Factory(router.factory());
    }

    receive() external payable {
        assert(msg.sender == WETH);
    }

    function inETH (address iiVault, uint256 slippage) external payable returns (uint256 outMin){
        require(msg.value >= minimumAmount, "iiVault Swapper: Insignificant input amount");
        require(slippage <= MAX_SLIPPAGE, "iiVault Swapper: Invalid slippage");
        
        IWETH(WETH).deposit{value: msg.value}();

        (, uint256 swapAmountOut, ) = estimateSwap(iiVault, WETH, msg.value);
        outMin = swapAmountOut - swapAmountOut * slippage / MAX_SLIPPAGE;

        _swapAndStake(iiVault, outMin, WETH);
    }   
    
    function inToken (address iiVault, address tokenIn, uint256 tokenInAmount, uint256 slippage) external returns (uint256 outMin) {
        require(tokenInAmount >= minimumAmount, "iiVault Swapper: Insignificant input amount");
        require(IERC20(tokenIn).allowance(msg.sender, address(this)) >= tokenInAmount, "iiVault Swapper: Input token is not approved");
        require(slippage <= MAX_SLIPPAGE, "iiVault Swapper: Invalid slippage");

        (, uint256 swapAmountOut, ) = estimateSwap(iiVault, tokenIn, tokenInAmount);
        outMin = swapAmountOut - swapAmountOut * slippage / MAX_SLIPPAGE;

        IERC20(tokenIn).safeTransferFrom(msg.sender, address(this), tokenInAmount);

        _swapAndStake(iiVault, outMin, tokenIn);
    }

    function inAnyToken (address iiVault, address tokenIn, uint256 tokenInAmount, address[] memory path, uint256 slippageIn, uint256 slippageOut) external returns (uint256 outMinWETH, uint256 outMinToken){
        Vars memory v;
		(v.vault, v.pair) = _getVaultPair(iiVault);
        require(tokenInAmount >= minimumAmount, "iiVault Swapper: Insignificant input amount");
        require(slippageIn <= MAX_SLIPPAGE, "iiVault Swapper: Invalid slippageIn");
        require(slippageOut <= MAX_SLIPPAGE, "iiVault Swapper: Invalid slippageOut");

        address liquidityToken = factory.getPair(tokenIn, WETH);
        require(liquidityToken != address(0), "iiVault Swapper: Invalid token, no paif found");
        
        require(IERC20(tokenIn).allowance(msg.sender, address(this)) >= tokenInAmount, "iiVault Swapper: Input token is not approved");

        v.pair = IUniswapV2Pair(liquidityToken);
                
        (v.reserveA, v.reserveB,) = v.pair.getReserves();
        require(v.reserveA > minimumAmount && v.reserveB > minimumAmount, "iiVault Swapper: Liquidity pair reserves too low");
                
        uint256 swapAmountOutWETH = v.pair.token0() == tokenIn ? router.getAmountOut(tokenInAmount, v.reserveA, v.reserveB) : router.getAmountOut(tokenInAmount, v.reserveB, v.reserveA);       
        outMinWETH = swapAmountOutWETH - swapAmountOutWETH * slippageIn / MAX_SLIPPAGE;

        if (path.length == 0) {
            path = new address[](2);
            path[0] = tokenIn;
            path[1] = WETH;
        }
        
        require(path.length - 1 <= 10, "iiVault Swapper: Path is too big 10 max allowed");
        require(path[0] == tokenIn, "iiVault Swapper: Input token in path must be same as tokenIn");
        require(path[path.length - 1] == WETH, "iiVault Swapper: Output token in path must be WETH");
        
        IERC20(tokenIn).safeTransferFrom(msg.sender, address(this), tokenInAmount);

        _approveTokenIfNeeded(tokenIn, address(router));
        
        router.swapExactTokensForTokens(tokenInAmount, outMinWETH, path, address(this), block.timestamp);

        uint256 balanceWETH = IERC20(WETH).balanceOf(address(this));
        
        (, uint256 swapAmountOutToken, ) = estimateSwap(iiVault, WETH, balanceWETH);
        outMinToken = swapAmountOutToken - swapAmountOutToken * slippageOut / MAX_SLIPPAGE;

        _swapAndStake(iiVault, outMinToken, WETH);
    }

    function outToken (address iiVault, uint256 withdrawAmount) external  returns (address token0, uint256 amount0, address token1, uint256 amount1) {
        (IiiVault vault, IUniswapV2Pair pair) = _getVaultPair(iiVault);

        IERC20(iiVault).safeTransferFrom(msg.sender, address(this), withdrawAmount);
        vault.withdraw(withdrawAmount);

        if (pair.token0() != WETH && pair.token1() != WETH) {
            token0 = pair.token0();
            token1 = pair.token1();
            (amount0, amount1) = _removeLiqudity(address(pair), msg.sender);

            return (token0, amount0, token1, amount1);
        }

        (amount0, amount1) = _removeLiqudity(address(pair), address(this));

        address[] memory tokens = new address[](2);
        tokens[0] = pair.token0();
        tokens[1] = pair.token1();

        _returnAssets(tokens);
    }

	struct Vars {		
		IUniswapV2Pair pair;
		IiiVault vault;
        address[] path;
		uint256 reserveA;
		uint256 reserveB;		
	}
	function outAndSwapToAny(address iiVault, uint256 withdrawAmount, address tokenOut, uint256 slippageIn, uint256 slippageOut) external returns (uint256 outMinWETH, uint256 outMinToken, uint256 tBalance, uint256 wBalance) {
        Vars memory v;
		(v.vault, v.pair) = _getVaultPair(iiVault);        		
		
		bool isFirstWETH = v.pair.token0() == WETH;
        require(isFirstWETH || v.pair.token1() == WETH, "iiVault Swapper: WETH not present in liqudity pair of input token");

        v.vault.safeTransferFrom(msg.sender, address(this), withdrawAmount);
        v.vault.withdraw(withdrawAmount);
        _removeLiqudity(address(v.pair), address(this));
				
        v.path = new address[](2);
        v.path[0] = isFirstWETH ? v.pair.token1() : v.pair.token0();
        v.path[1] = WETH;

		(v.reserveA, v.reserveB,) = v.pair.getReserves();
        require(v.reserveA > minimumAmount && v.reserveB > minimumAmount, "iiVault Swapper: Liquidity pair of input token reserves too low");

		tBalance = IERC20(v.path[0]).balanceOf(address(this));

        uint256 swapAmountOut = isFirstWETH ? router.getAmountOut(tBalance, v.reserveB, v.reserveA) : router.getAmountOut(tBalance, v.reserveA, v.reserveB);
		outMinWETH = swapAmountOut - swapAmountOut * slippageIn / MAX_SLIPPAGE;

        _approveTokenIfNeeded(v.path[0], address(router));
        router.swapExactTokensForTokens(tBalance, outMinWETH, v.path, address(this), block.timestamp);
		
        
		address liquidityToken = factory.getPair(tokenOut, WETH);
        require(liquidityToken != address(0), "iiVault Swapper: Invalid out token, no paif found");

        v.pair = IUniswapV2Pair(liquidityToken);
		isFirstWETH = v.pair.token0() == WETH;
		require(isFirstWETH || v.pair.token1() == WETH, "iiVault Swapper: WETH not present in liqudity pair of output token");

		(v.reserveA, v.reserveB,) = v.pair.getReserves();
        require(v.reserveA > minimumAmount && v.reserveB > minimumAmount, "iiVault Swapper: Liquidity pair of output token reserves too low");

		wBalance = IERC20(WETH).balanceOf(address(this));
		swapAmountOut = v.pair.token0() == WETH ? router.getAmountOut(wBalance, v.reserveA, v.reserveB) : router.getAmountOut(wBalance, v.reserveB, v.reserveA);
        outMinToken = swapAmountOut - swapAmountOut * slippageOut / MAX_SLIPPAGE;
		        
        v.path[0] = WETH;
        v.path[1] = tokenOut;
		
		_approveTokenIfNeeded(WETH, address(router));
		router.swapExactTokensForTokens(wBalance, outMinToken, v.path, address(this), block.timestamp);

		IERC20(tokenOut).safeTransfer(msg.sender, IERC20(tokenOut).balanceOf(address(this)));
    }

	function outAndSwap(address iiVault, uint256 withdrawAmount, address tokenOut, uint256 slippage) external returns (uint256 outMin, uint256 balanceOut) {
        (IiiVault vault, IUniswapV2Pair pair) = _getVaultPair(iiVault);
		
		address token0 = pair.token0();
        address token1 = pair.token1();
        
        require(token0 == tokenOut || token1 == tokenOut, "iiVault Swapper: desired token not present in liqudity pair");

        vault.safeTransferFrom(msg.sender, address(this), withdrawAmount);
        vault.withdraw(withdrawAmount);
        _removeLiqudity(address(pair), address(this));

        address swapToken = token1 == tokenOut ? token0 : token1;

        address[] memory path = new address[](2);
        path[0] = swapToken;
        path[1] = tokenOut;

		(uint256 reserveA, uint256 reserveB,) = pair.getReserves();
        require(reserveA > minimumAmount && reserveB > minimumAmount, "iiVault Swapper: Liquidity pair of output token reserves too low");

		balanceOut = IERC20(swapToken).balanceOf(address(this));

		uint256 swapAmountOut = swapToken == token0 ? router.getAmountOut(balanceOut, reserveA, reserveB) : router.getAmountOut(balanceOut, reserveB, reserveA);
        outMin = swapAmountOut - swapAmountOut * slippage / MAX_SLIPPAGE;

        _approveTokenIfNeeded(path[0], address(router));
        router.swapExactTokensForTokens(balanceOut, outMin, path, address(this), block.timestamp);

        _returnAssets(path);
    }

    function _removeLiqudity(address pair, address to) private returns (uint256 amount0, uint256 amount1) {
        IERC20(pair).safeTransfer(pair, IERC20(pair).balanceOf(address(this)));
        (amount0, amount1) = IUniswapV2Pair(pair).burn(to);

        require(amount0 >= minimumAmount, "iiVault Swapper UniswapV2Router: INSUFFICIENT_A_AMOUNT");
        require(amount1 >= minimumAmount, "iiVault Swapper UniswapV2Router: INSUFFICIENT_B_AMOUNT");
    }

    function _getVaultPair (address iiVault) private view returns (IiiVault vault, IUniswapV2Pair pair) {
        vault = IiiVault(iiVault);
        pair = IUniswapV2Pair(vault.want());
        require(pair.factory() == router.factory(), "iiVault Swapper: Incompatible liquidity pair factory");
    }

    function _swapAndStake(address iiVault, uint256 tokenAmountOutMin, address tokenIn) private {
        (IiiVault vault, IUniswapV2Pair pair) = _getVaultPair(iiVault);

        (uint256 reserveA, uint256 reserveB,) = pair.getReserves();
        require(reserveA > minimumAmount && reserveB > minimumAmount, "iiVault Swapper: Liquidity pair reserves too low");

        bool isInputA = pair.token0() == tokenIn;
        require(isInputA || pair.token1() == tokenIn, "iiVault Swapper: Input token not present in liqudity pair");

        address[] memory path = new address[](2);
        path[0] = tokenIn;
        path[1] = isInputA ? pair.token1() : pair.token0();

        uint256 fullInvestment = IERC20(tokenIn).balanceOf(address(this));
        uint256 swapAmountIn;
        if (isInputA) {
            swapAmountIn = _getSwapAmount(fullInvestment, reserveA, reserveB);
        } else {
            swapAmountIn = _getSwapAmount(fullInvestment, reserveB, reserveA);
        }

        _approveTokenIfNeeded(path[0], address(router));
        uint256[] memory swapedAmounts = router
            .swapExactTokensForTokens(swapAmountIn, tokenAmountOutMin, path, address(this), block.timestamp);

        _approveTokenIfNeeded(path[1], address(router));
        (,, uint256 amountLiquidity) = router
            .addLiquidity(path[0], path[1], fullInvestment.sub(swapedAmounts[0]), swapedAmounts[1], 1, 1, address(this), block.timestamp);

        _approveTokenIfNeeded(address(pair), address(vault));
        vault.deposit(amountLiquidity);

        vault.safeTransfer(msg.sender, vault.balanceOf(address(this)));
        _returnAssets(path);
    }

    function _returnAssets(address[] memory tokens) private {
        uint256 balance;
        for (uint256 i; i < tokens.length; i++) {
            balance = IERC20(tokens[i]).balanceOf(address(this));
            if (balance > 0) {
                if (tokens[i] == WETH) {
                    IWETH(WETH).withdraw(balance);
                    (bool success,) = msg.sender.call{value: balance}(new bytes(0));
                    require(success, "iiVault Swapper: ETH transfer failed");
                } else {
                    IERC20(tokens[i]).safeTransfer(msg.sender, balance);
                }
            }
        }
    }

    function _getSwapAmount(uint256 investmentA, uint256 reserveA, uint256 reserveB) private view returns (uint256 swapAmount) {
        uint256 halfInvestment = investmentA / 2;
        uint256 nominator = router.getAmountOut(halfInvestment, reserveA, reserveB);
        uint256 denominator = router.quote(halfInvestment, reserveA.add(halfInvestment), reserveB.sub(nominator));
        swapAmount = investmentA.sub(Babylonian.sqrt(halfInvestment * halfInvestment * nominator / denominator));
    }

    function estimateSwap(address iiVault, address tokenIn, uint256 fullInvestmentIn) public view returns(uint256 swapAmountIn, uint256 swapAmountOut, address swapTokenOut) {
        checkWETH();
        (, IUniswapV2Pair pair) = _getVaultPair(iiVault);

        bool isInputA = pair.token0() == tokenIn;
        require(isInputA || pair.token1() == tokenIn, "iiVault Swapper: Input token not present in liqudity pair");

        (uint256 reserveA, uint256 reserveB,) = pair.getReserves();
        (reserveA, reserveB) = isInputA ? (reserveA, reserveB) : (reserveB, reserveA);

        swapAmountIn = _getSwapAmount(fullInvestmentIn, reserveA, reserveB);
        swapAmountOut = router.getAmountOut(swapAmountIn, reserveA, reserveB);
        swapTokenOut = isInputA ? pair.token1() : pair.token0();
    }

    function checkWETH() public view returns (bool isValid) {
        isValid = WETH == router.WETH();
        require(isValid, "iiVault Swapper: WETH address not matching Router.WETH()");
    }

    function _approveTokenIfNeeded(address token, address spender) private {
        if (IERC20(token).allowance(address(this), spender) == 0) {
            IERC20(token).safeApprove(spender, MAX_INT);
        }
    }

}