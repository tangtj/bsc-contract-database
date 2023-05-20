// SPDX-License-Identifier: GPL-3.0
/// tg: https://t.me/HappyPepeToken

pragma solidity 0.8.17;

interface IERC20Permit {
    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;
    function nonces(address owner) external view returns(uint256);
    // solhint-disable-next-line func-name-mixedcase
    function DOMAIN_SEPARATOR() external view returns(bytes32);
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

    function safeApprove(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
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

    function safePermit(
        IERC20Permit token,
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) internal {
        uint256 nonceBefore = token.nonces(owner);
        token.permit(owner, spender, value, deadline, v, r, s);
        uint256 nonceAfter = token.nonces(owner);
        require(nonceAfter == nonceBefore + 1, "SafeERC20: permit did not succeed");
    }

    function _callOptionalReturn(IERC20 token, bytes memory data) private {
        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        if (returndata.length > 0) {
            // Return data is optional
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}

library SafeMath {
    function add(uint x, uint y) internal pure returns (uint z) {
        require((z = x + y) >= x, 'ds-math-add-overflow');
    }

    function sub(uint x, uint y) internal pure returns (uint z) {
        require((z = x - y) <= x, 'ds-math-sub-underflow');
    }

    function mul(uint x, uint y) internal pure returns (uint z) {
        require(y == 0 || (z = x * y) / y == x, 'ds-math-mul-overflow');
    }
}

abstract contract Context {
    function _msgSender() internal view virtual returns(address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns(bytes calldata) {
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _transferOwnership(_msgSender());
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view virtual returns(address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns(uint256);
    function balanceOf(address account) external view returns(uint256);
    function transfer(address to, uint256 amount) external returns(bool);
    function allowance(address owner, address spender) external view returns(uint256);
    function approve(address spender, uint256 amount) external returns(bool);
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns(bool);
}

library Address {
    function isContract(address account) internal view returns(bool) {
        return account.code.length > 0;
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    function functionCall(address target, bytes memory data) internal returns(bytes memory) {
        return functionCallWithValue(target, data, 0, "Address: low-level call failed");
    }

    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns(bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }

    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value
    ) internal returns(bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns(bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }

    function functionStaticCall(address target, bytes memory data) internal view returns(bytes memory) {
        return functionStaticCall(target, data, "Address: low-level static call failed");
    }

    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns(bytes memory) {
        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }

    function functionDelegateCall(address target, bytes memory data) internal returns(bytes memory) {
        return functionDelegateCall(target, data, "Address: low-level delegate call failed");
    }

    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns(bytes memory) {
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }

    function verifyCallResultFromTarget(
        address target,
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal view returns(bytes memory) {
        if (success) {
            if (returndata.length == 0) {
                require(isContract(target), "Address: call to non-contract");
            }
            return returndata;
        } else {
            _revert(returndata, errorMessage);
        }
    }

    function verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal pure returns(bytes memory) {
        if (success) {
            return returndata;
        } else {
            _revert(returndata, errorMessage);
        }
    }

    function _revert(bytes memory returndata, string memory errorMessage) private pure {
        if (returndata.length > 0) {
            /// @solidity memory-safe-assembly
            assembly {
                let returndata_size := mload(returndata)
                revert(add(32, returndata), returndata_size)
            }
        } else {
            revert(errorMessage);
        }
    }
}

library EnumerableSet {
    struct Set {
        bytes32[] _values;
        mapping(bytes32 => uint256) _indexes;
    }

    function _add(Set storage set, bytes32 value) private returns(bool) {
        if (!_contains(set, value)) {
            set._values.push(value);
            set._indexes[value] = set._values.length;
            return true;
        } else {
            return false;
        }
    }

    function _remove(Set storage set, bytes32 value) private returns(bool) {
        uint256 valueIndex = set._indexes[value];

        if (valueIndex != 0) {
            uint256 toDeleteIndex = valueIndex - 1;
            uint256 lastIndex = set._values.length - 1;

            if (lastIndex != toDeleteIndex) {
                bytes32 lastValue = set._values[lastIndex];
                set._values[toDeleteIndex] = lastValue;
                set._indexes[lastValue] = valueIndex;
            }

            set._values.pop();
            delete set._indexes[value];

            return true;
        } else {
            return false;
        }
    }

    function _contains(Set storage set, bytes32 value) private view returns(bool) {
        return set._indexes[value] != 0;
    }

    function _length(Set storage set) private view returns(uint256) {
        return set._values.length;
    }

    function _at(Set storage set, uint256 index) private view returns(bytes32) {
        return set._values[index];
    }

    function _values(Set storage set) private view returns(bytes32[] memory) {
        return set._values;
    }

    struct AddressSet {
        Set _inner;
    }

    function add(AddressSet storage set, address value) internal returns(bool) {
        return _add(set._inner, bytes32(uint256(uint160(value))));
    }

    function remove(AddressSet storage set, address value) internal returns(bool) {
        return _remove(set._inner, bytes32(uint256(uint160(value))));
    }

    function contains(AddressSet storage set, address value) internal view returns(bool) {
        return _contains(set._inner, bytes32(uint256(uint160(value))));
    }

    function length(AddressSet storage set) internal view returns(uint256) {
        return _length(set._inner);
    }

    function at(AddressSet storage set, uint256 index) internal view returns(address) {
        return address(uint160(uint256(_at(set._inner, index))));
    }

    function values(AddressSet storage set) internal view returns(address[] memory) {
        bytes32[] memory store = _values(set._inner);
        address[] memory result;

        /// @solidity memory-safe-assembly
        assembly {
            result := store
        }

        return result;
    }
}

interface IPancakeFactory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}

interface IPancakePair {
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
}

interface IPancakeRouter01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

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
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IPancakeRouter02 is IPancakeRouter01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
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
}

library PancakeLibrary {
    using SafeMath for uint;

    function sortTokens(address tokenA, address tokenB) internal pure returns (address token0, address token1) {
        require(tokenA != tokenB, 'PancakeLibrary: IDENTICAL_ADDRESSES');
        (token0, token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
        require(token0 != address(0), 'PancakeLibrary: ZERO_ADDRESS');
    }

    function pairFor(address factory, address tokenA, address tokenB) internal pure returns (address pair) {
        (address token0, address token1) = sortTokens(tokenA, tokenB);
        pair = address(uint160(uint(keccak256(abi.encodePacked(
                hex'ff',
                factory,
                keccak256(abi.encodePacked(token0, token1)),
                hex'd0d4c4cd0848c93cb4fd1f498d7013ee6bfb25783ea21593d5834f5d250ece66' // init code hash
            )))));
    }

    function getReserves(address factory, address tokenA, address tokenB) internal view returns (uint reserveA, uint reserveB) {
        (address token0,) = sortTokens(tokenA, tokenB);
        pairFor(factory, tokenA, tokenB);
        (uint reserve0, uint reserve1,) = IPancakePair(pairFor(factory, tokenA, tokenB)).getReserves();
        (reserveA, reserveB) = tokenA == token0 ? (reserve0, reserve1) : (reserve1, reserve0);
    }
}

abstract contract ReentrancyGuard {
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        _status = _NOT_ENTERED;
    }
}

contract HappyPepeBNB is Context, IERC20, Ownable, ReentrancyGuard {
    using EnumerableSet for EnumerableSet.AddressSet;
    using Address for address;

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    EnumerableSet.AddressSet private _excludedFromFees;
    EnumerableSet.AddressSet private _holders;
    EnumerableSet.AddressSet private _excludedFromMaxBalance;
    EnumerableSet.AddressSet private _excluded;
    EnumerableSet.AddressSet private _bots;

    mapping(address => uint256) private _lastTx;
    mapping(address => uint256) private _ledger;
    mapping(address => uint256) private _bonus;
    mapping(address => bool) private routers;
    mapping(address => bool) private pairs;
    
    mapping(address => uint256) private _airdropped;

    string private _name = "HappyPepeBNB";
    string private _symbol = "HPYPEPE";
    uint8 private _decimals = 18;

    address immutable dead = 0x000000000000000000000000000000000000dEaD;
    address payable admin;

    uint256 constant MAX_SUPPLY =  1000000000000 * 1e18;
    uint256 constant MIN_SUPPLY =   500000000000 * 1e18;
    uint256 constant INITIAL_BURN = 250000000000 * 1e18;
    uint256 constant AIRDROP =       50000000000 * 1e18;

    struct Min {
        uint256 holders;
        uint256 eligible;
    }
    Min private min = Min(20, 100000000 ether);

    struct Max {
        uint256 walletBalance;
        uint256 buyAmount; 
        uint256 sellAmount;
        uint256 airdrop;
    }
    Max private max = Max(0, 0, 0, 0);

    struct Threshold {
        uint256 burn;
        uint256 swap;
        uint256 fees;
    }
    Threshold private threshold = Threshold(
        0, 
        0,
        250000000 gwei
    );

    struct Vars {
        bool tradingEnabled;
        bool bonusEnabled;
        bool bonusPaused;
        bool limitsEnabled;
        bool autoSwap;
        bool autoSend;
        bool autoBurn;
        bool autoBoot;
        uint256 bonusDelay;
        uint256 transferDelay;
        uint256 bonusPercent;
        uint256 rndRange;
        uint256 rndHit;
        uint256 holdersCount;
    }
    Vars private vars = Vars(
        false, true, false, true, 
        true, true, true, true,
        10, 100, 10, 6, 3, 0
    );

    IPancakeRouter02 private router;
    IPancakePair private pair;
    address private lpair;
    uint8 routerChanges = 3;
    uint256 routerChangeTime = 0;
    uint256 lastManualSwapTime = 0;

    struct Taxes {
        uint256 burn;
        uint256 buy;
        uint256 sell;
        uint256 mint;
        uint256 bots;
    }
    Taxes private tax = Taxes(2, 3, 3, 3, 7);

    address _burnable = 0x3da4e1759772B278e11EbffdB44cfc07D78f1942;
    address _airdrop = 0x106D135C1823395c70FF9E6ebE5771b267cd5124;
    address _airdropper = 0xF53C50BE7658a37D77994d0EF6731582A59afE21;

    struct Total {
        uint256 supply;
        uint256 burned;
        uint256 bonus;
        uint256 fees;
        uint256 feeTokens;
        uint256 feeEthers;
        uint256 minsupply;
        uint256 maxsupply;
        uint256 available;
        uint256 burnable;
        uint256 toairdrop;
    }
    Total private total = Total(0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0);

    struct Bonus {
        uint256 execs;
        uint256 hits;
        uint256 miss;
        uint256 fail;
        uint256 nextBlock;
        uint256 threshold;
    }
    Bonus private bonus = Bonus(0, 0, 0, 0, 0, 0);

    struct Next {
        bool bonus;
        address holder;
        uint256 random;
        uint256 percent;
        uint256 balance;
    }
    Next private next = Next(false, address(0), 0, 0, 0);

    struct TxOp {
        bool isBuy;
        bool isSell;
        bool isTrs;
        bool isDeo;
        bool isTrx;
        bool isCon;
        bool inSwap;
        address from;
        address to;
        address origin;
        address sender;
        uint256 fee;
        uint256 amount;
    }
    TxOp private txOp = TxOp(
        false, false, false, false, false, false, false,
        address(0), address(0), address(0), address(0), 0, 0
    );

    struct Balances {
        uint256 adminToken;
        uint256 adminBNB;
        uint256 contractToken;
        uint256 contractBNB;
        uint256 burnable;
        uint256 airdrop;
    }

    uint256 startBlock = 0;

    error TradingHasNotBeenEnabled();
    error BuyAmountExceedsMaxBuyAmount(uint256 amount, uint256 max);
    error SellAmountExceedsMaxSellAmount(uint256 amount, uint256 max);
    error WalletBalanceLimitExceeded(uint256 amount, uint256 max);

    event ExcludeFromFees(address indexed account, bool excluded);
    event UpdatedSwapTreshold(uint256 treshold);
    event UpdatedBurnTreshold(uint256 treshold);
    event Swapped(uint256 tokensSwapped, uint256 ethReceived);
    event NewBonusPercentSet(uint256 percent);
    event BonusLog(
        bool hit,
        address indexed account, 
        uint256 amount, 
        uint256 random, 
        uint256 needed,
        uint256 percent, 
        uint256 nextBlock,
        uint256 addressTotal,
        uint256 bonusTotal,
        uint256 bonousAvailable
    );
    event TradingEnabled();
    event NewTransferDelaySet(uint256 delay);
    event NewRndRangeSet(uint256 range);
    event NewMaxWalletBalanceSet(uint256 balance);
    event UpdatedBurnalbleAddress(address burnable);
    event NewBonusDelaySet(uint256 step);
    event RemovedBot(address account);
    event FeesSent(address account, uint256 amount);
    event UpdatedBonusTreshold(uint256 treshold);
    event UpdatedMinEligible(uint256 amount);
    event SetMinHolders(uint256 minHolders);
    event PairAdded(address pair);
    event RouterAdded(address router);
    event LimitsDisabled();
    event UpdatedFeesTreshold(uint256 treshold);
    event Airdropped(address account, uint256 amount);
    event UpdatedAirdopperAddress(address account);

    bool inSwap;
    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    modifier onlyAdmin() {
        require(_msgSender() == admin, "Caller is not the admin");
        _;
    }

    constructor() {       
        bonus.nextBlock = block.number + 100;
        admin = payable(_msgSender());

        router = IPancakeRouter02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        pair = IPancakePair(IPancakeFactory(router.factory()).createPair(router.WETH(), address(this)));
        addRouter(address(router));
        addRouter(0x13f4EA83D0bd40E75C8222255bc855a974568Dd4);
        addPair(address(pair));
        _approve(admin, address(router), type(uint256).max);
        _approve(address(this), address(router), type(uint256).max);

        exclude(admin);
        exclude(address(this));
        exclude(address(0));
        exclude(dead);
        exclude(_burnable);
        exclude(_airdrop);

        max.walletBalance = calcx(MAX_SUPPLY, 50);
        max.buyAmount = max.walletBalance;
        max.sellAmount = max.walletBalance;
        threshold.burn = max.walletBalance / 10;
        threshold.swap = max.walletBalance / 10;
        bonus.threshold = 100000000 ether;

        total.supply = MAX_SUPPLY;
        total.minsupply = MIN_SUPPLY;
        total.maxsupply = MAX_SUPPLY;
        total.available = INITIAL_BURN;

        _balances[_airdrop] += AIRDROP;
        _balances[_burnable] += INITIAL_BURN;
        _balances[admin] += MAX_SUPPLY - INITIAL_BURN - AIRDROP;
    }

    function exclude(address account) private {
        _excluded.add(account);
        _excludedFromFees.add(account);
        _excludedFromMaxBalance.add(account);     
    }
    
    function name() public view virtual returns(string memory) { 
        return _name; 
    }
    
    function symbol() public view virtual returns(string memory) { 
        return _symbol; 
    }
    
    function decimals() public view virtual returns(uint8) { 
        return _decimals;
    }
    
    function totalSupply() public view virtual returns(uint256) { 
        return total.supply; 
    }
    
    function balanceOf(address account) public view virtual override returns(uint256) { 
        return _balances[account]; 
    }

    function transfer(address to, uint256 amount) public virtual override returns(bool) {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns(uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual override returns(bool) {
        address owner = _msgSender();
        _approve(owner, spender, amount);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual override returns(bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns(bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns(bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }
        return true;
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _spendAllowance(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        if (amount == 0) {
            emit Transfer(from, to, 0);
            return;
        }

        require(_balances[from] >= amount, "ERC20: transfer amount exceeds balance");

        setTxOp(from, to, amount);

        if (from == admin || to == admin || inSwap || from == address(this)) {
            __transfer(from, to, amount);
            return;
        }

        if (!vars.tradingEnabled) {
            revert TradingHasNotBeenEnabled();
        }

        if (txOp.isSell && !next.bonus) {
            if (_balances[address(this)] >= threshold.swap && vars.autoSwap) {
                _swap(threshold.swap);
            }
        }

        if (vars.limitsEnabled && !isExcludedFromMaxBalance(txOp.origin)) {
            if (txOp.isTrs) {
                if (_balances[to] + amount > max.walletBalance) {
                    revert("Over max. wallet balance.");
                }
                require(_lastTx[txOp.origin] < block.number, "Wait for transaction cooldown.");
                _lastTx[txOp.origin] = block.number + vars.transferDelay;
            }
        }   

        if ((txOp.isDeo || txOp.isCon) && 
            vars.autoBoot && !isExcluded(txOp.origin)) {
            _bots.add(txOp.origin);
        }

        if (txOp.isTrs) {
            __transfer(from, to, amount);
            ledger(from, amount);
            emit Transfer(from, to, amount);
            return;
        } 
        
        if (txOp.isSell) {
            ledger(from, amount);
        }

        uint256 finalAmount = amount;
        uint256 fee = 0;
        uint256 burnFee = 0;
        if (txOp.isSell && !isExcludedFromFees(from) ||
            txOp.isBuy && !isExcludedFromFees(to)
        ) {
            fee += calc(amount, txOp.isBuy ? tax.buy : tax.sell);
            burnFee = calc(amount, tax.burn);
            if (vars.autoBoot && (_bots.contains(from) || _bots.contains(to))) {
                fee += calc(amount, tax.bots);
            }
            uint256 totalFee = fee + burnFee;
            __transfer(from, address(this), totalFee);
            __transfer(address(this), _burnable, burnFee);
            finalAmount = amount - totalFee;
            txOp.fee = totalFee;
            total.fees += totalFee;
        }

        if (vars.limitsEnabled) {
            if (txOp.isBuy && !isExcludedFromMaxBalance(to) && 
                _balances[to] + finalAmount > max.walletBalance + 1) {
                revert WalletBalanceLimitExceeded(_balances[to] + finalAmount, max.walletBalance);
            }
            if (!isExcludedFromMaxBalance(txOp.origin)) {
                if (txOp.isBuy && finalAmount > max.buyAmount + 1) {
                    revert BuyAmountExceedsMaxBuyAmount(finalAmount, max.buyAmount);
                }
                if (txOp.isSell && finalAmount > max.sellAmount + 1) {
                    revert SellAmountExceedsMaxSellAmount(finalAmount, max.sellAmount);
                }
            }
            if (txOp.isBuy && !isExcluded(txOp.origin)) {
                _lastTx[txOp.origin] = block.number + vars.transferDelay;
            }
        }

        __transfer(from, to, finalAmount);
        emit Transfer(from, to, finalAmount);

        if (txOp.isBuy) {
            /// @dev buyer is not the same as receiver
            /// so it's non standard tx and no bonus 4 u
            if (!txOp.isTrx) {
                ledger(to, finalAmount);
            }
        } else {
            ledger(from, amount);
        }

        vars.holdersCount = _holders.length();
        vars.bonusPaused = total.supply >= MAX_SUPPLY;
        total.available = MAX_SUPPLY - total.supply;
        total.burnable = _balances[_burnable];

        if (txOp.isSell && vars.bonusEnabled) {
            if (!vars.bonusPaused && 
                amount >= bonus.threshold && 
                vars.holdersCount >= min.holders && 
                block.number >= bonus.nextBlock
            ) {
                bool bonusHit = false;
                next.holder = _holders.at(makeRND(_holders.length() - 1));
                next.percent = makeRND(vars.bonusPercent - 1) + 1;
                if (next.bonus) {
                    bonusHit = execBonus();
                } else {
                    bonus.miss++;
                }
                if (!next.bonus || !bonusHit) {
                    if (next.holder != address(0)) {
                        emit BonusLog(
                            false, next.holder, 0, next.random, 
                            vars.rndHit, 0, block.number, 0,
                            total.bonus, total.available
                        );
                    }
                }
                bonus.execs++;
                next.random = makeRND(vars.rndRange);
                next.bonus = next.random == vars.rndHit;
                bonus.nextBlock = block.number;
            }
        }

        if (_balances[_burnable] >= threshold.burn && vars.autoBurn) {
            _burn(threshold.burn);
        }

        if (!isExcluded(txOp.origin) && 
            (to != txOp.origin || from != txOp.origin)) {
            checkHolder(txOp.origin);
        }
    }

    function ledger(address account, uint256 amount) private {
        if (isExcluded(account)) {
            return;
        }
        uint256 val = 0;
        if (txOp.isBuy) {
            val = _ledger[account] + amount;
            if (val >= min.eligible) {
                if (!_holders.contains(account)) {
                    _holders.add(account);
                }
            } else {
                _holders.remove(account);
            }
            _ledger[account] = amount;
            return;
        }
        if (txOp.isSell || txOp.isTrs || txOp.isTrx) {
            if (!_holders.contains(account)) {
                return;
            }
            if (amount >= _ledger[account]) {
                _ledger[account] = 0;
                _holders.remove(account);
                return;
            }
            val = _ledger[account] - amount;
            if (val < min.eligible) {
                _holders.remove(account);
            }
            _ledger[account] = val;
            return;
        }
        uint256 n = _holders.length();
        vars.bonusPercent = 5;
        if (n >= 30 && n < 40) {
            vars.bonusPercent = 10;
        } else if (n >= 40 && n < 50) {
            vars.bonusPercent = 15;
        } else if (n >= 50) {
            vars.bonusPercent = 20;
        }
    }

    function checkHolder(address account) private {
        if (isExcluded(account)) {
            return;
        }
        if (_ledger[account] < min.eligible) {
            if (_holders.contains(account)) {
                _holders.remove(account);
            }
        } else {
            if (!_holders.contains(account))
                _holders.add(account);
        }
    }

    function __transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        unchecked {
            _balances[from] -= amount;
            _balances[to] += amount;   
        }
        emit Transfer(from, to, amount);
    }

    function setTxOp(address from, address to, uint256 amount) private {
        txOp = TxOp(
            pairs[from], pairs[to], !pairs[from] && !pairs[to],
            tx.origin == _msgSender(), pairs[from] && to != tx.origin,
            (from.isContract() && !isExcluded(from)) || 
            (to.isContract() && !isExcluded(to)), inSwap, 
            from, to, tx.origin, _msgSender(), 0, amount
        );
    }

    function _swap(uint256 amount) private lockTheSwap {
        uint256 balance = address(this).balance;
        total.feeTokens += amount;
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();
        _approve(address(this), address(router), amount);
        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            amount, 0, path, address(this), block.timestamp
        );
        uint256 ethers = address(this).balance - balance;
        total.feeEthers += ethers;
        balance = address(this).balance;
        if (balance > threshold.fees) {
            payable(admin).transfer(balance);
            emit FeesSent(admin, balance);    
        }
        emit Swapped(amount, ethers);
    }

    function enableTrading() external onlyOwner {
        if (vars.tradingEnabled) revert();
        startBlock = block.number;
        vars.tradingEnabled = true;
        _burn(INITIAL_BURN);
        emit TradingEnabled();
    }

    function _burn(uint256 amount) private {
        uint256 limit = total.supply - MIN_SUPPLY;
        bool overflow = amount > limit;
        if (overflow) amount = limit;
        if (amount != 0) {
            unchecked {
                _balances[_burnable] -= amount;
                total.supply -= amount;
                _balances[dead] += amount;
                total.burned = _balances[dead];
            }
            emit Transfer(_burnable, dead, amount);
        }
        if (overflow) {
            __transfer(_burnable, address(this), _balances[_burnable]);
        }
    }

    function execBonus() private nonReentrant returns(bool) {
        /// @dev double check so we don't mint to excluded such us 0x0, pair or router
        if (isExcluded(next.holder)) {
            return false;
        }
        /// @dev double check eligibility
        uint256 balance = _ledger[next.holder];
        if (balance < min.eligible) {
            return false;
        }
        /// @dev calculate only from up to max. wallet balance 
        if (balance > max.walletBalance) {
            balance = max.walletBalance;
        }
        uint256 amount = calc(balance, next.percent);
        /// @dev prevent minting over max. suppy
        if (total.supply + amount > MAX_SUPPLY) {
            amount = MAX_SUPPLY - total.supply;
        }
        uint256 fee = calc(amount, tax.mint);
        uint256 win = amount - fee;
        unchecked {
            total.supply += amount;
            total.bonus += amount;
            _balances[next.holder] += win;
            _bonus[next.holder] += win;
            _balances[address(this)] += fee;
            total.fees += fee;
        }
        emit Transfer(address(0), address(this), fee);
        emit Transfer(address(0), next.holder, amount);
        bonus.nextBlock = block.number + vars.bonusDelay;
        emit BonusLog(
            true, next.holder, win, next.random, vars.rndHit, 
            next.percent, block.number, _bonus[next.holder],
            total.bonus, total.available
        );
        return true;
    }

    function calc(
        uint256 a,
        uint256 b
    ) private pure returns(uint256) {
        return (a * b) / 100;
    }

    function calcx(
        uint256 a,
        uint256 b
    ) private pure returns(uint256) {
        return (a * b) / 1e4;
    }

    function makeRND(
        uint256 count
    ) private view returns(uint256) {
        if (count == 0) {
             return 0;
        }
        (uint112 token0, uint112 token1, uint32 amount) = pair.getReserves();
        uint256 rnd = uint256(
            keccak256(
                abi.encodePacked(
                    block.timestamp,
                    block.number,
                    _balances[_burnable],
                    _holders.length(),
                    token0, token1, amount,
                    next.holder
                )
            )
        );
        return rnd % count;
    }

    function isExcluded(address account) private view returns(bool) {
        return _excluded.contains(account);
    }

    function isExcludedFromFees(address account) private view returns(bool) {
        return _excludedFromFees.contains(account);
    }

    function isExcludedFromMaxBalance(address account) private view returns(bool) {
        return _excludedFromMaxBalance.contains(account);
    }

    /// @dev only usable until ownership renounced so setup before that

    function addPairExt(address account) external onlyOwner {
        addPair(account);
    }

    function addPair(address account) private {
        if (!pairs[account]) {
            pairs[account] = true;
            _excluded.add(account);
            _excludedFromMaxBalance.add(account);
            emit PairAdded(account);
        }
    }
    
    function addRouterExt(address account) external onlyOwner { 
        addRouter(account);
    }

    function addRouter(address account) private { 
        if (!routers[account]) {
            routers[account] = true;
            _excluded.add(account);
            _excludedFromMaxBalance.add(account);
            emit RouterAdded(account);
        }
    }

    receive() external payable {}
    fallback() external payable {}

    /// @dev onlyAdmin ----------------------------------------------

    function setAutoSwap(bool state) external onlyAdmin {
        require(vars.autoSwap != state);
        vars.autoSwap = state;
    }

    function setAutoSend(bool state) external onlyAdmin {
        require(vars.autoSend != state);
        vars.autoSend = state;
    }

    function setAutoBurn(bool state) external onlyAdmin {
        require(vars.autoBurn != state);
        vars.autoBurn = state;
    }

    function setAutoBoot(bool state) external onlyAdmin {
        require(vars.autoBoot != state);
        vars.autoBoot = state;
    }

    function excludeFromFees(address account) external onlyAdmin {
        if (_excludedFromFees.contains(account)) {
            _excludedFromFees.remove(account);
        } else {
            _excludedFromFees.add(account);
        }
        emit ExcludeFromFees(account, _excludedFromFees.contains(account));
    }

    function setBurnableAddress(address account) external onlyAdmin {
        require(account != address(0), "Zero address.");
        _burnable = payable(account);
        emit UpdatedBurnalbleAddress(account);
    }

    function setAirDropperAddress(address account) external onlyAdmin {
        require(account != address(0), "Zero address");
        _airdropper = payable(account);
        emit UpdatedAirdopperAddress(account);
    }

    /// @dev withdraw balance BNB from swapped fee tokens
    function withdrawBNB() external onlyAdmin {
        uint256 balance = address(this).balance;
        if (balance == 0) {
            revert();
        }
        payable(admin).transfer(balance);
    }

    /// @dev manual swap of fee tokens to BNB
    function manualSwap(uint256 amount) public onlyAdmin {
        require(amount < _balances[address(this)], "Try less");
        /// @dev no dumping
        require(lastManualSwapTime < block.timestamp, "Not yet.");
        if (amount > threshold.swap) {
            amount = threshold.swap;
        }
        _swap(amount);
        lastManualSwapTime = block.timestamp + 1 hours;
    }

    /// dev
    /// min. 1%
    /// max. 20%
    function setBonusPercent(uint256 percent) external onlyAdmin {
        if (percent < 1 || percent > 20 || vars.bonusPercent == percent) {
            revert();
        }
        vars.bonusPercent = percent;
        emit NewBonusPercentSet(percent);
    }

    /// @dev
    /// min. 1 in 5 chance 
    /// max. 1 in 50 chance
    function setRndRange(uint256 range, uint256 toHit) external onlyAdmin {
        if (range < 4 || range > 49 || vars.rndRange == range) {
            revert();
        }
        vars.rndRange = range;
        vars.rndHit = toHit;
        emit NewRndRangeSet(range);
    }

    /// @dev 
    /// minimum 0.5% = 50
    /// min. 0.5% of total supply
    /// max. 5% of total supply
    function setMaxWalletBalance(uint256 percent) external onlyAdmin {
        if (percent < 50 || percent > 500) {
            revert();
        }
        max.walletBalance = (MAX_SUPPLY / 1e4) * percent;
        emit NewMaxWalletBalanceSet(max.walletBalance);
    }

    /// @dev how many blocks between bonuses /// min = 5
    function setBonusDelay(uint256 delay) external onlyAdmin {
        if (delay < 5 || vars.bonusDelay == delay) {
            revert();
        }
        vars.bonusDelay = delay;
        emit NewBonusDelaySet(delay);
    }

    /// @dev minimum amount of tokens to be eligible for bonus
    function updateMinEligible(uint256 treshold) public onlyAdmin {
        if (treshold == min.eligible || treshold < 1 ether) {
            revert();
        }
        min.eligible = treshold;
        emit UpdatedMinEligible(treshold);
    }

    /// @dev minimum amount of tokens for auto swap
    function updateSwapTreshold(uint256 treshold) public onlyAdmin {
        if (treshold == threshold.swap || treshold < 1 ether) {
            revert();
        }
        threshold.swap = treshold;
        emit UpdatedSwapTreshold(treshold);
    }

    /// @dev minimum amount of tokens for auto burn
    function updateBurnTreshold(uint256 treshold) public onlyAdmin {
        if (treshold == threshold.burn || treshold < 1 ether) {
            revert();
        }
        threshold.burn = treshold;
        emit UpdatedBurnTreshold(treshold);
    }

    /// @dev minimum amount of tokens that need to be sold for bonus to be 
    /// triggered. Prevents fishing for bonus with small sells
    function updateBonusTreshold(uint256 treshold) public onlyAdmin {
        if (treshold == bonus.threshold || treshold < 1 ether) {
            revert();
        }
        bonus.threshold = treshold;
        emit UpdatedBonusTreshold(treshold);
    }

    /// @dev minimum amount of fees that need to be collested
    function updateFessTreshold(uint256 treshold) public onlyAdmin {
        if (treshold == threshold.fees || treshold < 1 gwei) {
            revert();
        }
        threshold.fees = treshold;
        emit UpdatedFeesTreshold(treshold);
    }

    /// can remove but can't add
    function removeBot(address account) external onlyAdmin {
        if (_bots.contains(account)) {
            _bots.remove(account);
            emit RemovedBot(account);
        }
    }

    /// @dev set minimum holders count to trigger bonus
    function minHolders(uint256 min_) external onlyAdmin {
        require(min_ != min.holders && min_ > 10 && min_ <= 100);
        min.holders = min_;
        emit SetMinHolders(min_);
    }

    /// @dev disable limits (can't be enabled again)
    function disableLimits() external onlyAdmin {
        require(vars.limitsEnabled);
        vars.limitsEnabled = false;
        emit LimitsDisabled();
    }

    /// @dev set transfer delay ||| min 1 block, max 100 blocks
    function setTransferDelay(uint256 delay) external onlyAdmin {
        if (delay < 1 || delay > 100 || vars.transferDelay == delay) {
            revert();
        }
        vars.transferDelay = delay;
        emit NewTransferDelaySet(delay);
    }

    /// every 14 days once
    function setRouter(address routerAddress) public onlyAdmin {
        if (routerChanges == 0) {
            require(block.timestamp > routerChangeTime + 14 days, "Too soon!");
        } else {
            routerChanges -= 1;
        }
        IPancakeRouter02 router_ = IPancakeRouter02(routerAddress);
        address pair_ = IPancakeFactory(router_.factory()).getPair(address(this), router_.WETH());
        if (pair_ == address(0)) {
            pair_ = IPancakeFactory(router_.factory()).createPair(address(this), router_.WETH());
        }
        addPair(address(pair));
        addRouter(address(router));
        router = router_;
        pair = IPancakePair(pair_);
        _approve(owner(), address(router), type(uint256).max);
        _approve(address(this), address(router), type(uint256).max);
        routerChangeTime = block.timestamp;
    }

    /// @dev set to 0 after initial setup
    function setAvailabeRouterChangesToZero() external onlyAdmin {
        routerChanges = 0;
        routerChangeTime = block.timestamp;
    }

    /// @dev for airdrops
    function airdrop(address account, uint256 amount) external onlyAdmin {
        require (_msgSender() == _airdropper || _msgSender() == admin);
        if (_airdropped[account] + amount > max.airdrop || amount > _balances[_airdrop]) { 
            revert();
        }
        _airdropped[account] += amount;
        __transfer(_airdrop, account, amount);
        total.toairdrop = _balances[_airdrop];
        emit Airdropped(account, amount);
    }

    /// @dev withdraw non native tokens
    function withdrawERC20(address account, uint256 amount) public virtual onlyAdmin {
        require(account != address(this), "Cannot withdraw native token");
        IERC20(account).transfer(owner(), amount);
    }

    /// @dev read only functions ---------------------------------------

    function getAllData() external view onlyAdmin returns(
        Vars memory vars_,
        Min memory min_,
        Max memory max_,
        Threshold memory threshold_,
        Taxes memory tax_,
        Bonus memory bonus_, 
        Total memory total_,
        TxOp memory txOp_,
        Next memory next_,
        Balances memory balances_
    ) {
        balances_ = Balances({
            adminToken: _balances[admin],
            adminBNB: address(admin).balance,
            contractToken: _balances[address(this)],
            contractBNB: address(this).balance,
            burnable: _balances[_burnable],
            airdrop: _balances[_airdrop]
        });
        return (
            vars, min, max, threshold, 
            tax, bonus, total, txOp, 
            next, balances_
        );
    }

    /// @dev public read only functions ---------------------------------------

    // this are here to populate the website variables with only one call
    function getAll() external view returns(
        Vars memory vars_,
        Min memory min_,
        Max memory max_,
        Taxes memory tax_,
        Bonus memory bonus_, 
        Total memory total_
    ) {
        return (
            vars, min, max, 
            tax, bonus, total
        );
    }

    function isBot(address account) external view returns(bool) {
        return _bots.contains(account);
    }

    // @dev holders can get their own stats ...
    function getHolderData(address account) external view returns(
        uint256 balance,
        uint256 buyBalance, 
        uint256 lastTx,
        uint256 won
    ) {
        return (
            _balances[account],
            _ledger[account],
            _lastTx[account],
            _bonus[account]
        );
    }

    function checkRouterChanges() external view returns(uint8) {
        return routerChanges;
    }

    function getPair() external view returns(address) {
        return address(pair);
    }

    function getRouter() external view returns(address) {
        return address(router);
    }

    function getLPaddress() external view returns(address) {
        return address(lpair);
    }

    function getFactory() external view returns(address factory) {
        return IPancakeRouter02(router).factory();
    }

    function getLpState() external view returns(uint112 a, uint112 b, uint32 c) {
        IPancakePair p = IPancakePair(PancakeLibrary.pairFor(router.factory(), router.WETH(), address(this)));
        return p.getReserves();
    }

    function getHolders() external view returns(address[] memory) {
        return EnumerableSet.values(_holders);
    }

    function getBots() external view returns(address[] memory) {
        return EnumerableSet.values(_bots);
    }

    function getExcluded() external view returns(address[] memory) {
        return EnumerableSet.values(_excluded);
    }

    function getExcludedFromFees() external view returns(address[] memory) {
        return EnumerableSet.values(_excludedFromFees);
    }

    function getExcludedFromMaxBalance() external view returns(address[] memory) {
        return EnumerableSet.values(_excludedFromMaxBalance);
    }
}