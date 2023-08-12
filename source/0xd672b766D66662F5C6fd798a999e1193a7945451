// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

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

interface ISasWecoin {
    /*-----------------------------------------------------------\
    |                      Type Definitions                      |
    \-----------------------------------------------------------*/
    struct UserInfo {
        uint depositAmount;
        uint bonusAmount;
        uint offsetPoints; // DEBT to update based on current accumulator
        uint lastAction;
        uint lockDuration; // amount of weeks
        uint endLockEpoch; // Actual END LOCK EPOCH
        uint lockedRewards;
    }
    struct EpochInfo {
        uint epochTotalBaseReward;
        // final epoch accumulation is the accumulatedRewardsStakingPower
        uint finalEpochAccumulation;
        // This adjustment only is added/substracted to the totalBonusStakingPower when the epoch starts.
        uint totalBonusStakingPowerAdjustment;
        bool adjusted;
    }

    /**
     * Returns the staking power of the user, relative to the user's deposit amount and time staked.
     * @param _user The address of the user
     * @return The current staking power of the user
     */
    function getStakingPower(address _user) external view returns (uint256);

    /**
     * Add rewards to the total reward pool.
     * @param _amount The amount of WECOIN to deposit
     */
    function addTokenRewards(uint256 _amount) external;

    /**
     * Deposit WECOIN tokens to create staking power and determine the user's share of the reward pool.
     * @param _amount Amount of WECOIN to deposit
     * @param _weeksLocked Time to LOCK the WECOIN tokens for
     * @dev _weeksLocked must be saved in WEEKS, not seconds
     * @dev penalty pool must be distributed amongst users that are currently staked with a duration
     */
    function deposit(uint256 _amount, uint256 _weeksLocked) external;

    /**
     * Update the reward tracker to the current epoch and timestamp.
     */
    function updateAccumulator() external;

    /**
     * Calculate the multiplier a user would get for a certain amount of epochs.
     * @param _lockedEpochs The amount of epochs to lock tokens for
     * @return The multiplier with 4 decimal places.
     * @dev Minimum is always 1x => 10000
     */
    function calculateMultiplier(
        uint _lockedEpochs
    ) external pure returns (uint);

    /**
     * Returns the total staking power of the contract.
     * @return The total staking power of the contract.
     */
    function getCurrentTotalStakingPower() external view returns (uint);

    //---------------------------------------------------
    //                  EVENTS
    //---------------------------------------------------
    event LockReward(address indexed user, uint256 totalAmountLocked);

    event Deposit(
        address indexed user,
        uint256 amount,
        uint256 bonus,
        uint256 totalLockPeriod
    );

    event Withdraw(address indexed user, uint256 amount);

    event ClaimReward(address indexed user, uint256 amount);
    event WithdrawPenalty(
        address indexed user,
        uint256 principalPenalty,
        uint256 rewardPenalty
    );
}

// OpenZeppelin Contracts (last updated v4.9.0) (utils/math/Math.sol)

/**
 * @dev Standard math utilities missing in the Solidity language.
 */
library Math {
    enum Rounding {
        Down, // Toward negative infinity
        Up, // Toward infinity
        Zero // Toward zero
    }

    /**
     * @dev Returns the largest of two numbers.
     */
    function max(uint256 a, uint256 b) internal pure returns (uint256) {
        return a > b ? a : b;
    }

    /**
     * @dev Returns the smallest of two numbers.
     */
    function min(uint256 a, uint256 b) internal pure returns (uint256) {
        return a < b ? a : b;
    }

    /**
     * @dev Returns the average of two numbers. The result is rounded towards
     * zero.
     */
    function average(uint256 a, uint256 b) internal pure returns (uint256) {
        // (a + b) / 2 can overflow.
        return (a & b) + (a ^ b) / 2;
    }

    /**
     * @dev Returns the ceiling of the division of two numbers.
     *
     * This differs from standard division with `/` in that it rounds up instead
     * of rounding down.
     */
    function ceilDiv(uint256 a, uint256 b) internal pure returns (uint256) {
        // (a + b - 1) / b can overflow on addition, so we distribute.
        return a == 0 ? 0 : (a - 1) / b + 1;
    }

    /**
     * @notice Calculates floor(x * y / denominator) with full precision. Throws if result overflows a uint256 or denominator == 0
     * @dev Original credit to Remco Bloemen under MIT license (https://xn--2-umb.com/21/muldiv)
     * with further edits by Uniswap Labs also under MIT license.
     */
    function mulDiv(uint256 x, uint256 y, uint256 denominator) internal pure returns (uint256 result) {
        unchecked {
            // 512-bit multiply [prod1 prod0] = x * y. Compute the product mod 2^256 and mod 2^256 - 1, then use
            // use the Chinese Remainder Theorem to reconstruct the 512 bit result. The result is stored in two 256
            // variables such that product = prod1 * 2^256 + prod0.
            uint256 prod0; // Least significant 256 bits of the product
            uint256 prod1; // Most significant 256 bits of the product
            assembly {
                let mm := mulmod(x, y, not(0))
                prod0 := mul(x, y)
                prod1 := sub(sub(mm, prod0), lt(mm, prod0))
            }

            // Handle non-overflow cases, 256 by 256 division.
            if (prod1 == 0) {
                // Solidity will revert if denominator == 0, unlike the div opcode on its own.
                // The surrounding unchecked block does not change this fact.
                // See https://docs.soliditylang.org/en/latest/control-structures.html#checked-or-unchecked-arithmetic.
                return prod0 / denominator;
            }

            // Make sure the result is less than 2^256. Also prevents denominator == 0.
            require(denominator > prod1, "Math: mulDiv overflow");

            ///////////////////////////////////////////////
            // 512 by 256 division.
            ///////////////////////////////////////////////

            // Make division exact by subtracting the remainder from [prod1 prod0].
            uint256 remainder;
            assembly {
                // Compute remainder using mulmod.
                remainder := mulmod(x, y, denominator)

                // Subtract 256 bit number from 512 bit number.
                prod1 := sub(prod1, gt(remainder, prod0))
                prod0 := sub(prod0, remainder)
            }

            // Factor powers of two out of denominator and compute largest power of two divisor of denominator. Always >= 1.
            // See https://cs.stackexchange.com/q/138556/92363.

            // Does not overflow because the denominator cannot be zero at this stage in the function.
            uint256 twos = denominator & (~denominator + 1);
            assembly {
                // Divide denominator by twos.
                denominator := div(denominator, twos)

                // Divide [prod1 prod0] by twos.
                prod0 := div(prod0, twos)

                // Flip twos such that it is 2^256 / twos. If twos is zero, then it becomes one.
                twos := add(div(sub(0, twos), twos), 1)
            }

            // Shift in bits from prod1 into prod0.
            prod0 |= prod1 * twos;

            // Invert denominator mod 2^256. Now that denominator is an odd number, it has an inverse modulo 2^256 such
            // that denominator * inv = 1 mod 2^256. Compute the inverse by starting with a seed that is correct for
            // four bits. That is, denominator * inv = 1 mod 2^4.
            uint256 inverse = (3 * denominator) ^ 2;

            // Use the Newton-Raphson iteration to improve the precision. Thanks to Hensel's lifting lemma, this also works
            // in modular arithmetic, doubling the correct bits in each step.
            inverse *= 2 - denominator * inverse; // inverse mod 2^8
            inverse *= 2 - denominator * inverse; // inverse mod 2^16
            inverse *= 2 - denominator * inverse; // inverse mod 2^32
            inverse *= 2 - denominator * inverse; // inverse mod 2^64
            inverse *= 2 - denominator * inverse; // inverse mod 2^128
            inverse *= 2 - denominator * inverse; // inverse mod 2^256

            // Because the division is now exact we can divide by multiplying with the modular inverse of denominator.
            // This will give us the correct result modulo 2^256. Since the preconditions guarantee that the outcome is
            // less than 2^256, this is the final result. We don't need to compute the high bits of the result and prod1
            // is no longer required.
            result = prod0 * inverse;
            return result;
        }
    }

    /**
     * @notice Calculates x * y / denominator with full precision, following the selected rounding direction.
     */
    function mulDiv(uint256 x, uint256 y, uint256 denominator, Rounding rounding) internal pure returns (uint256) {
        uint256 result = mulDiv(x, y, denominator);
        if (rounding == Rounding.Up && mulmod(x, y, denominator) > 0) {
            result += 1;
        }
        return result;
    }

    /**
     * @dev Returns the square root of a number. If the number is not a perfect square, the value is rounded down.
     *
     * Inspired by Henry S. Warren, Jr.'s "Hacker's Delight" (Chapter 11).
     */
    function sqrt(uint256 a) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        // For our first guess, we get the biggest power of 2 which is smaller than the square root of the target.
        //
        // We know that the "msb" (most significant bit) of our target number `a` is a power of 2 such that we have
        // `msb(a) <= a < 2*msb(a)`. This value can be written `msb(a)=2**k` with `k=log2(a)`.
        //
        // This can be rewritten `2**log2(a) <= a < 2**(log2(a) + 1)`
        // → `sqrt(2**k) <= sqrt(a) < sqrt(2**(k+1))`
        // → `2**(k/2) <= sqrt(a) < 2**((k+1)/2) <= 2**(k/2 + 1)`
        //
        // Consequently, `2**(log2(a) / 2)` is a good first approximation of `sqrt(a)` with at least 1 correct bit.
        uint256 result = 1 << (log2(a) >> 1);

        // At this point `result` is an estimation with one bit of precision. We know the true value is a uint128,
        // since it is the square root of a uint256. Newton's method converges quadratically (precision doubles at
        // every iteration). We thus need at most 7 iteration to turn our partial result with one bit of precision
        // into the expected uint128 result.
        unchecked {
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            return min(result, a / result);
        }
    }

    /**
     * @notice Calculates sqrt(a), following the selected rounding direction.
     */
    function sqrt(uint256 a, Rounding rounding) internal pure returns (uint256) {
        unchecked {
            uint256 result = sqrt(a);
            return result + (rounding == Rounding.Up && result * result < a ? 1 : 0);
        }
    }

    /**
     * @dev Return the log in base 2, rounded down, of a positive value.
     * Returns 0 if given 0.
     */
    function log2(uint256 value) internal pure returns (uint256) {
        uint256 result = 0;
        unchecked {
            if (value >> 128 > 0) {
                value >>= 128;
                result += 128;
            }
            if (value >> 64 > 0) {
                value >>= 64;
                result += 64;
            }
            if (value >> 32 > 0) {
                value >>= 32;
                result += 32;
            }
            if (value >> 16 > 0) {
                value >>= 16;
                result += 16;
            }
            if (value >> 8 > 0) {
                value >>= 8;
                result += 8;
            }
            if (value >> 4 > 0) {
                value >>= 4;
                result += 4;
            }
            if (value >> 2 > 0) {
                value >>= 2;
                result += 2;
            }
            if (value >> 1 > 0) {
                result += 1;
            }
        }
        return result;
    }

    /**
     * @dev Return the log in base 2, following the selected rounding direction, of a positive value.
     * Returns 0 if given 0.
     */
    function log2(uint256 value, Rounding rounding) internal pure returns (uint256) {
        unchecked {
            uint256 result = log2(value);
            return result + (rounding == Rounding.Up && 1 << result < value ? 1 : 0);
        }
    }

    /**
     * @dev Return the log in base 10, rounded down, of a positive value.
     * Returns 0 if given 0.
     */
    function log10(uint256 value) internal pure returns (uint256) {
        uint256 result = 0;
        unchecked {
            if (value >= 10 ** 64) {
                value /= 10 ** 64;
                result += 64;
            }
            if (value >= 10 ** 32) {
                value /= 10 ** 32;
                result += 32;
            }
            if (value >= 10 ** 16) {
                value /= 10 ** 16;
                result += 16;
            }
            if (value >= 10 ** 8) {
                value /= 10 ** 8;
                result += 8;
            }
            if (value >= 10 ** 4) {
                value /= 10 ** 4;
                result += 4;
            }
            if (value >= 10 ** 2) {
                value /= 10 ** 2;
                result += 2;
            }
            if (value >= 10 ** 1) {
                result += 1;
            }
        }
        return result;
    }

    /**
     * @dev Return the log in base 10, following the selected rounding direction, of a positive value.
     * Returns 0 if given 0.
     */
    function log10(uint256 value, Rounding rounding) internal pure returns (uint256) {
        unchecked {
            uint256 result = log10(value);
            return result + (rounding == Rounding.Up && 10 ** result < value ? 1 : 0);
        }
    }

    /**
     * @dev Return the log in base 256, rounded down, of a positive value.
     * Returns 0 if given 0.
     *
     * Adding one to the result gives the number of pairs of hex symbols needed to represent `value` as a hex string.
     */
    function log256(uint256 value) internal pure returns (uint256) {
        uint256 result = 0;
        unchecked {
            if (value >> 128 > 0) {
                value >>= 128;
                result += 16;
            }
            if (value >> 64 > 0) {
                value >>= 64;
                result += 8;
            }
            if (value >> 32 > 0) {
                value >>= 32;
                result += 4;
            }
            if (value >> 16 > 0) {
                value >>= 16;
                result += 2;
            }
            if (value >> 8 > 0) {
                result += 1;
            }
        }
        return result;
    }

    /**
     * @dev Return the log in base 256, following the selected rounding direction, of a positive value.
     * Returns 0 if given 0.
     */
    function log256(uint256 value, Rounding rounding) internal pure returns (uint256) {
        unchecked {
            uint256 result = log256(value);
            return result + (rounding == Rounding.Up && 1 << (result << 3) < value ? 1 : 0);
        }
    }
}

error SasWecoin__NoBalanceToWithdraw();
error SasWecoin__NothingToClaim();
error SasWecoin__InvalidDepositAmount();
error SasWecoin__RewardsAlreadyAdded();

contract WECOStaking is ISasWecoin {
    mapping(address => UserInfo) public users;
    mapping(uint => EpochInfo) public epochs;

    IERC20 public WECOIN;

    uint public totalRewards;
    uint public immutable stakingStartTime;
    uint public prevTimestamp;
    uint public totalBaseStakingPower;
    uint public totalBonusStakingPower;
    uint public accumulatedRewardsPerStakingPower;
    uint public rewardPenalty = 5;
    uint public nextEpochRewardAddition;
    uint private lastAccumulatedEpoch;
    uint private lastTotalReward;
    // This magnifier keeps all decimals when doing integer division
    // Not concerned about overflow since it is only used internally and
    // WECOIN has 18 decimals + a max supply of 10B
    // 29 decimal places total
    // 29 + 20 = 49 decimal places total
    // considering uint256 has a max of 77 decimal places
    // THE MAX VALUE  where MAGNIFIER is used is
    // (rewardOffset * WEEKLY_EMISSIONS * diff * MAGNIFIER)
    //     29               2               10      20    <= DECIMALS USED
    // MAX is 61 DECIMAL PLACES
    uint private constant MAGNIFIER = 1e20;
    uint private constant EPOCH_DURATION = 1 weeks;
    uint public constant SQRT_ADJUSTMENT = 100_00;
    uint private constant DAILY_EMISSIONS = 4;
    uint private constant WEEKLY_EMISSIONS = 28;
    uint private constant EMISSIONS_BASE = 100_00;
    uint private constant INT_PERCENTAGE_BASE = 100;

    //---------------------------------------------------
    //                  CONSTRUCTOR
    //---------------------------------------------------
    constructor(address _wecoin, uint _stakingStartTime) {
        WECOIN = IERC20(_wecoin);
        stakingStartTime = _stakingStartTime;
        prevTimestamp = _stakingStartTime;
    }

    function deposit(uint _amount, uint256 _weeksLocked) external {
        if (_amount == 0) {
            revert SasWecoin__InvalidDepositAmount();
        }
        UserInfo storage user = users[msg.sender];
        // Lock or claim rewards
        // NOTE here is where offsetpoints are updated 1 time
        _claimAndLock(msg.sender);
        // get current epoch
        uint currentEpoch = _currentEpoch();
        // Deposit amount
        uint fullAmount = user.depositAmount + _amount;
        uint bonusStakingPower = user.bonusAmount;
        totalBonusStakingPower -= bonusStakingPower;
        totalBaseStakingPower += _amount;
        if (block.timestamp > stakingStartTime)
            user.lastAction = block.timestamp;
        else user.lastAction = stakingStartTime;
        user.depositAmount = fullAmount;
        // need to check if there is a previous locking period and extend it.
        if (
            _weeksLocked > 0 ||
            (currentEpoch > 0 && user.endLockEpoch >= currentEpoch)
        ) {
            if (user.endLockEpoch >= currentEpoch) {
                epochs[user.endLockEpoch + 1]
                    .totalBonusStakingPowerAdjustment -= user.bonusAmount;
                user.endLockEpoch += _weeksLocked;
                user.lockDuration = user.endLockEpoch - currentEpoch;
            } else {
                user.endLockEpoch = _weeksLocked + currentEpoch;
                user.lockDuration = _weeksLocked;
            }

            uint multiplier = calculateMultiplier(user.lockDuration);
            bonusStakingPower = (fullAmount * multiplier) / SQRT_ADJUSTMENT;
            bonusStakingPower -= fullAmount;
            // Set the adjustment needed when the end epoch arrives
            epochs[user.endLockEpoch + 1]
                .totalBonusStakingPowerAdjustment += bonusStakingPower;

            user.bonusAmount = bonusStakingPower;
            totalBonusStakingPower += bonusStakingPower;
        } else {
            user.bonusAmount = 0;
        }

        // adjust offsetPoints here as well
        user.offsetPoints =
            ((fullAmount + bonusStakingPower) *
                accumulatedRewardsPerStakingPower) /
            MAGNIFIER;
        // Transfer in WECOIN
        WECOIN.transferFrom(msg.sender, address(this), _amount);
    }

    function claimOrLock() external {
        UserInfo storage user = users[msg.sender];
        if (user.depositAmount == 0) revert SasWecoin__NothingToClaim();
        _claimAndLock(msg.sender);
    }

    function updateAccumulator() external {
        _updateAccumulator();
    }

    function addTokenRewards(uint amount) external {
        WECOIN.transferFrom(msg.sender, address(this), amount);
        totalRewards += amount;
        uint currentEpoch = _currentEpoch();
        if (block.timestamp < stakingStartTime) {
            epochs[currentEpoch].epochTotalBaseReward += amount;
            lastTotalReward += amount;
        } else nextEpochRewardAddition += amount;
    }

    function withdraw() external {
        // Update all values
        _claimAndLock(msg.sender);

        UserInfo storage user = users[msg.sender];
        if (user.depositAmount == 0) {
            revert SasWecoin__NoBalanceToWithdraw();
        }
        uint currentEpoch = _currentEpoch();
        uint withdrawableAmount = user.depositAmount;
        uint penalty;
        if (user.endLockEpoch > 0 && currentEpoch <= user.endLockEpoch) {
            // Calculate and apply penalties
            uint epochDiff = (user.endLockEpoch + 1) *
                EPOCH_DURATION +
                stakingStartTime -
                block.timestamp;
            uint full = user.lockDuration * EPOCH_DURATION;
            uint threshold = full / 2;
            // Penalty of 50% threshold
            if (epochDiff > threshold) {
                // Principal penalty
                penalty =
                    (withdrawableAmount * rewardPenalty) /
                    INT_PERCENTAGE_BASE;
                withdrawableAmount -= penalty;

                emit WithdrawPenalty(msg.sender, penalty, user.lockedRewards);
                // Reward penalty fully penalized
                penalty += user.lockedRewards;
            } else {
                // MATH penalty
                penalty = (180 * full) - ((full - epochDiff) * 160);
                penalty = (user.lockedRewards * penalty) / (full * 100);
                withdrawableAmount += user.lockedRewards - penalty;
                emit WithdrawPenalty(msg.sender, 0, penalty);
            }
            totalRewards += penalty;
            nextEpochRewardAddition += penalty;
            // Adjust the next epochs adjustment since this is an early withdrawal
            epochs[user.endLockEpoch + 1]
                .totalBonusStakingPowerAdjustment -= user.bonusAmount;
            totalBonusStakingPower -= user.bonusAmount;
        }

        totalBaseStakingPower -= user.depositAmount;
        // Reset all values
        users[msg.sender] = UserInfo({
            depositAmount: 0,
            bonusAmount: 0,
            offsetPoints: 0,
            lastAction: block.timestamp,
            endLockEpoch: 0,
            lockDuration: 0,
            lockedRewards: 0
        });
        emit Withdraw(msg.sender, withdrawableAmount);
        // Transfer out WECOIN
        if (withdrawableAmount > 0)
            WECOIN.transfer(msg.sender, withdrawableAmount);
    }

    //-----------------------------------------------------------------------------------
    // Internal & Private functions
    //-----------------------------------------------------------------------------------
    /**
     * Adjust the current accumulator to the current epoch and timestamp.
     * @dev This function checks 2 different scenarios:
     * 1. If the current epoch is the same as the last accumulated epoch, it will update the accumulator with the reward difference between the last update and the current timestamp.
     * 2. If the current epoch is greater than the last accumulated epoch, it will update the accumulator with the reward difference between the last epoch, any in between epochs(unlikely) and the current epoch.
     */
    function _updateAccumulator() internal {
        // Make sure updates only happen after initial time
        if (block.timestamp <= prevTimestamp) return;

        uint currentEpoch = _currentEpoch();

        uint rewardOffset = lastTotalReward;
        uint epochEmissions = 0;
        uint baseStakingPower = _latestStakingPower(lastAccumulatedEpoch);
        // NOTE Single EPOCH update
        if (currentEpoch == lastAccumulatedEpoch) {
            if (block.timestamp > prevTimestamp) {
                uint diff = block.timestamp - prevTimestamp;
                // Get the next accumulation of rewards
                epochs[currentEpoch].epochTotalBaseReward = rewardOffset;
                diff =
                    (rewardOffset * WEEKLY_EMISSIONS * diff * MAGNIFIER) /
                    (EPOCH_DURATION * EMISSIONS_BASE * baseStakingPower);

                accumulatedRewardsPerStakingPower += diff;

                epochs[currentEpoch]
                    .finalEpochAccumulation = accumulatedRewardsPerStakingPower;

                prevTimestamp = block.timestamp;
            }
            return;
        }
        // NOTE Multiple Epoch diff update
        uint emissionDenominator = EMISSIONS_BASE * EPOCH_DURATION;

        for (uint i = lastAccumulatedEpoch; i <= currentEpoch; i++) {
            EpochInfo storage epoch = epochs[i];
            epoch.epochTotalBaseReward = rewardOffset;

            if (nextEpochRewardAddition > 0 && i > lastAccumulatedEpoch) {
                rewardOffset += nextEpochRewardAddition;
                nextEpochRewardAddition = 0;
                epoch.epochTotalBaseReward = rewardOffset;
            }
            epochEmissions = (rewardOffset * WEEKLY_EMISSIONS);
            rewardOffset -= epochEmissions / EMISSIONS_BASE;
            // NOTE Previous epoch accumulated
            if (i == lastAccumulatedEpoch) {
                uint lastEpochTimeDiff = (lastAccumulatedEpoch *
                    EPOCH_DURATION) + stakingStartTime;
                lastEpochTimeDiff += EPOCH_DURATION;
                lastEpochTimeDiff -= prevTimestamp;
                lastEpochTimeDiff =
                    (epochEmissions * lastEpochTimeDiff * MAGNIFIER) /
                    (baseStakingPower * emissionDenominator);

                accumulatedRewardsPerStakingPower += lastEpochTimeDiff;
            }
            // NOTE current epoch reached
            else if (i == currentEpoch) {
                baseStakingPower = _latestStakingPower(i);
                uint diff1 = block.timestamp -
                    (i * EPOCH_DURATION + stakingStartTime);
                diff1 =
                    (epochEmissions * diff1 * MAGNIFIER) /
                    (baseStakingPower * emissionDenominator);
                accumulatedRewardsPerStakingPower += diff1;
            }
            // NOTE Any epochs between last and current epochs
            else {
                baseStakingPower = _latestStakingPower(i);
                accumulatedRewardsPerStakingPower +=
                    (epochEmissions * MAGNIFIER * EPOCH_DURATION) /
                    (baseStakingPower * emissionDenominator);
            }
            epoch.finalEpochAccumulation = accumulatedRewardsPerStakingPower;
        }
        lastAccumulatedEpoch = currentEpoch;
        lastTotalReward = rewardOffset;
        prevTimestamp = block.timestamp;
    }

    /**
     * Returns the total staking power for the given epoch.
     * @param epochToCheck The epoch to check if adjustement ahs been made
     * @return The total staking power with adjustments made for the given epoch
     */
    function _latestStakingPower(uint epochToCheck) internal returns (uint) {
        EpochInfo storage epoch = epochs[epochToCheck];
        (
            uint stakingPower,
            uint adjustment,
            bool update
        ) = _getLatestStakingPower(epochToCheck);
        if (update) {
            epoch.adjusted = true;
            totalBonusStakingPower -= adjustment;
        }
        return stakingPower;
    }

    function _getLatestStakingPower(
        uint epochToCheck
    ) internal view returns (uint stakingPower, uint adjustment, bool update) {
        EpochInfo storage epoch = epochs[epochToCheck];
        if (epoch.adjusted)
            return (totalBaseStakingPower + totalBonusStakingPower, 0, false);
        return (
            totalBaseStakingPower +
                totalBonusStakingPower -
                epoch.totalBonusStakingPowerAdjustment,
            epoch.totalBonusStakingPowerAdjustment,
            true
        );
    }

    /**
     * Claims the user's current rewards and if time is before lock period, locks them in place. Otherwise it transfers the user their rewards.
     *
     */
    function _claimAndLock(address _user) private {
        _updateAccumulator();

        UserInfo storage user = users[_user];
        if (user.depositAmount == 0) return;
        // Check current EPOCH, if it's <= user.endLockEpoch
        // this determines which stakingPower to use for rewards.
        uint lastEpoch = _getLastEpoch(user.lastAction);
        uint currentEpoch = _currentEpoch();
        uint userStakingPower = user.depositAmount;
        uint accumulatedRewards;
        uint claimableRewards;
        bool hasLocked = user.endLockEpoch != 0;

        if (currentEpoch <= user.endLockEpoch && hasLocked) {
            userStakingPower += user.bonusAmount;
            accumulatedRewards =
                userStakingPower *
                accumulatedRewardsPerStakingPower;
            user.lockedRewards +=
                (accumulatedRewards - user.offsetPoints) /
                MAGNIFIER;
            user.offsetPoints = accumulatedRewards;
            emit LockReward(_user, user.lockedRewards);
        } else if (
            currentEpoch > user.endLockEpoch &&
            lastEpoch <= user.endLockEpoch &&
            hasLocked
        ) {
            // Add full rewards UP TO end of LOCK epoch
            uint endEpochAccumulated = epochs[user.endLockEpoch]
                .finalEpochAccumulation;
            userStakingPower += user.bonusAmount;

            accumulatedRewards = userStakingPower * endEpochAccumulated;
            claimableRewards = accumulatedRewards - user.offsetPoints;

            endEpochAccumulated = user.depositAmount * endEpochAccumulated;

            userStakingPower = user.depositAmount;
            accumulatedRewards =
                userStakingPower *
                accumulatedRewardsPerStakingPower;
            claimableRewards += accumulatedRewards - endEpochAccumulated;
            claimableRewards /= MAGNIFIER;
            claimableRewards += user.lockedRewards;
            user.offsetPoints = accumulatedRewards;
            user.lockedRewards = 0;
        } else {
            accumulatedRewards =
                userStakingPower *
                accumulatedRewardsPerStakingPower;
            claimableRewards =
                (accumulatedRewards - user.offsetPoints) /
                MAGNIFIER;
            user.offsetPoints = accumulatedRewards;
            user.lockedRewards = 0;
        }

        user.lastAction = block.timestamp;

        if (claimableRewards > 0) {
            WECOIN.transfer(_user, claimableRewards);
            emit ClaimReward(_user, claimableRewards);
        }
    }

    //-----------------------------------------------------------------------------------
    // Internal view functions
    //-----------------------------------------------------------------------------------
    /**
     * @return The current epoch number
     * @dev if staking has not started, it will return 0
     */
    function _currentEpoch() internal view returns (uint256) {
        return _getLastEpoch(block.timestamp);
    }

    function _getLastEpoch(uint timestamp) internal view returns (uint) {
        if (stakingStartTime > timestamp) return 0;
        return (timestamp - stakingStartTime) / EPOCH_DURATION;
    }

    //-----------------------------------------------------------------------------------
    // External/Public view/pure functions
    //-----------------------------------------------------------------------------------
    function calculateMultiplier(
        uint _lockedEpochs
    ) public pure returns (uint) {
        return 10000 + (25 * Math.sqrt(_lockedEpochs * SQRT_ADJUSTMENT));
    }

    function getStakingPower(address _user) external view returns (uint) {
        UserInfo storage user = users[_user];
        if (user.endLockEpoch >= _currentEpoch())
            return user.depositAmount + user.bonusAmount;
        return user.depositAmount;
    }

    function getCurrentTotalStakingPower()
        external
        view
        returns (uint totalStakingPower)
    {
        uint currentEpoch = _currentEpoch();
        totalStakingPower = totalBaseStakingPower + totalBonusStakingPower;
        for (uint i = lastAccumulatedEpoch; i <= currentEpoch; i++) {
            EpochInfo storage epoch = epochs[i];
            if (!epoch.adjusted)
                totalStakingPower -= epoch.totalBonusStakingPowerAdjustment;
        }
    }

    function getCurrentEpoch() external view returns (uint) {
        return _currentEpoch();
    }

    function getEpochFromTimestamp(
        uint timestamp
    ) external view returns (uint) {
        return _getLastEpoch(timestamp);
    }

    function pendingRewards(address _user) external view returns (uint) {
        UserInfo storage user = users[_user];
        if (user.depositAmount == 0) return 0;

        uint lastEpoch = _getLastEpoch(user.lastAction);
        uint lastTimestamp = user.lastAction;
        uint currentEpoch = _currentEpoch();
        uint rewardsAccumulated;
        uint epochTotal; // total reward pool amount on that epoch
        uint totalEmitted;
        uint usedStakingPower;
        uint offset;

        // USE accumulators
        if (user.lastAction < prevTimestamp) {
            rewardsAccumulated =
                user.depositAmount *
                accumulatedRewardsPerStakingPower;
            if (user.endLockEpoch > 0) {
                if (user.endLockEpoch >= lastAccumulatedEpoch) {
                    rewardsAccumulated +=
                        user.bonusAmount *
                        accumulatedRewardsPerStakingPower;
                } else {
                    rewardsAccumulated +=
                        user.bonusAmount *
                        epochs[user.endLockEpoch].finalEpochAccumulation;
                }
            }
            lastEpoch = lastAccumulatedEpoch;
            lastTimestamp = prevTimestamp;
        }

        for (uint i = lastEpoch; i <= currentEpoch; i++) {
            uint userStakingPower = user.depositAmount;
            // If current EPOCHTotalBaseReward is 0, get previous epoch and adjust
            if (epochs[i].epochTotalBaseReward == 0 && i > 0) {
                if (epochs[i - 1].epochTotalBaseReward > 0)
                    epochTotal = epochs[i - 1].epochTotalBaseReward;

                totalEmitted = (epochTotal * WEEKLY_EMISSIONS) / EMISSIONS_BASE;
                epochTotal -= totalEmitted;
                totalEmitted = 0;
            } else {
                epochTotal = epochs[i].epochTotalBaseReward;
            }
            (usedStakingPower, , ) = _getLatestStakingPower(i);
            if (user.endLockEpoch >= i && user.endLockEpoch != 0) {
                userStakingPower += user.bonusAmount;
            }
            if (i < lastAccumulatedEpoch && i > 0) {}
            totalEmitted = epochTotal * WEEKLY_EMISSIONS * MAGNIFIER;
            if (lastEpoch == currentEpoch) {
                offset = block.timestamp - lastTimestamp;
                totalEmitted =
                    (totalEmitted * offset * userStakingPower) /
                    (EMISSIONS_BASE * EPOCH_DURATION * usedStakingPower);
                return (rewardsAccumulated + totalEmitted) / MAGNIFIER;
            }
            offset = stakingStartTime + (i * EPOCH_DURATION);
            if (i == lastEpoch) {
                offset = offset + EPOCH_DURATION - lastTimestamp;
                totalEmitted =
                    (totalEmitted * offset * userStakingPower) /
                    (EMISSIONS_BASE * EPOCH_DURATION * usedStakingPower);
                rewardsAccumulated += totalEmitted;
            } else if (i == currentEpoch) {
                offset = block.timestamp - offset;
                totalEmitted =
                    (totalEmitted * offset * userStakingPower) /
                    (EMISSIONS_BASE * EPOCH_DURATION * usedStakingPower);
                rewardsAccumulated += totalEmitted;
            } else {
                totalEmitted =
                    (totalEmitted * userStakingPower) /
                    (usedStakingPower * EMISSIONS_BASE);
                rewardsAccumulated += totalEmitted;
            }
        }
        return rewardsAccumulated / MAGNIFIER;
    }
}