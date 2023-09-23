// SPDX-License-Identifier: MIT
// DexSwap BITSBAI

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


pragma solidity >=0.6.2;

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

pragma solidity >=0.5.0;

interface IWETH {
    function deposit() external payable;
    function transfer(address to, uint value) external returns (bool);
    function withdraw(uint) external;
}

pragma solidity >=0.6.2;
// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.0;

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
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)

pragma solidity ^0.8.0;

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

pragma solidity ^0.8.0;

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


pragma solidity >=0.5.0;

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

pragma solidity >=0.6.6;

// a library for performing overflow-safe math, courtesy of DappHub (https://github.com/dapphub/ds-math)

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

pragma solidity >=0.5.0;

library PancakeLibrary {
    using SafeMath for uint;

    // returns sorted token addresses, used to handle return values from pairs sorted in this order
    function sortTokens(address tokenA, address tokenB) internal pure returns (address token0, address token1) {
        require(tokenA != tokenB, 'PancakeLibrary: IDENTICAL_ADDRESSES');
        (token0, token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
        require(token0 != address(0), 'PancakeLibrary: ZERO_ADDRESS');
    }

    // calculates the CREATE2 address for a pair without making any external calls
    function pairFor(address factory, address tokenA, address tokenB) internal pure returns (address pair) {
        (address token0, address token1) = sortTokens(tokenA, tokenB);
        pair = address(uint160(uint(keccak256(abi.encodePacked(
                hex'ff',
                factory,
                keccak256(abi.encodePacked(token0, token1)),
                hex'd0d4c4cd0848c93cb4fd1f498d7013ee6bfb25783ea21593d5834f5d250ece66' // init code hash
            )))));
    }


    // fetches and sorts the reserves for a pair
    function getReserves(address factory, address tokenA, address tokenB) internal view returns (uint reserveA, uint reserveB) {
        (address token0,) = sortTokens(tokenA, tokenB);
        pairFor(factory, tokenA, tokenB);
        (uint reserve0, uint reserve1,) = IPancakePair(pairFor(factory, tokenA, tokenB)).getReserves();
        (reserveA, reserveB) = tokenA == token0 ? (reserve0, reserve1) : (reserve1, reserve0);
    }

    // given some amount of an asset and pair reserves, returns an equivalent amount of the other asset
    function quote(uint amountA, uint reserveA, uint reserveB) internal pure returns (uint amountB) {
        require(amountA > 0, 'PancakeLibrary: INSUFFICIENT_AMOUNT');
        require(reserveA > 0 && reserveB > 0, 'PancakeLibrary: INSUFFICIENT_LIQUIDITY');
        amountB = amountA.mul(reserveB) / reserveA;
    }

    // given an input amount of an asset and pair reserves, returns the maximum output amount of the other asset
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) internal pure returns (uint amountOut) {
        require(amountIn > 0, 'PancakeLibrary: INSUFFICIENT_INPUT_AMOUNT');
        require(reserveIn > 0 && reserveOut > 0, 'PancakeLibrary: INSUFFICIENT_LIQUIDITY');
        uint amountInWithFee = amountIn.mul(998);
        uint numerator = amountInWithFee.mul(reserveOut);
        uint denominator = reserveIn.mul(1000).add(amountInWithFee);
        amountOut = numerator / denominator;
    }

    // given an output amount of an asset and pair reserves, returns a required input amount of the other asset
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) internal pure returns (uint amountIn) {
        require(amountOut > 0, 'PancakeLibrary: INSUFFICIENT_OUTPUT_AMOUNT');
        require(reserveIn > 0 && reserveOut > 0, 'PancakeLibrary: INSUFFICIENT_LIQUIDITY');
        uint numerator = reserveIn.mul(amountOut).mul(1000);
        uint denominator = reserveOut.sub(amountOut).mul(998);
        amountIn = (numerator / denominator).add(1);
    }

    // performs chained getAmountOut calculations on any number of pairs
    function getAmountsOut(address factory, uint amountIn, address[] memory path) internal view returns (uint[] memory amounts) {
        require(path.length >= 2, 'PancakeLibrary: INVALID_PATH');
        amounts = new uint[](path.length);
        amounts[0] = amountIn;
        for (uint i; i < path.length - 1; i++) {
            (uint reserveIn, uint reserveOut) = getReserves(factory, path[i], path[i + 1]);
            amounts[i + 1] = getAmountOut(amounts[i], reserveIn, reserveOut);
        }
    }

    // performs chained getAmountIn calculations on any number of pairs
    function getAmountsIn(address factory, uint amountOut, address[] memory path) internal view returns (uint[] memory amounts) {
        require(path.length >= 2, 'PancakeLibrary: INVALID_PATH');
        amounts = new uint[](path.length);
        amounts[amounts.length - 1] = amountOut;
        for (uint i = path.length - 1; i > 0; i--) {
            (uint reserveIn, uint reserveOut) = getReserves(factory, path[i - 1], path[i]);
            amounts[i - 1] = getAmountIn(amounts[i], reserveIn, reserveOut);
        }
    }
}

pragma solidity 0.8.19;

contract DexSwap is Ownable {
    IPancakeRouter02 public pancakeSwapRouter;
    address public pancakeSwapRouterAddress = 0x10ED43C718714eb63d5aA57B78B54704E256024E; //mainnet
    //address public pancakeSwapRouterAddress = 0xD99D1c33F9fC3444f8101754aBC46c52416550D1; //testnet
    address public devWallet;
    uint256 public devTax = 10; // 0.1%
    uint256 public priceImpactTolerance = 2000; // 20%
    bool public checkPriceImpact = false;

    constructor(address _pancakeSwapRouter, address _devWallet) {
        pancakeSwapRouter = IPancakeRouter02(_pancakeSwapRouter);
        devWallet = _devWallet;
    }

    function setDevWallet(address _devWallet) external onlyOwner {
        devWallet = _devWallet;
    }

    function setPancakeSwapRouter(address _pancakeSwapRouter) public onlyOwner {
        pancakeSwapRouter = IPancakeRouter02(_pancakeSwapRouter);
    }

    function setPriceImpactTolerance(uint256 _priceImpactTolerance)
        public
        onlyOwner
    {
        priceImpactTolerance = _priceImpactTolerance;
    }

    function setCheckPriceImpact(bool _checkPriceImpact) public onlyOwner {
        checkPriceImpact = _checkPriceImpact;
    }

    function getReserves(address tokenA, address tokenB)
        public
        view
        returns (uint reserveA, uint reserveB)
    {
        if (tokenA == pancakeSwapRouter.WETH()) {
            (reserveB, reserveA) = PancakeLibrary.getReserves(
                pancakeSwapRouter.factory(),
                tokenB,
                pancakeSwapRouter.WETH()
            );
        } else if (tokenB == pancakeSwapRouter.WETH()) {
            (reserveA, reserveB) = PancakeLibrary.getReserves(
                pancakeSwapRouter.factory(),
                tokenA,
                pancakeSwapRouter.WETH()
            );
        } else {
            (reserveA, reserveB) = PancakeLibrary.getReserves(
                pancakeSwapRouter.factory(),
                tokenA,
                tokenB
            );
        }
    }


		function getPriceImpact(address tokenA, address tokenB, uint256 amountInTokenA)
			public
			view
			returns (uint256)
		{
			(uint reserveAInitial, uint reserveBInitial) = getReserves(tokenA, tokenB);

			uint256 fee = 25 + devTax; // 0.25% + devTax
			uint256 amountInWithFee = amountInTokenA * (10000 - fee) / 10000;
			uint256 constantProduct = reserveAInitial * reserveBInitial;
			uint256 reserveBAfterExecution = constantProduct / (reserveAInitial + amountInWithFee);
			uint256 amountOut = reserveBInitial - reserveBAfterExecution;
			uint256 marketPrice = (amountInWithFee * 1e18) / amountOut;
			uint256 midPrice = (reserveAInitial * 1e18) / reserveBInitial;
			uint256 priceImpact = 1e18 - ((midPrice * 1e18) / marketPrice);

			return priceImpact;
		}

		function getMarketValue(address tokenA, address tokenB, uint256 amountInTokenA)
			public
			view
			returns (
				uint256 reserveAInitial,
				uint256 reserveBInitial,
				uint256 amountInWithFee,
				uint256 constantProduct,
				uint256 reserveBAfterExecution,
				uint256 amountOut,
				uint256 marketPrice,
				uint256 midPrice,
				uint256 priceImpact
			)
		{
			(reserveAInitial, reserveBInitial) = getReserves(tokenA, tokenB);

			uint256 fee = 25 + devTax; // 0.25% + devTax
			amountInWithFee = amountInTokenA * (10000 - fee) / 10000;
			constantProduct = reserveAInitial * reserveBInitial;
			reserveBAfterExecution = constantProduct / (reserveAInitial + amountInWithFee);
			amountOut = reserveBInitial - reserveBAfterExecution;
			marketPrice = (amountInWithFee * 1e18) / amountOut;
			midPrice = (reserveAInitial * 1e18) / reserveBInitial;
			priceImpact = 1e18 - ((midPrice * 1e18) / marketPrice);

			return (
				reserveAInitial,
				reserveBInitial,
				amountInWithFee,
				constantProduct,
				reserveBAfterExecution,
				amountOut,
				marketPrice,
				midPrice,
				priceImpact
			);
		}


    /**
     * @dev Swaps tokens on PancakeSwap with support for slippage tolerance.
     * @param _tokenIn The address of the token to be swapped.
     * @param _tokenOut The address of the token to be received.
     * @param _amountIn The amount of _tokenIn to be swapped.
     * @param _slippage The maximum slippage tolerance as a percentage (e.g., a value of 50 would represent a maximum slippage tolerance of 0.5%).
     * @param _to The recipient of the swapped tokens.
     */
    function swapTokens(
        address _tokenIn,
        address _tokenOut,
        uint256 _amountIn,
        uint256 _slippage,
        address _to
    ) external {
        // Check if the price impact is within the specified tolerance
        if (checkPriceImpact) {
            uint priceImpact = getPriceImpact(_tokenIn, _tokenOut, _amountIn);
            require(
                priceImpact <= priceImpactTolerance * 1e14,
                "Price Impact is too High!"
            );
        }

        IERC20(_tokenIn).transferFrom(msg.sender, address(this), _amountIn);
        IERC20(_tokenIn).approve(pancakeSwapRouterAddress, _amountIn);

        uint256 taxAmount = (_amountIn * devTax) / 100;
        IERC20(_tokenIn).transfer(devWallet, taxAmount);

        uint256 amountAfterTax = _amountIn - taxAmount;

        uint size = 2;
        address[] memory path = new address[](size);
        path[0] = _tokenIn;
        path[1] = _tokenOut;

        // Get an estimate of the amount of _tokenOut that will be received for the specified amount of _tokenIn
        uint256[] memory amounts =
            pancakeSwapRouter.getAmountsOut(amountAfterTax, path);

        // Calculate the minimum amount of _tokenOut to be received based on the specified slippage tolerance
        uint256 amountOutMin =
            (amounts[1] * (10000 - _slippage)) / 10000;

        pancakeSwapRouter.swapExactTokensForTokens(
            amountAfterTax,
            amountOutMin,
            path,
            _to,
            block.timestamp + 1200
        );
    }

    /**
     * @dev Swaps Binance Coin (BNB) for tokens on PancakeSwap with support for slippage tolerance.
     * @param _tokenOut The address of the token to be received.
     * @param _amountIn The amount of BNB to be swapped.
     * @param _slippage The maximum slippage tolerance as a percentage (e.g., a value of 50 would represent a maximum slippage tolerance of 0.5%).
     * @param _to The recipient of the swapped tokens.
     */
    function swapBNBForTokens(
        address _tokenOut,
        uint256 _amountIn,
        uint256 _slippage,
        address _to
    ) external payable {
        require(msg.value == _amountIn, "Incorrect amount of BNB sent");

        // Check if the price impact is within the specified tolerance
        if (checkPriceImpact) {
            uint priceImpact =
                getPriceImpact(pancakeSwapRouter.WETH(), _tokenOut, _amountIn);
            require(
                priceImpact <= priceImpactTolerance * 1e14,
                "Price Impact is too High!"
            );
        }

        uint256 taxAmount = (_amountIn * devTax) / 100;
        payable(devWallet).transfer(taxAmount);

        uint256 amountAfterTax = _amountIn - taxAmount;

        uint size = 2;
        address[] memory path = new address[](size);
        path[0] = pancakeSwapRouter.WETH();
        path[1] = _tokenOut;
        uint256[] memory amounts =
            pancakeSwapRouter.getAmountsOut(amountAfterTax, path);
        uint256 amountOutMin =
            (amounts[1] * (10000 - _slippage)) / 10000;

        pancakeSwapRouter.swapExactETHForTokens{value: amountAfterTax}(
            amountOutMin,
            path,
            _to,
            block.timestamp + 1200 //20mins
        );
    }

    function swapTokensForBNB(
        address _tokenIn,
        uint256 _amountIn,
        uint256 _slippage,
        address _to
    ) external {
        // Check if the price impact is within the specified tolerance
        if (checkPriceImpact) {
            uint priceImpact =
                getPriceImpact(_tokenIn, pancakeSwapRouter.WETH(), _amountIn);
            require(
                priceImpact <= priceImpactTolerance * 1e14,
                "Price Impact is too High!"
            );
        }

        IERC20(_tokenIn).transferFrom(msg.sender, address(this), _amountIn);

        uint256 taxAmount = (_amountIn * devTax) / 100;
        IERC20(_tokenIn).transfer(devWallet, taxAmount);

        uint256 amountAfterTax = _amountIn - taxAmount;

        uint size = 2;
        address[] memory path = new address[](size);
        path[0] = _tokenIn;

        path[1] = pancakeSwapRouter.WETH();

        uint256[] memory amounts =
            pancakeSwapRouter.getAmountsOut(amountAfterTax, path);
        uint256 amountOutMin =
            (amounts[1] * (10000 - _slippage)) / 10000;

        IERC20(_tokenIn).approve(address(pancakeSwapRouter), amountAfterTax);
        pancakeSwapRouter.swapExactTokensForETH(
            amountAfterTax,
            amountOutMin,
            path,
            _to,
            block.timestamp + 1200 //20mins
        );
    }

    /**
     * @dev Wraps Binance Coin (BNB) into Wrapped Binance Coin (WBNB).
     * @param _amount The amount of BNB to be wrapped.
     */
    function wrapBNB(uint256 _amount) external payable {
        require(msg.value == _amount, "Incorrect amount of BNB sent");

        uint256 taxAmount = (_amount * devTax) / 100;
        payable(devWallet).transfer(taxAmount);

        uint256 amountAfterTax = _amount - taxAmount;

        IWETH(pancakeSwapRouter.WETH()).deposit{value: amountAfterTax}();
        IERC20(pancakeSwapRouter.WETH()).transfer(msg.sender, amountAfterTax);
    }
}