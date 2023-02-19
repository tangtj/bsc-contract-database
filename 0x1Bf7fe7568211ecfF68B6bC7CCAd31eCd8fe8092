pragma solidity ^0.6.0;
pragma experimental ABIEncoderV2;
// SPDX-License-Identifier: GPL-3.0-or-later

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

library xWinLib {
   
    // Info of each pool.
    struct PoolInfo {
        address lpToken;           
        uint256 rewardperblock;       
        uint256 multiplier;       
    }
    
    struct UserInfo {
        uint256 amount;     
        uint256 blockstart; 
    }

    struct TradeParams {
      address xFundAddress;
      uint256 amount;
      uint256 priceImpactTolerance;
      uint256 deadline;
      bool returnInBase;
      address referral;
    }  
   
    struct transferData {
      
      address[] targetNamesAddress;
      uint256 totalTrfAmt;
      uint256 totalUnderlying;
      uint256 qtyToTrfAToken;
    }
    
    struct xWinReward {
      uint256 blockstart;
      uint256 accBasetoken;
      uint256 accMinttoken;
      uint256 previousRealizedQty;
    }
    
    struct xWinReferral {
      address referral;
    }
    
    struct UnderWeightData {
      uint256 activeWeight;
      uint256 fundWeight;
      bool overweight;
      address token;
    }
    
    struct DeletedNames {
      address token;
      uint256 targetWeight;
    }
    
    struct PancakePriceToken {
        string tokenname;
        address addressToken;     
    }

}

// helper methods for interacting with BEP20 tokens and sending ETH that do not consistently return true/false
library TransferHelper {
    function safeApprove(
        address token,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('approve(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x095ea7b3, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: APPROVE_FAILED');
    }

    function safeTransfer(
        address token,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FAILED');
    }

    function safeTransferFrom(
        address token,
        address from,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }

    function safeTransferBNB(address to, uint256 value) internal {
        (bool success, ) = to.call{value: value}(new bytes(0));
        require(success, 'TransferHelper: BNB_TRANSFER_FAILED');
    }
}

interface xWinFund {
    
    function getManagerFee() external view returns(uint256);
    function getTargetWeight(address addr) external view returns (uint256);
    function getWhoIsManager() external view returns(address mangerAddress);
    function getBalance(address fromAdd) external view returns (uint256 balance);
    function getFundValues() external view returns (uint256);
    function getTargetWeightQty(address targetAdd, uint256 srcQty) external view returns (uint256);
    function updateManager(address managerAdd) external payable;
    function updateManagerFee(uint256 newFeebps) external payable;
    function updateRebalancePeriod(uint newCycle) external payable;
    function updateProtocol(address _newProtocol) external;
    
    function Redeem(
        xWinLib.TradeParams memory _tradeParams,
        address _investorAddress
    ) external payable returns (uint256);
        
    function Rebalance(
        address[] calldata _toAddresses, 
        uint256[] calldata _targetWeight,
        uint256 deadline,
        uint256 priceImpactTolerance
        ) external payable returns (uint256 baseccyBal);
        
    function Subscribe(
        xWinLib.TradeParams memory _tradeParams,
        address _investorAddress
    ) external payable returns (uint256);
        
    function MoveNonIndexNameToBase(
        address _tokenaddress,
        uint256 deadline,
        uint256 priceImpactTolerance
        ) external returns (uint256 balanceToken, uint256 swapOutput);
        
    function CreateTargetNames(
        address[] calldata _toAddresses, 
        uint256[] calldata _targetWeight
    ) external payable;
    
    function emergencyRedeem(uint256 redeemUnit, address _investorAddress) external payable; 
    function emergencyRemoveFromFarm() external;
   
    function getUnitPrice() external view returns(uint256 unitprice);
    function getUnitPriceInUSD() external view returns(uint256 unitprice);
    function getTargetNamesAddress() external view returns (address[] memory _targetNamesAddress);
}


interface xWinStake {
    
    function StakeReward(
        address payable _investorAddress,
        uint256 rewardQty,
        uint256 bnbQty,
        uint256 deadline
        ) external payable; 
        
    function GetQuotes(
        uint256 rewardQty, 
        uint256 baseQty,
        address targetToken
        ) external view 
        returns (uint amountB, uint amountA, uint amountOut); 
}


contract xWinDefi is Ownable, ReentrancyGuard {
    
    using SafeMath for uint256;
    using SafeBEP20 for IBEP20;


    string public name;
    address public xWinToken;
    address private platformWallet;
    address public xwinBenefitPool;
    address private deployeraddress;
    address private stakeAddress;
    uint256 private platformFeeBps;
    uint256 public startblock;
    bool public emergencyOn = false;
    mapping (uint256 => mapping (address => xWinLib.UserInfo)) public userInfo;
    mapping(address => bool) public isxwinFund;
    xWinLib.PoolInfo[] public poolInfo;
    
    mapping(address => xWinLib.xWinReward) public xWinRewards;
    mapping(address => xWinLib.xWinReferral) public xWinReferral;
    uint256 private rewardperuint = 95129375951;
    uint256 private referralperunit = 100000000000000000;
    uint256 private managerRewardperunit = 50000000000000000;
    uint256 public rewardRemaining = 60000000000000000000000000;
    
    event Received(address, uint);

    event _MoveNonIndexNameToBaseEvent(address indexed from, address indexed toFund, address tokenAddress, uint256 amount, uint swapOutput);
    event _RebalanceAllInOne(address indexed from, address indexed toFund, uint256 baseBalance, uint txnTime);
    event _Subscribe(address indexed from, address indexed toFund, uint256 subsAmt, uint256 mintQty);
    event _Redeem(address indexed from, address indexed toFund, uint256 redeemUnit, uint256 rewardQty, uint256 redeemratio);
    event _CreateTarget(address indexed from, address indexed toFund, address[] newTargets, uint256[] newWeight, uint txnTime);
    event _StakeMyReward(address indexed from, uint256 rewardQty);
    event _WithdrawReward(address indexed from, uint256 rewardQty);
    event _DepositFarm(address indexed from, uint256 pid, uint256 amount);
    event _WithdrawFarm(address indexed from, uint256 pid, uint256 amount);
    event _EmergencyRedeem(address indexed user, address fundaddress, uint256 amount);
    
    modifier onlyEmergency {
        require(emergencyOn == true, "only emergency can call this");
        _;
    }
    
    modifier onlyNonEmergency {
        require(emergencyOn == false, "only non-emergency can call this");
        _;
    }
    
    constructor (
            uint256 _platformFeeBps,
            address _platformWallet,
            address _xwinBenefitPool,
            address _stakeAddress,
            address _xWinToken
        ) public {
        
        name = "xWinDefi Protocol";
        platformWallet = _platformWallet;
        xwinBenefitPool = _xwinBenefitPool;
        platformFeeBps = _platformFeeBps;
        deployeraddress = msg.sender;
        startblock = block.number;
        stakeAddress = _stakeAddress;
        xWinToken = _xWinToken;
    }
    
    receive() external payable {
        emit Received(msg.sender, msg.value);
    }
    
    function addxwinFund(address[] calldata _fundaddress, bool [] memory _isxwinFund) public onlyOwner {
        
        for (uint i = 0; i < _fundaddress.length; i++) {
            isxwinFund[_fundaddress[i]] = _isxwinFund[i];
        }
    }
    
    function poolLength() external view returns (uint256) {
        return poolInfo.length;
    }
    
    // Add a new lp to the pool. Can only be called by the owner.
    // DO NOT add the same LP token more than once. Rewards will be messed up if you do.
    function add(address _lpToken, uint256 _rewardperblock, uint256 _multiplier) public onlyOwner onlyNonEmergency {
        
        poolInfo.push(xWinLib.PoolInfo({
            lpToken: _lpToken,
            rewardperblock : _rewardperblock,
            multiplier : _multiplier
        }));
    }
    
    // Withdraw without caring about rewards. EMERGENCY ONLY.
    function emergencyWithdraw(uint256 _pid) public onlyEmergency {
        
        xWinLib.PoolInfo memory pool = poolInfo[_pid];
        xWinLib.UserInfo storage user = userInfo[_pid][msg.sender];
        TransferHelper.safeTransfer(pool.lpToken, msg.sender, user.amount);
        user.amount = 0;
        user.blockstart = 0;
    }
    
    /// @dev reward per block by deployer
    function updateRewardPerBlock(uint256 _rewardperblock) external onlyOwner {
        rewardperuint = _rewardperblock;
    }
    
    /// @dev turn on emerrgency state by deployer
    function updateEmergencyState(bool _state) external onlyOwner {
        emergencyOn = _state;
    }
    
    /// @dev update xwin defi protocol
    function updateProtocol(address _fundaddress, address _newProtocol) external onlyOwner {
        xWinFund _xWinFund = xWinFund(_fundaddress);
        _xWinFund.updateProtocol(_newProtocol);
    }
    
     /// @dev create or update farm pool fee by deployer
    function updateFarmPoolInfo(uint256 _pid, uint256 _rewardperblock, uint256 _multiplier) external onlyOwner {
        
        xWinLib.PoolInfo storage pool = poolInfo[_pid];
        if(pool.lpToken != address(0)){
            pool.rewardperblock = _rewardperblock;
            pool.multiplier = _multiplier;
        }
    }
    
    /// @dev View function to see all pending xWin token earn on frontend.
    function getAllPendingXwin(address _user) public view returns (uint256) {
        
        uint256 length = poolInfo.length;
        uint256 total = 0;
        for (uint256 pid = 0; pid < length; ++pid) {
            total = total.add(pendingXwin(pid, _user));
        }
        return total;
    }
    
    /// @dev View function to see pending xWin on frontend.
    function pendingXwin(uint256 _pid, address _user) public view returns (uint256) {
        
        if(rewardRemaining == 0) return 0;
        xWinLib.PoolInfo memory pool = poolInfo[_pid];
        xWinLib.UserInfo memory user = userInfo[_pid][_user];
        uint blockdiff = block.number.sub(user.blockstart);
        uint256 currentRealizedQty = pool.multiplier.mul(pool.rewardperblock).mul(blockdiff).mul(user.amount).div(1e18).div(100);
        return currentRealizedQty;
    }
    
    /// @dev Deposit LP tokens to xWin Protocol for xWin allocation.
    function DepositFarm(uint256 _pid, uint256 _amount) public nonReentrant onlyNonEmergency {

        xWinLib.PoolInfo memory pool = poolInfo[_pid];
        require(pool.lpToken != address(0), "No pool found");
        xWinLib.UserInfo storage user = userInfo[_pid][msg.sender];
        if (user.amount > 0) {
            uint256 pending = pendingXwin(_pid, msg.sender);
            _sendRewards(msg.sender, pending);
        }
        if (_amount > 0) {
            TransferHelper.safeTransferFrom(pool.lpToken, msg.sender, address(this), _amount);
            user.amount = user.amount.add(_amount);
        }
        user.blockstart = block.number;
        emit _DepositFarm(msg.sender, _pid, _amount);
    }
    
    /// @dev Withdraw LP tokens from xWin Protocol.
    function WithdrawFarm(uint256 _pid, uint256 _amount) public nonReentrant onlyNonEmergency {

        xWinLib.PoolInfo memory pool = poolInfo[_pid];
        xWinLib.UserInfo storage user = userInfo[_pid][msg.sender];
        require(user.amount >= _amount, "withdraw: not good");

        uint256 pending = pendingXwin(_pid, msg.sender);
        if(pending > 0) _sendRewards(msg.sender, pending);
        
        if(_amount > 0) {
            user.amount = user.amount.sub(_amount);
            TransferHelper.safeTransfer(pool.lpToken, msg.sender, _amount);
        }
        user.blockstart = block.number;
        emit _WithdrawFarm(msg.sender, _pid, _amount);
    }
    
   
    /// @dev perform subscription based on ratio setup and put into lending if available 
    function Subscribe(xWinLib.TradeParams memory _tradeParams) public nonReentrant onlyNonEmergency payable {
        
        require(isxwinFund[_tradeParams.xFundAddress] == true, "not xwin fund");
        xWinLib.xWinReferral memory _xWinReferral = xWinReferral[msg.sender];
        require(msg.sender != _tradeParams.referral, "referal cannot be own address");
        
        if(_xWinReferral.referral != address(0)){
            require(_xWinReferral.referral == _tradeParams.referral, "already had referral");
        }
        xWinFund _xWinFund = xWinFund(_tradeParams.xFundAddress);
        TransferHelper.safeTransferBNB(_tradeParams.xFundAddress, _tradeParams.amount);
        uint256 mintQty = _xWinFund.Subscribe(_tradeParams, msg.sender);
        
        if(rewardRemaining > 0){
            _storeRewardQty(msg.sender, _tradeParams.amount, mintQty);
            _updateReferralReward(_tradeParams, _xWinFund.getWhoIsManager());
        }
        emit _Subscribe(msg.sender, _tradeParams.xFundAddress, _tradeParams.amount, mintQty);
    }
    
    /// @dev perform redemption based on unit redeem
    function Redeem(xWinLib.TradeParams memory _tradeParams) external nonReentrant onlyNonEmergency payable {
        
        require(IBEP20(_tradeParams.xFundAddress).balanceOf(msg.sender) >= _tradeParams.amount, "Not enough balance to redeem");
        require(isxwinFund[_tradeParams.xFundAddress] == true, "not xwin fund");
        TransferHelper.safeTransferFrom(_tradeParams.xFundAddress, msg.sender, address(this), _tradeParams.amount);
        xWinFund _xWinFund = xWinFund(_tradeParams.xFundAddress);
        uint256 redeemratio = _xWinFund.Redeem(_tradeParams, msg.sender);
        uint256 rewardQty = _updateRewardBal(msg.sender, _tradeParams.amount);
        emit _Redeem(msg.sender, _tradeParams.xFundAddress, _tradeParams.amount, rewardQty, redeemratio);
    }
    
    /// @dev perform redemption based on unit redeem and give up all xwin rewards
    function emergencyRedeem(uint256 _redeemAmount, address _fundaddress) external nonReentrant onlyEmergency payable {
        
        require(IBEP20(_fundaddress).balanceOf(msg.sender) >= _redeemAmount, "Not enough balance to redeem");
        TransferHelper.safeTransferFrom(_fundaddress, msg.sender, address(this), _redeemAmount);
        xWinFund _xWinFund = xWinFund(_fundaddress);
        _xWinFund.emergencyRedeem(_redeemAmount, msg.sender);
        _resetRewards(msg.sender);
        emit _EmergencyRedeem(msg.sender, _fundaddress, _redeemAmount);
    }
    
    /// @dev manager perform remove from farm for emergency state
    function emergencyRemoveFromFarm(address _fundaddress) external nonReentrant onlyEmergency payable {
        
        xWinFund _xWinFund = xWinFund(_fundaddress);
        require(msg.sender == _xWinFund.getWhoIsManager(), "not the manager to move from farm");
        _xWinFund.emergencyRemoveFromFarm();
    }
    
    /// @dev perform MoveNonIndexNameTo BNB for non benchmark name
    function MoveNonIndexNameToBase(
        address xFundAddress,
        address _tokenaddress,
        uint256 deadline,
        uint256 priceImpactTolerance
        ) external nonReentrant payable {
        
        xWinFund _xWinFund = xWinFund(xFundAddress);
        require(msg.sender == _xWinFund.getWhoIsManager(), "not the manager to move the balance");
         (uint256 balanceToken, uint256 swapOutput) = _xWinFund.MoveNonIndexNameToBase(_tokenaddress, deadline, priceImpactTolerance);
        emit _MoveNonIndexNameToBaseEvent(msg.sender, xFundAddress, _tokenaddress, balanceToken, swapOutput);
    }
    
    /// @dev create target ratio by portfolio manager
    function CreateTarget(
        address[] calldata _toAddresses, 
        uint256[] calldata _targetWeight,
        address xFundAddress 
        ) external nonReentrant onlyNonEmergency {
        
        xWinFund _xWinFund = xWinFund(xFundAddress);
        require(msg.sender == _xWinFund.getWhoIsManager(), "only owner of the fund is allowed");
        _xWinFund.CreateTargetNames(_toAddresses, _targetWeight);
        emit _CreateTarget(msg.sender, xFundAddress, _toAddresses, _targetWeight, block.timestamp);
    }
    
    /// @dev perform update target, move non-bm to base and finally rebalance
    function RebalanceAllInOne(
        xWinLib.TradeParams memory _tradeParams,
        address[] calldata _toAddresses, 
        uint256[] calldata _targetWeight
        ) external nonReentrant onlyNonEmergency payable {
        
        xWinFund _xWinFund = xWinFund(_tradeParams.xFundAddress);
        require(msg.sender == _xWinFund.getWhoIsManager(), "only owner of the fund is allowed");
        
        uint256 baseccyBal = _xWinFund.Rebalance(_toAddresses, _targetWeight, _tradeParams.deadline, _tradeParams.priceImpactTolerance);
        emit _RebalanceAllInOne(msg.sender, _tradeParams.xFundAddress, baseccyBal, block.timestamp);
    }
    
    /// @dev update platform fee by deployer
    function updatePlatformFee(uint256 newPlatformFee) external onlyOwner {
        platformFeeBps = newPlatformFee;
    }
    
    /// @dev get platform fee
    function getPlatformFee() view external returns (uint256) {
        return platformFeeBps;
    }
    
    /// @dev get platform wallet address
    function getPlatformAddress() view external returns (address) {
        return platformWallet;
    }
    
    /// @dev get platform wallet address
    function gexWinBenefitPool() view external returns (address) {
        return xwinBenefitPool;
    }
    
    /// @dev update platform fee by deployer
    function updateXwinBenefitPool(address _xwinBenefitPool) external onlyOwner {
        xwinBenefitPool = _xwinBenefitPool;
    }
    
    /// @dev update rewardRemaining by deployer
    function updateRewardRemaining(uint256 _newRemaining) external onlyOwner {
        rewardRemaining = _newRemaining;
    }
    
    /// @dev update platform fee by deployer
    function updateStakeProtocol(address newStakeProtocol) external onlyOwner {
        stakeAddress = newStakeProtocol;
    }
    
    /// @dev update referal fee by deployer
    function updateReferralRewardPerUnit(uint256 _referralperunit) external onlyOwner {
        referralperunit = _referralperunit;
    }

    /// @dev update manager reward by deployer
    function updateManagerRewardPerUnit(uint256 _managerRewardperunit) external onlyOwner {
        managerRewardperunit = _managerRewardperunit;
    }
    
    function _multiplier(uint256 _blockstart) internal view returns (uint256) {
        
        if(_blockstart == 0) return 0;
        uint256 blockdiff = _blockstart.sub(startblock); 
        if(blockdiff < 5256000) return 50000; //first 6 months, 5x
        if(blockdiff >= 5256000 && blockdiff <= 10512000) return 25000; //then following 6 months 
        if(blockdiff >= 10512000 && blockdiff <= 15768000) return 12500; //then following 6 months
        if(blockdiff > 15768000) return 10000;
    }
    
    /// @dev get estimated reward of XWN token
    function GetEstimateReward(address fromAddress) public view returns (uint256) {
        
        xWinLib.xWinReward memory _xwinReward =  xWinRewards[fromAddress];
        if(_xwinReward.blockstart == 0) return 0;
        uint blockdiff = block.number.sub(_xwinReward.blockstart);
        uint256 currentRealizedQty = _multiplier(_xwinReward.blockstart).mul(rewardperuint).mul(blockdiff).mul(_xwinReward.accBasetoken).div(1e18).div(10000); 
        uint256 allRealizedQty = currentRealizedQty.add(_xwinReward.previousRealizedQty);
        return  (rewardRemaining >= allRealizedQty) ? allRealizedQty: rewardRemaining;
    }
    
    function GetQuotes(
        uint tokenBal,
        address targetToken
        ) external view returns (uint amountB, uint amountA) {
        xWinStake _xWinStake = xWinStake(stakeAddress);
        (amountB, amountA, ) = _xWinStake.GetQuotes(tokenBal, 1e18, targetToken);
        return (amountB, amountA);
    }   
    
    /// @dev User to claim the reward and stake them into DEX
    function StakeMyReward(
        uint256 deadline 
        ) external nonReentrant onlyNonEmergency payable {
        
        //only token owner are allowed
        xWinLib.xWinReward storage _xwinReward =  xWinRewards[msg.sender];
        uint256 rewardQty = GetEstimateReward(msg.sender);
        require(rewardQty > 0, "No reward to claim");
        
        _xwinReward.previousRealizedQty = 0;
        _xwinReward.blockstart = block.number;
        
        xWinStake _xWinStake = xWinStake(stakeAddress);
        TransferHelper.safeTransferBNB(stakeAddress, msg.value); 
        
        _sendRewards(stakeAddress, rewardQty);

        _xWinStake.StakeReward(msg.sender, rewardQty, msg.value, deadline);
        emit _StakeMyReward(msg.sender, rewardQty);
    }
    
    function _updateReferralReward(xWinLib.TradeParams memory _tradeParams, address _managerAddress) internal {
        
        xWinLib.xWinReferral storage _xWinReferral = xWinReferral[msg.sender];
        if(_xWinReferral.referral == address(0)){
            _xWinReferral.referral = _tradeParams.referral; //store referal address
        }
        xWinLib.xWinReward storage _xwinReward =  xWinRewards[_xWinReferral.referral];
        
        if(_xwinReward.accBasetoken > 0){
            uint256 entitleAmt = _tradeParams.amount.mul(referralperunit).div(1e18);  //0.10
            _xwinReward.previousRealizedQty = _xwinReward.previousRealizedQty.add(entitleAmt);
        } 

        xWinLib.xWinReward storage _xwinRewardManager =  xWinRewards[_managerAddress];
        if(_xwinRewardManager.blockstart == 0){
            _xwinRewardManager.blockstart = block.number;
        }
        uint256 entitleAmtManager = _tradeParams.amount.mul(managerRewardperunit).div(1e18); //manager get 0.05
        _xwinRewardManager.previousRealizedQty = _xwinRewardManager.previousRealizedQty.add(entitleAmtManager);
    }
    
    /// @dev withdraw reward of XWN token
    function WithdrawReward() external nonReentrant onlyNonEmergency payable {
        
        xWinLib.xWinReward storage _xwinReward =  xWinRewards[msg.sender];
        uint256 rewardQty = GetEstimateReward(msg.sender);
        require(rewardQty > 0, "No reward");
        
        _xwinReward.previousRealizedQty = 0;
        _xwinReward.blockstart = block.number;
        
        uint amountWithdraw = (rewardRemaining >= rewardQty) ? rewardQty: rewardRemaining;
        
        if(amountWithdraw > 0) _sendRewards(msg.sender, amountWithdraw);
        emit _WithdrawReward(msg.sender, amountWithdraw);
    }
    
    function _storeRewardQty(address from, uint256 baseQty, uint256 mintQty) internal {

        xWinLib.xWinReward storage _xwinReward =  xWinRewards[from];
        if(_xwinReward.blockstart == 0){
            _xwinReward.blockstart = block.number;
            _xwinReward.accBasetoken = baseQty;
            _xwinReward.accMinttoken = mintQty;
            _xwinReward.previousRealizedQty = 0;
        }else{
            
            uint blockdiff = block.number.sub(_xwinReward.blockstart);
            uint256 currentRealizedQty = _multiplier(_xwinReward.blockstart).mul(rewardperuint).mul(blockdiff).mul(_xwinReward.accBasetoken).div(1e18).div(10000); 
            _xwinReward.blockstart = block.number;
            _xwinReward.accBasetoken = baseQty.add(_xwinReward.accBasetoken);
            _xwinReward.accMinttoken = mintQty.add(_xwinReward.accMinttoken);
            _xwinReward.previousRealizedQty = _xwinReward.previousRealizedQty.add(currentRealizedQty);
        }
    }
    
    function _updateRewardBal(address from, uint256 redeemUnit) internal returns (uint256 rewardQty){

        if(rewardRemaining == 0) return 0;
        xWinLib.xWinReward storage _xwinReward =  xWinRewards[from];
        rewardQty = GetEstimateReward(from);
        
        if(_xwinReward.accMinttoken == 0) return 0;
        if(rewardQty == 0) return 0;
        
        if(_xwinReward.accMinttoken >= redeemUnit){
            uint256 ratio = redeemUnit.mul(1e8).div(_xwinReward.accMinttoken);
            uint256 reducedBal = _xwinReward.accBasetoken.mul(ratio).div(1e8);
            _xwinReward.accBasetoken = _xwinReward.accBasetoken.sub(reducedBal);    
            _xwinReward.accMinttoken = _xwinReward.accMinttoken.sub(redeemUnit);
        }else{
            _xwinReward.accMinttoken = 0;
            _xwinReward.accBasetoken = 0;
        }
        _xwinReward.previousRealizedQty = 0;
        _xwinReward.blockstart = block.number;
        
        _sendRewards(msg.sender, rewardQty);
        return rewardQty;
    }
    
    /// @dev emergency trf XWN token to new protocol
    function ProtocolTransfer(address _newProtocol, uint256 amount) public onlyOwner onlyEmergency payable {
        TransferHelper.safeTransfer(xWinToken, _newProtocol, amount);
    }
    
    function _sendRewards(address _to, uint256 amount) internal {
        
        if(rewardRemaining == 0) return;
        uint256 xwinTokenBal = IBEP20(xWinToken).balanceOf(address(this));
        if(xwinTokenBal == 0) return;
        
        if(rewardRemaining >= amount && xwinTokenBal >= amount){
            TransferHelper.safeTransfer(xWinToken, _to, amount);
            rewardRemaining = rewardRemaining.sub(amount);
        }else{
            uint amountTosend = (xwinTokenBal >= amount) ? amount: xwinTokenBal;
            TransferHelper.safeTransfer(xWinToken, _to, amountTosend);
            rewardRemaining = 0; //mark reward ended
        }
    }
    
    function _resetRewards(address _from) internal {
        xWinLib.xWinReward storage _xwinReward =  xWinRewards[_from];
        _xwinReward.accMinttoken = 0;
        _xwinReward.accBasetoken = 0;
        _xwinReward.previousRealizedQty = 0;
        _xwinReward.blockstart = block.number;
    }
}