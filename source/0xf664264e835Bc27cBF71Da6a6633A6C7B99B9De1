// SPDX-License-Identifier: MIT
//ASCENSION PROTOCOL V2
//ascensionprotocol.io

pragma solidity =0.6.8;

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
abstract contract Context {
  function _msgSender() internal view virtual returns (address payable) {
    return msg.sender;
  }

  function _msgData() internal view virtual returns (bytes memory) {
    this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
    return msg.data;
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
    return _functionCallWithValue(target, data, value, errorMessage);
  }

  function _functionCallWithValue(
    address target,
    bytes memory data,
    uint256 weiValue,
    string memory errorMessage
  ) private returns (bytes memory) {
    require(isContract(target), "Address: call to non-contract");

    // solhint-disable-next-line avoid-low-level-calls
    (bool success, bytes memory returndata) = target.call{ value: weiValue }(data);
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
   * @dev Returns the addition of two unsigned integers, with an overflow flag.
   *
   * _Available since v3.4._
   */
  function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    uint256 c = a + b;
    if (c < a) return (false, 0);
    return (true, c);
  }

  /**
   * @dev Returns the substraction of two unsigned integers, with an overflow flag.
   *
   * _Available since v3.4._
   */
  function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    if (b > a) return (false, 0);
    return (true, a - b);
  }

  /**
   * @dev Returns the multiplication of two unsigned integers, with an overflow flag.
   *
   * _Available since v3.4._
   */
  function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
    // benefit is lost if 'b' is also tested.
    // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
    if (a == 0) return (true, 0);
    uint256 c = a * b;
    if (c / a != b) return (false, 0);
    return (true, c);
  }

  /**
   * @dev Returns the division of two unsigned integers, with a division by zero flag.
   *
   * _Available since v3.4._
   */
  function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    if (b == 0) return (false, 0);
    return (true, a / b);
  }

  /**
   * @dev Returns the remainder of dividing two unsigned integers, with a division by zero flag.
   *
   * _Available since v3.4._
   */
  function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    if (b == 0) return (false, 0);
    return (true, a % b);
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
    require(b <= a, "SafeMath: subtraction overflow");
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
    if (a == 0) return 0;
    uint256 c = a * b;
    require(c / a == b, "SafeMath: multiplication overflow");
    return c;
  }

  /**
   * @dev Returns the integer division of two unsigned integers, reverting on
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
    require(b > 0, "SafeMath: division by zero");
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
    require(b > 0, "SafeMath: modulo by zero");
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
    require(b <= a, errorMessage);
    return a - b;
  }

  /**
   * @dev Returns the integer division of two unsigned integers, reverting with custom message on
   * division by zero. The result is rounded towards zero.
   *
   * CAUTION: This function is deprecated because it requires allocating memory for the error
   * message unnecessarily. For custom revert reasons use {tryDiv}.
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
    return a / b;
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
    require(b > 0, errorMessage);
    return a % b;
  }
}

interface IUniswapV2Router01 {
  function factory() external pure returns (address);

  function WETH() external pure returns (address);

  function addLiquidity(
    address tokenA,
    address tokenB,
    uint256 amountADesired,
    uint256 amountBDesired,
    uint256 amountAMin,
    uint256 amountBMin,
    address to,
    uint256 deadline
  )
    external
    returns (
      uint256 amountA,
      uint256 amountB,
      uint256 liquidity
    );

  function addLiquidityETH(
    address token,
    uint256 amountTokenDesired,
    uint256 amountTokenMin,
    uint256 amountETHMin,
    address to,
    uint256 deadline
  )
    external
    payable
    returns (
      uint256 amountToken,
      uint256 amountETH,
      uint256 liquidity
    );

  function removeLiquidity(
    address tokenA,
    address tokenB,
    uint256 liquidity,
    uint256 amountAMin,
    uint256 amountBMin,
    address to,
    uint256 deadline
  ) external returns (uint256 amountA, uint256 amountB);

  function removeLiquidityETH(
    address token,
    uint256 liquidity,
    uint256 amountTokenMin,
    uint256 amountETHMin,
    address to,
    uint256 deadline
  ) external returns (uint256 amountToken, uint256 amountETH);

  function removeLiquidityWithPermit(
    address tokenA,
    address tokenB,
    uint256 liquidity,
    uint256 amountAMin,
    uint256 amountBMin,
    address to,
    uint256 deadline,
    bool approveMax,
    uint8 v,
    bytes32 r,
    bytes32 s
  ) external returns (uint256 amountA, uint256 amountB);

  function removeLiquidityETHWithPermit(
    address token,
    uint256 liquidity,
    uint256 amountTokenMin,
    uint256 amountETHMin,
    address to,
    uint256 deadline,
    bool approveMax,
    uint8 v,
    bytes32 r,
    bytes32 s
  ) external returns (uint256 amountToken, uint256 amountETH);

  function swapExactTokensForTokens(
    uint256 amountIn,
    uint256 amountOutMin,
    address[] calldata path,
    address to,
    uint256 deadline
  ) external returns (uint256[] memory amounts);

  function swapTokensForExactTokens(
    uint256 amountOut,
    uint256 amountInMax,
    address[] calldata path,
    address to,
    uint256 deadline
  ) external returns (uint256[] memory amounts);

  function swapExactETHForTokens(
    uint256 amountOutMin,
    address[] calldata path,
    address to,
    uint256 deadline
  ) external payable returns (uint256[] memory amounts);

  function swapTokensForExactETH(
    uint256 amountOut,
    uint256 amountInMax,
    address[] calldata path,
    address to,
    uint256 deadline
  ) external returns (uint256[] memory amounts);

  function swapExactTokensForETH(
    uint256 amountIn,
    uint256 amountOutMin,
    address[] calldata path,
    address to,
    uint256 deadline
  ) external returns (uint256[] memory amounts);

  function swapETHForExactTokens(
    uint256 amountOut,
    address[] calldata path,
    address to,
    uint256 deadline
  ) external payable returns (uint256[] memory amounts);

  function quote(
    uint256 amountA,
    uint256 reserveA,
    uint256 reserveB
  ) external pure returns (uint256 amountB);

  function getAmountOut(
    uint256 amountIn,
    uint256 reserveIn,
    uint256 reserveOut
  ) external pure returns (uint256 amountOut);

  function getAmountIn(
    uint256 amountOut,
    uint256 reserveIn,
    uint256 reserveOut
  ) external pure returns (uint256 amountIn);

  function getAmountsOut(uint256 amountIn, address[] calldata path)
    external
    view
    returns (uint256[] memory amounts);

  function getAmountsIn(uint256 amountOut, address[] calldata path)
    external
    view
    returns (uint256[] memory amounts);
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
  function removeLiquidityETHSupportingFeeOnTransferTokens(
    address token,
    uint256 liquidity,
    uint256 amountTokenMin,
    uint256 amountETHMin,
    address to,
    uint256 deadline
  ) external returns (uint256 amountETH);

  function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
    address token,
    uint256 liquidity,
    uint256 amountTokenMin,
    uint256 amountETHMin,
    address to,
    uint256 deadline,
    bool approveMax,
    uint8 v,
    bytes32 r,
    bytes32 s
  ) external returns (uint256 amountETH);

  function swapExactTokensForTokensSupportingFeeOnTransferTokens(
    uint256 amountIn,
    uint256 amountOutMin,
    address[] calldata path,
    address to,
    uint256 deadline
  ) external;

  function swapExactETHForTokensSupportingFeeOnTransferTokens(
    uint256 amountOutMin,
    address[] calldata path,
    address to,
    uint256 deadline
  ) external payable;

  function swapExactTokensForETHSupportingFeeOnTransferTokens(
    uint256 amountIn,
    uint256 amountOutMin,
    address[] calldata path,
    address to,
    uint256 deadline
  ) external;
}

interface IUniswapV2Factory {
  event PairCreated(address indexed token0, address indexed token1, address pair, uint256);

  function feeTo() external view returns (address);

  function feeToSetter() external view returns (address);

  function getPair(address tokenA, address tokenB) external view returns (address pair);

  function allPairs(uint256) external view returns (address pair);

  function allPairsLength() external view returns (uint256);

  function createPair(address tokenA, address tokenB) external returns (address pair);

  function setFeeTo(address) external;

  function setFeeToSetter(address) external;
}

interface IAscensionStrategy {
  function validateProposal(address[] calldata targets, uint256[] calldata values)
    external
    returns (bool);

  function executeStrategy(address[] calldata targets, uint256[] calldata values)
    external
    returns (bool);
}

interface IAscension {
  function checkExcluded(address account) external view returns (bool);

  function totalFees() external view returns (uint256);

  function getNumCheckpoints(address account) external view returns (uint256);

  function getPriorValue(address account, uint256 blockNumber) external view returns (uint256);

  function getLevel(address account) external view returns (uint256);

  function distributeShares(uint256 tAmount) external;

  function bulkTransfer(address[] calldata _to, uint256[] calldata _values) external returns (bool);

  function ascensionEvent(
    address payable strategy,
    address[] calldata targets,
    uint256[] calldata values
  ) external;

  function lockLiquidity() external;
}

//------------------------------------------------------------------------------------------------------------------------------

contract ASCENSIONV2 is Context, IERC20, IAscension {
  using SafeMath for uint256;
  using Address for address;

  address public owner; //contract admin, will be transferred to GOVERNANCE

  struct Checkpoint {
    uint256 fromBlock;
    uint256 value;
  }
  //checkpoints tracks previous balances updated on every transaction
  mapping(address => mapping(uint256 => Checkpoint)) private checkpoints;
  //numCheckpoints tracks the number of checkpoints for each account
  mapping(address => uint256) private numCheckpoints;
  //_shares tracks the total shares of each user, multiplied by rate to calculate token balance
  mapping(address => uint256) private _shares;
  //_tokens tracks balance directly for excluded accounts
  mapping(address => uint256) private _tokens;
  mapping(address => mapping(address => uint256)) private _allowances;

  //excluded addresses dont accumulate or pay fees
  mapping(address => bool) private isExcluded;
  address[] private _excluded;

  //token state variables
  uint256 private constant MAX = ~uint256(0);
  uint256 private constant TOTAL_TOKENS = 14400000e9;
  uint256 private totalShares;
  uint256 private tFeeTotal;
  address public burn;

  //treasury state variables
  address public uniswapV2Router;
  address public pairAddress;
  address public daoAddress;
  uint256 public totalAscensions; //total number of ascension events
  uint256 public previousAscensionEth; //value of previous ascension event liquidity pull
  uint256 public previousAscensionTimestamp; //timestamp of previous ascension event liquidity pull
  uint256 public ascensionInterval; //the amount of time that must pass before another ascension event can be called
  uint256 public ascensionDivisor; //the divisor used to calculate the price increase required for ascension
  uint256 public pullDivisor; //the divisor used to calculate the amount of liquidity called for ascension
  uint256 public lockDivisor; //the divisor used to calculate the amount of transfer fees sent to contract for lock
  uint256 public rewardFee; //The amount of tokens a user gets from a rewardable function rewardFee/100;
  uint256 public minLockAmount; //The minimum token balance required in this contract to call lock liquidity function
  bool public ascensionActive; //bool controls calling ascensionEvents

  //ERC-20 standard state variables
  string private _name;
  string private _symbol;
  uint8 private _decimals;

  //MODIFIERS----------------------------------------------------------------------------------------
  modifier onlyOwner() {
    require(
      _msgSender() == owner,
      "ASCENSION:onlyOwner:This function can only be called by the contract owner"
    );
    _;
  }

  //EVENTS-------------------------------------------------------------------------------------------
  event AccountCheckpointUpdated(address account, uint256 newValue);
  event AscensionEvent(address strategy, uint256 amountETH, address[] targets, uint256[] values);
  event AscensionIntervalSet(uint256 interval, uint256 timestamp);
  event AscensionActive(bool active);
  event DaoAddressSet(address dao, uint256 timestamp);
  event RouterSet(address router, uint256 timestamp);
  event LiquidityLocked(
    address caller,
    uint256 ethAmount,
    uint256 tokenAmount,
    uint256 rewardAmount,
    uint256 liquidity
  );

  //CONSTRUCTOR--------------------------------------------------------------------------------------
  constructor(address _uniswapV2Router, address _owner) public {
    require(_uniswapV2Router != address(0) && _owner != address(0));

    //set router
    uniswapV2Router = _uniswapV2Router;

    //set owner
    owner = _owner;

    //set pair token
    pairAddress = IUniswapV2Factory(IUniswapV2Router02(uniswapV2Router).factory()).createPair(
      address(this),
      IUniswapV2Router02(uniswapV2Router).WETH()
    );

    //default values
    _name = "ASCENSION PROTOCOL V2";
    _symbol = "ASCEND";
    _decimals = 9;
    totalShares = (MAX - (MAX % TOTAL_TOKENS));
    burn = 0x3333333333333333333333333333333333333333;
    ascensionDivisor = 10;
    ascensionInterval = 3600; //1 hour
    pullDivisor = 50;
    lockDivisor = 2;
    rewardFee = 5;
    minLockAmount = 1000e9;

    //mint to owner
    _shares[_owner] = totalShares;
    _writeCheckpoint(_owner, TOTAL_TOKENS);
    emit Transfer(address(0), _owner, TOTAL_TOKENS);
  }

  //RECEIVE ETH FOR LIQUIDITY-------------------------------------------------------------------------
  receive() external payable {}

  //OWNABLE FUNCTIONS-----------------------------------------------------------------------------------
  function transferOwnership(address _newOwner) external onlyOwner() {
    require(_newOwner != address(0));
    owner = _newOwner;
  }

  //ERC20 BASIC FUNCTIONS-----------------------------------------------------------------------------
  function name() external view returns (string memory) {
    return _name;
  }

  function symbol() external view returns (string memory) {
    return _symbol;
  }

  function decimals() external view returns (uint8) {
    return _decimals;
  }

  function totalSupply() external view override returns (uint256) {
    return TOTAL_TOKENS;
  }

  function balanceOf(address account) public view override returns (uint256) {
    if (isExcluded[account]) return _tokens[account];
    return tokenFromShares(_shares[account]);
  }

  function transfer(address recipient, uint256 amount) external override returns (bool) {
    _transfer(_msgSender(), recipient, amount);
    return true;
  }

  function allowance(address account, address spender) external view override returns (uint256) {
    return _allowances[account][spender];
  }

  function approve(address spender, uint256 amount) external override returns (bool) {
    _approve(_msgSender(), spender, amount);
    return true;
  }

  function transferFrom(
    address sender,
    address recipient,
    uint256 amount
  ) external override returns (bool) {
    _transfer(sender, recipient, amount);
    _approve(
      sender,
      _msgSender(),
      _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance")
    );
    return true;
  }

  function increaseAllowance(address spender, uint256 addedValue) external virtual returns (bool) {
    _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
    return true;
  }

  function decreaseAllowance(address spender, uint256 subtractedValue)
    external
    virtual
    returns (bool)
  {
    _approve(
      _msgSender(),
      spender,
      _allowances[_msgSender()][spender].sub(
        subtractedValue,
        "ERC20: decreased allowance below zero"
      )
    );
    return true;
  }

  //ASCENSION EXTERNAL FUNCTIONS---------------------------------------------------
  function checkExcluded(address account) external view override returns (bool) {
    return isExcluded[account];
  }

  function totalFees() external view override returns (uint256) {
    return tFeeTotal;
  }

  function getNumCheckpoints(address account) external view override returns (uint256) {
    return numCheckpoints[account];
  }

  function getPriorValue(address account, uint256 blockNumber)
    external
    view
    override
    returns (uint256)
  {
    require(blockNumber < block.number, "ASCENSION:getPriorValue: not yet determined");

    uint256 nCheckpoints = numCheckpoints[account];
    if (nCheckpoints == 0) {
      return 0;
    }

    // First check most recent balance
    if (checkpoints[account][nCheckpoints - 1].fromBlock <= blockNumber) {
      return checkpoints[account][nCheckpoints - 1].value;
    }

    // Next check implicit zero balance
    if (checkpoints[account][0].fromBlock > blockNumber) {
      return 0;
    }

    uint256 lower = 0;
    uint256 upper = nCheckpoints - 1;
    while (upper > lower) {
      uint256 center = upper - (upper - lower) / 2; // ceil, avoiding overflow
      Checkpoint memory cp = checkpoints[account][center];
      if (cp.fromBlock == blockNumber) {
        return cp.value;
      } else if (cp.fromBlock < blockNumber) {
        lower = center;
      } else {
        upper = center - 1;
      }
    }
    return checkpoints[account][lower].value;
  }

  function getLevel(address account) external view override returns (uint256) {
    if (balanceOf(account) >= 1000000e9) {
      return 9;
    } else if (balanceOf(account) >= 500000e9) {
      return 8;
    } else if (balanceOf(account) >= 320000e9) {
      return 7;
    } else if (balanceOf(account) >= 160000e9) {
      return 6;
    } else if (balanceOf(account) >= 80000e9) {
      return 5;
    } else if (balanceOf(account) >= 40000e9) {
      return 4;
    } else if (balanceOf(account) >= 20000e9) {
      return 3;
    } else if (balanceOf(account) >= 10000e9) {
      return 2;
    } else if (balanceOf(account) >= 1000e9) {
      return 1;
    } else {
      return 0;
    }
  }

  function distributeShares(uint256 tAmount) external override {
    address sender = _msgSender();
    require(
      !isExcluded[sender],
      "ASCENSION:distributeShares: Excluded addresses cannot call this function"
    );
    (uint256 sAmount, , , , ) = _getValues(sender, tAmount);
    //subtract shares from sender
    _shares[sender] = _shares[sender].sub(sAmount);
    //update checkpoint
    _writeCheckpoint(sender, tokenFromShares(_shares[sender]));
    //subtract shares from total to distribute
    totalShares = totalShares.sub(sAmount);
    //add tAmount to total fees tFeeTotal
    tFeeTotal = tFeeTotal.add(tAmount);
  }

  function includeAccount(address account) external onlyOwner() {
    require(isExcluded[account], "ASCENSION:includeAccount: Account is already included");
    for (uint256 i = 0; i < _excluded.length; i++) {
      if (_excluded[i] == account) {
        _excluded[i] = _excluded[_excluded.length - 1];
        _tokens[account] = 0;
        isExcluded[account] = false;
        _excluded.pop();
        break;
      }
    }
  }

  function excludeAccount(address account) external onlyOwner() {
    require(!isExcluded[account], "ASCENSION:excludeAccount: Account is already excluded");
    if (_shares[account] > 0) {
      _tokens[account] = tokenFromShares(_shares[account]);
    }
    isExcluded[account] = true;
    _excluded.push(account);
  }

  function bulkTransfer(address[] calldata _to, uint256[] calldata _values)
    external
    override
    returns (bool)
  {
    require(_to.length == _values.length);
    for (uint256 i = 0; i < _to.length; i++) {
      _transfer(msg.sender, _to[i], _values[i]);
    }
    return true;
  }

  //ASCENSION INTERNAL FUNCTIONS---------------------------------------------------
  function sharesFromToken(uint256 tAmount, bool deductTransferFee)
    internal
    view
    returns (uint256)
  {
    require(tAmount <= TOTAL_TOKENS, "ASCENSION:sharesFromToken: Amount must be less than supply");
    address sender = _msgSender();
    if (!deductTransferFee) {
      (uint256 sAmount, , , , ) = _getValues(sender, tAmount);
      return sAmount;
    } else {
      (, uint256 sTransferAmount, , , ) = _getValues(sender, tAmount);
      return sTransferAmount;
    }
  }

  function tokenFromShares(uint256 sAmount) internal view returns (uint256) {
    require(
      sAmount <= totalShares,
      "ASCENSION:tokenFromShares: Amount must be less than total shares"
    );
    uint256 currentRate = _getRate();
    return sAmount.div(currentRate);
  }

  function _approve(
    address account,
    address spender,
    uint256 amount
  ) private {
    require(account != address(0), "ERC20: approve from the zero address");
    require(spender != address(0), "ERC20: approve to the zero address");

    _allowances[account][spender] = amount;
    emit Approval(account, spender, amount);
  }

  function _transfer(
    address sender,
    address recipient,
    uint256 amount
  ) private {
    require(sender != address(0), "ERC20: transfer from the zero address");
    require(recipient != address(0), "ERC20: transfer to the zero address");
    require(amount > 0, "Transfer amount must be greater than zero");

    if (isExcluded[sender] && !isExcluded[recipient]) {
      _transferFromExcluded(sender, recipient, amount);
    } else if (!isExcluded[sender] && isExcluded[recipient]) {
      _transferToExcluded(sender, recipient, amount);
    } else if (!isExcluded[sender] && !isExcluded[recipient]) {
      _transferStandard(sender, recipient, amount);
    } else if (isExcluded[sender] && isExcluded[recipient]) {
      _transferBothExcluded(sender, recipient, amount);
    } else {
      _transferStandard(sender, recipient, amount);
    }
  }

  function _transferStandard(
    address sender,
    address recipient,
    uint256 tAmount
  ) private {
    (
      uint256 sAmount,
      uint256 sTransferAmount,
      uint256 sFee,
      uint256 tTransferAmount,
      uint256 tFee
    ) = _getValues(sender, tAmount);
    //update shares balances
    _shares[sender] = _shares[sender].sub(sAmount);
    _shares[recipient] = _shares[recipient].add(sTransferAmount);
    //update checkpoints
    _writeCheckpoint(sender, tokenFromShares(_shares[sender]));
    _writeCheckpoint(recipient, tokenFromShares(_shares[recipient]));
    //distribute fee to all users
    _distributeFee(sFee, tFee);
    emit Transfer(sender, recipient, tTransferAmount);
  }

  function _transferToExcluded(
    address sender,
    address recipient,
    uint256 tAmount
  ) private {
    (
      uint256 sAmount,
      uint256 sTransferAmount,
      uint256 sFee,
      uint256 tTransferAmount,
      uint256 tFee
    ) = _getValues(sender, tAmount);
    //update token balance for excluded account
    _tokens[recipient] = _tokens[recipient].add(tTransferAmount);
    //update shares balance
    _shares[sender] = _shares[sender].sub(sAmount);
    _shares[recipient] = _shares[recipient].add(sTransferAmount);
    //update checkpoints
    _writeCheckpoint(sender, tokenFromShares(_shares[sender]));
    _writeCheckpoint(recipient, tokenFromShares(_shares[recipient]));
    //distribute fee to all users
    _distributeFee(sFee, tFee);
    emit Transfer(sender, recipient, tTransferAmount);
  }

  function _transferFromExcluded(
    address sender,
    address recipient,
    uint256 tAmount
  ) private {
    (uint256 sAmount, , , , ) = _getValues(sender, tAmount);
    //update token balance for excluded account
    _tokens[sender] = _tokens[sender].sub(tAmount);
    //update shares balance
    _shares[sender] = _shares[sender].sub(sAmount);
    _shares[recipient] = _shares[recipient].add(sAmount);
    //update checkpoints
    _writeCheckpoint(sender, tokenFromShares(_shares[sender]));
    _writeCheckpoint(recipient, tokenFromShares(_shares[recipient]));

    emit Transfer(sender, recipient, tAmount);
  }

  function _transferBothExcluded(
    address sender,
    address recipient,
    uint256 tAmount
  ) private {
    (uint256 sAmount, , , , ) = _getValues(sender, tAmount);
    //update token balances for excluded accounts
    _tokens[sender] = _tokens[sender].sub(tAmount);
    _tokens[recipient] = _tokens[recipient].add(tAmount);
    //update shares balances
    _shares[sender] = _shares[sender].sub(sAmount);
    _shares[recipient] = _shares[recipient].add(sAmount);
    //update checkpoints
    _writeCheckpoint(sender, tokenFromShares(_shares[sender]));
    _writeCheckpoint(recipient, tokenFromShares(_shares[recipient]));

    emit Transfer(sender, recipient, tAmount);
  }

  function _writeCheckpoint(address account, uint256 newValue) internal {
    uint256 nCheckpoints = numCheckpoints[account];
    if (nCheckpoints > 0 && checkpoints[account][nCheckpoints - 1].fromBlock == block.number) {
      checkpoints[account][nCheckpoints - 1].value = newValue;
    } else {
      checkpoints[account][nCheckpoints] = Checkpoint(block.number, newValue);
      numCheckpoints[account] = nCheckpoints + 1;
    }

    emit AccountCheckpointUpdated(account, tokenFromShares(newValue));
  }

  function _distributeFee(uint256 sFee, uint256 tFee) private {
    //portion of fee goes to contract for liquidity locks, amount controlled by lockDivisor
    uint256 lockFee = sFee.div(lockDivisor);
    _shares[address(this)] = _shares[address(this)].add(lockFee);

    //distribute remaining fee to holders
    totalShares = totalShares.sub(sFee.sub(lockFee));

    //update total fee variable
    tFeeTotal = tFeeTotal.add(tFee);
  }

  function _getValues(address sender, uint256 tAmount)
    private
    view
    returns (
      uint256,
      uint256,
      uint256,
      uint256,
      uint256
    )
  {
    (uint256 tTransferAmount, uint256 tFee) = _getTValues(sender, tAmount);
    uint256 currentRate = _getRate();
    (uint256 sAmount, uint256 sTransferAmount, uint256 sFee) =
      _getSValues(tAmount, tFee, currentRate);
    return (sAmount, sTransferAmount, sFee, tTransferAmount, tFee);
  }

  function _getTValues(address sender, uint256 tAmount) private view returns (uint256, uint256) {
    uint256 tFee = _getFee(sender, tAmount);
    uint256 tTransferAmount = tAmount.sub(tFee);
    return (tTransferAmount, tFee);
  }

  function _getSValues(
    uint256 tAmount,
    uint256 tFee,
    uint256 currentRate
  )
    private
    pure
    returns (
      uint256,
      uint256,
      uint256
    )
  {
    uint256 sAmount = tAmount.mul(currentRate);
    uint256 sFee = tFee.mul(currentRate);
    uint256 sTransferAmount = sAmount.sub(sFee);
    return (sAmount, sTransferAmount, sFee);
  }

  function _getFee(address account, uint256 tAmount) private view returns (uint256) {
    if (balanceOf(account) >= 1000000e9) {
      return 0;
    } else if (balanceOf(account) >= 500000e9) {
      return tAmount.div(100);
    } else if (balanceOf(account) >= 320000e9) {
      return tAmount.div(95);
    } else if (balanceOf(account) >= 160000e9) {
      return tAmount.div(90);
    } else if (balanceOf(account) >= 80000e9) {
      return tAmount.div(85);
    } else if (balanceOf(account) >= 40000e9) {
      return tAmount.div(80);
    } else if (balanceOf(account) >= 20000e9) {
      return tAmount.div(75);
    } else if (balanceOf(account) >= 10000e9) {
      return tAmount.div(70);
    } else if (balanceOf(account) >= 1000e9) {
      return tAmount.div(65);
    } else {
      return tAmount.div(60);
    }
  }

  function _getRate() private view returns (uint256) {
    (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
    return rSupply.div(tSupply);
  }

  function _getCurrentSupply() private view returns (uint256, uint256) {
    uint256 rSupply = totalShares;
    uint256 tSupply = TOTAL_TOKENS;
    for (uint256 i = 0; i < _excluded.length; i++) {
      if (_shares[_excluded[i]] > rSupply || _tokens[_excluded[i]] > tSupply)
        return (totalShares, TOTAL_TOKENS);
      rSupply = rSupply.sub(_shares[_excluded[i]]);
      tSupply = tSupply.sub(_tokens[_excluded[i]]);
    }
    if (rSupply < totalShares.div(TOTAL_TOKENS)) return (totalShares, TOTAL_TOKENS);
    return (rSupply, tSupply);
  }

  //TREASURY EXTERNAL FUNCTIONS--------------------------------------------------------------------------------------
  function setUniswapRouter(address _newAddr) external onlyOwner() {
    require(_newAddr != address(0));
    uniswapV2Router = _newAddr;

    emit RouterSet(_newAddr, block.timestamp);
  }

  function setDao(address _newAddr) external onlyOwner() {
    require(_newAddr != address(0));
    daoAddress = _newAddr;

    emit DaoAddressSet(_newAddr, block.timestamp);
  }

  function setAscensionInterval(uint256 _interval) external onlyOwner() {
    ascensionInterval = _interval;

    emit AscensionIntervalSet(_interval, block.timestamp);
  }

  function setAscensionActive(bool _active) external onlyOwner() {
    ascensionActive = _active;

    emit AscensionActive(_active);
  }

  function setAscensionDivisor(uint256 _divisor) external onlyOwner() {
    require(_divisor > 0 && _divisor <= 25);
    ascensionDivisor = _divisor;
  }

  function setPullDivisor(uint256 _divisor) external onlyOwner() {
    require(_divisor > 0 && _divisor <= 100);
    pullDivisor = _divisor;
  }

  function setLockDivisor(uint256 _divisor) external onlyOwner() {
    require(_divisor > 0 && _divisor <= 100);
    lockDivisor = _divisor;
  }

  function setMinLockAmount(uint256 _amount) external onlyOwner() {
    require(_amount >= 1e9, "ASCENSION:setMinLockAmount:amount must but greater than 1 token!");
    minLockAmount = _amount;
  }

  function setRewardFee(uint256 _fee) external onlyOwner() {
    require(_fee > 0 && _fee <= 5, "ASCENSION:setRewardFee: Reward fee can only be in 1-5");
    rewardFee = _fee;
  }

  function ascensionEvent(
    address payable _strategy,
    address[] calldata _targets,
    uint256[] calldata _values
  ) external override {
    require(
      ascensionActive,
      "ASCENSION:ascensionEvent: Ascension functions are currently inactive!"
    );
    require(
      _msgSender() == daoAddress || _msgSender() == owner,
      "ASCENSION:ascensionEvent: Sender must be DAO address!"
    );
    require(
      block.timestamp > previousAscensionTimestamp + ascensionInterval,
      "ASCENSION:ascensionEvent: Too soon to call Ascension Event!"
    );
    require(
      pairAddress != address(0),
      "ASCENSION:ascensionEvent: Liquidity pair token not yet set!"
    );
    require(uniswapV2Router != address(0), "ASCENSION:ascensionEvent: Router address not yet set!");
    require(
      IERC20(pairAddress).balanceOf(address(this)) > 0,
      "ASCENSION:ascensionEvent: No liquidity to remove!"
    );

    //+1 total Ascension events
    totalAscensions++;

    //set ascension timestamp
    previousAscensionTimestamp = block.timestamp;

    //set amount liquidity to be pulled
    uint256 pullAmount = IERC20(pairAddress).balanceOf(address(this)).div(pullDivisor);

    //aprove router contract
    require(IERC20(pairAddress).approve(uniswapV2Router, pullAmount));

    //pull liquidity
    //both ASCEND and BNB from pull are sent to strategy address
    uint256 amountEth =
      IUniswapV2Router02(uniswapV2Router).removeLiquidityETHSupportingFeeOnTransferTokens(
        address(this),
        pullAmount,
        0,
        0,
        _strategy,
        block.timestamp
      );
    //require amountEth is greater than previous ascension event by at least x
    require(
      amountEth >= previousAscensionEth.add(previousAscensionEth.div(ascensionDivisor)),
      "ASCENSION:ascensionEvent: price has not ascended!"
    );

    //set Ascension ETH amount
    previousAscensionEth = amountEth;

    //execute strategy
    require(IAscensionStrategy(_strategy).executeStrategy(_targets, _values));

    emit AscensionEvent(_strategy, amountEth, _targets, _values);
  }

  function lockLiquidity() external override {
    address caller = _msgSender();
    require(
      ascensionActive,
      "ASCENSION:ascensionEvent: Ascension functions are currently inactive!"
    );
    require(
      balanceOf(address(this)) >= minLockAmount,
      "ASCENSION:lockLiquidity: Balance of contract is too low to lock"
    );
    require(balanceOf(caller) >= 1e9, "ASCENSION:lockLiquidity: Caller has no ASCEND!");

    //get the amount of tokens
    uint256 callerReward = _shares[address(this)].mul(rewardFee).div(100);
    uint256 lockAmount = _shares[address(this)].sub(callerReward);

    uint256 half = lockAmount.div(2);
    uint256 otherHalf = lockAmount.sub(half);

    //add reward shares to caller _shares
    _shares[caller] = _shares[caller].add(callerReward);

    // capture the contract's current ETH balance.
    // this is so that we can capture exactly the amount of ETH that the
    // swap creates, and not make the liquidity event include any ETH that
    // has been manually sent to the contract
    uint256 initialBalance = address(this).balance;

    // swap tokens for ETH
    // generate the uniswap pair path of token -> weth
    address[] memory path = new address[](2);
    path[0] = address(this);
    path[1] = IUniswapV2Router02(uniswapV2Router).WETH();

    _approve(address(this), address(uniswapV2Router), half);

    // make the swap
    IUniswapV2Router02(uniswapV2Router).swapExactTokensForETHSupportingFeeOnTransferTokens(
      tokenFromShares(half),
      0, // accept any amount of ETH
      path,
      address(this),
      block.timestamp
    );

    // how much ETH did we just swap into?
    uint256 ethAmount = address(this).balance.sub(initialBalance);

    // add liquidity to uniswap
    // approve token transfer to cover all possible scenarios
    _approve(address(this), address(uniswapV2Router), tokenFromShares(otherHalf));

    // add the liquidity
    (, , uint256 liquidity) =
      IUniswapV2Router02(uniswapV2Router).addLiquidityETH{ value: ethAmount }(
        address(this),
        tokenFromShares(otherHalf),
        0, // slippage is unavoidable
        0, // slippage is unavoidable
        address(this),
        block.timestamp
      );

    emit LiquidityLocked(
      _msgSender(),
      ethAmount,
      tokenFromShares(otherHalf),
      tokenFromShares(callerReward),
      liquidity
    );
  }

  //--------------------------------------------------------------------------------------------------------------------
}