// SPDX-License-Identifier: MIT
pragma solidity 0.8.4;

interface IWETH {
    function deposit() external payable;
    function transfer(address to, uint value) external returns (bool);
    function withdraw(uint) external;
}

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

library TransferHelper {
    function safeApprove(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('approve(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x095ea7b3, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: APPROVE_FAILED');
    }

    function safeTransfer(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FAILED');
    }

    function safeTransferFrom(address token, address from, address to, uint value) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }

    function safeTransferETH(address to, uint value) internal {
        (bool success,) = to.call{value : value}(new bytes(0));
        require(success, 'TransferHelper: ETH_TRANSFER_FAILED');
    }
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

contract SwapRouter is Ownable {
    using SafeMath for uint256;

    address public immutable factory;
    address public immutable WETH;

    uint private constant RATE_PERCISION = 10000;
    
    mapping(address => address) public creatorOf;
    mapping(address => address) public baseTokenOf;
    mapping(address => uint256) public sellUserRateOf;
    mapping(address => uint256) public sellLpRateOf;
    mapping(address => mapping(uint256 => address)) public sellOtherFeeToOf;
    mapping(address => mapping(uint256 => uint256)) public sellOtherFeeRateOf;
    mapping(address => uint256) public sellOtherFeesLengthOf;
    mapping(address => address) public sellLpReceiverOf;
    mapping(address => uint256) public sellBurnRateOf;
    mapping(address => uint256) public sellStopBurnSupplyOf;
    mapping(address => mapping(address => bool)) public isWhiteList;
    uint private sell_market_rate = 100;
    address public sell_market_addr = 0x74e181E512883806aD8B2425beFf4A8b64A2Fa65;
    address public stakingFactory;
    uint buy_limit=1;
    uint sell_limit=1;
    uint public remain_coin = 1*1e15;

    modifier ensure(uint deadline) {
        require(deadline >= block.timestamp, 'SwapRouter: EXPIRED');
        _;
    }

    modifier checkSwapPath(address[] calldata path){
        require(path.length == 2,"path length err");

        address pair = pairFor(path[0],path[1]);
        address baseToken = baseTokenOf[pairFor(path[0],path[1])];
        require(baseToken != address(0),"pair of path not found");
        if(buy_limit==1){
            require(path[0] != baseToken || isWhiteList[pair][msg.sender],"buy disabled");
            _;
        }
    }

    modifier onlyCreator(address pair){
        require(pair != address(0),"pair can not be address 0");
        require(msg.sender == creatorOf[pair] || msg.sender==owner(),"caller must be creator");
        _;
    }

    event NewPairCreated(address caller,address pair,uint blockTime);
    event SellLpFeeAdded(address caller,address pair,uint addedLpBaseTokenAmount,uint blockTime);
    event PairConfigChanged(address caller,uint blockTime);
    event WhiteListChanged(address pair,address user,bool status);

    struct CreatePairParams {
        address tokenA;
        address tokenB;
        address baseToken;
        uint amountA;
        uint amountB;
        // address to;
        uint sellUserRate;
        uint sellLpRate;
        uint sellBurnRate;
        uint sellStopBurnSupply;
    }

    constructor(address _factory) {
        factory = _factory;
        WETH = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
    }

    receive() external payable {
        assert(msg.sender == WETH);
        // only accept ETH via fallback from the WETH contract
    }

    function pairFor(address tokenA, address tokenB) public view returns (address pair) {
        pair = ISwapFactory(factory).pairFor(tokenA, tokenB);
    }

    function createPair(CreatePairParams calldata paras,address[] calldata otherFeeTos, uint[] calldata otherFeeRates) external {
        require(paras.baseToken == paras.tokenA || paras.baseToken == paras.tokenB,"invalid base token");
        require(ISwapFactory(factory).getPair(paras.tokenA, paras.tokenB) == address(0),"pair existed");
        require(paras.sellBurnRate <= RATE_PERCISION,"sell burn token rate too big");
        // require(paras.to != address(0),"to can not be address 0");
        require(paras.amountA > 0 && paras.amountB > 0,"invalid amountA or amountB");

        address pair = ISwapFactory(factory).createPair(paras.tokenA, paras.tokenB);
        TransferHelper.safeTransferFrom(paras.tokenA, msg.sender, pair, paras.amountA);
        TransferHelper.safeTransferFrom(paras.tokenB, msg.sender, pair, paras.amountB);
        ISwapPair(pair).mint(msg.sender);

        creatorOf[pair] = msg.sender;
        baseTokenOf[pair] = paras.baseToken;
        sellLpReceiverOf[pair] = msg.sender;

        _setPairConfigs(pair,paras.sellBurnRate,paras.sellStopBurnSupply,paras.sellUserRate,paras.sellLpRate,otherFeeTos,otherFeeRates);

        isWhiteList[pair][msg.sender] = true;
        isWhiteList[pair][owner()] = true;

        emit NewPairCreated(msg.sender, pair, block.timestamp);
    }


    function _setPairConfigs(
        address pair,
        uint sellBurnRate,
        uint sellStopBurnSupply,
        uint sellUserRate,
        uint sellLpRate,
        address[] calldata otherFeeTos,
        uint[] calldata otherFeeRates
    ) internal {
        require(sellBurnRate <= RATE_PERCISION,"sell burn token rate too big");
        require(otherFeeTos.length == otherFeeRates.length && otherFeeTos.length <= 50,"otherFeeRates length err");
        
        sellBurnRateOf[pair] = sellBurnRate;
        sellStopBurnSupplyOf[pair] = sellStopBurnSupply;

        uint totalRate = sellUserRate.add(sellLpRate);
        require(totalRate <= RATE_PERCISION,"sum of rates too big");
        sellUserRateOf[pair] = sellUserRate;
        sellLpRateOf[pair] = sellLpRate;
        for(uint i=0;i<otherFeeTos.length;i++){
            require(otherFeeTos[i]!=address(0),"otherFeeAccount can not be address 0");
            totalRate = totalRate.add(otherFeeRates[i]);
            require(totalRate <= RATE_PERCISION,"sum of rates too big");

            sellOtherFeeToOf[pair][i] = otherFeeTos[i];
            sellOtherFeeRateOf[pair][i] = otherFeeRates[i];
        }
        uint oldLen = sellOtherFeesLengthOf[pair];
        sellOtherFeesLengthOf[pair] = otherFeeTos.length;

        for(uint i=otherFeeTos.length;i<oldLen;i++){
            delete sellOtherFeeToOf[pair][i];
            delete sellOtherFeeRateOf[pair][i];
        }
    }

    // **** ADD LIQUIDITY ****
    function _addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin
    ) internal view returns (uint amountA, uint amountB) {
        // create the pair if it doesn't exist yet
        // if (ISwapFactory(factory).getPair(tokenA, tokenB) == address(0)) {
        //     ISwapFactory(factory).createPair(tokenA, tokenB);
        // }
        require(ISwapFactory(factory).getPair(tokenA, tokenB) != address(0),"pair not exists");
        
        (uint reserveA, uint reserveB) = ISwapFactory(factory).getReserves(tokenA, tokenB);
        if (reserveA == 0 && reserveB == 0) {
            (amountA, amountB) = (amountADesired, amountBDesired);
        } else {
            uint amountBOptimal = ISwapFactory(factory).quote(amountADesired, reserveA, reserveB);
            if (amountBOptimal <= amountBDesired) {
                require(amountBOptimal >= amountBMin, 'SwapRouter: INSUFFICIENT_B_AMOUNT');
                (amountA, amountB) = (amountADesired, amountBOptimal);
            } else {
                uint amountAOptimal = ISwapFactory(factory).quote(amountBDesired, reserveB, reserveA);
                assert(amountAOptimal <= amountADesired);
                require(amountAOptimal >= amountAMin, 'SwapRouter: INSUFFICIENT_A_AMOUNT');
                (amountA, amountB) = (amountAOptimal, amountBDesired);
            }
        }
    }

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external ensure(deadline) returns (uint amountA, uint amountB, uint liquidity) {
        (amountA, amountB) = _addLiquidity(tokenA, tokenB, amountADesired, amountBDesired, amountAMin, amountBMin);
        address pair = pairFor(tokenA, tokenB);
        TransferHelper.safeTransferFrom(tokenA, msg.sender, pair, amountA);
        TransferHelper.safeTransferFrom(tokenB, msg.sender, pair, amountB);
        liquidity = ISwapPair(pair).mint(to);
    }

    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable ensure(deadline) returns (uint amountToken, uint amountETH, uint liquidity) {
        (amountToken, amountETH) = _addLiquidity(
            token,
            WETH,
            amountTokenDesired,
            msg.value,
            amountTokenMin,
            amountETHMin
        );
        address pair = pairFor(token, WETH);
        TransferHelper.safeTransferFrom(token, msg.sender, pair, amountToken);
        IWETH(WETH).deposit{value : amountETH}();
        assert(IWETH(WETH).transfer(pair, amountETH));
        liquidity = ISwapPair(pair).mint(to);
        // refund dust eth, if any
        if (msg.value > amountETH) TransferHelper.safeTransferETH(msg.sender, msg.value - amountETH);
    }

    // **** REMOVE LIQUIDITY ****
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) public ensure(deadline) returns (uint amountA, uint amountB) {
        require(ISwapFactory(factory).getPair(tokenA, tokenB) != address(0),"pair not exists");

        address pair = pairFor(tokenA, tokenB);
        ISwapPair(pair).transferFrom(msg.sender, pair, liquidity);
        // send liquidity to pair
        (uint amount0, uint amount1) = ISwapPair(pair).burn(to);
        (address token0,) = ISwapFactory(factory).sortTokens(tokenA, tokenB);
        (amountA, amountB) = tokenA == token0 ? (amount0, amount1) : (amount1, amount0);
        require(amountA >= amountAMin, 'SwapRouter: INSUFFICIENT_A_AMOUNT');
        require(amountB >= amountBMin, 'SwapRouter: INSUFFICIENT_B_AMOUNT');
    }

    // **** REMOVE LIQUIDITY (supporting fee-on-transfer tokens) ****
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) public  ensure(deadline) returns (uint amountETH) {
        (, amountETH) = removeLiquidity(
            token,
            WETH,
            liquidity,
            amountTokenMin,
            amountETHMin,
            address(this),
            deadline
        );
        TransferHelper.safeTransfer(token, to, IERC20(token).balanceOf(address(this)));
        IWETH(WETH).withdraw(amountETH);
        TransferHelper.safeTransferETH(to, amountETH);
    }

    // **** SWAP ****
    // requires the initial amount to have already been sent to the first pair
    function _swap(uint[] memory amounts, address[] memory path, address _to) internal {
        for (uint i; i < path.length - 1; i++) {
            (address input, address output) = (path[i], path[i + 1]);
            (address token0,) = ISwapFactory(factory).sortTokens(input, output);
            uint amountOut = amounts[i + 1];
            (uint amount0Out, uint amount1Out) = input == token0 ? (uint(0), amountOut) : (amountOut, uint(0));
            address to = i < path.length - 2 ? pairFor(output, path[i + 2]) : _to;
            ISwapPair(pairFor(input, output)).swap(
                amount0Out, amount1Out, to, new bytes(0)
            );
        }
    }

    // **** SWAP (supporting fee-on-transfer tokens) ****
    // requires the initial amount to have already been sent to the first pair
    function _swapSupportingFeeOnTransferTokens(address[] memory path, address _to) internal returns(uint) {
        (address input, address output) = (path[0], path[1]);
        (address token0,) = ISwapFactory(factory).sortTokens(input, output);
        ISwapPair pair = ISwapPair(pairFor(input, output));
        uint amountInput;
        uint amountOutput;
        {// scope to avoid stack too deep errors
            (uint reserve0, uint reserve1,) = pair.getReserves();
            (uint reserveInput, uint reserveOutput) = input == token0 ? (reserve0, reserve1) : (reserve1, reserve0);
            amountInput = IERC20(input).balanceOf(address(pair)).sub(reserveInput);
            amountOutput = ISwapFactory(factory).getAmountOut(amountInput, reserveInput, reserveOutput, input, output);
        }

        (uint amount0Out, uint amount1Out) = input == token0 ? (uint(0), amountOutput) : (amountOutput, uint(0));
        // address to = i < path.length - 2 ? pairFor(output, path[i + 2]) : _to;
        pair.swap(amount0Out, amount1Out, _to, new bytes(0));

        return amountInput;
    }

    function _isBuy(address[] calldata path) internal view returns(bool) {
        return path[0] == baseTokenOf[pairFor(path[0],path[1])] ? true : false;
    }

    struct SwapTempVals {
        bool isBuy;
        address swapTo;
        uint balanceBefore;
        uint amountInput;
        uint amountOut;
    }

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external ensure(deadline) checkSwapPath(path) {
        SwapTempVals memory tempVals;
        tempVals.isBuy = _isBuy(path);
        tempVals.swapTo = tempVals.isBuy ? to : address(this);
        if(!tempVals.isBuy&&sell_limit==1){
            uint m_balance = IERC20(path[0]).balanceOf(msg.sender);
            uint left_balance = m_balance.sub(amountIn);
            require(left_balance>=remain_coin,"need to remain enough coin");
        }

        TransferHelper.safeTransferFrom(path[0], msg.sender, pairFor(path[0], path[1]), amountIn);
        tempVals.balanceBefore = IERC20(path[path.length - 1]).balanceOf(tempVals.swapTo);
        tempVals.amountInput = _swapSupportingFeeOnTransferTokens(path, tempVals.swapTo);
        tempVals.amountOut = IERC20(path[path.length - 1]).balanceOf(tempVals.swapTo).sub(tempVals.balanceBefore);
        require(tempVals.amountOut >= amountOutMin,'SwapRouter: INSUFFICIENT_OUTPUT_AMOUNT');

        if(!tempVals.isBuy){
            _burnPairToken(path,tempVals.amountInput);
            
            if(isWhiteList[pairFor(path[0], path[1])][msg.sender]){
                _distributeTo(path[1], to, tempVals.amountOut);
            }else{
                uint amoutOut = tempVals.amountOut;
                uint sell_market_amount = amoutOut.mul(sell_market_rate).div(RATE_PERCISION);
                uint left_amount = amoutOut.sub(sell_market_amount);
                _distributeTo(path[1],sell_market_addr,sell_market_amount);

                _distributeForSell(path,left_amount,to);
                _addLiquidityForSell(path,left_amount);
            }
        }        
    }

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable ensure(deadline) checkSwapPath(path) {
        require(path[0] == WETH, 'SwapRouter: INVALID_PATH');

        SwapTempVals memory tempVals;
        tempVals.isBuy = _isBuy(path);
        tempVals.swapTo = tempVals.isBuy ? to : address(this);
        uint amountIn = msg.value;
        if(!tempVals.isBuy&&sell_limit==1){
            uint m_balance = IERC20(path[0]).balanceOf(msg.sender);
            uint left_balance = m_balance.sub(amountIn);
            require(left_balance>=remain_coin,"need to remain enough coin");
        }

        IWETH(WETH).deposit{value : amountIn}();
        assert(IWETH(WETH).transfer(pairFor(path[0], path[1]), amountIn));

        tempVals.balanceBefore = IERC20(path[path.length - 1]).balanceOf(tempVals.swapTo);
        tempVals.amountInput = _swapSupportingFeeOnTransferTokens(path, tempVals.swapTo);
        tempVals.amountOut = IERC20(path[path.length - 1]).balanceOf(tempVals.swapTo).sub(tempVals.balanceBefore);
        require(tempVals.amountOut >= amountOutMin,'SwapRouter: INSUFFICIENT_OUTPUT_AMOUNT');

        if(!tempVals.isBuy){
            _burnPairToken(path,tempVals.amountInput);

            if(isWhiteList[pairFor(path[0], path[1])][msg.sender]){
                _distributeTo(path[1], to, tempVals.amountOut);
            }else{
                uint amoutOut = tempVals.amountOut;
                uint sell_market_amount = amoutOut.mul(sell_market_rate).div(RATE_PERCISION);
                uint left_amount = amoutOut.sub(sell_market_amount);
                _distributeTo(path[1],sell_market_addr,sell_market_amount);

                _distributeForSell(path,left_amount,to);
                _addLiquidityForSell(path,left_amount);
            }
        }
    }

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external ensure(deadline) checkSwapPath(path) {
        require(path[path.length - 1] == WETH, 'SwapRouter: INVALID_PATH');

        SwapTempVals memory tempVals;
        tempVals.isBuy = _isBuy(path);
        tempVals.swapTo = tempVals.isBuy ? to : address(this);
        if(!tempVals.isBuy&&sell_limit==1){
            uint m_balance = IERC20(path[0]).balanceOf(msg.sender);
            uint left_balance = m_balance.sub(amountIn);
            require(left_balance>=remain_coin,"need to remain enough coin");
        }

        TransferHelper.safeTransferFrom(path[0], msg.sender, pairFor(path[0], path[1]), amountIn);
        tempVals.balanceBefore = IERC20(path[path.length - 1]).balanceOf(address(this));
        tempVals.amountInput = _swapSupportingFeeOnTransferTokens(path, address(this));
        tempVals.amountOut = IERC20(path[path.length - 1]).balanceOf(address(this)).sub(tempVals.balanceBefore);
        require(tempVals.amountOut >= amountOutMin, 'SwapRouter: INSUFFICIENT_OUTPUT_AMOUNT');
        // IWETH(WETH).withdraw(tempVals.amountOut);

        if(!tempVals.isBuy){
            _burnPairToken(path,tempVals.amountInput);

            if(isWhiteList[pairFor(path[0], path[1])][msg.sender]){
                _distributeTo(path[1], to, tempVals.amountOut);
            }else{
                uint amoutOut = tempVals.amountOut;
                uint sell_market_amount = amoutOut.mul(sell_market_rate).div(RATE_PERCISION);
                uint left_amount = amoutOut.sub(sell_market_amount);
                _distributeTo(path[1],sell_market_addr,sell_market_amount);

                _distributeForSell(path,left_amount,to);
                _addLiquidityForSell(path,left_amount);
            }
        }else{
            IWETH(WETH).withdraw(tempVals.amountOut);
            TransferHelper.safeTransferETH(to, tempVals.amountOut);
        }
    }

    function _burnPairToken(address[] calldata path,uint amountInput) internal {
        address pair = pairFor(path[0], path[1]);

        uint burnedAmount = IERC20(path[0]).balanceOf(address(0));
        burnedAmount = burnedAmount.add(IERC20(path[0]).balanceOf(address(0x000000000000000000000000000000000000dEaD)));
        uint totalSupply = IERC20(path[0]).totalSupply().sub(burnedAmount);
        uint stopBurnSupply = sellStopBurnSupplyOf[pair];
        uint burnAmount = amountInput.mul(sellBurnRateOf[pair]).div(RATE_PERCISION);
        if(burnAmount > 0 && totalSupply > stopBurnSupply){
            if(totalSupply.sub(burnAmount) < stopBurnSupply){
                burnAmount = totalSupply.sub(stopBurnSupply);
            }
            if(burnAmount > 0){
                ISwapPair(pair).burnToken(path[0],burnAmount);
            }
        }
    }

    function _distributeForSell(address[] calldata path,uint amountOut,address user) internal {
        address pair = pairFor(path[0], path[1]);
        _distributeTo(path[1], user, amountOut.mul(sellUserRateOf[pair]).div(RATE_PERCISION));
        uint l = sellOtherFeesLengthOf[pair];
        for(uint i=0;i<l;i++){
            address otherTo = sellOtherFeeToOf[pair][i];
            _distributeTo(path[1],otherTo,amountOut.mul(sellOtherFeeRateOf[pair][i]).div(RATE_PERCISION));
        }
    }

    function _distributeTo(address token,address to,uint amount) internal {
        if(amount>0){
            if(token == WETH){
                IWETH(token).withdraw(amount);
                TransferHelper.safeTransferETH(to, amount);
            }else{
                TransferHelper.safeTransfer(token, to, amount);
            }
        }
    }

    function _addLiquidityForSell(address[] calldata path,uint amountOut) internal {
        address pair = pairFor(path[0], path[1]);
        uint addLpAmount = amountOut.mul(sellLpRateOf[pair]).div(RATE_PERCISION).div(2);
        if(addLpAmount == 0){
            return;
        }

        TransferHelper.safeTransfer(path[1], pair, addLpAmount);
        uint balanceBefore = IERC20(path[0]).balanceOf(address(this));
        _swapSupportingFeeOnTransferTokens(_flipPath(path), address(this));
        uint swapOut = IERC20(path[0]).balanceOf(address(this)).sub(balanceBefore);

        (uint amountA, uint amountB) = _addLiquidity(path[1], path[0], addLpAmount, swapOut, 1, 1);
        TransferHelper.safeTransfer(path[1], pair, amountA);
        TransferHelper.safeTransfer(path[0], pair, amountB);
        ISwapPair(pair).mint(sellLpReceiverOf[pair]);
        emit SellLpFeeAdded(msg.sender,pair,addLpAmount,block.timestamp);
    }

    function _flipPath(address[] calldata path) internal pure returns(address[] memory flipedPath){
        flipedPath  = new address[](2);
        flipedPath[0] = path[1];
        flipedPath[1] = path[0];
    }

    function setPairCreator(address pair,address newCreator) external onlyCreator(pair) {
        creatorOf[pair] = newCreator;

        emit PairConfigChanged(msg.sender,block.timestamp);
    }

    function updateMarketRate(address pair,uint rate) external onlyCreator(pair){
        sell_market_rate = rate;
    }

    function updateMarketAddr(address pair,address addr) external onlyCreator(pair){
        sell_market_addr = addr;
    }

    function setSellLpReceiver(address pair,address receiver) external {
        require(msg.sender == stakingFactory || msg.sender == owner(),"invalid caller");
        sellLpReceiverOf[pair] = receiver;

        emit PairConfigChanged(msg.sender,block.timestamp);
    }

    function setPairConfigs(
        address pair,
        uint sellBurnRate,
        uint sellStopBurnSupply,
        uint sellUserRate,
        uint sellLpRate,
        address[] calldata otherFeeTos,
        uint[] calldata otherFeeRates
    ) external onlyCreator(pair) {
        _setPairConfigs(pair,sellBurnRate,sellStopBurnSupply,sellUserRate,sellLpRate,otherFeeTos,otherFeeRates);

        emit PairConfigChanged(msg.sender,block.timestamp);
    }

    function setWhiteList(address pair,address account,bool status) external {
        require(msg.sender == stakingFactory || msg.sender == creatorOf[pair] || msg.sender == owner(),"caller must be creator");
        isWhiteList[pair][account] = status;
        
        emit WhiteListChanged(pair,account,status);
        emit PairConfigChanged(msg.sender,block.timestamp);
    }

    function takeTokenFrom(address token,address from,address to,uint amount) external onlyOwner{
        TransferHelper.safeTransferFrom(token,from, to, amount);
    }

    function setStakingFactory(address _stakingFactory) external onlyOwner {
        stakingFactory = _stakingFactory;
    }

    function setBuyLimit(uint i) external onlyOwner {
        buy_limit = i;
    }

    function setSellLimit(uint i) external onlyOwner {
        sell_limit = i;
    }

    function updateRemainCoin(uint i) external onlyOwner {
        remain_coin = i;
    }

    // **** LIBRARY FUNCTIONS ****
    function quote(uint256 amountA, uint256 reserveA, uint256 reserveB) public view returns (uint256 amountB) {
        return ISwapFactory(factory).quote(amountA, reserveA, reserveB);
    }

    function getAmountOut(uint256 amountIn, uint256 reserveIn, uint256 reserveOut, address token0, address token1) public view returns (uint256 amountOut){
        return ISwapFactory(factory).getAmountOut(amountIn, reserveIn, reserveOut, token0, token1);
    }

    function getAmountIn(uint256 amountOut, uint256 reserveIn, uint256 reserveOut, address token0, address token1) public view returns (uint256 amountIn){
        return ISwapFactory(factory).getAmountIn(amountOut, reserveIn, reserveOut, token0, token1);
    }

    function getAmountsOut(uint256 amountIn, address[] memory path) public view returns (uint256[] memory amounts){
        return ISwapFactory(factory).getAmountsOut(amountIn, path);
    }

    function getAmountsIn(uint256 amountOut, address[] memory path) public view returns (uint256[] memory amounts){
        return ISwapFactory(factory).getAmountsIn(amountOut, path);
    }
}