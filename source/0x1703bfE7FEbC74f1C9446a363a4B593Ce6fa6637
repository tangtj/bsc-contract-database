// SPDX-License-Identifier: MIT
pragma solidity 0.8.4;

interface ISwapFactory {

    function initCodeHash() external view returns (bytes32);

    function feeTo() external view returns(address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);

    function allPairs(uint index) external view returns (address pair);

    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function swapFee() external view returns (uint256);

    function protocolFee() external view returns (uint256);

    function liquidityFee() external view returns(uint256);

    function sortTokens(address tokenA, address tokenB) external view returns (address token0, address token1);

    function pairFor(address tokenA, address tokenB) external view returns (address pair);

    function getReserves(address tokenA, address tokenB) external view returns (uint256 reserveA, uint256 reserveB);

    function quote(uint256 amountA, uint256 reserveA, uint256 reserveB) external view returns (uint256 amountB);

    function getAmountOut(uint256 amountIn, uint256 reserveIn, uint256 reserveOut, address token0, address token1) external view returns (uint256 amountOut);

    function getAmountIn(uint256 amountOut, uint256 reserveIn, uint256 reserveOut, address token0, address token1) external view returns (uint256 amountIn);

    function getAmountsOut(uint256 amountIn, address[] calldata path) external view returns (uint256[] memory amounts);

    function getAmountsIn(uint256 amountOut, address[] calldata path) external view returns (uint256[] memory amounts);

    function router() external view returns(address);
}

interface ISwapERC20 {
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
}

interface ISwapPair is ISwapERC20 {

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

    function burnToken(address token,uint amount) external;
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

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

interface IWETH {
    function deposit() external payable;
    function transfer(address to, uint value) external returns (bool);
    function withdraw(uint) external;
}

interface ISwapRouter {
    function factory() external view returns(address);

    function WETH() external view returns(address);

    function creatorOf(address pair) external view returns(address);

    function baseTokenOf(address pair) external view returns(address);

    function sellUserRateOf(address pair) external view returns(uint);

    function sellLpRateOf(address pair) external view returns(uint);

    function sellOtherFeesLengthOf(address pair) external view returns(uint);

    function sellOtherFeeToOf(address pair,uint index) external view returns(address);

    function sellOtherFeeRateOf(address pair,uint index) external view returns(uint);

    function sellLpReceiverOf(address pair) external;

    function sellBurnRateOf(address pair) external view returns(uint);

    function sellStopBurnSupplyOf(address pair) external view returns(uint);

    function isWhiteList(address pair,address account) external view returns(bool);

    function setSellLpReceiver(address pair,address receiver) external;

    function setWhiteList(address pair,address account,bool status) external;

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external;

    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external;
}

interface IDeployStakingParams {
    struct DeployStakingParams {
        address factory;
        address creator;
        address pair;
        address stakingToken;
        address rewardToken;
        uint periodDuration;
        uint rewardRate;
        uint startTime;
    }
}

interface IStakingFactory is IDeployStakingParams {

    function deployParams() external view returns(DeployStakingParams memory paras);

    function WETH() external view returns(address);

    function factoryOwner() external view returns(address);

    // function isAdmin(address account) external view returns(address);

    function swapRouter() external view returns(address);

    function isRewardOperator(address account) external view returns(bool);

    function isRewardSigner(address account) external view returns(bool);
}

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

library SafeERC20 {
    using Address for address;

    function safeTransfer(
        IERC20 token,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(
        IERC20 token,
        address from,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    /**
     * @dev Deprecated. This function has issues similar to the ones found in
     * {IERC20-approve}, and its usage is discouraged.
     *
     * Whenever possible, use {safeIncreaseAllowance} and
     * {safeDecreaseAllowance} instead.
     */
    function safeApprove(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        require(
            (value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        uint256 newAllowance = token.allowance(address(this), spender) + value;
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        unchecked {
            uint256 oldAllowance = token.allowance(address(this), spender);
            require(oldAllowance >= value, "SafeERC20: decreased allowance below zero");
            uint256 newAllowance = oldAllowance - value;
            _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
        }
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
        if (returndata.length > 0) {
            // Return data is optional
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}

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

abstract contract Pausable is Context {
    /**
     * @dev Emitted when the pause is triggered by `account`.
     */
    event Paused(address account);

    /**
     * @dev Emitted when the pause is lifted by `account`.
     */
    event Unpaused(address account);

    bool private _paused;

    /**
     * @dev Initializes the contract in unpaused state.
     */
    constructor() {
        _paused = false;
    }

    /**
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        require(!paused(), "Pausable: paused");
        _;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is paused.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    modifier whenPaused() {
        require(paused(), "Pausable: not paused");
        _;
    }

    /**
     * @dev Triggers stopped state.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }

    /**
     * @dev Returns to normal state.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
    }
}

contract StakingRewards is Pausable,ReentrancyGuard,IDeployStakingParams {
    using SafeMath for uint256;
    using SafeERC20 for IERC20;

    uint private constant RATE_PERCISION = 10000;
    // address private constant ETHAddress = address(0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE);

    address private immutable factory;
    address private creator;
    address private immutable pair;
    address private immutable stakingToken;
    address private immutable rewardToken;
    uint private immutable periodDuration;
    uint private rewardRate;
    uint private immutable startTime;
    uint public aiex_pool_rate = 4000;
    uint public mpex_pool_rate = 4000;
    address public market_address = 0xbb464F20AC97c1A0dDF2fB483750218b02C06467;//TODO
    address public aiex_address = 0x54028359297b74De2913af18B73B3B1970336d82;
    address public swapRouterAddr = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    address public operator_addr = 0x30aAfcE1D1D84E90525CACD000537917BC11A4a2;

    bytes32 public constant DOMAIN_TYPEHASH = keccak256("EIP712Domain(string name,uint256 chainId,address verifyingContract)");
    bytes32 public constant WITHDRAW_REWARDS_PERMIT_TYPEHASH = keccak256("claim(address account,address token,uint256 amount,uint256 rand)");
    bytes32 public constant NOTIFY_REWARDS_PERMIT_TYPEHASH = keccak256("notifyRewards(uint256 epoch)");
    bytes32 public immutable DOMAIN_SEPARATOR;

    mapping(address => uint256) public stakedOf;
    mapping(uint256 => bool) public isUsedRand;
    mapping(uint256 => bool) public isNotifiedEpoch;

    event Staked(address user,uint amount,uint blockTime);
    event RewardPaid(address user, address token,uint256 reward,uint rand,uint blockTime);
    event SwapedRewards(uint removedLpAmount,uint remainLpAmount,uint rewardAmount, uint blockTime, uint rewardRate);

    modifier onlyCreator() {
        require(msg.sender == creator|| msg.sender == operator_addr,"caller must be creator");
        _;
    }

    constructor() {        
        DOMAIN_SEPARATOR = keccak256(abi.encode(DOMAIN_TYPEHASH, keccak256(bytes("StakingRewards")), block.chainid, address(this)));

        DeployStakingParams memory paras = IStakingFactory(msg.sender).deployParams();
        factory = paras.factory;
        creator = paras.creator;
        pair = paras.pair;
        stakingToken = paras.stakingToken;
        rewardToken = paras.rewardToken;
        periodDuration = paras.periodDuration;
        rewardRate = paras.rewardRate;
        startTime = paras.startTime;

        address router = IStakingFactory(msg.sender).swapRouter();
        IERC20(paras.stakingToken).safeApprove(router,type(uint256).max); 
        IERC20(paras.rewardToken).safeApprove(router,type(uint256).max);
        IERC20(paras.pair).safeApprove(router,type(uint256).max);
    }

    function stake(uint amount) external payable whenNotPaused nonReentrant {
        require(amount > 0,"amount can not be 0");
        require(block.timestamp >= startTime,"not start");
        uint receivedAmount = _transferFrom(msg.sender,stakingToken,amount);
        uint aiex_amount = receivedAmount.mul(aiex_pool_rate).div(RATE_PERCISION);
        uint mpex_amount = receivedAmount.mul(mpex_pool_rate).div(RATE_PERCISION);
        uint market_amount = receivedAmount.sub(aiex_amount).sub(mpex_amount);

        _addLiquidity(mpex_amount);
        _addAiexLiquidity(aiex_amount);
        IERC20(stakingToken).transferFrom(address(this),market_address, market_amount);
        stakedOf[msg.sender] = stakedOf[msg.sender].add(receivedAmount);

        emit Staked(msg.sender, receivedAmount, block.timestamp);
    }

    function claim(address account, uint amount,uint rand,uint8 v,bytes32 r,bytes32 s) external whenNotPaused nonReentrant {
        require(account != address(0),"account can not be address 0");
        require(amount > 0,"amount can not be 0");
        require(rand > 0,"rand can not be 0");
        require(stakedOf[account] > 0,"account not staked");
        require(account == msg.sender,"caller must be account");
        require(!isUsedRand[rand],"rand used");
        isUsedRand[rand] = true;
    
        address signatory = recoverWithdrawRewardsSign(account,rewardToken,amount,rand,v,r,s);
        require(signatory != address(0), "invalid signature");
        require(IStakingFactory(factory).isRewardSigner(signatory), "unauthorized");

        _transferTo(rewardToken,account,amount);

        emit RewardPaid(account,rewardToken, amount,rand,block.timestamp);
    }

    function recoverWithdrawRewardsSign(address account,address token, uint amount,uint rand,uint8 v,bytes32 r,bytes32 s) public view returns(address){
        bytes32 structHash = keccak256(abi.encode(WITHDRAW_REWARDS_PERMIT_TYPEHASH, account, token, amount, rand));
        bytes32 digest = keccak256(abi.encodePacked("\x19\x01", DOMAIN_SEPARATOR, structHash));
        address signatory = ecrecover(digest, v, r, s);

        return signatory;
    }

    function notifyRewards(uint epoch,uint8 v,bytes32 r,bytes32 s) external nonReentrant {
        require(_calcPeriodStart(block.timestamp,startTime).add(epoch.mul(periodDuration)) <= block.timestamp,"can not notify future rewards");
        require(!isNotifiedEpoch[epoch],"notified");
        isNotifiedEpoch[epoch] = true;
        address signer = recoverNotifyRewardsSign(epoch,v,r,s);
        require(IStakingFactory(factory).isRewardOperator(signer),"unauthorized");

        uint lpAmount = IERC20(pair).balanceOf(address(this)).mul(rewardRate) / RATE_PERCISION;
        uint balanceBefore = IERC20(rewardToken).balanceOf(address(this));
        _removeLiquidityForRewards(lpAmount);
        uint rewardAmount = IERC20(rewardToken).balanceOf(address(this)).sub(balanceBefore);

        emit SwapedRewards(lpAmount,IERC20(pair).balanceOf(address(this)),rewardAmount,block.timestamp,rewardRate);
    }

    function recoverNotifyRewardsSign(uint epoch,uint8 v,bytes32 r,bytes32 s) public view returns(address){
        bytes32 structHash = keccak256(abi.encode(NOTIFY_REWARDS_PERMIT_TYPEHASH, epoch));
        bytes32 digest = keccak256(abi.encodePacked("\x19\x01", DOMAIN_SEPARATOR, structHash));
        address signatory = ecrecover(digest, v, r, s);

        return signatory;
    }
    
    function _transferFrom(address from,address token,uint amount) internal returns(uint receivedAmount){
        address weth = IStakingFactory(factory).WETH();
        if(token == weth){
            require(msg.value >= amount,"insufficient input value");
            IWETH(weth).deposit{value : msg.value}();
            return msg.value;
        }

        uint beforeBalance = IERC20(token).balanceOf(address(this));
        IERC20(token).transferFrom(from, address(this), amount);
        return IERC20(token).balanceOf(address(this)).sub(beforeBalance);
    }

    function _transferTo(address token,address to,uint amount) internal {
        address weth = IStakingFactory(factory).WETH();
        if(token == weth){
            IWETH(weth).withdraw(amount);
            _safeTransferETH(to,amount);
        }else{
            IERC20(token).safeTransfer(to,amount);
        }
    }

    function _addLiquidity(uint stakeAmount) internal {
        if(stakeAmount>0){
            uint stakingTokenAmount = stakeAmount.div(2);
            ISwapRouter router = ISwapRouter(IStakingFactory(factory).swapRouter());
            address[] memory path = new address[](2);
            path[0] = stakingToken;
            path[1] = rewardToken;
            uint balanceBefore = IERC20(rewardToken).balanceOf(address(this));
            router.swapExactTokensForTokensSupportingFeeOnTransferTokens(stakingTokenAmount,1,path,address(this),type(uint256).max);
            uint swapedAmount = IERC20(rewardToken).balanceOf(address(this)).sub(balanceBefore);

            router.addLiquidity(stakingToken, rewardToken, stakingTokenAmount, swapedAmount, 1, 1, address(this), type(uint256).max);
        }
    }

    function _addAiexLiquidity(uint stakeAmount) internal {
        if(stakeAmount>0){
            uint stakingTokenAmount = stakeAmount.div(2);
            ISwapRouter router = ISwapRouter(swapRouterAddr);
            address[] memory path = new address[](2);
            path[0] = stakingToken;
            path[1] = aiex_address;
            uint balanceBefore = IERC20(aiex_address).balanceOf(address(this));
            router.swapExactTokensForTokensSupportingFeeOnTransferTokens(stakingTokenAmount,1,path,address(this),type(uint256).max);
            uint swapedAmount = IERC20(aiex_address).balanceOf(address(this)).sub(balanceBefore);

            router.addLiquidity(stakingToken, aiex_address, stakingTokenAmount, swapedAmount, 1, 1, address(this), type(uint256).max);
        }
    }

    function _removeLiquidityForRewards(uint lpAmount) internal {
        if(lpAmount == 0){
            return;
        }
        ISwapRouter router = ISwapRouter(IStakingFactory(factory).swapRouter());
        router.removeLiquidity(stakingToken, rewardToken, lpAmount, 1, 1, address(this), type(uint256).max);
        address[] memory path = new address[](2);
        path[0] = stakingToken;
        path[1] = rewardToken;
        router.swapExactTokensForTokensSupportingFeeOnTransferTokens(IERC20(path[0]).balanceOf(address(this)),1,path,address(this),type(uint256).max);
    }

    function _calcPeriodStart(uint _utcTime,uint _periodDuration) internal pure returns(uint){
        uint utcPeriodStart = _utcTime - (_utcTime % _periodDuration);

        return utcPeriodStart - 28800;
    }

    function _safeTransferETH(address to, uint value) internal {
        (bool success,) = to.call{value : value}(new bytes(0));
        require(success, 'TransferHelper: ETH_TRANSFER_FAILED');
    }

    function setRewardRate(uint _rate) external onlyCreator {
        require(_rate <= RATE_PERCISION,"rate too large");
        rewardRate = _rate;
    }

    function setAiexRate(uint _rate) external onlyCreator {
        require(_rate <= RATE_PERCISION,"rate too large");
        aiex_pool_rate = _rate;
    }

    function setMpexRate(uint _rate) external onlyCreator {
        require(_rate <= RATE_PERCISION,"rate too large");
        mpex_pool_rate = _rate;
    }

    function setAiexAddress(address addr) external onlyCreator {
        aiex_address = addr;
    }

    function setMarketAddress(address addr) external onlyCreator {
        market_address = addr;
    }

    function setRouterAddress(address addr) external onlyCreator {
        swapRouterAddr = addr;
    }

    function setOperatorAddress(address addr) external onlyCreator {
        operator_addr = addr;
    }

    function takeToken(address token,address to,uint amount) external onlyCreator {
        require(msg.sender == creator,"caller must be creator");
        if(token == address(0) || token == address(0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE)){
            _safeTransferETH(to,amount);
        }else{
            IERC20(token).safeTransfer(to,amount);
        }
    }

    function setPauseStatus(bool _paused) external {
        require(msg.sender == creator || msg.sender == IStakingFactory(factory).factoryOwner(),"caller must be creator");
        if(_paused){
            _pause();
        }else{
            _unpause();
        }
    }

    function transferCreator(address newCreator) external {
        require(newCreator != address(0),"new creator can not be address 0");
        require(msg.sender == creator || msg.sender == IStakingFactory(factory).factoryOwner(),"caller must be creator");
        creator = newCreator;
    }

    function infos() external view returns(
        address _factory,
        address _creator,
        address _pair,
        address _stakingToken,
        address _rewardToken,
        uint _periodDuration,
        uint _rewardRate,
        uint _startTime
    ){
        _factory = factory;
        _creator = creator;
        _pair = pair;
        _stakingToken = stakingToken;
        _rewardToken = rewardToken;
        _periodDuration = periodDuration;
        _rewardRate = rewardRate;
        _startTime = startTime;
    }
}

abstract contract CfoTakeableV2 is Ownable {
    using Address for address;
    using SafeERC20 for IERC20;

    address public cfo;

    modifier onlyCfoOrOwner {
        require(msg.sender == cfo || msg.sender == owner(),"onlyCfo: forbidden");
        _;
    }

    constructor(){
        cfo = msg.sender;
    }

    function takeToken(address token,address to,uint256 amount) public onlyCfoOrOwner {
        require(token != address(0),"invalid token");
        require(amount > 0,"amount can not be 0");
        require(to != address(0),"invalid to address");
        IERC20(token).safeTransfer(to, amount);
    }

    function takeETH(address to,uint256 amount) public onlyCfoOrOwner {
        require(amount > 0,"amount can not be 0");
        require(address(this).balance>=amount,"insufficient balance");
        require(to != address(0),"invalid to address");
        
        payable(to).transfer(amount);
    }

    function takeAllToken(address token, address to) public {
        uint balance = IERC20(token).balanceOf(address(this));
        if(balance > 0){
            takeToken(token, to, balance);
        }
    }

    function takeAllETH(address to) public {
        uint balance = address(this).balance;
        if(balance > 0){
            takeETH(to, balance);
        }
    }

    function setCfo(address _cfo) external onlyOwner {
        require(_cfo != address(0),"_cfo can not be address 0");
        cfo = _cfo;
    }
}

contract StakingFactory is CfoTakeableV2,ReentrancyGuard,IStakingFactory {
    using SafeERC20 for IERC20;

    uint private constant RATE_PERCISION = 10000;

    DeployStakingParams private tempDeployParams;

    address public override swapRouter;
    mapping(address => bool) public override isRewardOperator;
    mapping(address => bool) public override isRewardSigner;
    mapping(uint256 => address) public allStakings;
    uint public allStakingsLength;
    mapping(address => uint256) public stakingIndexOf;
    mapping(address => address) public pairStakingOf;
    address public operator_addr = 0x30aAfcE1D1D84E90525CACD000537917BC11A4a2;

    uint public createETHFee;

    event StakingCreated(address caller,address stakingPool,uint blockTime);

    constructor(
        address _swapRouter
    ){
        require(_swapRouter!=address(0),"swap router can not be address 0");
        swapRouter = _swapRouter;

        isRewardOperator[msg.sender] = true;
    }

    function create(
        address pair,
        uint periodDuration,
        uint rewardRate,
        uint initLpAmount,
        uint startTime
    ) external payable nonReentrant {
        require(msg.value >= createETHFee,"insufficient input value");
        require(pair != address(0),"invalid pair");
        require(pairStakingOf[pair] == address(0),"pair staking pool already existed");
        address stakingToken = ISwapRouter(swapRouter).baseTokenOf(pair);
        require(stakingToken != address(0),"invalid pair: 2");
        require(ISwapPair(pair).factory() == ISwapRouter(swapRouter).factory(),"invalid pair: 3");
        require(periodDuration >= 60 && periodDuration % 60 == 0,"invalid period duration");
        require(rewardRate <= RATE_PERCISION,"invalid reward rate");
        require(initLpAmount > 0,"init lp amount can not be 0");
        require(startTime >= block.timestamp,"invalid start time");
        require(ISwapRouter(swapRouter).creatorOf(pair) == msg.sender,"caller must be pair creator");

        address rewardToken;
        {
            (address t0,address t1) = (ISwapPair(pair).token0(),ISwapPair(pair).token1());
            rewardToken = stakingToken == t0 ? t1 : t0;
        }

        tempDeployParams.creator = msg.sender;
        tempDeployParams.factory = address(this);
        tempDeployParams.pair = pair;
        tempDeployParams.stakingToken = stakingToken;
        tempDeployParams.rewardToken = rewardToken;
        tempDeployParams.periodDuration = periodDuration;
        tempDeployParams.rewardRate = rewardRate;
        tempDeployParams.startTime = startTime;

        address stakingPool = address(new StakingRewards{
            salt: keccak256(abi.encode(stakingToken, rewardToken, allStakingsLength))
        }());
        IERC20(pair).safeTransferFrom(msg.sender,stakingPool,initLpAmount);
        delete tempDeployParams;

        pairStakingOf[pair] = stakingPool;
        ISwapRouter(swapRouter).setSellLpReceiver(pair,stakingPool);
        ISwapRouter(swapRouter).setWhiteList(pair,stakingPool,true);

        uint len = allStakingsLength;
        allStakings[len] = stakingPool;
        stakingIndexOf[stakingPool] = len;
        allStakingsLength = len + 1;

        emit StakingCreated(msg.sender,stakingPool,block.timestamp);
    }

    function factoryOwner() external view override returns(address){
        return owner();
    }

    function deployParams() external view override returns(DeployStakingParams memory){
        DeployStakingParams memory item = tempDeployParams;
        return item;
    }

    function WETH() external view override returns(address){
        return ISwapRouter(swapRouter).WETH();
    }

    function setRewardOperator(address account,bool status) external  {
        require(account!=address(0),"account can not be address 0");
        require(msg.sender==owner()||msg.sender==operator_addr,"no rights");
        isRewardOperator[account] = status;
    }

    function setRewardSigner(address account,bool status) external  {
        require(account!=address(0),"account can not be address 0");
        require(msg.sender==owner()||msg.sender==operator_addr,"no rights");
        isRewardSigner[account] = status;
    }

    function setCreateETHFee(uint fee) external  {
        require(msg.sender==owner()||msg.sender==operator_addr,"no rights");
        createETHFee = fee;        
    }

    function setSwapRouter(address _swapRouter) external  {
        require(msg.sender==owner()||msg.sender==operator_addr,"no rights");
        swapRouter = _swapRouter;
    }
}