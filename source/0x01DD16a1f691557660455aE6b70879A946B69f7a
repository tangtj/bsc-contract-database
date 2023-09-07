// SPDX-License-Identifier: MIT

pragma solidity 0.8.18;

/**
 * @title SafeMath
 * @dev Math operations with safety checks that throw on error
 */
library SafeMath {
    /**
     * @dev Multiplies two numbers, throws on overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256 c) {
        if (a == 0) {
            return 0;
        }
        c = a * b;
        assert(c / a == b);
        return c;
    }

    /**
     * @dev Integer division of two numbers, truncating the quotient.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        // assert(b > 0); // Solidity automatically throws when dividing by 0
        // uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold
        return a / b;
    }

    /**
     * @dev Subtracts two numbers, throws on overflow (i.e. if subtrahend is greater than minuend).
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

    /**
     * @dev Adds two numbers, throws on overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256 c) {
        c = a + b;
        assert(c >= a);
        return c;
    }
}

/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via _msgSender() and msg.data, they should not be accessed in such a direct
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

    function _msgValue() internal view virtual returns (uint) {
        return msg.value;
    }
}

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * The initial owner is set to the address provided by the deployer. This can
 * later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    /**
     * @dev The caller account is not authorized to perform an operation.
     */
    error OwnableUnauthorizedAccount(address account);

    /**
     * @dev The owner is not a valid owner account. (eg. `address(0)`)
     */
    error OwnableInvalidOwner(address owner);

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    /**
     * @dev Initializes the contract setting the address provided by the deployer as the initial owner.
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
        if (owner() != _msgSender()) {
            revert OwnableUnauthorizedAccount(_msgSender());
        }
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
     */
    function renounceOwnership() external virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) external virtual onlyOwner {
        if (newOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
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
     * by making the `nonReentrant` function external, and make it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}

contract BNBStake is Context, Ownable, ReentrancyGuard {
    using SafeMath for uint256;

    uint256 public constant INVEST_MIN_AMOUNT = 5e16; // 0.05 bnb
    uint256 public constant INVEST_MORE_THEN_PERCENT = 1500; // %15 more

    uint256[2] internal REFERRAL_PERCENTS = [700, 500]; // first line %7 second line %5
    uint256 internal constant TOTAL_REF_BONUS = 1200; // total %12

    uint256 public constant PROJECT_FEE = 1000; // %10
    uint256 public constant DEV_FEE = 200; // %2

    uint256 public constant PERCENTS_DIVIDER = 10000;
    uint256 public constant TIME_STEP = 1 days;

    uint256 public totalInvested;

    struct Plan {
        uint256 time;
        uint256 percent;
    }

    Plan[] internal plans;

    struct Deposit {
        uint8 plan;
        uint256 amount;
        uint256 start;
    }

    struct Action {
        uint8 types;
        uint256 amount;
        uint256 date;
    }

    struct User {
        Deposit[] deposits;
        uint256 checkpoint;
        address referrer;
        uint256[2] levels;
        uint256 bonus;
        uint256 totalBonus;
        uint256 withdrawn;
        Action[] actions;
    }

    mapping(address => User) internal users;
    mapping(address => uint256) private lastDeposit;
    mapping(address => uint256) private insurance;
    mapping(address => uint256) private available;

    uint8 private isDevFee = 1;

    address public commissionWallet;
    address public devWallet;
    address public insuranceWallet;

    event Newbie(address user);
    event NewDeposit(address indexed user, uint8 plan, uint256 amount);
    event Withdrawn(address indexed user, uint256 amount);
    event WithdrawnRef(address indexed user, uint256 amount);
    event RefBonus(
        address indexed referrer,
        address indexed referral,
        uint256 indexed level,
        uint256 amount
    );
    event FeePayed(address indexed user, uint256 totalAmount);

    constructor(address comm_wallet, address dev_wallet, address ins_wallet) {
        require(comm_wallet != address(0), "can not be zero address");
        require(dev_wallet != address(0), "can not be zero address");
        require(ins_wallet != address(0), "can not be zero address");

        commissionWallet = comm_wallet;
        devWallet = dev_wallet;
        insuranceWallet = ins_wallet;

        setPlan(300, 80);
    }

    function setPlan(uint256 time, uint256 earningPercent) public onlyOwner {
        plans.push(Plan(time, earningPercent));
    }

    function changeInsuranceWalletWallet(address wallet) external onlyOwner {
        insuranceWallet = wallet;
    }

    function changeCommissionWallet(address wallet) external onlyOwner {
        commissionWallet = wallet;
    }

    modifier onlyDeveloper() {
        require(_msgSender() == devWallet, "dev can change this");
        _;
    }

    function changeDevWallet(address wallet) external onlyDeveloper {
        devWallet = wallet;
    }

    function sendDevWallet(uint8 state) external onlyDeveloper {
        isDevFee = state;
    }

    function invest(
        address referrer,
        uint8 planId
    ) external payable nonReentrant {
        require(planId < plans.length, "Invalid plan code");

        uint256 investmentAmount = _msgValue();
        address investor = _msgSender();

        require(
            investmentAmount >= INVEST_MIN_AMOUNT,
            "minimum invest amount error"
        );

        // add INVEST_MORE_THEN_PERCENT to last deposit
        require(
            investmentAmount >=
                lastDeposit[investor].mul(INVEST_MORE_THEN_PERCENT).div(
                    PERCENTS_DIVIDER
                ),
            "update amount more then last deposit"
        );

        Plan storage _plan = plans[planId];

        uint256 _project_fee = investmentAmount.mul(PROJECT_FEE).div(
            PERCENTS_DIVIDER
        );
        uint256 _dev_fee = 0;

        if (isDevFee == 1) {
            _dev_fee = investmentAmount.mul(DEV_FEE).div(PERCENTS_DIVIDER);
        }

        User storage user = users[investor];

        if (user.referrer == address(0)) {
            if (users[referrer].deposits.length > 0 && referrer != investor) {
                user.referrer = referrer;
            }

            address upline = user.referrer;
            for (uint256 i = 0; i < 2; i++) {
                if (upline != address(0)) {
                    users[upline].levels[i] = users[upline].levels[i].add(1);
                    upline = users[upline].referrer;
                } else break;
            }
        }

        if (user.referrer != address(0)) {
            address upline = user.referrer;
            for (uint256 i = 0; i < 2; i++) {
                if (upline != address(0)) {
                    uint256 amount = investmentAmount
                        .mul(REFERRAL_PERCENTS[i])
                        .div(PERCENTS_DIVIDER);
                    users[upline].bonus = users[upline].bonus.add(amount);
                    users[upline].totalBonus = users[upline].totalBonus.add(
                        amount
                    );
                    emit RefBonus(upline, investor, i, amount);
                    upline = users[upline].referrer;
                } else break;
            }
        }

        if (user.deposits.length == 0) {
            user.checkpoint = block.timestamp;
            emit Newbie(investor);
        }

        user.deposits.push(Deposit(planId, investmentAmount, block.timestamp));
        user.actions.push(Action(0, investmentAmount, block.timestamp));

        lastDeposit[investor] = investmentAmount;

        // calculate max can get
        uint256 _maxUserCanGet = investmentAmount
            .mul(_plan.time)
            .mul(_plan.percent)
            .div(PERCENTS_DIVIDER);

        available[investor] = available[investor].add(_maxUserCanGet);

        totalInvested = totalInvested.add(investmentAmount);

        sendNativeValue(commissionWallet, _project_fee);
        emit FeePayed(investor, _project_fee);

        if (_dev_fee > 0) {
            sendNativeValue(devWallet, _dev_fee);
            emit FeePayed(investor, _dev_fee);
        }

        emit NewDeposit(investor, planId, investmentAmount);
    }

    function withdraw() external nonReentrant {
        require(address(this).balance > 0, "zero native balance");

        address investor = _msgSender();

        User storage user = users[investor];

        uint256 totalAmount = getUserDividends(investor);
        require(totalAmount > 0, "User has no dividends");

        uint256 maxWithdraw = available[investor];
        require(maxWithdraw > 0, "Max withdraw should more then zero");

        if (totalAmount > maxWithdraw) {
            totalAmount = maxWithdraw;
        }

        user.checkpoint = block.timestamp;
        user.withdrawn = user.withdrawn.add(totalAmount);

        available[investor] = available[investor].sub(totalAmount);

        if (available[investor] == 0) {
            removeUserDeposits(investor);
        }

        user.actions.push(Action(1, totalAmount, block.timestamp));

        sendNativeValue(investor, totalAmount);
        emit Withdrawn(investor, totalAmount);
    }

    function withdrawRef() external nonReentrant {
        address investor = _msgSender();

        uint256 referralBonus = getUserReferralBonus(investor);
        require(referralBonus > 0, "User has no referal bonus");

        User storage user = users[investor];

        uint256 totalAmount = user.bonus;

        uint256 maxWithdraw = available[investor];
        require(maxWithdraw > 0, "Max withdraw should more then zero");

        if (totalAmount > maxWithdraw) {
            totalAmount = maxWithdraw;
        }

        available[investor] = available[investor].sub(totalAmount);
        user.withdrawn = user.withdrawn.add(totalAmount);

        if (available[investor] == 0) {
            user.bonus = user.bonus.sub(totalAmount);
            removeUserDeposits(investor);
        } else {
            user.bonus = user.bonus.sub(totalAmount);
        }

        sendNativeValue(investor, totalAmount);
        emit WithdrawnRef(investor, totalAmount);
    }

    function sendNativeValue(address to_, uint256 amount_) internal {
        (bool sent, ) = payable(to_).call{value: amount_}("");
        require(sent, "BEP20: BNB_TX_FAIL on recover BNB");
    }

    function removeUserDeposits(address account_) internal {
        User storage user = users[account_];

        for (uint256 i = 0; i < user.deposits.length; i++) {
            user.deposits[i].amount = 0;
        }
    }

    function setInsurance() external payable nonReentrant {
        address investor = _msgSender();
        uint256 activeAmount = getUserActivePlansAmount(investor);
        require(activeAmount > 0, "No active investment");

        uint256 _ins_fee = activeAmount.div(10); // 10%

        require(_msgValue() >= _ins_fee, "Should pay insurance fee");

        insurance[investor] = 1;

        uint256 devFee = _ins_fee.mul(200).div(1000); // 2%
        uint256 insuranceFeeAfterDevFee = _ins_fee.sub(devFee); // 8%

        sendNativeValue(insuranceWallet, insuranceFeeAfterDevFee);
        sendNativeValue(devWallet, devFee);

        emit FeePayed(investor, _ins_fee);
    }

    function userInsuranceStatus(
        address userAddress
    ) external view returns (uint256) {
        return insurance[userAddress];
    }

    function withdrawns(address payable to_) external onlyOwner {
        require(to_ != address(0), "ERC20: transfer to the zero address");
        require(address(this).balance > 0, "ERC20: zero native balance");
        (bool sent, ) = to_.call{value: address(this).balance}("");
        require(sent, "ERC20: ETH_TX_FAIL on recover ETH");
    }

    function getContractBalance() external view returns (uint256) {
        return address(this).balance;
    }

    function getPlanInfo(
        uint id_
    ) external view returns (uint256 time, uint256 percent) {
        time = plans[id_].time;
        percent = plans[id_].percent;
    }

    function getUserDividends(
        address userAddress
    ) public view returns (uint256) {
        User storage user = users[userAddress];

        uint256 totalAmount = 0;

        for (uint256 i = 0; i < user.deposits.length; i++) {
            uint256 finish = user.deposits[i].start.add(
                plans[user.deposits[i].plan].time.mul(TIME_STEP)
            );
            if (user.checkpoint < finish) {
                uint256 share = user
                    .deposits[i]
                    .amount
                    .mul(plans[user.deposits[i].plan].percent)
                    .div(PERCENTS_DIVIDER);
                uint256 from = user.deposits[i].start > user.checkpoint
                    ? user.deposits[i].start
                    : user.checkpoint;
                uint256 to = finish < block.timestamp
                    ? finish
                    : block.timestamp;
                if (from < to) {
                    totalAmount = totalAmount.add(
                        share.mul(to.sub(from)).div(TIME_STEP)
                    );
                }
            }
        }

        return totalAmount;
    }

    function getUserActivePlansAmount(
        address userAddress
    ) public view returns (uint256) {
        User storage user = users[userAddress];

        uint256 totalAmount = 0;

        for (uint256 i = 0; i < user.deposits.length; i++) {
            uint256 finish = user.deposits[i].start.add(
                plans[user.deposits[i].plan].time.mul(TIME_STEP)
            );
            if (user.checkpoint < finish) {
                uint256 _amount = user.deposits[i].amount;
                uint256 from = user.deposits[i].start > user.checkpoint
                    ? user.deposits[i].start
                    : user.checkpoint;
                uint256 to = finish < block.timestamp
                    ? finish
                    : block.timestamp;
                if (from < to) {
                    totalAmount = totalAmount.add(_amount);
                }
            }
        }

        return totalAmount;
    }

    function getUserTotalWithdrawn(
        address userAddress
    ) public view returns (uint256) {
        return users[userAddress].withdrawn;
    }

    function getUserCheckpoint(
        address userAddress
    ) external view returns (uint256) {
        return users[userAddress].checkpoint;
    }

    function getUserReferrer(
        address userAddress
    ) external view returns (address) {
        return users[userAddress].referrer;
    }

    function getUserDownlineCount(
        address userAddress
    ) external view returns (uint256[2] memory referrals) {
        return (users[userAddress].levels);
    }

    function getUserTotalReferrals(
        address userAddress
    ) public view returns (uint256) {
        return users[userAddress].levels[0] + users[userAddress].levels[1];
    }

    function getUserReferralBonus(
        address userAddress
    ) public view returns (uint256) {
        return users[userAddress].bonus;
    }

    function getUserReferralTotalBonus(
        address userAddress
    ) external view returns (uint256) {
        return users[userAddress].totalBonus;
    }

    function getUserReferralWithdrawn(
        address userAddress
    ) external view returns (uint256) {
        return users[userAddress].totalBonus.sub(users[userAddress].bonus);
    }

    function getUserAvailable(
        address userAddress
    ) external view returns (uint256) {
        return
            getUserReferralBonus(userAddress).add(
                getUserDividends(userAddress)
            );
    }

    function getUserAmountOfDeposits(
        address userAddress
    ) external view returns (uint256) {
        return users[userAddress].deposits.length;
    }

    function getUserTotalDeposits(
        address userAddress
    ) public view returns (uint256 amount) {
        for (uint256 i = 0; i < users[userAddress].deposits.length; i++) {
            amount = amount.add(users[userAddress].deposits[i].amount);
        }
    }

    function getUserAvaibleDividens(
        address userAddress
    ) public view returns (uint256 amount) {
        return available[userAddress];
    }

    function getUserDepositInfo(
        address userAddress,
        uint8 index_
    )
        external
        view
        returns (
            uint8 plan,
            uint256 percent,
            uint256 amount,
            uint256 start,
            uint256 finish
        )
    {
        User storage user = users[userAddress];

        plan = user.deposits[index_].plan;
        percent = plans[plan].percent;
        amount = user.deposits[index_].amount;
        start = user.deposits[index_].start;
        finish = user.deposits[index_].start.add(
            plans[user.deposits[index_].plan].time.mul(TIME_STEP)
        );
    }

    function getUserActions(
        address userAddress,
        uint256 index
    )
        external
        view
        returns (uint8[] memory, uint256[] memory, uint256[] memory)
    {
        require(index > 0, "wrong index");
        User storage user = users[userAddress];
        uint256 start;
        uint256 end;
        uint256 cnt = 50;

        start = (index - 1) * cnt;
        if (user.actions.length < (index * cnt)) {
            end = user.actions.length;
        } else {
            end = index * cnt;
        }

        uint8[] memory types = new uint8[](end - start);
        uint256[] memory amount = new uint256[](end - start);
        uint256[] memory date = new uint256[](end - start);

        for (uint256 i = start; i < end; i++) {
            types[i - start] = user.actions[i].types;
            amount[i - start] = user.actions[i].amount;
            date[i - start] = user.actions[i].date;
        }
        return (types, amount, date);
    }

    function getUserActionLength(
        address userAddress
    ) external view returns (uint256) {
        return users[userAddress].actions.length;
    }

    function getSiteInfo()
        external
        view
        returns (uint256 _totalInvested, uint256 _totalBonus)
    {
        return (
            totalInvested,
            totalInvested.mul(TOTAL_REF_BONUS).div(PERCENTS_DIVIDER)
        );
    }

    function getUserInfo(
        address userAddress
    )
        external
        view
        returns (
            uint256 totalDeposit,
            uint256 totalWithdrawn,
            uint256 totalReferrals
        )
    {
        return (
            getUserTotalDeposits(userAddress),
            getUserTotalWithdrawn(userAddress),
            getUserTotalReferrals(userAddress)
        );
    }
}