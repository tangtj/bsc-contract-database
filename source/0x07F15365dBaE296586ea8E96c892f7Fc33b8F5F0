/*
 * LUSD (LUSD)
 *
 * LUSD is a cryptocurrency stable coin.
 * The value of LUSD is pegged to the US Dollar (USD).
 * LUSD was designed to tokenize the US dollar
 * to be used for blockchain transactions and
 * as a vehicle for yield and rewards.
 *
 * Website: https://legacyfinancial.io/
 * dApp: https://dapp.legacyfinancial.io/
 * Twitter: https://twitter.com/LegacyCapital_
 * Telegram: https://t.me/legacycapital
 *
 * By Studio L, a Legacy Capital Division
 * https://legacycapital.cc/StudioL/
*/
// 999,999,999 Optimization Runs
// SPDX-License-Identifier: MIT


pragma solidity = 0.8.19;

/// @title The interface for the Uniswap V3 Factory
/// @notice The Uniswap V3 Factory facilitates creation of Uniswap V3 pools and control over the protocol fees
interface IFactoryV3 {
    /// @notice Emitted when the owner of the factory is changed
    /// @param oldOwner The owner before the owner was changed
    /// @param newOwner The owner after the owner was changed
    event OwnerChanged(address indexed oldOwner, address indexed newOwner);

    /// @notice Emitted when a pool is created
    /// @param token0 The first token of the pool by address sort order
    /// @param token1 The second token of the pool by address sort order
    /// @param fee The fee collected upon every swap in the pool, denominated in hundredths of a bip
    /// @param tickSpacing The minimum number of ticks between initialized ticks
    /// @param pool The address of the created pool
    event PoolCreated(
        address indexed token0,
        address indexed token1,
        uint24 indexed fee,
        int24 tickSpacing,
        address pool
    );

    /// @notice Emitted when a new fee amount is enabled for pool creation via the factory
    /// @param fee The enabled fee, denominated in hundredths of a bip
    /// @param tickSpacing The minimum number of ticks between initialized ticks for pools created with the given fee
    event FeeAmountEnabled(uint24 indexed fee, int24 indexed tickSpacing);

    /// @notice Returns the current owner of the factory
    /// @dev Can be changed by the current owner via setOwner
    /// @return The address of the factory owner
    function owner() external view returns (address);

    /// @notice Returns the tick spacing for a given fee amount, if enabled, or 0 if not enabled
    /// @dev A fee amount can never be removed, so this value should be hard coded or cached in the calling context
    /// @param fee The enabled fee, denominated in hundredths of a bip. Returns 0 in case of unenabled fee
    /// @return The tick spacing
    function feeAmountTickSpacing(uint24 fee) external view returns (int24);

    /// @notice Returns the pool address for a given pair of tokens and a fee, or address 0 if it does not exist
    /// @dev tokenA and tokenB may be passed in either token0/token1 or token1/token0 order
    /// @param tokenA The contract address of either token0 or token1
    /// @param tokenB The contract address of the other token
    /// @param fee The fee collected upon every swap in the pool, denominated in hundredths of a bip
    /// @return pool The pool address
    function getPool(
        address tokenA,
        address tokenB,
        uint24 fee
    ) external view returns (address pool);

    /// @notice Creates a pool for the given two tokens and fee
    /// @param tokenA One of the two tokens in the desired pool
    /// @param tokenB The other of the two tokens in the desired pool
    /// @param fee The desired fee for the pool
    /// @dev tokenA and tokenB may be passed in either order: token0/token1 or token1/token0. tickSpacing is retrieved
    /// from the fee. The call will revert if the pool already exists, the fee is invalid, or the token arguments
    /// are invalid.
    /// @return pool The address of the newly created pool
    function createPool(
        address tokenA,
        address tokenB,
        uint24 fee
    ) external returns (address pool);

    /// @notice Updates the owner of the factory
    /// @dev Must be called by the current owner
    /// @param _owner The new owner of the factory
    function setOwner(address _owner) external;

    /// @notice Enables a fee amount with the given tickSpacing
    /// @dev Fee amounts may never be removed once enabled
    /// @param fee The fee amount to enable, denominated in hundredths of a bip (i.e. 1e-6)
    /// @param tickSpacing The spacing between ticks to be enforced for all pools created with the given fee amount
    function enableFeeAmount(uint24 fee, int24 tickSpacing) external;
}



pragma solidity = 0.8.19;

interface IPairV2 {
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
}



pragma solidity = 0.8.19;

interface IDexRouterV2 {
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
    ) external returns (uint256 amountA, uint256 amountB, uint256 liquidity);

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
        returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);

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

    function getAmountsOut(
        uint256 amountIn,
        address[] calldata path
    ) external view returns (uint256[] memory amounts);

    function getAmountsIn(
        uint256 amountOut,
        address[] calldata path
    ) external view returns (uint256[] memory amounts);

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




pragma solidity = 0.8.19;

interface IFactoryV2 {
    event PairCreated(address indexed token0, address indexed token1, address lpPair, uint);
    function createPair(address tokenA, address tokenB) external returns (address lpPair);
    function getPair(address tokenA, address tokenB) external view returns (address lpPair);
}



pragma solidity = 0.8.19;

// helper methods for interacting with ERC20 tokens and sending ETH that do not consistently return true/false
library TransferHelper {
    function safeApprove(
        address token,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('approve(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x095ea7b3, to, value));
        require(
            success && (data.length == 0 || abi.decode(data, (bool))),
            'TransferHelper::safeApprove: approve failed'
        );
    }

    function safeTransfer(
        address token,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(
            success && (data.length == 0 || abi.decode(data, (bool))),
            'TransferHelper::safeTransfer: transfer failed'
        );
    }

    function safeTransferFrom(
        address token,
        address from,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(
            success && (data.length == 0 || abi.decode(data, (bool))),
            'TransferHelper::transferFrom: transferFrom failed'
        );
    }

    function safeTransferETH(address to, uint256 value) internal {
        (bool success, ) = to.call{value: value}(new bytes(0));
        require(success, 'TransferHelper::safeTransferETH: ETH transfer failed');
    }
}


// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)

pragma solidity = 0.8.19;

/**
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
        return msg.data;
    }
}


// OpenZeppelin Contracts (last updated v4.9.0) (access/Ownable.sol)

pragma solidity = 0.8.19;


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
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}


// OpenZeppelin Contracts (last updated v4.9.0) (access/Ownable2Step.sol)

pragma solidity = 0.8.19;


/**
 * @dev Contract module which provides access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner account will be the one that deploys the contract. This
 * can later be changed with {transferOwnership} and {acceptOwnership}.
 *
 * This module is used through inheritance. It will make available all functions
 * from parent (Ownable).
 */
abstract contract Ownable2Step is Ownable {
    address private _pendingOwner;

    event OwnershipTransferStarted(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Returns the address of the pending owner.
     */
    function pendingOwner() public view virtual returns (address) {
        return _pendingOwner;
    }

    /**
     * @dev Starts the ownership transfer of the contract to a new account. Replaces the pending transfer if there is one.
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual override onlyOwner {
        _pendingOwner = newOwner;
        emit OwnershipTransferStarted(owner(), newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`) and deletes any pending owner.
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual override {
        delete _pendingOwner;
        super._transferOwnership(newOwner);
    }

    /**
     * @dev The new owner accepts the ownership transfer.
     */
    function acceptOwnership() public virtual {
        address sender = _msgSender();
        require(pendingOwner() == sender, "Ownable2Step: caller is not the new owner");
        _transferOwnership(sender);
    }
}


// OpenZeppelin Contracts (last updated v4.9.0) (security/ReentrancyGuard.sol)

pragma solidity = 0.8.19;

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
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _status will be _NOT_ENTERED
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Returns true if the reentrancy guard is currently set to "entered", which indicates there is a
     * `nonReentrant` function in the call stack.
     */
    function _reentrancyGuardEntered() internal view returns (bool) {
        return _status == _ENTERED;
    }
}

/*
 * LUSD (LUSD)
 *
 * LUSD is a cryptocurrency stable coin.
 * The value of LUSD is pegged to the US Dollar (USD).
 * LUSD was designed to tokenize the US dollar
 * to be used for blockchain transactions and
 * as a vehicle for yield and rewards.
 *
 * Website: https://legacyfinancial.io/
 * dApp: https://dapp.legacyfinancial.io/
 * Twitter: https://twitter.com/LegacyCapital_
 * Telegram: https://t.me/legacycapital
 *
 * By Studio L, a Legacy Capital Division
 * https://legacycapital.cc/StudioL/
*/
// 999,999,999 Optimization Runs


pragma solidity = 0.8.19;










interface IERC20 {
  function totalSupply() external view returns (uint256);
  function decimals() external view returns (uint8);
  function symbol() external view returns (string memory);
  function name() external view returns (string memory);
  function balanceOf(address account) external view returns (uint256);
  function transfer(address recipient, uint256 amount) external returns (bool);
  function allowance(address _owner, address spender) external view returns (uint256);
  function approve(address spender, uint256 amount) external returns (bool);
  function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
  event Transfer(address indexed from, address indexed to, uint256 value);
  event Approval(address indexed owner, address indexed spender, uint256 value);
}

/**
 * @dev 999,999,999 optimization runs.
*/
contract Legacy_Stable_1_02 is IERC20, ReentrancyGuard, Ownable2Step {

//Token Variables
    string constant private _name = "Legacy USD";
    string constant private _symbol = "LUSD";

    uint64 constant private startingSupply = 1_000; //1 Thousand, underscores aid readability

    uint8 constant private _decimals = 18;
    uint256 constant private MAX = ~uint256(0);

    uint256 private _tTotal = startingSupply * (10 ** _decimals);
    uint256 private _circSupply = _tTotal;

    address public constant DEAD = 0x000000000000000000000000000000000000dEaD;

    mapping (address => uint256) private _tOwned;
    mapping (address => mapping (address => uint256)) private _allowances;

//Treasury Variables

    address private treasury;

    address private pathCoin0;
    address private pathCoin1;

    address[] private refPath = [pathCoin0, pathCoin1];
    uint256[] private amounts = [0, 0];

    event TreasurySet(address Setter, address indexed OldTreasury, address indexed NewTreasury);
    event ReferencePathSet(address Setter, address indexed OldCoin0, address NewCoin0, address indexed OldCoin1, address NewCoin1);
    event PoolSynced(address Caller, address PairedCoin, address PoolAddress, bool nativeORstable, uint256 BlockStamp );

//Router, LP Pair Variables

    //Routers
    address private v2Router;

    event V2RouterSet(address Setter, address indexed OldV2Router, address NewV2Router);


    address[] private dexRouters;

    mapping (address => bool) private isDexRouter;

    mapping(address => address) private factoryCAperDex;

    mapping(address => bool) private enableAggregateDex;

    mapping(address => uint32) private dexTradingPauseTime;
    mapping(address => uint32) private dexTradingPausedTimestamp;

    //LP Pairs
    address private poolAddr;

    LPool[] private liqPoolList;
    struct LPool {
        address poolCA;
        address pairedCoinCA;
        address dexCA;
        bool V2orV3;
        bool pairedIsStable;
        bool tradingEnabled;
        bool liqAdded;
        uint8 pairedCoinDec;
        uint24 v3Fee;
        uint32 tradingPauseTime;
        uint32 tradingPausedTimestamp;
    }
    mapping (address => bool) private isLiqPool;

    uint32 constant private maxTradePauseTime = 28 days;

    event NewDexRouter(address DexRouterCA);
    event NewLPCreated(address DexRouterCA, address LPCA, address PairedCoinCA);
    event DexRouterEnabled(address Setter, address DexRouterCA, uint256 EnabledBlock, uint256 EnabledTime);
    event DexRouterPaused(address Setter, address DexRouterCA, uint256 PausedBlock, uint32 PauseTime, uint256 DisabledTimestamp);
    event PoolEnabled(address Setter, address LPool, uint256 EnabledBlock, uint256 EnabledTime);
    event PoolPaused(address Setter, address PoolCA, uint256 PausedBlock, uint32 PauseTime, uint256 DisabledTimestamp);

    mapping (address => bool) private _isExcludedFromLimits;

//===============================================================================================================
//Constructor

    constructor() {

        _isExcludedFromLimits[_msgSender()] = true;
        _isExcludedFromLimits[address(this)] = true;

        _tOwned[ _msgSender() ] = _tTotal;

        emit Transfer(address(0), _msgSender(), _tTotal);
    }

    // ============================================== Modifiers ==============================================

    modifier onlyTreasury() {
        require(_msgSender() == treasury, "StudioL_Token: caller is not the treasury");
        _;
    }

//===============================================================================================================
//Override Functions

    function totalSupply() external view override returns (uint256) { return _tTotal; }
    function decimals() external pure override returns (uint8) { return _decimals; }
    function symbol() external pure override returns (string memory) { return _symbol; }
    function name() external pure override returns (string memory) { return _name; }
    function allowance(address holder, address spender) external view override returns (uint256) { return _allowances[holder][spender]; }

    function balanceOf(address account) external view override returns (uint256) {
        return _tOwned[account];
    }

    function transfer(address recipient, uint256 amount) external override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        if (_allowances[sender][_msgSender()] != type(uint256).max) {
            _approve( sender, _msgSender(), (_allowances[sender][_msgSender()] - amount) );
        }
        _transfer(sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint256 amount) external override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function _approve(address sender, address spender, uint256 amount) internal {
        require(sender != address(0), "ERC20: Zero Address");
        require(spender != address(0), "ERC20: Zero Address");

        _allowances[sender][spender] = amount;
        emit Approval(sender, spender, amount);
    }

//===============================================================================================================
//Common Functions

    function rescueStuckAssets(bool ethOrToken, address tokenCA, uint256 amt, address receivable) external onlyOwner {
        require(amt <= contractBalanceInWei(ethOrToken, tokenCA));
        if (ethOrToken){
            TransferHelper.safeTransferETH(receivable, amt);
        } else {
            TransferHelper.safeTransfer(tokenCA, receivable, amt);
        }
    }

    function contractBalanceInWei(bool ethOrToken, address tokenCA) public view returns (uint256) {
        if (ethOrToken){
            return address(this).balance;
        } else {
            return IERC20(tokenCA).balanceOf(address(this));
        }
    }

    receive() payable external {}

    function multiSendTokens(address[] calldata accounts, uint256[] calldata amountsInWei) external onlyOwner {
        require(accounts.length == amountsInWei.length, "StudioL_Token: Lengths do not match.");
        for (uint8 i = 0; i < accounts.length; i++) {
            require(_tOwned[ _msgSender() ] >= amountsInWei[i]);
            _transfer(_msgSender(), accounts[i], amountsInWei[i]);
        }
    }

    function isContract(address account) internal view returns (bool) {
        // This method relies on extcodesize/address.code.length, which returns 0
        // for contracts in construction, since the code is only stored at the end
        // of the constructor execution.

        return account.code.length > 0;
    }

//===============================================================================================================
//Dex Router and LPool Manager Functions

    function setV2Router(address _v2Router) external onlyOwner {
        require(_v2Router != address(0), "StudioL_Token: zero router address.");
        require(v2Router != _v2Router, "StudioL_Token: already set to the desired address");
        emit V2RouterSet( _msgSender(), v2Router, _v2Router);
        v2Router = _v2Router;
    }

    function getV2Router() external view returns (address) {
        return v2Router;
    }

    function setNewRouterAndPool(address _router, bool _V2orV3, uint24 _v3Fee, address _LPTargetCoinCA, bool _pairedIsStable) external onlyOwner {
        if (isDexRouter[_router] == false) {

            dexRouters.push(_router);

            factoryCAperDex[_router] = IDexRouterV2(_router).factory();
            enableAggregateDex[_router] = true;

            isDexRouter[_router] = true;

            _isExcludedFromLimits[_router] = true;

            _allowances[ _msgSender() ][_router] = type(uint256).max;
            _allowances[_router][ _msgSender() ] = type(uint256).max;
            _allowances[ owner() ][_router] = type(uint256).max;
            _allowances[_router][ owner() ] = type(uint256).max;
            _allowances[ address(this) ][_router] = type(uint256).max;
            _allowances[_router][ address(this) ] = type(uint256).max;

            emit NewDexRouter(_router);
        }

        address lpCA;
        if(_V2orV3) {

            lpCA = IFactoryV2( factoryCAperDex[_router] ).getPair( _LPTargetCoinCA, address(this) );

            require(!isLiqPool[lpCA], "StudioL_Token: Pair already exists!");

            lpCA = IFactoryV2( factoryCAperDex[_router] ).createPair( _LPTargetCoinCA, address(this) );

        } else {

            require(_v3Fee > 0, "StudioL_Token: Uniswap v3 fee must be greater than 0!");

            lpCA = IFactoryV3( factoryCAperDex[_router] ).getPool( _LPTargetCoinCA, address(this), _v3Fee );

            require(!isLiqPool[lpCA], "StudioL_Token: Pool already exists!");

            lpCA = IFactoryV3( factoryCAperDex[_router] ).createPool( _LPTargetCoinCA, address(this), _v3Fee );
        }

        liqPoolList.push( LPool(
            lpCA,
            _LPTargetCoinCA,
            _router,
            _V2orV3,
            _pairedIsStable,
            false,
            false,
            IERC20(_LPTargetCoinCA).decimals(),
            _v3Fee,
            0,
            0
        ) );

        isLiqPool[lpCA] = true;

        _allowances[ lpCA ][_router] = type(uint256).max;
        _allowances[_router][ lpCA ] = type(uint256).max;

        emit NewLPCreated(_router, lpCA, _LPTargetCoinCA);
    }

    function setDexRouterTradingStatus(address _router, bool _switch, uint32 pauseTimeInSecs) external onlyOwner {
        require(_router != address(0), "StudioL: zero router address");

        if(_switch) {
            require(!enableAggregateDex[_router], "StudioL_Token: trading already enabled.");

            enableAggregateDex[_router] = true;

            if(dexTradingPauseTime[_router] != 0) {
                
                dexTradingPauseTime[_router] = 0;
            }

            emit DexRouterEnabled(_msgSender(), _router, block.number, block.timestamp);
        } else {
            require(pauseTimeInSecs <= maxTradePauseTime, "StudioL_Token: cannot pause longer than max trade pause time.");
            require(block.timestamp > 1 days + dexTradingPausedTimestamp[_router], "StudioL_Token: can't pause again until cooldown is over.");

            enableAggregateDex[_router] = false;
            dexTradingPauseTime[_router] = pauseTimeInSecs;
            dexTradingPausedTimestamp[_router] = uint32(block.timestamp);

            emit DexRouterPaused(_msgSender(), _router, block.number, pauseTimeInSecs, block.timestamp);
        }
    }

    function getDexRouterPauseInfo(
        address _router
    ) external view returns (
        bool enabled_,
        uint32 dexTradingPauseTime_,
        uint32 dexTradingPausedTimestamp_
    ) {
        return ( enableAggregateDex[_router], dexTradingPauseTime[_router], dexTradingPausedTimestamp[_router] );
    }

    function getDexRemainingPauseTimeInSecs(address _router) public view returns (uint256) {
        if(dexTradingPauseTime[_router] + dexTradingPausedTimestamp[_router] > block.timestamp) {
            return dexTradingPauseTime[_router] + dexTradingPausedTimestamp[_router] - block.timestamp;
        } else {
            return 0;            
        }
    }

    function getDexRoutersCount() external view returns (uint256) {
        return dexRouters.length;
    }

    function verifyRouter(address _ca) external view returns (bool) {
        return isDexRouter[_ca];
    }

    function setLPTradingStatus(address[] calldata poolCA, bool _switch, uint32 pauseTimeInSecs) external onlyOwner {
        uint8 index;
        LPool[] storage poolInfo = liqPoolList;
        if(_switch) {
            for(uint8 i = 0; i < poolCA.length; i++) {

                index = searchLiqPool(poolCA[i]);
                
                require(!poolInfo[index].tradingEnabled, "StudioL_Token: trading already enabled.");
                require(poolInfo[index].liqAdded, "StudioL_Token: Liquidity must be added.");

                poolInfo[index].tradingEnabled = true;

                if(poolInfo[index].tradingPauseTime != 0) {
                    
                    poolInfo[index].tradingPauseTime = 0;
                }
                emit PoolEnabled(_msgSender(), poolCA[i], block.number, block.timestamp);
            }
        } else {
            require(pauseTimeInSecs <= maxTradePauseTime, "StudioL_Token: cannot pause longer than max trade pause time.");

            for(uint8 i = 0; i < poolCA.length; i++) {

                index = searchLiqPool(poolCA[i]);
                require(block.timestamp > 1 days + poolInfo[index].tradingPausedTimestamp, "StudioL_Token: can't pause again until cooldown is over.");

                poolInfo[index].tradingEnabled = false;
                poolInfo[index].tradingPauseTime = pauseTimeInSecs;
                poolInfo[index].tradingPausedTimestamp = uint32(block.timestamp);

                emit PoolPaused(_msgSender(), poolCA[i], block.number, pauseTimeInSecs, block.timestamp);
            }
        }
    }

    function getMaxTradePauseTimeInDays() external pure returns (uint32) {
        return maxTradePauseTime / 1 days;
    }

    function getLPRemainingPauseTimeInSecs(address poolCA) public view returns (uint256) {
        uint8 index = searchLiqPool(poolCA);
        if(liqPoolList[index].tradingPauseTime + liqPoolList[index].tradingPausedTimestamp > block.timestamp) {
            return liqPoolList[index].tradingPauseTime + liqPoolList[index].tradingPausedTimestamp - block.timestamp;
        } else {
            return 0;            
        }
    }

    function searchLiqPool(address pool) public view returns (uint8) {
        LPool[] memory poolInfo = liqPoolList;

        for(uint8 i = 0; i < poolInfo.length; i++) {
            if(poolInfo[i].poolCA == pool) {return i;}
        }
        return type(uint8).max;
    }

    function getAllLiqPoolsData() external view onlyOwner returns (LPool[] memory) {
        return liqPoolList;
    }

    function getLiqPoolsCount() external view returns (uint256) {
        return liqPoolList.length;
    }

    function verifyLiqPool(address _ca) external view returns (bool) {
        return isLiqPool[_ca];
    }

//===============================================================================================================
//Limit Settings


    function setExcludedFromLimits(address account, bool _switch) external onlyOwner {
        _isExcludedFromLimits[account] = _switch;
    }

    function isExcludedFromLimits(address account) external view returns (bool) {
        return _isExcludedFromLimits[account];
    }

    function _hasLimits(address from, address to) internal view returns (bool) {
        return _msgSender() != owner()
            && !_isExcludedFromLimits[from]
            && !_isExcludedFromLimits[to] 
            && to != DEAD
            && to != address(0);
    }

//======================================================================================
//Treasury Functions

    function setTreasury(address _treasury) external onlyOwner {
        require(_treasury != address(0), "StudioL: zero treasury address");
        require(treasury != _treasury, "StudioL_Token: already set to the desired value");
        emit TreasurySet( _msgSender(), treasury, _treasury);

        _isExcludedFromLimits[treasury] = false;
        treasury = _treasury;
        _isExcludedFromLimits[_treasury] = true;
    }

    function getTreasury() external view onlyOwner returns (address) {
        return treasury;
    }

    function setReferencePath(address pathCoin0CA, address pathCoin1CA) external onlyOwner {
        require(isContract(pathCoin0CA), "StudioL_Token: Invalid pathCoin0 CA");
        require(isContract(pathCoin1CA), "StudioL_Token: Invalid pathCoin1 CA");
        emit ReferencePathSet( _msgSender(), pathCoin0, pathCoin0CA, pathCoin1, pathCoin1CA);
        pathCoin0 = pathCoin0CA;
        pathCoin1 = pathCoin1CA;
        refPath = [pathCoin0CA, pathCoin1CA];
    }

    function getReferenceCoins() external view returns (address pathCoin0_, address pathCoin1_) {
        return (pathCoin0, pathCoin1);
    }

    function syncNativeANDStablePairs(bool _syncNativeCoin, bool _syncStableCoin) public {
        for(uint8 i = 0; i < liqPoolList.length; i++) {
            LPool memory LiqPool = liqPoolList[i];
            poolAddr = LiqPool.poolCA;
            if ( _syncNativeCoin && !LiqPool.pairedIsStable ) {

                uint256 ourPathCoin0Balance = IERC20(pathCoin0).balanceOf(poolAddr);
                uint256 ourLUSDBalance = _tOwned[poolAddr];

                amounts = IDexRouterV2(v2Router).getAmountsOut(ourPathCoin0Balance, refPath);

                if( ourLUSDBalance > amounts[1] ) { // ourLUSDBalance is over

                    _supplyControl(poolAddr, ( ourLUSDBalance - amounts[1] ), false); // Match the stablecoin amount

                } else if( ourLUSDBalance < amounts[1]) { // ourLUSDBalance is short

                    _supplyControl(poolAddr, ( amounts[1] - ourLUSDBalance ), true); // Match the stablecoin amount
                }
                if( LiqPool.V2orV3 ) {
                    IPairV2(poolAddr).sync();
                }
                emit PoolSynced( _msgSender(), LiqPool.pairedCoinCA, poolAddr, true, block.timestamp );
            }
            if( _syncStableCoin && LiqPool.pairedIsStable ) {

                uint256 pairedCoinBalance = IERC20(LiqPool.pairedCoinCA).balanceOf(poolAddr);

                if(LiqPool.pairedCoinDec != 18) {

                    pairedCoinBalance = pairedCoinBalance * (10 ** ( 18 - LiqPool.pairedCoinDec ) );
                }

                if( pairedCoinBalance > _tOwned[poolAddr] ) {

                    _supplyControl(poolAddr, ( pairedCoinBalance - _tOwned[poolAddr] ), true);

                } else if( pairedCoinBalance < _tOwned[poolAddr] ){

                    _supplyControl(poolAddr, ( _tOwned[poolAddr] - pairedCoinBalance ), false);
                }
                if( LiqPool.V2orV3 ) {
                    IPairV2(poolAddr).sync();
                }
                emit PoolSynced( _msgSender(), LiqPool.pairedCoinCA, poolAddr, false, block.timestamp );
            }
        }
    }

    function supplyControl(address account, uint256 amount, bool mintORburn) external onlyTreasury nonReentrant returns (bool) {
        return _supplyControl(account, amount, mintORburn);
    }

    function _supplyControl(address account, uint256 amount, bool mintORburn) private returns (bool) {
        require(account != address(0), "StudioL_Token: supply control at the zero address");
        bool success = false;

        if(mintORburn) {
            /** @dev Creates `amount` tokens and assigns them to `account`, increasing
            * the circulating supply and total supply.
            *
            * Emits a {Transfer} event with `from` set to the zero address.
            *
            * Requirements:
            *
            * - `account` cannot be the zero address.
            */
            _circSupply += amount;
            _tTotal += amount;

            _tOwned[account] += amount;

            emit Transfer(address(0), account, amount);
        } else {
            /**
            * @dev Buries or removes `amount` tokens from `account`, reducing the
            * circulating supply.
            *
            * Emits a {Transfer} event with `to` set to the `DEAD` address.
            *
            * Requirements:
            *
            * - `account` cannot be the zero address.
            * - `account` must have at least `amount` tokens.
            */
            _tOwned[account] -= amount;
            _tOwned[DEAD] += amount;
            
            _circSupply -= amount;

            emit Transfer(account, DEAD, amount);
        }
        success = true;
        return success;
    }

    function circulatingSupply() external view returns (uint256) {
        return _circSupply;
    }

//======================================================================================
//Transfer Functions

    function _transfer(address from, address to, uint256 amount) internal {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(to != DEAD, "StudioL_Token: transfer to the DEAD address. Can only burn via supply control.");
        require(amount > 0, "Transfer amount must be greater than zero");
        bool buy = false;
        bool sell = false;

        if ( isLiqPool[from] ) {
            buy = true;
            poolAddr = from;
        } else if (isLiqPool[to]) {
            sell = true;
            poolAddr = to;
        }

        if(_hasLimits(from, to)) {

            if(buy || sell) {

                uint8 index = searchLiqPool(poolAddr);
                LPool memory LiqPool = liqPoolList[index];

                require(enableAggregateDex[LiqPool.dexCA], "StudioL_Token: Trading not enabled for this router!");
                require(LiqPool.tradingEnabled, "StudioL_Token: Trading not enabled for this pair!");

                if(LiqPool.tradingPauseTime != 0) {

                    if( getDexRemainingPauseTimeInSecs(LiqPool.dexCA) == 0 ) {
                        
                        enableAggregateDex[LiqPool.dexCA] = true;
                        dexTradingPauseTime[LiqPool.dexCA] = 0;

                        emit DexRouterEnabled(_msgSender(), LiqPool.dexCA, block.number, block.timestamp);
                    }
                    if( getLPRemainingPauseTimeInSecs(LiqPool.poolCA) == 0 ) {

                        liqPoolList[index].tradingEnabled = true;
                        liqPoolList[index].tradingPauseTime = 0;

                        emit PoolEnabled(_msgSender(), LiqPool.poolCA, block.number, block.timestamp);
                    }
                }
            }
        }

        _tOwned[from] -= amount;
        _tOwned[to] += amount;
        emit Transfer(from, to, amount);

        if(buy || sell) {

            uint8 index = searchLiqPool(poolAddr);
            LPool memory LiqPool = liqPoolList[index];

            //Liquidity Removal
            if(isLiqPool[from] && to == LiqPool.dexCA || from == LiqPool.dexCA) { 

                if(LiqPool.pairedIsStable) {

                    syncNativeANDStablePairs(true, false); // sync native paired pools when trading stable paired pools

                } else {

                    syncNativeANDStablePairs(false, true); // sync stable paired pools when trading native paired pools
                }
                return;
            }

            if(!LiqPool.liqAdded) {

                if(!_hasLimits(from, to) && to == poolAddr) {

                    liqPoolList[index].liqAdded = true; // LiqPool is set to memory

                    return;
                }
            }
            if(LiqPool.pairedIsStable) {

                syncNativeANDStablePairs(true, false); // sync native paired pools when trading stable paired pools

                return;
            } else {
                
                syncNativeANDStablePairs(false, true); // sync stable paired pools when trading native paired pools

                return;
            }
        }
        syncNativeANDStablePairs(true, true);
    }

}