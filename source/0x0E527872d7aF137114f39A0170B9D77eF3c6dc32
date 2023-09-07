// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;
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

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

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
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
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

library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
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
    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
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
    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
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
    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
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

interface AggregatorV3Interface {
    function decimals() external view returns (uint8);

    function description() external view returns (string memory);

    function version() external view returns (uint256);

    function getRoundData(
        uint80 _roundId
    )
        external
        view
        returns (
            uint80 roundId,
            uint256 answer,
            uint256 startedAt,
            uint256 updatedAt,
            uint80 answeredInRound
        );

    function latestRoundData()
        external
        view
        returns (
            uint80 roundId,
            uint256 answer,
            uint256 startedAt,
            uint256 updatedAt,
            uint80 answeredInRound
        );
}

contract PriceConsumerV3 {
    AggregatorV3Interface internal priceFeed;

    function getLatestPrice() public view returns (uint) {
        (, uint price, , , ) = priceFeed.latestRoundData();

        return uint256(price);
    }
}

contract BFMTokenPresale is Ownable, PriceConsumerV3 {
    using SafeMath for uint256;
    uint public referrerPercentage = 5;
    uint256 public minbuyToken = 10000e8;
    uint256 public maxbuyToken = 1000000e8;

    enum PresalePhase {
        Phase1,
        Phase2,
        Phase3
    }

    struct PresaleInfo {
        uint256 totalTokens;
        uint256 tokenPrice;
        uint256 releaseStart;
        uint256 releaseDuration;
        uint256 totalSold;
        uint256[] releasedPercentPerMonth;
        uint256 phasetime;
    }

    IERC20 public bfmToken;
    PresaleInfo[3] public presalePhases;

    mapping(address => mapping(uint256 => uint256)) private balances;
    mapping(address => mapping(PresalePhase => uint256))
        private releasedAmounts;
    mapping(address => address) public referrers;

    event TokensPurchased(
        address indexed buyer,
        uint256 amount,
        uint256 paidAmount,
        PresalePhase phase
    );
    event TokensReleased(
        address indexed buyer,
        uint256 amount,
        PresalePhase phase
    );

    constructor(IERC20 token) {
        bfmToken = token;
        priceFeed = AggregatorV3Interface(
            0x0567F2323251f0Aab15c8dFb1967E4e8A7D42aeE
        );

        uint256[] memory p1ReleasedPercentPerMonth = new uint256[](6);
        p1ReleasedPercentPerMonth[0] = 50;
        p1ReleasedPercentPerMonth[1] = 10;
        p1ReleasedPercentPerMonth[2] = 10;
        p1ReleasedPercentPerMonth[3] = 10;
        p1ReleasedPercentPerMonth[4] = 10;
        p1ReleasedPercentPerMonth[5] = 10;

        presalePhases[uint256(PresalePhase.Phase1)] = PresaleInfo(
            3000000 * 1e8,
            14285714285,
            block.timestamp + 9 * 30 days, // Lock period of 9 months
            30 days,
            0,
            p1ReleasedPercentPerMonth,
            block.timestamp + 7 days
        );

        uint256[] memory p2ReleasedPercentPerMonth = new uint256[](6);
        p2ReleasedPercentPerMonth[3] = 10;
        p2ReleasedPercentPerMonth[0] = 50;
        p2ReleasedPercentPerMonth[1] = 10;
        p2ReleasedPercentPerMonth[2] = 10;
        p2ReleasedPercentPerMonth[4] = 10;
        p2ReleasedPercentPerMonth[5] = 10;

        presalePhases[uint256(PresalePhase.Phase2)] = PresaleInfo(
            10000000 * 1e8,
            11111111111,
            presalePhases[uint256(PresalePhase.Phase1)].releaseStart +
                6 *
                30 days, // Lock period of 6 months,
            30 days,
            0,
            p2ReleasedPercentPerMonth,
            presalePhases[uint256(PresalePhase.Phase1)].phasetime + 21 days
        );

        uint256[] memory p3ReleasedPercentPerMonth = new uint256[](4);
        p3ReleasedPercentPerMonth[0] = 50;
        p3ReleasedPercentPerMonth[1] = 15;
        p3ReleasedPercentPerMonth[2] = 15;
        p3ReleasedPercentPerMonth[3] = 20;
        presalePhases[uint256(PresalePhase.Phase3)] = PresaleInfo(
            17000000 * 1e8,
            9090909090,
            presalePhases[uint256(PresalePhase.Phase2)].releaseStart +
                3 *
                30 days, // Lock period of 3 months,
            30 days,
            0,
            p3ReleasedPercentPerMonth,
            presalePhases[uint256(PresalePhase.Phase2)].phasetime + 40 days
        );
    }

    function buyTokensWithReferral(
        PresalePhase phase,
        address referrer
    ) external payable {
        require(referrer != msg.sender, "Cannot refer yourself");
        require(phase == getActivePhase(), "Invalid phase");
        require(
            referrers[msg.sender] == address(0),
            "You already have a referrer"
        );

        referrers[msg.sender] = referrer;
        buyTokens(phase);

        if (referrer != address(0)) {
            uint256 referralReward = msg.value.mul(referrerPercentage).div(100);
            payable(referrer).transfer(referralReward);
        }
    }

    function buyTokens(PresalePhase phase) public payable {
        require(msg.value > 0, "Amount must be greater than 0");
        require(phase == getActivePhase(), "Invalid phase");
        PresaleInfo storage presale = presalePhases[uint256(phase)];
        require(block.timestamp < presale.phasetime, "Phase is not active");
        uint256 tokensToBuy = bnbToToken(msg.value, phase);
        require(
            tokensToBuy >= minbuyToken,
            "Minimum purchase is 10,000 tokens"
        );
        require(
            tokensToBuy <= maxbuyToken,
            "Maximum purchase is 1,000,000 tokens"
        );

        require(
            presale.totalSold.add(tokensToBuy) <= presale.totalTokens,
            "Not enough tokens left for sale"
        );

        balances[msg.sender][uint256(phase)] = balances[msg.sender][
            uint256(phase)
        ].add(tokensToBuy);
        presale.totalSold = presale.totalSold.add(tokensToBuy);

        emit TokensPurchased(msg.sender, tokensToBuy, msg.value, phase);
    }

    function bnbToToken(
        uint256 bnb,
        PresalePhase phase
    ) public view returns (uint256) {
        PresaleInfo storage presale = presalePhases[uint256(phase)];
        uint256 bnbToUsd = bnb.mul(getLatestPrice());
        uint256 numberOfTokens = bnbToUsd.mul(presale.tokenPrice);
        return numberOfTokens.div(1e18).div(1e8);
    }

    function releaseTokens(PresalePhase phase) external {
        PresaleInfo storage presale = presalePhases[uint256(phase)];
        require(
            block.timestamp >= presale.releaseStart,
            "Presale for this phase hasn't started yet, Please wait for next phase"
        );
        require(block.timestamp >= presale.phasetime, "Phase Not closed");
        uint256 _releasableAmount = releasableAmount(msg.sender, phase);
        require(_releasableAmount > 0, "No tokens to release");

        releasedAmounts[msg.sender][phase] = releasedAmounts[msg.sender][phase]
            .add(_releasableAmount);
        bfmToken.transferFrom(owner(), msg.sender, _releasableAmount);

        emit TokensReleased(msg.sender, _releasableAmount, phase);
    }

    function releasableAmount(
        address user,
        PresalePhase phase
    ) public view returns (uint256) {
        PresaleInfo storage presale = presalePhases[uint256(phase)];

        uint256 elapsedTime = block.timestamp.sub(presale.releaseStart);
        uint256 releasedMonths = elapsedTime.div(presale.releaseDuration);
        uint256 userPhaseBalance = balances[user][uint256(phase)];

        uint256 totalReleasableAmount = 0;
        for (
            uint256 i = 0;
            i <= releasedMonths && i < presale.releasedPercentPerMonth.length;
            i++
        ) {
            totalReleasableAmount = totalReleasableAmount.add(
                userPhaseBalance.mul(presale.releasedPercentPerMonth[i]).div(
                    100
                )
            );
        }

        return totalReleasableAmount.sub(releasedAmounts[user][phase]);
    }

    function getActivePhase() public view returns (PresalePhase) {
        uint256 currentTimestamp = block.timestamp;
        PresalePhase activePhase = PresalePhase.Phase3;

        for (
            uint256 i = uint256(PresalePhase.Phase1);
            i <= uint256(PresalePhase.Phase3);
            i++
        ) {
            if (
                currentTimestamp < presalePhases[i].phasetime &&
                presalePhases[i].totalSold < presalePhases[i].totalTokens
            ) {
                activePhase = PresalePhase(i);
                break;
            }
        }

        return activePhase;
    }

    function getUserBalanceInfo(
        address user,
        PresalePhase phase
    )
        public
        view
        returns (uint256 totalBalance, uint256 released, uint256 pendingRelease)
    {
        totalBalance = balances[user][uint256(phase)];
        released = releasedAmounts[user][phase];
        pendingRelease = totalBalance.sub(released);
    }

    function withdrawBNB() external onlyOwner {
        payable(owner()).transfer(address(this).balance);
    }

    function setMinBuyToken(uint256 _newMinBuyToken) external onlyOwner {
        minbuyToken = _newMinBuyToken;
    }

    function setMaxBuyToken(uint256 _newMaxBuyToken) external onlyOwner {
        maxbuyToken = _newMaxBuyToken;
    }

    function setReferrerPercentage(uint256 _percentage) external onlyOwner {
        require(
            referrerPercentage > 0 && referrerPercentage <= 100,
            "referrerPercentage must be greater than 0 and less than 100"
        );
        referrerPercentage = _percentage;
    }

    function setToken(IERC20 token) external onlyOwner {
        require(
            address(token) != address(0),
            "Token address cannot be the zero address"
        );
        bfmToken = token;
    }

    function setTokenPrice(
        PresalePhase phase,
        uint256 _price
    ) external onlyOwner {
        require(
            phase == PresalePhase.Phase1 ||
                phase == PresalePhase.Phase2 ||
                phase == PresalePhase.Phase3,
            "Invalid phase"
        );
        presalePhases[uint256(phase)].tokenPrice = _price;
    }

    function setReleaseStart(
        PresalePhase phase,
        uint256 _releaseStart
    ) external onlyOwner {
        require(
            phase == PresalePhase.Phase1 ||
                phase == PresalePhase.Phase2 ||
                phase == PresalePhase.Phase3,
            "Invalid phase"
        );
        presalePhases[uint256(phase)].releaseStart =
            block.timestamp +
            _releaseStart;
    }

    function setReleasedPercentPerMonth(
        PresalePhase phase,
        uint256 _releaseDuration
    ) external onlyOwner {
        require(
            phase == PresalePhase.Phase1 ||
                phase == PresalePhase.Phase2 ||
                phase == PresalePhase.Phase3,
            "Invalid phase"
        );
        presalePhases[uint256(phase)].releaseDuration = _releaseDuration;
    }

    function setPhaseTime(
        PresalePhase phase,
        uint256 _phasetime
    ) external onlyOwner {
        require(
            phase == PresalePhase.Phase1 ||
                phase == PresalePhase.Phase2 ||
                phase == PresalePhase.Phase3,
            "Invalid phase"
        );
        presalePhases[uint256(phase)].phasetime = block.timestamp + _phasetime;
    }

    function emergencyWithdrawTokens(
        address _token,
        uint256 _amount
    ) external onlyOwner {
        IERC20(_token).transfer(owner(), _amount);
    }

    function setTokensaleLimits(
        PresalePhase phase,
        uint256 _totalTokens
    ) external onlyOwner {
        require(
            phase == PresalePhase.Phase1 ||
                phase == PresalePhase.Phase2 ||
                phase == PresalePhase.Phase3,
            "Invalid phase"
        );
        require(_totalTokens > 0, "Tokens must be greater than 0");
        presalePhases[uint256(phase)].totalTokens = _totalTokens;
    }

    function setSoldToken(
        PresalePhase phase,
        uint256 _totalSold
    ) external onlyOwner {
        require(
            phase == PresalePhase.Phase1 ||
                phase == PresalePhase.Phase2 ||
                phase == PresalePhase.Phase3,
            "Invalid phase"
        );
        presalePhases[uint256(phase)].totalSold = _totalSold;
    }
}