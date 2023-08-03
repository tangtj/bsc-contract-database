/*
 * Day of Defeat (DOD)
 *
 * Radical Social Experiment token mathematically designed to give holders 10,000,000X PRICE INCREASE
 *
 * Website: https://dayofdefeat.app/
 * To buy: https://ref.dayofdefeat.app/
 * To stake: https://stake.dayofdefeat.app/
 * NFT: https://nft.dayofdefeat.app/
 * DAO: https://dao.dayofdefeat.app/
 * Twitter: https://twitter.com/dayofdefeatBSC
 * Telegram: https://t.me/DayOfDefeatBSC
 * BTok: https://titanservice.cn/dayofdefeatCN
 *
 * By Studio L, a Legacy Capital Division
 * https://legacycapital.cc/StudioL/
*/
// 999,999,999 Optimization Runs
// SPDX-License-Identifier: MIT


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

/*
 * Day of Defeat (DOD)
 *
 * Radical Social Experiment token mathematically designed to give holders 10,000,000X PRICE INCREASE
 *
 * Website: https://dayofdefeat.app/
 * To buy: https://ref.dayofdefeat.app/
 * To stake: https://stake.dayofdefeat.app/
 * NFT: https://nft.dayofdefeat.app/
 * DAO: https://dao.dayofdefeat.app/
 * Twitter: https://twitter.com/dayofdefeatBSC
 * Telegram: https://t.me/DayOfDefeatBSC
 * BTok: https://titanservice.cn/dayofdefeatCN
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
  function allowance(address _operator, address spender) external view returns (uint256);
  function approve(address spender, uint256 amount) external returns (bool);
  function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
  event Transfer(address indexed from, address indexed to, uint256 value);
  event Approval(address indexed owner, address indexed spender, uint256 value);
}

/**
 *  999,999,999 Optimization Runs
 */
contract DOD_Token_2_1_05 is IERC20, Ownable2Step {

//Common Variables
    uint16 constant public DIVISOR = 10000;
    address constant public DEAD = 0x000000000000000000000000000000000000dEaD;

//Token Variables
    string constant private _name = "Day of Defeat 2.0";
    string constant private _symbol = "DOD";

    uint64 constant private startingSupply = 100_000_000_000_000; //100 Trillion, underscores aid readability
    uint8 constant private _decimals = 18;

    uint256 constant private genesisTotalSupply = startingSupply * (10 ** _decimals);

    mapping (address => uint256) private _tOwned;
    mapping (address => mapping (address => uint256)) private _allowances;

//Router, LP Pair Variables
    address private _poolCA;

    address private v2Router;

    address private nativeCoin;

    event V2RouterAndNativeCoinSet(
        address Setter,
        address indexed OldV2Router,
        address NewV2Router,
        address indexed OldNativeCoin,
        address NewNativeCoin
    );


    LPool[] private liqPoolList;
    //LP Pairs
    struct LPool {
        address poolCA;
        address pairedCoinCA;
        bool tradingEnabled;
        uint32 tradingEnabledBlock;
        uint32 tradingEnabledTime;
        uint32 tradingPauseTime;
        uint32 tradingPausedTimestamp;
    }
    mapping (address => bool) private isLiqPool;
    mapping (address => bool) private isPairedCoin;

    event NewLPCreated(address DexRouterCA, address LPCA, address PairedCoinCA);

    bool private contractSwapEnabled = false;
    uint256 private swapThreshold = 2_000_000_000 * (10 ** _decimals);

    uint32 constant private maxTradePauseTime = 28 days;
    event TradeEnabled(address Setter, address PoolCA, uint256 EnabledBlock, uint256 EnabledTime);
    event TradePaused(address Setter, address PoolCA, uint256 PausedBlock, uint32 PauseTime, uint256 DisabledTimestamp);
    event ContractSwapSettingsUpdated(address Setter, bool Enabled, uint256 SwapThreshold);

//Fee Variables

    uint16 private stakingTax = 100;
    uint16 private referralTax = 100;
    uint16 private marketingTax = 400;
    uint16 private prizeTax = 1300;
    uint16 private totalTax = 1900;

    uint16 constant public maxTotalTax = 2500;

    event TaxesUpdated(
        address Setter,
        uint16 Staking,
        uint16 Referral,
        uint16 Marketing,
        uint16 Prize,
        uint256 Timestamp
    );

    mapping (address => bool) private _isExcludedFromFees;
    mapping (address => bool) private _isExcludedFromLimits;

    uint256 public launchedTime;
    uint256 private sniperBlockTime;
    mapping (address => bool) private _isSniper;
    uint256 public sniperCount;

    event SniperCaught(address Sniper, uint256 Timestamp);


    address private stakingPool;
    address private referralPool;
    address private marketingWallet;
    address private transitionCollector;

    event StakingAndReferralPoolUpdated(
        address Setter,
        address indexed OldStakingPool,
        address NewStakingPool,
        address indexed OldReferralPool,
        address NewReferralPool
    );
    event MarketingWalletAndTransitionCollectorUpdated(
        address Setter,
        address indexed OldMarketingWallet,
        address NewMarketingWallet,
        address indexed OldPrizeTaxCollector,
        address NewPrizeTaxCollector
    );

    //Contract Swap
    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    bool private inSwap;
    address[] private contractSwapPath = [ address(this), nativeCoin ];

//Operator

    address private genesis; // genesis wallet address
    address private prizeVault; // prize vault contract
    address private migrationCA; // sacrifice migration contract
    
    event PrizeVaultSet(address Setter, address indexed OldPrizeVault, address NewPrizeVault);
    event MigrationCASet(address Setter, address indexed OldMigrationCA, address NewMigrationCA);

    // ============================================== Constructor ==============================================

    constructor () {
        stakingPool = _msgSender();
        referralPool = _msgSender();

        marketingWallet = _msgSender();

        transitionCollector = _msgSender();

        _isExcludedFromFees[ _msgSender() ] = true;
        _isExcludedFromFees[ address(this) ] = true;
        _isExcludedFromLimits[ _msgSender() ] = true;
        _isExcludedFromLimits[ address(this) ] = true;

        _tOwned[ _msgSender() ] = genesisTotalSupply;
        emit Transfer(address(0), _msgSender(), genesisTotalSupply);
    }

    // ============================================== Modifiers ==============================================

    modifier onlyVaultAuthorized() {
        require(_msgSender() == owner() ||
                _msgSender() == prizeVault,
                "StudioL: caller is not prize vault authorized");
        _;
    }

    modifier onlyMigrationAuthorized() {
        require(_msgSender() == owner() ||
                _msgSender() == migrationCA,
                "StudioL: caller is not migration contract authorized");
        _;
    }

//===============================================================================================================
//Override Functions

    /**
     * @dev See {IERC20-totalSupply}.
     */
    function totalSupply() external pure override returns (uint256) { return genesisTotalSupply; }

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
            require(_allowances[sender][ _msgSender() ] >= amount, "ERC20: transfer amount exceeds allowance");
            _approve(sender, _msgSender(), _allowances[sender][ _msgSender() ] - amount);
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

//===============================================================================================================
//Dex Router and LPool Manager Functions

    function setNewLiquidityPool(address _LPTargetCoinCA) external onlyOwner {
        require(_LPTargetCoinCA != address(0), "StudioL: zero _LPTargetCoinCA address");

        address lpCA;

        lpCA = IFactoryV2( IDexRouterV2(v2Router).factory() ).getPair( _LPTargetCoinCA, address(this) );
        require(lpCA == address(0) && !isLiqPool[lpCA], "StudioL_Token: Pair already exists!");
        lpCA = IFactoryV2( IDexRouterV2(v2Router).factory() ).createPair( _LPTargetCoinCA, address(this) );

        liqPoolList.push( LPool( lpCA, _LPTargetCoinCA, false, 0, 0, 0, 0 ) );

        isLiqPool[lpCA] = true;
        isPairedCoin[_LPTargetCoinCA] = true;

        _allowances[lpCA][v2Router] = type(uint256).max;
        _allowances[v2Router][lpCA] = type(uint256).max;

        emit NewLPCreated(v2Router, lpCA, _LPTargetCoinCA);
    }

    function searchLiqPool(address pool) private view returns (uint8) {
        LPool[] memory poolInfo = liqPoolList;

        for(uint8 i = 0; i < poolInfo.length; i++) {
            
            if(poolInfo[i].poolCA == pool) {return i;}
        }
        return type(uint8).max;
    }

    function getAllLiqPoolsData() external view returns (LPool[] memory) {
        return liqPoolList;
    }

    function getLiqPoolsCount() external view returns (uint256) {
        return liqPoolList.length;
    }

    function verifyLiqPool(address _ca) external view returns (bool) {
        return isLiqPool[_ca];
    }

    function verifyPairedCoin(address tokenCA) external view returns (bool) {
        return isPairedCoin[tokenCA];
    }

    /**
     * @dev sets a CEX liquidity pool address properly. 
     * Do not use a regular setNewLiquidityPool function, as that function is designed only for dex LP.
     */
    function setNewCEXLiquidityPool(address lpCA) external onlyOwner {
        require(lpCA != address(0), "StudioL: zero lpCA address");

        liqPoolList.push( LPool( lpCA, address(0), false, 0, 0, 0, 0 ) );

        isLiqPool[lpCA] = true;

        emit NewLPCreated( address(0), lpCA, address(0) );
    }

    function enableTrading(address poolCA) external onlyOwner {

        if(launchedTime == 0) {

            launchedTime = block.timestamp;
        }

        LPool storage poolInfo = liqPoolList[ searchLiqPool(poolCA) ];

        if(poolInfo.tradingPauseTime != 0) {

            poolInfo.tradingEnabled = true;
            poolInfo.tradingPauseTime = 0;

        } else {

            require(!poolInfo.tradingEnabled, "StudioL_Token: trading already enabled.");

            poolInfo.tradingEnabled = true;
            poolInfo.tradingEnabledBlock = uint32(block.number);
            poolInfo.tradingEnabledTime = uint32(block.timestamp);

            contractSwapEnabled = true;
        }
        emit TradeEnabled(_msgSender(), poolCA, block.number, block.timestamp);
    }

    function pauseTradeByPool(address[] calldata poolCA, bool pauseAllPools, uint32 pauseTimeInSecs) external onlyVaultAuthorized returns (bool) {
        require(pauseTimeInSecs <= maxTradePauseTime, "StudioL_Token: cannot pause longer than max trade pause time.");
        LPool[] storage poolInfo = liqPoolList;

        if(pauseAllPools) {

            for(uint8 i = 0; i < poolInfo.length; i++) {

                require(block.timestamp > 1 days + poolInfo[i].tradingPausedTimestamp, "StudioL_Token: can't pause again until cooldown is over.");
                poolInfo[i].tradingEnabled = false;
                poolInfo[i].tradingPauseTime = pauseTimeInSecs;
                poolInfo[i].tradingPausedTimestamp = uint32(block.timestamp);
                emit TradePaused(_msgSender(), poolInfo[i].poolCA, block.number, pauseTimeInSecs, block.timestamp);
            }

        } else {

            for(uint8 i = 0; i < poolCA.length; i++) {

                uint8 index = searchLiqPool(poolCA[i]);
                require(block.timestamp > 1 days + poolInfo[index].tradingPausedTimestamp, "StudioL_Token: can't pause again until cooldown is over.");
                poolInfo[index].tradingEnabled = false;
                poolInfo[index].tradingPauseTime = pauseTimeInSecs;
                poolInfo[index].tradingPausedTimestamp = uint32(block.timestamp);
                emit TradePaused(_msgSender(), poolCA[i], block.number, pauseTimeInSecs, block.timestamp);
            }
        }
        return true;
    }

    function getMaxTradePauseTimeInDays() external pure returns (uint32) {
        return maxTradePauseTime / 1 days;
    }

    function getRemainingPauseTimeInSecs(address poolCA) public view returns (uint256) {

        uint8 index = searchLiqPool(poolCA);

        if(liqPoolList[index].tradingPauseTime + liqPoolList[index].tradingPausedTimestamp > block.timestamp) {

            return liqPoolList[index].tradingPauseTime + liqPoolList[index].tradingPausedTimestamp - block.timestamp;

        } else {

            return 0;            
        }
    }

//===============================================================================================================
//Fee Settings

    /**
     * @dev Regulate fees within limits (maximum 25%)
     */
    function setTaxes(
        uint16 _staking,
        uint16 _referral,
        uint16 _marketing,
        uint16 _prize
    ) external onlyOwner {

        stakingTax = _staking;
        referralTax = _referral;
        marketingTax = _marketing;
        prizeTax = _prize;

        totalTax = _staking + _referral + _marketing + _prize;
        require(totalTax <= maxTotalTax, "StudioL_Token: total tax must be less than maxTotalTax");

        emit TaxesUpdated(_msgSender(), _staking, _referral, _marketing, _prize, block.timestamp);
    }

    function getTaxes() external view returns (uint16 staking_, uint16 referral_, uint16 marketing_, uint16 prize_, uint16 total_) {
        return (stakingTax, referralTax, marketingTax, prizeTax, totalTax);
    }

//Contract Swap functions

    function setContractSwapSettings(bool _switch, uint256 swapThresholdInWeiValue) external onlyOwner {

        contractSwapEnabled = _switch;
        swapThreshold = swapThresholdInWeiValue;

        emit ContractSwapSettingsUpdated(_msgSender(), contractSwapEnabled, swapThreshold);
    }

    function getContractSwapSettings() external view returns (bool contractSwapEnabled_, uint256 swapThreshold_) {
        return (contractSwapEnabled, swapThreshold);
    }

//Tax wallet functions

    function setStakingAndReferralPool(address _staking, address _referral) external onlyOwner {
        require(_staking != address(0), "StudioL: zero staking address");
        require(_referral != address(0), "StudioL: zero referral address");

        emit StakingAndReferralPoolUpdated(_msgSender(), stakingPool, _staking, referralPool, _referral);

        _isExcludedFromFees[stakingPool] = false;
        _isExcludedFromLimits[stakingPool] = false;
        stakingPool = _staking;
        _isExcludedFromFees[_staking] = true;
        _isExcludedFromLimits[_staking] = true;

        _isExcludedFromFees[referralPool] = false;
        _isExcludedFromLimits[referralPool] = false;
        referralPool = _referral;
        _isExcludedFromFees[_referral] = true;
        _isExcludedFromLimits[_referral] = true;
    }

    function setMarketingWalletAndTransitionCollector(address _marketing, address _collector) external onlyOwner {
        require(_marketing != address(0), "StudioL: zero marketing address");
        require(_collector != address(0), "StudioL: zero transition collector address");

        emit MarketingWalletAndTransitionCollectorUpdated(_msgSender(), marketingWallet, _marketing, transitionCollector, _collector);

        _isExcludedFromFees[marketingWallet] = false;
        _isExcludedFromLimits[marketingWallet] = false;
        marketingWallet = _marketing;
        _isExcludedFromFees[_marketing] = true;
        _isExcludedFromLimits[_marketing] = true;

        _isExcludedFromFees[transitionCollector] = false;
        _isExcludedFromLimits[transitionCollector] = false;
        transitionCollector = _collector;
        _isExcludedFromFees[_collector] = true;
        _isExcludedFromLimits[_collector] = true;
    }

    function getFeeWallets() external view returns (address staking_, address referral_, address marketing_, address transition_) {
        return (stakingPool, referralPool, marketingWallet, transitionCollector);
    }

//===============================================================================================================
//Tx Settings


    function setExcludedFromFees(address account, bool _switch) external onlyMigrationAuthorized returns (bool) {
        _isExcludedFromFees[account] = _switch;
        return true;
    }

    function isExcludedFromFees(address account) external view returns (bool) {
        return _isExcludedFromFees[account];
    }

    function setExcludedFromLimits(address account, bool _switch) external onlyOwner {
        _isExcludedFromLimits[account] = _switch;
    }

    function isExcludedFromLimits(address account) external view returns (bool) {
        return _isExcludedFromLimits[account];
    }

    function releaseSniper(address account) external onlyOwner {
        require(_isSniper[account], "StudioL_Token: target account is not a sniper");
        _isSniper[account] = false;

        sniperCount--;
    }

    function isSniper(address account) external view returns (bool) {
        return _isSniper[account];
    }

//======================================================================================
//Transfer Functions

    function _transfer(address from, address to, uint256 amount) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "StudioL_Token: Transfer amount must be greater than zero");
        bool buy = false;
        bool sell = false;
        bool other = false;

        if (isLiqPool[from]) {
            buy = true;
            _poolCA = from;
        } else if (isLiqPool[to]) {
            sell = true;
            _poolCA = to;
        } else {
            other = true;
        }

        if( !_isExcludedFromLimits[from] && !_isExcludedFromLimits[to] ) {

            require(to != DEAD, "StudioL_Token: can only burn via contract swap.");

            if(!buy) {

                require(!_isSniper[from], "StudioL_Token: Sniper Rejected");
            }
        }

        if(!other) {

            uint8 index = searchLiqPool(_poolCA);
            LPool memory poolInfo = liqPoolList[index];


            if( !_isExcludedFromLimits[from] && !_isExcludedFromLimits[to] ) {

                if(buy) {

                    if( block.timestamp <= ( launchedTime + sniperBlockTime ) ) {

                        if( !_isSniper[to] ) {

                            _isSniper[to] = true;

                            sniperCount++;

                            emit SniperCaught(to, block.timestamp);
                        }
                    }
                }

                if(poolInfo.tradingPauseTime != 0) {

                    if( getRemainingPauseTimeInSecs(poolInfo.poolCA) == 0 ) {

                        liqPoolList[index].tradingEnabled = true;
                        liqPoolList[index].tradingPauseTime = 0;
                    }
                }
                require(poolInfo.tradingEnabled, "StudioL_Token: Trading not enabled!");
            }
        }

        if(!inSwap) {

            if(contractSwapEnabled) {

                if(!buy) {

                    if(_tOwned[ address(this) ] >= swapThreshold) {

                        contractSwap( _tOwned[ address(this) ]);
                    }
                }
            }
        }

        uint256 _feeAmount = amount * totalTax / DIVISOR;

        if(_isExcludedFromFees[from] || _isExcludedFromFees[to] || other) {

            _feeAmount = 0;
        }

        uint256 _transferAmount = amount;

        if(_feeAmount > 0) {

            _transferAmount = amount - _feeAmount;
            _tOwned[from] -= _feeAmount;
            _tOwned[ address(this) ] += _feeAmount;

            emit Transfer(from, address(this), _feeAmount);
        }

        _tOwned[from] -= _transferAmount;
        _tOwned[to] += _transferAmount;

        emit Transfer(from, to, _transferAmount);
    }

    function triggerContractSwap(uint256 swapAmount) external onlyOwner {
        contractSwap(swapAmount);
    }

    function contractSwap(uint256 swapAmount) private lockTheSwap {

        if (totalTax == 0) {
            return;
        }

        uint256 origSwapAmount = swapAmount;
        uint256 transferBalance;

        if(stakingTax > 0) {
            transferBalance = ( ( origSwapAmount * stakingTax ) / totalTax );
            _tOwned[ stakingPool ] += transferBalance;
            _tOwned[ address(this) ] -= transferBalance;
            emit Transfer( address(this), stakingPool, transferBalance );

            swapAmount -= transferBalance;
            transferBalance = 0;
        }

        if(referralTax > 0) {
            transferBalance = ( ( origSwapAmount * referralTax ) / totalTax );
            _tOwned[ referralPool ] += transferBalance;
            _tOwned[ address(this) ] -= transferBalance;
            emit Transfer( address(this), referralPool, transferBalance );

            swapAmount -= transferBalance;
            transferBalance = 0;
        }

        if(prizeTax > 0) {
            transferBalance = ( (origSwapAmount * prizeTax) / totalTax );
            uint256 transferBalanceHalf = transferBalance / 2;
            _tOwned[ DEAD ] += transferBalanceHalf; // half of the prize tax gets burned to DEAD
            _tOwned[ transitionCollector ] += (transferBalance - transferBalanceHalf); // half of the prize tax goes to LStable treasury
            _tOwned[ address(this) ] -= transferBalance;
            emit Transfer( address(this), DEAD, (transferBalanceHalf) );
            emit Transfer( address(this), transitionCollector, (transferBalance - transferBalanceHalf) );

            swapAmount -= transferBalance;
        }

        IDexRouterV2(v2Router)
            .swapExactTokensForETH(
                swapAmount,
                0,
                contractSwapPath,
                marketingWallet,
                block.timestamp + 60
        );
    }

//===============================================================================================================
//Operator Settings

    function setSniperBlockTime(uint256 _sniperBlockTime) external onlyOwner {
        require(sniperBlockTime != _sniperBlockTime, "StudioL_Token: already set to the desired value.");

        sniperBlockTime = _sniperBlockTime;
    }

    function getSniperBlockTime() external view onlyOwner returns (uint256) {
        return sniperBlockTime;
    }

    function setV2RouterANDNativeCoin(address _v2Router, address _nativeCoin) external onlyOwner {
        require(_v2Router != address(0), "StudioL_Token: zero router address.");
        require(_nativeCoin != address(0), "StudioL_Token: zero native coin address.");

        emit V2RouterAndNativeCoinSet( _msgSender(), v2Router, _v2Router, nativeCoin, _nativeCoin);
        v2Router = _v2Router;

        _isExcludedFromFees[v2Router] = true;
        _isExcludedFromLimits[v2Router] = true;

        _allowances[ _msgSender() ][v2Router] = type(uint256).max;
        _allowances[v2Router][ _msgSender() ] = type(uint256).max;
        _allowances[ address(this) ][v2Router] = type(uint256).max;
        _allowances[v2Router][ address(this) ] = type(uint256).max;

        nativeCoin = _nativeCoin;

        contractSwapPath = [ address(this), _nativeCoin ];
    }

    function getV2Router() external view returns (address) {
        return v2Router;
    }

    function getNativeCoin() external view returns (address) {
        return nativeCoin;
    }

    function setPrizeVault(address _prizeVault) external onlyOwner {
        require(_prizeVault != address(0), "StudioL_Token: zero vault address.");
        require(prizeVault != _prizeVault, "StudioL_Token: already set to the desired address");
        emit PrizeVaultSet( _msgSender(), prizeVault, _prizeVault);
        prizeVault = _prizeVault;
    }

    function getPrizeVault() external view returns (address) {
        return prizeVault;
    }

    function setMigrationCA(address _migrationCA) external onlyOwner {
        require(_migrationCA != address(0), "StudioL: zero migration contract address");
        require(migrationCA != _migrationCA, "StudioL: already set to the desired address");
        emit MigrationCASet( _msgSender(), migrationCA, _migrationCA);
        migrationCA = _migrationCA;
    }

    function getMigrationCA() external view onlyOwner returns (address) {
        return migrationCA;
    }

}