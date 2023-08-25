// File: @openzeppelin/contracts/math/SafeMath.sol

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

// File: @openzeppelin/contracts/math/Math.sol

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

// File: contracts/interface/IERC20.sol

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
    function mint(address account, uint amount) external;
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

// File: contracts/interface/IPlayerBook.sol

pragma solidity ^0.5.0;


interface IPlayerBook {
    function settleReward( address from,uint256 amount ) external returns (uint256);
    function bindRefer( address from,string calldata  affCode )  external returns (bool);
    function hasRefer(address from) external returns(bool);

    function addPool(address poolAddr, address rewardToken) external;
}

// File: contracts/interface/IUniswapV2Router02.sol

pragma solidity ^0.5.0;

interface IUniswapV2Router02 {
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
}

// File: contracts/interface/IPool.sol

pragma solidity ^0.5.0;


interface IPool {
    function totalSupply( ) external view returns (uint256);
    function balanceOf( address player ) external view returns (uint256);
}

// File: @openzeppelin/contracts/utils/Address.sol

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

// File: contracts/library/SafeERC20.sol

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

    bytes4 private constant SELECTOR = bytes4(keccak256(bytes('transfer(address,uint256)')));

    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        (bool success, bytes memory data) = address(token).call(abi.encodeWithSelector(SELECTOR, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'SafeERC20: TRANSFER_FAILED');
    }
    // function safeTransfer(IERC20 token, address to, uint256 value) internal {
    //     callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    // }

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

// File: contracts/library/Governance.sol

pragma solidity ^0.5.0;

contract Governance {

    address public _governance;

    constructor() public {
        _governance = tx.origin;
    }

    event GovernanceTransferred(address indexed previousOwner, address indexed newOwner);

    modifier onlyGovernance {
        require(msg.sender == _governance, "not governance");
        _;
    }

    function setGovernance(address governance)  public  onlyGovernance
    {
        require(governance != address(0), "new governance the zero address");
        emit GovernanceTransferred(_governance, governance);
        _governance = governance;
    }


}

// File: contracts/interface/IPowerStrategy.sol

pragma solidity ^0.5.0;


interface IPowerStrategy {
    function lpIn(address sender, uint256 amount) external;
    function lpOut(address sender, uint256 amount) external;
    
    function getPower(address sender) view  external returns (uint256);
}

// File: contracts/interface/IWBNB.sol

pragma solidity >=0.5.0;

interface IWBNB {
    function deposit() external payable;
    function transfer(address to, uint value) external returns (bool);
    function withdraw(uint) external;
}

// File: contracts/library/SegmentPowerStrategy.sol

pragma solidity ^0.5.0;



contract SegmentPowerStrategy is IPowerStrategy {
    using SafeMath for uint256;
    ////
    struct degoSegment {
        uint256 min;
        uint256 max;
    }
    struct countSegment {
        uint32 length;
        uint32 curCount;
    }
    struct playerInfo {
        uint256 amount;
        uint8 segIndex;
        uint32 playerId;
        uint32 offset;
    }

    mapping(address => uint32) public _addressXId;
    mapping(uint8 => degoSegment) public _degoSegment;
    mapping(uint8 => countSegment) public _countSegment;
    mapping(uint8 => mapping(uint32 => uint32)) public _playerIds;
    mapping(uint32 => playerInfo) public _playerMap;

    uint8[3] public _ruler = [8, 1, 1];
    uint8[3] public _factor = [3, 5, 1];

    uint8 public _high = 3;
    uint8 public _mid = 2;
    uint8 public _low = 1;

    uint32 public _playerId = 0;
    uint32 public _base = 100;
    uint32 public _anchor = _base;
    uint32 public _grouthCondition = 100;
    uint32 public _grouthStep = 10;
    uint32 constant public _highMax = 50;
    uint32 constant public _midMax = 50;

    uint256 constant public  _initMaxValue = 500 * (10**18);  //500lp,10w usdt,100 eth

    address public _contractCaller = address(0x0);

    /**
     * check pool
     */
    modifier isNormalPool(){
        require( msg.sender==_contractCaller,"invalid pool address!");
        _;
    }

    constructor()
        public
    {
        _contractCaller = msg.sender;
        _playerId = 0;

        initSegment();
        updateRuler(_initMaxValue);
    }

    function lpIn(address sender, uint256 amount) 
    isNormalPool()
    external {

        uint32 playerId = _addressXId[sender];
        if ( playerId > 0 ) {
            _playerMap[playerId].amount = _playerMap[playerId].amount.add(amount);
        } else {
            //new addr
            _playerId = _playerId+1;
            _addressXId[sender] = _playerId;

            playerId = _playerId;
            _playerMap[playerId].playerId = playerId;
            _playerMap[playerId].amount = amount;
            _playerMap[playerId].segIndex = 0;
            _playerMap[playerId].offset =  0;

            //update segment
            updateSegment();
        }

        settlePowerData(playerId);
    }

    function lpOut(address sender, uint256 amount) 
    isNormalPool()
    external{
        uint32 playerId = _addressXId[sender];
        if ( playerId > 0 ) {
            _playerMap[playerId].amount = _playerMap[playerId].amount.sub(amount);
        } else {
            return;
        }

        settlePowerData(playerId);
    }
    
    function getPower(address sender) 
    view external
    returns (uint256) {

        uint32 playerId = _addressXId[sender];
        if ( playerId > 0 ) {
            uint8 segment = _playerMap[playerId].segIndex;
            if(segment>0){
                return uint256(_factor[segment-1]).mul(_playerMap[playerId].amount);
            }
        }

        return 0;
    }

    function updateRuler( uint256 maxCount ) internal{

        uint256 lastBegin = 0;
        uint256 lastEnd = 0;
        uint256 splitPoint = 0;
        for (uint8 i = 1; i <= _ruler.length; i++) {
            splitPoint = maxCount * _ruler[i - 1]/10;
            if (splitPoint <= 0) {
                splitPoint = 1;
            }
            lastEnd = lastBegin + splitPoint;
            if (i == _ruler.length) {
                lastEnd = maxCount;
            }
            _degoSegment[i].min = lastBegin + 1;
            _degoSegment[i].max = lastEnd;
            lastBegin = lastEnd;
        }
    }

    function initSegment() internal {    

        _countSegment[_low].length = 80;
        _countSegment[_mid].length = 10;
        _countSegment[_high].length = 10;

        _countSegment[_low].curCount = 0;
        _countSegment[_mid].curCount = 0;
        _countSegment[_high].curCount = 0;
    }

    function updateSegment( ) internal {

        if (_playerId >= _grouthCondition+_anchor ) {
            if (_countSegment[_high].length + _grouthStep > _highMax) {
                _countSegment[_high].length = _highMax;
            } else {
                _countSegment[_high].length = _countSegment[_high].length+_grouthStep;
            }

            if (_countSegment[_mid].length + _grouthStep > _midMax) {
                _countSegment[_mid].length = _midMax;
            } else {
                _countSegment[_mid].length = _countSegment[_mid].length+_grouthStep;
            }
            _anchor = _playerId;
        }
    }

    function hasCountSegmentSlot(uint8 segIndex) internal view returns (bool){
        uint32 value = _countSegment[segIndex].length-_countSegment[segIndex].curCount;
        if (value > 0) {
            return true;
        } else {
            return false;
        }
    }

    function findSegmentMinPlayer(uint8 segIndex) internal view returns (uint32,uint256){
        uint256 firstMinAmount = _degoSegment[segIndex].max;
        uint256 secondMinAmount = _degoSegment[segIndex].max;
        uint32 minPlayerOffset = 0;
        for (uint8 i = 0; i < _countSegment[segIndex].curCount; i++) {
            uint32 playerId = _playerIds[segIndex][i];
            if( playerId==0 ){
                continue;
            }
            uint256 amount = _playerMap[playerId].amount;

            //find min amount;
            if ( amount < firstMinAmount) {
                if (firstMinAmount < secondMinAmount) {
                    secondMinAmount = firstMinAmount;
                }
                firstMinAmount = amount;
                minPlayerOffset = i;
            }else{
                //find second min amount
                if(amount < secondMinAmount ){
                    secondMinAmount = amount;
                }
            }
        }

        return (minPlayerOffset,secondMinAmount);
    }

    //swap the player data from old segment to the new segment
    function segmentSwap(uint32 playerId, uint8 segIndex) internal {

        uint8 oldSegIndex = _playerMap[playerId].segIndex;

        uint32 oldOffset = _playerMap[playerId].offset;
        uint32 tail = _countSegment[segIndex].curCount;

        _playerMap[playerId].segIndex = segIndex;
        _playerMap[playerId].offset = tail;

        _countSegment[segIndex].curCount = _countSegment[segIndex].curCount+1;
        _playerIds[segIndex][tail] = playerId;

        if (oldSegIndex>0 && segIndex != oldSegIndex && _playerIds[oldSegIndex][oldOffset] > 0) {

            uint32 originTail = _countSegment[oldSegIndex].curCount-1;
            uint32 originTailPlayer = _playerIds[oldSegIndex][originTail];

            if(originTailPlayer != playerId){

                _playerMap[originTailPlayer].segIndex = oldSegIndex;
                _playerMap[originTailPlayer].offset = oldOffset;
                _playerIds[oldSegIndex][oldOffset] = originTailPlayer;
            }

            _playerIds[oldSegIndex][originTail] = 0;
            _countSegment[oldSegIndex].curCount = _countSegment[oldSegIndex].curCount-1;
        }
    }

    //swap the player data with tail 
    function tailSwap( uint8 segIndex) internal returns (uint32){

        uint32 minPlayerOffset;
        uint256 secondMinAmount;
        (minPlayerOffset,secondMinAmount) = findSegmentMinPlayer(segIndex);
        _degoSegment[segIndex].min = secondMinAmount;

        uint32 leftPlayerId = _playerIds[segIndex][minPlayerOffset];

        //segmentSwap to reset
        uint32 tail = _countSegment[segIndex].curCount - 1;
        uint32 tailPlayerId = _playerIds[segIndex][tail];
        _playerIds[segIndex][minPlayerOffset] = tailPlayerId;

        _playerMap[tailPlayerId].offset = minPlayerOffset;

        return leftPlayerId;
    }

    function joinHigh(uint32 playerId) internal {
        uint8 segIndex = _high;
        if (hasCountSegmentSlot(segIndex)) {
            segmentSwap(playerId, segIndex);
        } else {
            uint32 leftPlayerId = tailSwap(segIndex);
            joinMid(leftPlayerId);
            segmentSwap(playerId, segIndex);

        }
    }

    function joinMid(uint32 playerId) internal {
        uint8 segIndex = _mid;
        if (hasCountSegmentSlot(segIndex)) {
            segmentSwap(playerId, segIndex);
        } else {
            uint32 leftPlayerId = tailSwap(segIndex);
            joinLow(leftPlayerId);
            segmentSwap(playerId, segIndex);
        }
        _degoSegment[segIndex].max = _degoSegment[segIndex + 1].min;
    }

    function joinLow(uint32 playerId) internal {

        uint8 segIndex = _low;
        segmentSwap(playerId, segIndex);
        _degoSegment[segIndex].max = _degoSegment[segIndex + 1].min;
        //_low segment length update
        if( _countSegment[segIndex].curCount > _countSegment[segIndex].length){
            _countSegment[segIndex].length = _countSegment[segIndex].curCount;
        }
    }

    function settlePowerData(uint32 playerId) internal {

        uint256 amount = _playerMap[playerId].amount;
        uint8 segIndex = 0;
        for (uint8 i = 1; i <= _high; i++) {
            if (amount < _degoSegment[i].max) {
                segIndex = i;
                break;
            }
        }
        if (segIndex == 0) {
            _degoSegment[_high].max = amount;
            segIndex = _high;
        }

        if (_playerMap[playerId].segIndex == segIndex) {
            return;
        }

        if (segIndex == _high) {
            joinHigh(playerId);
        } else if (segIndex == _mid) {
            joinMid(playerId);
        } else {
            joinLow(playerId);
        }
    }

    ////////////////////////////
}

// File: contracts/library/LPTokenWrapper.sol

pragma solidity ^0.5.0;











contract LPTokenWrapper is IPool,Governance {
    using SafeMath for uint256;
    using SafeERC20 for IERC20;

    IERC20 public _lpToken;

    address public _playerBook = address(0xA86b351704319493aD17faC7C1779a07194506e3);
    address public _wbnb = address(0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c);

    uint256 private _totalSupply;
    mapping(address => uint256) private _balances;

    uint256 private _totalPower;
    mapping(address => uint256) private _powerBalances;
    
    address public _powerStrategy = address(0x0);
    address public powerStrategy = address(0x0);

    constructor(address lpToken_) public {
        require(lpToken_ != address(0), 'LPTokenWrapper::constructor: lpToken_ is empty');
        _lpToken = IERC20(lpToken_);
    }

    function totalSupply() public view returns (uint256) {
        return _totalSupply;
    }

    function enablePowerStragegy()  public  onlyGovernance{
        if( powerStrategy == address(0x0)){
            powerStrategy = address(new SegmentPowerStrategy());
        }
        _powerStrategy = powerStrategy;
    }

    function disablePowerStragegy()  public  onlyGovernance{
        _powerStrategy = address(0x0);
    }

    function balanceOf(address account) public view returns (uint256) {
        return _balances[account];
    }

    function balanceOfPower(address account) public view returns (uint256) {
        return _powerBalances[account];
    }

    function totalPower() public view returns (uint256) {
        return _totalPower;
    }


    function _stake(uint256 amount, string memory affCode) internal {
        _totalSupply = _totalSupply.add(amount);
        _balances[msg.sender] = _balances[msg.sender].add(amount);

        if( _powerStrategy != address(0x0)){ 
            _totalPower = _totalPower.sub(_powerBalances[msg.sender]);
            IPowerStrategy(_powerStrategy).lpIn(msg.sender, amount);

            _powerBalances[msg.sender] = IPowerStrategy(_powerStrategy).getPower(msg.sender);
            _totalPower = _totalPower.add(_powerBalances[msg.sender]);
        }else{
            _totalPower = _totalSupply;
            _powerBalances[msg.sender] = _balances[msg.sender];
        }
        if(address(_lpToken) == _wbnb && msg.value > 0){
            require(msg.value == amount,"Invalid amount");
            IWBNB(_wbnb).deposit.value(amount)();
        }else{
            _lpToken.safeTransferFrom(msg.sender, address(this), amount);
        }
        if (!IPlayerBook(_playerBook).hasRefer(msg.sender)) {
            IPlayerBook(_playerBook).bindRefer(msg.sender, affCode);
        }

        
    }

    function withdraw(uint256 amount) public {
        require(amount > 0, "amount > 0");

        _totalSupply = _totalSupply.sub(amount);
        _balances[msg.sender] = _balances[msg.sender].sub(amount);
        
        if( _powerStrategy != address(0x0)){ 
            _totalPower = _totalPower.sub(_powerBalances[msg.sender]);
            IPowerStrategy(_powerStrategy).lpOut(msg.sender, amount);
            _powerBalances[msg.sender] = IPowerStrategy(_powerStrategy).getPower(msg.sender);
            _totalPower = _totalPower.add(_powerBalances[msg.sender]);

        }else{
            _totalPower = _totalSupply;
            _powerBalances[msg.sender] = _balances[msg.sender];
        }

        if(address(_lpToken) == _wbnb){
            IWBNB(_wbnb).withdraw(amount);
            address(msg.sender).transfer(amount);
        }else{
            _lpToken.safeTransfer( msg.sender, amount);
        }
    }


    function setPlayerBook(address playerbook) public onlyGovernance{
        _playerBook = playerbook;
    }
}

// File: contracts/reward/UniswapReward.sol

pragma solidity ^0.5.0;











contract RewardsDistributionRecipient {
    address public rewardsDistribution;

    modifier onlyRewardsDistribution() {
        require(msg.sender == rewardsDistribution, "Caller is not RewardsDistribution contract");
        _;
    }
}


interface IUniswapV2ERC20 {
    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;
}


contract UniswapReward is LPTokenWrapper,RewardsDistributionRecipient{
    using SafeERC20 for IERC20;

    IERC20 public _rewardsToken;
    address public _teamWallet = 0x171a30624b0DbE315DbEBB41261D4496dDe5bbd0;
    address public _rewardPool = 0xf547e707beaCed069092141c2C4690F62BDcf668;

    uint256 public constant DURATION = 1 days;

    uint256 public _initReward;
    uint256 public _periodFinish = 0;
    uint256 public _rewardRate = 0;
    uint256 public _lastUpdateTime;
    uint256 public _rewardPerTokenStored;

    uint256 public _teamRewardRate = 0;
    uint256 public _poolRewardRate = 0;
    uint256 public _baseRate = 10000;
    uint256 public _punishTime = 0 days;
    uint256 public _claimPunishTime = 0;
    uint256 public _claimRewardRate = 0;

    mapping(address => uint256) public _userRewardPerTokenPaid;
    mapping(address => uint256) public _rewards;
    mapping(address => uint256) public _lastStakedTime;
    mapping(address => uint256) public _lastEarnedTime;

    event RewardAdded(uint256 reward);
    event Staked(address indexed user, uint256 amount);
    event Withdrawn(address indexed user, uint256 amount);
    event RewardPaid(address indexed user, uint256 reward);


    modifier updateReward(address account) {
        _rewardPerTokenStored = rewardPerToken();
        _lastUpdateTime = lastTimeRewardApplicable();
        if (account != address(0)) {
            _rewards[account] = earned(account);
            _userRewardPerTokenPaid[account] = _rewardPerTokenStored;
        }
        _;
    }

    function () external payable {
    }

    constructor(
        address rewardsDistribution_,
        address token_,
        address lpToken_
    ) LPTokenWrapper(lpToken_) public {
        require(token_ != address(0), 'UniswapReward::constructor: _token is empty');

        _rewardsToken = IERC20(token_);
        rewardsDistribution = rewardsDistribution_;
    }

    /* Fee collection for any other token */
    function seize(IERC20 token, uint256 amount) external onlyGovernance{
        require(token != _rewardsToken, "reward");
        require(token != _lpToken, "stake");
        token.safeTransfer(_governance, amount);
    }

    function setTeamRewardRate( uint256 teamRewardRate ) public onlyGovernance{
        require(teamRewardRate <= 2000, "teamRewardRate <= 2000");
        _teamRewardRate = teamRewardRate;
    }

    function setPoolRewardRate( uint256  poolRewardRate ) public onlyGovernance{
        require(poolRewardRate <= 7000, "poolRewardRate <= 7000");
        _poolRewardRate = poolRewardRate;
    }

    function setWithDrawPunishTime( uint256  punishTime ) public onlyGovernance{
        require(punishTime <= 15 days, 'punishTime <= 15 days');
        _punishTime = punishTime;
    }

    function lastTimeRewardApplicable() public view returns (uint256) {
        return Math.min(block.timestamp, _periodFinish);
    }

    function rewardPerToken() public view returns (uint256) {
        if (totalPower() == 0) {
            return _rewardPerTokenStored;
        }
        return
            _rewardPerTokenStored.add(
                lastTimeRewardApplicable()
                    .sub(_lastUpdateTime)
                    .mul(_rewardRate)
                    .mul(1e18)
                    .div(totalPower())
            );
    }

    function earned(address account) public view returns (uint256) {
        return
            balanceOfPower(account)
                .mul(rewardPerToken().sub(_userRewardPerTokenPaid[account]))
                .div(1e18)
                .add(_rewards[account]);
    }

    // stake visibility is public as overriding LPTokenWrapper's stake() function
    function stake(uint256 amount, string memory affCode)
        public payable
        updateReward(msg.sender)
    {
        require(amount > 0, "Cannot stake 0");
        super._stake(amount, affCode);

        _lastStakedTime[msg.sender] = now;

        if(_lastEarnedTime[msg.sender] == 0){
            _lastEarnedTime[msg.sender] = now;
        }

        emit Staked(msg.sender, amount);
    }

    // stake visibility is public as overriding LPTokenWrapper's stake() function
    function stakeWithPermit(uint256 amount, string memory affCode, uint deadline, uint8 v, bytes32 r, bytes32 s)
        public payable
    {
        // permit
        IUniswapV2ERC20(address(_lpToken)).permit(msg.sender, address(this), amount, deadline, v, r, s);
        stake(amount,affCode);
    }



    function withdraw(uint256 amount)
    public
    updateReward(msg.sender)
    {
        require(amount > 0, "Cannot withdraw 0");
        super.withdraw(amount);
        emit Withdrawn(msg.sender, amount);
    }

    function exit() external {
        withdraw(balanceOf(msg.sender));
        getReward();
    }

    function getReward() public updateReward(msg.sender) {
        uint256 reward = earned(msg.sender);
        if (reward > 0) {
            _rewards[msg.sender] = 0;
            uint256 fee = IPlayerBook(_playerBook).settleReward(msg.sender, reward);
            if(fee > 0){
                _rewardsToken.safeTransfer(_playerBook, fee);
            }
            
            uint256 teamReward = reward.mul(_teamRewardRate).div(_baseRate);
            if(teamReward>0){
                _rewardsToken.safeTransfer(_teamWallet, teamReward);
            }
            uint256 leftReward = reward.sub(fee).sub(teamReward);
            uint256 poolReward = 0;

            //withdraw time check

            if(now  < (_lastStakedTime[msg.sender] + _punishTime) ){
                poolReward = leftReward.mul(_poolRewardRate).div(_baseRate);
            }else if(now  < (_lastEarnedTime[msg.sender] + _claimPunishTime)){
                poolReward = leftReward.mul(_claimRewardRate).div(_baseRate);
            }
            if(poolReward>0){
                _rewardsToken.safeTransfer(_rewardPool, poolReward);
                leftReward = leftReward.sub(poolReward);
            }

            if(leftReward>0){
                _rewardsToken.safeTransfer(msg.sender, leftReward );
            }
      
            emit RewardPaid(msg.sender, leftReward);
        }
        _lastEarnedTime[msg.sender] = now;
    }

    //for extra reward
    function notifyRewardAmount(uint256 reward)
        external
        onlyRewardsDistribution
        updateReward(address(0))
    {
        if (block.timestamp >= _periodFinish) {
            _rewardRate = reward.div(DURATION);
        } else {
            uint256 remaining = _periodFinish.sub(block.timestamp);
            uint256 leftover = remaining.mul(_rewardRate);
            _rewardRate = reward.add(leftover).div(DURATION);
        }
        _lastUpdateTime = block.timestamp;
        _periodFinish = block.timestamp.add(DURATION);
        emit RewardAdded(reward);
    }

    function setClaimRewardRate(uint256 claimTime,uint256 claimRate) public onlyGovernance{
        require(claimTime <= 15 days, 'startTime <= 15 days');
        require(claimRate <= 7000, "claimRate <= 7000");
        _claimPunishTime = claimTime;
        _claimRewardRate = claimRate;
    }


}

// File: contracts/reward/StakingRewardsFactory.sol

pragma solidity ^0.5.16;




contract StakingRewardsFactory is Governance {
    using SafeMath for uint;
    // immutables
    address public rewardsToken;
    uint public stakingRewardsTotal=0;
    uint public stakingRewardsGenesis;

    // the staking tokens for which the rewards contract has been deployed
    address[] public stakingTokens;

    uint public rewardRateTotal=0;

    // info about rewards for a particular staking token
    struct StakingRewardsInfo {
        address payable stakingRewards;
        uint rewardRate;
    }

    // rewards info by staking token
    mapping(address => StakingRewardsInfo) public stakingRewardsInfoByStakingToken;

    constructor(
        address _rewardsToken,
        uint _stakingRewardsGenesis
    ) Governance() public {
        require(_stakingRewardsGenesis >= block.timestamp, 'genesis too soon');

        rewardsToken = _rewardsToken;
        stakingRewardsGenesis = _stakingRewardsGenesis;
    }

    ///// permissioned functions

    // deploy a staking reward contract for the staking token, and store the reward amount
    // the reward will be distributed to the staking reward contract no sooner than the genesis
    function deploy(address[] memory _stakingTokens, uint[] memory _rewardRates) public onlyGovernance {
        require(_stakingTokens.length == _rewardRates.length, "stakingTokens and rewardRates lengths mismatch");

        for (uint i = 0; i < _rewardRates.length; i++) {
            require(_stakingTokens[i] != address(0), "stakingToken empty");

            StakingRewardsInfo storage  info = stakingRewardsInfoByStakingToken[_stakingTokens[i]];

            rewardRateTotal = rewardRateTotal.sub(info.rewardRate).add(_rewardRates[i]);
            info.rewardRate = _rewardRates[i];

            if(info.stakingRewards == address(0)){
                info.stakingRewards = address(new UniswapReward(
                    /*rewardsDistribution_=*/ address(this),
                    /*token_=*/     rewardsToken,
                    /*lpToken_=*/     _stakingTokens[i]
                    ));
                stakingTokens.push(_stakingTokens[i]);
            }
        }
    }

    function setStakingRate(address stakingToken,uint rewardRate) public onlyGovernance {
        StakingRewardsInfo storage info = stakingRewardsInfoByStakingToken[stakingToken];
        require(info.stakingRewards != address(0), 'not deployed');

        rewardRateTotal = rewardRateTotal.sub(info.rewardRate).add(rewardRate);
        info.rewardRate = rewardRate;
    }

    // call notifyRewardAmount for all staking tokens.
    function notifyRewardAmounts(uint rewardAmount) public onlyGovernance{
        require(stakingTokens.length > 0, 'called before any deploys');
        require(block.timestamp >= stakingRewardsGenesis, 'reward not start');

        uint256 _surplusRewardAmount = IERC20(rewardsToken).balanceOf(address(this));
        require(_surplusRewardAmount >= rewardAmount, 'reward amount overflows');

        stakingRewardsGenesis = stakingRewardsGenesis + 1 days;
        stakingRewardsTotal = stakingRewardsTotal.add(rewardAmount);

        _notifyPoolRewardAmounts(rewardAmount);
    }

    function _notifyPoolRewardAmounts(uint _poolRewardAmount) private {
        for (uint i = 0; i < stakingTokens.length; i++) {
            StakingRewardsInfo memory info = stakingRewardsInfoByStakingToken[stakingTokens[i]];
            if(info.rewardRate <= 0){
                continue;
            }
            uint _rewardAmount = _poolRewardAmount.mul(info.rewardRate).div(rewardRateTotal);
            _notifyRewardAmount(info.stakingRewards,_rewardAmount);
        }
    }

    // notify reward amount for an individual staking token.
    // this is a fallback in case the notifyRewardAmounts costs too much gas to call for all contracts
    function _notifyRewardAmount(address payable _stakingToken,uint _rewardAmount) private {
        require(_stakingToken != address(0), 'not deployed');

        if (_rewardAmount > 0) {
            require(
                IERC20(rewardsToken).transfer(_stakingToken, _rewardAmount),
                'transfer failed'
            );
            UniswapReward(_stakingToken).notifyRewardAmount(_rewardAmount);
        }
    }
}