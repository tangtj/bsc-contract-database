// File: @uniswap/v2-core/contracts/interfaces/IUniswapV2Factory.sol

pragma solidity >=0.5.0;

interface IUniswapV2Factory {
    event PairCreated(
        address indexed token0,
        address indexed token1,
        address pair,
        uint256
    );

    function feeTo() external view returns (address);

    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB)
        external
        view
        returns (address pair);

    function allPairs(uint256) external view returns (address pair);

    function allPairsLength() external view returns (uint256);

    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);

    function setFeeTo(address) external;

    function setFeeToSetter(address) external;
}

// File: @uniswap/v2-core/contracts/interfaces/IUniswapV2Pair.sol

pragma solidity >=0.5.0;

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

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

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
        returns (
            uint112 reserve0,
            uint112 reserve1,
            uint32 blockTimestampLast
        );

    function price0CumulativeLast() external view returns (uint256);

    function price1CumulativeLast() external view returns (uint256);

    function kLast() external view returns (uint256);

    function mint(address to) external returns (uint256 liquidity);

    function burn(address to)
        external
        returns (uint256 amount0, uint256 amount1);

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
pragma solidity >=0.6.2;

interface IUniswapV2Router01 {
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
    )
        external
        returns (
            uint256 amountA,
            uint256 amountB,
            uint256 liquidity
        );

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
        returns (
            uint256 amountToken,
            uint256 amountETH,
            uint256 liquidity
        );

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

    function getAmountsOut(uint256 amountIn, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);

    function getAmountsIn(uint256 amountOut, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
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
// File: Investment.sol

pragma solidity ^0.8.0;

// import "@pancakeswap/pancakeswap-lib/contracts/interfaces/IPancakeRouter02.sol";
interface IERC20 {
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Transfer(address indexed from, address indexed to, uint256 value);

    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;
}

contract InvestmentContract {
    //check deploy
    //1 timestamps for daily Rewards
    //2 Address of tokens
    //3 address of router
    address public owner;

    uint256 public totalUsers;
    bool public returnInBusd = true;
    uint256 public MIN_AMT = 100;
    uint256 totalDeposits;
    //0x782e720D674dBb4f77F63c118Faf98d9bC573136
    //0x55d398326f99059fF775485246999027B3197955
    address public AIMB = 0x782e720D674dBb4f77F63c118Faf98d9bC573136;
    address public BUSD = 0x55d398326f99059fF775485246999027B3197955;
    IERC20 public token = IERC20(AIMB);
    IERC20 public usdt = IERC20(BUSD);
    // IUniswapV2Factory factory =
    //     IUniswapV2Factory(0x1097053Fd2ea711dad45caCcc45EfF7548fCB362);
    //0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73

    struct User {
        uint256 depositAmount;
        uint256 depositTime;
        uint256 totalEarnings;
        uint256 dailyRewardsTime;
        uint256 withdrawalAmount;
        address referred_by;
        uint256 level1;
        uint256 level2;
        uint256 level3;
        uint256 max_cap;
    }
    uint256 public MAX_CAP = 300;

    mapping(address => User) public users;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }

    function deposit(
        address _user,
        address _referral,
        uint256 _amount
    ) external {
        User storage user = users[_user];
        uint256 usdtAmount = returnUsdtAmount(_amount);
        require(
            usdtAmount >= (MIN_AMT * 1e18),
            "Amount is less than minimum amount"
        );
        require(
            token.transferFrom(_user, address(this), _amount),
            "USDT transfer failed"
        );
        if (_referral != address(0) && _referral != msg.sender) {
            User storage referrer = users[_referral];
            // referee(_referral, _amount);
            user.referred_by = _referral;
            if (
                referrer.totalEarnings <= referrer.max_cap &&
                referrer.depositAmount > 0
            ) {
                referrer.totalEarnings += ((_amount * 5) / 100);
                referrer.withdrawalAmount += ((_amount * 5) / 100);
                if (returnInBusd) {
                    require(
                        usdt.transfer(
                            _referral,
                            returnUsdtAmount(((_amount * 5) / 100))
                        ),
                        "Transfer Failed"
                    );
                } else {
                    require(
                        token.transfer(_referral, ((_amount * 5) / 100)),
                        "Transfer Failed"
                    );
                }
            }
        }
        // Update user data
        user.max_cap = (_amount * 300) / 100;
        user.dailyRewardsTime = block.timestamp + 24 hours;
        user.depositAmount += _amount;
        if (user.depositTime <= 0) {
            user.depositTime = block.timestamp;
        }
        //  user.dailyRewardsTime+=(block.timestamp+24 hours);
        totalDeposits += _amount;
        totalUsers++;
    }

    function EarnDailyRewards() external {
        User storage user = users[msg.sender];
        require(
            block.timestamp <=
                user.depositTime +
                    (getPlanDuration(user.depositTime) * 24 * 60 * 60),
            "Deposit is matured "
        );
        require(user.depositAmount > 0, "No active deposit");
        require(user.totalEarnings <= user.max_cap, "Already Reached Max Cap");
        require(
            block.timestamp > user.dailyRewardsTime,
            "Already Claimed ,Please come back after 24 hours"
        );
        referee(
            user.referred_by,
            ((calculateROI(user.depositAmount) * user.depositAmount) / 10000) *
                calculateDaysBetween(user.dailyRewardsTime, block.timestamp)
        );
        uint256 earningAmount = ((user.depositAmount *
            (calculateROI(user.depositAmount))) / 10000)*calculateDaysBetween(user.dailyRewardsTime, block.timestamp);
        user.withdrawalAmount += earningAmount;
        user.dailyRewardsTime = block.timestamp + 24 hours;
        if (returnInBusd) {
            require(
                usdt.transfer(msg.sender, returnUsdtAmount(earningAmount)),
                "trasnferFailded"
            );
        } else {
            require(
                token.transfer(msg.sender, earningAmount),
                "trasnferFailded"
            );
        }
        user.totalEarnings += earningAmount;
    }

    function priceOfToken() public view returns (uint256) {
        IUniswapV2Router02 router = IUniswapV2Router02(
            0x10ED43C718714eb63d5aA57B78B54704E256024E
        );
        address[] memory path = new address[](2);
        path[0] = AIMB;
        path[1] = BUSD;
        uint256[] memory amountsOut = router.getAmountsOut(1e18, path); // 1e18 is the amount of Token A (in wei)
        return amountsOut[1]; // The price of Token A in terms of Token B
        // return 318841499845546159;
    }

    function returnUsdtAmount(uint256 AIMBamount)
        public
        view
        returns (uint256)
    {
        uint256 price = priceOfToken();
        uint256 finalAmount = (price * AIMBamount) / 10**18;
        return finalAmount;
    }

    function withdraw() external {
        User storage user = users[msg.sender];
        require(user.depositAmount > 0, "No active deposit");
        require(
            block.timestamp >=
                user.depositTime +
                    (getPlanDuration(user.depositTime) * 24 * 60 * 60),
            "Deposit is not matured yet"
        );

        uint256 totalEarnings = user.totalEarnings - user.withdrawalAmount;
        require(totalEarnings > 0, "No earnings to withdraw");
        // uint256 withdrawalFee = (totalEarnings * 5) / 100;
        uint256 netWithdrawalAmount = totalEarnings - (totalEarnings * 5) / 100;
        user.totalEarnings = 0;
        user.withdrawalAmount += netWithdrawalAmount;
        // totalWithdrawn += netWithdrawalAmount;
        // Transfer USDT tokens to the user
        if (returnInBusd) {
            require(
                usdt.transfer(
                    msg.sender,
                    returnUsdtAmount(netWithdrawalAmount)
                ),
                "USDT transfer failed"
            );
        } else {
            require(
                token.transfer(msg.sender, netWithdrawalAmount),
                "USDT transfer failed"
            );
        }
    }

    function withdrawStakedAmount() external {
        User storage user = users[msg.sender];
        require(
            block.timestamp >= user.depositTime + 365 days,
            "Staked amount will be realsed after 365 days of deposit"
        );
        require(
            token.transfer(msg.sender, user.depositAmount),
            "Amount transfer failed"
        );
    }

    function getPlanDuration(uint256 _depositAmount)
        internal
        pure
        returns (uint256)
    {
        if (_depositAmount < 2500 * 10**18) {
            return 300 days;
        } else if (_depositAmount < 10000 * 10**18) {
            return 240 days;
        } else {
            return 200 days;
        }
    }

    function calculateROI(uint256 _depositAmount)
        internal
        pure
        returns (uint256)
    {
        uint256 planDuration = getPlanDuration(_depositAmount);
        if (planDuration == 300 days) {
            return 100;
        } else if (planDuration == 240 days) {
            return 125;
        } else {
            return 150;
        }
    }

    function referee(address ref_add, uint256 _amount) internal  returns (bool) {
        require(ref_add != msg.sender, " You cannot refer yourself ");
        address level1 = users[msg.sender].referred_by;
        address level2 = users[level1].referred_by;
        address level3 = users[level2].referred_by;
        if (
            (level1 != msg.sender) &&
            (level1 != address(0)) &&
            users[level1].depositAmount > 0
        ) {
            users[level1].level1 += ((_amount * 15) / 100);
            if (returnInBusd) {
                require(
                    usdt.transfer(
                        level1,
                        returnUsdtAmount((_amount * 15) / 100)
                    ),
                    "Transfer Failer"
                );
            } else {
                require(
                    token.transfer(level1, (_amount * 15) / 100),
                    "Transfer Failer"
                );
            }
        }
        if (
            (level2 != msg.sender) &&
            (level2 != address(0)) &&
            users[level2].depositAmount > 0
        ) {
            users[level2].level2 += ((_amount * 10) / 100);
            if (returnInBusd) {
                require(
                    usdt.transfer(
                        level2,
                        returnUsdtAmount((_amount * 10) / 100)
                    ),
                    "Transfer Failer"
                );
            } else {
                require(
                    token.transfer(level2, (_amount * 10) / 100),
                    "Transfer Failer"
                );
            }
        }
        if (
            (level3 != msg.sender) &&
            (level3 != address(0)) &&
            users[level3].depositAmount > 0
        ) {
            users[level3].level3 += ((_amount * 5) / 100);
            if (returnInBusd) {
                require(
                    usdt.transfer(
                        level3,
                        returnUsdtAmount((_amount * 5) / 100)
                    ),
                    "Transfer Failer"
                );
            } else {
                require(
                    token.transfer(level3, (_amount * 5) / 100),
                    "Transfer Failer"
                );
            }
        }
        return true;
    }

    function withdrawBusd(uint256 _amount) external onlyOwner {
        // require(msg.sender==owner,"Only owner");
        require(
            usdt.transfer(owner, _amount),
            "Succesfull Transfer"
        );
    }

    function withdrawAimb(uint256 _amount) external onlyOwner {
        // require(msg.sender==owner,"Only owner");
        require(
            token.transfer(owner, _amount),
            "Succesfull Transfer"
        );
    }

    function AddStake(
        address _user,
        uint256 _amount,
        address _referral
    ) external onlyOwner {
        User storage user = users[_user];
        if (_referral != address(0) && _referral != msg.sender) {
            user.referred_by = _referral;
        }
        // Update user data
        user.max_cap = (_amount * 300) / 100;
        user.dailyRewardsTime = block.timestamp + 24 hours;
        user.depositAmount += _amount;
        user.depositTime = block.timestamp;
        totalDeposits += _amount;
        // user.dailyRewardsTime += (block.timestamp + 24 hours);
        totalUsers++;
    }

    function changeMINamount(uint256 amount) external onlyOwner {
        MIN_AMT = amount;
    }

    function changeReturnInBUSD(bool value) external onlyOwner {
        returnInBusd = value;
    }

    function calculateDaysBetween(uint256 timestamp1, uint256 timestamp2)
        public 
        pure
        returns (uint256)
    {
        require(
            timestamp1 <= timestamp2,
            "Timestamp1 should be less than or equal to Timestamp2"
        );

        // Calculate the difference in seconds
        uint256 timeDifference = timestamp2+ 24 hours - timestamp1;

        // Convert seconds to days (86400 seconds in a day)
        //86400
        uint256 daysDifference = timeDifference / 86400;

        return daysDifference;
    }
}