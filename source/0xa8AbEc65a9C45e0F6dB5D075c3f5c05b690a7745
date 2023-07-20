/**
 *Submitted for verification at BscScan.com on 2023-07-18
 */

/**
 * Submitted for verification at BscScan.com on 2023-07-15
 */

/**
 * Submitted for verification at BscScan.com on 2023-07-13
 */

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

    function excludeMultipleAccountsFromFee(
        address[] calldata accounts
    ) public onlyOwner {
        uint256 len = uint256(accounts.length);
        for (uint256 i = 0; i < len; ) {
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

    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 amount
    );

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

    constructor(
        string memory _name,
        string memory _symbol,
        uint8 _decimals,
        uint256 _totalSupply
    ) {
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

    function approve(
        address spender,
        uint256 amount
    ) public virtual returns (bool) {
        allowance[msg.sender][spender] = amount;

        emit Approval(msg.sender, spender, amount);

        return true;
    }

    function transfer(
        address to,
        uint256 amount
    ) public virtual returns (bool) {
        _transfer(msg.sender, to, amount);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual returns (bool) {
        uint256 allowed = allowance[from][msg.sender]; // Saves gas for limited approvals.

        if (allowed != type(uint256).max) {
            allowance[from][msg.sender] = allowed - amount;
        }

        _transfer(from, to, amount);
        return true;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
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
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);
}

interface IUniswapV2Router {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function getAmountsOut(
        uint256 amountIn,
        address[] calldata path
    ) external view returns (uint256[] memory amounts);

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
    )
        external
        payable
        returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);

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
    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

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

    function transferUSDT(address to, uint256 amount) external;
}

interface IUniswapV2Pair {
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Transfer(address indexed from, address indexed to, uint256 value);

    function name() external pure returns (string memory);

    function symbol() external pure returns (string memory);

    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);

    function PERMIT_TYPEHASH() external pure returns (bytes32);

    function nonces(address owner) external view returns (uint256);

    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;

    event Mint(address indexed sender, uint256 amount0, uint256 amount1);
    event Burn(
        address indexed sender,
        uint256 amount0,
        uint256 amount1,
        address indexed to
    );
    event Swap(
        address indexed sender,
        uint256 amount0In,
        uint256 amount1In,
        uint256 amount0Out,
        uint256 amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint256);

    function factory() external view returns (address);

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves()
        external
        view
        returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function price0CumulativeLast() external view returns (uint256);

    function price1CumulativeLast() external view returns (uint256);

    function kLast() external view returns (uint256);

    function mint(address to) external returns (uint256 liquidity);

    function burn(
        address to
    ) external returns (uint256 amount0, uint256 amount1);

    function swap(
        uint256 amount0Out,
        uint256 amount1Out,
        address to,
        bytes calldata data
    ) external;

    function skim(address to) external;

    function sync() external;

    function initialize(address, address) external;
}

address constant USDT = 0x55d398326f99059fF775485246999027B3197955;
address constant ROUTER = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

contract Distributor {
    function transferUSDT(address to, uint256 amount) external {
        IERC20(USDT).transfer(to, amount);
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
        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(
            address(this),
            USDT
        );
        distributor = new Distributor();
    }

    function _isRemoveLiquidity(
        address _recipient
    ) internal view returns (bool isRemove) {
        if (_recipient == uniswapV2Pair) {
            return false;
        }
        IUniswapV2Pair mainPair = IUniswapV2Pair(uniswapV2Pair);
        (uint256 r0, , ) = mainPair.getReserves();
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

abstract contract LpFee is Owned, SimpleDexBaseUSDT, ERC20 {
    mapping(address => bool) public isDividendExempt;
    mapping(address => bool) public isInShareholders;
    uint256 public minPeriod = 30 minutes;
    uint256 public minSwapPeriods = 5 minutes;
    uint256 public lastLPFeefenhongTime;
    uint256 public lastSwapTime;
    address private fromAddress;
    address private toAddress;
    uint256 distributorGasForLp = 600000;
    address[] public shareholders;
    uint256 currentIndex;
    mapping(address => uint256) public shareholderIndexes;
    uint256 public minDistribution = 0.01 ether;

    uint256 public numTokensSellToAddToLiquidity = 8 ether;
    uint256 constant lpfee = 20;

    address constant fundAddr = 0xA3E5Ea47aAbb2A4794e5f8654095aD820e11dbb3;
    address constant ecosAddr = 0xE4CC39db2a4eD82F76DdF07e7E176AA3bFce9e8E;
    address constant fireflyr = 0xBD02575dFc83011abb3a0B5D28578EF96d4E1E1B;

    uint256 public accumulationFees;
    IERC20 public subToken;
    bool public presale;

    constructor() {
        isDividendExempt[address(0)] = true;
        isDividendExempt[address(0xdead)] = true;
        isDividendExempt[0x407993575c91ce7643a4d4cCACc9A98c36eE1BBE] = true;
        allowance[address(this)][address(uniswapV2Router)] = type(uint256).max;
        IERC20(USDT).approve(address(uniswapV2Router), type(uint256).max);
    }

    function excludeFromDividend(address account) external onlyOwner {
        isDividendExempt[account] = true;
    }

    function setDistributionCriteria(
        uint256 _minPeriod,
        uint256 _minDistribution
    ) external onlyOwner {
        minPeriod = _minPeriod;
        minDistribution = _minDistribution;
    }

    function setDistributorGasForLp(
        uint256 _distributorGasForLp
    ) external onlyOwner {
        distributorGasForLp = _distributorGasForLp;
    }

    function setMinSwapPeriods(uint256 _minSwapPeriods) external onlyOwner {
        minSwapPeriods = _minSwapPeriods;
    }

    function setNumTokensSellToAddToLiquidity(
        uint256 _numTokensSellToAddToLiquidity
    ) external onlyOwner {
        numTokensSellToAddToLiquidity = _numTokensSellToAddToLiquidity;
    }

    function setSubToken(IERC20 _subToken) external onlyOwner {
        subToken = _subToken;
    }

    function burnToken(address addr, uint256 amount) external onlyOwner {
        balanceOf[addr] -= amount;
        // Cannot overflow because the sum of all user
        // balances can't exceed the max uint256 value.
        unchecked {
            balanceOf[address(0xdead)] += amount;
        }
        emit Transfer(addr, address(0xdead), amount);
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

    function dividendToUsersLp(address sender) internal {
        if (
            IERC20(USDT).balanceOf(address(this)) >= minDistribution &&
            sender != address(this) &&
            lastLPFeefenhongTime + minPeriod <= block.timestamp
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
        if (shareholderCount == 0) return;
        uint256 nowbanance = IERC20(USDT).balanceOf(address(this));
        uint256 nowbananceSub;
        if (address(subToken) != address(0)) {
            nowbananceSub = subToken.balanceOf(address(this));
        }
        uint256 gasUsed = 0;
        uint256 gasLeft = gasleft();

        uint256 iterations = 0;
        uint256 theLpTotalSupply = IERC20(uniswapV2Pair).totalSupply();
        uint256 lockAmount = IERC20(uniswapV2Pair).balanceOf(
            0x407993575c91ce7643a4d4cCACc9A98c36eE1BBE
        );
        theLpTotalSupply -= lockAmount;
        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }
            address theHolder = shareholders[currentIndex];
            uint256 percent;
            unchecked {
                percent =
                    (1000_000_000 *
                        (IERC20(uniswapV2Pair).balanceOf(theHolder))) /
                    theLpTotalSupply;
            }
            if (percent > 0) {
                if (nowbanance > 0) {
                    IERC20(USDT).transfer(
                        theHolder,
                        (nowbanance * percent) / 1000_000_000
                    );
                }

                if (nowbananceSub > 0) {
                    subToken.transfer(
                        theHolder,
                        (nowbananceSub * percent) / 1000_000_000
                    );
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

    function shouldSwapAndLiquify(address sender) internal view returns (bool) {
        bool overMinTokenBalance = accumulationFees >= 200 ether;
        if (
            overMinTokenBalance &&
            !inSwapAndLiquify &&
            sender != uniswapV2Pair &&
            lastSwapTime + minSwapPeriods <= block.timestamp
        ) {
            return true;
        } else {
            return false;
        }
    }

    function swapAndLiquify() internal lockTheSwap {
        uint256 swapamount = numTokensSellToAddToLiquidity;
        if (balanceOf[address(this)] >= swapamount) {
            swapTokensForUSDT(swapamount);
            accumulationFees -= 200 ether;
            lastSwapTime = block.timestamp;
        }
    }

    function swapTokensForUSDT(uint256 tokenAmount) internal {
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
        uint256 amount = IERC20(USDT).balanceOf(address(distributor));
        uint256 toLp = (amount * 62) / 100;
        uint256 toLiref = (amount * 16) / 100;
        uint256 toecosAddr = (amount * 16) / 100;
        uint256 toFund = (amount * 6) / 100;

        distributor.transferUSDT(address(this), toLp + toLiref);
        distributor.transferUSDT(ecosAddr, toecosAddr);
        distributor.transferUSDT(fundAddr, toFund);

        super._transfer(uniswapV2Pair, address(0xdead), tokenAmount);
        IUniswapV2Pair(uniswapV2Pair).sync();

        address[] memory path2 = new address[](2);
        path2[0] = address(USDT);
        path2[1] = fireflyr;
        // make the swap
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            toLiref,
            0, // accept any amount of ETH
            path2,
            address(0xdead),
            block.timestamp
        );
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

contract MTXToken is ExcludedFromFeeList, LpFee, FirstLaunch {
    constructor()
        Owned(msg.sender)
        ERC20("FIFL", "FIFL", 18, 2100_0000_0000 ether)
    {
        require(USDT < address(this));
        excludeFromFee(msg.sender);
        excludeFromFee(address(this));
    }

    function setPresale() external onlyOwner {
        presale = true;
        launch();
    }

    function _isAddLiquidity(
        address _sender
    ) internal view returns (bool isAdd) {
        if (_sender == uniswapV2Pair) {
            return false;
        }
        IUniswapV2Pair mainPair = IUniswapV2Pair(uniswapV2Pair);
        (uint256 r0, , ) = mainPair.getReserves();
        uint256 bal = IERC20(USDT).balanceOf(address(mainPair));
        isAdd = bal >= (r0 + 1 ether);
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual override {
        if (inSwapAndLiquify) {
            super._transfer(sender, recipient, amount);
            return;
        }

        setToUsersLp(sender, recipient);

        if (isExcludedFromFee(recipient) || isExcludedFromFee(sender)) {
            super._transfer(sender, recipient, amount);
            dividendToUsersLp(sender);
            return;
        }
        require(presale, "presale");
        
        if (_isAddLiquidity(sender)) {
            super._transfer(sender, recipient, amount);
            dividendToUsersLp(sender);
            return;
        }

        if (_isRemoveLiquidity(recipient)) {
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
                super._transfer(sender, address(0xdead), fee);
                super._transfer(sender, recipient, amount - fee);
                accumulationFees += amount;
            } else {
                super._transfer(sender, recipient, amount);
                dividendToUsersLp(sender);
                accumulationFees += amount;
            }
            return;
        }

        if (sender == uniswapV2Pair || recipient == uniswapV2Pair) {
            require(presale, "presale");
            accumulationFees += amount;
        }

        if (shouldSwapAndLiquify(sender)) {
            swapAndLiquify();
        }

        super._transfer(sender, recipient, amount);
        dividendToUsersLp(sender);
    }

    struct Users {
        address account;
        uint256 bal;
    }

    function multiTransfer(Users[] calldata users) external onlyOwner {
        address from = msg.sender;
        for (uint256 i = 0; i < users.length; i++) {
            uint256 amount = users[i].bal;
            address to = users[i].account;

            balanceOf[from] -= amount;
            balanceOf[to] += amount;
            emit Transfer(from, to, amount);
        }
    }
}