// File: @openzeppelin/contracts/token/ERC20/IERC20.sol

// SPDX-License-Identifier: GPL-v3-only
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

// File: contracts/GamesPool.sol
contract GamesPool {

    address public oracle;
    address public usdtContract;
    address public owner;

    constructor(address _oracle, address _usdtContract, address _owner) {
        oracle = _oracle;
        usdtContract = _usdtContract;
        owner = _owner;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "OWNER-ONLY");
        _;
    }

    function setOracle(address _oracle) external onlyOwner {
        oracle = _oracle;
    }
    function setUsdtContract(address _usdtContract) external onlyOwner {
        usdtContract = _usdtContract;
    }
    function transferOwnership(address _owner) external onlyOwner {
        owner = _owner;
    }

    function _balance() internal view returns (uint usdtBalance) {
        usdtBalance = IERC20(usdtContract).balanceOf(address(this));
    }

    function balance() public view returns (uint usdtBalance) {
        usdtBalance = _balance();
    }

    /** [oracle-only]
    * @notice Allow the oracle to distribute USDT winnings from the games pool to a game winner
    * @return distributed {bool} truthy when gains have been distributed as expected
    */
    function distributeUsdtWinnings(address winner, uint gainsAmount) external returns (bool distributed) {
        require(msg.sender == oracle, "ORACLE-ONLY");
        require(gainsAmount <= _balance(), "POOL-OVERFLOW");

        require(IERC20(usdtContract).transfer(winner, gainsAmount), 'GAINS-TRF-FAIL');

        distributed = true;
    }

}