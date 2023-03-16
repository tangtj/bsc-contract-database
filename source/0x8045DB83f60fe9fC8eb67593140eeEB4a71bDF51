// File: openzeppelin-contracts-2.5.1/contracts/token/ERC20/IERC20.sol

pragma solidity ^0.5.0;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP. Does not include
 * the optional functions; to access them see {ERC20Detailed}.
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

// File: openzeppelin-contracts-2.5.1/contracts/math/SafeMath.sol

pragma solidity ^0.5.0;

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
     * - Subtraction cannot overflow.
     *
     * _Available since v2.4.0._
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
     * - The divisor cannot be zero.
     *
     * _Available since v2.4.0._
     */
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
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
     * - The divisor cannot be zero.
     *
     * _Available since v2.4.0._
     */
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

// File: openzeppelin-contracts-2.5.1/contracts/math/Math.sol

pragma solidity ^0.5.0;

/**
 * @dev Standard math utilities missing in the Solidity language.
 */
library Math {
    /**
     * @dev Returns the largest of two numbers.
     */
    function max(uint256 a, uint256 b) internal pure returns (uint256) {
        return a >= b ? a : b;
    }

    /**
     * @dev Returns the smallest of two numbers.
     */
    function min(uint256 a, uint256 b) internal pure returns (uint256) {
        return a < b ? a : b;
    }

    /**
     * @dev Returns the average of two numbers. The result is rounded towards
     * zero.
     */
    function average(uint256 a, uint256 b) internal pure returns (uint256) {
        // (a + b) / 2 can overflow, so we distribute
        return (a / 2) + (b / 2) + ((a % 2 + b % 2) / 2);
    }
}

// File: openzeppelin-contracts-2.5.1/contracts/utils/Address.sol

pragma solidity ^0.5.5;

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
        assembly { codehash := extcodehash(account) }
        return (codehash != accountHash && codehash != 0x0);
    }

    /**
     * @dev Converts an `address` into `address payable`. Note that this is
     * simply a type cast: the actual underlying value is not changed.
     *
     * _Available since v2.4.0._
     */
    function toPayable(address account) internal pure returns (address payable) {
        return address(uint160(account));
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
     *
     * _Available since v2.4.0._
     */
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-call-value
        (bool success, ) = recipient.call.value(amount)("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
}

// File: openzeppelin-contracts-2.5.1/contracts/token/ERC20/SafeERC20.sol

pragma solidity ^0.5.0;




/**
 * @title SafeERC20
 * @dev Wrappers around ERC20 operations that throw on failure (when the token
 * contract returns false). Tokens that return no value (and instead revert or
 * throw on failure) are also supported, non-reverting calls are assumed to be
 * successful.
 * To use this library you can add a `using SafeERC20 for ERC20;` statement to your contract,
 * which allows you to call the safe operations as `token.safeTransfer(...)`, etc.
 */
library SafeERC20 {
    using SafeMath for uint256;
    using Address for address;

    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    function safeApprove(IERC20 token, address spender, uint256 value) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        // solhint-disable-next-line max-line-length
        require((value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).add(value);
        callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).sub(value, "SafeERC20: decreased allowance below zero");
        callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     */
    function callOptionalReturn(IERC20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves.

        // A Solidity high level call has three parts:
        //  1. The target address is checked to verify it contains contract code
        //  2. The call itself is made, and success asserted
        //  3. The return value is decoded, which in turn checks the size of the returned data.
        // solhint-disable-next-line max-line-length
        require(address(token).isContract(), "SafeERC20: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = address(token).call(data);
        require(success, "SafeERC20: low-level call failed");

        if (returndata.length > 0) { // Return data is optional
            // solhint-disable-next-line max-line-length
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}

// File: interfaces/yearn/IController.sol

// SPDX-License-Identifier: MIT

pragma solidity ^0.5.17;

interface IController {
    function withdraw(address, uint256) external;

    function balanceOf(address) external view returns (uint256);

    function earn(address, uint256) external;

    function want(address) external view returns (address);

    function rewards() external view returns (address);

    function vaults(address) external view returns (address);

    function strategies(address) external view returns (address);
}

// File: contracts/strategies/StrategyACryptoS0V6_ACSI.sol

pragma solidity ^0.5.17;
pragma experimental ABIEncoderV2;







contract StrategyACryptoS0V6_ACSI {
    using SafeERC20 for IERC20;
    using Address for address;
    using SafeMath for uint256;
    using Math for uint256;

    address public constant want = address(0x5b17b4d5e4009B5C43e3e3d63A5229F794cBA389); //ACSI

    struct BlpToLiquidate {
        address pool;
        address tokenOut; //token to get
    }
    struct PairToLiquidate {
        address pair;
        address tokenA;
        address tokenB;
        address router;
    }
    struct SsToLiquidate {
        address pool;
        address lpToken;
        int128 i; //token to get
    }
    struct TokenToSwap {
        address tokenIn;
        address tokenOut;
        address router;
    }
    struct SsTokenToSwap {
        address tokenIn;
        address pool;
        bool underlying;
        int128 i;
        int128 j;
    }
    struct BlpTokenToSwap {
        address pool;
        address tokenIn;
        address tokenOut;
    }
    address public blpFeesCollector;
    address[] public blpFeesTokensToCollect;
    address[] public ssToWithdraw; //StableSwap pools to withdraw admin fees from
    BlpToLiquidate[] public blpsToLiquidate;
    SsToLiquidate[] public ssToLiquidate;
    PairToLiquidate[] public pairsToLiquidate;
    SsTokenToSwap[] public ssTokensToSwap;
    TokenToSwap[] public tokensToSwap0;
    TokenToSwap[] public tokensToSwap1;
    BlpTokenToSwap[] public blpTokensToSwap0;
    BlpTokenToSwap[] public blpTokensToSwap1;

    address public governance;
    address public controller;
    address public strategist;

    uint256 public withdrawalFee = 1000; //10%
    uint256 public harvesterReward = 30;
    uint256 public constant FEE_DENOMINATOR = 10000;

    constructor(address _controller, address _governance) public {
      strategist = msg.sender;
      governance = _governance;
      controller = _controller;

      blpFeesCollector = address(0x86182be6C6233b59ba531022aAfA68929d7CeDBF);

      blpFeesTokensToCollect.push(address(0x2170Ed0880ac9A755fd29B2688956BD959F933F8)); //eth
      blpFeesTokensToCollect.push(address(0x4197C6EF3879a08cD51e5560da5064B773aa1d29)); //acs

      ssToWithdraw.push(address(0xb3F0C9ea1F05e312093Fdb031E789A756659B0AC)); //ACS4 StableSwap
      ssToWithdraw.push(address(0x191409D5A4EfFe25b0f4240557BA2192D18a191e)); //ACS4VAI StableSwap
      ssToWithdraw.push(address(0x99c92765EfC472a9709Ced86310D64C4573c4b77)); //ACS4UST StableSwap
      ssToWithdraw.push(address(0x3919874C7bc0699cF59c981C5eb668823FA4f958)); //ACS4QUSD StableSwap
      ssToWithdraw.push(address(0xc61639E5626EcfB0788b5308c67CBbBD1cAecBF0)); //ACS4IRON StableSwap
      ssToWithdraw.push(address(0xbE7CAa236544d1B9A0E7F91e94B9f5Bfd3B5ca81)); //ACS3BTC StableSwap

      ssToLiquidate.push(SsToLiquidate({
          pool: address(0xb3F0C9ea1F05e312093Fdb031E789A756659B0AC), //ACS4 StableSwap
          lpToken: address(0x83D69Ef5c9837E21E2389D47d791714F5771F29b), //ACS4
          i: 0
      }));

      ssTokensToSwap.push(SsTokenToSwap({
          tokenIn: address(0xfCe146bF3146100cfe5dB4129cf6C82b0eF4Ad8c), //renBTC
          pool: address(0xbE7CAa236544d1B9A0E7F91e94B9f5Bfd3B5ca81), //ACS3BTC StableSwap
          underlying: false,
          i: 1,
          j: 0
      }));
      ssTokensToSwap.push(SsTokenToSwap({
          tokenIn: address(0xeD28A457A5A76596ac48d87C0f577020F6Ea1c4C), //pBTC
          pool: address(0xbE7CAa236544d1B9A0E7F91e94B9f5Bfd3B5ca81), //ACS3BTC StableSwap
          underlying: false,
          i: 2,
          j: 0
      }));

      ssTokensToSwap.push(SsTokenToSwap({
          tokenIn: address(0x55d398326f99059fF775485246999027B3197955), //usdt
          pool: address(0xb3F0C9ea1F05e312093Fdb031E789A756659B0AC), //ACS4 StableSwap
          underlying: false,
          i: 1,
          j: 0
      }));
      ssTokensToSwap.push(SsTokenToSwap({
          tokenIn: address(0x1AF3F329e8BE154074D8769D1FFa4eE058B1DBc3), //dai
          pool: address(0xb3F0C9ea1F05e312093Fdb031E789A756659B0AC), //ACS4 StableSwap
          underlying: false,
          i: 2,
          j: 0
      }));
      ssTokensToSwap.push(SsTokenToSwap({
          tokenIn: address(0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d), //usdc
          pool: address(0xb3F0C9ea1F05e312093Fdb031E789A756659B0AC), //ACS4 StableSwap
          underlying: false,
          i: 3,
          j: 0
      }));

      ssTokensToSwap.push(SsTokenToSwap({
          tokenIn: address(0x4BD17003473389A42DAF6a0a729f6Fdb328BbBd7), //vai
          pool: address(0x191409D5A4EfFe25b0f4240557BA2192D18a191e), //ACS4VAI StableSwap
          underlying: true,
          i: 0,
          j: 1
      }));
      ssTokensToSwap.push(SsTokenToSwap({
          tokenIn: address(0x23396cF899Ca06c4472205fC903bDB4de249D6fC), //ust
          pool: address(0x99c92765EfC472a9709Ced86310D64C4573c4b77), //ACS4UST StableSwap
          underlying: true,
          i: 0,
          j: 1
      }));
      ssTokensToSwap.push(SsTokenToSwap({
          tokenIn: address(0xb8C540d00dd0Bf76ea12E4B4B95eFC90804f924E), //qusd
          pool: address(0x3919874C7bc0699cF59c981C5eb668823FA4f958), //ACS4QUSD StableSwap
          underlying: true,
          i: 0,
          j: 1
      }));
      ssTokensToSwap.push(SsTokenToSwap({
          tokenIn: address(0x7b65B489fE53fCE1F6548Db886C08aD73111DDd8), //iron
          pool: address(0xc61639E5626EcfB0788b5308c67CBbBD1cAecBF0), //ACS4IRON StableSwap
          underlying: true,
          i: 0,
          j: 1
      }));

      blpTokensToSwap0.push(BlpTokenToSwap({
        pool: address(0x7ea9F435c7CcB2eEF266F5366fe13ea6C9F3e245), //acs3
        tokenIn: address(0x7130d2A12B9BCbFAe4f2634d864A1Ee1Ce3Ead9c), //btcb
        tokenOut: address(0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c) //wbnb
      }));
      blpTokensToSwap0.push(BlpTokenToSwap({
        pool: address(0x7ea9F435c7CcB2eEF266F5366fe13ea6C9F3e245), //acs3
        tokenIn: address(0x2170Ed0880ac9A755fd29B2688956BD959F933F8), //eth
        tokenOut: address(0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c) //wbnb
      }));

      blpTokensToSwap1.push(BlpTokenToSwap({
        pool: address(0x894eD9026De37AfD9CCe1E6C0BE7d6b510e3FfE5), //a2b2
        tokenIn: address(0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c), //wbnb
        tokenOut: address(0x5b17b4d5e4009B5C43e3e3d63A5229F794cBA389) //acsi
      }));
      blpTokensToSwap1.push(BlpTokenToSwap({
        pool: address(0x894eD9026De37AfD9CCe1E6C0BE7d6b510e3FfE5), //a2b2
        tokenIn: address(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56), //busd
        tokenOut: address(0x5b17b4d5e4009B5C43e3e3d63A5229F794cBA389) //acsi
      }));
      blpTokensToSwap1.push(BlpTokenToSwap({
        pool: address(0x894eD9026De37AfD9CCe1E6C0BE7d6b510e3FfE5), //a2b2
        tokenIn: address(0x4197C6EF3879a08cD51e5560da5064B773aa1d29), //acs
        tokenOut: address(0x5b17b4d5e4009B5C43e3e3d63A5229F794cBA389) //acsi
      }));

    }

    function getName() external pure returns (string memory) {
        return "StrategyACryptoS0V6_ACSI";
    }

    function deposit() public {
    }

    // Controller only function for creating additional rewards from dust
    function withdraw(IERC20 _asset) external returns (uint256 balance) {
        require(msg.sender == controller, "!controller");
        require(want != address(_asset), "want");
        balance = _asset.balanceOf(address(this));
        _asset.safeTransfer(controller, balance);
    }

    // Withdraw partial funds, normally used with a vault withdrawal
    function withdraw(uint256 _amount) external {
      require(msg.sender == controller, "!controller");
      uint256 _balance = IERC20(want).balanceOf(address(this));
      if (_balance < _amount) {
          _amount = _balance;
      }

      uint256 _fee = _amount.mul(withdrawalFee).div(FEE_DENOMINATOR);

      address _vault = IController(controller).vaults(address(want));
      require(_vault != address(0), "!vault"); // additional protection so we don't burn the funds
      IERC20(want).safeTransfer(_vault, _amount.sub(_fee));
    }

    // Withdraw all funds, normally used when migrating strategies
    function withdrawAll() external returns (uint256 balance) {
      require(msg.sender == controller || msg.sender == strategist || msg.sender == governance, "!authorized");

      balance = IERC20(want).balanceOf(address(this));

      address _vault = IController(controller).vaults(address(want));
      require(_vault != address(0), "!vault"); // additional protection so we don't burn the funds
      IERC20(want).safeTransfer(_vault, balance);
    }

    function balanceOfWant() public view returns (uint256) {
        return IERC20(want).balanceOf(address(this));
    }

    function harvest() public returns (uint harvesterRewarded) {
      require(msg.sender == tx.origin, "not eoa");

      uint _before = IERC20(want).balanceOf(address(this));
      _convertAllToWant();
      uint _harvested = IERC20(want).balanceOf(address(this)).sub(_before);

      if (_harvested > 0) {
        uint256 _harvesterReward = _harvested.mul(harvesterReward).div(FEE_DENOMINATOR);
        IERC20(want).safeTransfer(msg.sender, _harvesterReward);
        return _harvesterReward;
      }
    }


    function _convertAllToWant() internal {
      if(blpFeesTokensToCollect.length > 0) _collectBlpFees();

      for (uint i=ssToWithdraw.length; i>0; i--) {
        IStableSwap(ssToWithdraw[i-1]).withdraw_admin_fees();
      }

      for (uint i=blpsToLiquidate.length; i>0; i--) {
        _liquidateBlp(blpsToLiquidate[i-1]);
      }

      for (uint i=ssToLiquidate.length; i>0; i--) {
        _liquidateSs(ssToLiquidate[i-1]);
      }

      for (uint i=pairsToLiquidate.length; i>0; i--) {
        _liquidatePair(pairsToLiquidate[i-1].pair, pairsToLiquidate[i-1].tokenA, pairsToLiquidate[i-1].tokenB, pairsToLiquidate[i-1].router);
      }

      for (uint i=ssTokensToSwap.length; i>0; i--) {
        _swapSs(ssTokensToSwap[i-1]);
      }

      for (uint i=tokensToSwap0.length; i>0; i--) {
        _convertToken(tokensToSwap0[i-1].tokenIn, tokensToSwap0[i-1].tokenOut, tokensToSwap0[i-1].router);
      }

      for (uint i=blpTokensToSwap0.length; i>0; i--) {
        _swapBlpToken(blpTokensToSwap0[i-1]);
      }

      for (uint i=tokensToSwap1.length; i>0; i--) {
        _convertToken(tokensToSwap1[i-1].tokenIn, tokensToSwap1[i-1].tokenOut, tokensToSwap1[i-1].router);
      }

      for (uint i=blpTokensToSwap1.length; i>0; i--) {
        _swapBlpToken(blpTokensToSwap1[i-1]);
      }

    }

    function _swapBlpToken(BlpTokenToSwap memory _blpTokenToSwap) internal {
      uint256 _amount = IERC20(_blpTokenToSwap.tokenIn).balanceOf(address(this));
      if(_amount > 0) {
        address _vault = IBlpPool(_blpTokenToSwap.pool).getVault();
        IERC20(_blpTokenToSwap.tokenIn).safeApprove(_vault, 0);
        IERC20(_blpTokenToSwap.tokenIn).safeApprove(_vault, _amount);
        IBlpVault(_vault).swap(
          IBlpVault.SingleSwap({
            poolId: IBlpPool(_blpTokenToSwap.pool).getPoolId(),
            kind: IBlpVault.SwapKind.GIVEN_IN,
            assetIn: _blpTokenToSwap.tokenIn,
            assetOut: _blpTokenToSwap.tokenOut,
            amount: _amount,
            userData: ''
          }),
          IBlpVault.FundManagement({
            sender: address(this),
            fromInternalBalance: false,
            recipient: address(this),
            toInternalBalance: false
          }),
          0,                //limit
          now.add(1800)     // deadline
        );
      }      
    }

    function _liquidateBlp(BlpToLiquidate memory _blpToLiquidate) internal {
      uint256 _amount = IERC20(_blpToLiquidate.pool).balanceOf(address(this));
      if(_amount > 0) {
        address _vault = IBlpPool(_blpToLiquidate.pool).getVault();
        IERC20(_blpToLiquidate.pool).safeApprove(_vault, 0);
        IERC20(_blpToLiquidate.pool).safeApprove(_vault, _amount);

        bytes32 poolId = IBlpPool(_blpToLiquidate.pool).getPoolId();
        (address[] memory assets,,) = IBlpVault(_vault).getPoolTokens(poolId);

        uint256[] memory minAmountsOut = new uint256[](assets.length);

        uint256 tokenIndex;
        for(uint i = 0; i < assets.length; i++) {
          if(assets[i] == _blpToLiquidate.tokenOut) {
            tokenIndex = i;
            break;
          }
        }

        IBlpVault(_vault).exitPool(
          poolId,
          address(this), //sender
          address(this), //recipient
          IBlpVault.ExitPoolRequest({
            assets: assets,  
            minAmountsOut: minAmountsOut,
            userData: abi.encode(IBlpPool.ExitKind.EXACT_BPT_IN_FOR_ONE_TOKEN_OUT, _amount, tokenIndex),
            toInternalBalance: false
          })
        );
      }
    }

    function _collectBlpFees() internal {
      IBlpFeesCollector(blpFeesCollector).withdrawCollectedFees(
        blpFeesTokensToCollect,
        IBlpFeesCollector(blpFeesCollector).getCollectedFeeAmounts(blpFeesTokensToCollect),
        address(this)
      );
    }

    function _liquidateSs(SsToLiquidate memory _ssToLiquidate) internal {
      uint256 _amount = IERC20(_ssToLiquidate.lpToken).balanceOf(address(this));
      if(_amount > 0) {
        IERC20(_ssToLiquidate.lpToken).safeApprove(_ssToLiquidate.pool, 0);
        IERC20(_ssToLiquidate.lpToken).safeApprove(_ssToLiquidate.pool, _amount);
        IStableSwap(_ssToLiquidate.pool).remove_liquidity_one_coin(_amount, _ssToLiquidate.i, 0);
      }
    }

    function _swapSs(SsTokenToSwap memory _ssTokenToSwap) internal {
      uint256 _amount = IERC20(_ssTokenToSwap.tokenIn).balanceOf(address(this));
      if(_amount > 0) {
        IERC20(_ssTokenToSwap.tokenIn).safeApprove(_ssTokenToSwap.pool, 0);
        IERC20(_ssTokenToSwap.tokenIn).safeApprove(_ssTokenToSwap.pool, _amount);
        if(_ssTokenToSwap.underlying) {
          IStableSwap(_ssTokenToSwap.pool).exchange_underlying(_ssTokenToSwap.i, _ssTokenToSwap.j, _amount, 0);            
        } else {
          IStableSwap(_ssTokenToSwap.pool).exchange(_ssTokenToSwap.i, _ssTokenToSwap.j, _amount, 0);            
        }
      }      
    }

    function _liquidatePair(address _pair, address _tokenA, address _tokenB, address _router) internal {
      uint256 _amount = IERC20(_pair).balanceOf(address(this));
      if(_amount > 0 ) {
        IERC20(_pair).safeApprove(_router, 0);
        IERC20(_pair).safeApprove(_router, _amount);

        IUniswapRouter(_router).removeLiquidity(
            _tokenA, // address tokenA,
            _tokenB, // address tokenB,
            _amount, // uint liquidity,
            0, // uint amountAMin,
            0, // uint amountBMin,
            address(this), // address to,
            now.add(1800) // uint deadline
          );
      }
    }

    function _convertToken(address _tokenIn, address _tokenOut, address _router) internal {
      uint256 _amount = IERC20(_tokenIn).balanceOf(address(this));
      if(_amount > 0 ) {
        IERC20(_tokenIn).safeApprove(_router, 0);
        IERC20(_tokenIn).safeApprove(_router, _amount);

        address[] memory path = new address[](2);
        path[0] = _tokenIn;
        path[1] = _tokenOut;

        IUniswapRouter(_router).swapExactTokensForTokens(_amount, uint256(0), path, address(this), now.add(1800));
      }
    }

    function balanceOf() public view returns (uint256) {
      return balanceOfWant();
    }

    function setGovernance(address _governance) external {
        require(msg.sender == governance, "!governance");
        governance = _governance;
    }

    function setController(address _controller) external {
        require(msg.sender == governance, "!governance");
        controller = _controller;
    }

    function setStrategist(address _strategist) external {
        require(msg.sender == governance, "!governance");
        strategist = _strategist;
    }

    function setBlpFeesCollector(address _blpFeesCollector) external {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      blpFeesCollector = _blpFeesCollector;
    }

    function addBlpFeesTokensToCollect(address[] calldata _blpFeesTokensToCollect) external {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      for (uint i=0; i < _blpFeesTokensToCollect.length; i++) {
        blpFeesTokensToCollect.push(_blpFeesTokensToCollect[i]);
      }
    }

    function addSsToWithdraw(address[] calldata _ssToWithdraw) external {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      for (uint i=0; i < _ssToWithdraw.length; i++) {
        ssToWithdraw.push(_ssToWithdraw[i]);
      }
    }

    function addBlpsToLiquidate(BlpToLiquidate[] memory _blpsToLiquidate) public {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      for (uint i=0; i < _blpsToLiquidate.length; i++) {
        blpsToLiquidate.push(_blpsToLiquidate[i]);
      }
    }

    function addSsToLiquidate(SsToLiquidate[] memory _ssToLiquidate) public {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      for (uint i=0; i < _ssToLiquidate.length; i++) {
        ssToLiquidate.push(_ssToLiquidate[i]);
      }
    }

    function addPairsToLiquidate(PairToLiquidate[] memory _pairsToLiquidate) public {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      for (uint i=0; i < _pairsToLiquidate.length; i++) {
        pairsToLiquidate.push(_pairsToLiquidate[i]);
      }
    }

    function addSsTokensToSwap(SsTokenToSwap[] memory _ssTokensToSwap) public {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      for (uint i=0; i < _ssTokensToSwap.length; i++) {
        ssTokensToSwap.push(_ssTokensToSwap[i]);
      }
    }

    function addTokensToSwap0(TokenToSwap[] memory _tokensToSwap) public {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      for (uint i=0; i < _tokensToSwap.length; i++) {
        tokensToSwap0.push(_tokensToSwap[i]);
      }
    }

    function addTokensToSwap1(TokenToSwap[] memory _tokensToSwap) public {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      for (uint i=0; i < _tokensToSwap.length; i++) {
        tokensToSwap1.push(_tokensToSwap[i]);
      }
    }

    function addBlpTokensToSwap0(BlpTokenToSwap[] memory _blpTokensToSwap) public {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      for (uint i=0; i < _blpTokensToSwap.length; i++) {
        blpTokensToSwap0.push(_blpTokensToSwap[i]);
      }
    }

    function addBlpTokensToSwap1(BlpTokenToSwap[] memory _blpTokensToSwap) public {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      for (uint i=0; i < _blpTokensToSwap.length; i++) {
        blpTokensToSwap1.push(_blpTokensToSwap[i]);
      }
    }

    function deleteBlpFeesTokensToCollect() external {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      delete blpFeesTokensToCollect;
    }

    function deleteSsToWithdraw() external {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      delete ssToWithdraw;
    }

    function deleteBlpsToLiquidate() external {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      delete blpsToLiquidate;
    }

    function deleteSsToLiquidate() external {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      delete ssToLiquidate;
    }

    function deletePairsToLiquidate() external {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      delete pairsToLiquidate;
    }

    function deleteSsTokensToSwap() external {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      delete ssTokensToSwap;
    }

    function deleteTokensToSwap0() external {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      delete tokensToSwap0;
    }

    function deleteTokensToSwap1() external {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      delete tokensToSwap1;
    }

    function deleteBlpTokensToSwap0() external {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      delete blpTokensToSwap0;
    }

    function deleteBlpTokensToSwap1() external {
      require(msg.sender == strategist || msg.sender == governance, "!authorized");
      delete blpTokensToSwap1;
    }

    function setWithdrawalFee(uint256 _withdrawalFee) external {
        require(msg.sender == governance, "!governance");
        withdrawalFee = _withdrawalFee;
    }

    function setHarvesterReward(uint256 _harvesterReward) external {
        require(msg.sender == strategist || msg.sender == governance, "!authorized");
        harvesterReward = _harvesterReward;
    }

    //In case anything goes wrong.
    //This does not increase user risk. Governance already controls funds via strategy upgrade, and is behind timelock and/or multisig.
    function executeTransaction(address target, uint value, string memory signature, bytes memory data) public payable returns (bytes memory) {
        require(msg.sender == governance, "!governance");

        bytes memory callData;

        if (bytes(signature).length == 0) {
            callData = data;
        } else {
            callData = abi.encodePacked(bytes4(keccak256(bytes(signature))), data);
        }

        // solium-disable-next-line security/no-call-value
        (bool success, bytes memory returnData) = target.call.value(value)(callData);
        require(success, "Timelock::executeTransaction: Transaction execution reverted.");

        return returnData;
    }
}



interface IUniswapRouter {
  function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
  function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
  function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IStableSwap {
  function withdraw_admin_fees() external;
  function remove_liquidity_one_coin(uint256 _token_amount, int128 i, uint256 _min_amount) external;
  function exchange(int128 i, int128 j, uint256 dx, uint256 min_dy) external returns (uint256 dy);
  function exchange_underlying(int128 i, int128 j, uint256 dx, uint256 min_dy) external returns (uint256 dy);
}

interface IBlpVault {
  function getPoolTokens(bytes32 poolId)
      external
      view
      returns (
          address[] memory tokens,
          uint256[] memory balances,
          uint256 lastChangeBlock
      );

  function exitPool(
      bytes32 poolId,
      address sender,
      address recipient,
      ExitPoolRequest calldata request
  ) external;

  struct ExitPoolRequest {
      address[] assets;
      uint256[] minAmountsOut;
      bytes userData;
      bool toInternalBalance;
  }

  function swap(
      SingleSwap calldata singleSwap,
      FundManagement calldata funds,
      uint256 limit,
      uint256 deadline
  ) external payable returns (uint256);

  struct SingleSwap {
      bytes32 poolId;
      SwapKind kind;
      address assetIn;
      address assetOut;
      uint256 amount;
      bytes userData;
  }

  enum SwapKind { GIVEN_IN, GIVEN_OUT }

  struct FundManagement {
      address sender;
      bool fromInternalBalance;
      address recipient;
      bool toInternalBalance;
  }
}

interface IBlpPool {
  enum ExitKind { EXACT_BPT_IN_FOR_ONE_TOKEN_OUT, EXACT_BPT_IN_FOR_TOKENS_OUT, BPT_IN_FOR_EXACT_TOKENS_OUT }

  function getVault() external view returns (address);
  function getPoolId() external view returns (bytes32);
}

interface IBlpFeesCollector {
  function withdrawCollectedFees(
      address[] calldata tokens,
      uint256[] calldata amounts,
      address recipient
  ) external;

  function getCollectedFeeAmounts(address[] calldata tokens) external view returns (uint256[] memory feeAmounts);
}