// Sources flattened with hardhat v2.2.1 https://hardhat.org

// File @openzeppelin/contracts/utils/Address.sol@v4.1.0

// SPDX-License-Identifier: Apache-2.0

pragma solidity ^0.8.0;

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
     * ====
     */
    function isContract(address account) internal view returns (bool) {
        // This method relies on extcodesize, which returns 0 for contracts in
        // construction, since the code is only stored at the end of the
        // constructor execution.

        uint256 size;
        // solhint-disable-next-line no-inline-assembly
        assembly { size := extcodesize(account) }
        return size > 0;
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
     * https://diligence.consensys.net/posts/2019/09/stop-using-soliditys-transfer-now/[Learn more].
     *
     * IMPORTANT: because control is transferred to `recipient`, care must be
     * taken to not create reentrancy vulnerabilities. Consider using
     * {ReentrancyGuard} or the
     * https://solidity.readthedocs.io/en/v0.5.11/security-considerations.html#use-the-checks-effects-interactions-pattern[checks-effects-interactions pattern].
     */
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{ value: amount }("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    /**
     * @dev Performs a Solidity function call using a low level `call`. A
     * plain`call` is an unsafe replacement for a function call: use this
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
      return functionCall(target, data, "Address: low-level call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`], but with
     * `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCall(address target, bytes memory data, string memory errorMessage) internal returns (bytes memory) {
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
    function functionCallWithValue(address target, bytes memory data, uint256 value, string memory errorMessage) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        require(isContract(target), "Address: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{ value: value }(data);
        return _verifyCallResult(success, returndata, errorMessage);
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
    function functionStaticCall(address target, bytes memory data, string memory errorMessage) internal view returns (bytes memory) {
        require(isContract(target), "Address: static call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.staticcall(data);
        return _verifyCallResult(success, returndata, errorMessage);
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
    function functionDelegateCall(address target, bytes memory data, string memory errorMessage) internal returns (bytes memory) {
        require(isContract(target), "Address: delegate call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return _verifyCallResult(success, returndata, errorMessage);
    }

    function _verifyCallResult(bool success, bytes memory returndata, string memory errorMessage) private pure returns(bytes memory) {
        if (success) {
            return returndata;
        } else {
            // Look for revert reason and bubble it up if present
            if (returndata.length > 0) {
                // The easiest way to bubble the revert reason is using memory via assembly

                // solhint-disable-next-line no-inline-assembly
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


// File @openzeppelin/contracts/utils/math/SafeMath.sol@v4.1.0

pragma solidity ^0.8.0;

// CAUTION
// This version of SafeMath should only be used with Solidity 0.8 or later,
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
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
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


// File @openzeppelin/contracts/utils/Context.sol@v4.1.0

pragma solidity ^0.8.0;

/*
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
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}


// File @openzeppelin/contracts/access/Ownable.sol@v4.1.0

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
    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
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
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}


// File @openzeppelin/contracts/token/ERC20/IERC20.sol@v4.1.0

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
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount) external returns (bool);

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
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

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


// File @pancakeswap-libs/pancake-swap-core/contracts/interfaces/IPancakeFactory.sol@v0.1.0

pragma solidity >=0.5.0;

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


// File pancakeswap-peripheral/contracts/interfaces/IPancakeRouter01.sol@v1.1.0-beta.0

pragma solidity >=0.6.2;

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


// File pancakeswap-peripheral/contracts/interfaces/IPancakeRouter02.sol@v1.1.0-beta.0

pragma solidity >=0.6.2;
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


// File contracts/PancakeSwapProxy.sol

pragma solidity 0.8.4;

/// Import libraries
/// Import base contract from OpenZeppelin
/// Import PancakeSwap interface
/**
 * @title TODAMOON PancakeSwap Proxy Contract
 * 
 * @notice This contract is used to handle the logic involved in the swap and liquify of TODAMOON token via PancakeSwap.
 */
contract PancakeSwapProxy is Ownable {

    using SafeMath for uint256;
	using Address for address;

    /// Define the Pancakeswap contract interface
	IPancakeRouter02 public immutable pancakeswapV2Router;
	/// Record the Pancakeswap TDM-BUSD pair address
	address public immutable pancakeswapV2Pair;

    /// Define the TDM BEP20 token contract interface
    IERC20 public immutable tdm;
    /// Define the BUSD BEP20 token contract interface
	IERC20 public immutable busd;

    /// Define the TDM Ownable token contract interface
    Ownable public immutable tdmOwnable; 

    constructor() {
        /// Set Pancakeswap router address
		IPancakeRouter02 _pancakeswapV2Router = IPancakeRouter02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
		// Create a Pancakeswap pair for TDM (i.e. the contract owner and creator) against BUSD
		pancakeswapV2Pair = IPancakeFactory(_pancakeswapV2Router.factory())
			.createPair(address(owner()), address(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56));
		/// Set contract variables
		pancakeswapV2Router = _pancakeswapV2Router;
        /// Initialize TDM BEP20 token contract interface
        tdm = IERC20(owner()); // This contract should be created by the TDM token contract
        /// Initialize TDM Ownable contract interface
        tdmOwnable = Ownable(owner()); // This contract should be created by the TDM token contract
        /// Initialize BUSD BEP20 token contract interface
		busd = IERC20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56);
    }

    /**
	 * @notice Swap TDM token owned by this contract into BUSD and provide liquidity to Pancakeswap
     * @return (
     *      uint256 - the amount of TDM token which was used to swap into BUSD,
     *      uint256 - the amount of BUSD token obtained from the swap and was put back into the swap,
     *      uint256 - the amount of TDM token which was put into the swap    
     * )
	 */
	function swapAndLiquify() external onlyOwner() returns (uint256, uint256, uint256) {
        /// Get current TDM token balance
        uint256 contractTokenBalance = tdm.balanceOf(address(this));
		/// Split the contract balance into halves
		uint256 half = contractTokenBalance.div(2);
		uint256 otherHalf = contractTokenBalance.sub(half);
		/// Capture the contract's current BUSD balance
		uint256 initialBalance = busd.balanceOf(address(this));
		/// Swap half of the tokens for BUSD
		swapTokensForBUSD(half);
		/// Calculate how much BUSD did we just swap into
		uint256 busdSwapped = busd.balanceOf(address(this)).sub(initialBalance);
		/// Add liquidity to pancakeswap
		addLiquidity(otherHalf, busdSwapped);
        /// Return the amount of TDM token swapped for BUSD, amount of BUSD we get from the swap, 
        /// and the amount of TDM token which was put into PancakeSwap
        return (half, busdSwapped, otherHalf);
	}

    /**
	 * @notice Swap TDM into BUSD
	 * @param tokenAmount amount of TDM to be swapped into BUSD
	 */
	function swapTokensForBUSD(uint256 tokenAmount) private {
		/// Generate the pancakeswap pair path of TDM -> BUSD
		address[] memory path = new address[](2);
		path[0] = address(tdm);
		path[1] = address(busd);
		/// Approve Pancakeswap router to spend tokenAmount of TDM from this contract
		tdm.approve(address(pancakeswapV2Router), tokenAmount);
		/// Make the swap
		pancakeswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
			tokenAmount,
			0, /// Accept any amount of BUSD
			path,
			address(this),
			block.timestamp
		);
	}

	/**
	 * @notice Add liquidity to the TDM-BUSD pool
	 * @param tokenAmount amount of TDM token to be supplied
	 * @param busdAmount amount BUSD to be supplied
	 */
	function addLiquidity(uint256 tokenAmount, uint256 busdAmount) private {
		/// Approve Pancakeswap to use 'tokenAmount' of TDM token from this contract
		tdm.approve(address(pancakeswapV2Router), tokenAmount);
		/// Approve Pancakeswap to use 'busdAmount' of BUSD from this contract
		busd.approve(address(pancakeswapV2Router), busdAmount);
		/// Add the liquidity
		pancakeswapV2Router.addLiquidity(
			address(tdm),
			address(busd),
			tokenAmount,
			busdAmount,
			0, /// Slippage is unavoidable, we will provide liqudity regardless of price
			0, /// Slippage is unavoidable, we will provide liqudity regardless of price
			tdmOwnable.owner(),
			block.timestamp
		);
	}

}


// File contracts/TDM.sol

pragma solidity 0.8.4;

/// Import libraries
/// Import base contract from OpenZeppelin
/// Import PancakeSwapProxy contract
/**
 * @title TODAMOON Token Contract
 *
 * @notice This contract implements the TODAMOON token.
 */
contract TODAMOON is Context, IERC20, Ownable {
	
	using SafeMath for uint256;
	using Address for address;

	/// Record the amount of reflection owned by an address
	mapping (address => uint256) private _rOwned;
	/// Record the amount of "actual token" owned by an address
	/// Only used for address that are excluded from reflection reward
	/// It should be 0 for address that are not excluded
	mapping (address => uint256) private _tOwned;
	/// For ERC20 allowances mechancism
	mapping (address => mapping (address => uint256)) private _allowances;

	/// Record whether an address is excluded from paying the transaction tax
	/// If _isExcludedFromFee(address) = true, the address are exempted from transaction tax
	mapping (address => bool) private _isExcludedFromFee;

	/// Record whether an address is excluded from reflection reward
	/// If _isExcluded(address) = true, the address would not receive reflection reward
	mapping (address => bool) private _isExcluded;
	/// Another variable for storing address(es) that are excluded from reflection reward
	/// We need another variable since we need to loop over thses address(es) in _getCurrentSupply()
	address[] private _excluded;
   
	/// MAX is always the maximum integer represented by uint256 - 1
	uint256 private constant MAX = ~uint256(0);
	/// Define the maximum token supply which is 10 trillion TDM with 6 decimal places
	uint256 private constant _tTotal = 10 * 10**12 * 10**6;
	/// Define the initial maximum amount of reflection which should be the maximum
	/// integer that is divisible by _tTotal.
	/// It is because reflection ultimately represents certain underlying token balance
	/// and we want the conversion ratio between reflection and actual token to be an integer.
	uint256 private _rTotal = (MAX - (MAX % _tTotal));
	/// Record the total amount of token that was distributed to all owners due to transaction tax 
	uint256 private _tFeeTotal;

	/// Token name
	string private constant _name = "TODAMOON";
	/// Token symbol
	string private constant _symbol = "TDM";
	/// Decimal places used
	uint8 private constant _decimals = 6;
	
	/// Set TDM to distribute 5% of net transaction value to all owners
	uint256 public _taxFee = 5;
	/// Temp variable used in removeAllFee() and restoreAllFee()
	uint256 private _previousTaxFee = _taxFee;
	
	/// Set TDM to use 5% of net transaction value to provide liquidity
	uint256 public _liquidityFee = 5;
	/// Temp variable used in removeAllFee() and restoreAllFee()
	uint256 private _previousLiquidityFee = _liquidityFee;

	/// Define the PancakeSwapProxy interface
	PancakeSwapProxy public immutable pancakeSwapProxy;
	
	/// Used by swapAndLiquify() method to signal whether we are already providing liquidity
	/// so that we don't fall into an infinite loop in that method
	bool inSwapAndLiquify;
	/// Define whether the contract will use collected tax to provide liquidity onto the swap
	bool public swapAndLiquifyEnabled = true;
	
	/// Define that each transaction cannot exceeds 5% of token total supply to protect investor
	uint256 public _maxTxAmount = _tTotal.mul(5).div(100);
	/// Define the minimum amount of collected tax before we sent it to the swap to add liquidity
	/// It is set as 0.05% of total token supply to avoid excessive transaction cost
	uint256 private numTokensSellToAddToLiquidity = _tTotal.mul(5).div(10000);
	
	/// Event that will be emitted when an address is excluded or included in reflection reward
	event AddressExcludedFromRewardUpdated(address addressModified, bool isExcludedFromReward);
	/// Event that will be emitted when an address is excluded or included to pay transaction fee
	event AddressExcludedFromFeeUpdated(address addressModified, bool isExcludedFromFee);
	/// Event that will be emitted when the percentage of the net transaction value to be used to redistribute to token holders is updated
	event TransactionFeeUpdated(uint256 updatedTransactionFee);
	/// Event that will be emitted when the percentage of the net transaction value to be used to provide liquidity is updated
	event LiquidityFeeUpdated(uint256 updatedLiquidityFee);
	/// Event that will be emitted when the maximum transaction limit is updated
	event MaxTxAmountUpdated(uint256 updatedMaxTxAmount);
	/// Event that will be emitted when numTokensSellToAddToLiquidity is modified
	event MinTokensBeforeSwapUpdated(uint256 minTokensBeforeSwap);
	/// Event that will be emitted when swapAndLiquifyEnabled is modified
	event SwapAndLiquifyEnabledUpdated(bool enabled);
	/// Event that will be emitted when liquidity is provided to the swap
	event SwapAndLiquify(
		uint256 tokensSwapped,
		uint256 busdReceived,
		uint256 tokensIntoLiquidity
	);
	
	/// Modifier for preventing the contract from checking whether to provide liquidity when we are providing liquidity
	/// This is used to prevent an infinite loop
	modifier lockTheSwap {
		inSwapAndLiquify = true;
		_;
		inSwapAndLiquify = false;
	}

	/// ERC-20 Token Functions
	
	constructor() {
		/// Give deployer entire toke supply
		_rOwned[_msgSender()] = _rTotal;
		/// Initialize PancakeSwapProxy contract for swapping purposes
		pancakeSwapProxy = new PancakeSwapProxy();
		/// Exclude owner and this contract from fee
		_isExcludedFromFee[owner()] = true;
		_isExcludedFromFee[address(this)] = true;
		/// Emit Transfer event that signal all token has been transferred to the deployer
		emit Transfer(address(0), _msgSender(), _tTotal);
	}

	/**
	 * @notice Get the name of the token
	 * @return string name of the token
	 */
	function name() public pure returns (string memory) {
		return _name;
	}

	/**
	 * @notice Get the symbol of the token
	 * @return string symbol of the token
	 */
	function symbol() public pure returns (string memory) {
		return _symbol;
	}

	/**
	 * @notice Get the number of decimal place of the token
	 * @return uint8 number of decimal place of the token
	 */
	function decimals() public pure returns (uint8) {
		return _decimals;
	}

	/**
	 * @notice Get the total supply of the token
	 * @return uint256 total supply of the token
	 */
	function totalSupply() public pure override returns (uint256) {
		return _tTotal;
	}

	/**
	 * @notice Get the amount of token owned by an address
	 * @param account address to be checked
	 * @return uint256 amount of token owned by an address
	 */
	function balanceOf(address account) public view override returns (uint256) {
		/// For address excluded from reflection reward, their token balance is defined by the _tOwned variable
		if (_isExcluded[account]) return _tOwned[account];
		/// Otherwise, their token balance is defined by the amount of reflection they owned in the _rOwned variable
		return tokenFromReflection(_rOwned[account]);
	}

	/**
	 * @notice Moves 'amount' tokens from the caller's account to 'recipient'.
	 * @notice Emits a {Transfer} event after the transfer.
	 * @param recipient the recipient address of the token
	 * @param amount the amount of token to be transferred to the recipient
	 * @return bool a boolean indicating whether the operation succeeded.
	 */
	function transfer(address recipient, uint256 amount) public override returns (bool) {
		_transfer(_msgSender(), recipient, amount);
		return true;
	}

	/**
	 * @notice Returns the remaining number of tokens that 'spender' will be allowed to spend on behalf of 'owner' through {transferFrom}.
	 * @notice This is zero by default.
	 * @notice This value changes when {approve} or {transferFrom} are called.
	 * @param owner address which has allowed spender to spend their token
	 * @param spender address which can spend owner's token on their behalf
	 * @return uint256 the remaining number of tokens that 'spender' will be allowed to spend on behalf of 'owner'
	 */
	function allowance(address owner, address spender) public view override returns (uint256) {
		return _allowances[owner][spender];
	}

	/**
	 * @notice Sets 'amount' as the allowance of 'spender' over the caller's tokens.
	 * @notice Emits an {Approval} event.
	 * @param spender address which can spend caller's token on their behalf
	 * @param amount the amount of token that spender can spend on the caller's behalf
	 * @return bool a boolean indicating whether the operation succeeded.
	 */
	function approve(address spender, uint256 amount) public override returns (bool) {
		_approve(_msgSender(), spender, amount);
		return true;
	}

	/**
	 * @notice Moves 'amount' tokens from 'sender' to 'recipient' using the allowance mechanism
	 * @param sender address which sends the token
	 * @param recipient address that receives the token transferred
	 * @param amount the amount of token to be transferred
	 * @return bool a boolean indicating whether the operation succeeded.
	 */
	function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
		/// Transfer token from sender to recipient
		_transfer(sender, recipient, amount);
		/// Reduce the allowance that caller can spend on behalf of sender
		/// It should throw if the caller did not have enough allowance
		_approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
		return true;
	}

	/**
	 * @notice Increase the allowance of spender to spend caller's token by addedValue
	 * @param spender address which can spend caller's token on their behalf
	 * @param addedValue amount of token that spender can additionally spend on the caller's behalf
	 * @return bool a boolean indicating whether the operation succeeded.
	 */
	function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
		/// Increase allowance by addedValue
		_approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
		return true;
	}

	/**
	 * @notice Decrease the allowance of spender to spend caller's token by addedValue
	 * @param spender address which can spend caller's token on their behalf
	 * @param subtractedValue amount of allowance to be reduced
	 * @return bool a boolean indicating whether the operation succeeded.
	 */
	function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
		/// Decrease allowance by subtractedValue and throws if it drops below 0
		_approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
		return true;
	}

	/**
	 * @notice Get whether an account is excluded from reflection reward
	 * @param account address to be checked
	 * @return bool whether an account is excluded from reflection reward
	 */
	function isExcludedFromReward(address account) public view returns (bool) {
		return _isExcluded[account];
	}

	/**
	 * @notice Get the total amount of token that has been redistributed to holders via reflection
	 * @return uint256 the total amount of token that has been redistributed
	 */
	function totalFees() public view returns (uint256) {
		return _tFeeTotal;
	}

	/// Tokenomics Management Functions

	/**
	 * @notice Exclude an address from receiving reflection reward
	 * @param account address to be excluded
	 */
	function excludeFromReward(address account) external onlyOwner() {
		/// Do not exclude address that are already excluded
		require(!_isExcluded[account], "Account is already excluded");
		if(_rOwned[account] > 0) {
			/// Award the excluded address with _tOwned actual token that is equivalent to the amount of reflection it owned
			/// This is needed since excluded address can only transaction token owned as defined in the _tOwned variable
			_tOwned[account] = tokenFromReflection(_rOwned[account]);
		}
		/// Include the address in _isExcluded mapping and _excluded array
		_isExcluded[account] = true;
		_excluded.push(account);
		/// Emit AddressExcludedFromRewardUpdated event
		emit AddressExcludedFromRewardUpdated(account, true);
	}

	/**
	 * @notice Include an excluded address to receive reflection rewards.
	 * @notice Note that when an address is re-included, it will receive reflection rewards as if it was never excluded.
	 * @param account address to be included
	 */
	function includeInReward(address account) external onlyOwner() {
		/// Require the address to be currenrly excluded
		require(_isExcluded[account], "Account is not excluded");
		/// Loop over the _excluded array
		for (uint256 i = 0; i < _excluded.length; i++) {
			/// Look for the address in the _excluded array
			if (_excluded[i] == account) {
				/// Overwrite last excluded address to current address position
				_excluded[i] = _excluded[_excluded.length - 1];
				/// Set _tOwned to 0 since included address should use the reflection mechanism instead
				_tOwned[account] = 0;
				/// Set _isExcluded to false
				_isExcluded[account] = false;
				/// Pop the last excluded address since it has overwritten to current position already
				_excluded.pop();
				break;
			}
		}
		/// Emit AddressExcludedFromRewardUpdated event
		emit AddressExcludedFromRewardUpdated(account, false);
	}
	
	/**
	 * @notice Exclude an address from paying the transaction fee
	 * @param account address to be excluded from paying the transaction fee
	 */
	function excludeFromFee(address account) external onlyOwner() {
		_isExcludedFromFee[account] = true;
		emit AddressExcludedFromFeeUpdated(account, true);
	}
	
	/**
	 * @notice Include an address to pay the transaction fee
	 * @param account address to be included to pay the transaction fee
	 */
	function includeInFee(address account) external onlyOwner() {
		_isExcludedFromFee[account] = false;
		emit AddressExcludedFromFeeUpdated(account, false);
	}
	
	/**
	 * @notice Set the percentage of the net transaction value to be used to redistribute to token holders
	 * @param taxFee new percentage of the net transaction value to be used to redistribute to token holders
	 */
	function setTaxFeePercent(uint256 taxFee) external onlyOwner() {
		_taxFee = taxFee;
		emit TransactionFeeUpdated(taxFee);
	}
	
	/**
	 * @notice Set the percentage of the net transaction value to be used to provide liquidity
	 * @param liquidityFee new percentage of the net transaction value to be used to provide liquidity
	 */
	function setLiquidityFeePercent(uint256 liquidityFee) external onlyOwner() {
		_liquidityFee = liquidityFee;
		emit LiquidityFeeUpdated(liquidityFee);
	}
   
	/**
	 * @notice Set the maximum transaction limit as a percentage of the total token supply
	 * @param maxTxPercent new transaction limit as a percentage of the total token supply
	 */
	function setMaxTxPercent(uint256 maxTxPercent) external onlyOwner() {
		_maxTxAmount = _tTotal.mul(maxTxPercent).div(
			10**2
		);
		emit MaxTxAmountUpdated(_maxTxAmount);
	}

	/**
	 * @notice Set the minimum number of tokens collected from transaction fees before we sent them to PancakeSwap to provide liquidity
	 * @param minTokensBeforeSwap new minimum number of tokens collected from transaction fees before we sent them to PancakeSwap to provide liquidity
	 */
	function setMinTokensBeforeSwap(uint256 minTokensBeforeSwap) external onlyOwner() {
		numTokensSellToAddToLiquidity = minTokensBeforeSwap;
		emit MinTokensBeforeSwapUpdated(numTokensSellToAddToLiquidity);
	}

	/**
	 * @notice Enable or disable the feature to sent collected tokens from transaction fee to swap to provide liquidity
	 * @param _enabled enable or disable the feature
	 */
	function setSwapAndLiquifyEnabled(bool _enabled) external onlyOwner() {
		swapAndLiquifyEnabled = _enabled;
		emit SwapAndLiquifyEnabledUpdated(_enabled);
	}

	/// Tokenomics Features

	/**
	 * @notice Redistribute tAmount of token to all holders via reflection as if it is collected via a transaction fee
	 * @param tAmount amount of token to be redistributed
	 */
	function redistribute(uint256 tAmount) public {
		address sender = _msgSender();
		/// Since token balance of excluded address is defined by _tOwned instead of _rOwned,
		/// their token balance cannot be redistributed
		require(!_isExcluded[sender], "Excluded addresses cannot call this function");
		/// The equivalent amount of reflection that correspond to tAmount of token
		(uint256 rAmount,,,,,) = _getValues(tAmount);
		/// Subtract the amount of reflection (and therefore tAmount of token) from caller
		_rOwned[sender] = _rOwned[sender].sub(rAmount);
		/// Decrease total amount of reflection and increment the _tFeeTotal variable
		_rTotal = _rTotal.sub(rAmount);
		_tFeeTotal = _tFeeTotal.add(tAmount);
	}

	/**
	 * @notice Convert tAmount of token to an equivalent amount of the corresponding reflection, before or after fee
	 * @param tAmount amount of token
	 * @param deductTransferFee whether to deduct transaction fee or not
	 * @return uint256 amount of equivalent reflection
	 */
	function reflectionFromToken(uint256 tAmount, bool deductTransferFee) public view returns(uint256) {
		/// Check that tAmount is less than total token supply
		require(tAmount <= _tTotal, "Amount must be less than supply");
		if (!deductTransferFee) {
			/// Get the equivalent amount of reflection that corresponds to tAmount of token before fee
			(uint256 rAmount,,,,,) = _getValues(tAmount);
			return rAmount;
		} else {
			/// Get the equivalent amount of reflection that corresponds to tAmount of token after all transaction fee
			(,uint256 rTransferAmount,,,,) = _getValues(tAmount);
			return rTransferAmount;
		}
	}

	/**
	 * @notice Convert amount of reflection to amount of token
	 * @param rAmount amount of reflection
	 * @return uint256 equivalent amount of token
	 */
	function tokenFromReflection(uint256 rAmount) public view returns(uint256) {
		/// Check that rAmount is less than the total supply of reflection
		require(rAmount <= _rTotal, "Amount must be less than total reflections");
		/// Get the conversion rate between reflection and token
		uint256 currentRate =  _getRate();
		/// Actual token = amount of reflection / conversion rate
		return rAmount.div(currentRate);
	}

	/**
	 * @notice Reflect fee collected from a transaction in the total amount of reflection and the total fee variable
	 * @param rFee transaction fee collected in the unit of reflection
	 * @param tFee transaction fee collected in the unit of the actual token
	 */
	function _reflectFee(uint256 rFee, uint256 tFee) private {
		/// Subtract reflection destroyed due to transaction fee from total reflection supply
		_rTotal = _rTotal.sub(rFee);
		/// Increment total fee variable
		_tFeeTotal = _tFeeTotal.add(tFee);
	}

	/**
	 * @notice Get transaction parameter if tAmount of tokens is transferred
	 * @param tAmount amount of tokens to be transferred
	 * @return (
	 *		uint256 - the amount of reflection to be transferred or subtracted from the sender, 
	 *		uint256 - the amount of reflection to be rewarded to the recipient (with fee subtracted), 
	 *		uint256 - the amount of reflection to be collected as transaction tax for redistributing to token holders, 
	 *		uint256 - the amount of actual token to be rewarded to the recipient (with fee subtracted), 
	 *		uint256 - the amount of actual token to be collected as transaction tax for redistributing to token holders,
	 *		uint256 - the amount of actual token to be collected as transaction tax for providing liquidity
	 * )
	 */
	function _getValues(uint256 tAmount) private view returns (uint256, uint256, uint256, uint256, uint256, uint256) {
		/// Get amount of token to be collected as fee and rewarded to recipient if tAmount of token was sent
		/// tAmount = tTransferAmount - tFee - tLiquidity
		(uint256 tTransferAmount, uint256 tFee, uint256 tLiquidity) = _getTValues(tAmount);
		/// Get equivalent amount of reflection to be sent, received and collected as fee
		/// rAmount = rTransferAmount - rFee - tLiquidity * _getRate()
		(uint256 rAmount, uint256 rTransferAmount, uint256 rFee) = _getRValues(tAmount, tFee, tLiquidity, _getRate());
		return (rAmount, rTransferAmount, rFee, tTransferAmount, tFee, tLiquidity);
	}

	/**
	 * @notice Get transaction parameter if tAmount of tokens is transferred
	 * @param tAmount amount of tokens to be transferred
	 * @return (
	 *		uint256 - the amount of actual token to be rewarded to the recipient (with fee subtracted), 
	 *		uint256 - the amount of actual token to be collected as transaction tax for redistributing to token holders,
	 *		uint256 - the amount of actual token to be collected as transaction tax for providing liquidity
	 * )
	 */
	function _getTValues(uint256 tAmount) private view returns (uint256, uint256, uint256) {
		/// Calculate the amount of token to be collected as transaction tax for redistributing to token holders
		uint256 tFee = calculateTaxFee(tAmount);
		/// Calculate the amount of token to be collected as transaction tax for providing liquidity
		uint256 tLiquidity = calculateLiquidityFee(tAmount);
		/// Actual amount of token rewarded to recipient should be tAmount - tFee - tLiquidity
		uint256 tTransferAmount = tAmount.sub(tFee).sub(tLiquidity);
		return (tTransferAmount, tFee, tLiquidity);
	}

	/**
	 * @notice Get transaction parameter if tAmount of tokens is transferred
	 * @param tAmount amount of tokens to be transferred
	 * @param tFee amount of actual tokens to be collected as transaction tax for redistributing to token holders
	 * @param tLiquidity amount of actual tokens to be collected as transaction tax for providing liquidity
	 * @param currentRate current conversion ratio between reflection and the actual tokens
	 * @return (
	 *		uint256 - the amount of reflection to be transferred or subtracted from the sender, 
	 *		uint256 - the amount of reflection to be rewarded to the recipient (with fee subtracted), 
	 *		uint256 - the amount of reflection to be collected as transaction tax for redistributing to token holders where rAmount = rTransferAmount + rFee
	 * )
	 */
	function _getRValues(uint256 tAmount, uint256 tFee, uint256 tLiquidity, uint256 currentRate) private pure returns (uint256, uint256, uint256) {
		/// Amount of reflection to be subtracted from sender should be tAmount * currentRate
		uint256 rAmount = tAmount.mul(currentRate);
		/// Amount of reflection to be collected as transaction tax for redistribution should be tFee * currentRate
		uint256 rFee = tFee.mul(currentRate);
		/// Amount of reflection to be collected as transaction tax for providing liquidity should be tLiquidity * currentRate
		uint256 rLiquidity = tLiquidity.mul(currentRate);
		/// Actual amount of token to be rewarded to recipient should be rAmount - rFee - rLiquidity
		uint256 rTransferAmount = rAmount.sub(rFee).sub(rLiquidity);
		return (rAmount, rTransferAmount, rFee);
	}

	/**
	 * @notice Get the conversion rate between reflection and token
	 * @return uint256 conversion rate between reflection and token
	 */
	function _getRate() private view returns(uint256) {
		/// Get current total supply of reflection and token
		(uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
		/// Conversion rate is derived by total supply of reflection / total supply of token
		return rSupply.div(tSupply);
	}

	/**
	 * @notice Get the current supply of reflection and token
	 * @notice Note that reflection and token owned by an excluded address are excluded from the current supply
	 * @return (uint256 - current supply of reflection, uint256 - current token supply)
	 */
	function _getCurrentSupply() private view returns(uint256, uint256) {
		/// Default reflection supply and token supply should be its current total supply
		uint256 rSupply = _rTotal;
		uint256 tSupply = _tTotal;      
		/// Exclude excluded address from current supply
		for (uint256 i = 0; i < _excluded.length; i++) {
			/// This statement should never run since this will implies some address owns more than the total supply
			if (_rOwned[_excluded[i]] > rSupply || _tOwned[_excluded[i]] > tSupply) return (_rTotal, _tTotal);
			/// Subtract reflection and token supply owned by the excluded address
			rSupply = rSupply.sub(_rOwned[_excluded[i]]);
			tSupply = tSupply.sub(_tOwned[_excluded[i]]);
		}
		/// This statement should never run unless almost all token supply is excluded
		if (rSupply < _rTotal.div(_tTotal)) return (_rTotal, _tTotal);
		/// Return the reflection and token supply with those owned by excluded address excluded
		return (rSupply, tSupply);
	}
	
	/**
	 * @notice Collect tLiquidity amount of token as transaction tax for providing liquidity to swap
	 * @param tLiquidity amount of token collected as transaction tax for providing liquidity to swap
	 */
	function _takeLiquidity(uint256 tLiquidity) private {
		/// Get current conversion ratio between reflection and token
		uint256 currentRate =  _getRate();
		/// Compute the equivalent amount of reflection that corresponds to tLiquidity amount of token
		uint256 rLiquidity = tLiquidity.mul(currentRate);
		/// Add the amount of reflection to this contract address
		/// Collected transaction tax will be automatically send to swap to provide liquidity 
		/// once it collects more than numTokensSellToAddToLiquidity of token in the next transaction
		/// Note that we don't have to subtract any reflection here, since it is already handled by the 
		/// transfer function
		_rOwned[address(this)] = _rOwned[address(this)].add(rLiquidity);
		if(_isExcluded[address(this)])
			/// If this contract is an excluded address, then we have to add the token collected to _tOwned as well
			_tOwned[address(this)] = _tOwned[address(this)].add(tLiquidity);
	}
	
	/**
	 * @notice Calculate the amount of token to be collected as a transaction fee for redistribution
	 * @param _amount amount of token transferred
	 * @return uint256 amount of token to be collected as a transaction fee for redistribution for this transaction
	 */
	function calculateTaxFee(uint256 _amount) private view returns (uint256) {
		/// _taxFee represent the percentage of net transaction value to be collected
		return _amount.mul(_taxFee).div(
			10**2
		);
	}

	/**
	 * @notice Calculate the amount of token to be collected as a transaction fee for providing liquidity
	 * @param _amount amount of token transferred
	 * @return uint256 amount of token to be collected as a transaction fee for providing liquidity for this transaction
	 */
	function calculateLiquidityFee(uint256 _amount) private view returns (uint256) {
		/// _liquidityFee represent the percentage of net transaction value to be collected
		return _amount.mul(_liquidityFee).div(
			10**2
		);
	}
	
	/**
	 * @notice Temporarily disable transaction fee in _tokenTransfer for address in _isExcludedFromFee mapping
	 */
	function removeAllFee() private {
		/// No need to set _taxFee and _liquidityFee if it is already 0
		if(_taxFee == 0 && _liquidityFee == 0) return;
		/// Store the original _taxFee and _liquidityFee
		_previousTaxFee = _taxFee;
		_previousLiquidityFee = _liquidityFee;
		/// Set both fee to 0
		_taxFee = 0;
		_liquidityFee = 0;
	}
	
	/**
	 * @notice Revert removeAllFee() after transaction is over
	 */
	function restoreAllFee() private {
		/// Restore _taxFee and _liquidityFee
		_taxFee = _previousTaxFee;
		_liquidityFee = _previousLiquidityFee;
	}
	
	/**
	 * @notice Check whether an address is excluded from the transaction fee
	 * @param account address to be checked
	 * @return bool whether the address is excluded from transaction fee
	 */
	function isExcludedFromFee(address account) public view returns(bool) {
		return _isExcludedFromFee[account];
	}

	/**
	 * @notice Set allowance that spender can spend on behalf of the owner
	 * @param owner address that authorizes spender to spend on its behalf
	 * @param spender address that is authorized to spend owner's token
	 * @param amount amount of token the spender is authorized to spend
	 */
	function _approve(address owner, address spender, uint256 amount) private {
		/// Make sure owner and spender is not 0 address and set the allowance
		/// The use of reflection does not affect the allowance mechanics
		require(owner != address(0), "ERC20: approve from the zero address");
		require(spender != address(0), "ERC20: approve to the zero address");
		_allowances[owner][spender] = amount;
		emit Approval(owner, spender, amount);
	}

	/**
	 * @notice Transfer 'amount' of token from 'from' to 'to'
	 * @param from the sender address
	 * @param to the recipient address
	 * @param amount amount of token sent out from sender address, note that recipient will receive less token
	 */
	function _transfer(address from, address to, uint256 amount) private {
		/// Require non-zero address and non-zero amount to be transferred
		require(from != address(0), "ERC20: transfer from the zero address");
		require(to != address(0), "ERC20: transfer to the zero address");
		require(amount > 0, "Transfer amount must be greater than zero");
		/// Require amount of token transferred to be under the _maxTxAmount limit
		if (from != owner() && to != owner()) {
			require(amount <= _maxTxAmount, "Transfer amount exceeds the maxTxAmount.");
		}
		/// Get the amount of token collected from transaction fee and is now owned by this contract
		uint256 contractTokenBalance = balanceOf(address(this));
		/// Since this contract cannot transfer more than _maxTxAmount at once either,
		/// set the contractTokenBalance to _maxTxAmount if it exceeds it
		if (contractTokenBalance >= _maxTxAmount) {
			contractTokenBalance = _maxTxAmount;
		}
		/// Check if contractTokenBalance exceeds the minimum required to provide liquidity to swap
		bool overMinTokenBalance = contractTokenBalance >= numTokensSellToAddToLiquidity;
		/// Initiate to provide liquidity if,
		/// 1. contractTokenBalance exceeds the minimum required to provide liquidity to swap
		/// 2. We are not already providing liquidity as indicated by the inSwapAndLiquify (i.e. it should be false)
		/// 3. Sender is not a pancakeswap pair otherwise it may affect the swap itself
		/// 4. Contract is set to provide liquidity via the swapAndLiquifyEnabled flag
		if (
			overMinTokenBalance &&
			!inSwapAndLiquify &&
			from != pancakeSwapProxy.pancakeswapV2Pair() &&
			swapAndLiquifyEnabled
		) {
			/// We will provide numTokensSellToAddToLiquidity amount of token for liquidity
			contractTokenBalance = numTokensSellToAddToLiquidity;
			/// Add liquidity to swap
			swapAndLiquify(contractTokenBalance);
		}
		/// Boolean that indicates if fee should be deducted from transfer
		bool takeFee = true;
		// If any account belongs to _isExcludedFromFee account then remove the fee
		if(_isExcludedFromFee[from] || _isExcludedFromFee[to]){
			takeFee = false;
		}
		/// Transfer amount of token, it will take transaction fee if takeFee is true
		_tokenTransfer(from, to, amount, takeFee);
	}

	/**
	 * @notice Swap token into BUSD and provide liquidity to Pancakeswap
	 * @param contractTokenBalance amount of token to be swap and provide liquidity
	 */
	function swapAndLiquify(uint256 contractTokenBalance) private lockTheSwap {
		/// Transfer the TDM token to be swapped and liquify into PancakeSwapProxy without fee
		_tokenTransfer(address(this), address(pancakeSwapProxy), contractTokenBalance, false);
		/// Call PancakeSwapProxy to swap and liquify for us
		(uint256 half, uint256 busdSwapped, uint256 otherHalf) = pancakeSwapProxy.swapAndLiquify();
		/// Emit event
		emit SwapAndLiquify(half, busdSwapped, otherHalf);
	}

	/**
	 * @notice Process the transaction and take transaction fee if takeFee is true
	 * @param sender transaction sender
	 * @param recipient transaction recipient
	 * @param amount amount of token transferred
	 * @param takeFee whether to take transaction fee or not
	 */
	function _tokenTransfer(address sender, address recipient, uint256 amount, bool takeFee) private {
		/// Exclude fee if takeFee is false
		if (!takeFee) {
			removeAllFee();
		}
		/// Process transaction depending on whether sender/receipient is an excluded address or not
		if (_isExcluded[sender] && !_isExcluded[recipient]) {
			_transferFromExcluded(sender, recipient, amount);
		} 
		else if (!_isExcluded[sender] && _isExcluded[recipient]) {
			_transferToExcluded(sender, recipient, amount);
		} 
		else if (_isExcluded[sender] && _isExcluded[recipient]) {
			_transferBothExcluded(sender, recipient, amount);
		} 
		else {
			_transferStandard(sender, recipient, amount);
		}
		/// Restore fee if takeFee is false after transaction
		if (!takeFee) {
			restoreAllFee();
		}
	}

	/**
	 * @notice Handle token transfer between non-excluded addresses.
	 * @param sender transaction sender
	 * @param recipient transaction recipient
	 * @param tAmount amount of token transferred
	 */
	function _transferStandard(address sender, address recipient, uint256 tAmount) private {
		/// Get transaction parameters
		(uint256 rAmount, uint256 rTransferAmount, uint256 rFee, uint256 tTransferAmount, uint256 tFee, uint256 tLiquidity) = _getValues(tAmount);
		/// For non-excluded addresses, it is only necessary to transfer the reflection from sender to recipient
		_rOwned[sender] = _rOwned[sender].sub(rAmount);
		_rOwned[recipient] = _rOwned[recipient].add(rTransferAmount);
		/// Take liquidity from the transaction by reserving tLiquidity amount of token to this contract
		_takeLiquidity(tLiquidity);
		/// Record the amount of reflection burned and amount of token redistributed to all token holders
		_reflectFee(rFee, tFee);
		/// Emit transfer event
		emit Transfer(sender, recipient, tTransferAmount);
	}

	/**
	 * @notice Handle token transfer from a non-excluded address to an excluded address.
	 * @param sender transaction sender
	 * @param recipient transaction recipient
	 * @param tAmount amount of token transferred
	 */
	function _transferToExcluded(address sender, address recipient, uint256 tAmount) private {
		/// Get transaction parameters
		(uint256 rAmount, uint256 rTransferAmount, uint256 rFee, uint256 tTransferAmount, uint256 tFee, uint256 tLiquidity) = _getValues(tAmount);
		/// For non-excluded sender, it is only necessary to subtract equivalent amount of reflection
		_rOwned[sender] = _rOwned[sender].sub(rAmount);
		/// For excluded recipient, it is necessary to add both reflection in _rOwned and actual token counter in _tOwned
		_tOwned[recipient] = _tOwned[recipient].add(tTransferAmount);
		_rOwned[recipient] = _rOwned[recipient].add(rTransferAmount);           
		/// Take liquidity from the transaction by reserving tLiquidity amount of token to this contract
		_takeLiquidity(tLiquidity);
		/// Record the amount of reflection burned and amount of token redistributed to all token holders
		_reflectFee(rFee, tFee);
		/// Emit transfer event
		emit Transfer(sender, recipient, tTransferAmount);
	}

	/**
	 * @notice Handle token transfer from an excluded address to a non-excluded address.
	 * @param sender transaction sender
	 * @param recipient transaction recipient
	 * @param tAmount amount of token transferred
	 */
	function _transferFromExcluded(address sender, address recipient, uint256 tAmount) private {
		/// Get transaction parameters
		(uint256 rAmount, uint256 rTransferAmount, uint256 rFee, uint256 tTransferAmount, uint256 tFee, uint256 tLiquidity) = _getValues(tAmount);
		/// For excluded recipient, it is necessary to subtract both reflection in _rOwned and actual token counter in _tOwned
		_tOwned[sender] = _tOwned[sender].sub(tAmount);
		_rOwned[sender] = _rOwned[sender].sub(rAmount);
		/// For non-excluded sender, it is only necessary to add the corresponding amount of reflection
		_rOwned[recipient] = _rOwned[recipient].add(rTransferAmount);   
		/// Take liquidity from the transaction by reserving tLiquidity amount of token to this contract
		_takeLiquidity(tLiquidity);
		/// Record the amount of reflection burned and amount of token redistributed to all token holders
		_reflectFee(rFee, tFee);
		/// Emit transfer event
		emit Transfer(sender, recipient, tTransferAmount);
	}

	/**
	 * @notice Handle token transfer between excluded addresses.
	 * @param sender transaction sender
	 * @param recipient transaction recipient
	 * @param tAmount amount of token transferred
	 */
	function _transferBothExcluded(address sender, address recipient, uint256 tAmount) private {
		/// Get transaction parameters
		(uint256 rAmount, uint256 rTransferAmount, uint256 rFee, uint256 tTransferAmount, uint256 tFee, uint256 tLiquidity) = _getValues(tAmount);
		/// For excluded addresses, it is necessary to transfer both the reflection in _rOwned and actual token count in _tOwned from sender to recipient
		_tOwned[sender] = _tOwned[sender].sub(tAmount);
		_rOwned[sender] = _rOwned[sender].sub(rAmount);
		_tOwned[recipient] = _tOwned[recipient].add(tTransferAmount);
		_rOwned[recipient] = _rOwned[recipient].add(rTransferAmount);      
		/// Take liquidity from the transaction by reserving tLiquidity amount of token to this contract
		_takeLiquidity(tLiquidity);
		/// Record the amount of reflection burned and amount of token redistributed to all token holders
		_reflectFee(rFee, tFee);
		/// Emit transfer event
		emit Transfer(sender, recipient, tTransferAmount);
	}

}