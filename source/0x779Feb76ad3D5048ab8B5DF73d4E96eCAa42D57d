// File: @openzeppelin/contracts/utils/math/Math.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/math/Math.sol)

pragma solidity ^0.8.0;

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

// File: @openzeppelin/contracts/security/ReentrancyGuard.sol


// OpenZeppelin Contracts (last updated v4.9.0) (security/ReentrancyGuard.sol)

pragma solidity ^0.8.0;

/**
 * @dev Contract module that helps prevent reentrant calls to a function.
 *
 * Inheriting from `ReentrancyGuard` will make the {nonReentrant} modifier
 * available, which can be applied to functions to make sure there are no nested
 * (reentrant) calls to them.
 *
 * Note that because there is a single `nonReentrant` guard, functions marked as
 * `nonReentrant` may not call one another. This can be worked around by making
 * those functions `private`, and then adding `external` `nonReentrant` entry
 * points to them.
 *
 * TIP: If you would like to learn more about reentrancy and alternative ways
 * to protect against it, check out our blog post
 * https://blog.openzeppelin.com/reentrancy-after-istanbul/[Reentrancy After Istanbul].
 */
abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _status will be _NOT_ENTERED
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Returns true if the reentrancy guard is currently set to "entered", which indicates there is a
     * `nonReentrant` function in the call stack.
     */
    function _reentrancyGuardEntered() internal view returns (bool) {
        return _status == _ENTERED;
    }
}

// File: contracts/Glutech.sol

//SPDX-License-Identifier: Unlicense
pragma solidity 0.8.9;



contract StakingBot is ReentrancyGuard {
    struct StakingPlan {
        uint256 minStakeAmount;
        uint256 maxStakeAmount;
        uint256 dailyROI;
        uint256 stakingPeriod;
        uint256 totalStaked;
        uint256 totalPayouts;
    }

    struct Staking {
        uint256 rewardDebt;
        uint256 totalInvestments;
        uint256 amount;
        uint256 initialTime;
        uint256 earningStartTime;
        uint256 totalWithdrawal;
        uint256 lockEndTime;
    }

    struct User {
        uint256 referralDebt;
        uint256 teamSalesDebt;
        uint256 lastTeamHarvest;
        uint256 totalInvestments;
        address referrer;
        mapping(uint256 => Staking) stakings;
        uint256 currentRank;
        uint256 totalDirectReferrals;
        uint256 totalCommissionEarned;
        address[] referrals;
        uint256 totalTeam;
        uint256 leadershipScore;
    }

    struct LeadershipRank {
        uint256 weeklyEarnings;
        uint256 directReferrals;
        uint256 teamVolume;
        uint256 investments;
    }

    address payable public immutable admin;
    address payable public immutable liquiditySupportBot;

    uint16 private constant HARVEST_FEE_PERCENTAGE = 300; // in percentage
    uint16 private constant PENALTY_PERCENTAGE = 2500; // in percentage
    uint16 private constant REFERRAL_LEVELS = 10;
    uint16 public constant LIQUIDITY_SUPPORT_PERCENTAGE = 3000; // 30%

    mapping(uint256 => LeadershipRank) public leadershipRanks;
    mapping(uint256 => StakingPlan) public stakingPlans;
    mapping(uint256 => uint256) public referralLevels;
    mapping(address => uint256) public referralCounts;
    mapping(address => address) private referrals;
    mapping(address => User) public users;

    uint256 public immutable contractInitializedAt;
    uint256 public totalTeams = 0;

    event Staked(address indexed staker, uint256 amount);
    event ReleaseBotSupport(uint256 amount);
    event Unstaked(address indexed staker, uint256 amount);
    event Harvested(address indexed staker, uint256 amount);
    event ReferralRecorded(address indexed user, address indexed referrer);
    event ReferralEarningsReceived(address indexed user, uint256 amount);
    event PenaltyCharged(address indexed offender, uint256 amount);

    modifier validPlanId(uint256 planId) {
        require(planId >= 1 && planId <= 3, "Invalid plan ID");
        _;
    }

    constructor(
        address payable adminAdd,
        address payable liquiditySupportBotAddress
    ) {
        require(
            adminAdd != address(0) && liquiditySupportBotAddress != address(0)
        );
        admin = adminAdd;
        liquiditySupportBot = liquiditySupportBotAddress;
        contractInitializedAt = block.timestamp;

        // Initialize referral levels
        referralLevels[1] = 1000;
        referralLevels[2] = 700;
        referralLevels[3] = 500;
        referralLevels[4] = 300;
        referralLevels[5] = 200;
        referralLevels[6] = 200;
        referralLevels[7] = 200;
        referralLevels[8] = 50;
        referralLevels[9] = 50;
        referralLevels[10] = 50;

        leadershipRanks[1] = LeadershipRank(
            0.18 ether,
            5,
            18.01 ether,
            0.36 ether
        );
        leadershipRanks[2] = LeadershipRank(
            0.36 ether,
            7,
            36 ether,
            0.36 ether
        );
        leadershipRanks[3] = LeadershipRank(
            0.90 ether,
            10,
            72.05 ether,
            1.80 ether
        );
        leadershipRanks[4] = LeadershipRank(
            1.8 ether,
            20,
            180.14 ether,
            3.60 ether
        );
        leadershipRanks[5] = LeadershipRank(
            7.21 ether,
            20,
            360.27 ether,
            10.7959 ether
        );
        leadershipRanks[6] = LeadershipRank(
            18.01 ether,
            20,
            900.69 ether,
            18 ether
        );
        leadershipRanks[7] = LeadershipRank(
            36.03 ether,
            20,
            3602.74 ether,
            36 ether
        );

        // Initialize staking plans
        stakingPlans[1] = StakingPlan(
            0.033 ether,
            1663.22 ether,
            200,
            5 days,
            0,
            0
        );
        stakingPlans[2] = StakingPlan(
            0.033 ether,
            1663.22 ether,
            250,
            15 days,
            0,
            0
        );
        stakingPlans[3] = StakingPlan(
            0.033 ether,
            1663.22 ether,
            260,
            30 days,
            0,
            0
        );
    }

    function teamEarnings(address userAddress) public view returns (uint256) {
        User storage user = users[userAddress];

        if (user.currentRank == 0 || user.lastTeamHarvest == 0) {
            return 0;
        }

        LeadershipRank memory leadershipRank = leadershipRanks[
            user.currentRank
        ];
        uint256 weeklyEarnings = leadershipRank.weeklyEarnings;

        uint256 lastHarvestWeek = user.lastTeamHarvest / 1 weeks;
        uint256 currentWeek = block.timestamp / 1 weeks;
        uint256 elapsedWeeks = currentWeek - lastHarvestWeek;

        if (elapsedWeeks == 0) {
            return 0;
        }

        uint256 pendingReward = elapsedWeeks * weeklyEarnings;

        return pendingReward;
    }

    function getRewards(
        address userAddress,
        uint256 planId
    ) public view validPlanId(planId) returns (uint256) {
        Staking storage staking = users[userAddress].stakings[planId];

        if (staking.amount == 0) {
            return staking.rewardDebt;
        }

        StakingPlan storage stakingPlan = stakingPlans[planId];
        uint256 timeDiff = block.timestamp - staking.earningStartTime;
        uint256 earningRate = staking.amount * stakingPlan.dailyROI;
        uint256 rewardAmount = (earningRate * Math.min(timeDiff, stakingPlan.stakingPeriod)) / (10000 * 1 days);

        return staking.rewardDebt + rewardAmount;
    }

    function getUserPlanDetails(
        address userAddress,
        uint256 planId
    ) external view returns (Staking memory, uint256) {
        uint256 reward = getRewards(userAddress, planId);
        Staking memory staking = users[userAddress].stakings[planId];
        return (staking, reward);
    }

    function getAllUserPlansEarnings(
        address userAddress
    ) public view returns (uint256) {
        uint256 planOneReward = getRewards(userAddress, 1);
        uint256 planTwoReward = getRewards(userAddress, 2);
        uint256 planThreeReward = getRewards(userAddress, 3);

        return planOneReward + planTwoReward + planThreeReward;
    }

    function getUserDetails(
        address userAddress
    )
        external
        view
        returns (
            uint256,
            uint256,
            uint256,
            uint256,
            address,
            uint256,
            uint256,
            uint256,
            uint256,
            uint256,
            uint256
        )
    {
        uint256 reward = getAllUserPlansEarnings(userAddress);
        User storage user = users[userAddress];
        return (
            user.referralDebt,
            user.teamSalesDebt,
            user.lastTeamHarvest,
            user.totalInvestments,
            user.referrer,
            user.currentRank,
            user.totalDirectReferrals,
            user.totalCommissionEarned,
            user.totalTeam,
            user.leadershipScore,
            reward
        );
    }

    function getAvailableReferralRewards(
        address userAddress
    ) public view returns (uint256) {
        User storage user = users[userAddress];
        return user.referralDebt;
    }

    function getReferralRewards(
        address userAddress
    ) public view returns (uint256) {
        uint256 pendingReward = 0;
        User storage user = users[userAddress];
        for (uint256 i = 0; i < user.referrals.length; i++) {
            uint256 userRewards = getAllUserPlansEarnings(user.referrals[i]);
            uint256 rewardsPercentage = referralLevels[1];
            pendingReward = pendingReward +
                ((userRewards * rewardsPercentage) / 10000);

            User storage referredUser = users[user.referrals[i]];
            uint256 generation = 1;
            uint256 maxGenerations = REFERRAL_LEVELS - 1;

            while (
                generation <= maxGenerations &&
                referredUser.referrals.length > 0
            ) {
                uint256 _referralsCount = referredUser.referrals.length;
                for (uint256 j = 0; j < _referralsCount; j++) {
                    address referrer = referredUser.referrals[j];
                    if (referrer == address(0)) continue;

                    userRewards = getAllUserPlansEarnings(referrer);
                    uint256 referralEarningsPercentage = referralLevels[
                        generation + 1
                    ];

                    uint256 referralEarningsShare = (
                        userRewards * referralEarningsPercentage
                    ) / 10000;
                    pendingReward = pendingReward + referralEarningsShare;
                }

                referredUser = users[referredUser.referrals[0]];
                generation++;
            }
        }

        return pendingReward;
    }

    function stake(
        uint256 planId
    ) external payable nonReentrant validPlanId(planId) {
        require(
            msg.value >= stakingPlans[planId].minStakeAmount &&
                msg.value <= stakingPlans[planId].maxStakeAmount,
            "Invalid staking amount"
        );

        require(users[msg.sender].referrer != address(0), "You must be referred to participate");

        updateUplinesEarnings(msg.sender, planId);
        updateUserStake(msg.sender, planId);

        User storage user = users[msg.sender];
        user.totalInvestments = user.totalInvestments + msg.value;

        StakingPlan storage stakingPlan = stakingPlans[planId];

        Staking storage staking = users[msg.sender].stakings[planId];
        staking.amount += msg.value;
        staking.initialTime = block.timestamp;
        staking.earningStartTime = block.timestamp;
        staking.lockEndTime = block.timestamp + stakingPlan.stakingPeriod;
        staking.totalInvestments = staking.totalInvestments + msg.value;

        stakingPlan.totalStaked += msg.value;

        updateLeadershipRank(msg.sender);
        balanceLeadershipRank(msg.sender, msg.value);

        emit Staked(msg.sender, msg.value);

        uint256 liquiditySupportFee = (
            msg.value * LIQUIDITY_SUPPORT_PERCENTAGE
        ) / 10000;

        emit ReleaseBotSupport(liquiditySupportFee);

        liquiditySupportBot.transfer(liquiditySupportFee);
    }

    function harvest(uint256 planId) external nonReentrant validPlanId(planId) {
        User storage user = users[msg.sender];
        Staking storage staking = users[msg.sender].stakings[planId];
        StakingPlan storage stakingPlan = stakingPlans[planId];

        uint256 teamSalesEarnings = user.teamSalesDebt + teamEarnings(msg.sender);
        uint256 rewardAmount = getRewards(msg.sender, planId) + teamSalesEarnings;
        require(rewardAmount > 0, "harvest: not enough funds");

        updateUplinesEarnings(msg.sender, planId);

        staking.rewardDebt = 0;
        staking.totalWithdrawal = staking.totalWithdrawal + rewardAmount;
        staking.earningStartTime = block.timestamp;

        user.teamSalesDebt = 0;
        user.lastTeamHarvest = block.timestamp;

        uint256 harvestFee = (rewardAmount * HARVEST_FEE_PERCENTAGE) / 10000;

        stakingPlan.totalPayouts += rewardAmount;

        emit Harvested(msg.sender, rewardAmount - harvestFee);

        payable(msg.sender).transfer(rewardAmount - harvestFee);
        admin.transfer(harvestFee);
    }

    function harvestReferralEarnings() external nonReentrant {
        User storage user = users[msg.sender];
        require(user.referralDebt > 0, "harvest: not enough funds");

        uint256 rewardAmount = user.referralDebt;

        user.referralDebt = 0;

        uint256 harvestFee = (rewardAmount * HARVEST_FEE_PERCENTAGE) / 10000;

        emit Harvested(msg.sender, rewardAmount - harvestFee);

        payable(msg.sender).transfer(rewardAmount - harvestFee);
        admin.transfer(harvestFee);
    }

    function unstake(uint256 planId) external nonReentrant validPlanId(planId) {
        User storage user = users[msg.sender];
        Staking storage staking = users[msg.sender].stakings[planId];
        StakingPlan storage stakingPlan = stakingPlans[planId];

        uint256 teamSalesEarnings = user.teamSalesDebt + teamEarnings(msg.sender);

        uint256 totalBalance = getRewards(msg.sender, planId) + staking.amount + teamSalesEarnings;
        require(totalBalance > 0, "Unstake: nothing to unstake");

        uint256 harvestFee = (totalBalance * HARVEST_FEE_PERCENTAGE) / 10000;
        uint256 harvestableAmount = _harvestableAmount(
            totalBalance,
            harvestFee,
            msg.sender,
            planId
        );

        require(
            address(this).balance >= totalBalance,
            "Insufficient fund to initiate unstake"
        );

        updateUplinesEarnings(msg.sender, planId);
        updateUserStake(msg.sender, planId);

        staking.amount = 0;
        staking.rewardDebt = 0;
        staking.totalWithdrawal = staking.totalWithdrawal + totalBalance;
        staking.earningStartTime = 0;

        user.teamSalesDebt = 0;
        user.lastTeamHarvest = block.timestamp;

        stakingPlan.totalPayouts += totalBalance;

        emit Unstaked(msg.sender, harvestableAmount);

        payable(msg.sender).transfer(harvestableAmount);
        admin.transfer(harvestFee);
    }

    function recordReferral(address referrerAddress) public nonReentrant {
        require(msg.sender.code.length == 0, "Contracts not allowed.");

        // Check for circular referral
        address currentReferrer = referrerAddress;
        while (currentReferrer != address(0)) {
            require(
                currentReferrer != msg.sender,
                "Circular referral detected."
            );
            currentReferrer = referrals[currentReferrer];
        }

        if (referrerAddress != address(0) && referrals[msg.sender] == address(0)) {
            User storage user = users[referrerAddress];
            User storage referredUser = users[msg.sender];

            referrals[msg.sender] = referrerAddress;
            referralCounts[referrerAddress]++;

            referredUser.referrer = referrerAddress;

            user.referrals.push(msg.sender);
            user.totalDirectReferrals = user.totalDirectReferrals + 1;

            totalTeams = totalTeams + 1;

            updateUplines(msg.sender);

            emit ReferralRecorded(msg.sender, referrerAddress);
        }
    }

    function _harvestableAmount(
        uint256 _amount,
        uint256 _harvestFee,
        address userAddress,
        uint256 planId
    ) private view returns (uint256) {
        Staking storage staking = users[userAddress].stakings[planId];
        uint256 harvestableAmount = _amount - _harvestFee;

        if (staking.lockEndTime > block.timestamp) {
            uint256 penalty = (harvestableAmount * PENALTY_PERCENTAGE) / 10000;
            harvestableAmount = harvestableAmount - penalty;
        }

        return harvestableAmount;
    }

    function updateUserStake(address userAddress, uint256 planId) internal {
        Staking storage staking = users[userAddress].stakings[planId];
        StakingPlan storage stakingPlan = stakingPlans[planId];

        uint256 rewardAmount = getRewards(userAddress, planId);
        staking.rewardDebt = rewardAmount;
        staking.initialTime = block.timestamp;
        staking.lockEndTime = staking.initialTime + stakingPlan.stakingPeriod;
    }

    function updateUplinesEarnings(address userAddress, uint256 planId) internal {
        if (referrals[userAddress] != address(0)) {
            uint256 userEarnings = getRewards(userAddress, planId);
            address[] memory userUps = getUplines(userAddress);

            for (uint i = 0; i < userUps.length; i++) {
                if (userUps[i] == address(0)) {
                    break;
                }
                User storage user = users[userUps[i]];
                uint256 referralEarningsPercentage = referralLevels[i + 1];
                uint256 referralReward = userEarnings * referralEarningsPercentage / 10000;
                user.referralDebt = user.referralDebt + referralReward;
                user.totalCommissionEarned += referralReward;
                emit ReferralEarningsReceived(userUps[i], referralReward);
            }
        }
    }

    function updateUplines(address userAddress) internal {
        address[] memory userUplines = getUplines(userAddress);

        for (uint256 i = 0; i < userUplines.length; i++) {
            address referrer = userUplines[i];

            if (referrer == address(0)) {
                break;
            }
            updateLeadershipRank(referrer);
            User storage user = users[referrer];
            user.totalTeam = user.totalTeam + 1;
        }
    }

    function getUplines(
        address userAddress
    ) internal view returns (address[] memory) {
        uint16 limit = 10;
        address[] memory uplines = new address[](limit);
        address current = userAddress;
        for (uint i = 0; i < limit; i++) {
            if (referrals[current] == address(0)) {
                break;
            }
            uplines[i] = referrals[current];
            current = uplines[i];
        }
        return uplines;
    }

    function balanceLeadershipRank(
        address userAddress,
        uint256 transactionAmount
    ) internal {
        address referrer = referrals[userAddress];
        if (referrer != address(0)) {
            address[] memory userUps = getUplines(userAddress);

            for (uint256 i = 0; i < userUps.length; i++) {
                if (userUps[i] == address(0)) {
                    break;
                }
                User storage user = users[userUps[i]];
                user.leadershipScore = user.leadershipScore + transactionAmount;
                updateLeadershipRank(userUps[i]);
            }
        }
    }

    function updateLeadershipRank(address uplineAddress) internal {
        uint256 totalLeadershipRanks = 7;
        User storage user = users[uplineAddress];
        uint256 currentPosition = user.currentRank;
        if (currentPosition != totalLeadershipRanks) {
            for (
                uint256 index = currentPosition;
                index < totalLeadershipRanks;
                index++
            ) {
                LeadershipRank memory leadershipRank = leadershipRanks[
                    index + 1
                ];
                if (
                    user.leadershipScore >= leadershipRank.teamVolume &&
                    user.totalInvestments >= leadershipRank.investments &&
                    user.totalDirectReferrals >= leadershipRank.directReferrals
                ) {
                    currentPosition = currentPosition + 1;
                }
            }
            if (currentPosition != user.currentRank) {
                user.teamSalesDebt += teamEarnings(uplineAddress);
                user.lastTeamHarvest = block.timestamp;
            }
            user.currentRank = currentPosition;
        }
    }
}