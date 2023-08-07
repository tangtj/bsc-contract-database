// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.6.0) (token/ERC20/IERC20.sol)

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
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

// File: contracts/118_TTDAO_RemvoeLp.sol


pragma solidity ^0.8.9;


interface IUniswapV2Router01 {
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

contract TTDAORemoveLiquidity {
    address private ttdao = 0xdD7d5DD1C91abE4376065266a1956e47c7567D5b;
    address private usdt = 0x55d398326f99059fF775485246999027B3197955;
    address private lp = 0x0A789170Aa1679c230DAF0435E0f8A314BAaBB97;
    address private pancakeRouter = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    uint256 public  constant MAX = ~uint256(0);

    constructor() {
         IERC20(lp).approve(pancakeRouter, MAX);
    }

    function removeLiquidity(uint256 amount) public {
        uint256 balance = balanceOf(msg.sender);
        require(balance >= amount, "token limit");
        IERC20(lp).transferFrom(msg.sender, address(this), amount);

        uint256 usdBefore = IERC20(usdt).balanceOf(address(this));
        uint256 ttdaoBefore = IERC20(ttdao).balanceOf(address(this));

        IUniswapV2Router01(pancakeRouter).removeLiquidity(ttdao, usdt, amount, 0, 0, address(this), block.timestamp + 60);

        uint256 usdtAmount = IERC20(usdt).balanceOf(address(this)) - usdBefore;
        uint256 ttdaoAmount = IERC20(ttdao).balanceOf(address(this)) - ttdaoBefore;

        if (usdtAmount > 0) {
            IERC20(usdt).transfer(msg.sender, usdtAmount);
        }

        if (ttdaoAmount > 0) {
             IERC20(ttdao).transfer(msg.sender, ttdaoAmount);
        }
    }

    function balanceOf(address account) public view returns (uint256) {
        return IERC20(lp).balanceOf(account);
    }
}