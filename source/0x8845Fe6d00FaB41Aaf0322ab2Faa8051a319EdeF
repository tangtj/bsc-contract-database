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

// File: interfaces/yearn/Token.sol

// SPDX-License-Identifier: MIT

pragma solidity ^0.5.17;

// NOTE: Basically an alias for Vaults
interface yERC20 {
    function deposit(uint256 _amount) external;

    function withdraw(uint256 _amount) external;

    function getPricePerFullShare() external view returns (uint256);
}

// File: contracts/strategies/StrategyACryptoSVenusLeverageV2.sol

pragma solidity ^0.5.17;






// import "../../interfaces/curve/Curve.sol";
// import "../../interfaces/curve/Mintr.sol";




interface IUniswapRouter {
  function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
}

interface IVenusComptroller {
  function _addVenusMarkets ( address[] calldata vTokens ) external;
  function _become ( address unitroller ) external;
  function _borrowGuardianPaused (  ) external view returns ( bool );
  function _dropVenusMarket ( address vToken ) external;
  function _mintGuardianPaused (  ) external view returns ( bool );
  function _setCloseFactor ( uint256 newCloseFactorMantissa ) external returns ( uint256 );
  function _setCollateralFactor ( address vToken, uint256 newCollateralFactorMantissa ) external returns ( uint256 );
  function _setLiquidationIncentive ( uint256 newLiquidationIncentiveMantissa ) external returns ( uint256 );
  function _setMaxAssets ( uint256 newMaxAssets ) external returns ( uint256 );
  function _setPauseGuardian ( address newPauseGuardian ) external returns ( uint256 );
  function _setPriceOracle ( address newOracle ) external returns ( uint256 );
  function _setProtocolPaused ( bool state ) external returns ( bool );
  function _setVAIController ( address vaiController_ ) external returns ( uint256 );
  function _setVAIMintRate ( uint256 newVAIMintRate ) external returns ( uint256 );
  function _setVenusRate ( uint256 venusRate_ ) external;
  function _supportMarket ( address vToken ) external returns ( uint256 );
  function accountAssets ( address, uint256 ) external view returns ( address );
  function admin (  ) external view returns ( address );
  function allMarkets ( uint256 ) external view returns ( address );
  function borrowAllowed ( address vToken, address borrower, uint256 borrowAmount ) external returns ( uint256 );
  function borrowGuardianPaused ( address ) external view returns ( bool );
  function borrowVerify ( address vToken, address borrower, uint256 borrowAmount ) external;
  function checkMembership ( address account, address vToken ) external view returns ( bool );
  function claimVenus ( address holder, address[] calldata vTokens ) external;
  function claimVenus ( address holder ) external;
  function claimVenus ( address[] calldata holders, address[] calldata vTokens, bool borrowers, bool suppliers ) external;
  function closeFactorMantissa (  ) external view returns ( uint256 );
  function comptrollerImplementation (  ) external view returns ( address );
  function enterMarkets ( address[] calldata vTokens ) external returns ( uint256[] memory );
  function exitMarket ( address vTokenAddress ) external returns ( uint256 );
  function getAccountLiquidity ( address account ) external view returns ( uint256, uint256, uint256 );
  function getAllMarkets (  ) external view returns ( address[] memory );
  function getAssetsIn ( address account ) external view returns ( address[] memory );
  function getBlockNumber (  ) external view returns ( uint256 );
  function getHypotheticalAccountLiquidity ( address account, address vTokenModify, uint256 redeemTokens, uint256 borrowAmount ) external view returns ( uint256, uint256, uint256 );
  function getMintableVAI ( address minter ) external view returns ( uint256, uint256 );
  function getVAIMintRate (  ) external view returns ( uint256 );
  function getXVSAddress (  ) external view returns ( address );
  function isComptroller (  ) external view returns ( bool );
  function liquidateBorrowAllowed ( address vTokenBorrowed, address vTokenCollateral, address liquidator, address borrower, uint256 repayAmount ) external returns ( uint256 );
  function liquidateBorrowVerify ( address vTokenBorrowed, address vTokenCollateral, address liquidator, address borrower, uint256 actualRepayAmount, uint256 seizeTokens ) external;
  function liquidateCalculateSeizeTokens ( address vTokenBorrowed, address vTokenCollateral, uint256 actualRepayAmount ) external view returns ( uint256, uint256 );
  function liquidationIncentiveMantissa (  ) external view returns ( uint256 );
  function markets ( address ) external view returns ( bool isListed, uint256 collateralFactorMantissa, bool isVenus );
  function maxAssets (  ) external view returns ( uint256 );
  function mintAllowed ( address vToken, address minter, uint256 mintAmount ) external returns ( uint256 );
  function mintGuardianPaused ( address ) external view returns ( bool );
  function mintVAI ( uint256 mintVAIAmount ) external returns ( uint256 );
  function mintVAIGuardianPaused (  ) external view returns ( bool );
  function mintVerify ( address vToken, address minter, uint256 actualMintAmount, uint256 mintTokens ) external;
  function mintedVAIOf ( address owner ) external view returns ( uint256 );
  function mintedVAIs ( address ) external view returns ( uint256 );
  function oracle (  ) external view returns ( address );
  function pauseGuardian (  ) external view returns ( address );
  function pendingAdmin (  ) external view returns ( address );
  function pendingComptrollerImplementation (  ) external view returns ( address );
  function protocolPaused (  ) external view returns ( bool );
  function redeemAllowed ( address vToken, address redeemer, uint256 redeemTokens ) external returns ( uint256 );
  function redeemVerify ( address vToken, address redeemer, uint256 redeemAmount, uint256 redeemTokens ) external;
  function refreshVenusSpeeds (  ) external;
  function repayBorrowAllowed ( address vToken, address payer, address borrower, uint256 repayAmount ) external returns ( uint256 );
  function repayBorrowVerify ( address vToken, address payer, address borrower, uint256 actualRepayAmount, uint256 borrowerIndex ) external;
  function repayVAI ( uint256 repayVAIAmount ) external returns ( uint256 );
  function repayVAIGuardianPaused (  ) external view returns ( bool );
  function seizeAllowed ( address vTokenCollateral, address vTokenBorrowed, address liquidator, address borrower, uint256 seizeTokens ) external returns ( uint256 );
  function seizeGuardianPaused (  ) external view returns ( bool );
  function seizeVerify ( address vTokenCollateral, address vTokenBorrowed, address liquidator, address borrower, uint256 seizeTokens ) external;
  function setMintedVAIOf ( address owner, uint256 amount ) external returns ( uint256 );
  function transferAllowed ( address vToken, address src, address dst, uint256 transferTokens ) external returns ( uint256 );
  function transferGuardianPaused (  ) external view returns ( bool );
  function transferVerify ( address vToken, address src, address dst, uint256 transferTokens ) external;
  function vaiController (  ) external view returns ( address );
  function vaiMintRate (  ) external view returns ( uint256 );
  function venusAccrued ( address ) external view returns ( uint256 );
  function venusBorrowState ( address ) external view returns ( uint224 index, uint32 block );
  function venusBorrowerIndex ( address, address ) external view returns ( uint256 );
  function venusClaimThreshold (  ) external view returns ( uint256 );
  function venusInitialIndex (  ) external view returns ( uint224 );
  function venusRate (  ) external view returns ( uint256 );
  function venusSpeeds ( address ) external view returns ( uint256 );
  function venusSupplierIndex ( address, address ) external view returns ( uint256 );
  function venusSupplyState ( address ) external view returns ( uint224 index, uint32 block );
}

interface IVToken {
  function _acceptAdmin (  ) external returns ( uint256 );
  function _addReserves ( uint256 addAmount ) external returns ( uint256 );
  function _reduceReserves ( uint256 reduceAmount ) external returns ( uint256 );
  function _setComptroller ( address newComptroller ) external returns ( uint256 );
  function _setImplementation ( address implementation_, bool allowResign, bytes calldata becomeImplementationData ) external;
  function _setInterestRateModel ( address newInterestRateModel ) external returns ( uint256 );
  function _setPendingAdmin ( address newPendingAdmin ) external returns ( uint256 );
  function _setReserveFactor ( uint256 newReserveFactorMantissa ) external returns ( uint256 );
  function accrualBlockNumber (  ) external view returns ( uint256 );
  function accrueInterest (  ) external returns ( uint256 );
  function admin (  ) external view returns ( address );
  function allowance ( address owner, address spender ) external view returns ( uint256 );
  function approve ( address spender, uint256 amount ) external returns ( bool );
  function balanceOf ( address owner ) external view returns ( uint256 );
  function balanceOfUnderlying ( address owner ) external returns ( uint256 );
  function borrow ( uint256 borrowAmount ) external returns ( uint256 );
  function borrowBalanceCurrent ( address account ) external returns ( uint256 );
  function borrowBalanceStored ( address account ) external view returns ( uint256 );
  function borrowIndex (  ) external view returns ( uint256 );
  function borrowRatePerBlock (  ) external view returns ( uint256 );
  function comptroller (  ) external view returns ( address );
  function decimals (  ) external view returns ( uint8 );
  function delegateToImplementation ( bytes calldata data ) external returns ( bytes memory );
  function delegateToViewImplementation ( bytes calldata data ) external view returns ( bytes memory );
  function exchangeRateCurrent (  ) external returns ( uint256 );
  function exchangeRateStored (  ) external view returns ( uint256 );
  function getAccountSnapshot ( address account ) external view returns ( uint256, uint256, uint256, uint256 );
  function getCash (  ) external view returns ( uint256 );
  function implementation (  ) external view returns ( address );
  function interestRateModel (  ) external view returns ( address );
  function isVToken (  ) external view returns ( bool );
  function liquidateBorrow ( address borrower, uint256 repayAmount, address vTokenCollateral ) external returns ( uint256 );
  function mint ( uint256 mintAmount ) external returns ( uint256 );
  function name (  ) external view returns ( string memory );
  function pendingAdmin (  ) external view returns ( address );
  function redeem ( uint256 redeemTokens ) external returns ( uint256 );
  function redeemUnderlying ( uint256 redeemAmount ) external returns ( uint256 );
  function repayBorrow ( uint256 repayAmount ) external returns ( uint256 );
  function repayBorrowBehalf ( address borrower, uint256 repayAmount ) external returns ( uint256 );
  function reserveFactorMantissa (  ) external view returns ( uint256 );
  function seize ( address liquidator, address borrower, uint256 seizeTokens ) external returns ( uint256 );
  function supplyRatePerBlock (  ) external view returns ( uint256 );
  function symbol (  ) external view returns ( string memory );
  function totalBorrows (  ) external view returns ( uint256 );
  function totalBorrowsCurrent (  ) external returns ( uint256 );
  function totalReserves (  ) external view returns ( uint256 );
  function totalSupply (  ) external view returns ( uint256 );
  function transfer ( address dst, uint256 amount ) external returns ( bool );
  function transferFrom ( address src, address dst, uint256 amount ) external returns ( bool );
  function underlying (  ) external view returns ( address );
}


contract StrategyACryptoSVenusLeverageV2 {
    using SafeERC20 for IERC20;
    using Address for address;
    using SafeMath for uint256;
    using Math for uint256;

    address public constant xvs = address(0xcF6BB5389c92Bdda8a3747Ddb454cB7a64626C63);
    address public constant wbnb = address(0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c);
    address public constant venusComptroller = address(0xfD36E2c2a6789Db23113685031d7F16329158384);
    address public constant uniswapRouter = address(0x05fF2B0DB69458A0750badebc4f9e13aDd608C7F);

    address public want;
    address public vToken;
    uint256 public targetBorrowLimit;
    uint256 public targetBorrowLimitHysteresis;

    address public governance;
    address public controller;
    address public strategist;

    uint256 public performanceFee = 450;
    uint256 public strategistReward = 50;
    uint256 public withdrawalFee = 50;
    uint256 public harvesterReward = 30;
    uint256 public constant FEE_DENOMINATOR = 10000;

    constructor(address _controller, address _want, address _vToken, uint _targetBorrowLimit, uint _targetBorrowLimitHysteresis) public {
        governance = msg.sender;
        strategist = msg.sender;
        controller = _controller;

        want = _want;
        vToken = _vToken;
        targetBorrowLimit = _targetBorrowLimit;
        targetBorrowLimitHysteresis = _targetBorrowLimitHysteresis;

        address[] memory _markets = new address[](1);
        _markets[0] = vToken;
        IVenusComptroller(venusComptroller).enterMarkets(_markets);
    }

    function getName() external pure returns (string memory) {
        return "StrategyACryptoSVenusLeverage";
    }

    function deposit() public {
      uint256 _want = IERC20(want).balanceOf(address(this));
      if (_want > 0) {
        _supplyWant();
        _rebalance(0);
      }
    }

    function _supplyWant() internal {
      uint256 _want = IERC20(want).balanceOf(address(this));
      IERC20(want).safeApprove(vToken, 0);
      IERC20(want).safeApprove(vToken, _want);
      IVToken(vToken).mint(_want);
    }

    function _claimXvs() internal {
      address[] memory _markets = new address[](1);
      _markets[0] = vToken;
      IVenusComptroller(venusComptroller).claimVenus(address(this), _markets);
    }


    function _rebalance(uint withdrawAmount) internal {
      uint256 _ox = IVToken(vToken).balanceOfUnderlying(address(this));
      if(withdrawAmount >= _ox) withdrawAmount = _ox.sub(1);
      uint256 _x = _ox.sub(withdrawAmount);
      uint256 _y = IVToken(vToken).borrowBalanceCurrent(address(this));
      uint256 _c = collateralFactor();
      uint256 _L = _c.mul(targetBorrowLimit).div(1e18);
      uint256 _currentL = _y.mul(1e18).div(_x);
      uint256 _liquidityAvailable = IVToken(vToken).getCash();

      if(_currentL < _L && _L.sub(_currentL) > targetBorrowLimitHysteresis) {
        uint256 _dy = _L.mul(_x).div(1e18).sub(_y).mul(1e18).div(uint256(1e18).sub(_L));
        uint256 _max_dy = _ox.mul(_c).div(1e18).sub(_y);
        if(_dy > _max_dy) _dy = _max_dy;
        if(_dy > _liquidityAvailable) _dy = _liquidityAvailable;
        IVToken(vToken).borrow(_dy);
        _supplyWant();
      } else {
        while(_currentL > _L && _currentL.sub(_L) > targetBorrowLimitHysteresis) {
          uint256 _dy = _y.sub(_L.mul(_x).div(1e18)).mul(1e18).div(uint256(1e18).sub(_L));
          uint256 _max_dy = _ox.sub(_y.mul(1e18).div(_c));
          if(_dy > _max_dy) _dy = _max_dy;
          if(_dy > _liquidityAvailable) _dy = _liquidityAvailable;
          require(IVToken(vToken).redeemUnderlying(_dy) == 0, "_rebalance: redeem failed");

          _ox = _ox.sub(_dy);
          if(withdrawAmount >= _ox) withdrawAmount = _ox.sub(1);
          _x = _ox.sub(withdrawAmount);

          if(_dy > _y) _dy = _y;
          IERC20(want).safeApprove(vToken, 0);
          IERC20(want).safeApprove(vToken, _dy);
          IVToken(vToken).repayBorrow(_dy);
          _y = _y.sub(_dy);

          _currentL = _y.mul(1e18).div(_x);
          _liquidityAvailable = IVToken(vToken).getCash();
        }
      }



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
          _amount = _withdrawSome(_amount.sub(_balance));
          _amount = _amount.add(_balance);
      }

      uint256 _fee = _amount.mul(withdrawalFee).div(FEE_DENOMINATOR);
      IERC20(want).safeTransfer(IController(controller).rewards(), _fee);
      address _vault = IController(controller).vaults(address(want));
      require(_vault != address(0), "!vault"); // additional protection so we don't burn the funds
      IERC20(want).safeTransfer(_vault, _amount.sub(_fee));
    }

    function _withdrawSome(uint256 _amount) internal returns (uint256) {
      _rebalance(_amount);
      uint _balance = IVToken(vToken).balanceOfUnderlying(address(this));
      if(_amount > _balance) _amount = _balance;
      require(IVToken(vToken).redeemUnderlying(_amount) == 0, "_withdrawSome: redeem failed");
      return _amount;
    }

    // Withdraw all funds, normally used when migrating strategies
    function withdrawAll() external returns (uint256 balance) {
      require(msg.sender == controller || msg.sender == strategist || msg.sender == governance, "!authorized");
      _withdrawAll();

      balance = IERC20(want).balanceOf(address(this));

      address _vault = IController(controller).vaults(address(want));
      require(_vault != address(0), "!vault"); // additional protection so we don't burn the funds
      IERC20(want).safeTransfer(_vault, balance);
    }

    function _withdrawAll() internal {
      targetBorrowLimit = 0;
      targetBorrowLimitHysteresis = 0;
      _rebalance(0);
      require(IVToken(vToken).redeem(IVToken(vToken).balanceOf(address(this))) == 0, "_withdrawAll: redeem failed");      
    }

    function _convertRewardsToWant() internal {
      uint256 _xvs = IERC20(xvs).balanceOf(address(this));
      if(_xvs > 0 ) {
        IERC20(xvs).safeApprove(uniswapRouter, 0);
        IERC20(xvs).safeApprove(uniswapRouter, _xvs);

        address[] memory path = new address[](3);
        path[0] = xvs;
        path[1] = wbnb;
        path[2] = want;

        IUniswapRouter(uniswapRouter).swapExactTokensForTokens(_xvs, uint256(0), path, address(this), now.add(1800));
      }
    }

    function balanceOfWant() public view returns (uint256) {
        return IERC20(want).balanceOf(address(this));
    }

    function balanceOfStakedWant() public view returns (uint256) {
      return IVToken(vToken).balanceOf(address(this)).mul(IVToken(vToken).exchangeRateStored()).div(1e18)
        .sub(IVToken(vToken).borrowBalanceStored(address(this)));
    }

    function balanceOfStakedWantCurrent() public returns (uint256) {
      return IVToken(vToken).balanceOfUnderlying(address(this))
        .sub(IVToken(vToken).borrowBalanceCurrent(address(this)));
    }

    function borrowLimit() public returns (uint256) {
      return IVToken(vToken).borrowBalanceCurrent(address(this))
        .mul(1e18).div(IVToken(vToken).balanceOfUnderlying(address(this)).mul(collateralFactor()).div(1e18));
    }

    function collateralFactor() public view returns (uint256) {
      (,uint256 _collateralFactor,) = IVenusComptroller(venusComptroller).markets(vToken);
      return _collateralFactor;
    }


    function harvest() public returns (uint harvesterRewarded) {
      // require(msg.sender == strategist || msg.sender == governance, "!authorized");
      require(msg.sender == tx.origin, "not eoa");

      _claimXvs();

      uint _xvs = IERC20(xvs).balanceOf(address(this)); 
      uint256 _harvesterReward;
      if (_xvs > 0) {
        uint256 _fee = _xvs.mul(performanceFee).div(FEE_DENOMINATOR);
        uint256 _reward = _xvs.mul(strategistReward).div(FEE_DENOMINATOR);
        _harvesterReward = _xvs.mul(harvesterReward).div(FEE_DENOMINATOR);
        IERC20(xvs).safeTransfer(IController(controller).rewards(), _fee);
        IERC20(xvs).safeTransfer(strategist, _reward);
        IERC20(xvs).safeTransfer(msg.sender, _harvesterReward);
      }

      _convertRewardsToWant();
      _supplyWant();
      _rebalance(0);

      return _harvesterReward;
    }

    function balanceOf() public view returns (uint256) {
      return balanceOfWant()
        .add(balanceOfStakedWant());
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

    function setTargetBorrowLimit(uint256 _targetBorrowLimit, uint256 _targetBorrowLimitHysteresis) external {
        require(msg.sender == strategist || msg.sender == governance, "!authorized");
        targetBorrowLimit = _targetBorrowLimit;
        targetBorrowLimitHysteresis = _targetBorrowLimitHysteresis;
    }

    //In case anything goes wrong - Venus contracts are upgradeable and we have no guarantees how they might change.
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