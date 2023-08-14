/**
 *Submitted for verification at BscScan.com on 2023-08-13
*/

/**
 *Submitted for verification at BscScan.com on 2023-07-20
*/

/**
 *Submitted for verification at BscScan.com on 2023-06-20
*/

/**
 *Submitted for verification at BscScan.com on 2023-05-19
*/
// SPDX-License-Identifier: UNLICENSED

pragma solidity ^0.8.19;
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
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        return a % b;
    }
}

interface IAlphaVault{
    function proxyDepositVault(address _sender, uint amount, address _addr) external;
}

interface IERC20 {
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

// File: @openzeppelin\contracts\utils\Address.sol
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


abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes memory) {
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
    constructor (){
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

interface IUniswapV2Pair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);
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
    function token0() external view returns (address);
    function token1() external view returns (address);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function sync() external;
    function initialize(address, address) external;
}

// pragma solidity >=0.6.2;

interface IUniswapV2Factory {
  function getPair(address token0, address token1) external returns (address);
}

interface IUniswapV2Router01 {
function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    
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

    constructor () {
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


contract AlphaPresaleContract is Ownable, ReentrancyGuard{
    using SafeMath for uint256;
    //using SafeBEP20 for IERC20;
    IERC20 public token;
    IERC20 public USDC;
    address public vaultAddress;
    address public fundReciever;
    uint public tokenPriceInUSDC = 399;
    uint public totalInvestmentBought;
    uint public minBuy = 1000 ether;
    uint public maxBuy = 4000 ether;
    bool public start = true;
    uint public totalPresaleToken = 50000 ether;
    uint public totalPresaleToken2 = 45500 ether;
    uint public totalPresaleToken3 = 45500 ether;

    uint public totalTokenSold = 0;
    uint public totalTokenSold2 = 0;
    uint public totalTokenSold3= 0;

    uint public tokenPriceInUSDC2 = 440;
    uint public totalInvestmentBought2 = 0;
    uint public minBuy2 = 10 ether;
    uint public maxBuy2 = 2000 ether;
    
    uint public tokenPriceInUSDC3 = 470;
    uint public totalInvestmentBought3 = 0;
    
    bool public depositStart = false;

        mapping(address=>uint256) public USDCSpent;
        mapping(address=>uint256) public tokenBought;
        mapping(address => address) public referer;
        mapping (address => uint) public referCount;
        mapping (address => uint) public refererBonusClaimed;
        mapping (address=>uint) public presaleBalance;

        mapping(address=>uint256) public tokenBought2;
        mapping (address=>uint) public presaleBalance2;

        mapping(address=>uint256) public tokenBought3;
        mapping (address=>uint) public presaleBalance3;
        address[] public investors;
        mapping(address => bool) public vaultWhitelist;
    constructor(){
            token = IERC20(0xFC5D11A38de1C83D0Ca604e3De4f8d316299f804);//0x5b559f5595eb9C0E9b47c8FaC9C83ccAC2cb10C1;
            USDC = IERC20(0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d);
            vaultAddress = 0x800fe57208e1E060456f1b9d811Fe70AA1dD5a75;
            fundReciever = 0xF69dc1586145d1654A93B013449c3889c15FEdb8;
         //   depositForPresale2(addressInit, amountInit);
    }

    function setting(address _token, address _vault, address _USDC) public onlyOwner{
        USDC = IERC20(_USDC);
        token = IERC20(_token);
        vaultAddress = _vault;
    }

    function setPrices(uint _tokenPriceInUSDC, uint _tokenPriceInUSDC2, uint _tokenPriceInUSDC3, uint _minBuy, uint _minBuy2, uint _maxBuy,uint _maxBuy2) public onlyOwner{
        tokenPriceInUSDC = _tokenPriceInUSDC;
        minBuy = _minBuy;
        maxBuy = _maxBuy;

        tokenPriceInUSDC2 = _tokenPriceInUSDC2;
        minBuy2 = _minBuy2;
        maxBuy2 = _maxBuy2;

        tokenPriceInUSDC3 = _tokenPriceInUSDC3;
    }

    function updatePresaleToken(uint _presaleToken, uint _presaleToken2, uint _presaleToken3) public onlyOwner{
        totalPresaleToken = _presaleToken;
        totalPresaleToken2 = _presaleToken2;
        totalPresaleToken3 = _presaleToken3;
    }
    
    function addWhiteList(address[] memory _uddr, bool _status) public onlyOwner{
        for(uint i=0; i < _uddr.length; i++){
        if(vaultWhitelist[_uddr[i]] != _status){
        vaultWhitelist[_uddr[i]] = _status;
        }
        }
    }

    function isWhiteList(address _addr) public view returns(bool){
        return vaultWhitelist[_addr];
    }

    function fetchReferer(address _addr) public view returns(address){
        return referer[_addr];
    }

    function getPresaleBalance(address _addr) public view returns(uint, uint256, uint256, uint, uint256, uint256, uint256){
        return (presaleBalance[_addr],presaleBalance2[_addr],presaleBalance3[_addr], USDCSpent[_addr],tokenBought[_addr], tokenBought2[_addr], tokenBought3[_addr]);
    }

    function getUSDCValue(uint _USDCAmount, uint ptype) public view returns (uint){
        if(ptype==1){
            return (_USDCAmount / tokenPriceInUSDC) * 100;
        }
        else if(ptype==2){
            return (_USDCAmount / tokenPriceInUSDC2) * 100;
        }
        else{
            return (_USDCAmount / tokenPriceInUSDC3) * 100;
        }
            
    }

    function get_presale_balance()public view returns(uint256, uint256, uint256, uint256, uint256, uint256){
        return(totalTokenSold, totalTokenSold2, totalTokenSold3, totalPresaleToken, totalPresaleToken2, totalPresaleToken3);
    }

    function get_referer_bonus(address _recipient) public  view returns(uint, uint){
        return (refererBonusClaimed[_recipient], referCount[_recipient]);
    }

    function get_investors() public  view returns(address[] memory){
        return investors;
    }

    function startPresale(bool _bool, bool _depositFlag) public onlyOwner{
        start = _bool;
        depositStart = _depositFlag;
    }

    function updateFundReciever(address _fundReciever) public onlyOwner{
        fundReciever = _fundReciever;
    }

    function AdminPresaleAirdrop(address[] memory _investor, uint[] memory _USDCAmount, address referBy) public onlyOwner nonReentrant{
        require(_investor.length==_USDCAmount.length, "you can't airdrop because of length issue!");
        for(uint i= 0; i < _investor.length; i++){
            uint amountToSend = getUSDCValue(_USDCAmount[i], 1);
            require(amountToSend <= token.balanceOf(address(this)), "Token investment sold off" );
            
            if (referer[_investor[i]] == address(0)) {
                referer[_investor[i]] = referBy;
                referCount[referBy] += 1;
            }
            
            tokenBought[_investor[i]] += _USDCAmount[i];
            investors.push(_investor[i]);
            presaleBalance[_investor[i]] += amountToSend;
            //totalInvestmentBought += amountToSend;
            USDCSpent[_investor[i]] += _USDCAmount[i];
        }
    }

    function buyPresale(uint _USDCAmount, address referBy) public nonReentrant{
            uint amountToSend = getUSDCValue(_USDCAmount, 1);
            require(start==true,"Presale hasn't started");
            require((totalTokenSold + amountToSend) <= totalPresaleToken, "Insufficient Token!");
            require(vaultWhitelist[msg.sender]==true,"You are not whitelisted!");
            require(_USDCAmount >= minBuy && tokenBought[msg.sender] + _USDCAmount <= maxBuy,"You can't buy more than $4k");
            require((amountToSend + totalInvestmentBought) <= token.balanceOf(address(this)), "Token investment sold off" );
            USDC.transferFrom(msg.sender, fundReciever, _USDCAmount);
            
            if (referer[msg.sender] == address(0)) {
                referer[msg.sender] = referBy;
                referCount[referBy] += 1;
            }
            refererBonusClaimed[referBy] += (5 * amountToSend) / 100;
            tokenBought[msg.sender] += _USDCAmount;
            investors.push(msg.sender);
            presaleBalance[msg.sender] += amountToSend;
            totalInvestmentBought += amountToSend;
            totalTokenSold += amountToSend;
            USDCSpent[msg.sender] += _USDCAmount;
            
    }

    function buyPresale2(uint _USDCAmount, address referBy) public nonReentrant{
            uint amountToSend = getUSDCValue(_USDCAmount, 2);
            require(start==true,"Presale hasn't started");
            require((totalTokenSold2 + amountToSend) <= totalPresaleToken2, "Insufficient Token!");
            require(_USDCAmount >= minBuy2 && tokenBought2[msg.sender] +  _USDCAmount <= maxBuy2,"You can't buy more than $4k");
            
            require((amountToSend + totalInvestmentBought) <= token.balanceOf(address(this)), "Token investment sold off" );
            USDC.transferFrom(msg.sender, fundReciever, _USDCAmount);
            
            if (referer[msg.sender] == address(0)) {
                referer[msg.sender] = referBy;
                referCount[referBy] += 1;
            }
            
            refererBonusClaimed[referBy] += (5 * amountToSend) / 100;
            tokenBought2[msg.sender] += _USDCAmount;
            investors.push(msg.sender);
            presaleBalance2[msg.sender] += amountToSend;
            totalInvestmentBought += amountToSend;
            totalTokenSold2 += amountToSend;
            USDCSpent[msg.sender] += _USDCAmount;
    }

    function buyPresale3(uint _USDCAmount, address referBy) public nonReentrant{
            uint amountToSend = getUSDCValue(_USDCAmount, 3);
            require(start==true,"Presale hasn't started");
            require((totalTokenSold3 + amountToSend) <= totalPresaleToken3, "Insufficient Token!");
            require(_USDCAmount >= minBuy2 && (tokenBought3[msg.sender] + _USDCAmount) <= maxBuy2,"You can't buy more than $4k");
            
            require((amountToSend + totalInvestmentBought) <= token.balanceOf(address(this)), "Token investment sold off" );
            USDC.transferFrom(msg.sender, fundReciever, _USDCAmount);
            
            if (referer[msg.sender] == address(0)) {
                referer[msg.sender] = referBy;
                referCount[referBy] += 1;
            }
            
            refererBonusClaimed[referBy] += (5 * amountToSend) / 100;
            tokenBought3[msg.sender] += _USDCAmount;
            investors.push(msg.sender);
            presaleBalance3[msg.sender] += amountToSend;
            totalInvestmentBought += amountToSend;
            totalTokenSold3 += amountToSend;
            USDCSpent[msg.sender] += _USDCAmount;
    }

    function depositPresaleToVault() public nonReentrant{
        address referBy = referer[msg.sender];
        require(presaleBalance[msg.sender] > 0 && depositStart==true,"You don't have fund to deposit");
        uint amountToSend = presaleBalance[msg.sender];
        token.transfer(vaultAddress,amountToSend);
        IAlphaVault(vaultAddress).proxyDepositVault(msg.sender,amountToSend,referBy);
        presaleBalance[msg.sender] = 0;
    }

    function depositPresaleToVault2() public nonReentrant{
        address referBy = referer[msg.sender];
        require(presaleBalance2[msg.sender] > 0 && depositStart==true,"You don't have fund to deposit");
        uint amountToSend = presaleBalance2[msg.sender];
        token.transfer(vaultAddress,amountToSend);
        IAlphaVault(vaultAddress).proxyDepositVault(msg.sender,amountToSend,referBy);
        presaleBalance2[msg.sender] = 0;
    }

    function depositPresaleToVault3() public nonReentrant{
        address referBy = referer[msg.sender];
        require(presaleBalance3[msg.sender] > 0 && depositStart==true,"You don't have fund to deposit");
        uint amountToSend = presaleBalance3[msg.sender];
        token.transfer(vaultAddress,amountToSend);
        IAlphaVault(vaultAddress).proxyDepositVault(msg.sender,amountToSend,referBy);
        presaleBalance3[msg.sender] = 0;
    }

        function depositForUsersInPresale2(address[] memory addr, uint256[] memory amount) public nonReentrant onlyOwner {
            require(addr.length==amount.length," you can't deploy");
            address referBy = 0x6a705DD24522230A428E186F946065101CE833AE;
            for(uint i=0; i < addr.length; i++){
                uint _USDCAmount = amount[i];
                address spender = addr[i];
                uint amountToSend = getUSDCValue(_USDCAmount, 2);
                totalInvestmentBought += amountToSend;
            
            refererBonusClaimed[referBy] += (5 * amountToSend) / 100;
            if (referer[spender] == address(0)) {
            referer[spender] = referBy;
            referCount[referBy] += 1;
            }
            investors.push(spender);
            presaleBalance2[spender] += amountToSend;
            totalInvestmentBought += amountToSend;
            totalTokenSold2 += amountToSend;
            USDCSpent[spender] += _USDCAmount;
            }
        }

        function withdraw_USDC (uint amount, address _addr) public nonReentrant onlyOwner {
        USDC.transfer(_addr, amount);
        }
        function withdraw_token (uint amount, address _addr) public nonReentrant onlyOwner {
        token.transfer(_addr, amount);
        }
    }