// SPDX-License-Identifier: MIT
pragma solidity 0.8.4;

interface ISwapFactory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function setFeeTo(address) external;

    function initCodeHash() external view returns (bytes32);
    function feeTo() external view returns(address);
    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint index) external view returns (address pair);
    function allPairsLength() external view returns (uint);
    function createPair(address tokenA, address tokenB) external returns (address pair);

    function swapFee() external view returns (uint256);

    function sortTokens(address tokenA, address tokenB) external view returns (address token0, address token1);
    function pairFor(address tokenA, address tokenB) external view returns (address pair);
    function getReserves(address tokenA, address tokenB) external view returns (uint256 reserveA, uint256 reserveB);
    function quote(uint256 amountA, uint256 reserveA, uint256 reserveB) external view returns (uint256 amountB);
    function getAmountOut(uint256 amountIn, uint256 reserveIn, uint256 reserveOut) external view returns (uint256 amountOut);
    function getAmountIn(uint256 amountOut, uint256 reserveIn, uint256 reserveOut) external view returns (uint256 amountIn);
    function getAmountsOut(uint256 amountIn, address[] calldata path) external view returns (uint256[] memory amounts);
    function getAmountsIn(uint256 amountOut, address[] calldata path) external view returns (uint256[] memory amounts);
    function router() external view returns(address);
}

interface IStakeFactory {
    function rewardSigner(address) external view returns (bool);
    function router() external view returns(address);
}

interface IERC20 {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);
}

interface ISwapPair is IERC20 {
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
    function distributeToken(address token,address[] memory feeAddressList, uint256[] memory feeList) external;
}
// a library for performing overflow-safe math, courtesy of DappHub (https://github.com/dapphub/ds-math)
library SafeMath {
    uint256 constant WAD = 10 ** 18;
    uint256 constant RAY = 10 ** 27;

    function wad() public pure returns (uint256) {
        return WAD;
    }

    function ray() public pure returns (uint256) {
        return RAY;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

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

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }

    function min(uint256 a, uint256 b) internal pure returns (uint256) {
        return a <= b ? a : b;
    }

    function max(uint256 a, uint256 b) internal pure returns (uint256) {
        return a >= b ? a : b;
    }

    function sqrt(uint256 a) internal pure returns (uint256 b) {
        if (a > 3) {
            b = a;
            uint256 x = a / 2 + 1;
            while (x < b) {
                b = x;
                x = (a / x + x) / 2;
            }
        } else if (a != 0) {
            b = 1;
        }
    }

    function wmul(uint256 a, uint256 b) internal pure returns (uint256) {
        return mul(a, b) / WAD;
    }

    function wmulRound(uint256 a, uint256 b) internal pure returns (uint256) {
        return add(mul(a, b), WAD / 2) / WAD;
    }

    function rmul(uint256 a, uint256 b) internal pure returns (uint256) {
        return mul(a, b) / RAY;
    }

    function rmulRound(uint256 a, uint256 b) internal pure returns (uint256) {
        return add(mul(a, b), RAY / 2) / RAY;
    }

    function wdiv(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(mul(a, WAD), b);
    }

    function wdivRound(uint256 a, uint256 b) internal pure returns (uint256) {
        return add(mul(a, WAD), b / 2) / b;
    }

    function rdiv(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(mul(a, RAY), b);
    }

    function rdivRound(uint256 a, uint256 b) internal pure returns (uint256) {
        return add(mul(a, RAY), b / 2) / b;
    }

    function wpow(uint256 x, uint256 n) internal pure returns (uint256) {
        uint256 result = WAD;
        while (n > 0) {
            if (n % 2 != 0) {
                result = wmul(result, x);
            }
            x = wmul(x, x);
            n /= 2;
        }
        return result;
    }

    function rpow(uint256 x, uint256 n) internal pure returns (uint256) {
        uint256 result = RAY;
        while (n > 0) {
            if (n % 2 != 0) {
                result = rmul(result, x);
            }
            x = rmul(x, x);
            n /= 2;
        }
        return result;
    }
}

interface IStakePool {
    function initialize(address _holder, address _lpAddr, uint256 _period, uint256 _ref, uint256 _limit, uint256 _start) external;
    function transferInitHolder(address _newHolder) external;
    function setDailyRewardHour(uint256 _hour) external;
}

interface ISwapRouter {
    function factory() external view returns(address);
    function baseTokenOf(address pair) external view returns (address base);
    function setWhiteList(address pair,address account,bool status) external;
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
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
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
    function getAmountOut(uint256 amountIn, uint256 reserveIn, uint256 reserveOut) external view returns (uint256 amountOut);
    function getAmountIn(uint256 amountOut, uint256 reserveIn, uint256 reserveOut) external view returns (uint256 amountIn);
    function getAmountsOut(uint256 amountIn, address[] memory path) external view returns (uint256[] memory amounts);
    function getAmountsIn(uint256 amountOut, address[] memory path) external view returns (uint256[] memory amounts);
    function quote(uint256 amountA, uint256 reserveA, uint256 reserveB) external view returns (uint256 amountB);
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

// File: @openzeppelin/contracts/GSN/Context.sol
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

// File: @openzeppelin/contracts/ownership/Ownable.sol
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

// File: contracts/swap-libs/SafeERC20.sol
/**
 * @title SafeERC20
 * @dev Wrappers around ERC20 operations that throw on failure (when the token
 * contract returns false). Tokens that return no value (and instead revert or
 * throw on failure) are also supported, non-reverting calls are assumed to be
 * successful.
 * To use this library you can add a `using SafeERC20 for IERC20;` statement to your contract,
 * which allows you to call the safe operations as `token.safeTransfer(...)`, etc.
 */
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

// File: contracts/swap-libs/CfoTakeableV2.sol
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

contract ECDSA {

   function splitSignature(bytes memory sig)
        internal
        pure
        returns (
            uint8,
            bytes32,
            bytes32
        )
    {
        require(sig.length == 65);

        bytes32 r;
        bytes32 s;
        uint8 v;

        assembly {
            // first 32 bytes, after the length prefix
            r := mload(add(sig, 32))
            // second 32 bytes
            s := mload(add(sig, 64))
            // final byte (first byte of the next 32 bytes)
            v := byte(0, mload(add(sig, 96)))
        }
        return (v, r, s);
    }

    function recoverSigner(bytes32 message, bytes memory sig)
        internal
        pure
        returns (address)
    {
        uint8 v;
        bytes32 r;
        bytes32 s;
        (v, r, s) = splitSignature(sig);
        return ecrecover(message, v, r, s);
    }
}

contract StakePool is ECDSA {
    using SafeMath for uint;
    using SafeMath for uint256;
    using SafeERC20 for IERC20;

    uint256 public constant FEE_RATE_BASE = 10000;

    address public factory;
    address public stakeFactory;
    address public router;
    uint256 public period;
    address public lpAddress;
    uint256 public releaseRatio;
    uint256 private initHolderAmount;
    address private initHolderAddress;
    uint256 public starttime;
    uint256 public lpReleaseTime;
    uint256 public dailyRewardHour;
    uint256 public nextReleaseTime;
    uint256 public stakeNodeRatio;
    uint256 public stakeFundRatio;
    uint256 public stakeCommRatio;
    address public fundAddress;
    address public commAddress;
    uint256 public releaseOperRatio;
    uint256 public releaseFundRatio;
    uint256 public releaseCommRatio;
    address public fundAddr2;
    address public commAddr2;
    address public operAddr;
    address public baseToken;
    address public otherToken;
    address private _owner;
    bool public flag;
    mapping(address => uint256) private nonce;
    mapping(bytes32 => bool) private orders;

    uint private unlocked = 1;
    modifier lock() {
        require(unlocked == 1, 'Pool: LOCKED');
        unlocked = 0;
        _;
        unlocked = 1;
    }

    event Claim(address user, address token, uint amount, uint rType, uint timeout, uint time);
    event Stake(address user, uint amount, uint time);
    event Release(uint amount, uint time);

    modifier onlyOwner() {
        require(_owner == msg.sender, "Pool: caller is not the owner");
        _;
    }

    constructor() {
        stakeFactory = msg.sender;
    }

    // called once by the factory at time of deployment
    function initialize(address _holder, address _lpAddr, uint256 _period, uint256 _ref, uint256 _limit, uint256 _start) external {
        require(msg.sender == stakeFactory, 'Pool: FORBIDDEN'); // sufficient check
        initHolderAddress = _holder;
        _owner = _holder;
        lpAddress = _lpAddr;
        period = _period;
        releaseRatio = _ref;
        initHolderAmount = _limit;
        starttime = _start;
        lpReleaseTime = block.timestamp;
        nextReleaseTime = computeNextReleaseTime(_start);
        dailyRewardHour = 10;
        router = IStakeFactory(msg.sender).router();
        factory = ISwapRouter(router).factory();
        IERC20(ISwapPair(_lpAddr).token0()).safeApprove(router, ~uint(0));
        IERC20(ISwapPair(_lpAddr).token1()).safeApprove(router, ~uint(0));
        IERC20(_lpAddr).safeApprove(router, ~uint(0));
        baseToken = ISwapRouter(router).baseTokenOf(_lpAddr);
        otherToken = ISwapPair(_lpAddr).token0() == baseToken ? ISwapPair(_lpAddr).token1() : ISwapPair(_lpAddr).token0();
    }

    function computeNextReleaseTime(uint256 timestamp) public view returns (uint256) {
        uint256 todayAM = (timestamp / period) * period - 8 * 60 * 60 + dailyRewardHour * 60 * 60;
        if (timestamp < todayAM) {
            return todayAM;
        } else {
            return todayAM + period;
        }
    }

    function getOwner()public view returns(address){
        return _owner;
    }

    function setDailyRewardHour(uint256 _hour) public {
        require(msg.sender == stakeFactory, 'Pool: FORBIDDEN'); // sufficient check
        dailyRewardHour = _hour;
    }

    function takeInitLp() public {
        require(msg.sender == initHolderAddress, "Pool: only init holder can take init lp");
        require(initHolderAmount > 0, "Pool: init lp token has been token");
        require(lpReleaseTime <= block.timestamp, "Pool:  Not yet time to release");
        if(IERC20(lpAddress).balanceOf(address(this)) < initHolderAmount) {
            IERC20(lpAddress).safeTransfer(msg.sender, IERC20(lpAddress).balanceOf(address(this)));
        } else {
            IERC20(lpAddress).safeTransfer(msg.sender, initHolderAmount);
        }
        initHolderAmount = 0;
    }

    function addLock(uint _time) public onlyOwner {
        require(!flag, "have been locked");
        lpReleaseTime = lpReleaseTime + _time;
        flag = true;
    }

    function takeToken(address token, address to, uint256 amount) public onlyOwner {
        require(token == baseToken, "Pool: only base token can be taken");
        IERC20(token).safeTransfer(to, amount);
    }

    function setFeeInfo(address _fund, address _community, uint256 _fundFee, uint256 _commFee, uint256 _nodeFee) public onlyOwner {
        fundAddress = _fund;
        stakeFundRatio = _fundFee;
        commAddress = _community;
        stakeCommRatio = _commFee;
        stakeNodeRatio = _nodeFee;
    }

    function setStartTime(uint256 _start) public onlyOwner {
        starttime = _start;
        nextReleaseTime = computeNextReleaseTime(_start);
    }

    function setFeeInfo2(address _fund, address _comm, address _operate, uint256 _fundFee, uint256 _commFee, uint256 _operateFee) public onlyOwner {
        fundAddr2 = _fund;
        releaseFundRatio = _fundFee;
        commAddr2 = _comm;
        releaseCommRatio = _commFee;
        operAddr = _operate;
        releaseOperRatio = _operateFee;
    }

    function transferOwnership(address _newOwner) public {
        require(_owner == msg.sender, "Pool: only owner can transfer ownership");
        _owner = _newOwner;
    }

    function transferInitHolder(address _newHolder) public {
        require(msg.sender == stakeFactory, 'Pool: FORBIDDEN'); // sufficient check
        initHolderAddress = _newHolder;
    }

    function removeLiqRelease() external lock {
        require(block.timestamp >= nextReleaseTime, "Pool: Not yet time to release");
        require(ISwapPair(lpAddress).balanceOf(address(this)) > 0, "Pool: No tokens to release");
        uint initBalance = IERC20(otherToken).balanceOf(address(this));
        uint amountToRelease = ISwapPair(lpAddress).balanceOf(address(this)).mul(releaseRatio).div(FEE_RATE_BASE);
        address tokenA = ISwapPair(lpAddress).token0();
        address tokenB = ISwapPair(lpAddress).token1();
        (uint amountA, uint amountB) = ISwapRouter(router).removeLiquidity(
            tokenA, tokenB, amountToRelease, 0, 0, address(this), block.timestamp + 300);

        (address token0,) = ISwapFactory(factory).sortTokens(tokenA, tokenB);
        (uint amount0, uint amount1) = tokenA == token0 ? (amountA, amountB) : (amountB, amountA);
        // 买币
        uint usdtAmount = tokenA == ISwapRouter(router).baseTokenOf(lpAddress) ? amount0 : amount1;
        address[] memory path = new address[](2);
        path[0] = baseToken;
        path[1] = otherToken;
        ISwapRouter(router).swapExactTokensForTokensSupportingFeeOnTransferTokens(
            usdtAmount, 0, path, address(this), block.timestamp + 300);
        nextReleaseTime = computeNextReleaseTime(block.timestamp);
        uint addBlance = IERC20(otherToken).balanceOf(address(this)).sub(initBalance);
        if(releaseFundRatio > 0) {
            IERC20(otherToken).safeTransfer(fundAddr2, addBlance.mul(releaseFundRatio).div(FEE_RATE_BASE));
        }
        if(releaseCommRatio > 0) {
            IERC20(otherToken).safeTransfer(commAddr2, addBlance.mul(releaseCommRatio).div(FEE_RATE_BASE));
        }
        if(releaseOperRatio > 0) {
            IERC20(otherToken).safeTransfer(operAddr, addBlance.mul(releaseOperRatio).div(FEE_RATE_BASE));
        }
        emit Release(addBlance, block.timestamp);
    }

    function stake(uint256 amount) external lock {
        require(amount >= 100e18, 'Pool: stake amount must be greater than 100');
        require(block.timestamp >= starttime, 'Pool: NOT START');
        uint nodeAmount;
        if(stakeNodeRatio > 0) {
            nodeAmount = amount.mul(stakeNodeRatio).div(FEE_RATE_BASE);
            IERC20(baseToken).safeTransferFrom(msg.sender, address(this), nodeAmount);
        }
        uint fundAmount;
        if(stakeFundRatio > 0) {
            fundAmount = amount.mul(stakeFundRatio).div(FEE_RATE_BASE);
            IERC20(baseToken).safeTransferFrom(msg.sender, fundAddress, fundAmount);
        }
        uint commAmount;
        if(stakeCommRatio > 0) {
            commAmount = amount.mul(stakeCommRatio).div(FEE_RATE_BASE);
            IERC20(baseToken).safeTransferFrom(msg.sender, commAddress, commAmount);
        }
        uint256 lpAmount = amount.sub(nodeAmount).sub(fundAmount).sub(commAmount);
        IERC20(baseToken).safeTransferFrom(msg.sender, address(this), lpAmount);
        
        uint256 usdtAmount = lpAmount.div(2);
        address[] memory path = new address[](2);
        path[0] = baseToken;
        path[1] = otherToken;

        uint256 initialBalance = IERC20(otherToken).balanceOf(address(this));
        ISwapRouter(router).swapExactTokensForTokensSupportingFeeOnTransferTokens(
            usdtAmount, 0, path, address(this), block.timestamp + 300);
        uint256 newBalance = IERC20(otherToken).balanceOf(address(this)).sub(initialBalance);
        ISwapRouter(router).addLiquidity(
            baseToken, otherToken, usdtAmount, newBalance, 0, 0, address(this), block.timestamp + 300);
        emit Stake(msg.sender, amount, block.timestamp);
    }

    function claim(uint256 _amount, uint256 _rType, uint256 _timeout, bytes memory signature) external lock {
        require(block.timestamp >= starttime, 'Pool: NOT START');
        require(_amount > 0, 'Pool: claim amount must be greater than zero');
        
        require(_timeout >= block.timestamp, 'Pool: timeout');
        uint256 nonce_ = ++nonce[msg.sender];
        // 验证签名
        bytes32 hash = keccak256(abi.encodePacked(
            msg.sender, _amount, _rType, _timeout, nonce_
        ));
        require(!orders[hash], "Pool: hash expired");
        require(IStakeFactory(stakeFactory).rewardSigner(recoverSigner(hash, signature)), "Pool: sign error");
        orders[hash] = true;
        if(_rType == 2) {
            require(_amount <= IERC20(baseToken).balanceOf(address(this)), 'Pool: claim amount must be less than balance');
            IERC20(baseToken).safeTransfer(msg.sender, _amount);
            emit Claim(msg.sender, baseToken, _amount, _rType, _timeout, block.timestamp);
        } else {
            require(_amount <= IERC20(otherToken).balanceOf(address(this)), 'Pool: claim amount must be less than balance');
            IERC20(otherToken).safeTransfer(msg.sender, _amount);
            emit Claim(msg.sender, otherToken, _amount, _rType, _timeout, block.timestamp);
        }
    }
}

contract StakeFactory is IStakeFactory,Ownable {
    address private router_; 

    mapping(address=>address) public getStakePool;
    mapping(address=>bool) isRewardSigner;

    constructor(address _router){
        router_ = _router;
    }

    function createStakePool(address lpAddr,address _holder,uint256 _period,uint256 _ref, uint256 _limit, uint256 _start)public onlyOwner returns(address pool){
    require(getStakePool[lpAddr] == address(0), 'StakePoolFactory: StakePool_EXISTS'); // single check is sufficient   
    bytes memory bytecode = type(StakePool).creationCode;
        bytes32 salt = keccak256(abi.encodePacked(lpAddr));
        assembly {
            pool := create2(0, add(bytecode, 32), mload(bytecode), salt)
        }
        IStakePool(pool).initialize(_holder,lpAddr,_period,_ref,_limit,_start);
        IStakePool(pool).setDailyRewardHour(8);
        IStakePool(pool).transferInitHolder(msg.sender);
        getStakePool[lpAddr] = pool;
        isRewardSigner[msg.sender] =true;
        return pool;
    }

    function setDailyRewardHour(address lp,uint hour)public onlyOwner{
        require(getStakePool[lp] != address(0), 'StakePoolFactory: StakePool_EXISTS'); // single check is sufficient   
        IStakePool(getStakePool[lp]).setDailyRewardHour(hour);
    }
   function transferInitHolder(address lp,address holder)public onlyOwner{
        require(getStakePool[lp] != address(0), 'StakePoolFactory: StakePool_EXISTS'); // single check is sufficient   
        IStakePool(getStakePool[lp]).transferInitHolder(holder);
    }

     function rewardSigner(address account)  external  view override returns (bool){
         return isRewardSigner[account];
     }

    function router() external view override returns(address){
        return router_;
    }


}