/**
 *Submitted for verification at BscScan.com on 2023-06-09
*/

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
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

contract Presale {
   uint256 public prices = 40000000000000000;
   uint256 public sellprices = 25000000000000000000;

    uint256 public stage = 0;
    uint256 public tokenSold = 0;
    IERC20 public token = IERC20(0x36EE1459bFfEe035D8e98bBE982c0322607F8675);
    IERC20 public usdt = IERC20(0x55d398326f99059fF775485246999027B3197955);
    address public owner = 0x2cB31202c014bFB379E87754f631AD254c3e4A91;
    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function.");
        _;
    }
    constructor() {
    }

  function buyTokens(uint256 usdtAmount) public {
    require(usdt.balanceOf(msg.sender) >= usdtAmount, "Not enough USDT");
    
    uint256 tokensToBuy = (usdtAmount * prices) / 1000000000000000000; 
   require(token.balanceOf(address(this)) >= tokensToBuy / 10000000000, "Not enough tokens left");
   require(usdt.transferFrom(msg.sender,address(this),usdtAmount), "No approval");
    
   require(token.transfer(msg.sender, tokensToBuy / 10000000000), "No transfer");
    tokenSold += tokensToBuy;
    

   
}   
 function SellToken(uint256 tokenAmount) public {
    require(token.balanceOf(msg.sender) >= tokenAmount / 10000000000, "Not enough Token In your Wallet");
    
    uint256 tokensToBuy = (tokenAmount * sellprices) / 1000000000000000000; 
   require(usdt.balanceOf(address(this)) >= tokensToBuy, "Not enough USDT in contract left");
   require(token.transferFrom(msg.sender,address(this),tokenAmount / 10000000000), "No approval");
    
   require(usdt.transfer(msg.sender, tokensToBuy), "No transfer");
    tokenSold += tokensToBuy;
    
}   
  
     function withdrawStablecoins() external onlyOwner {
        uint256 balance = usdt.balanceOf(address(this));
        require(balance > 0, "Presale: No stablecoins to withdraw");

        usdt.transfer(owner, balance);
    }

    function withdrawTokens() external onlyOwner {
        uint256 balance = token.balanceOf(address(this));
        require(balance > 0, "Presale: No tokens to withdraw");

        token.transfer(owner, balance);
    }
}