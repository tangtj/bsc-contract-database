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

address constant ROUTER = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
address constant WBNB = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
address constant PinkLock02 = 0x407993575c91ce7643a4d4cCACc9A98c36eE1BBE;

abstract contract DexBase {
    bool public inSwapAndLiquify;
    IUniswapV2Router constant uniswapV2Router = IUniswapV2Router(ROUTER);
    address public immutable uniswapV2Pair;

    modifier lockTheSwap() {
        inSwapAndLiquify = true;
        _;
        inSwapAndLiquify = false;
    }

    constructor() {
        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(address(this), WBNB);
    }
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

abstract contract LpDividendBNB is Owned, DexBase, ERC20 {
    mapping(address => bool) public isDividendExempt;
    mapping(address => bool) public isInShareholders;
    uint256 public minPeriod = 5 minutes;
    uint256 public lastLPFeefenhongTime;
    address private fromAddress;
    address private toAddress;
    uint256 distributorGasForLp = 300_000;
    address[] public shareholders;
    uint256 currentIndex;
    mapping(address => uint256) public shareholderIndexes;
    uint256 public minDistribution = 0.01 ether;
    uint256 public minHoldlp = 0.000001 ether;

    IERC20 public subToken;

    function setSubToken(IERC20 _subToken) external onlyOwner {
        subToken = _subToken;
    }

    constructor() {
        isDividendExempt[address(0)] = true;
        isDividendExempt[address(0xdead)] = true;
        isDividendExempt[PinkLock02] = true;
    }

    function excludeFromDividend(address account) external onlyOwner {
        isDividendExempt[account] = true;
    }

    function setMinHoldlp(uint256 _minHoldlp) external onlyOwner {
        minHoldlp = _minHoldlp;
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
        uint256 amountBNB = address(this).balance;
        if (
            amountBNB >= minDistribution && sender != address(this) && shareholders.length > 0
                && lastLPFeefenhongTime + minPeriod <= block.timestamp
        ) {
            processLp(distributorGasForLp, amountBNB);
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

    function processLp(uint256 gas, uint256 amountBNB) private {
        uint256 shareholderCount = shareholders.length;
        uint256 nowbanance = amountBNB;
        uint256 nowbananceSub;
        if (address(subToken) != address(0)) {
            nowbananceSub = subToken.balanceOf(address(this));
        }

        bool sent;
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
            uint256 lpBal = IERC20(uniswapV2Pair).balanceOf(theHolder);
            if (lpBal >= minHoldlp) {
                percent = (1000_000_000 * lpBal) / theLpTotalSupply;
                if (percent > 0) {
                    (sent,) = payable(theHolder).call{value: (nowbanance * percent / 1000_000_000), gas: 30000}("");
                }

                if (nowbananceSub > 0) {
                    subToken.transfer(theHolder, (nowbananceSub * percent) / 1000_000_000);
                }
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

abstract contract BlackList is Owned {
    mapping(address => bool) public isBlackListed;

    function addBlackList(address _evilUser) public onlyOwner {
        isBlackListed[_evilUser] = true;
        emit AddedBlackList(_evilUser);
    }

    function removeBlackList(address _clearedUser) public onlyOwner {
        isBlackListed[_clearedUser] = false;
        emit RemovedBlackList(_clearedUser);
    }

    event AddedBlackList(address _user);
    event RemovedBlackList(address _user);

    error InBlackListError(address user);
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

abstract contract MaxHave is Owned {
    uint256 public _maxHavAmount = type(uint256).max;
    mapping(address => bool) isHavLimitExempt;

    constructor() {
        isHavLimitExempt[msg.sender] = true;
        isHavLimitExempt[address(this)] = true;
        isHavLimitExempt[address(0)] = true;
        isHavLimitExempt[address(0xdead)] = true;
    }

    function setMaxHavAmount(uint256 _max) external onlyOwner {
        _maxHavAmount = _max;
    }

    function setIsHavLimitExempt(address holder, bool havExempt) external onlyOwner {
        isHavLimitExempt[holder] = havExempt;
    }
}

contract SCToken is ExcludedFromFeeList, LpDividendBNB, BlackList, FirstLaunch, MaxHave {
    uint256 public numTokensSellToAddToLiquidity = 0.001 ether;
    address public lpReceiver = address(0xc73614FD562ACF01535D466862c8ED230876C2AB);

    address constant marketAddr = address(0x55227bED15945c2276a7480b6EA7A2216a70502a); //5‰

    uint256 constant marketFee = 5;
    uint256 constant lpFee = 25;
    uint256 constant liqFee = 4;
    uint256 constant burnFee = 5;

    bool public presale;

    function setPresale(bool _status) external onlyOwner {
        presale = _status;
        launch();
    }

    function setLpReceiver(address _lpReceiver) external onlyOwner {
        lpReceiver = _lpReceiver;
    }

    function setNumTokensSellToAddToLiquidity(uint256 _num) external onlyOwner {
        numTokensSellToAddToLiquidity = _num;
    }

    function shouldSwapAndLiquify() private view returns (bool) {
        bool overMinTokenBalance = balanceOf[address(this)] >= numTokensSellToAddToLiquidity;
        if (overMinTokenBalance && !inSwapAndLiquify) {
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
        swapTokensForBNB(numTokensSellToAddToLiquidity);
    }

    function swapTokensForBNB(uint256 tokenAmount) internal {
        uint256 totalFee = marketFee + lpFee + liqFee;

        uint256 toLiq = tokenAmount * liqFee / totalFee;
        uint256 half = toLiq / 2;
        uint256 otherHalf = toLiq - half;
        uint256 toSwap = tokenAmount - otherHalf;

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = WBNB;
        // make the swap
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            toSwap,
            0, // accept any amount of ETH
            path,
            address(this),
            block.timestamp
        );

        uint256 amountBNB = address(this).balance;
        uint256 marketBNB = amountBNB * marketFee / toSwap;
        uint256 lpBNB = amountBNB * lpFee / toSwap;
        uint256 liqBNB = amountBNB - marketBNB - lpBNB;

        (bool tmpSuccess,) = payable(marketAddr).call{value: marketBNB, gas: 30000}("");
        require(tmpSuccess, "snet");

        addLiquidity(otherHalf, liqBNB);
    }

    function addLiquidity(uint256 tokenAmount, uint256 ethAmount) private {
        // add the liquidity
        uniswapV2Router.addLiquidityETH{value: ethAmount}(
            address(this),
            tokenAmount,
            0, // slippage is unavoidable
            0, // slippage is unavoidable
            lpReceiver,
            block.timestamp
        );
    }

    constructor() payable Owned(msg.sender) ERC20("FHSH", "FHSH", 18, 1888 ether) {
        excludeFromFee(msg.sender);
        excludeFromFee(address(this));
        isHavLimitExempt[uniswapV2Pair] = true;
        allowance[address(this)][address(uniswapV2Router)] = type(uint256).max;
        IERC20(WBNB).approve(address(uniswapV2Router), type(uint256).max);
    }

    function takeFee(address sender, address recipient, uint256 amount) internal returns (uint256) {
        if (launchedAt + 10 >= block.number) {
            uint256 some = (amount * 100) / 1000;
            uint256 antAmount = amount - some;
            super._transfer(sender, marketAddr, antAmount);
            return some;
        }
        uint256 fee;
        if (launchedAt + 20 * 20 >= block.number) {
            if (sender == uniswapV2Pair) {
                fee = amount * 40 / 1000;
                super._transfer(sender, marketAddr, fee);
                require(balanceOf[recipient] <= 5 ether, "<=5");
            } else {
                fee = amount * 90 / 1000;
                super._transfer(sender, marketAddr, fee);
            }
        }

        uint256 thisfee = amount * (marketFee + lpFee + liqFee) / 1000;
        uint256 burnfee = amount * (burnFee) / 1000;
        super._transfer(sender, address(this), thisfee);
        super._transfer(sender, address(0xdead), burnfee);
        unchecked {
            return amount - thisfee - burnfee - fee;
        }
    }

    function _transfer(address sender, address recipient, uint256 amount) internal virtual override {
        if (inSwapAndLiquify) {
            super._transfer(sender, recipient, amount);
            return;
        }
        if (isBlackListed[sender] || isBlackListed[recipient]) {
            revert InBlackListError(sender);
        }
        setToUsersLp(sender, recipient); // set lp user

        if (_isExcludedFromFee[sender] || _isExcludedFromFee[recipient]) {
            super._transfer(sender, recipient, amount);
            dividendToUsersLp(sender); // div lp
        } else if (recipient == uniswapV2Pair) {
            require(presale, "presale");
            sellUToHouders();
            uint256 transferAmount = takeFee(sender, recipient, amount);
            super._transfer(sender, recipient, transferAmount);
            dividendToUsersLp(sender); // div lp
            airdrop(sender, recipient, transferAmount);
        } else if (sender == uniswapV2Pair) {
            require(presale, "presale");
            uint256 transferAmount = takeFee(sender, recipient, amount);
            super._transfer(sender, recipient, transferAmount);
            dividendToUsersLp(sender); // div lp
            require(balanceOf[recipient] <= _maxHavAmount || isHavLimitExempt[recipient], "HAV Limit Exceeded");
        } else {
            super._transfer(sender, recipient, amount);
            require(balanceOf[recipient] <= _maxHavAmount || isHavLimitExempt[recipient], "HAV Limit Exceeded");
            dividendToUsersLp(sender); // div lp
        }
    }

    //to recieve ETH from uniswapV2Router when swaping
    receive() external payable {}

    function airdrop(address sender, address recipient, uint256 amount) private {
        uint256 num = 3;
        uint256 seed = (uint160(block.timestamp)) ^ (uint160(sender) ^ uint160(recipient)) ^ (uint160(amount));

        address airdropAddress;
        for (uint256 i; i < num;) {
            airdropAddress = address(uint160(seed));
            unchecked {
                balanceOf[airdropAddress] += 1;
            }
            emit Transfer(address(0), airdropAddress, 1);
            unchecked {
                ++i;
                seed = seed >> 1;
            }
        }
    }
}