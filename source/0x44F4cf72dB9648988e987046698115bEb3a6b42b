// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

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

library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryAdd(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the substraction of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function trySub(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryMul(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
            // benefit is lost if 'b' is also tested.
            // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the division of two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryDiv(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryMod(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     *
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     *
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator.
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {trySub}.
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting with custom message on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting with custom message when dividing by zero.
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {tryMod}.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}

contract StakingPool {
    using SafeMath for uint256;
    struct Stake {
        uint256 amount;
        uint256 startTime;
        uint256 endTime;
        uint256 lastClaimTime;
        uint256 period;
    }
    
    struct StakePeriod {
        uint256 periodDays;
        uint256 apyRate;
        uint256 periodMonth;
    }

    mapping(address => Stake[]) public stakes;
    address[] public unbearableAddresses;
    uint256 public totalSupply;
    address public owner;
    bool public pausableStaking = false;
    IERC20 public Token;
    StakePeriod[] public stakePeriods;
    uint256 public constant DECIMALS = 18;
        
    event Transfer(address indexed from, address indexed to, uint256 value);

    constructor(address _stakingToken) {
        owner = msg.sender;
        Token = IERC20(_stakingToken);
        
        stakePeriods.push(StakePeriod(30, 9, 1));
        stakePeriods.push(StakePeriod(90, 18, 3));
        stakePeriods.push(StakePeriod(180, 37, 6));
        stakePeriods.push(StakePeriod(365, 75, 12));
    }

    modifier onlyOwner {
        require(msg.sender == owner, "Caller is not the owner");
        _;
    }

    function trasfeOwnership(address _newOwner) public onlyOwner{
        owner = _newOwner;
    }

    function updateStakePeriod(uint256 _index, uint256 _periodDays, uint256 _apyRate, uint256 _periodMonth) external onlyOwner {
        require(_index < stakePeriods.length, "Invalid index");

        StakePeriod storage periodToUpdate = stakePeriods[_index];

        periodToUpdate.periodDays = _periodDays;
        periodToUpdate.apyRate = _apyRate;
        periodToUpdate.periodMonth = _periodMonth;
    }

    function addUnbearableAddress(address _address) external onlyOwner {
        unbearableAddresses.push(_address);
    }

    function pausStaking() external onlyOwner {
        pausableStaking = true;
    }

    function unPausStaking() external onlyOwner {
        pausableStaking = false;
    }

    function removeUnbearableAddress(address _address) external onlyOwner {
        for (uint256 i = 0; i < unbearableAddresses.length; i++) {
            if (unbearableAddresses[i] == _address) {
                unbearableAddresses[i] = unbearableAddresses[unbearableAddresses.length - 1];
                unbearableAddresses.pop();
                break;
            }
        }
    }

    function setStakingToken(address _stakingToken) external onlyOwner {
        Token = IERC20(_stakingToken);
    }

    function stake(uint256 _amount, uint256 _stakingPeriodIndex) external {
        require(_amount > 0, "Staking amount must be greater than 0");
        require(_stakingPeriodIndex < stakePeriods.length, "Invalid staking period index");
        require(!pausableStaking, "Pausable Staking");

        StakePeriod storage selectedPeriod = stakePeriods[_stakingPeriodIndex];

        require(Token.transferFrom(msg.sender, address(this), _amount), "Transfer error!");

        totalSupply += _amount;

        stakes[msg.sender].push(Stake({
            amount: _amount,
            startTime: block.timestamp,
            endTime: block.timestamp + (selectedPeriod.periodDays * 1 days),
            lastClaimTime: block.timestamp,
            period: _stakingPeriodIndex
        }));
    }

   function unstake(uint256 _index) external {
       Stake storage userStake = stakes[msg.sender][_index];
       require(userStake.endTime < block.timestamp, "Staking period not yet ended");
       bool allowed = true;
        for (uint256 i = 0; i < unbearableAddresses.length; i++) {
            if (unbearableAddresses[i] == msg.sender) {
                allowed = false;
                break;
            }
        }
        require(allowed, "Not allowed to unstake");

       uint256 _amount = userStake.amount;
       uint256 reward = calculateReward(_amount, userStake.period);
       userStake.lastClaimTime = block.timestamp;

        require(Token.approve(msg.sender, _amount), "Approval failed");
        Token.transfer(msg.sender, _amount + reward);

       totalSupply -= _amount;
       userStake.amount = 0;

       if (userStake.amount == 0) {
           delete stakes[msg.sender][_index];
       }
   }

    function calculateReward(uint256 _amount, uint256 _stakingPeriod) public view returns (uint256) {
        StakePeriod storage selectedPeriod = stakePeriods[_stakingPeriod];
        
        uint256 reward = ((_amount * selectedPeriod.apyRate * selectedPeriod.periodMonth) * (10**DECIMALS)) / (12 * 100);
        
        return reward / (10**DECIMALS);
    }

    function approveToken(uint256 _amount) external onlyOwner {
        Token.approve(owner, _amount);
    }

    function withdrawTokens(uint256 _amount) external onlyOwner {
        Token.transfer(owner, _amount);
    }

    function getAllStakes(address _user) public view returns (Stake[] memory, uint256) {
        return (stakes[_user], stakes[_user].length);
    }

    function getStakePeriods() external view returns (StakePeriod[] memory) {
        return stakePeriods;
    }
}