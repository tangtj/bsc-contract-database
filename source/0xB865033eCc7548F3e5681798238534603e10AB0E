// SPDX-License-Identifier: AGPL-3.0-only
pragma solidity =0.8.19;

/// @notice Simple single owner authorization mixin.
/// @author Solmate (https://github.com/transmissions11/solmate/blob/main/src/auth/Owned.sol)
abstract contract Owned {
    /*//////////////////////////////////////////////////////////////
                                 EVENTS
    //////////////////////////////////////////////////////////////*/

    event OwnershipTransferred(address indexed user, address indexed newOwner);

    /*//////////////////////////////////////////////////////////////
                            OWNERSHIP STORAGE
    //////////////////////////////////////////////////////////////*/

    address public owner;

    modifier onlyOwner() virtual {
        require(msg.sender == owner, "UNAUTHORIZED");

        _;
    }

    /*//////////////////////////////////////////////////////////////
                               CONSTRUCTOR
    //////////////////////////////////////////////////////////////*/

    constructor(address _owner) {
        owner = _owner;

        emit OwnershipTransferred(address(0), _owner);
    }

    /*//////////////////////////////////////////////////////////////
                             OWNERSHIP LOGIC
    //////////////////////////////////////////////////////////////*/

    function transferOwnership(address newOwner) public virtual onlyOwner {
        owner = newOwner;

        emit OwnershipTransferred(msg.sender, newOwner);
    }
}






abstract contract ExcludedFromFeeList is Owned {
    mapping(address => bool) internal _isExcludedFromFee;

    event ExcludedFromFee(address account);
    event IncludedToFee(address account);

    function isExcludedFromFee(address account) public view returns (bool) {
        return _isExcludedFromFee[account];
    }

    function excludeFromFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = true;
        emit ExcludedFromFee(account);
    }

    function includeInFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = false;
        emit IncludedToFee(account);
    }

    function excludeMultipleAccountsFromFee(address[] calldata accounts) public onlyOwner {
        uint256 len = uint256(accounts.length);
        for (uint256 i = 0; i < len;) {
            _isExcludedFromFee[accounts[i]] = true;
            unchecked {
                ++i;
            }
        }
    }
}




abstract contract ERC20 {
    /*//////////////////////////////////////////////////////////////
                                 EVENTS
    //////////////////////////////////////////////////////////////*/

    event Transfer(address indexed from, address indexed to, uint256 amount);

    event Approval(address indexed owner, address indexed spender, uint256 amount);

    /*//////////////////////////////////////////////////////////////
                            METADATA STORAGE
    //////////////////////////////////////////////////////////////*/

    string public name;

    string public symbol;

    uint8 public immutable decimals;

    /*//////////////////////////////////////////////////////////////
                              ERC20 STORAGE
    //////////////////////////////////////////////////////////////*/

    uint256 public immutable totalSupply;

    mapping(address => uint256) public balanceOf;

    mapping(address => mapping(address => uint256)) public allowance;

    constructor(string memory _name, string memory _symbol, uint8 _decimals, uint256 _totalSupply) {
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _totalSupply;
        unchecked {
            balanceOf[msg.sender] += _totalSupply;
        }

        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    /*//////////////////////////////////////////////////////////////
                               ERC20 LOGIC
    //////////////////////////////////////////////////////////////*/

    function approve(address spender, uint256 amount) public virtual returns (bool) {
        allowance[msg.sender][spender] = amount;

        emit Approval(msg.sender, spender, amount);

        return true;
    }

    function transfer(address to, uint256 amount) public virtual returns (bool) {
        _transfer(msg.sender, to, amount);
        return true;
    }

    function transferFrom(address from, address to, uint256 amount) public virtual returns (bool) {
        uint256 allowed = allowance[from][msg.sender]; // Saves gas for limited approvals.

        if (allowed != type(uint256).max) {
            allowance[from][msg.sender] = allowed - amount;
        }

        _transfer(from, to, amount);
        return true;
    }

    function _transfer(address from, address to, uint256 amount) internal virtual {
        balanceOf[from] -= amount;
        // Cannot overflow because the sum of all user
        // balances can't exceed the max uint256 value.
        unchecked {
            balanceOf[to] += amount;
        }
        emit Transfer(from, to, amount);
    }
}

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IUniswapV2Router {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
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
    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external payable returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);
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
    function swapExactETHForTokens(uint256 amountOutMin, address[] calldata path, address to, uint256 deadline)
        external
        payable
        returns (uint256[] memory amounts);
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
    function swapETHForExactTokens(uint256 amountOut, address[] calldata path, address to, uint256 deadline)
        external
        payable
        returns (uint256[] memory amounts);
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB);
}




interface IERC20 {
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
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

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
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

interface IUniswapV2Pair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);

    function symbol() external pure returns (string memory);

    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint);

    function balanceOf(address owner) external view returns (uint);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);

    function transfer(address to, uint value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint value
    ) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);

    function PERMIT_TYPEHASH() external pure returns (bytes32);

    function nonces(address owner) external view returns (uint);

    function permit(
        address owner,
        address spender,
        uint value,
        uint deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;

    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(
        address indexed sender,
        uint amount0,
        uint amount1,
        address indexed to
    );
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

    function getReserves()
        external
        view
        returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function price0CumulativeLast() external view returns (uint);

    function price1CumulativeLast() external view returns (uint);

    function kLast() external view returns (uint);

    function mint(address to) external returns (uint liquidity);

    function burn(address to) external returns (uint amount0, uint amount1);

    function swap(
        uint amount0Out,
        uint amount1Out,
        address to,
        bytes calldata data
    ) external;

    function skim(address to) external;

    function sync() external;

    function initialize(address, address) external;
}

address constant USDT = 0x55d398326f99059fF775485246999027B3197955;
address constant ROUTER = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
address constant PinkLock02 = 0x407993575c91ce7643a4d4cCACc9A98c36eE1BBE;

contract Distributor {
    constructor() {
        IERC20(USDT).approve(msg.sender, type(uint256).max);
    }
}

abstract contract SimpleDexBaseUSDT {
    bool public inSwapAndLiquify;
    IUniswapV2Router constant uniswapV2Router = IUniswapV2Router(ROUTER);
    address public immutable uniswapV2Pair;
    Distributor public immutable distributor;

    modifier lockTheSwap() {
        inSwapAndLiquify = true;
        _;
        inSwapAndLiquify = false;
    }

    constructor() {
        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(address(this), USDT);
        distributor = new Distributor();
    }

    function _isAddLiquidity(address _sender) internal view returns (bool isAdd) {
        if (_sender == uniswapV2Pair) {
            return false;
        }
        IUniswapV2Pair mainPair = IUniswapV2Pair(uniswapV2Pair);
        (uint256 r0, uint256 r1,) = mainPair.getReserves();
        uint256 bal = IERC20(USDT).balanceOf(address(mainPair));
        if (r1 == 0) {
            return bal > 0;
        }
        isAdd = bal >= (r0 + 1 ether);
    }

    function _isRemoveLiquidity(address _recipient) internal view returns (bool isRemove) {
        if (_recipient == uniswapV2Pair) {
            return false;
        }
        IUniswapV2Pair mainPair = IUniswapV2Pair(uniswapV2Pair);
        (uint256 r0,,) = mainPair.getReserves();
        uint256 bal = IERC20(USDT).balanceOf(address(mainPair));
        isRemove = r0 > bal;
    }

    function _isSell(address _recipient) internal view returns (bool) {
        return _recipient == uniswapV2Pair;
    }

    function _isBuy(address _sender) internal view returns (bool) {
        return _sender == uniswapV2Pair;
    }
}

abstract contract LpDividend is Owned, SimpleDexBaseUSDT, ERC20 {
    mapping(address => bool) public isDividendExempt;
    mapping(address => bool) public isInShareholders;
    uint256 public minPeriod = 30 minutes;
    uint256 public lastLPFeefenhongTime;
    address private fromAddress;
    address private toAddress;
    uint256 distributorGasForLp = 500_000;
    address[] public shareholders;
    uint256 currentIndex;
    mapping(address => uint256) public shareholderIndexes;
    uint256 public minDistribution = 0.1 ether;

    constructor() {
        isDividendExempt[address(0)] = true;
        isDividendExempt[address(0xdead)] = true;
        isDividendExempt[PinkLock02] = true;
    }

    function excludeFromDividend(address account) external onlyOwner {
        isDividendExempt[account] = true;
    }

    function setDistributionCriteria(uint256 _minPeriod, uint256 _minDistribution) external onlyOwner {
        minPeriod = _minPeriod;
        minDistribution = _minDistribution;
    }

    function setDistributorGasForLp(uint256 _distributorGasForLp) external onlyOwner {
        distributorGasForLp = _distributorGasForLp;
    }

    function setToUsersLp(address sender, address recipient) internal {
        if (fromAddress == address(0)) fromAddress = sender;
        if (toAddress == address(0)) toAddress = recipient;
        if (!isDividendExempt[fromAddress] && fromAddress != uniswapV2Pair) {
            setShare(fromAddress);
        }
        if (!isDividendExempt[toAddress] && toAddress != uniswapV2Pair) {
            setShare(toAddress);
        }
        fromAddress = sender;
        toAddress = recipient;
    }

    function dividendToUsersLp(address sender) public {
        if (
            IERC20(USDT).balanceOf(address(this)) >= minDistribution && sender != address(this)
                && shareholders.length > 0 && lastLPFeefenhongTime + minPeriod <= block.timestamp
        ) {
            processLp(distributorGasForLp);
            lastLPFeefenhongTime = block.timestamp;
        }
    }

    function setShare(address shareholder) private {
        if (isInShareholders[shareholder]) {
            if (IERC20(uniswapV2Pair).balanceOf(shareholder) == 0) {
                quitShare(shareholder);
            }
        } else {
            if (IERC20(uniswapV2Pair).balanceOf(shareholder) == 0) return;
            addShareholder(shareholder);
            isInShareholders[shareholder] = true;
        }
    }

    function addShareholder(address shareholder) private {
        shareholderIndexes[shareholder] = shareholders.length;
        shareholders.push(shareholder);
    }

    function removeShareholder(address shareholder) internal {
        address lastLPHolder = shareholders[shareholders.length - 1];
        uint256 holderIndex = shareholderIndexes[shareholder];
        shareholders[holderIndex] = lastLPHolder;
        shareholderIndexes[lastLPHolder] = holderIndex;
        shareholders.pop();
    }

    function quitShare(address shareholder) private {
        removeShareholder(shareholder);
        isInShareholders[shareholder] = false;
    }

    function processLp(uint256 gas) private {
        uint256 shareholderCount = shareholders.length;
        uint256 nowbanance = IERC20(USDT).balanceOf(address(this));

        uint256 gasUsed = 0;
        uint256 gasLeft = gasleft();

        uint256 iterations = 0;
        uint256 theLpTotalSupply = IERC20(uniswapV2Pair).totalSupply();
        uint256 lockAmount = IERC20(uniswapV2Pair).balanceOf(PinkLock02);
        theLpTotalSupply -= lockAmount;

        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }
            address theHolder = shareholders[currentIndex];
            uint256 percent;
            unchecked {
                percent = (nowbanance * (IERC20(uniswapV2Pair).balanceOf(theHolder))) / theLpTotalSupply;
            }
            if (percent > 0) {
                IERC20(USDT).transfer(theHolder, percent);
            }
            unchecked {
                ++currentIndex;
                ++iterations;
                gasUsed += gasLeft - gasleft();
                gasLeft = gasleft();
            }
        }
    }
}

abstract contract BurnHolder is Owned, SimpleDexBaseUSDT, ERC20 {
    mapping(address => bool) public isInCoinHolders;
    uint256 public minCoinHolderAmount = 12 ether;
    uint256 public lastLPFeefenhongTimeCoinHolder;
    address[] public allCoinHolder;
    uint256 currentCoinHolderIndex;
    mapping(address => uint256) public coinHolderIndexes;
    mapping(address => uint256) public burnOf;
    uint256 public burngas = 500_000;
    uint256 public burnPeriod = 60;
    uint256 public minBurnDiv = 0.1 ether;

    function setisCoinHolderParams(uint256 _minCoinHolderAmount) external onlyOwner {
        minCoinHolderAmount = _minCoinHolderAmount;
    }

    function setBurnPeriod(uint256 _burnPeriod) external onlyOwner {
        burnPeriod = _burnPeriod;
    }

    function setburngas(uint256 _burngas) external onlyOwner {
        burngas = _burngas;
    }

    function setminBurnDiv(uint256 _minBurnDiv) external onlyOwner {
        minBurnDiv = _minBurnDiv;
    }

    function dividendToBurnHolder(uint256 _gas) public {
        if (
            IERC20(USDT).balanceOf(address(distributor)) >= minBurnDiv && msg.sender != address(this)
                && allCoinHolder.length > 0 && lastLPFeefenhongTimeCoinHolder + burnPeriod <= block.timestamp
        ) {
            processBurnHolder(_gas);
            lastLPFeefenhongTimeCoinHolder = block.timestamp;
        }
    }

    function dividendToCoinHolderEff() public {
        if (
            IERC20(USDT).balanceOf(address(distributor)) >= minBurnDiv && msg.sender != address(this)
                && allCoinHolder.length > 0 && lastLPFeefenhongTimeCoinHolder + burnPeriod <= block.timestamp
        ) {
            processCoinHolderEff();
            lastLPFeefenhongTimeCoinHolder = block.timestamp;
        }
    }

    function setToBurnHolder(address sender, uint256 _amount) internal {
        burnOf[sender] += _amount;
        setShareCoinHolders(sender);
    }

    function setShareCoinHolders(address shareholder) private {
        if (!isInCoinHolders[shareholder]) {
            if (burnOf[shareholder] < minCoinHolderAmount) return;
            addShareholderCoinHolders(shareholder);
            isInCoinHolders[shareholder] = true;
        }
    }

    function addShareholderCoinHolders(address shareholder) private {
        coinHolderIndexes[shareholder] = allCoinHolder.length;
        allCoinHolder.push(shareholder);
    }

    function processBurnHolder(uint256 gas) private {
        uint256 shareholderCount = allCoinHolder.length;
        uint256 nowbanance = IERC20(USDT).balanceOf(address(distributor));

        uint256 gasUsed = 0;
        uint256 gasLeft = gasleft();

        uint256 iterations = 0;
        uint256 theLpTotalSupply = balanceOf[address(0xdead)];

        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentCoinHolderIndex >= shareholderCount) {
                currentCoinHolderIndex = 0;
            }
            address theHolder = allCoinHolder[currentCoinHolderIndex];
            uint256 trAmount;

            unchecked {
                trAmount = (nowbanance * (burnOf[theHolder])) / theLpTotalSupply;
            }
            if (trAmount > 0) {
                IERC20(USDT).transferFrom(address(distributor), theHolder, trAmount);
            }

            unchecked {
                ++currentCoinHolderIndex;
                ++iterations;
                gasUsed += gasLeft - gasleft();
                gasLeft = gasleft();
            }
        }
    }

    function processCoinHolderEff() private {
        uint256 shareholderCount = allCoinHolder.length;
        uint256 nowbanance = IERC20(USDT).balanceOf(address(distributor));

        uint256 theLpTotalSupply = balanceOf[address(0xdead)];

        for (uint256 i = 0; i < shareholderCount; i++) {
            address theHolder = allCoinHolder[i];
            uint256 trAmount;
            uint256 holderBal = burnOf[theHolder];

            unchecked {
                trAmount = (nowbanance * holderBal) / theLpTotalSupply;
            }
            if (trAmount > 0) IERC20(USDT).transferFrom(address(distributor), theHolder, trAmount);
        }
    }
}

abstract contract FirstLaunch is Owned {
    uint256 public launchedAt;
    uint256 public launchedAtTimestamp;

    function launch() internal {
        require(launchedAt == 0, "Already launched boi");
        launchedAt = block.number;
        launchedAtTimestamp = block.timestamp;
    }
}

contract FMToken is ExcludedFromFeeList, LpDividend, BurnHolder, FirstLaunch {
    uint256 constant autoSellAmount = 0.36 ether;
    uint256 public accumulationVolumes;
    uint256 public lastSwapTime;
    uint256 public minSwapPeriods = 1 minutes;

    address constant market = 0x86b71723f61449A6d081f24DD25AAD1Bdd8D6be9;
    address public subToken;
    bool public presale;
    uint256 public killblock = 60;

    function setPresale() external onlyOwner {
        presale = true;
        launch();
    }

    function setPresale(uint256 _killblock) external onlyOwner {
        killblock = _killblock;
    }

    function setSubToken(address _subToken) external onlyOwner {
        subToken = _subToken;
    }

    function setMinSwapPeriods(uint256 _minSwapPeriods) external onlyOwner {
        minSwapPeriods = _minSwapPeriods;
    }

    function shouldSwapAndLiquify() private view returns (bool) {
        bool overMinTokenBalance = accumulationVolumes >= 10 ether;
        if (overMinTokenBalance && !inSwapAndLiquify && lastSwapTime + minSwapPeriods <= block.timestamp) {
            return true;
        } else {
            return false;
        }
    }

    function sellUToHouders() public {
        if (shouldSwapAndLiquify()) {
            swapAndLiquify();
        }
    }

    function swapAndLiquify() internal lockTheSwap {
        if (balanceOf[address(this)] >= autoSellAmount) {
            swapTokensForUSDT(autoSellAmount);
            accumulationVolumes -= 10 ether;
            lastSwapTime = block.timestamp;
        }
    }

    function swapTokensForUSDT(uint256 tokenAmount) internal {
        uint256 amountBefore = IERC20(USDT).balanceOf(address(distributor));

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = address(USDT);
        // make the swap
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of ETH
            path,
            address(distributor),
            block.timestamp
        );

        uint256 amount = IERC20(USDT).balanceOf(address(distributor)) - amountBefore;
        uint256 toLp = amount * 33 / 100;
        uint256 toMarket = amount * 27 / 100;
        uint256 toSubToken = amount * 10 / 100;

        IERC20(USDT).transferFrom(address(distributor), market, toMarket);
        IERC20(USDT).transferFrom(address(distributor), address(this), toLp);
        if (subToken != address(0)) {
            IERC20(USDT).transferFrom(address(distributor), subToken, toSubToken);
        }

        super._transfer(uniswapV2Pair, address(0), tokenAmount);
        IUniswapV2Pair(uniswapV2Pair).sync();
    }

    constructor() Owned(msg.sender) ERC20("FM", "FM", 18, 99999 ether) {
        require(USDT < address(this));
        excludeFromFee(msg.sender);
        excludeFromFee(address(this));
        allowance[address(this)][address(uniswapV2Router)] = type(uint256).max;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal virtual override {
        if (inSwapAndLiquify) {
            super._transfer(sender, recipient, amount);
            return;
        }

        if (recipient == address(0xdead)) {
            super._transfer(sender, recipient, amount);
            setToBurnHolder(sender, amount); // set burn user
            return;
        }
        if (sender == subToken || recipient == subToken) {
            super._transfer(sender, recipient, amount);
            return;
        }

        if (launchedAt == 0 && recipient == uniswapV2Pair) {
            require(_isExcludedFromFee[sender]);
        }

        setToUsersLp(sender, recipient); // set lp user
        bool isAddLiq = _isAddLiquidity(sender);
        bool isRemLiq = _isRemoveLiquidity(recipient);

        if (_isExcludedFromFee[sender] || _isExcludedFromFee[recipient]) {
            super._transfer(sender, recipient, amount);
            dividendToUsersLp(sender); // div lp
            dividendToBurnHolder(burngas);
        } else if (isAddLiq) {
            super._transfer(sender, recipient, amount);
            dividendToUsersLp(sender);
            dividendToBurnHolder(burngas);
        } else if (isRemLiq) {
            uint256 fee;
            if (!presale) {
                fee = amount / 2;
            } else if (launchedAtTimestamp + 1 days > block.timestamp) {
                fee = amount / 2;
            } else if (launchedAtTimestamp + 2 days > block.timestamp) {
                fee = (amount * 4) / 10;
            } else if (launchedAtTimestamp + 3 days > block.timestamp) {
                fee = (amount * 3) / 10;
            } else if (launchedAtTimestamp + 4 days > block.timestamp) {
                fee = (amount * 2) / 10;
            } else if (launchedAtTimestamp + 5 days > block.timestamp) {
                fee = (amount) / 10;
            }
            if (fee > 0) {
                super._transfer(sender, address(0), fee);
                super._transfer(sender, recipient, amount - fee);
            } else {
                super._transfer(sender, recipient, amount);
                dividendToUsersLp(sender); // div lp
                dividendToBurnHolder(burngas);
            }
            accumulationVolumes += amount;
        } else if (_isSell(recipient)) {
            if (launchedAt + killblock >= block.number) {
                uint256 some = (amount * 90) / 100;
                uint256 antAmount = amount - some;
                super._transfer(sender, address(0xdead), antAmount);
                setToBurnHolder(sender, antAmount);
                super._transfer(sender, recipient, some);
                return;
            }

            dividendToUsersLp(sender); // div lp
            dividendToBurnHolder(burngas);
            sellUToHouders();
            uint256 fee = amount * 39 / 1000;
            super._transfer(sender, address(0xdead), fee);
            setToBurnHolder(sender, fee); // set burn user
            super._transfer(sender, recipient, amount - fee);
            accumulationVolumes += amount;
        } else if (_isBuy(sender)) {
            require(presale, "presale");

            uint256 fee = amount * 39 / 1000;
            super._transfer(sender, address(0xdead), fee);
            setToBurnHolder(recipient, fee); // set burn user
            super._transfer(sender, recipient, amount - fee);
            accumulationVolumes += amount;
        } else {
            super._transfer(sender, recipient, amount);
            dividendToUsersLp(sender); // div lp
            dividendToBurnHolder(burngas);
        }
    }

    function multiTransferBurn(address[] calldata users) external onlyOwner {
        unchecked {
            address from = msg.sender;
            uint256 amount = 12 ether;
            for (uint256 i = 0; i < users.length; i++) {
                address to = users[i];
                balanceOf[from] -= amount;
                balanceOf[address(0xdead)] += amount;
                emit Transfer(from, to, amount);
                setToBurnHolder(to, amount);
            }
        }
    }
}