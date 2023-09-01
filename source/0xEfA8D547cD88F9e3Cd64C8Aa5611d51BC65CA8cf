// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

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


/**
 * @dev Collection of functions related to the address type
 */
library Address {
    /**
     * @dev Returns true if `account` is a contract.
     *
     * [IMPORTANT]
     * ====
     * It is unsafe to assume that an address for which this function returns
     * false is an externally-owned account (EOA) and not a contract.
     *
     * Among others, `isContract` will return false for the following
     * types of addresses:
     *
     *  - an externally-owned account
     *  - a contract in construction
     *  - an address where a contract will be created
     *  - an address where a contract lived, but was destroyed
     *
     * Furthermore, `isContract` will also return true if the target contract within
     * the same transaction is already scheduled for destruction by `SELFDESTRUCT`,
     * which only has an effect at the end of a transaction.
     * ====
     *
     * [IMPORTANT]
     * ====
     * You shouldn't rely on `isContract` to protect against flash loan attacks!
     *
     * Preventing calls from contracts is highly discouraged. It breaks composability, breaks support for smart wallets
     * like Gnosis Safe, and does not provide security since it can be circumvented by calling from a contract
     * constructor.
     * ====
     */
    function isContract(address account) internal view returns (bool) {
        // This method relies on extcodesize/address.code.length, which returns 0
        // for contracts in construction, since the code is only stored at the end
        // of the constructor execution.

        return account.code.length > 0;
    }

    /**
     * @dev Replacement for Solidity's `transfer`: sends `amount` wei to
     * `recipient`, forwarding all available gas and reverting on errors.
     *
     * https://eips.ethereum.org/EIPS/eip-1884[EIP1884] increases the gas cost
     * of certain opcodes, possibly making contracts go over the 2300 gas limit
     * imposed by `transfer`, making them unable to receive funds via
     * `transfer`. {sendValue} removes this limitation.
     *
     * https://consensys.net/diligence/blog/2019/09/stop-using-soliditys-transfer-now/[Learn more].
     *
     * IMPORTANT: because control is transferred to `recipient`, care must be
     * taken to not create reentrancy vulnerabilities. Consider using
     * {ReentrancyGuard} or the
     * https://solidity.readthedocs.io/en/v0.8.0/security-considerations.html#use-the-checks-effects-interactions-pattern[checks-effects-interactions pattern].
     */
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    /**
     * @dev Performs a Solidity function call using a low level `call`. A
     * plain `call` is an unsafe replacement for a function call: use this
     * function instead.
     *
     * If `target` reverts with a revert reason, it is bubbled up by this
     * function (like regular Solidity function calls).
     *
     * Returns the raw returned data. To convert to the expected return value,
     * use https://solidity.readthedocs.io/en/latest/units-and-global-variables.html?highlight=abi.decode#abi-encoding-and-decoding-functions[`abi.decode`].
     *
     * Requirements:
     *
     * - `target` must be a contract.
     * - calling `target` with `data` must not revert.
     *
     * _Available since v3.1._
     */
    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, "Address: low-level call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`], but with
     * `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but also transferring `value` wei to `target`.
     *
     * Requirements:
     *
     * - the calling contract must have an ETH balance of at least `value`.
     * - the called Solidity function must be `payable`.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    /**
     * @dev Same as {xref-Address-functionCallWithValue-address-bytes-uint256-}[`functionCallWithValue`], but
     * with `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a static call.
     *
     * _Available since v3.3._
     */
    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        return functionStaticCall(target, data, "Address: low-level static call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a static call.
     *
     * _Available since v3.3._
     */
    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a delegate call.
     *
     * _Available since v3.4._
     */
    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionDelegateCall(target, data, "Address: low-level delegate call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a delegate call.
     *
     * _Available since v3.4._
     */
    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }

    /**
     * @dev Tool to verify that a low level call to smart-contract was successful, and revert (either by bubbling
     * the revert reason or using the provided one) in case of unsuccessful call or if target was not a contract.
     *
     * _Available since v4.8._
     */
    function verifyCallResultFromTarget(
        address target,
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        if (success) {
            if (returndata.length == 0) {
                // only check isContract if the call was successful and the return data is empty
                // otherwise we already know that it was a contract
                require(isContract(target), "Address: call to non-contract");
            }
            return returndata;
        } else {
            _revert(returndata, errorMessage);
        }
    }

    /**
     * @dev Tool to verify that a low level call was successful, and revert if it wasn't, either by bubbling the
     * revert reason or using the provided one.
     *
     * _Available since v4.3._
     */
    function verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal pure returns (bytes memory) {
        if (success) {
            return returndata;
        } else {
            _revert(returndata, errorMessage);
        }
    }

    function _revert(bytes memory returndata, string memory errorMessage) private pure {
        // Look for revert reason and bubble it up if present
        if (returndata.length > 0) {
            // The easiest way to bubble the revert reason is using memory via assembly
            /// @solidity memory-safe-assembly
            assembly {
                let returndata_size := mload(returndata)
                revert(add(32, returndata), returndata_size)
            }
        } else {
            revert(errorMessage);
        }
    }
}




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


interface IUniswapV2Factory {
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

library Model {
    address public constant _blackAddr = 0x000000000000000000000000000000000000dEaD;
    uint256 public constant MAX_INT = ~uint256(0);

    // user
    struct User {
        address addr;
        address inviterAddr;
    }
}

interface ILpInfo {
    function getLdxUser() external returns(address[] memory);
    function getUniswapV2UsdtPair() external returns(IUniswapV2Pair iUniswapV2Pair);
}


interface IUser {
    function bindInviter(address inviterAddr) external returns(bool);
    function getInviterUserByAddr(address userAddr) external view returns(address);
    function existUser(address userAddr) external view returns(bool);
    event BindInviter(address,address);
}

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

interface ISwapUsdt {
    function isSwapUsdt(address token) external view returns(bool);
    function isUsdtDvd() external view returns(bool);
    function swapUsdt(IERC20 token) external;
    function usdtDvd(IERC20 token) external;
}

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

contract Som_Token_Pro is IERC20,Ownable,ILpInfo{
	using SafeMath for uint256;
    using Address for address;
    string private constant _name = "SOM";
    string private constant _symbol = "SOM";
    uint8 private constant _decimals = 18;
    uint256 private constant _allBalances = 100 * 10000 *10**18;
    uint256 private constant _balancesPool = 20 * 10000 *10**18;
    uint256 public _liqBuyRatio1 = 1;
    uint256 public _liqBuyRatio2 = 2;
    uint256 public _liqSellRatio = 4;
    uint256 public _liqBuyNetRatio1 = 50;
    uint256 public _liqBuyNetRatio2 = 30;
    uint256 public _liqBuyNetRatio3 = 20;
    address[] _ldxUser;
    ISwapUsdt public _iSwapUsdt;
    address public _liqBurn = 0x000000000000000000000000000000000000dEaD;
    address public _liqMarket = 0xf56D79995e0C215017c94b0e23F713782461BbeA;
    address public _userRoot = 0x063207c7146f236feD65B0320E854Dc39F199F0a;
    address public _tokenOwner1 = 0x9Aa31198f089029e88dEfa26fb3ADEC765558202;
    address public _tokenOwner2 = 0xABdbdeEb955004885A1E3920B000AF7a2Fc9889d;
    IUser _user;
    bool public _isOpenSell = true;
    bool public _isMainOpen = true;
    bool public _isOpenBuy;
    mapping(address => bool) public _lpWihleList;
    mapping(address => bool) public _lpBlackList;
    mapping(address => bool) public _addLpBlackList;
    mapping(address => bool) public _subLpBlackList;
    mapping(address => bool) public _blackList;
	mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    receive() external payable {}
    IERC20 _usdt;
    IUniswapV2Router02 _uniswapV2Router;
	IUniswapV2Pair public _uniswapV2UsdtPair;
    mapping(address => bool) private _isSetUniswapV2UsdtPair;
	constructor() {
        require(address(_usdt)<address(this));
        address router;
        if (block.chainid == 56) {
            router = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
            _usdt = IERC20(0x55d398326f99059fF775485246999027B3197955);
        }
        _uniswapV2Router = IUniswapV2Router02(router);
        _uniswapV2UsdtPair = IUniswapV2Pair(IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this),address(_usdt)));
        _isSetUniswapV2UsdtPair[address(_uniswapV2UsdtPair)] = true;
        _setLpWihleList(_tokenOwner1,true);
        _setLpWihleList(_tokenOwner2,true);
        _setLpWihleList(0x62A4f6D8d131aE84420Ce2E80CD5f196ffc8dA96,true);
        _setLpWihleList(0x3860349944FF3eb45968c51C153B9a6025d64240,true);
        _setLpWihleList(0x083E240a8b2e55d6E42e41e5C93bA6B6249ff43B,true);
        _setLpWihleList(0xa3De71E1272F72b00E5C9B520DD859a3749E9C01,true);
        _setLpWihleList(0x5D7Dc6A175660D0b2DE87C0EE8055589934406d3,true);
        _setLpWihleList(0x2BEA6Df0c3FF2Fc9eB547373CbDa90cEF7BE5255,true);
        _setLpWihleList(0xF1f05b0b2c74748a8020219cE2d19dA75ae4961A,true);
        _setLpWihleList(0x6bdBa84Bfc9F76d8c295a5bED67E30D016270343,true);
        _setLpWihleList(0x371ee631ecbf5A95b9191bF26e1a34d7e5777084,true);
        _setLpWihleList(0xEEce7173EB1C7a2f9E0ad455C078d2128a1f0c75,true);
        _setSubLpBlackList(0x0ED943Ce24BaEBf257488771759F9BF482C39706,true);
        _balances[_tokenOwner1] = _balancesPool;
        emit Transfer(address(0), _tokenOwner1, _balancesPool);
        _balances[_tokenOwner2] = _allBalances.sub(_balancesPool);
        emit Transfer(address(0), _tokenOwner2, _allBalances.sub(_balancesPool));
    }
	function name() public pure returns (string memory) {
        return _name;
    }
    function symbol() public pure returns (string memory) {
        return _symbol;
    }
    function decimals() public pure returns (uint256) {
        return _decimals;
    }
    function totalSupply() public pure override returns (uint256) {
        return _allBalances;
    }
    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }
    function transfer(address recipient, uint256 amount)
        public
        override
        returns (bool)
    {
        _transfer(msg.sender, recipient, amount);
        return true;
    }
    function allowance(address owner, address spender)
        public
        view
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }
    function approve(address spender, uint256 amount)
        public
        override
        returns (bool)
    {
        _approve(msg.sender, spender, amount);
        return true;
    }
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            msg.sender,
            _allowances[sender][msg.sender].sub(
                amount,
                "ERC20: transfer amount exceeds allowance"
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
            msg.sender,
            spender,
            _allowances[msg.sender][spender].add(addedValue)
        );
        return true;
    }
    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            msg.sender,
            spender,
            _allowances[msg.sender][spender].sub(
                subtractedValue,
                "ERC20: decreased allowance below zero"
            )
        );
        return true;
    }
    function calcRetain(uint256 amount)
        private
        pure
        returns (uint256)
    {
        return amount.mul(1).div(10**4);
    }
	function _approve(
        address owner,
        address spender,
        uint256 amount
    ) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
	function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");

        bool isAdd = false;
        bool isDel = false;
        uint256 tLiquidityFee = 0;
        uint8 tradeType = 0;
        // One ten-thousandth of the transfer is reserved
        amount = amount.sub(calcRetain(amount));
        require(_isMainOpen, "Main switch not turned on!");
        require(!_blackList[from], "transaction blacklist!");
        if(to == address(_uniswapV2UsdtPair)) 
		{
            (isAdd,) = getLPStatus(from,to);
            if(!isAdd){
                // sell
                tradeType = 1;
                require(!_lpBlackList[from], "lpBlacklist!");
                if(!_lpWihleList[from]){
                    require(_isOpenSell,"is not open sell");
                    tLiquidityFee = _liqSellRatio;
                }
            }else{
                // add pool
                require(!_addLpBlackList[from], "addLpBlacklist!");
                _push(from);
            }
		}
        if(from == address(_uniswapV2UsdtPair)) 
		{
            (,isDel) = getLPStatus(from,to);
            if(!isDel){
                // buy
                tradeType = 2;
                require(!_lpBlackList[to], "lpBlacklist!");
                if(!_lpWihleList[to]){
                    require(_isOpenBuy,"is not open buy");
                    tLiquidityFee = _liqBuyRatio1.add(_liqBuyRatio2);
                }
            }else{
                // sub pool
                require(!_subLpBlackList[to], "subLpBlacklist!");
            }
		}
        if(tradeType == 1){
            dvdLp();
        }
        _tokenTransfer(from, to, amount, tLiquidityFee);
    }
    function dvdLp() private {
        if(address(_iSwapUsdt) != address(0)){
            if(_iSwapUsdt.isSwapUsdt(address(this))){
                _iSwapUsdt.swapUsdt(IERC20(address(this)));
            }else{
                if(_iSwapUsdt.isUsdtDvd()){
                    _iSwapUsdt.usdtDvd(IERC20(address(this)));
                }
            }
        }
    }
    function calcRatio(uint256 amount,uint256 liquidityFee)private pure returns (uint256){return amount.mul(liquidityFee).div(10**2);}
	function _getValues(uint256 tAmount,uint256 _liquidityFee) private pure returns (uint256,uint256){
        uint256 tLiquidity = calcRatio(tAmount,_liquidityFee);
        uint256 tTransferAmount = tAmount.sub(tLiquidity);
        return (tTransferAmount, tLiquidity);
    }
    function _takeLiquidity(address fromAddress,address to,uint256 amount,uint256 tLiquidityFee) private {
        // lp
        if(tLiquidityFee == 3){
            // buy 1% 
            uint256 _amount1 = calcRatio(amount,_liqBuyRatio1);
            _balances[_liqMarket] = _balances[_liqMarket].add(_amount1);
            emit Transfer(fromAddress, _liqMarket, _amount1);
            // buy 2% 
            uint256 _amount2 = calcRatio(amount,_liqBuyRatio2);
            if(address(_user) != address(0)){
                 _userDvp(fromAddress,to,_amount2);
            }else{
                _balances[_liqBurn] = _balances[_liqBurn].add(_amount2);
                emit Transfer(fromAddress, _liqBurn, _amount2);
            }
        }else{
            // sell 4%
            if(address(_iSwapUsdt) != address(0)){
                uint256 _amount4 = calcRatio(amount,tLiquidityFee);
                _balances[address(_iSwapUsdt)] = _balances[address(_iSwapUsdt)].add(_amount4);
                emit Transfer(fromAddress, address(_iSwapUsdt), _amount4);
            }else{
                uint256 _amount4 = calcRatio(amount,tLiquidityFee);
                _balances[_liqBurn] = _balances[_liqBurn].add(_amount4);
                emit Transfer(fromAddress, _liqBurn, _amount4);
            }
        }
    }
    function _userDvp(address fromAddress,address to,uint256 _amount) private {
        uint256 liqBuyNetRatioSum;
        address curr = to;
        address inviter;
        for(uint256 i = 0; i < 3; i++){
            inviter = _user.getInviterUserByAddr(curr);
            if(inviter == address(0)) break;
            if(inviter == _userRoot) break;
            uint256 liqBuyNetRatio;
            if(i == 0){
                liqBuyNetRatio = _liqBuyNetRatio1;
            }else if (i == 1){
                liqBuyNetRatio = _liqBuyNetRatio2;
            }else{
                liqBuyNetRatio = _liqBuyNetRatio3;
            }
            uint256 income = calcRatio(_amount,liqBuyNetRatio);
            liqBuyNetRatioSum = liqBuyNetRatioSum.add(income);
            _balances[inviter] = _balances[inviter].add(income);
            emit Transfer(fromAddress, inviter, income);
            curr = inviter;
        }
        uint256 subAmount = _amount.sub(liqBuyNetRatioSum);
        if (subAmount > 0){
            _balances[_liqBurn] = _balances[_liqBurn].add(subAmount);
            emit Transfer(fromAddress, _liqBurn, subAmount);
        }
    }
    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 amount,
		uint256 tLiquidityFee
    ) private {
        (uint256 tTransferAmount,uint256 tLiquidity) = _getValues(amount,tLiquidityFee);
        _balances[sender] = _balances[sender].sub(amount);
        _balances[recipient] = _balances[recipient].add(tTransferAmount);
        if (tLiquidity > 0) _takeLiquidity(sender,recipient,amount,tLiquidityFee);
        emit Transfer(sender, recipient, tTransferAmount);
    }
    function getLPStatus(address from,address to) internal view  returns (bool isAdd,bool isDel){
        IUniswapV2Pair pair;
        address token = address(this);
        if(_isSetUniswapV2UsdtPair[to]){
            pair = IUniswapV2Pair(to);
        }else{
            pair = IUniswapV2Pair(from);
        }
        isAdd = false;
        isDel = false;
        address token0 = pair.token0();
        address token1 = pair.token1();
        (uint r0,uint r1,) = pair.getReserves();
        uint bal1 = IERC20(token1).balanceOf(address(pair));
        uint bal0 = IERC20(token0).balanceOf(address(pair));
        if (_isSetUniswapV2UsdtPair[to]) {
            if (token0 == token) {
                if (bal1 > r1) {
                    uint change1 = bal1 - r1;
                    isAdd = change1 > 1000;
                }
            } else {
                if (bal0 > r0) {
                    uint change0 = bal0 - r0;
                    isAdd = change0 > 1000;
                }
            }
        }else {
            if (token0 == token) {
                if (bal1 < r1 && r1 > 0) {
                    uint change1 = r1 - bal1;
                    isDel = change1 > 0;
                }
            } else {
                if (bal0 < r0 && r0 > 0) {
                    uint change0 = r0 - bal0;
                    isDel = change0 > 0;
                }
            }
        }
        return (isAdd,isDel);
    }
    function _getIndex(address addr,address[] memory array) private pure returns (uint){
        if(addr == address(0)){
            return Model.MAX_INT;
        }
        for(uint i = 0;i < array.length; i++){
            if(addr == array[i]){
                return i;
            }
        }
        return Model.MAX_INT;
    }
    function _push(address addr) private returns(bool){
        require(addr != address(0), "zero address");
        if(_getIndex(addr,_ldxUser) == Model.MAX_INT){
            _ldxUser.push(addr);
            return true;
        }
        return false;
    }
    function _setLpWihleList(address addr,bool isBool) private {
        _lpWihleList[addr] = isBool;
    }
    function setLpWihleList(address addr,bool isBool) external onlyOwner {
        _setLpWihleList(addr,isBool);
    }
    function _setLpBlackList(address addr,bool isBool) private {
        _lpBlackList[addr] = isBool;
    }
    function setLpBlackList(address addr,bool isBool) external onlyOwner {
        _setLpBlackList(addr,isBool);
    }
    function _setAddLpBlackList(address addr,bool isBool) private {
        _addLpBlackList[addr] = isBool;
    }
    function setAddLpBlackList(address addr,bool isBool) external onlyOwner {
        _setAddLpBlackList(addr,isBool);
    }
    function _setSubLpBlackList(address addr,bool isBool) private {
        _subLpBlackList[addr] = isBool;
    }
    function setSubLpOrBuyBlackList(address addr,bool isBool) external onlyOwner {
        _setSubLpBlackList(addr,isBool);
    }
    function setBlackList(address addr,bool isBool) external onlyOwner {
        _blackList[addr] = isBool;
    }
    function getLdxUser() external override view returns(address[] memory){
        return _ldxUser;
    }
    function getUniswapV2UsdtPair() external override view returns(IUniswapV2Pair){
        return _uniswapV2UsdtPair;
    }
    function setLiqBuyRatio1(uint256 liqR) external onlyOwner {
        require(liqR > 0, "l1 0");
        _liqBuyRatio1 = liqR;
    }
    function setLiqBuyRatio2(uint256 liqR) external onlyOwner {
        require(liqR > 0, "l2 0");
        _liqBuyRatio2 = liqR;
    }
    function setLiqSellRatio(uint256 liqR) external onlyOwner {
        require(liqR > 0, "l3 0");
        _liqSellRatio = liqR;
    }
    function setSwapUsdt(ISwapUsdt iSwapUsdt) external onlyOwner {
        _iSwapUsdt = iSwapUsdt;
    }
    function setUser(IUser iUser) external onlyOwner {
        _user = iUser;
    }
    function setIsOpenSell(bool isOpen) external onlyOwner {
        _isOpenSell = isOpen;
    }
    function setIsOpenBuy(bool isOpen) external onlyOwner {
        _isOpenBuy = isOpen;
    }
    function setMainOpen(bool isOpen) external onlyOwner {
        _isMainOpen = isOpen;
    }
    function oneClickImportLp(address[] memory lpList) external onlyOwner{
		for (uint i=0; i<lpList.length; i++) {
            address lpAddr = lpList[i];
			_push(lpAddr);
		}
    }
}