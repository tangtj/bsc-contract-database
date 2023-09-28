// File: contracts/UniswapV2Interfaces.sol


pragma solidity ^0.8;

// https://uniswap.org/docs/v2/smart-contracts

interface IUniswapV2Router {
    function getAmountsOut(
        uint amountIn,
        address[] memory path
    ) external view returns (uint[] memory amounts);

    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);

    function swapExactTokensForETH(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);

    function swapExactETHForTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable returns (uint[] memory amounts);

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

    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
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

interface IUniswapV2Factory {
    event PairCreated(
        address indexed token0,
        address indexed token1,
        address pair,
        uint
    );

    function feeTo() external view returns (address);

    function feeToSetter() external view returns (address);

    function getPair(
        address tokenA,
        address tokenB
    ) external view returns (address pair);

    function allPairs(uint) external view returns (address pair);

    function allPairsLength() external view returns (uint);

    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);

    function setFeeTo(address) external;

    function setFeeToSetter(address) external;
}
// File: @openzeppelin/contracts/utils/Context.sol


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

// File: @openzeppelin/contracts/access/Ownable.sol


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

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


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

// File: contracts/FlashSwapper.sol


pragma solidity ^0.8;




interface IUniswapV2Callee {
    function uniswapV2Call(
        address sender,
        uint amount0,
        uint amount1,
        bytes calldata data
    ) external;
}

// SCENARIO AND GOAL EXAMPLE

// PEPE/USDC (0.90$), USDC/WETH (1.00$), PEPE/WETH (1.00$), USDC/USDT (1.00$)
// Borrowing USDC from USDC/USDT pair, Purchasing PEPE for USDC, Selling PEPE for WETH, Selling WETH for USDC and repaying debt

contract FlashSwapper is IUniswapV2Callee, Ownable {
    address public ROUTER;
    address public FACTORY;

    constructor(address _routerAddress, address _factoryAddress) {
        ROUTER = _routerAddress;
        FACTORY = _factoryAddress;
    }

    // Activate ability to receive ETH from uniswapV2Router when swapping
    receive() external payable {}

    function flashSwapArbitrage(
        address _tokenBorrow, // e.g. USDC
        address _tokenBorrowPair, // e.g. USDT
        uint _borrowedAmount, // in wei, e.g. 5000000000000
        address _tokenWithProfit, // e.g. PEPE
        address _tokenWithProfitPair // e.g. WETH
    ) external {
        // check for approvals
        IERC20(_tokenBorrow).approve(address(ROUTER), type(uint256).max);
        IERC20(_tokenWithProfit).approve(address(ROUTER), type(uint256).max);
        IERC20(_tokenBorrowPair).approve(address(ROUTER), type(uint256).max);
        IERC20(_tokenWithProfitPair).approve(
            address(ROUTER),
            type(uint256).max
        );

        // define the pair for borrowing
        address pair = IUniswapV2Factory(FACTORY).getPair(
            _tokenBorrow,
            _tokenBorrowPair
        );
        require(pair != address(0), "!pair");

        address token0 = IUniswapV2Pair(pair).token0();
        address token1 = IUniswapV2Pair(pair).token1();
        uint amount0Out = _tokenBorrow == token0 ? _borrowedAmount : 0;
        uint amount1Out = _tokenBorrow == token1 ? _borrowedAmount : 0;

        // define the data to pass to the uniswapV2Call
        bytes memory data = abi.encode(
            _tokenBorrow,
            _borrowedAmount,
            _tokenWithProfit,
            _tokenWithProfitPair
        );

        IUniswapV2Pair(pair).swap(amount0Out, amount1Out, address(this), data);
    }

    // called by pair contract
    function uniswapV2Call(
        address _sender,
        uint _amount0,
        uint _amount1,
        bytes calldata _data
    ) external override {
        address token0 = IUniswapV2Pair(msg.sender).token0();
        address token1 = IUniswapV2Pair(msg.sender).token1();
        address pair = IUniswapV2Factory(FACTORY).getPair(token0, token1);
        require(msg.sender == pair, "!pair");
        require(_sender == address(this), "!sender");

        (
            address tokenBorrow,
            uint borrowedAmount,
            address tokenWithProfit,
            address tokenWithProfitPair
        ) = abi.decode(_data, (address, uint, address, address));

        // NOOP to silence compiler "unused parameter" warning
        if (false) {
            _amount0;
            _amount1;
        }

        // about 0.3%
        uint fee = ((borrowedAmount * 3) / 997) + 1;
        uint amountToRepay = borrowedAmount + fee;

        // DO WHAT YOU WANT WITH BORROWED

        // swap borrowed for profit (e.g. USDC for PEPE)
        address[] memory profitPath = new address[](2);
        profitPath[0] = address(tokenBorrow);
        profitPath[1] = address(tokenWithProfit);

        IUniswapV2Router(ROUTER).swapExactTokensForTokens(
            borrowedAmount,
            0,
            profitPath,
            address(this),
            block.timestamp + 600
        );

        // sell profit in arbitrage pair (e.g. PEPE for WETH)
        uint pepeBalance = IERC20(tokenWithProfit).balanceOf(address(this));

        address[] memory profitToEthPath = new address[](2);
        profitToEthPath[0] = address(tokenWithProfit);
        profitToEthPath[1] = address(tokenWithProfitPair);

        IUniswapV2Router(ROUTER).swapExactTokensForTokens(
            pepeBalance,
            0,
            profitToEthPath,
            address(this),
            block.timestamp + 600
        );

        // swap arbitraged profit to borrowed (e.g. WETH for USDC)
        uint wethBalance = IERC20(tokenWithProfitPair).balanceOf(address(this));

        address[] memory wethToBorrowedPath = new address[](2);
        wethToBorrowedPath[0] = address(tokenWithProfitPair);
        wethToBorrowedPath[1] = address(tokenBorrow);

        IUniswapV2Router(ROUTER).swapExactTokensForTokens(
            wethBalance,
            0,
            wethToBorrowedPath,
            address(this),
            block.timestamp + 600
        );

        // END WHAT YOU WANT WITH BORROWED AND RETURN THE DEBT

        IERC20(tokenBorrow).transfer(pair, amountToRepay);

        // the profit will be left on this contract address
    }

    // Withdraw contract ether balance
    function withdrawEther(address to) public onlyOwner {
        payable(to).transfer(address(this).balance);
    }

    // Withdraw contract token balance
    function withdrawToken(address tokenAddress, address to) public onlyOwner {
        uint currentTokenBalance = IERC20(tokenAddress).balanceOf(
            address(this)
        );
        require(currentTokenBalance > 0, "Tokens amount must be > 0");
        IERC20(tokenAddress).transfer(to, currentTokenBalance);
    }
}