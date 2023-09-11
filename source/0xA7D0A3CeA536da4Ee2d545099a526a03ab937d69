// SPDX-License-Identifier: MIT
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

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol

// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

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
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

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

    function burnFrom(address account, uint256 amount) external;
    function burn(uint256 amount) external;
    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

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

// File: @openzeppelin/contracts/utils/Context.sol

// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)

pragma solidity ^0.8.0;

/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
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

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

// File: @openzeppelin/contracts/security/Pausable.sol

// OpenZeppelin Contracts (last updated v4.7.0) (security/Pausable.sol)

pragma solidity ^0.8.0;

/**
 * @dev Contract module which allows children to implement an emergency stop
 * mechanism that can be triggered by an authorized account.
 *
 * This module is used through inheritance. It will make available the
 * modifiers `whenNotPaused` and `whenPaused`, which can be applied to
 * the functions of your contract. Note that they will not be pausable by
 * simply including this module, only once the modifiers are put in place.
 */
abstract contract Pausable is Context {
    /**
     * @dev Emitted when the pause is triggered by `account`.
     */
    event Paused(address account);

    /**
     * @dev Emitted when the pause is lifted by `account`.
     */
    event Unpaused(address account);

    bool private _paused;

    /**
     * @dev Initializes the contract in unpaused state.
     */
    constructor() {
        _paused = false;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        _requireNotPaused();
        _;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is paused.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    modifier whenPaused() {
        _requirePaused();
        _;
    }

    /**
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Throws if the contract is paused.
     */
    function _requireNotPaused() internal view virtual {
        require(!paused(), "Pausable: paused");
    }

    /**
     * @dev Throws if the contract is not paused.
     */
    function _requirePaused() internal view virtual {
        require(paused(), "Pausable: not paused");
    }

    /**
     * @dev Triggers stopped state.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }

    /**
     * @dev Returns to normal state.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
    }
}

// File: @openzeppelin/contracts/access/Ownable.sol

// OpenZeppelin Contracts (last updated v4.9.0) (access/Ownable.sol)

pragma solidity ^0.8.0;

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner account will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

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
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
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

library DateTime {
    uint256 constant SECONDS_PER_DAY = 24 * 60 * 60;
    uint256 constant SECONDS_PER_HOUR = 60 * 60;
    uint256 constant SECONDS_PER_MINUTE = 60;
    int256 constant OFFSET19700101 = 2440588;

    uint256 constant DOW_MON = 1;
    uint256 constant DOW_TUE = 2;
    uint256 constant DOW_WED = 3;
    uint256 constant DOW_THU = 4;
    uint256 constant DOW_FRI = 5;
    uint256 constant DOW_SAT = 6;
    uint256 constant DOW_SUN = 7;

    // ------------------------------------------------------------------------
    // Calculate the number of days from 1970/01/01 to year/month/day using
    // the date conversion algorithm from
    //   http://aa.usno.navy.mil/faq/docs/JD_Formula.php
    // and subtracting the offset 2440588 so that 1970/01/01 is day 0
    //
    // days = day
    //      - 32075
    //      + 1461 * (year + 4800 + (month - 14) / 12) / 4
    //      + 367 * (month - 2 - (month - 14) / 12 * 12) / 12
    //      - 3 * ((year + 4900 + (month - 14) / 12) / 100) / 4
    //      - offset
    // ------------------------------------------------------------------------
    function _daysFromDate(
        uint256 year,
        uint256 month,
        uint256 day
    ) internal pure returns (uint256 _days) {
        require(year >= 1970);
        int256 _year = int256(year);
        int256 _month = int256(month);
        int256 _day = int256(day);

        int256 __days =
            _day -
                32075 +
                (1461 * (_year + 4800 + (_month - 14) / 12)) /
                4 +
                (367 * (_month - 2 - ((_month - 14) / 12) * 12)) /
                12 -
                (3 * ((_year + 4900 + (_month - 14) / 12) / 100)) /
                4 -
                OFFSET19700101;

        _days = uint256(__days);
    }

    // ------------------------------------------------------------------------
    // Calculate year/month/day from the number of days since 1970/01/01 using
    // the date conversion algorithm from
    //   http://aa.usno.navy.mil/faq/docs/JD_Formula.php
    // and adding the offset 2440588 so that 1970/01/01 is day 0
    //
    // int L = days + 68569 + offset
    // int N = 4 * L / 146097
    // L = L - (146097 * N + 3) / 4
    // year = 4000 * (L + 1) / 1461001
    // L = L - 1461 * year / 4 + 31
    // month = 80 * L / 2447
    // dd = L - 2447 * month / 80
    // L = month / 11
    // month = month + 2 - 12 * L
    // year = 100 * (N - 49) + year + L
    // ------------------------------------------------------------------------
    function _daysToDate(uint256 _days)
        internal
        pure
        returns (
            uint256 year,
            uint256 month,
            uint256 day
        )
    {
        int256 __days = int256(_days);

        int256 L = __days + 68569 + OFFSET19700101;
        int256 N = (4 * L) / 146097;
        L = L - (146097 * N + 3) / 4;
        int256 _year = (4000 * (L + 1)) / 1461001;
        L = L - (1461 * _year) / 4 + 31;
        int256 _month = (80 * L) / 2447;
        int256 _day = L - (2447 * _month) / 80;
        L = _month / 11;
        _month = _month + 2 - 12 * L;
        _year = 100 * (N - 49) + _year + L;

        year = uint256(_year);
        month = uint256(_month);
        day = uint256(_day);
    }

    function isLeapYear(uint256 timestamp)
        internal
        pure
        returns (bool leapYear)
    {
        (uint256 year, , ) = _daysToDate(timestamp / SECONDS_PER_DAY);
        leapYear = _isLeapYear(year);
    }

    function _isLeapYear(uint256 year) internal pure returns (bool leapYear) {
        leapYear = ((year % 4 == 0) && (year % 100 != 0)) || (year % 400 == 0);
    }

    function getDaysInMonth(uint256 timestamp)
        internal
        pure
        returns (uint256 daysInMonth)
    {
        (uint256 year, uint256 month, ) =
            _daysToDate(timestamp / SECONDS_PER_DAY);
        daysInMonth = _getDaysInMonth(year, month);
    }

    function _getDaysInMonth(uint256 year, uint256 month)
        internal
        pure
        returns (uint256 daysInMonth)
    {
        if (
            month == 1 ||
            month == 3 ||
            month == 5 ||
            month == 7 ||
            month == 8 ||
            month == 10 ||
            month == 12
        ) {
            daysInMonth = 31;
        } else if (month != 2) {
            daysInMonth = 30;
        } else {
            daysInMonth = _isLeapYear(year) ? 29 : 28;
        }
    }

    // 1 = Monday, 7 = Sunday
    function getDayOfWeek(uint256 timestamp)
        internal
        pure
        returns (uint256 dayOfWeek)
    {
        uint256 _days = timestamp / SECONDS_PER_DAY;
        dayOfWeek = ((_days + 3) % 7) + 1;
    }

    function getYear(uint256 timestamp) internal pure returns (uint256 year) {
        (year, , ) = _daysToDate(timestamp / SECONDS_PER_DAY);
    }

    function getMonth(uint256 timestamp) internal pure returns (uint256 month) {
        (, month, ) = _daysToDate(timestamp / SECONDS_PER_DAY);
    }

    function getDay(uint256 timestamp) internal pure returns (uint256 day) {
        (, , day) = _daysToDate(timestamp / SECONDS_PER_DAY);
    }

    function timestampFromDate(
        uint256 year,
        uint256 month,
        uint256 day
    ) internal pure returns (uint256 timestamp) {
        timestamp = _daysFromDate(year, month, day) * SECONDS_PER_DAY;
    }

    /**
     * @notice Gets the Friday of the same week
     * @param timestamp is the given date and time
     * @return the Friday of the same week in unix time
     */
    function getThisWeekFriday(uint256 timestamp)
        internal
        pure
        returns (uint256)
    {
        return timestamp + 5 days - getDayOfWeek(timestamp) * 1 days;
    }

    /**
     * @notice Gets the next friday after the given date and time
     * @param timestamp is the given date and time
     * @return the next friday after the given date and time
     */
    function getNextFriday(uint256 timestamp) internal pure returns (uint256) {
        uint256 friday = getThisWeekFriday(timestamp);
        return friday >= timestamp ? friday : friday + 1 weeks;
    }

    /**
     * @notice Gets the last day of the month
     * @param timestamp is the given date and time
     * @return the last day of the same month in unix time
     */
    function getLastDayOfMonth(uint256 timestamp)
        internal
        pure
        returns (uint256)
    {
        return
            timestampFromDate(getYear(timestamp), getMonth(timestamp) + 1, 1) -
            1 days;
    }

    /**
     * @notice Gets the last Friday of the month
     * @param timestamp is the given date and time
     * @return the last Friday of the same month in unix time
     */
    function getMonthLastFriday(uint256 timestamp)
        internal
        pure
        returns (uint256)
    {
        uint256 lastDay = getLastDayOfMonth(timestamp);
        uint256 friday = getThisWeekFriday(lastDay);

        return friday > lastDay ? friday - 1 weeks : friday;
    }

    /**
     * @notice Gets the last Friday of the quarter
     * @param timestamp is the given date and time
     * @return the last Friday of the quarter in unix time
     */
    function getQuarterLastFriday(uint256 timestamp)
        internal
        pure
        returns (uint256)
    {
        uint256 month = getMonth(timestamp);
        uint256 quarterMonth =
            (month <= 3) ? 3 : (month <= 6) ? 6 : (month <= 9) ? 9 : 12;

        uint256 quarterDate =
            timestampFromDate(getYear(timestamp), quarterMonth, 1);

        return getMonthLastFriday(quarterDate);
    }

    /**
     * @notice Gets the last Friday of the half-year
     * @param timestamp is the given date and time
     * @return the last friday of the half-year
     */
    function getBiannualLastFriday(uint256 timestamp)
        internal
        pure
        returns (uint256)
    {
        uint256 month = getMonth(timestamp);
        uint256 biannualMonth = (month <= 6) ? 6 : 12;

        uint256 biannualDate =
            timestampFromDate(getYear(timestamp), biannualMonth, 1);

        return getMonthLastFriday(biannualDate);
    }
}

// File: arma-stake.sol

pragma solidity ^0.8.18;

interface pancake {
    function getAmountsOut(uint256 amountIn, address[] memory path)
        external
        view
        returns (uint256[] memory amounts);
}


//time, transfer, liverate

contract aarmastaking is Ownable, Pausable, ReentrancyGuard {
    struct Package {
        uint256 number;
        uint256 months;
        uint256 min;
        uint256 max;
        uint256 bonus;
        uint256 percentage;
        uint256 roi;
        uint256 day;
        uint256 week;
    }
   
    struct RefferalRewardTxn {
        address from;
        address to;
        uint256 amount;
        uint256 reward;
        uint256 percentage;
        uint256 level;
        uint256 time;
    }
    struct BonusRewardTxn {
        address user;
        uint256 level;
        uint256 reward;
        uint256 member;
        uint256 teamBusiness;
        uint256 time;
    }
    struct Stakes {
        address user;
        uint256 uAmount; //  1000
        uint256 stakedArma; // 11640
        uint256 cAmount; // $405 roi + bonus
        uint256 rate;
        uint256 starttime;
        uint256 endtime;
        uint256 package;
        uint256 balance;
        uint256 _type;
    }
    struct Users {
        address refferal;
        address user;
        uint256 lastClaimedBonusLevel;
        uint256[9] userLevel;
        uint256[9] levelBusiness;
        uint256[9] bonusReward; // 0 1
        uint256[9] claimedBonusReward;
    }
    struct Totals {
        uint256 totalStake;
        uint256 totalStakeArma;
        uint256 totalRefReward;
        uint256 totalDailyReward;
        uint256 totalBonusReward;
        uint256 totalPending;
        uint256 totalWithdrawlArma; // 0
        uint256 totalWithdrawlArmaBusd; // 0
        uint256 totalWithdrawlBusd; // 1
        uint256 totalWithdrawlBusdArma; // 1
    }
    struct Withdrawl {
        address user;
        uint256 _type; //
        uint256 amount;
        uint256 totalToken;
        uint256 rate;
        uint256 time;
        uint256 fee;
    }
    BonusRewardTxn[] public bonusRewardTxn; // useraddress
    Withdrawl[] public withdrawl; // useraddress
    mapping(address => BonusRewardTxn) public bonusRewardTxnMap;
    uint256[3] public levelReward = [5e18, 25e17, 125e16];
    // mapping(address => Incomes) public incomes;
    // mapping(address => Rewards) public rewards;
    uint256 constant oneday = 86400;
    uint256 public constant MONTH = oneday * 30;
    uint256 constant SECONDS_PER_HOUR = 60 * 60;
    uint256 public minArmaWithrawl = 10e18;

    Package[] public packages;
    mapping(address => Stakes) public stakes;
    mapping(address => Users) public users;
    mapping(address => Totals) public totals;
    Stakes[] public stakesList;
    // uint256 public armaPrice() = 125e15;
    uint256 public stakeLenght;
    address[] public stakers;
    RefferalRewardTxn[] public refferalRewardTxn; // user to
    uint256[2] public bonusRewardMembers = [3, 9];
    address public defaultAddress = 0x066109250F3151f7D9d60a7Cc51EFa886697c765;
    address AUSD = 0x7e48A0eb32CDcE54794aFe505Bf0AF9A7b83Ef1E;
    address ARMA = 0xf1F0FA0287C47804636fFeF14e2C241f2587903e;
    address BUSD = 0x55d398326f99059fF775485246999027B3197955;
    address public routerAddress = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    IERC20 public ausd = IERC20(AUSD);
    IERC20 public arma = IERC20(ARMA);
    IERC20 public busd = IERC20(BUSD);
    uint256 public withdrawlFee = 2e18;
    uint256[9] public bonusRewawrdBusiness = [
        1500e18,
        5000e18,
        15000e18,
        50000e18,
        175000e18,
        500000e18,
        1500000e18,
        5000000e18,
        15000000e18
    ];
    uint256[9] public bonusRewawrdBusinessBack = [
        1500e18,
        6500e18,
        21500e18,
        71500e18,
        246500e18,
        746500e18,
        2246500e18,
        7246500e18,
        22246500e18
    ];
    uint256[9] public bonusReward = [
        75e18,
        200e18,
        550e18,
        750e18,
        6125e18,
        15000e18,
        40000e18,
        100000e18,
        300000e18
    ];

    mapping(address => bool) public blocked;
    bool public withdrawlPaused;
    bool public bonusPaused;
    mapping(address => bool) public pauseUserWithdrawl;
    mapping(address => bool) public pauseUserBonus;
    mapping(address => bool) public pauseUserStaking;
    uint256[3] public ausdPackage = [500e18, 15e18, 5e18];
    uint256 public totalSupplyLimit = 21150000e18;
    uint256 public allstakingarma;
    uint256 public allstakingausd;
    uint256 public allburning;
    uint256 public allwithdrawalarma;
    uint256 public allwithdrawalbusd;

    function changetoken(address _arma, address _ausd, address _busd) public onlyOwner{
        ARMA = _arma;
        AUSD = _ausd;
        BUSD = _busd;
    }

    function armaPrice() public view returns (uint256) {
        address[] memory path = new address[](2);
        path[0] = ARMA;
        path[1] = BUSD;
        uint256[] memory price = pancake(routerAddress).getAmountsOut(
            1e18,
            path
        );
        return price[1];
    }

    constructor() {
        packages.push(
            Package({
                number: 1,
                months: 27,
                min: 100e18,
                max: 2599e18,
                bonus: 5e18,
                percentage: 15e17,
                roi: 4050e16,
                day: 810,
                week: 108
            })
        );
        packages.push(
            Package({
                number: 2,
                months: 36,
                min: 2600e18,
                max: 5099e18,
                bonus: 10e18,
                percentage: 175e16,
                roi: 63e18,
                day: 1080,
                week: 144
            })
        );
        packages.push(
            Package({
                number: 3,
                months: 45,
                min: 5100e18,
                max: 10999e18,
                bonus: 15e18,
                percentage: 2e18,
                roi: 90e18,
                day: 1350,
                week: 180
            })
        );
        packages.push(
            Package({
                number: 4,
                months: 54,
                min: 11000e18,
                max: 25999e18,
                bonus: 20e18,
                percentage: 225e16,
                roi: 1215e17,
                day: 1620,
                week: 216
            })
        );
        packages.push(
            Package({
                number: 5,
                months: 63,
                min: 26000e18,
                max: 50999e18,
                bonus: 25e18,
                percentage: 25e17,
                roi: 1575e17,
                day: 1890,
                week: 250
            })
        );
        packages.push(
            Package({
                number: 6,
                months: 72,
                min: 51000e18,
                max: arma.totalSupply(),
                // max: 10000000e18,
                bonus: 30e18,
                percentage: 3e18,
                roi: 216e18,
                day: 2160,
                week: 288
            })
        );
    }

    function pause() public onlyOwner {
        _pause();
    }

    function unpause() public onlyOwner {
        _unpause();
    }

    function blockUser(address user) public onlyOwner {
        blocked[user] = true;
    }

    function unBlockUser(address user) public onlyOwner {
         blocked[user] = false;
    }

    function changeWithdrawlStatus() public onlyOwner {
        withdrawlPaused = withdrawlPaused ? false : true;
    }

    function changeBonusStatus() public onlyOwner {
        bonusPaused = bonusPaused ? false : true;
    }

    function changePauseUserWithdrawl(address user) public onlyOwner {
        pauseUserWithdrawl[user] = pauseUserWithdrawl[user] ? false : true;
    }

    function changePauseUserBonus(address user) public onlyOwner {
        pauseUserBonus[user] = pauseUserBonus[user] ? false : true;
    }

    function changePauseUserStaking(address user) public onlyOwner {
        pauseUserStaking[user] = pauseUserStaking[user] ? false : true;
    }

    modifier isBlocked() {
        require(blocked[msg.sender] == false, "miner is blocked !");
        _;
    }

    modifier WithdrawlStatus() {
        require(withdrawlPaused == false, "withdrawl is paused !");
        _;
    }
    
    modifier BonusStatus() {
        require(bonusPaused == false, "bonus is paused !");
        _;
    }
    
    modifier PauseUserWithdrawlStatus() {
        require(
            pauseUserWithdrawl[msg.sender] == false,
            "this user's withdrawl is paused !"
        );
        _;
    }
    
    modifier PauseUserBonusStatus() {
        require(
            pauseUserBonus[msg.sender] == false,
            "this user's bonus is paused !"
        );
        _;
    }

    modifier PauseUserStakingStatus() {
        require(
            pauseUserStaking[msg.sender] == false,
            "this user's staking is paused !"
        );
        _;
    }

    function changeWithdrawFee(uint256 value) public onlyOwner {
        withdrawlFee = value;
    }

    function gettime(uint256 time) public pure returns (uint256) {
        uint256 hour = ((time / 60 / 60) % 24);
        uint256 minute = (time / 60) % 60;
        uint256 sec = time % 60;
        uint256 minutensec = (minute * 60) + sec;
        uint256 diffhour = (hour) * SECONDS_PER_HOUR;
        uint256 difftime = diffhour + minutensec;
        uint256 finaltime = (time - difftime);
        return finaltime;
    }

    function findPackageNumber(uint256 _amt) public view returns (uint256) {
        require(_amt > 0, "Amount is not ZERO.");
        uint256 pkg = 0;
        for (uint256 i = 0; i < packages.length; i++) {
            if (_amt >= packages[i].min && _amt <= packages[i].max) {
                pkg = i;
            }
        }
        return pkg;
    }

    //0 arma 1 ausd
    event Stake(address user, uint256 busd, uint256 _type, uint256 _arma);

    function stake(
        uint256 _busd,
        address _refferal,
        uint256 _type,
        address user,
        uint starttime,
        uint256 rate
    ) public whenNotPaused isBlocked PauseUserStakingStatus{
        // address user = msg.sender;
        uint256 armaRate = 0;
        uint256 timestamp = 0;
        if (msg.sender == owner()){
            armaRate = rate;
            timestamp = starttime;
        } else {
            armaRate = armaPrice();
            timestamp = block.timestamp;
        }
        require(
            stakes[user].endtime < timestamp,
            "user already staked !"
        );
        uint256 _arma = ((_busd * 1e18) / armaRate);
        if (_type == 0) {
            require(_arma >= 100e18, "minimum amount is 100 !");
            require(user != _refferal, "refferal and user address same !");

            require(
                checkUserStatus(_refferal) == 1,
                "refferal user is not active !"
            );
            if (stakes[user].user == address(0)) {
                stakers.push(user);
            }
            uint256 package = findPackageNumber(_busd);
            (
                ,
                ,
                ,
                uint256 _roi,
                uint256 _bonus,
                uint256 totalIncome
            ) = findPackage(_busd,armaRate);
            if (msg.sender == owner()){
                totalIncome = (((_roi + _bonus + _busd)*1e18)/armaRate);
            }
            stakes[user].user = user;
            users[user].user = user;
            stakes[user].uAmount = _busd;
            users[user].refferal = _refferal;
            stakes[user].starttime = timestamp;
            stakes[user].endtime =
                timestamp +
                (packages[package].months * MONTH);
            stakes[user].package = packages[package].number;
            stakes[user].stakedArma = totalIncome;
            stakes[user].cAmount = (_roi + _bonus);
            stakes[user].rate = armaRate;
            totals[user].totalStake += (_busd + (_roi + _bonus));
            totals[user].totalStakeArma += totalIncome;
            stakes[user]._type = 0;
            stakesList.push(
                Stakes({
                    user: user,
                    uAmount: _busd,
                    stakedArma: totalIncome,
                    cAmount: (_roi + _bonus),
                    rate: armaRate,
                    starttime: timestamp,
                    endtime: timestamp +
                        (packages[package].months * MONTH),
                    package: package,
                    balance: 0,
                    _type: 0
                })
            );
            allstakingarma += totalIncome;
            if (msg.sender != owner()){
                arma.transferFrom(user, address(this), _arma);
            }
            stakeLenght++;
            distLevelReward(user, _busd);
        }
        if (_type == 1) {
            uint256 ausdBalance = 0;
            if (msg.sender == owner()) {
                ausdBalance = _busd;
            } else {
                ausdBalance = ausd.balanceOf(user);
            }
            require(
                ausdBalance > ausdPackage[0],
                "minimum is 500 AUSD !"
            );

            (, uint256 _roi, uint256 _bonus) = calcAusdStaking(ausdBalance);
            stakes[user].user = user;
            users[user].user = user;
            stakes[user].uAmount = ausdBalance;
            users[user].refferal = _refferal;
            stakes[user].starttime = timestamp;
            stakes[user].endtime = timestamp + (30 * MONTH);
            stakes[user].package = 0;
            stakes[user].stakedArma = 0;
            stakes[user].cAmount = (_roi + _bonus);
            totals[user].totalStake += (ausdBalance + (_roi + _bonus));
            totals[user].totalStakeArma = 0;
            stakes[user]._type = 1;
            stakesList.push(
                Stakes({
                    user: user,
                    uAmount: ausdBalance,
                    stakedArma: 0,
                    cAmount: (_roi + _bonus),
                    rate: armaRate,
                    starttime: timestamp,
                    endtime: timestamp + (30 * MONTH),
                    package: 0,
                    balance: 0,
                    _type: 1
                })
            );
            allstakingausd +=ausdBalance;
            if (msg.sender != owner()){
                ausd.transferFrom(user, address(this), ausdBalance);
            }
        }
        emit Stake(user, _busd, _type, _arma);
    }
    
    function calcAusdStaking(uint256 value)
        public
        view
        returns (
            uint256 amount,
            uint256 roi,
            uint256 bonus
        )
    {
        amount = value;
        roi = (value * ausdPackage[1]) / 100e18;
        bonus = (value * ausdPackage[2]) / 100e18;
    }

    function checkUserStatus(address user) public view returns (uint256) {
        if (user == defaultAddress) {
            return (1);
        } else {
            if (
                (stakes[user].endtime > block.timestamp &&
                    stakes[user].endtime != 0)
            ) {
                return (1);
            } else {
                return (0);
            }
        }
    }

    function checkausduserstatus(address _user) public view returns (uint256) {
        require(stakes[_user]._type == 1, "Staking is not AUSD");
        uint256 maxreward = totals[_user].totalStake;
        return maxreward;
    }

    function getUserLevel(address user)
        public
        view
        returns (uint256[9] memory)
    {
        return users[user].userLevel;
    }

    function getLevelBusiness(address user)
        public
        view
        returns (uint256[9] memory)
    {
        return users[user].levelBusiness;
    }

    function getBonusReward(address user)
        public
        view
        returns (uint256[9] memory)
    {
        return users[user].bonusReward;
    }

    function getClaimedBonusReward(address user)
        public
        view
        returns (uint256[9] memory)
    {
        return users[user].claimedBonusReward;
    }

    event LevelReward(address user, uint256 value, uint256 time);
    function distLevelReward(address user, uint256 value)
        internal
        returns (address[] memory)
    {
        address[] memory uplines = new address[](9);
        address refferal = users[user].refferal;
        uint256 length;
        if (stakers.length >= levelReward.length) {
            length = levelReward.length;
        } else {
            length = stakers.length;
        }
        uint256 loopLength;
        if (stakers.length >= bonusReward.length) {
            loopLength = bonusReward.length;
        } else {
            loopLength = stakers.length;
        }
        for (uint256 i = 0; i < loopLength; i++) {
            uplines[i] = refferal;
            if (refferal != address(0)) {
                if(checkUserStatus(user) == 1)
                {
                    // break;
                    if (i < length) {
                        refferalRewardTxn.push(
                            RefferalRewardTxn({
                                from: user,
                                to: refferal,
                                amount: value,
                                reward: (value * levelReward[i]) / 100e18,
                                percentage: levelReward[i],
                                level: i + 1,
                                time: block.timestamp
                            })
                        );
                    totals[refferal].totalRefReward +=
                        (value * levelReward[i]) /
                        100e18;
                    }
                    users[refferal].userLevel[i] += 1;
                    users[refferal].levelBusiness[i] += value;
                }
                distBonusReward(refferal);
                refferal = users[refferal].refferal;
            }
        }
        emit LevelReward(user, value, block.timestamp);
        return uplines;
    }

    function updateUserLevel(
        uint256 leve,
        uint256 value,
        address user
    ) public onlyOwner {
        users[user].userLevel[leve] = value;
    }

    function updateBonusReward(
        uint256 leve,
        uint256 value,
        address user
    ) public onlyOwner {
        users[user].bonusReward[leve] = value;
    }

    function updateBusiness(
        address user,
        uint256 value,
        uint256 i
    ) public onlyOwner {
        users[user].levelBusiness[i] = value;
    }

    function updateMap(address user, uint256 level) public onlyOwner {
        bonusRewardTxnMap[user].user = user;
        bonusRewardTxnMap[user].level = level;
    }

    function distBonusReward(address _refferal) internal {
        uint256 i = bonusRewardTxnMap[_refferal].level;
        if (i == 0) {
            if (
                bonusRewawrdBusinessBack[i] <=
                users[_refferal].levelBusiness[i] &&
                users[_refferal].bonusReward[i] == 0
            ) {
                if (i < bonusRewardMembers.length) {
                    if (
                        users[_refferal].userLevel[i] >= bonusRewardMembers[i]
                    ) {
                        sendBonusReward(_refferal, i);
                    }
                } else {
                    sendBonusReward(_refferal, i);
                }
            }
        }
       
        else  {
            if (i < bonusRewardMembers.length) {
                if (
                    users[_refferal].levelBusiness[i] >=
                    bonusRewawrdBusinessBack[i] &&
                    (users[_refferal].userLevel[i] >= bonusRewardMembers[i])
                ) {
                    sendBonusReward(_refferal, i);
                }
            } else {
                if (
                    users[_refferal].levelBusiness[i] >=
                    bonusRewawrdBusinessBack[i]
                ) {
                    sendBonusReward(_refferal, i);
                }
            }
        }
    }

    event BonusReward(address user, uint256 reward, uint256 time);

    function sendBonusReward(address _refferal, uint256 i) internal {
        bonusRewardTxn.push(
            BonusRewardTxn({
                user: _refferal,
                level: i + 1,
                reward: bonusReward[i],
                member: users[_refferal].userLevel[i],
                teamBusiness: users[_refferal].levelBusiness[i],
                time: block.timestamp
            })
        );
        bonusRewardTxnMap[_refferal].user = _refferal;
        bonusRewardTxnMap[_refferal].level = i + 1;
        bonusRewardTxnMap[_refferal].reward = bonusReward[i];
        bonusRewardTxnMap[_refferal].member = users[_refferal].userLevel[i];
        bonusRewardTxnMap[_refferal].teamBusiness = users[_refferal]
            .levelBusiness[i];
        bonusRewardTxnMap[_refferal].time = block.timestamp;
        users[_refferal].bonusReward[i] = 1;
        totals[_refferal].totalPending += bonusReward[i];
        if(i > 1 && users[_refferal].bonusReward[i] == 0){
            users[_refferal].bonusReward[i-1] = 2;
        }
        emit BonusReward(_refferal, bonusReward[i], block.timestamp);
    }

    function monthlyReward(address _address)
        public
        view
        returns (uint256, uint256)
    {
        uint256 currentTime = block.timestamp;
        uint256 totalDays = (currentTime - stakes[_address].starttime) /
            60 /
            60 /
            24;
        uint256 months = totalDays / 30;
        (uint256 dailyIncome, , , , , ) = findPackage(stakes[_address].uAmount,stakes[_address].rate);
        uint256 monthly = months * dailyIncome * 30;
        uint256 currentReward = (totalDays * dailyIncome);
        return (monthly, currentReward);
    }

    function monthlyAUSDReward(address _address)
        public
        view
        returns (uint256, uint256)
    {
        // uint256 currentTime = time; //for testing
        uint256 currentTime = block.timestamp;
        uint256 totalDays = (currentTime - stakes[_address].starttime) /
            60 /
            60 /
            24;
        uint256 months = 30;
        (uint256 amount, uint256 roi, uint256 bonus) = calcAusdStaking(
            stakes[_address].uAmount
        );
        uint256 dailyIncome = (amount + roi + bonus) / (900);
        uint256 monthly = months * dailyIncome * 30;
        uint256 currentReward = (totalDays * dailyIncome);
        return (monthly, currentReward);
    }

    function findPackage(uint256 _busd,uint256 rate)
        public
        view
        returns (
            uint256,
            uint256,
            uint256,
            uint256,
            uint256,
            uint256
        )
    {
        uint256 dailyIncome = 0;
        uint256 monthIncome = 0;
        uint256 package = findPackageNumber(_busd);
        uint256 _roi = (_busd * packages[package].roi) / 100e18;
        uint256 _bonus = (_busd * packages[package].bonus) / 100e18;
        uint256 totalIncome = _busd + _roi + _bonus;
        uint256 armaa = ((totalIncome * 1e18) / rate);
        dailyIncome = armaa / packages[package].day;
        monthIncome = armaa / packages[package].months;
        return (
            dailyIncome,
            monthIncome,
            armaa / packages[package].week,
            _roi,
            _bonus,
            armaa
        );
    }
    event ClaimWithdrawl(
        address user,
        uint256 _tokenType,
        uint256 value,
        uint256 time
    );

    function claimWithdrawlOwner(uint256 _tokenType, uint256 value,address user, uint256 rate)
        public
       onlyOwner
    {
        uint time = block.timestamp;
        // 1 == arma , 2 = busd
        // at Withdrawal user must active
        // Withdrawal
        require(_tokenType == 0 || _tokenType == 1, "token type should be 0 or 1");
        // (uint256 remaining,,) = getTotalDailyRemainingWithrawls(user);
        uint256 fee = 0;
        uint256 totalArma = 0;
        if (_tokenType == 0) {
            fee = (value * withdrawlFee) / 100e18;
            // if (arma.totalSupply() > totalSupplyLimit) {
            //     arma.burn(fee);
            //     allburning += fee;
            // }
            if (stakes[user]._type == 0) {
                require(
                    remainingWithdrawl(user, _tokenType) >= value,
                    "invalid value type 0"
                );
                // require(remaining >= ((value * armaPrice()()) / 1e18),"your daily limit exceeds!");
                totalArma = value;
                totals[user].totalWithdrawlArma += value;
                totals[user].totalWithdrawlArmaBusd += ((value * rate) /
                    1e18);
                // arma.transfer(user, (value - fee));
                allwithdrawalbusd += ((value * rate) /
                    1e18);
                allwithdrawalarma += value;
            }
            if (stakes[user]._type == 1) {
                uint256 busdValue = (value * rate) / 1e18;
                (, uint256 currentReward) = monthlyAUSDReward(user);
                require(value >= minArmaWithrawl,"minimum arma is morethan 10!");
                require(
                    value <= (currentReward - totals[user].totalDailyReward),
                    "you enetred value more than your current reward !"
                );
                totalArma = value;
                uint256 maxBalance = checkausduserstatus(user);
                require(busdValue <= maxBalance, " enter valid amount");
                require(
                    stakes[user].balance <= maxBalance,
                    "might be you reached your max limit !"
                );
                
                stakes[user].balance += busdValue;
                // arma.transfer(user, (value - fee));
                allwithdrawalarma += value;
                totals[user].totalDailyReward += value;
            }
            
        }
        if (_tokenType == 1) {
            totalArma = ((value * 1e18) / rate);
            fee = (totalArma * withdrawlFee) / 100e18;
            // require(remaining >= value,"your daily limit exceeds!");
            // if (arma.totalSupply() > totalSupplyLimit) {
            //     arma.burn(fee);
            //     allburning += fee;
            // }
            require(
                value <= remainingWithdrawl(user, _tokenType),
                "invalid value type 1"
            );
            totals[user].totalWithdrawlBusd += value;
            totals[user].totalWithdrawlBusdArma += ((value * 1e18) / rate);
            // arma.transfer(user, (totalArma - fee));
            allwithdrawalarma += totalArma;
        }
        withdrawl.push(
            Withdrawl({
                user: user,
                _type: _tokenType,
                amount: value,
                totalToken:totalArma,
                rate: rate,
                time: time,
                fee: fee
            })
        );
    }

    function claimWithdrawl(uint256 _tokenType, uint256 value)
        public
        WithdrawlStatus
        PauseUserWithdrawlStatus
        isBlocked
    {
        // uint time = block.timestamp;
        // 1 == arma , 2 = busd
        // at Withdrawal user must active
        // Withdrawal
        require(_tokenType == 0 || _tokenType == 1, "token type should be 0 or 1");
        address user = msg.sender;
        (uint256 remaining,,) = getTotalDailyRemainingWithrawls(user);
        uint256 fee = 0;
        uint256 totalArma = 0;
        if (_tokenType == 0) {
            fee = (value * withdrawlFee) / 100e18;
            if (arma.totalSupply() > totalSupplyLimit) {
                arma.burn(fee);
                allburning += fee;
            }
            if (stakes[user]._type == 0) {
                require(
                    remainingWithdrawl(user, _tokenType) >= value,
                    "invalid value type 0"
                );
                require(remaining >= ((value * armaPrice()) / 1e18),"your daily limit exceeds!");
                totalArma = value;
                totals[user].totalWithdrawlArma += value;
                totals[user].totalWithdrawlArmaBusd += ((value * armaPrice()) /
                    1e18);
                arma.transfer(user, (value - fee));
                allwithdrawalbusd += ((value * armaPrice()) /
                    1e18);
                allwithdrawalarma += value;
            }
            if (stakes[user]._type == 1) {
                uint256 busdValue = (value * armaPrice()) / 1e18;
                (, uint256 currentReward) = monthlyAUSDReward(user);
                require(value >= minArmaWithrawl,"minimum arma is morethan 10!");
                require(
                    value <= (currentReward - totals[user].totalDailyReward),
                    "you enetred value more than your current reward !"
                );
                totalArma = value;
                uint256 maxBalance = checkausduserstatus(user);
                require(busdValue <= maxBalance, " enter valid amount");
                require(
                    stakes[user].balance <= maxBalance,
                    "might be you reached your max limit !"
                );
                
                stakes[user].balance += busdValue;
                arma.transfer(user, (value - fee));
                allwithdrawalarma += value;
                totals[user].totalDailyReward += value;
            }
            
        }
        if (_tokenType == 1) {
            totalArma = ((value * 1e18) / armaPrice());
            fee = (totalArma * withdrawlFee) / 100e18;
            require(
                value <= remainingWithdrawl(user, _tokenType),
                "invalid value type 1"
            );
            require(remaining >= value,"your daily limit exceeds!");
            if (arma.totalSupply() > totalSupplyLimit) {
                arma.burn(fee);
                allburning += fee;
            }
            totals[user].totalWithdrawlBusd += value;
            totals[user].totalWithdrawlBusdArma += ((value * 1e18) / armaPrice());
            arma.transfer(user, (totalArma - fee));
            allwithdrawalarma += totalArma;
        }
        withdrawl.push(
            Withdrawl({
                user: user,
                _type: _tokenType,
                amount: value,
                totalToken:totalArma,
                rate: armaPrice(),
                time: block.timestamp,
                fee: fee
            })
        );
        emit ClaimWithdrawl(user, _tokenType, value, block.timestamp);
    }

    function remainingWithdrawl(address user, uint256 _type)
        public
        view
        returns (uint256)
    {
        if (_type == 0) {
            (,uint256 daily ) = monthlyReward(user);
            return (daily - totals[user].totalWithdrawlArma);
        } else {
            uint256 reward = (totals[user].totalRefReward +
                totals[user].totalBonusReward) -
                totals[user].totalWithdrawlBusd;
            return reward;
        }
    }

    event ClaimBonusReward(address user, uint256 value, uint256 time);

        function claimBonusReward(uint256 level)
        public
        BonusStatus
        PauseUserBonusStatus
        isBlocked
    {
        address user = msg.sender;
        uint256 total = 0;
        require(
            users[user].bonusReward[level] == 1,
            "this level reward not active yet !"
        );
        distBonusReward(user);
        if (level == 0) {
            total += bonusReward[level];
            users[user].claimedBonusReward[level] = 1;
        } else if (users[user].claimedBonusReward[0] == 1){
            for (uint256 i = level; i > 0; i--) {
                if (users[user].bonusReward[level] == 1) {
                    uint256 bonusRewardRequire = 0;
                    uint256 bonusRewardUser = 0;
                    for (uint256 j = 0; j <= level; j++) {
                        bonusRewardRequire += bonusRewawrdBusiness[j];
                        bonusRewardUser += users[user].levelBusiness[j];
                    }
                    require(
                        bonusRewardUser >= bonusRewardRequire,
                        "bonus reward is not less than total of all level reward !"
                    );
                    total += bonusReward[level];
                    users[user].claimedBonusReward[level] = 1;
                }
                if (level != i) {
                    users[user].bonusReward[i] = 2;
                    users[user].claimedBonusReward[i] = 2;
                }
            }
        } else {
            revert("level is not open!");
        }

        totals[user].totalBonusReward += total;
        emit ClaimBonusReward(user, total, block.timestamp);
    }

    function getBonusRewardTxn(address _address)
        public
        view
        returns (BonusRewardTxn[] memory)
    {
        BonusRewardTxn[] memory txn = new BonusRewardTxn[](
            bonusRewardTxn.length
        );
        for (uint256 i = 0; i < bonusRewardTxn.length; i++) {
            BonusRewardTxn storage user = bonusRewardTxn[i];
            if (user.user == _address) {
                txn[i] = user;
            }
        }
        return txn;
    }

    function getWithdrawlTxn(address _address)
        public
        view
        returns (Withdrawl[] memory)
    {
        Withdrawl[] memory txn = new Withdrawl[](withdrawl.length);
        for (uint256 i = 0; i < withdrawl.length; i++) {
            Withdrawl storage user = withdrawl[i];
            if (user.user == _address) {
                txn[i] = user;
            }
        }
        return txn;
    }

    function getRefferalRewardTxn(address _address)
        public
        view
        returns (RefferalRewardTxn[] memory)
    {
        RefferalRewardTxn[] memory txn = new RefferalRewardTxn[](
            refferalRewardTxn.length
        );
        for (uint256 i = 0; i < refferalRewardTxn.length; i++) {
            RefferalRewardTxn storage user = refferalRewardTxn[i];
            if (user.to == _address) {
                txn[i] = user;
            }
        }
        return txn;
    }

    function getStakings(address _address)
        public
        view
        returns (Stakes[] memory)
    {
        Stakes[] memory txn = new Stakes[](stakesList.length);
        for (uint256 i = 0; i < stakesList.length; i++) {
            Stakes storage user = stakesList[i];
            if (user.user == _address) {
                txn[i] = user;
            }
        }
        return txn;
    }

    function withdrawlToken(address _address, uint256 value) public onlyOwner {
        IERC20 token = IERC20(_address);
        token.transfer(msg.sender, value);
    }

    function withdrawlCoin(uint256 value) public onlyOwner {
        (bool sent, ) = (msg.sender).call{value: value}("");
        require(sent, "Failed to send BNB");
    }

    function getTotalDailyRemainingWithrawls(address user) public view returns(uint256 remaining,uint staking,uint total) {
       
        (uint256 year,uint256 month ,uint256 day ) = DateTime._daysToDate(block.timestamp / DateTime.SECONDS_PER_DAY);
        uint startTime = (DateTime.timestampFromDate(year,month,day));
        uint endTime = startTime + 1 days;
        // Withdrawl[] memory txn = new Withdrawl[](withdrawl.length);
        uint256 value = 0;
        for (uint256 i = 0; i < withdrawl.length; i++) {
            Withdrawl storage withs = withdrawl[i];
            if (withs.user == user && startTime < withs.time && endTime > withs.time) {
                value += ((withs.totalToken * withs.rate) / 1e18);
            }
        }
        total = value;
        staking = stakes[user].uAmount;
        remaining = stakes[user].uAmount - value > 0 ? stakes[user].uAmount - value : 0;
    }
}