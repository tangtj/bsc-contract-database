// SPDX-License-Identifier: MIT

pragma solidity 0.6.12;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount) external returns (bool);

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
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

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
}


interface TwoWeeksNotice {
    
    function estimateAccumulated(address account) external view returns (uint128, uint128);
    
}

contract Rewards {
    
    mapping (address => uint128) public processed;
    uint64 public rewardPerDayPerToken;
    TwoWeeksNotice public twn;
    IERC20 public token;
    address public owner;
    
    constructor (IERC20 _token, TwoWeeksNotice _twn, address _owner, uint64 inital_reward) public {
        token = _token;
        twn = _twn;
        rewardPerDayPerToken = inital_reward;
        owner = _owner;
    }
    
    function estimateReward(address account) external view returns (uint256 reward) {
        uint128 prevPaid = processed[account];
        (uint128 acc, uint128 accStrict) = twn.estimateAccumulated(account);
        uint128 delta = acc - prevPaid;
        reward = (rewardPerDayPerToken * delta) / 1000000;
    }
    
    function setRewardRate(uint64 rewardRate) external {
        require(msg.sender == owner);
        
        rewardPerDayPerToken = rewardRate;
    }
    
    function payReward(address to) public {
        uint128 prevPaid = processed[to];
        (uint128 acc, uint128 accStrict) = twn.estimateAccumulated(to);
        if (acc > prevPaid) {
            uint128 delta = acc - prevPaid;
            uint128 reward = (rewardPerDayPerToken * delta) / 1000000;
            if (reward > 0) {
                processed[to] = acc;
                token.transfer(to, reward);
            }
        }
    }
    
    function distribute() external {
        payReward(msg.sender);
    }
    
    function returnToTresury () external {
        require(msg.sender == owner);
        token.transfer(owner, token.balanceOf(address(this)));
    }
    

}