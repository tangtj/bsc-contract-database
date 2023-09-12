/**
 *Submitted for verification at BscScan.com on 2021-02-20
*/

// SPDX-License-Identifier: GPL-3.0-only

// File: @pancakeswap/pancake-swap-lib/contracts/GSN/Context.sol


pragma solidity >=0.4.0;

/*
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with GSN meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
contract Context {
    // Empty internal constructor, to prevent people from mistakenly deploying
    // an instance of this contract, which should be used via inheritance.
    constructor() internal {}

    function _msgSender() internal view returns (address payable) {
        return msg.sender;
    }

    function _msgData() internal view returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

// File: @pancakeswap/pancake-swap-lib/contracts/access/Ownable.sol


pragma solidity >=0.4.0;


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
contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() internal {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
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
        require(_owner == _msgSender(), 'Ownable: caller is not the owner');
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     */
    function _transferOwnership(address newOwner) internal {
        require(newOwner != address(0), 'Ownable: new owner is the zero address');
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

// File: @pancakeswap/pancake-swap-lib/contracts/token/BEP20/IBEP20.sol


pragma solidity >=0.4.0;

interface IBEP20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the token decimals.
     */
    function decimals() external view returns (uint8);

    /**
     * @dev Returns the token symbol.
     */
    function symbol() external view returns (string memory);

    /**
     * @dev Returns the token name.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the bep token owner.
     */
    function getOwner() external view returns (address);

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
    function allowance(address _owner, address spender) external view returns (uint256);

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
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

// File: contracts/GTokenRegistry.sol

pragma solidity ^0.6.0;


/**
 * @notice This contract allows external agents to detect when new GTokens
 *         are deployed to the network.
 */
contract GTokenRegistry is Ownable
{
	/**
	 * @notice Registers a new gToken.
	 * @param _growthToken The address of the token being registered.
	 * @param _oldGrowthToken The address of the token implementation
	 *                        being replaced, for upgrades, or 0x0 0therwise.
	 */
	function registerNewToken(address _growthToken, address _oldGrowthToken) external onlyOwner
	{
		emit NewToken(_growthToken, _oldGrowthToken);
	}

	event NewToken(address indexed _growthToken, address indexed _oldGrowthToken);
}

// File: contracts/GExchange.sol

pragma solidity ^0.6.0;

/**
 * @dev Custom and uniform interface to a decentralized exchange. It is used
 *      to estimate and convert funds whenever necessary. This furnishes
 *      client contracts with the flexibility to replace conversion strategy
 *      and routing, dynamically, by delegating these operations to different
 *      external contracts that share this common interface. See
 *      GExchangeImpl.sol for further documentation.
 */
interface GExchange
{
	// view functions
	function calcConversionFromInput(address _from, address _to, uint256 _inputAmount) external view returns (uint256 _outputAmount);
	function calcConversionFromOutput(address _from, address _to, uint256 _outputAmount) external view returns (uint256 _inputAmount);

	// open functions
	function convertFundsFromInput(address _from, address _to, uint256 _inputAmount, uint256 _minOutputAmount) external returns (uint256 _outputAmount);
	function convertFundsFromOutput(address _from, address _to, uint256 _outputAmount, uint256 _maxInputAmount) external returns (uint256 _inputAmount);
}

// File: @pancakeswap/pancake-swap-lib/contracts/math/SafeMath.sol


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
        require(c >= a, 'SafeMath: addition overflow');

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
        return sub(a, b, 'SafeMath: subtraction overflow');
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
        require(c / a == b, 'SafeMath: multiplication overflow');

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
        return div(a, b, 'SafeMath: division by zero');
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
        return mod(a, b, 'SafeMath: modulo by zero');
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

// File: @pancakeswap/pancake-swap-lib/contracts/utils/Address.sol


pragma solidity ^0.6.2;

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
        require(address(this).balance >= amount, 'Address: insufficient balance');

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{value: amount}('');
        require(success, 'Address: unable to send value, recipient may have reverted');
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
        return functionCall(target, data, 'Address: low-level call failed');
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
        return _functionCallWithValue(target, data, 0, errorMessage);
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
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, 'Address: low-level call with value failed');
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
        require(address(this).balance >= value, 'Address: insufficient balance for call');
        return _functionCallWithValue(target, data, value, errorMessage);
    }

    function _functionCallWithValue(
        address target,
        bytes memory data,
        uint256 weiValue,
        string memory errorMessage
    ) private returns (bytes memory) {
        require(isContract(target), 'Address: call to non-contract');

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{value: weiValue}(data);
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

// File: @pancakeswap/pancake-swap-lib/contracts/token/BEP20/SafeBEP20.sol


pragma solidity ^0.6.0;




/**
 * @title SafeBEP20
 * @dev Wrappers around BEP20 operations that throw on failure (when the token
 * contract returns false). Tokens that return no value (and instead revert or
 * throw on failure) are also supported, non-reverting calls are assumed to be
 * successful.
 * To use this library you can add a `using SafeBEP20 for IBEP20;` statement to your contract,
 * which allows you to call the safe operations as `token.safeTransfer(...)`, etc.
 */
library SafeBEP20 {
    using SafeMath for uint256;
    using Address for address;

    function safeTransfer(
        IBEP20 token,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(
        IBEP20 token,
        address from,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    /**
     * @dev Deprecated. This function has issues similar to the ones found in
     * {IBEP20-approve}, and its usage is discouraged.
     *
     * Whenever possible, use {safeIncreaseAllowance} and
     * {safeDecreaseAllowance} instead.
     */
    function safeApprove(
        IBEP20 token,
        address spender,
        uint256 value
    ) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        // solhint-disable-next-line max-line-length
        require(
            (value == 0) || (token.allowance(address(this), spender) == 0),
            'SafeBEP20: approve from non-zero to non-zero allowance'
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(
        IBEP20 token,
        address spender,
        uint256 value
    ) internal {
        uint256 newAllowance = token.allowance(address(this), spender).add(value);
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(
        IBEP20 token,
        address spender,
        uint256 value
    ) internal {
        uint256 newAllowance = token.allowance(address(this), spender).sub(
            value,
            'SafeBEP20: decreased allowance below zero'
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     */
    function _callOptionalReturn(IBEP20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We use {Address.functionCall} to perform this call, which verifies that
        // the target address contains contract code and also asserts for success in the low-level call.

        bytes memory returndata = address(token).functionCall(data, 'SafeBEP20: low-level call failed');
        if (returndata.length > 0) {
            // Return data is optional
            // solhint-disable-next-line max-line-length
            require(abi.decode(returndata, (bool)), 'SafeBEP20: BEP20 operation did not succeed');
        }
    }
}

// File: contracts/modules/Transfers.sol

pragma solidity ^0.6.0;



/**
 * @dev This library abstracts ERC-20 operations in the context of the current
 * contract.
 */
library Transfers
{
	using SafeBEP20 for IBEP20;

	/**
	 * @dev Retrieves a given ERC-20 token balance for the current contract.
	 * @param _token An ERC-20 compatible token address.
	 * @return _balance The current contract balance of the given ERC-20 token.
	 */
	function _getBalance(address _token) internal view returns (uint256 _balance)
	{
		return IBEP20(_token).balanceOf(address(this));
	}

	/**
	 * @dev Allows a spender to access a given ERC-20 balance for the current contract.
	 * @param _token An ERC-20 compatible token address.
	 * @param _to The spender address.
	 * @param _amount The exact spending allowance amount.
	 */
	function _approveFunds(address _token, address _to, uint256 _amount) internal
	{
		uint256 _allowance = IBEP20(_token).allowance(address(this), _to);
		if (_allowance > _amount) {
			IBEP20(_token).safeDecreaseAllowance(_to, _allowance - _amount);
		}
		else
		if (_allowance < _amount) {
			IBEP20(_token).safeIncreaseAllowance(_to, _amount - _allowance);
		}
	}

	/**
	 * @dev Transfer a given ERC-20 token amount into the current contract.
	 * @param _token An ERC-20 compatible token address.
	 * @param _from The source address.
	 * @param _amount The amount to be transferred.
	 */
	function _pullFunds(address _token, address _from, uint256 _amount) internal
	{
		if (_amount == 0) return;
		IBEP20(_token).safeTransferFrom(_from, address(this), _amount);
	}

	/**
	 * @dev Transfer a given ERC-20 token amount from the current contract.
	 * @param _token An ERC-20 compatible token address.
	 * @param _to The target address.
	 * @param _amount The amount to be transferred.
	 */
	function _pushFunds(address _token, address _to, uint256 _amount) internal
	{
		if (_amount == 0) return;
		IBEP20(_token).safeTransfer(_to, _amount);
	}
}

// File: contracts/interop/PancakeSwap.sol

pragma solidity ^0.6.0;


/**
 * @dev Minimal set of declarations for Uniswap V2 interoperability.
 */
interface Factory
{
	function createPair(address _tokenA, address _tokenB) external returns (address _pair);
}

interface PoolToken is IBEP20
{
}

interface Pair is PoolToken
{
	function token0() external view returns (address _token0);
	function token1() external view returns (address _token1);
	function getReserves() external view returns (uint112 _reserve0, uint112 _reserve1, uint32 _blockTimestampLast);

	function mint(address _to) external returns (uint256 _liquidity);
}

interface Router01
{
	function WETH() external pure returns (address _token);
	function getAmountOut(uint256 _amountIn, uint256 _reserveIn, uint256 _reserveOut) external pure returns (uint256 _amountOut);
	function getAmountsOut(uint256 _amountIn, address[] calldata _path) external view returns (uint[] memory _amounts);
	function getAmountsIn(uint256 _amountOut, address[] calldata _path) external view returns (uint[] memory _amounts);

	function addLiquidity(address _tokenA, address _tokenB, uint256 _amountADesired, uint256 _amountBDesired, uint256 _amountAMin, uint256 _amountBMin, address _to, uint256 _deadline) external returns (uint256 _amountA, uint256 _amountB, uint256 _liquidity);
	function removeLiquidity(address _tokenA, address _tokenB, uint256 _liquidity, uint256 _amountAMin, uint256 _amountBMin, address _to, uint256 _deadline) external returns (uint256 _amountA, uint256 _amountB);
	function swapETHForExactTokens(uint256 _amountOut, address[] calldata _path, address _to, uint256 _deadline) external payable returns (uint256[] memory _amounts);
	function swapExactTokensForTokens(uint256 _amountIn, uint256 _amountOutMin, address[] calldata _path, address _to, uint256 _deadline) external returns (uint256[] memory _amounts);
	function swapTokensForExactTokens(uint256 _amountOut, uint256 _amountInMax, address[] calldata _path, address _to, uint256 _deadline) external returns (uint256[] memory _amounts);
}

interface Router02 is Router01
{
}

// File: contracts/modules/PancakeSwapExchangeAbstraction.sol

pragma solidity ^0.6.0;



/**
 * @dev This library abstracts the Uniswap V2 token exchange functionality.
 */
library PancakeSwapExchangeAbstraction
{
	/**
	 * @dev Calculates how much output to be received from the given input
	 *      when converting between two assets.
	 * @param _from The input asset address.
	 * @param _to The output asset address.
	 * @param _inputAmount The input asset amount to be provided.
	 * @return _outputAmount The output asset amount to be received.
	 */
	function _calcConversionFromInput(address _router, address _from, address _to, uint256 _inputAmount) internal view returns (uint256 _outputAmount)
	{
		address _WBNB = Router02(_router).WETH();
		address[] memory _path = _buildPath(_from, _WBNB, _to);
		return Router02(_router).getAmountsOut(_inputAmount, _path)[_path.length - 1];
	}

	/**
	 * @dev Calculates how much input to be received the given the output
	 *      when converting between two assets.
	 * @param _from The input asset address.
	 * @param _to The output asset address.
	 * @param _outputAmount The output asset amount to be received.
	 * @return _inputAmount The input asset amount to be provided.
	 */
	function _calcConversionFromOutput(address _router, address _from, address _to, uint256 _outputAmount) internal view returns (uint256 _inputAmount)
	{
		address _WBNB = Router02(_router).WETH();
		address[] memory _path = _buildPath(_from, _WBNB, _to);
		return Router02(_router).getAmountsIn(_outputAmount, _path)[0];
	}

	/**
	 * @dev Convert funds between two assets given the desired input amount.
	 * @param _from The input asset address.
	 * @param _to The output asset address.
	 * @param _inputAmount The exact input asset amount to be provided.
	 * @param _minOutputAmount The output asset minimum amount to be received.
	 * @return _outputAmount The output asset amount actually received.
	 */
	function _convertFundsFromInput(address _router, address _from, address _to, uint256 _inputAmount, uint256 _minOutputAmount) internal returns (uint256 _outputAmount)
	{
		address _WBNB = Router02(_router).WETH();
		address[] memory _path = _buildPath(_from, _WBNB, _to);
		Transfers._approveFunds(_from, _router, _inputAmount);
		return Router02(_router).swapExactTokensForTokens(_inputAmount, _minOutputAmount, _path, address(this), uint256(-1))[_path.length - 1];
	}

	/**
	 * @dev Convert funds between two assets given the desired output amount.
	 * @param _from The input asset address.
	 * @param _to The output asset address.
	 * @param _outputAmount The exact output asset amount to be received.
	 * @param _maxInputAmount The input asset maximum amount to be provided.
	 * @return _inputAmount The input asset amount actually provided.
	 */
	function _convertFundsFromOutput(address _router, address _from, address _to, uint256 _outputAmount, uint256 _maxInputAmount) internal returns (uint256 _inputAmount)
	{
		address _WBNB = Router02(_router).WETH();
		address[] memory _path = _buildPath(_from, _WBNB, _to);
		Transfers._approveFunds(_from, _router, _maxInputAmount);
		_inputAmount = Router02(_router).swapTokensForExactTokens(_outputAmount, _maxInputAmount, _path, address(this), uint256(-1))[0];
		Transfers._approveFunds(_from, _router, 0);
		return _inputAmount;
	}

	/**
	 * @dev Builds a routing path for conversion possibly using a thrid
	 *      token (likely WETH) as intermediate.
	 * @param _from The input asset address.
	 * @param _through The intermediate asset address.
	 * @param _to The output asset address.
	 * @return _path The route to perform conversion.
	 */
	function _buildPath(address _from, address _through, address _to) private pure returns (address[] memory _path)
	{
		assert(_from != _to);
		if (_from == _through || _to == _through) {
			_path = new address[](2);
			_path[0] = _from;
			_path[1] = _to;
			return _path;
		} else {
			_path = new address[](3);
			_path[0] = _from;
			_path[1] = _through;
			_path[2] = _to;
			return _path;
		}
	}
}

// File: contracts/GExchangeImpl.sol

pragma solidity ^0.6.0;




/**
 * @notice This contract implements the GExchange interface routing token
 *         conversions via a Uniswap V2 compatible exchange.
 */
contract GExchangeImpl is GExchange
{
	address public immutable router;

	constructor (address _router)
		public
	{
		router = _router;
	}

	/**
	 * @notice Computes the amount of tokens to be received upon conversion.
	 * @param _from The contract address of the ERC-20 token to convert from.
	 * @param _to The contract address of the ERC-20 token to convert to.
	 * @param _inputAmount The amount of the _from token to be provided (may be 0).
	 * @return _outputAmount The amount of the _to token to be received (may be 0).
	 */
	function calcConversionFromInput(address _from, address _to, uint256 _inputAmount) external view override returns (uint256 _outputAmount)
	{
		return PancakeSwapExchangeAbstraction._calcConversionFromInput(router, _from, _to, _inputAmount);
	}

	/**
	 * @notice Computes the amount of tokens to be provided upon conversion.
	 * @param _from The contract address of the ERC-20 token to convert from.
	 * @param _to The contract address of the ERC-20 token to convert to.
	 * @param _outputAmount The amount of the _to token to be received (may be 0).
	 * @return _inputAmount The amount of the _from token to be provided (may be 0).
	 */
	function calcConversionFromOutput(address _from, address _to, uint256 _outputAmount) external view override returns (uint256 _inputAmount)
	{
		return PancakeSwapExchangeAbstraction._calcConversionFromOutput(router, _from, _to, _outputAmount);
	}

	/**
	 * @notice Converts a given token amount to another token, as long as it
	 *         meets the minimum taken amount. Amounts are debited from and
	 *         and credited to the caller contract. It may fail if the
	 *         minimum output amount cannot be met.
	 * @param _from The contract address of the ERC-20 token to convert from.
	 * @param _to The contract address of the ERC-20 token to convert to.
	 * @param _inputAmount The amount of the _from token to be provided (may be 0).
	 * @param _minOutputAmount The minimum amount of the _to token to be received (may be 0).
	 * @return _outputAmount The actual amount of the _to token received (may be 0).
	 */
	function convertFundsFromInput(address _from, address _to, uint256 _inputAmount, uint256 _minOutputAmount) external override returns (uint256 _outputAmount)
	{
		address _sender = msg.sender;
		Transfers._pullFunds(_from, _sender, _inputAmount);
		_outputAmount = PancakeSwapExchangeAbstraction._convertFundsFromInput(router, _from, _to, _inputAmount, _minOutputAmount);
		Transfers._pushFunds(_to, _sender, _outputAmount);
		return _outputAmount;
	}

	/**
	 * @notice Converts a given token amount to another token, as long as it
	 *         meets the maximum given amount. Amounts are debited from and
	 *         and credited to the caller contract. It may fail if the
	 *         maximum input amount cannot be met.
	 * @param _from The contract address of the ERC-20 token to convert from.
	 * @param _to The contract address of the ERC-20 token to convert to.
	 * @param _outputAmount The amount of the _to token to be received (may be 0).
	 * @param _maxInputAmount The maximum amount of the _from token to be provided (may be 0).
	 * @return _inputAmount The actual amount of the _from token provided (may be 0).
	 */
	function convertFundsFromOutput(address _from, address _to, uint256 _outputAmount, uint256 _maxInputAmount) external override returns (uint256 _inputAmount)
	{
		address _sender = msg.sender;
		Transfers._pullFunds(_from, _sender, _maxInputAmount);
		_inputAmount = PancakeSwapExchangeAbstraction._convertFundsFromOutput(router, _from, _to, _outputAmount, _maxInputAmount);
		Transfers._pushFunds(_from, _sender, _maxInputAmount - _inputAmount);
		Transfers._pushFunds(_to, _sender, _outputAmount);
		return _inputAmount;
	}
}

// File: @pancakeswap/pancake-swap-lib/contracts/token/BEP20/BEP20.sol


pragma solidity >=0.4.0;






/**
 * @dev Implementation of the {IBEP20} interface.
 *
 * This implementation is agnostic to the way tokens are created. This means
 * that a supply mechanism has to be added in a derived contract using {_mint}.
 * For a generic mechanism see {BEP20PresetMinterPauser}.
 *
 * TIP: For a detailed writeup see our guide
 * https://forum.zeppelin.solutions/t/how-to-implement-BEP20-supply-mechanisms/226[How
 * to implement supply mechanisms].
 *
 * We have followed general OpenZeppelin guidelines: functions revert instead
 * of returning `false` on failure. This behavior is nonetheless conventional
 * and does not conflict with the expectations of BEP20 applications.
 *
 * Additionally, an {Approval} event is emitted on calls to {transferFrom}.
 * This allows applications to reconstruct the allowance for all accounts just
 * by listening to said events. Other implementations of the EIP may not emit
 * these events, as it isn't required by the specification.
 *
 * Finally, the non-standard {decreaseAllowance} and {increaseAllowance}
 * functions have been added to mitigate the well-known issues around setting
 * allowances. See {IBEP20-approve}.
 */
contract BEP20 is Context, IBEP20, Ownable {
    using SafeMath for uint256;
    using Address for address;

    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    /**
     * @dev Sets the values for {name} and {symbol}, initializes {decimals} with
     * a default value of 18.
     *
     * To select a different value for {decimals}, use {_setupDecimals}.
     *
     * All three of these values are immutable: they can only be set once during
     * construction.
     */
    constructor(string memory name, string memory symbol) public {
        _name = name;
        _symbol = symbol;
        _decimals = 18;
    }

    /**
     * @dev Returns the bep token owner.
     */
    function getOwner() external override view returns (address) {
        return owner();
    }

    /**
     * @dev Returns the token name.
     */
    function name() public override view returns (string memory) {
        return _name;
    }

    /**
     * @dev Returns the token decimals.
     */
    function decimals() public override view returns (uint8) {
        return _decimals;
    }

    /**
     * @dev Returns the token symbol.
     */
    function symbol() public override view returns (string memory) {
        return _symbol;
    }

    /**
     * @dev See {BEP20-totalSupply}.
     */
    function totalSupply() public override view returns (uint256) {
        return _totalSupply;
    }

    /**
     * @dev See {BEP20-balanceOf}.
     */
    function balanceOf(address account) public override view returns (uint256) {
        return _balances[account];
    }

    /**
     * @dev See {BEP20-transfer}.
     *
     * Requirements:
     *
     * - `recipient` cannot be the zero address.
     * - the caller must have a balance of at least `amount`.
     */
    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    /**
     * @dev See {BEP20-allowance}.
     */
    function allowance(address owner, address spender) public override view returns (uint256) {
        return _allowances[owner][spender];
    }

    /**
     * @dev See {BEP20-approve}.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    /**
     * @dev See {BEP20-transferFrom}.
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {BEP20};
     *
     * Requirements:
     * - `sender` and `recipient` cannot be the zero address.
     * - `sender` must have a balance of at least `amount`.
     * - the caller must have allowance for `sender`'s tokens of at least
     * `amount`.
     */
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            _msgSender(),
            _allowances[sender][_msgSender()].sub(amount, 'BEP20: transfer amount exceeds allowance')
        );
        return true;
    }

    /**
     * @dev Atomically increases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {BEP20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    /**
     * @dev Atomically decreases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {BEP20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     * - `spender` must have allowance for the caller of at least
     * `subtractedValue`.
     */
    function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender].sub(subtractedValue, 'BEP20: decreased allowance below zero')
        );
        return true;
    }

    /**
     * @dev Creates `amount` tokens and assigns them to `msg.sender`, increasing
     * the total supply.
     *
     * Requirements
     *
     * - `msg.sender` must be the token owner
     */
    function mint(uint256 amount) public onlyOwner returns (bool) {
        _mint(_msgSender(), amount);
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
    ) internal {
        require(sender != address(0), 'BEP20: transfer from the zero address');
        require(recipient != address(0), 'BEP20: transfer to the zero address');

        _balances[sender] = _balances[sender].sub(amount, 'BEP20: transfer amount exceeds balance');
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }

    /** @dev Creates `amount` tokens and assigns them to `account`, increasing
     * the total supply.
     *
     * Emits a {Transfer} event with `from` set to the zero address.
     *
     * Requirements
     *
     * - `to` cannot be the zero address.
     */
    function _mint(address account, uint256 amount) internal {
        require(account != address(0), 'BEP20: mint to the zero address');

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
     * Requirements
     *
     * - `account` cannot be the zero address.
     * - `account` must have at least `amount` tokens.
     */
    function _burn(address account, uint256 amount) internal {
        require(account != address(0), 'BEP20: burn from the zero address');

        _balances[account] = _balances[account].sub(amount, 'BEP20: burn amount exceeds balance');
        _totalSupply = _totalSupply.sub(amount);
        emit Transfer(account, address(0), amount);
    }

    /**
     * @dev Sets `amount` as the allowance of `spender` over the `owner`s tokens.
     *
     * This is internal function is equivalent to `approve`, and can be used to
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
    ) internal {
        require(owner != address(0), 'BEP20: approve from the zero address');
        require(spender != address(0), 'BEP20: approve to the zero address');

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    /**
     * @dev Destroys `amount` tokens from `account`.`amount` is then deducted
     * from the caller's allowance.
     *
     * See {_burn} and {_approve}.
     */
    function _burnFrom(address account, uint256 amount) internal {
        _burn(account, amount);
        _approve(
            account,
            _msgSender(),
            _allowances[account][_msgSender()].sub(amount, 'BEP20: burn amount exceeds allowance')
        );
    }
}

// File: contracts/modules/Math.sol

pragma solidity ^0.6.0;

/**
 * @dev This library implements auxiliary math definitions.
 */
library Math
{
	function _min(uint256 _amount1, uint256 _amount2) internal pure returns (uint256 _minAmount)
	{
		return _amount1 < _amount2 ? _amount1 : _amount2;
	}

	function _max(uint256 _amount1, uint256 _amount2) internal pure returns (uint256 _maxAmount)
	{
		return _amount1 > _amount2 ? _amount1 : _amount2;
	}

	function _sqrt(uint256 _y) internal pure returns (uint256 _z)
	{
		if (_y > 3) {
			_z = _y;
			uint256 _x = _y / 2 + 1;
			while (_x < _z) {
				_z = _x;
				_x = (_y / _x + _x) / 2;
			}
			return _z;
		}
		if (_y > 0) return 1;
		return 0;
	}
}

// File: contracts/GRewardToken.sol

pragma solidity ^0.6.0;



contract GRewardToken is BEP20
{
	constructor (string memory _name, string memory _symbol, uint8 _decimals, uint256 _initialSupply)
		BEP20(_name, _symbol) public
	{
		address _sender = msg.sender;
		require(_decimals == 18, "unsupported decimals");
		_mint(_sender, _initialSupply);
	}

	function allocateReward(uint256 _amount) external
	{
		address _from = msg.sender;
		address _to = address(this);
		_transfer(_from, _to, _amount);
	}

	function mint(address _to, uint256 _amount) external onlyOwner
	{
		address _from = address(this);
		uint256 _balance = balanceOf(_from);
		_transfer(_from, _to, Math._min(_balance, _amount));
	}
}

// File: @pancakeswap/pancake-swap-lib/contracts/utils/ReentrancyGuard.sol


pragma solidity ^0.6.0;

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
contract ReentrancyGuard {
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

    constructor () internal {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and make it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}

// File: contracts/GRewardStakeToken.sol

pragma solidity 0.6.12;






contract GRewardStakeToken is BEP20, ReentrancyGuard
{
	address public immutable rewardToken;

	constructor (string memory _name, string memory _symbol, uint8 _decimals, address _rewardToken)
		BEP20(_name, _symbol) public
	{
		require(_decimals == 18, "unsupported decimals");
		rewardToken = _rewardToken;
	}

	function mint(address _to, uint256 _amount) external onlyOwner nonReentrant
	{
		_mint(_to, _amount);
	}

	function burn(address _from ,uint256 _amount) external onlyOwner nonReentrant
	{
		_burn(_from, _amount);
	}

	function safeRewardTransfer(address _to, uint256 _amount) external onlyOwner nonReentrant
	{
		uint256 _balance = Transfers._getBalance(rewardToken);
		Transfers._pushFunds(rewardToken, _to, Math._min(_balance, _amount));
	}
}

// File: contracts/MasterChef.sol

pragma solidity 0.6.12;







// import "@nomiclabs/buidler/console.sol";

interface IMigratorChef {
    // Perform LP token migration from legacy PancakeSwap to CakeSwap.
    // Take the current LP token address and return the new LP token address.
    // Migrator should have full access to the caller's LP token.
    // Return the new LP token address.
    //
    // XXX Migrator must have allowance access to PancakeSwap LP tokens.
    // CakeSwap must mint EXACTLY the same amount of CakeSwap LP tokens or
    // else something bad will happen. Traditional PancakeSwap does not
    // do that so be careful!
    function migrate(IBEP20 token) external returns (IBEP20);
}

// MasterChef is the master of Cake. He can make Cake and he is a fair guy.
//
// Note that it's ownable and the owner wields tremendous power. The ownership
// will be transferred to a governance smart contract once CAKE is sufficiently
// distributed and the community can show to govern itself.
//
// Have fun reading it. Hopefully it's bug-free. God bless.
contract MasterChef is Ownable {
    using SafeMath for uint256;
    using SafeBEP20 for IBEP20;

    // Info of each user.
    struct UserInfo {
        uint256 amount;     // How many LP tokens the user has provided.
        uint256 rewardDebt; // Reward debt. See explanation below.
        //
        // We do some fancy math here. Basically, any point in time, the amount of CAKEs
        // entitled to a user but is pending to be distributed is:
        //
        //   pending reward = (user.amount * pool.accCakePerShare) - user.rewardDebt
        //
        // Whenever a user deposits or withdraws LP tokens to a pool. Here's what happens:
        //   1. The pool's `accCakePerShare` (and `lastRewardBlock`) gets updated.
        //   2. User receives the pending reward sent to his/her address.
        //   3. User's `amount` gets updated.
        //   4. User's `rewardDebt` gets updated.
    }

    // Info of each pool.
    struct PoolInfo {
        IBEP20 lpToken;           // Address of LP token contract.
        uint256 allocPoint;       // How many allocation points assigned to this pool. CAKEs to distribute per block.
        uint256 lastRewardBlock;  // Last block number that CAKEs distribution occurs.
        uint256 accCakePerShare; // Accumulated CAKEs per share, times 1e12. See below.
    }

    // The CAKE TOKEN!
    GRewardToken public cake;
    // The SYRUP TOKEN!
    GRewardStakeToken public syrup;
    // Dev address.
    address public devaddr;
    // CAKE tokens created per block.
    uint256 public cakePerBlock;
    // Bonus muliplier for early cake makers.
    uint256 public BONUS_MULTIPLIER = 1;
    // The migrator contract. It has a lot of power. Can only be set through governance (owner).
    IMigratorChef public migrator;

    // Info of each pool.
    PoolInfo[] public poolInfo;
    // Info of each user that stakes LP tokens.
    mapping (uint256 => mapping (address => UserInfo)) public userInfo;
    // Total allocation points. Must be the sum of all allocation points in all pools.
    uint256 public totalAllocPoint = 0;
    // The block number when CAKE mining starts.
    uint256 public startBlock;

    event Deposit(address indexed user, uint256 indexed pid, uint256 amount);
    event Withdraw(address indexed user, uint256 indexed pid, uint256 amount);
    event EmergencyWithdraw(address indexed user, uint256 indexed pid, uint256 amount);

    constructor(
        GRewardToken _cake,
        GRewardStakeToken _syrup,
        address _devaddr,
        uint256 _cakePerBlock,
        uint256 _startBlock
    ) public {
        cake = _cake;
        syrup = _syrup;
        devaddr = _devaddr;
        cakePerBlock = _cakePerBlock;
        startBlock = _startBlock;

        // staking pool
        poolInfo.push(PoolInfo({
            lpToken: _cake,
            allocPoint: 1000,
            lastRewardBlock: startBlock,
            accCakePerShare: 0
        }));

        totalAllocPoint = 1000;

    }

    function updateCakePerBlock(uint256 _cakePerBlock) public onlyOwner {
        cakePerBlock = _cakePerBlock;
    }

    function updateMultiplier(uint256 multiplierNumber) public onlyOwner {
        BONUS_MULTIPLIER = multiplierNumber;
    }

    function poolLength() external view returns (uint256) {
        return poolInfo.length;
    }

    // Add a new lp to the pool. Can only be called by the owner.
    // XXX DO NOT add the same LP token more than once. Rewards will be messed up if you do.
    function add(uint256 _allocPoint, IBEP20 _lpToken, bool _withUpdate) public onlyOwner {
        if (_withUpdate) {
            massUpdatePools();
        }
        uint256 lastRewardBlock = block.number > startBlock ? block.number : startBlock;
        totalAllocPoint = totalAllocPoint.add(_allocPoint);
        poolInfo.push(PoolInfo({
            lpToken: _lpToken,
            allocPoint: _allocPoint,
            lastRewardBlock: lastRewardBlock,
            accCakePerShare: 0
        }));
        updateStakingPool();
    }

    // Update the given pool's CAKE allocation point. Can only be called by the owner.
    function set(uint256 _pid, uint256 _allocPoint, bool _withUpdate) public onlyOwner {
        if (_withUpdate) {
            massUpdatePools();
        }
        uint256 prevAllocPoint = poolInfo[_pid].allocPoint;
        poolInfo[_pid].allocPoint = _allocPoint;
        if (prevAllocPoint != _allocPoint) {
            totalAllocPoint = totalAllocPoint.sub(prevAllocPoint).add(_allocPoint);
            updateStakingPool();
        }
    }

    function updateStakingPool() internal {
        uint256 length = poolInfo.length;
        uint256 points = 0;
        for (uint256 pid = 1; pid < length; ++pid) {
            points = points.add(poolInfo[pid].allocPoint);
        }
        if (points != 0) {
            points = points.div(3);
            totalAllocPoint = totalAllocPoint.sub(poolInfo[0].allocPoint).add(points);
            poolInfo[0].allocPoint = points;
        }
    }

    // Set the migrator contract. Can only be called by the owner.
    function setMigrator(IMigratorChef _migrator) public onlyOwner {
        migrator = _migrator;
    }

    // Migrate lp token to another lp contract. Can be called by anyone. We trust that migrator contract is good.
    function migrate(uint256 _pid) public {
        require(address(migrator) != address(0), "migrate: no migrator");
        PoolInfo storage pool = poolInfo[_pid];
        IBEP20 lpToken = pool.lpToken;
        uint256 bal = lpToken.balanceOf(address(this));
        lpToken.safeApprove(address(migrator), bal);
        IBEP20 newLpToken = migrator.migrate(lpToken);
        require(bal == newLpToken.balanceOf(address(this)), "migrate: bad");
        pool.lpToken = newLpToken;
    }

    // Return reward multiplier over the given _from to _to block.
    function getMultiplier(uint256 _from, uint256 _to) public view returns (uint256) {
        return _to.sub(_from).mul(BONUS_MULTIPLIER);
    }

    // View function to see pending CAKEs on frontend.
    function pendingCake(uint256 _pid, address _user) external view returns (uint256) {
        PoolInfo storage pool = poolInfo[_pid];
        UserInfo storage user = userInfo[_pid][_user];
        uint256 accCakePerShare = pool.accCakePerShare;
        uint256 lpSupply = pool.lpToken.balanceOf(address(this));
        if (block.number > pool.lastRewardBlock && lpSupply != 0) {
            uint256 multiplier = getMultiplier(pool.lastRewardBlock, block.number);
            uint256 cakeReward = multiplier.mul(cakePerBlock).mul(pool.allocPoint).div(totalAllocPoint);
            accCakePerShare = accCakePerShare.add(cakeReward.mul(1e12).div(lpSupply));
        }
        return user.amount.mul(accCakePerShare).div(1e12).sub(user.rewardDebt);
    }

    // Update reward variables for all pools. Be careful of gas spending!
    function massUpdatePools() public {
        uint256 length = poolInfo.length;
        for (uint256 pid = 0; pid < length; ++pid) {
            updatePool(pid);
        }
    }


    // Update reward variables of the given pool to be up-to-date.
    function updatePool(uint256 _pid) public {
        PoolInfo storage pool = poolInfo[_pid];
        if (block.number <= pool.lastRewardBlock) {
            return;
        }
        uint256 lpSupply = pool.lpToken.balanceOf(address(this));
        if (lpSupply == 0) {
            pool.lastRewardBlock = block.number;
            return;
        }
        uint256 multiplier = getMultiplier(pool.lastRewardBlock, block.number);
        uint256 cakeReward = multiplier.mul(cakePerBlock).mul(pool.allocPoint).div(totalAllocPoint);
        cake.mint(devaddr, cakeReward.div(10));
        cake.mint(address(syrup), cakeReward);
        pool.accCakePerShare = pool.accCakePerShare.add(cakeReward.mul(1e12).div(lpSupply));
        pool.lastRewardBlock = block.number;
    }

    // Deposit LP tokens to MasterChef for CAKE allocation.
    function deposit(uint256 _pid, uint256 _amount) public {

        require (_pid != 0, 'deposit CAKE by staking');

        PoolInfo storage pool = poolInfo[_pid];
        UserInfo storage user = userInfo[_pid][msg.sender];
        updatePool(_pid);
        if (user.amount > 0) {
            uint256 pending = user.amount.mul(pool.accCakePerShare).div(1e12).sub(user.rewardDebt);
            if(pending > 0) {
                safeCakeTransfer(msg.sender, pending);
            }
        }
        if (_amount > 0) {
            pool.lpToken.safeTransferFrom(address(msg.sender), address(this), _amount);
            user.amount = user.amount.add(_amount);
        }
        user.rewardDebt = user.amount.mul(pool.accCakePerShare).div(1e12);
        emit Deposit(msg.sender, _pid, _amount);
    }

    // Withdraw LP tokens from MasterChef.
    function withdraw(uint256 _pid, uint256 _amount) public {

        require (_pid != 0, 'withdraw CAKE by unstaking');
        PoolInfo storage pool = poolInfo[_pid];
        UserInfo storage user = userInfo[_pid][msg.sender];
        require(user.amount >= _amount, "withdraw: not good");

        updatePool(_pid);
        uint256 pending = user.amount.mul(pool.accCakePerShare).div(1e12).sub(user.rewardDebt);
        if(pending > 0) {
            safeCakeTransfer(msg.sender, pending);
        }
        if(_amount > 0) {
            user.amount = user.amount.sub(_amount);
            pool.lpToken.safeTransfer(address(msg.sender), _amount);
        }
        user.rewardDebt = user.amount.mul(pool.accCakePerShare).div(1e12);
        emit Withdraw(msg.sender, _pid, _amount);
    }

    // Stake CAKE tokens to MasterChef
    function enterStaking(uint256 _amount) public {
        PoolInfo storage pool = poolInfo[0];
        UserInfo storage user = userInfo[0][msg.sender];
        updatePool(0);
        if (user.amount > 0) {
            uint256 pending = user.amount.mul(pool.accCakePerShare).div(1e12).sub(user.rewardDebt);
            if(pending > 0) {
                safeCakeTransfer(msg.sender, pending);
            }
        }
        if(_amount > 0) {
            pool.lpToken.safeTransferFrom(address(msg.sender), address(this), _amount);
            user.amount = user.amount.add(_amount);
        }
        user.rewardDebt = user.amount.mul(pool.accCakePerShare).div(1e12);

        syrup.mint(msg.sender, _amount);
        emit Deposit(msg.sender, 0, _amount);
    }

    // Withdraw CAKE tokens from STAKING.
    function leaveStaking(uint256 _amount) public {
        PoolInfo storage pool = poolInfo[0];
        UserInfo storage user = userInfo[0][msg.sender];
        require(user.amount >= _amount, "withdraw: not good");
        updatePool(0);
        uint256 pending = user.amount.mul(pool.accCakePerShare).div(1e12).sub(user.rewardDebt);
        if(pending > 0) {
            safeCakeTransfer(msg.sender, pending);
        }
        if(_amount > 0) {
            user.amount = user.amount.sub(_amount);
            pool.lpToken.safeTransfer(address(msg.sender), _amount);
        }
        user.rewardDebt = user.amount.mul(pool.accCakePerShare).div(1e12);

        syrup.burn(msg.sender, _amount);
        emit Withdraw(msg.sender, 0, _amount);
    }

    // Withdraw without caring about rewards. EMERGENCY ONLY.
    function emergencyWithdraw(uint256 _pid) public {
        PoolInfo storage pool = poolInfo[_pid];
        UserInfo storage user = userInfo[_pid][msg.sender];
        pool.lpToken.safeTransfer(address(msg.sender), user.amount);
        emit EmergencyWithdraw(msg.sender, _pid, user.amount);
        user.amount = 0;
        user.rewardDebt = 0;
    }

    // Safe cake transfer function, just in case if rounding error causes pool to not have enough CAKEs.
    function safeCakeTransfer(address _to, uint256 _amount) internal {
        syrup.safeRewardTransfer(_to, _amount);
    }

    // Update dev address by the previous dev.
    function dev(address _devaddr) public {
        require(msg.sender == devaddr, "dev: wut?");
        devaddr = _devaddr;
    }
}

// File: contracts/network/$.sol

pragma solidity ^0.6.0;

/**
 * @dev This library is provided for convenience. It is the single source for
 *      the current network and all related hardcoded contract addresses.
 */
library $
{
	enum Network { Bscmain, Chapel }

	Network constant NETWORK = Network.Bscmain;

	function network() internal pure returns (Network _network)
	{
		uint256 _chainid;
		assembly { _chainid := chainid() }
		if (_chainid == 56) return Network.Bscmain;
		if (_chainid == 97) return Network.Chapel;
		require(false, "unsupported network");
	}

	address constant ETH =
		NETWORK == Network.Bscmain ? 0x2170Ed0880ac9A755fd29B2688956BD959F933F8 :
		NETWORK == Network.Chapel ? 0xd66c6B4F0be8CE5b39D52E0Fd1344c389929B378 :
		0x0000000000000000000000000000000000000000;

	address constant WBNB =
		NETWORK == Network.Bscmain ? 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c :
		NETWORK == Network.Chapel ? 0xd21BB48C35e7021Bf387a8b259662dC06a9df984 :
		0x0000000000000000000000000000000000000000;

	address constant PancakeSwap_FACTORY =
		NETWORK == Network.Bscmain ? 0xBCfCcbde45cE874adCB698cC183deBcF17952812 :
		NETWORK == Network.Chapel ? 0x1f3F51f2a7Bfe32f34446b3213C130EBB9e287A1 :
		0x0000000000000000000000000000000000000000;

	address constant PancakeSwap_ROUTER02 =
		NETWORK == Network.Bscmain ? 0x05fF2B0DB69458A0750badebc4f9e13aDd608C7F :
		NETWORK == Network.Chapel ? 0x428E5Be012f8D9cca6852479e522B75519E10980 :
		0x0000000000000000000000000000000000000000;

	address constant PancakeSwap_MASTERCHEF =
		NETWORK == Network.Bscmain ? 0x73feaa1eE314F8c655E354234017bE2193C9E24E :
		NETWORK == Network.Chapel ? 0x7C83Cab4B208A0cD5a1b222D8e6f9099C8F37897 :
		0x0000000000000000000000000000000000000000;
}

// File: contracts/modules/PancakeSwapLiquidityPoolAbstraction.sol

pragma solidity ^0.6.0;


// import { Babylonian } from "@uniswap/lib/contracts/libraries/Babylonian.sol";





/**
 * @dev This library provides functionality to facilitate adding/removing
 * single-asset liquidity to/from a Uniswap V2 pool.
 */
library PancakeSwapLiquidityPoolAbstraction
{
	using SafeMath for uint256;

	function _estimateJoinPool(address _pair, address _token, uint256 _amount) internal view returns (uint256 _shares)
	{
		if (_amount == 0) return 0;
		address _router = $.PancakeSwap_ROUTER02;
		address _token0 = Pair(_pair).token0();
		(uint256 _reserve0, uint256 _reserve1,) = Pair(_pair).getReserves();
		uint256 _balance = _token == _token0 ? _reserve0 : _reserve1;
		uint256 _otherBalance = _token == _token0 ? _reserve1 : _reserve0;
		uint256 _totalSupply = Pair(_pair).totalSupply();
		uint256 _swapAmount = _calcSwapOutputFromInput(_balance, _amount);
		if (_swapAmount == 0) _swapAmount = _amount / 2;
		uint256 _leftAmount = _amount.sub(_swapAmount);
		uint256 _otherAmount = Router02(_router).getAmountOut(_swapAmount, _balance, _otherBalance);
		_shares = Math._min(_totalSupply.mul(_leftAmount) / _balance.add(_swapAmount), _totalSupply.mul(_otherAmount) / _otherBalance.sub(_otherAmount));
		return _shares;
	}

	function _estimateExitPool(address _pair, address _token, uint256 _shares) internal view returns (uint256 _amount)
	{
		if (_shares == 0) return 0;
		address _router = $.PancakeSwap_ROUTER02;
		address _token0 = Pair(_pair).token0();
		(uint256 _reserve0, uint256 _reserve1,) = Pair(_pair).getReserves();
		uint256 _balance = _token == _token0 ? _reserve0 : _reserve1;
		uint256 _otherBalance = _token == _token0 ? _reserve1 : _reserve0;
		uint256 _totalSupply = Pair(_pair).totalSupply();
		uint256 _baseAmount = _balance.mul(_shares) / _totalSupply;
		uint256 _swapAmount = _otherBalance.mul(_shares) / _totalSupply;
		uint256 _additionalAmount = Router02(_router).getAmountOut(_swapAmount, _otherBalance.sub(_swapAmount), _balance.sub(_baseAmount));
		_amount = _baseAmount.add(_additionalAmount);
		return _amount;
	}

	function _joinPool(address _pair, address _token, uint256 _amount) internal returns (uint256 _shares)
	{
		if (_amount == 0) return 0;
		address _router = $.PancakeSwap_ROUTER02;
		address _token0 = Pair(_pair).token0();
		address _token1 = Pair(_pair).token1();
		require(_token == _token0 || _token == _token1, "invalid token");
		address _otherToken = _token == _token0 ? _token1 : _token0;
		(uint256 _reserve0, uint256 _reserve1,) = Pair(_pair).getReserves();
		uint256 _swapAmount = _calcSwapOutputFromInput(_token == _token0 ? _reserve0 : _reserve1, _amount);
		if (_swapAmount == 0) _swapAmount = _amount / 2;
		uint256 _leftAmount = _amount.sub(_swapAmount);
		Transfers._approveFunds(_token, _router, _amount);
		address[] memory _path = new address[](2);
		_path[0] = _token;
		_path[1] = _otherToken;
		uint256 _otherAmount = Router02(_router).swapExactTokensForTokens(_swapAmount, 1, _path, address(this), uint256(-1))[1];
		Transfers._approveFunds(_otherToken, _router, _otherAmount);
		(,,_shares) = Router02(_router).addLiquidity(_token, _otherToken, _leftAmount, _otherAmount, 1, 1, address(this), uint256(-1));
		// slippage must be checked by caller
		return _shares;
	}

	function _exitPool(address _pair, address _token, uint256 _shares) internal returns (uint256 _amount)
	{
		if (_shares == 0) return 0;
		address _router = $.PancakeSwap_ROUTER02;
		address _token0 = Pair(_pair).token0();
		address _token1 = Pair(_pair).token1();
		require(_token == _token0 || _token == _token1, "invalid token");
		address _otherToken = _token == _token0 ? _token1 : _token0;
		Transfers._approveFunds(_pair, _router, _shares);
		(uint256 _baseAmount, uint256 _swapAmount) = Router02(_router).removeLiquidity(_token, _otherToken, _shares, 1, 1, address(this), uint256(-1));
		Transfers._approveFunds(_otherToken, _router, _swapAmount);
		address[] memory _path = new address[](2);
		_path[0] = _otherToken;
		_path[1] = _token;
		uint256 _additionalAmount = Router02(_router).swapExactTokensForTokens(_swapAmount, 1, _path, address(this), uint256(-1))[1];
		_amount = _baseAmount.add(_additionalAmount);
		// slippage must be checked by caller
		return _amount;
	}

	function _calcSwapOutputFromInput(uint256 _reserveAmount, uint256 _inputAmount) private pure returns (uint256 _outputAmount)
	{
		return Math._sqrt(_reserveAmount.mul(_inputAmount.mul(3988000).add(_reserveAmount.mul(3988009)))).sub(_reserveAmount.mul(1997)) / 1994;
	}
}

// File: contracts/GRewardCompoundingStrategyToken.sol

pragma solidity ^0.6.0;










contract GRewardCompoundingStrategyToken is BEP20, ReentrancyGuard
{
	using SafeMath for uint256;

	uint256 constant MAXIMUM_PERFORMANCE_FEE = 50e16; // 50%
	uint256 constant DEFAULT_PERFORMANCE_FEE = 10e16; // 10%

	address immutable masterChef;
	uint256 immutable pid;

	address public immutable /*override*/ reserveToken;
	address public immutable /*override*/ routingToken;
	address public immutable /*override*/ rewardToken;

	address public exchange;
	address public treasury;

	uint256 public /*override*/ performanceFee = DEFAULT_PERFORMANCE_FEE;

	uint256 lastTotalSupply = 1;
	uint256 lastTotalReserve = 1;

	constructor (string memory _name, string memory _symbol, uint8 _decimals, address _masterChef, uint256 _pid, address _routingToken)
		BEP20(_name, _symbol) public
	{
		address _treasury = msg.sender;
		(IBEP20 _lpToken,,,) = MasterChef(_masterChef).poolInfo(_pid);
		address _reserveToken = address(_lpToken);
		address _rewardToken = address(MasterChef(_masterChef).cake());
		require(_decimals == 18, "unsupported decimals");
		require(_pid >= 1);
		require(_routingToken == Pair(_reserveToken).token0() || _routingToken == Pair(_reserveToken).token1(), "invalid token");
		masterChef = _masterChef;
		pid = _pid;
		reserveToken = _reserveToken;
		routingToken = _routingToken;
		rewardToken = _rewardToken;
		treasury = _treasury;
		_mint(address(1), 1); // avoids division by zero
	}

	function totalReserve() public view /*override*/ returns (uint256 _totalReserve)
	{
		(_totalReserve,) = MasterChef(masterChef).userInfo(pid, address(this));
		if (_totalReserve == uint256(-1)) return _totalReserve;
		return _totalReserve + 1; // avoids division by zero
	}

	function calcSharesFromCost(uint256 _cost) public view /*override*/ returns (uint256 _shares)
	{
		return _cost.mul(totalSupply()).div(totalReserve());
	}

	function calcCostFromShares(uint256 _shares) public view /*override*/ returns (uint256 _cost)
	{
		return _shares.mul(totalReserve()).div(totalSupply());
	}

	function estimatePendingRewards() external view /*override*/ returns (uint256 _rewardsCost)
	{
		require(exchange != address(0), "exchange not set");
		uint256 _rewardAmount = Transfers._getBalance(rewardToken);
		uint256 _routingAmount = _rewardAmount;
		if (routingToken != rewardToken) {
			_routingAmount = GExchange(exchange).calcConversionFromInput(rewardToken, routingToken, _rewardAmount);
		}
		return PancakeSwapLiquidityPoolAbstraction._estimateJoinPool(reserveToken, routingToken, _routingAmount);
	}

	function pendingFees() external view /*override*/ returns (uint256 _feeShares)
	{
		return _calcFees();
	}

	function deposit(uint256 _cost) external /*override*/ nonReentrant
	{
		address _from = msg.sender;
		uint256 _shares = calcSharesFromCost(_cost);
		Transfers._pullFunds(reserveToken, _from, _cost);
		Transfers._approveFunds(reserveToken, masterChef, _cost);
		MasterChef(masterChef).deposit(pid, _cost);
		_mint(_from, _shares);
	}

	function withdraw(uint256 _shares) external /*override*/ nonReentrant
	{
		address _from = msg.sender;
		uint256 _cost = calcCostFromShares(_shares);
		MasterChef(masterChef).withdraw(pid, _cost);
		Transfers._pushFunds(reserveToken, _from, _cost);
		_burn(_from, _shares);
	}

	function gulpRewards(uint256 _minRewardCost) external /*override*/ nonReentrant
	{
		require(exchange != address(0), "exchange not set");
		uint256 _rewardAmount = Transfers._getBalance(rewardToken);
		uint256 _routingAmount = _rewardAmount;
		if (routingToken != rewardToken) {
			Transfers._approveFunds(rewardToken, exchange, _rewardAmount);
			_routingAmount = GExchange(exchange).convertFundsFromInput(rewardToken, routingToken, _rewardAmount, 0);
		}
		uint256 _rewardCost = PancakeSwapLiquidityPoolAbstraction._joinPool(reserveToken, routingToken, _routingAmount);
	        require(_rewardCost >= _minRewardCost, "high slippage");
		Transfers._approveFunds(reserveToken, masterChef, _rewardCost);
		MasterChef(masterChef).deposit(pid, _rewardCost);
	}

	function gulpFees() external /*override*/ nonReentrant
	{
		uint256 _feeShares = _calcFees();
		if (_feeShares > 0) {
			lastTotalSupply = totalSupply();
			lastTotalReserve = totalReserve();
			_mint(treasury, _feeShares);
		}
	}

	function setExchange(address _newExchange) external /*override*/ onlyOwner nonReentrant
	{
		address _oldExchange = exchange;
		exchange = _newExchange;
		emit ChangeExchange(_oldExchange, _newExchange);
	}

	function setTreasury(address _newTreasury) external /*override*/ onlyOwner nonReentrant
	{
		require(_newTreasury != address(0), "invalid address");
		address _oldTreasury = treasury;
		treasury = _newTreasury;
		emit ChangeTreasury(_oldTreasury, _newTreasury);
	}

	function setPerformanceFee(uint256 _newPerformanceFee) external /*override*/ onlyOwner nonReentrant
	{
		require(_newPerformanceFee <= MAXIMUM_PERFORMANCE_FEE, "invalid rate");
		uint256 _oldPerformanceFee = performanceFee;
		performanceFee = _newPerformanceFee;
		emit ChangePerformanceFee(_oldPerformanceFee, _newPerformanceFee);
	}

	function _calcFees() internal view returns (uint256 _feeShares)
	{
		uint256 _oldTotalSupply = lastTotalSupply;
		uint256 _oldTotalReserve = lastTotalReserve;

		uint256 _newTotalSupply = totalSupply();
		uint256 _newTotalReserve = totalReserve();

		// calculates the profit using the following formula
		// ((P1 - P0) * S1 * f) / P1
		// where P1 = R1 / S1 and P0 = R0 / S0
		uint256 _positive = _oldTotalSupply.mul(_newTotalReserve);
		uint256 _negative = _newTotalSupply.mul(_oldTotalReserve);
		if (_positive > _negative) {
			uint256 _profitCost = _positive.sub(_negative).div(_oldTotalSupply);
			uint256 _feeCost = _profitCost.mul(performanceFee).div(1e18);
			return calcSharesFromCost(_feeCost);
		}

		return 0;
	}

	event ChangeExchange(address _oldExchange, address _newExchange);
	event ChangeTreasury(address _oldTreasury, address _newTreasury);
	event ChangePerformanceFee(uint256 _oldPerformanceFee, uint256 _newPerformanceFee);
}

// File: contracts/GDeflationaryToken.sol

pragma solidity ^0.6.0;



contract GDeflationaryToken is BEP20, ReentrancyGuard
{
	constructor (string memory _name, string memory _symbol, uint8 _decimals, uint256 _initialSupply)
		BEP20(_name, _symbol) public
	{
		address _sender = msg.sender;
		require(_decimals == 18, "unsupported decimals");
		_mint(_sender, _initialSupply);
	}

	function burn(uint256 _amount) external onlyOwner nonReentrant
	{
		address _sender = msg.sender;
		_burn(_sender, _amount);
	}
}

// File: contracts/GTokens.sol

pragma solidity ^0.6.0;






contract gROOT is GRewardToken
{
	constructor (uint256 _totalSupply)
		GRewardToken("growth Root Token", "gROOT", 18, _totalSupply) public
	{
	}
}

contract stkgROOT is GRewardStakeToken
{
	constructor (address _gROOT)
		GRewardStakeToken("stake gROOT", "stkgROOT", 18, _gROOT) public
	{
	}
}

contract SAFE is GDeflationaryToken
{
	constructor (uint256 _totalSupply)
		GDeflationaryToken("rAAVE Debt Token", "SAFE", 18, _totalSupply) public
	{
	}
}

contract stkgROOT_BNB is GRewardCompoundingStrategyToken
{
	constructor (address _masterChef, uint256 _pid, address _gROOT)
		GRewardCompoundingStrategyToken("stake gROOT/BNB", "stkgROOT/BNB", 18, _masterChef, _pid, _gROOT) public
	{
	}
}

// File: contracts/interop/WrappedBNB.sol

pragma solidity ^0.6.0;


/**
 * @dev Minimal set of declarations for WBNB interoperability.
 */
interface WBNB is IBEP20
{
	function deposit() external payable;
	function withdraw(uint256 _amount) external;
}

// File: contracts/modules/Wrapping.sol

pragma solidity ^0.6.0;



/**
 * @dev This library abstracts Wrapped Ether operations.
 */
library Wrapping
{
	/**
	 * @dev Sends some ETH to the Wrapped Ether contract in exchange for WETH.
	 * @param _amount The amount of ETH to be wrapped.
	 */
	function _wrap(uint256 _amount) internal
	{
		if (_amount == 0) return;
		WBNB($.WBNB).deposit{value: _amount}();
	}

	/**
	 * @dev Receives some ETH from the Wrapped Ether contract in exchange for WETH.
	 *      Note that the contract using this library function must declare a
	 *      payable receive/fallback function.
	 * @param _amount The amount of ETH to be unwrapped.
	 */
	function _unwrap(uint256 _amount) internal
	{
		if (_amount == 0) return;
		WBNB($.WBNB).withdraw(_amount);
	}
}

// File: contracts/Deployer.sol

pragma solidity ^0.6.0;














contract Deployer is Ownable
{
	address constant GROOT_TREASURY1 = 0x2165fa4a32B9c228cD55713f77d2e977297D03e8; // G
	address constant GROOT_TREASURY2 = 0x6248f5783A1F908F7fbB651bb3Ca27BF7c9f5022; // M
	address constant GROOT_TREASURY3 = 0xBf70B751BB1FC725bFbC4e68C4Ec4825708766c5; // S

	address constant GROOT_INITIAL_STAKE_HOLDER = 0x5c327D395D0617f5b6ad6E8Da5dCBb35A6Be5b11;
	address constant GROOT_DEFAULT_FEE_COLLECTOR = 0xB0632a01ee778E09625BcE2a257e221b49E79696;

	address constant GROOT_CONTRACTS_OWNER = 0xBf70B751BB1FC725bFbC4e68C4Ec4825708766c5;

	uint256 constant GROOT_TOTAL_SUPPLY = 20000e18; // 20,000
	uint256 constant GROOT_TREASURY_ALLOCATION = 10430e18; // 10,430
	uint256 constant GROOT_LIQUIDITY_ALLOCATION = 70e18; // 70
	uint256 constant GROOT_FARMING_ALLOCATION = 8000e18; // 8,000
	uint256 constant GROOT_INITIAL_FARMING_ALLOCATION = 0e18; // 0
	uint256 constant GROOT_AIRDROP_ALLOCATION = 1500e18; // 1,500

	uint256 constant SAFE_TOTAL_SUPPLY = 168675e18; // 168,675
	uint256 constant SAFE_AIRDROP_ALLOCATION = 168675e18; // 168,675

	uint256 constant WBNB_LIQUIDITY_ALLOCATION = 300e18; // 300

	uint256 constant AVERAGE_BLOCK_TIME = 3 seconds;
	uint256 constant INITIAL_GROOT_PER_MONTH = 0e18; // 0
	uint256 constant INITIAL_GROOT_PER_BLOCK = AVERAGE_BLOCK_TIME * INITIAL_GROOT_PER_MONTH / 30 days;

	bool constant STAKE_LP_SHARES = false;

	struct Payment {
		address receiver;
		uint256 amount;
	}

	Payment[] public paymentsGROOT;
	Payment[] public paymentsSAFE;

	uint256 public rewardStartBlock;
	address public pancakeSwapRouter;
	address public registry;
	address public exchange;
	address public SAFE;
	address public gROOT;
	address public stkgROOT;
	address public masterChef;
	address public gROOT_WBNB;
	address public stkgROOT_BNB;

	bool public deployed = false;
	bool public airdropped = false;

	constructor () public
	{
		require($.NETWORK == $.network(), "wrong network");
	}

	function registerReceiversGROOT(address[] memory _receivers, uint256[] memory _amounts) external onlyOwner
	{
		require(_receivers.length == _amounts.length, "length mismatch");
		for (uint256 _i = 0; _i < _receivers.length; _i++) {
			address _receiver = _receivers[_i];
			uint256 _amount = _amounts[_i];
			require(_amount > 0, "zero amount");
			paymentsGROOT.push(Payment({ receiver: _receiver, amount: _amount }));
		}
	}

	function registerReceiversSAFE(address[] memory _receivers, uint256[] memory _amounts) external onlyOwner
	{
		require(_receivers.length == _amounts.length, "length mismatch");
		for (uint256 _i = 0; _i < _receivers.length; _i++) {
			address _receiver = _receivers[_i];
			uint256 _amount = _amounts[_i];
			require(_amount > 0, "zero amount");
			paymentsSAFE.push(Payment({ receiver: _receiver, amount: _amount }));
		}
	}

	function deploy() payable external onlyOwner
	{
		uint256 _amount = msg.value;
		require(_amount == WBNB_LIQUIDITY_ALLOCATION, "BNB amount mismatch");

		require(!deployed, "deploy unavailable");

		// wraps LP liquidity BNB into WBNB
		Wrapping._wrap(WBNB_LIQUIDITY_ALLOCATION);

		// initialize handy fields
		rewardStartBlock = block.number;
		pancakeSwapRouter = $.PancakeSwap_ROUTER02;

		// deploy helper contracts
		registry = LibDeployer1.publishGTokenRegistry();
		exchange = LibDeployer1.publishGExchangeImpl(pancakeSwapRouter);

		// deploy SAFE token
		SAFE = LibDeployer1.publishSAFE(SAFE_TOTAL_SUPPLY);

		// deploy gROOT token and MasterChef for reward distribution
		gROOT = LibDeployer2.publishGROOT(GROOT_TOTAL_SUPPLY);
		stkgROOT = LibDeployer2.publishSTKGROOT(gROOT);
		masterChef = LibDeployer3.publishMasterChef(gROOT, stkgROOT, INITIAL_GROOT_PER_BLOCK, rewardStartBlock);
		GRewardToken(gROOT).allocateReward(GROOT_INITIAL_FARMING_ALLOCATION);

		// create gROOT/BNB LP and register it for reward distribution
		gROOT_WBNB = Factory($.PancakeSwap_FACTORY).createPair(gROOT, $.WBNB);
		MasterChef(masterChef).add(1000, IBEP20(gROOT_WBNB), false);

		// adds the liquidity to the gROOT/BNB LP
		Transfers._pushFunds(gROOT, gROOT_WBNB, GROOT_LIQUIDITY_ALLOCATION);
		Transfers._pushFunds($.WBNB, gROOT_WBNB, WBNB_LIQUIDITY_ALLOCATION);
		uint256 _lpshares = Pair(gROOT_WBNB).mint(address(this));

		// create and configure compounding strategy contract for gROOT/BNB
		stkgROOT_BNB = LibDeployer4.publishSTKGROOTBNB(masterChef, 1, gROOT);
		GRewardCompoundingStrategyToken(stkgROOT_BNB).setExchange(exchange);
		GRewardCompoundingStrategyToken(stkgROOT_BNB).setTreasury(GROOT_DEFAULT_FEE_COLLECTOR);

		// stake gROOT/BNB LP shares into strategy contract
		if (STAKE_LP_SHARES) {
			Transfers._approveFunds(gROOT_WBNB, stkgROOT_BNB, _lpshares);
			GRewardCompoundingStrategyToken(stkgROOT_BNB).deposit(_lpshares);
			Transfers._pushFunds(stkgROOT_BNB, GROOT_INITIAL_STAKE_HOLDER, _lpshares);
		} else {
			Transfers._pushFunds(gROOT_WBNB, GROOT_INITIAL_STAKE_HOLDER, _lpshares);
		}

		// transfer treasury funds to the treasury
		Transfers._pushFunds(gROOT, GROOT_TREASURY1, GROOT_TREASURY_ALLOCATION / 3);
		Transfers._pushFunds(gROOT, GROOT_TREASURY2, GROOT_TREASURY_ALLOCATION / 3);
		Transfers._pushFunds(gROOT, GROOT_TREASURY3, GROOT_TREASURY_ALLOCATION - 2 * (GROOT_TREASURY_ALLOCATION / 3));

		// transfer farming funds to the treasury
		Transfers._pushFunds(gROOT, GROOT_TREASURY1, GROOT_FARMING_ALLOCATION / 3);
		Transfers._pushFunds(gROOT, GROOT_TREASURY2, GROOT_FARMING_ALLOCATION / 3);
		Transfers._pushFunds(gROOT, GROOT_TREASURY3, GROOT_FARMING_ALLOCATION - 2 * (GROOT_FARMING_ALLOCATION / 3));

		require(Transfers._getBalance($.WBNB) == 0, "WBNB left over");
		require(Transfers._getBalance(gROOT_WBNB) == 0, "gROOT_WBNB left over");
		require(Transfers._getBalance(stkgROOT_BNB) == 0, "stkgROOT_BNB left over");
		require(Transfers._getBalance(gROOT) == GROOT_AIRDROP_ALLOCATION, "gROOT amount mismatch");
		require(Transfers._getBalance(SAFE) == SAFE_AIRDROP_ALLOCATION, "SAFE amount mismatch");

		// register tokens
		GTokenRegistry(registry).registerNewToken(SAFE, address(0));
		GTokenRegistry(registry).registerNewToken(gROOT, address(0));
		GTokenRegistry(registry).registerNewToken(stkgROOT, address(0));
		GTokenRegistry(registry).registerNewToken(stkgROOT_BNB, address(0));

		// transfer ownerships
		Ownable(gROOT).transferOwnership(masterChef);
		Ownable(stkgROOT).transferOwnership(masterChef);

		Ownable(registry).transferOwnership(GROOT_CONTRACTS_OWNER);
		Ownable(SAFE).transferOwnership(GROOT_CONTRACTS_OWNER);
		Ownable(masterChef).transferOwnership(GROOT_CONTRACTS_OWNER);
		Ownable(stkgROOT_BNB).transferOwnership(GROOT_CONTRACTS_OWNER);

		// wrap up the deployment
		deployed = true;
		emit DeployPerformed();
	}

	function airdrop() external onlyOwner
	{
		require(deployed, "airdrop unavailable");
		require(!airdropped, "airdrop unavailable");

		require(Transfers._getBalance(gROOT) == GROOT_AIRDROP_ALLOCATION, "gROOT amount mismatch");
		require(Transfers._getBalance(SAFE) == SAFE_AIRDROP_ALLOCATION, "SAFE amount mismatch");

		// airdrops gROOT
		for (uint256 _i = 0; _i < paymentsGROOT.length; _i++) {
			Payment storage _payment = paymentsGROOT[_i];
			Transfers._pushFunds(gROOT, _payment.receiver, _payment.amount);
		}

		// ardrops SAFE
		for (uint256 _i = 0; _i < paymentsSAFE.length; _i++) {
			Payment storage _payment = paymentsSAFE[_i];
			Transfers._pushFunds(SAFE, _payment.receiver, _payment.amount);
		}

		require(Transfers._getBalance(gROOT) == 0, "gROOT left over");
		require(Transfers._getBalance(SAFE) == 0, "SAFE left over");

		renounceOwnership();

		airdropped = true;
		emit AirdropPerformed();
	}

	event DeployPerformed();
	event AirdropPerformed();
}

library LibDeployer1
{
	function publishGTokenRegistry() public returns (address _address)
	{
		return address(new GTokenRegistry());
	}

	function publishGExchangeImpl(address _router) public returns (address _address)
	{
		return address(new GExchangeImpl(_router));
	}

	function publishSAFE(uint256 _totalSupply) public returns (address _address)
	{
		return address(new SAFE(_totalSupply));
	}
}

library LibDeployer2
{
	function publishGROOT(uint256 _totalSupply) public returns (address _address)
	{
		return address(new gROOT(_totalSupply));
	}

	function publishSTKGROOT(address _rewardToken) public returns (address _address)
	{
		return address(new stkgROOT(_rewardToken));
	}
}

library LibDeployer3
{
	function publishMasterChef(address _rewardToken, address _rewardStakeToken, uint256 _rewardPerBlock, uint256 _rewardStartBlock) public returns (address _address)
	{
		return address(new MasterChef(GRewardToken(_rewardToken), GRewardStakeToken(_rewardStakeToken), _rewardToken, _rewardPerBlock, _rewardStartBlock));
	}
}

library LibDeployer4
{
	function publishSTKGROOTBNB(address _masterChef, uint256 _pid, address _routingToken) public returns (address _address)
	{
		return address(new stkgROOT_BNB(_masterChef, _pid, _routingToken));
	}
}