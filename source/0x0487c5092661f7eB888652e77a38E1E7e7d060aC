// SPDX-License-Identifier: MIT
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

contract SimplePresale {
    IERC20 public btsToken;
    IERC20 public usdt;
    IERC20 public usdc;
    IERC20 public busd;
    address public owner;
    uint256 public rate = 20000; 

    constructor(address _btsToken, address _usdt, address _usdc, address _busd) {
        btsToken = IERC20(_btsToken);
        usdt = IERC20(_usdt);
        usdc = IERC20(_usdc);
        busd = IERC20(_busd);
        owner = msg.sender;
    }

    function buyTokensWithUSDT(uint256 usdtAmount) public {
        buyTokens(usdt, usdtAmount);
    }

    function buyTokensWithUSDC(uint256 usdcAmount) public {
        buyTokens(usdc, usdcAmount);
    }

    function buyTokensWithBUSD(uint256 busdAmount) public {
        buyTokens(busd, busdAmount);
    }

    function buyTokens(IERC20 stableCoin, uint256 amount) private {
        uint256 tokensToBuy = amount * rate;

        require(stableCoin.transferFrom(msg.sender, address(this), amount), "Stablecoin transfer failed");
        require(btsToken.transfer(msg.sender, tokensToBuy), "BTS transfer failed");
    }

    function recoverRemainingBtsTokens() public {
        recoverTokens(btsToken);
    }

    function recoverRemainingUsdtTokens() public {
        recoverTokens(usdt);
    }

    function recoverRemainingUsdcTokens() public {
        recoverTokens(usdc);
    }

    function recoverRemainingBusdTokens() public {
        recoverTokens(busd);
    }

    function recoverTokens(IERC20 token) private {
        require(msg.sender == owner, "Only the owner can recover remaining tokens");

        uint256 remainingTokens = token.balanceOf(address(this));
        require(remainingTokens > 0, "No tokens to recover");

        require(token.transfer(owner, remainingTokens), "Transfer failed");
    }
}