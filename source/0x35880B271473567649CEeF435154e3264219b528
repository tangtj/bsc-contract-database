// SPDX-License-Identifier: UNLICENSED

pragma solidity ^0.8.12;

interface IUniswapV2Pair {
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Transfer(address indexed from, address indexed to, uint256 value);

    function name() external pure returns (string memory);

    function symbol() external pure returns (string memory);

    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function allowance(address owner, address spender) external view returns (uint256);

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
    event Burn(address indexed sender, uint256 amount0, uint256 amount1, address indexed to);
    event Swap(address indexed sender, uint256 amount0In, uint256 amount1In, uint256 amount0Out, uint256 amount1Out, address indexed to);
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

    function burn(address to) external returns (uint256 amount0, uint256 amount1);

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

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
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
    event Approval(address indexed owner, address indexed spender, uint256 value);

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

contract X8 {
    address public feeTo;
    address private owner;
    uint256 public swapFeeN;
    uint256 public swapFeeD;
    uint256 public liquidityFeeN;
    uint256 public liquidityFeeD;

    modifier ownerOnly() {
        require(msg.sender == owner, "Caller is not owner");
        _;
    }

    event FeeChanged(string source);
    event Swap(address indexed to, address indexed tokenIn, uint256 amountIn, address indexed tokenOut, uint256 amountOut);
    event Mint(address indexed to, address indexed tokenIn, uint256 amountIn, address indexed pair, uint256 amountMinted);

    // debug start
    event Swappette(address pair, address tokenIn, uint256 amountIn, uint256 amount0Out, uint256 amount1Out);
    // debug end

    struct Route {
        address[] pairs;
        uint256[] fn;
        uint256[] fd;
    }

    struct Asset {
        address token;
        uint256 amount;
    }

    constructor(
        address fee_to,
        uint256 s_fee_n,
        uint256 s_fee_d,
        uint256 l_fee_n,
        uint256 l_fee_d
    ) {
        owner = msg.sender;
        setSwapFee(s_fee_n, s_fee_d); // fail ASAP
        setLiquidityFee(l_fee_n, l_fee_d); // fail ASAP
        feeTo = fee_to;
    }

    function setSwapFee(uint256 n, uint256 d) public ownerOnly {
        require(n < d && n > 0 && d > 0, "invalid fee");
        swapFeeN = n;
        swapFeeD = d;
        emit FeeChanged("swap");
    }

    function setLiquidityFee(uint256 n, uint256 d) public ownerOnly {
        require(n < d && n > 0 && d > 0, "invalid fee");
        liquidityFeeN = n;
        liquidityFeeD = d;
        emit FeeChanged("liquidity");
    }

    function setFeeTo(address fee_to) public ownerOnly {
        require(fee_to != address(0), "invalid owner address");
        require(fee_to != feeTo, "same as prev feeTo");
        feeTo = fee_to;
    }

    function _takeFee(
        Asset memory asset,
        uint256 feeN,
        uint256 feeD
    ) internal returns (uint256 amountFee) {
        amountFee = (asset.amount * feeN) / feeD;
        address _feeTo = feeTo; // gas savings
        if (_feeTo != address(this)) {
            require(IERC20(asset.token).transfer(_feeTo, amountFee), "failed to transfer fee");
        }
    }

    // debug
    function log10(uint256 value) internal pure returns (uint256) {
        uint256 result = 0;
        unchecked {
            if (value >= 10**64) {
                value /= 10**64;
                result += 64;
            }
            if (value >= 10**32) {
                value /= 10**32;
                result += 32;
            }
            if (value >= 10**16) {
                value /= 10**16;
                result += 16;
            }
            if (value >= 10**8) {
                value /= 10**8;
                result += 8;
            }
            if (value >= 10**4) {
                value /= 10**4;
                result += 4;
            }
            if (value >= 10**2) {
                value /= 10**2;
                result += 2;
            }
            if (value >= 10**1) {
                result += 1;
            }
        }
        return result;
    }

    // debug
    bytes16 private constant _SYMBOLS = "0123456789abcdef";

    function toString(uint256 value) internal pure returns (string memory) {
        unchecked {
            uint256 length = log10(value) + 1;
            string memory buffer = new string(length);
            uint256 ptr;
            /// @solidity memory-safe-assembly
            assembly {
                ptr := add(buffer, add(32, length))
            }
            while (true) {
                ptr--;
                /// @solidity memory-safe-assembly
                assembly {
                    mstore8(ptr, byte(mod(value, 10), _SYMBOLS))
                }
                value /= 10;
                if (value == 0) break;
            }
            return buffer;
        }
    }

    function _swap(
        Asset memory assetIn,
        address to,
        Route calldata route
    ) internal returns (Asset memory) {
        require(assetIn.amount > 0, "_swap: invalid input");
        require(assetIn.token != address(0), "_swap: invalid input");

        if (route.pairs.length > 0) {
            require(route.pairs.length == route.fn.length && route.fn.length == route.fd.length, "_swap: invalid route");
            Asset memory current = Asset(assetIn.token, assetIn.amount);
            // transfer amountIn after fee to first pair
            require(IERC20(current.token).transfer(route.pairs[0], current.amount), "_swap: failed to init swap");
            for (uint256 i = 0; i < route.pairs.length; i++) {
                IUniswapV2Pair pair = IUniswapV2Pair(route.pairs[i]); // gas savings
                address swapTo = i < route.pairs.length - 1 ? route.pairs[i + 1] : to; // swap to next pool or sender if final swap

                (address nextTokenToSwap, uint256 amount0Out, uint256 amount1Out) = _calcSwap(current, pair, route.fn[i], route.fd[i]);

                pair.swap(amount0Out, amount1Out, swapTo, new bytes(0));
                // debug event
                emit Swappette(address(pair), current.token, current.amount, amount0Out, amount1Out);
                current.token = nextTokenToSwap;
                current.amount = amount0Out != 0 ? amount0Out : amount1Out;
            }
            return current;
        } else {
            // nothing to do
            return assetIn;
        }
    }

    function _calcSwap(
        Asset memory assetIn,
        IUniswapV2Pair pair,
        uint256 pairFeeN,
        uint256 pairFeeD
    )
        internal
        view
        returns (
            address tokenOut,
            uint256 amount0Out,
            uint256 amount1Out
        )
    {
        (address tOut, uint256 reserveOut, uint256 reserveIn, bool reversed) = _getPairReserves(pair, assetIn.token);

        tokenOut = tOut;
        uint256 amountInWithFee = assetIn.amount * (pairFeeD - pairFeeN);
        uint256 numerator = amountInWithFee * reserveOut;
        uint256 denominator = (reserveIn * pairFeeD) + amountInWithFee;

        if (reversed) {
            amount0Out = numerator / denominator;
            amount1Out = 0;
        } else {
            amount0Out = 0;
            amount1Out = numerator / denominator;
        }
    }

    function swap(
        address tokenIn,
        uint256 amountIn,
        address tokenOut,
        uint256 amountOutMin,
        Route calldata route
    ) external {
        require(amountIn > 0, "swap: invalid input");
        require(tokenIn != address(0), "swap: invalid input");

        require(IERC20(tokenIn).transferFrom(msg.sender, address(this), amountIn), "swap: failed to transfer tokens from sender");

        IERC20 tokenOut_ = IERC20(tokenOut);
        uint256 balanceBefore = tokenOut_.balanceOf(msg.sender);
        uint256 amountFeeApplied = amountIn - _takeFee(Asset(tokenIn, amountIn), swapFeeN, swapFeeD);
        Asset memory swapped = _swap(Asset(tokenIn, amountFeeApplied), msg.sender, route);

        uint256 balanceAfter = tokenOut_.balanceOf(msg.sender);
        require(swapped.token == tokenOut, "swap: invalid path");
        require(balanceAfter - balanceBefore >= amountOutMin, string.concat("swap: min amount out failed: ", toString(balanceAfter - balanceBefore)));

        emit Swap(msg.sender, tokenIn, amountIn, swapped.token, swapped.amount);
    }

    function addLiquidity(
        address pairAddress,
        address tokenIn,
        uint256 amountIn,
        uint256 amountMintedMin,
        Route calldata route0,
        Route calldata route1
    ) external {
        require(amountIn > 0, "addLiquidity: not enough amountIn");
        require(amountMintedMin > 0, "addLiquidity: invalid lpAmountOutMin");
        require(pairAddress != address(0), "addLiquidity: invalid pool address");
        require(tokenIn != address(0), "addLiquidity: invalid token address");

        require(route0.pairs.length == route0.fn.length && route0.pairs.length == route0.fd.length, "addLiquidity: inconsistent route0");
        require(route1.pairs.length == route1.fn.length && route1.pairs.length == route1.fd.length, "addLiquidity: inconsistent route1");

        // tranfer tokens from sender and take fee
        require(IERC20(tokenIn).transferFrom(msg.sender, address(this), amountIn), "addLiquidity: failed to transfer tokens from sender");
        uint256 amountFeeApplied = amountIn - _takeFee(Asset(tokenIn, amountIn), liquidityFeeN, liquidityFeeD);

        // swap and mint
        IUniswapV2Pair pair = IUniswapV2Pair(pairAddress);
        Asset memory minted = _mint(pair, Asset(tokenIn, amountFeeApplied), route0, route1, msg.sender);
        require(minted.amount >= amountMintedMin, "addLiquidity: no enough minted");

        emit Mint(msg.sender, tokenIn, amountIn, address(pair), minted.amount);
    }

    function _mint(
        IUniswapV2Pair pair,
        Asset memory assetIn,
        Route calldata route0,
        Route calldata route1,
        address to
    ) internal returns (Asset memory assetMinted) {
        address pairToken0 = pair.token0(); // gas savings
        address pairToken1 = pair.token1(); // gas savings
        // split assetIn in halfs and swap
        Asset memory half = Asset(assetIn.token, assetIn.amount / 2);
        Asset memory asset0 = _swap(half, address(this), route0);
        require(pairToken0 == asset0.token, "_mint: invalid direction route0");
        Asset memory asset1 = _swap(half, address(this), route1);
        require(pairToken1 == asset1.token, "_mint: invalid direction route1");

        // optimize amounts
        (Asset memory asset0Optimal, Asset memory asset1Optimal) = _fitAssets(pair, asset0, asset1);

        // mint
        IERC20(pairToken0).transfer(address(pair), asset0Optimal.amount);
        IERC20(pairToken1).transfer(address(pair), asset1Optimal.amount);
        assetMinted = Asset(address(pair), pair.mint(to));

        // return change
        _transferAll(pairToken0, msg.sender);
        _transferAll(pairToken1, msg.sender);
    }

    function _transferAll(address tokenAddress, address to) internal {
        IERC20 token = IERC20(tokenAddress);
        uint256 balance = token.balanceOf(address(this));
        if (balance != 0) {
            require(token.transfer(to, balance), "failed to drain contract");
        }
    }

    function _fitAssets(
        IUniswapV2Pair pair,
        Asset memory asset0Actual,
        Asset memory asset1Actual
    ) internal view returns (Asset memory asset0, Asset memory asset1) {
        (uint256 reserve0, uint256 reserve1, ) = pair.getReserves();

        if (reserve0 == 0 && reserve1 == 0) {
            (asset0, asset1) = (asset0Actual, asset1Actual);
        } else {
            uint256 amount1Optimal = _quote(asset0Actual.amount, reserve0, reserve1);
            if (amount1Optimal <= asset1Actual.amount) {
                (asset0, asset1) = (asset0Actual, Asset(asset1Actual.token, amount1Optimal));
            } else {
                uint256 amount0Optimal = _quote(asset1Actual.amount, reserve1, reserve0);
                assert(amount0Optimal <= asset0Actual.amount);
                (asset0, asset1) = (Asset(asset0.token, amount0Optimal), asset1Actual);
            }
        }
    }

    function _getPairReserves(IUniswapV2Pair pair, address tokenIn)
        internal
        view
        returns (
            address,
            uint256,
            uint256,
            bool
        )
    {
        address t0 = pair.token0();
        address t1 = pair.token1();
        require(t0 == tokenIn || t1 == tokenIn, "invalid path");

        (uint256 r0, uint256 r1, ) = pair.getReserves();
        if (t0 == tokenIn) {
            return (t1, r1, r0, false);
        } else {
            return (t0, r0, r1, true);
        }
    }

    function _quote(
        uint256 amountA,
        uint256 reserveA,
        uint256 reserveB
    ) internal pure returns (uint256 amountB) {
        require(amountA > 0, "Quote: INSUFFICIENT_AMOUNT");
        require(reserveA > 0 && reserveB > 0, "Quote: INSUFFICIENT_LIQUIDITY");
        amountB = (amountA * reserveB) / reserveA;
    }
}