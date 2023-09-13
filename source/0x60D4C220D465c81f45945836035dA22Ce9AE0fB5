// SPDX-License-Identifier: MIT
pragma solidity ^0.8.1;

interface IERC20 {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

interface ISwapFactory {
    function getPair(
        address tokenA,
        address tokenB
    ) external view returns (address pair);

    function allPairs(uint256) external view returns (address pair);

    function allPairsLength() external view returns (uint256);

    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);
}

interface ISwapPair {
    function DOMAIN_SEPARATOR() external view returns (bytes32);

    function PERMIT_TYPEHASH() external pure returns (bytes32);

    function nonces(address owner) external view returns (uint256);

    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;

    function MINIMUM_LIQUIDITY() external pure returns (uint256);

    function factory() external view returns (address);

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves()
        external
        view
        returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function price0CumulativeLast() external view returns (uint256);

    function price1CumulativeLast() external view returns (uint256);

    function kLast() external view returns (uint256);

    function mint(address to) external returns (uint256 liquidity);

    function burn(
        address to
    ) external returns (uint256 amount0, uint256 amount1);

    function swap(
        uint256 amount0Out,
        uint256 amount1Out,
        address to,
        bytes calldata data
    ) external;

    function skim(address to) external;

    function sync() external;

    function initialize(address, address) external;
}

interface ISwapRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB, uint256 liquidity);

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);

    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB);

    function removeLiquidityETH(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountToken, uint256 amountETH);

    function swapExactTokensForTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapTokensForExactTokens(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactETHForTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);

    function swapTokensForExactETH(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactTokensForETH(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapETHForExactTokens(
        uint256 amountOut,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);

    function quote(
        uint256 amountA,
        uint256 reserveA,
        uint256 reserveB
    ) external pure returns (uint256 amountB);

    function getAmountOut(
        uint256 amountIn,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountOut);

    function getAmountIn(
        uint256 amountOut,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountIn);

    function getAmountsOut(
        uint256 amountIn,
        address[] calldata path
    ) external view returns (uint256[] memory amounts);

    function getAmountsIn(
        uint256 amountOut,
        address[] calldata path
    ) external view returns (uint256[] memory amounts);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

interface IERC165 {
    function supportsInterface(bytes4 interfaceId) external view returns (bool);
}

interface IERC721 is IERC165 {
    event Transfer(
        address indexed from,
        address indexed to,
        uint256 indexed tokenId
    );
    event Approval(
        address indexed owner,
        address indexed approved,
        uint256 indexed tokenId
    );
    event ApprovalForAll(
        address indexed owner,
        address indexed operator,
        bool approved
    );

    function balanceOf(address owner) external view returns (uint256 balance);

    function ownerOf(uint256 tokenId) external view returns (address owner);

    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

    function transferFrom(address from, address to, uint256 tokenId) external;

    function approve(address to, uint256 tokenId) external;

    function getApproved(
        uint256 tokenId
    ) external view returns (address operator);

    function setApprovalForAll(address operator, bool _approved) external;

    function isApprovedForAll(
        address owner,
        address operator
    ) external view returns (bool);

    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId,
        bytes calldata data
    ) external;
}

interface INFT is IERC721 {
    function mint(address to, uint256 level) external returns (uint256);
}

contract Ownable {
    address private _owner;
    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _transferOwnership(_msgSender());
    }

    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

contract Manager is Ownable {
    address public manager;
    modifier onlyManager() {
        require(
            owner() == _msgSender() || manager == _msgSender(),
            "Ownable: Not Manager"
        );
        _;
    }

    function setManager(address account) public virtual onlyManager {
        manager = account;
    }
}

contract DateTime {
    uint256 constant DAY_IN_SECONDS = 86400;
    uint256 constant YEAR_IN_SECONDS = 31536000;
    uint256 constant LEAP_YEAR_IN_SECONDS = 31622400;
    uint256 constant HOUR_IN_SECONDS = 3600;
    uint256 constant MINUTE_IN_SECONDS = 60;
    uint16 constant ORIGIN_YEAR = 1970;

    function isLeapYear(uint256 year) internal pure returns (bool) {
        if (year % 4 != 0) {
            return false;
        }
        if (year % 100 != 0) {
            return true;
        }
        if (year % 400 != 0) {
            return false;
        }
        return true;
    }

    function leapYearsBefore(uint256 year) internal pure returns (uint256) {
        year -= 1;
        return year / 4 - year / 100 + year / 400;
    }

    function getDaysInMonth(
        uint256 month,
        uint256 year
    ) internal pure returns (uint256) {
        if (
            month == 1 ||
            month == 3 ||
            month == 5 ||
            month == 7 ||
            month == 8 ||
            month == 10 ||
            month == 12
        ) {
            return 31;
        } else if (month == 4 || month == 6 || month == 9 || month == 11) {
            return 30;
        } else if (isLeapYear(year)) {
            return 29;
        } else {
            return 28;
        }
    }

    function parseTimestamp(
        uint256 timestamp
    )
        internal
        pure
        returns (
            uint256 year,
            uint256 month,
            uint256 day,
            uint256 weekday,
            uint256 hour,
            uint256 minute,
            uint256 second
        )
    {
        uint256 secondsAccountedFor = 0;
        uint256 buf;
        uint8 i;
        year = getYear(timestamp);
        buf = leapYearsBefore(year) - leapYearsBefore(ORIGIN_YEAR);
        secondsAccountedFor += LEAP_YEAR_IN_SECONDS * buf;
        secondsAccountedFor += YEAR_IN_SECONDS * (year - ORIGIN_YEAR - buf);
        uint256 secondsInMonth;
        for (i = 1; i <= 12; i++) {
            secondsInMonth = DAY_IN_SECONDS * getDaysInMonth(i, year);
            if (secondsInMonth + secondsAccountedFor > timestamp) {
                month = i;
                break;
            }
            secondsAccountedFor += secondsInMonth;
        }
        for (i = 1; i <= getDaysInMonth(month, year); i++) {
            if (DAY_IN_SECONDS + secondsAccountedFor > timestamp) {
                day = i;
                break;
            }
            secondsAccountedFor += DAY_IN_SECONDS;
        }
        hour = getHour(timestamp);
        minute = getMinute(timestamp);
        second = getSecond(timestamp);
        weekday = getWeekday(timestamp);
    }

    function getYear(uint256 timestamp) internal pure returns (uint16) {
        uint256 secondsAccountedFor = 0;
        uint16 year;
        uint256 numLeapYears;
        year = uint16(ORIGIN_YEAR + timestamp / YEAR_IN_SECONDS);
        numLeapYears = leapYearsBefore(year) - leapYearsBefore(ORIGIN_YEAR);
        secondsAccountedFor += LEAP_YEAR_IN_SECONDS * numLeapYears;
        secondsAccountedFor +=
            YEAR_IN_SECONDS *
            (year - ORIGIN_YEAR - numLeapYears);
        while (secondsAccountedFor > timestamp) {
            if (isLeapYear(uint16(year - 1))) {
                secondsAccountedFor -= LEAP_YEAR_IN_SECONDS;
            } else {
                secondsAccountedFor -= YEAR_IN_SECONDS;
            }
            year -= 1;
        }
        return year;
    }

    function getMonth(uint256 timestamp) internal pure returns (uint256 month) {
        (, month, , , , , ) = parseTimestamp(timestamp);
    }

    function getDay(uint256 timestamp) internal pure returns (uint256 day) {
        (, , day, , , , ) = parseTimestamp(timestamp);
    }

    function getHour(uint256 timestamp) internal pure returns (uint256) {
        return ((timestamp / 60 / 60) % 24);
    }

    function getMinute(uint256 timestamp) internal pure returns (uint256) {
        return ((timestamp / 60) % 60);
    }

    function getSecond(uint256 timestamp) internal pure returns (uint256) {
        return (timestamp % 60);
    }

    function getWeekday(uint256 timestamp) internal pure returns (uint256) {
        return ((timestamp / DAY_IN_SECONDS + 4) % 7);
    }

    function toTimestamp(
        uint16 year,
        uint8 month,
        uint8 day
    ) internal pure returns (uint256 timestamp) {
        return toTimestamp(year, month, day, 0, 0, 0);
    }

    function toTimestamp(
        uint16 year,
        uint8 month,
        uint8 day,
        uint8 hour
    ) internal pure returns (uint256 timestamp) {
        return toTimestamp(year, month, day, hour, 0, 0);
    }

    function toTimestamp(
        uint16 year,
        uint8 month,
        uint8 day,
        uint8 hour,
        uint8 minute
    ) internal pure returns (uint256 timestamp) {
        return toTimestamp(year, month, day, hour, minute, 0);
    }

    function toTimestamp(
        uint16 year,
        uint8 month,
        uint8 day,
        uint8 hour,
        uint8 minute,
        uint8 second
    ) internal pure returns (uint256 timestamp) {
        uint16 i;
        for (i = ORIGIN_YEAR; i < year; i++) {
            if (isLeapYear(i)) {
                timestamp += LEAP_YEAR_IN_SECONDS;
            } else {
                timestamp += YEAR_IN_SECONDS;
            }
        }
        uint8[12] memory monthDayCounts;
        monthDayCounts[0] = 31;
        if (isLeapYear(year)) {
            monthDayCounts[1] = 29;
        } else {
            monthDayCounts[1] = 28;
        }
        monthDayCounts[2] = 31;
        monthDayCounts[3] = 30;
        monthDayCounts[4] = 31;
        monthDayCounts[5] = 30;
        monthDayCounts[6] = 31;
        monthDayCounts[7] = 31;
        monthDayCounts[8] = 30;
        monthDayCounts[9] = 31;
        monthDayCounts[10] = 30;
        monthDayCounts[11] = 31;
        for (i = 1; i < month; i++) {
            timestamp += DAY_IN_SECONDS * monthDayCounts[i - 1];
        }
        timestamp += DAY_IN_SECONDS * (day - 1);
        timestamp += HOUR_IN_SECONDS * (hour);
        timestamp += MINUTE_IN_SECONDS * (minute);
        timestamp += second;
        return timestamp;
    }

    function getDayNum(uint256 timestamp) internal pure returns (uint256) {
        (uint256 year, uint256 month, uint256 day, , , , ) = parseTimestamp(
            timestamp
        );
        return year * 10000 + month * 100 + day;
    }

    function getDayHour(uint256 timestamp) internal pure returns (uint256) {
        (
            uint256 year,
            uint256 month,
            uint256 day,
            ,
            uint256 hour,
            ,

        ) = parseTimestamp(timestamp);
        return year * 1000000 + month * 10000 + day * 100 + hour;
    }

    function getDayMinute(uint256 timestamp) internal pure returns (uint256) {
        (
            uint256 year,
            uint256 month,
            uint256 day,
            ,
            uint256 hour,
            uint256 minute,

        ) = parseTimestamp(timestamp);
        return
            (year * 1000000) +
            (month * 10000) +
            (day * 100) +
            ((hour % 10) * 10 + minute / 10);
    }
}

interface IDO {
    struct UserInfo {
        bool isExist;
        bool isNFT;
        uint balance;
        uint amount;
        uint reward;
        uint invites;
        uint registerTime;
        address refer;
    }

    function userTotal() external view returns (uint);

    function userAdds(uint index) external view returns (address);

    function users(
        address account
    ) external view returns (UserInfo memory info);
}

contract SFCStake is Manager, DateTime {
    struct UserInfo {
        bool isExist;
        uint level;
        uint balanceSFC;
        uint balanceUSDF;
        uint balanceJTL;
        uint totalSFC;
        uint totalUSDF;
        uint totalJTL;
        uint releaseSFC;
        uint releaseUSDF;
        uint releaseTotalSFC;
        uint releaseTotalUSDF;
        address refer;
    }
    struct RewardInfo {
        bool isNFT;
        uint amount;
        uint rewardMax;
        uint rewardTotal;
        uint rewardStake;
        uint rewardInvite;
        uint rewardTeam;
        uint rewardRate;
        uint rewardBalance;
        uint lastTime;
        uint lastTimeStake;
    }
    struct StakeInfo {
        bool isValid;
        bool isSFC;
        uint index;
        uint userIndex;
        uint amount;
        uint actual;
        uint rate;
        uint startTime;
        uint lastTime;
        uint reward;
        uint rewardMax;
        address owner;
    }
    struct TotalReward {
        uint rewardStake;
        uint rewardInvite;
        uint rewardTeam;
        uint daoBalance;
        uint daoTotal;
    }
    struct TotalInfo {
        uint totalStake;
        uint totalWithdraw;
        uint totalReleaseSFC;
        uint totalWithdrawReleaseSFC;
        uint totalReleaseUSDF;
        uint totalWithdrawReleaseUSDF;
    }
    TotalReward public totalReward;
    TotalInfo public totalInfo;
    uint public stakeTotal;
    mapping(uint => StakeInfo) public stakes;
    mapping(address => mapping(uint => uint)) public userStakeIndex;
    mapping(address => uint) public userStakes;
    uint public userTotal;
    mapping(address => UserInfo) public users;
    mapping(address => RewardInfo) public userRewards;
    mapping(uint => address) public userAdds;
    mapping(address => mapping(uint => address)) public userInvites;
    mapping(address => uint) public userInviteTotals;
    mapping(address => bool) public isBlackList;
    uint _dayTimes = 86400;
    uint private _depositMin = 100e18;
    uint private _depositMax = 300e18;
    uint private _feeRate;
    uint private _teamEqualeRate = 200;
    uint private _stakeRate = 20;
    uint private _interestRate = 12;
    uint private _daoRate = 50;
    uint private _priceJTL = 2e18;
    uint private _nftJTL = 200e18;
    uint[2] private _inviteRates = [150, 100];
    uint[5] private _teamRates = [60, 120, 180, 240, 300];
    address private _feeTo;
    address private _system;
    address private _market;
    address private _dao;
    IDO private _IDO;
    INFT private _SFCN;
    INFT private _JTLN;
    IERC20 private _SFC;
    IERC20 private _USDF;
    IERC20 private _JTL;
    IERC20 private _USDT;
    ISwapRouter private _swapRouter;
    modifier onlySystem() {
        require(
            owner() == _msgSender() ||
                manager == _msgSender() ||
                _system == _msgSender(),
            "Ownable: Not System"
        );
        _;
    }
    event BindRefer(address account, address refer);
    event Actions(
        address account,
        uint category,
        uint amount1,
        uint amount2,
        uint amount3,
        uint amount4
    );

    constructor() {
        manager = 0xCf0a26e4E00a2ee8Eb6a47e4D3649485D2D8d11A;
        _feeTo = 0x3886f14F92562c802e999f969727a0A603E80BE7;
        _system = 0x63446730116b7F24529cC0a0CE3854728970D172;
        _IDO = IDO(0x66398F9fe934398eF689DfA6960FaF29eFFB5259);
        _SFCN = INFT(0x6711C0E65113A0f352f0053692aB43551FeF6b0D);
        _JTLN = INFT(0x4415aFb2f17F6E8cc381C4533A2aC35B47904c24);
        _SFC = IERC20(0x0EEFd77cc7e0a3C286aC7aCF409DF835D556107F);
        _USDF = IERC20(0x5C8228675C5862d3233f908DeaF08932a7F82CDc);
        _JTL = IERC20(0x4FD1158E462dF415286DD3755d56433851faCaD6);
        _USDT = IERC20(0x55d398326f99059fF775485246999027B3197955);
        _swapRouter = ISwapRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        userTotal++;
        userAdds[userTotal] = 0x63446730116b7F24529cC0a0CE3854728970D172;
        users[0x63446730116b7F24529cC0a0CE3854728970D172].isExist = true;
    }

    function withdrawToken(
        IERC20 token,
        address to,
        uint amount
    ) public onlyManager {
        token.transfer(to, amount);
    }

    function setTokenAdd(uint category, address data) public onlyManager {
        if (category == 1) _feeTo = data;
        if (category == 2) _system = data;
        if (category == 3) _market = data;
        if (category == 4) _dao = data;
        if (category == 5) _IDO = IDO(data);
        if (category == 6) _SFCN = INFT(data);
        if (category == 7) _JTLN = INFT(data);
        if (category == 8) _SFC = IERC20(data);
        if (category == 9) _USDF = IERC20(data);
        if (category == 10) _JTL = IERC20(data);
        if (category == 11) _USDT = IERC20(data);
        if (category == 12) _swapRouter = ISwapRouter(data);
    }

    function setConfig(uint category, uint data) public onlyManager {
        if (category == 1) _depositMin = data;
        if (category == 2) _depositMax = data;
        if (category == 3) _feeRate = data;
        if (category == 4) _teamEqualeRate = data;
        if (category == 5) _stakeRate = data;
        if (category == 6) _interestRate = data;
        if (category == 7) _daoRate = data;
        if (category == 8) _priceJTL = data;
        if (category == 9) _nftJTL = data;
        if (category == 10) _dayTimes = data;
    }

    function setConfigMulti(
        uint category,
        uint[] memory data
    ) public onlyManager {
        for (uint i = 0; i < data.length; i++) {
            if (category == 1 && i < _teamRates.length && data[i] < 1000)
                _teamRates[i] = data[i];
            if (category == 2 && i < _inviteRates.length && data[i] < 1000)
                _inviteRates[i] = data[i];
        }
    }

    function setIsBlackList(address account, bool data) public onlyManager {
        isBlackList[account] = data;
    }

    function setUserLevel(address account, uint level) public onlySystem {
        require(users[account].isExist, "User Not Exist");
        require(level <= _teamRates.length, "Level Error");
        updateUser(account);
        users[account].level = level;
    }

    function addUser(
        address[] calldata accounts,
        uint[] calldata amounts
    ) public onlyManager {
        for (uint256 i = 0; i < accounts.length; i++) {
            _deposit(accounts[i], false, amounts[i]);
        }
    }

    function synchroUser() public onlyManager {
        uint total = _IDO.userTotal();
        for (uint256 i = 0; i < total; i++) {
            address account = _IDO.userAdds(i + 1);
            IDO.UserInfo memory user = _IDO.users(account);
            if (user.balance > 0) {
                _deposit(account, false, user.balance);
            }
        }
    }

    function getTokenAdd()
        public
        view
        returns (
            address feeTo,
            address system,
            address market,
            address dao,
            address ido,
            address sfcn,
            address jtln,
            address sfc,
            address usdf,
            address usdt
        )
    {
        feeTo = _feeTo;
        system = _system;
        market = _market;
        dao = _dao;
        ido = address(_IDO);
        sfcn = address(_SFCN);
        jtln = address(_JTLN);
        sfc = address(_SFC);
        usdf = address(_USDF);
        usdt = address(_USDT);
    }

    function getConfig()
        public
        view
        returns (
            uint depositMin,
            uint depositMax,
            uint feeRate,
            uint teamEqualeRate,
            uint stakeRate,
            uint interestRate,
            uint daoRate,
            uint priceJTL,
            uint nftJTL
        )
    {
        depositMin = _depositMin;
        depositMax = _depositMax;
        feeRate = _feeRate;
        teamEqualeRate = _teamEqualeRate;
        stakeRate = _stakeRate;
        interestRate = _interestRate;
        daoRate = _daoRate;
        priceJTL = _priceJTL;
        nftJTL = _nftJTL;
    }

    function getConfigMulti()
        public
        view
        returns (uint[2] memory inviteRates, uint[5] memory teamRates)
    {
        inviteRates = _inviteRates;
        teamRates = _teamRates;
    }

    function getSwapAmount(uint usdt) public view returns (uint256) {
        address[] memory path = new address[](2);
        path[0] = address(_USDT);
        path[1] = address(_SFC);
        address swapPair = ISwapFactory(_swapRouter.factory()).getPair(
            address(_SFC),
            address(_USDT)
        );
        if (swapPair == address(0)) return 0;
        (uint256 reserve1, uint256 reserve2, ) = ISwapPair(swapPair)
            .getReserves();
        if (reserve1 == 0 || reserve2 == 0) {
            return 0;
        } else {
            return _swapRouter.getAmountsOut(usdt, path)[1];
        }
    }

    function getUserInfo(
        address account
    )
        public
        view
        returns (
            UserInfo memory user,
            RewardInfo memory userReward,
            StakeInfo[] memory infos
        )
    {
        user = users[account];
        userReward = userRewards[account];
        if (
            userReward.lastTime > 0 &&
            userReward.lastTime < block.timestamp &&
            user.balanceUSDF > 0
        ) {
            uint reward = (user.balanceUSDF *
                (block.timestamp - userReward.lastTime) *
                _interestRate) / (_dayTimes * 1000);
            uint tokenSFC = getSwapAmount(reward);
            user.balanceSFC += tokenSFC;
            user.totalSFC += tokenSFC;
            uint tokenJTL = (reward * 1e18) / _priceJTL;
            user.balanceJTL += tokenJTL;
            user.totalJTL += tokenJTL;
        }
        userReward.lastTime = block.timestamp;
        uint256 total = userStakes[account];
        infos = new StakeInfo[](total);
        uint256 rewardTotal;
        for (uint256 i = 0; i < total; i++) {
            uint256 index = userStakeIndex[account][i + 1];
            if (stakes[index].isValid && stakes[index].owner == account) {
                StakeInfo memory stake = stakes[index];
                uint256 timestamp = block.timestamp - stake.lastTime;
                uint256 reward = (stake.rate * stake.amount * timestamp) /
                    (1000 * _dayTimes);
                if (stake.reward + userReward.rewardBalance > stake.rewardMax) {
                    userReward.rewardBalance -= (stake.rewardMax -
                        stake.reward);
                    stake.reward = stake.rewardMax;
                } else if (userReward.rewardBalance > 0) {
                    stake.reward += userReward.rewardBalance;
                    userReward.rewardBalance = 0;
                }
                if (stake.reward + reward > stake.rewardMax) {
                    reward = stake.rewardMax - stake.reward;
                }
                rewardTotal += reward;
                stake.reward += reward;
                stake.lastTime = block.timestamp;
                if (stake.reward >= stake.rewardMax) {
                    stake.isValid = false;
                    stake.amount = 0;
                    if (userReward.amount < stake.actual) userReward.amount = 0;
                    else userReward.amount -= stake.actual;
                    if (userReward.amount == 0) userReward.rewardBalance = 0;
                    if (stake.isSFC) {
                        user.releaseSFC += stake.actual;
                        user.releaseTotalSFC += stake.actual;
                    } else {
                        user.releaseUSDF += stake.actual;
                        user.releaseTotalUSDF += stake.actual;
                    }
                }
                infos[i] = stake;
            } else {
                infos[i] = stakes[index];
            }
        }
        if (rewardTotal > 0) {
            if (userReward.rewardTotal + rewardTotal > userReward.rewardMax) {
                rewardTotal = userReward.rewardMax - userReward.rewardTotal;
            }
            user.balanceUSDF += rewardTotal;
            user.totalUSDF += rewardTotal;
            userReward.rewardStake += rewardTotal;
            userReward.rewardTotal += rewardTotal;
            if (
                userReward.lastTimeStake > 0 &&
                userReward.lastTimeStake < block.timestamp
            ) {
                uint times = (block.timestamp - userReward.lastTimeStake) / 60;
                uint reward;
                for (uint256 i = 0; i < times; i++) {
                    reward +=
                        ((rewardTotal / times) * (i + 1) * _interestRate) /
                        (1000 * (_dayTimes / 60));
                }
                if (reward > 0) {
                    uint tokenSFC = getSwapAmount(reward);
                    user.balanceSFC += tokenSFC;
                    user.totalSFC += tokenSFC;
                    uint tokenJTL = (reward * 1e18) / _priceJTL;
                    user.balanceJTL += tokenJTL;
                    user.totalJTL += tokenJTL;
                }
            }
        }
        userReward.lastTimeStake = block.timestamp;
    }

    function getInvitesInfo(
        address account
    ) public view returns (address[] memory invites, UserInfo[] memory infos) {
        invites = new address[](userInviteTotals[account]);
        infos = new UserInfo[](userInviteTotals[account]);
        for (uint i = 0; i < userInviteTotals[account]; i++) {
            invites[i] = userInvites[account][i + 1];
            infos[i] = users[invites[i]];
        }
    }

    function register(address refer) public {
        address account = msg.sender;
        require(users[refer].isExist, "Refer Not Exist");
        require(!users[account].isExist, "Has Exist");
        UserInfo storage user = users[account];
        user.isExist = true;
        userTotal++;
        userAdds[userTotal] = account;
        user.refer = refer;
        userRewards[account].lastTime = block.timestamp;
        userRewards[account].lastTimeStake = block.timestamp;
        userInviteTotals[refer]++;
        userInvites[refer][userInviteTotals[refer]] = account;
        emit BindRefer(account, refer);
    }

    function claimNFT() public {
        address account = msg.sender;
        require(users[account].isExist, "User Not Exist");
        require(_IDO.users(account).isNFT, "No NFT");
        require(!userRewards[account].isNFT, "Has Mint");
        updateUser(account);
        userRewards[account].isNFT = true;
        _SFCN.mint(account, 1);
    }

    function mintNFT() public {
        address account = msg.sender;
        require(users[account].isExist, "User Not Exist");
        updateUser(account);
        require(users[account].balanceJTL >= _nftJTL, "Insufficient balance");
        users[account].balanceJTL -= _nftJTL;
        _JTLN.mint(account, 1);
    }

    function deposit(bool isSFC, uint amount) public {
        address account = msg.sender;
        require(users[account].isExist, "User Not Exist");
        require(amount >= _depositMin, "Amount Lower");
        require(amount <= _depositMax, "Amount Over");
        updateUser(account);
        if (isSFC)
            _SFC.transferFrom(account, address(this), getSwapAmount(amount));
        else _USDF.transferFrom(account, address(this), amount);
        _deposit(account, isSFC, amount);
    }

    function claimRelease(bool isSFC) public {
        address account = msg.sender;
        require(!isBlackList[account], "Invalid address");
        require(users[account].isExist, "User Not Exist");
        updateUser(account);
        UserInfo storage user = users[account];
        if (isSFC) {
            uint amount = user.releaseSFC;
            if (amount == 0) {
                return;
            }
            user.releaseSFC = 0;
            uint tokens = getSwapAmount(amount);
            _SFC.transfer(account, tokens);
            emit Actions(
                account,
                3,
                amount,
                tokens,
                user.releaseTotalSFC,
                userRewards[account].amount
            );
            totalInfo.totalWithdrawReleaseSFC += amount;
            if (totalInfo.totalReleaseSFC > amount)
                totalInfo.totalReleaseSFC -= amount;
            else totalInfo.totalReleaseSFC = 0;
        } else {
            uint amount = user.releaseUSDF;
            if (amount == 0) {
                return;
            }
            user.releaseUSDF = 0;
            _USDF.transfer(account, amount);
            emit Actions(
                account,
                4,
                amount,
                0,
                user.releaseTotalUSDF,
                userRewards[account].amount
            );
            totalInfo.totalWithdrawReleaseUSDF += amount;
            if (totalInfo.totalReleaseUSDF > amount)
                totalInfo.totalReleaseUSDF -= amount;
            else totalInfo.totalReleaseUSDF = 0;
        }
    }

    function claimReward(bool isSFC) public {
        address account = msg.sender;
        require(!isBlackList[account], "Invalid address");
        require(users[account].isExist, "User Not Exist");
        updateUser(account);
        if (isSFC) {
            UserInfo storage user = users[account];
            uint amount = user.balanceSFC;
            if (amount == 0) {
                return;
            }
            user.balanceSFC = 0;
            uint fee = (amount * _feeRate) / 1000;
            if (fee > 0) _SFC.transfer(_feeTo, fee);
            _SFC.transfer(account, amount - fee);
            emit Actions(
                account,
                5,
                amount,
                0,
                user.totalSFC,
                userRewards[account].amount
            );
            totalInfo.totalWithdraw += amount;
        } else {
            UserInfo storage user = users[account];
            uint amount = user.balanceUSDF;
            if (amount == 0) {
                return;
            }
            user.balanceUSDF = 0;
            uint fee = (amount * _feeRate) / 1000;
            if (fee > 0) _USDF.transfer(_feeTo, fee);
            _USDF.transfer(account, amount - fee);
            emit Actions(
                account,
                6,
                amount,
                0,
                user.totalUSDF,
                userRewards[account].amount
            );
            totalInfo.totalWithdraw += amount;
        }
    }

    function claimSwapSFC() public {
        address account = msg.sender;
        require(!isBlackList[account], "Invalid address");
        require(users[account].isExist, "User Not Exist");
        updateUser(account);
        UserInfo storage user = users[account];
        uint amount = user.balanceUSDF;
        if (amount == 0) {
            return;
        }
        user.balanceUSDF = 0;
        uint tokens = getSwapAmount(amount);
        uint fee = (tokens * _feeRate) / 1000;
        if (fee > 0) _SFC.transfer(_feeTo, fee);
        _SFC.transfer(account, tokens - fee);
        emit Actions(
            account,
            7,
            amount,
            tokens,
            user.totalUSDF,
            userRewards[account].amount
        );
        totalInfo.totalWithdraw += amount;
    }

    function claimJTL() public {
        address account = msg.sender;
        require(!isBlackList[account], "Invalid address");
        require(users[account].isExist, "User Not Exist");
        updateUser(account);
        UserInfo storage user = users[account];
        uint amount = user.balanceJTL;
        if (amount == 0) {
            return;
        }
        user.balanceJTL = 0;
        uint fee = (amount * _feeRate) / 1000;
        if (fee > 0) _JTL.transfer(_feeTo, fee);
        _JTL.transfer(account, amount - fee);
        emit Actions(
            account,
            8,
            amount,
            0,
            user.totalJTL,
            userRewards[account].amount
        );
    }

    function claimDao() public {
        if (totalReward.daoBalance > 0) {
            _USDF.transfer(_dao, totalReward.daoBalance);
            totalReward.daoBalance = 0;
        }
    }

    function updateUser(address account) public {
        require(users[account].isExist, "User Not Exist");
        UserInfo storage user = users[account];
        RewardInfo storage userReward = userRewards[account];
        if (
            userReward.lastTime > 0 &&
            userReward.lastTime < block.timestamp &&
            user.balanceUSDF > 0
        ) {
            uint reward = (user.balanceUSDF *
                (block.timestamp - userReward.lastTime) *
                _interestRate) / (_dayTimes * 1000);
            uint tokenSFC = getSwapAmount(reward);
            user.balanceSFC += tokenSFC;
            user.totalSFC += tokenSFC;
            uint tokenJTL = (reward * 1e18) / _priceJTL;
            user.balanceJTL += tokenJTL;
            user.totalJTL += tokenJTL;
            emit Actions(
                account,
                21,
                reward,
                tokenSFC,
                user.balanceSFC,
                user.totalSFC
            );
            emit Actions(
                account,
                22,
                reward,
                tokenJTL,
                user.balanceJTL,
                user.totalJTL
            );
        }
        userReward.lastTime = block.timestamp;
        uint256 total = userStakes[account];
        uint256 rewardTotal;
        for (uint256 i = 0; i < total; i++) {
            uint256 index = userStakeIndex[account][i + 1];
            if (stakes[index].isValid && stakes[index].owner == account) {
                StakeInfo storage stake = stakes[index];
                uint256 timestamp = block.timestamp - stake.lastTime;
                uint256 reward = (stake.rate * stake.amount * timestamp) /
                    (1000 * _dayTimes);
                if (stake.reward + userReward.rewardBalance > stake.rewardMax) {
                    userReward.rewardBalance -= (stake.rewardMax -
                        stake.reward);
                    stake.reward = stake.rewardMax;
                } else if (userReward.rewardBalance > 0) {
                    stake.reward += userReward.rewardBalance;
                    userReward.rewardBalance = 0;
                }
                if (stake.reward + reward > stake.rewardMax) {
                    reward = stake.rewardMax - stake.reward;
                }
                rewardTotal += reward;
                stake.reward += reward;
                stake.lastTime = block.timestamp;
                if (stake.reward >= stake.rewardMax) {
                    stake.isValid = false;
                    stake.amount = 0;
                    if (userReward.amount < stake.actual) userReward.amount = 0;
                    else userReward.amount -= stake.actual;
                    if (userReward.amount == 0) userReward.rewardBalance = 0;
                    if (stake.isSFC) {
                        user.releaseSFC += stake.actual;
                        user.releaseTotalSFC += stake.actual;
                        totalInfo.totalReleaseSFC += stake.actual;
                        emit Actions(
                            account,
                            5,
                            stake.index,
                            stake.actual,
                            user.releaseSFC,
                            user.releaseTotalSFC
                        );
                    } else {
                        user.releaseUSDF += stake.actual;
                        user.releaseTotalUSDF += stake.actual;
                        totalInfo.totalReleaseUSDF += stake.actual;
                        emit Actions(
                            account,
                            5,
                            stake.index,
                            stake.actual,
                            user.releaseUSDF,
                            user.releaseTotalUSDF
                        );
                    }
                    if (totalInfo.totalStake <= stake.actual)
                        totalInfo.totalStake = 0;
                    else totalInfo.totalStake -= stake.actual;
                }
            }
        }
        if (rewardTotal > 0) {
            if (userReward.rewardTotal + rewardTotal > userReward.rewardMax) {
                rewardTotal = userReward.rewardMax - userReward.rewardTotal;
            }
            uint daoReward = (rewardTotal * _daoRate) / 1000;
            totalReward.daoBalance += daoReward;
            totalReward.daoTotal += daoReward;
            rewardTotal -= daoReward;
            user.balanceUSDF += rewardTotal;
            user.totalUSDF += rewardTotal;
            userReward.rewardStake += rewardTotal;
            userReward.rewardTotal += rewardTotal;
            totalReward.rewardStake += rewardTotal;
            emit Actions(
                account,
                11,
                rewardTotal,
                user.balanceUSDF,
                userReward.rewardStake,
                user.totalUSDF
            );
            if (
                userReward.lastTimeStake > 0 &&
                userReward.lastTimeStake < block.timestamp
            ) {
                uint times = (block.timestamp - userReward.lastTimeStake) / 60;
                uint reward;
                for (uint256 i = 0; i < times; i++) {
                    reward +=
                        ((rewardTotal / times) * (i + 1) * _interestRate) /
                        (1000 * (_dayTimes / 60));
                }
                if (reward > 0) {
                    uint tokenSFC = getSwapAmount(reward);
                    user.balanceSFC += tokenSFC;
                    user.totalSFC += tokenSFC;
                    uint tokenJTL = (reward * 1e18) / _priceJTL;
                    user.balanceJTL += tokenJTL;
                    user.totalJTL += tokenJTL;
                    emit Actions(
                        account,
                        21,
                        reward,
                        tokenSFC,
                        user.balanceSFC,
                        user.totalSFC
                    );
                    emit Actions(
                        account,
                        22,
                        reward,
                        tokenJTL,
                        user.balanceJTL,
                        user.totalJTL
                    );
                }
            }
            userReward.lastTimeStake = block.timestamp;
            _sendTeamReward(account, rewardTotal);
        }
        userReward.lastTimeStake = block.timestamp;
    }

    function _sendTeamReward(address account, uint amount) private {
        address refer = users[account].refer;
        uint currLevel;
        uint currRate;
        uint currReward;
        for (uint i = 0; i < 30; i++) {
            if (refer == address(0)) break;
            UserInfo storage parent = users[refer];
            RewardInfo storage parentR = userRewards[refer];
            if (parentR.lastTime == 0) parentR.lastTime = block.timestamp;
            if (parentR.lastTime < block.timestamp && parent.balanceUSDF > 0) {
                uint reward = (parent.balanceUSDF *
                    (block.timestamp - parentR.lastTime) *
                    _interestRate) / (_dayTimes * 1000);
                uint tokenSFC = getSwapAmount(reward);
                parent.balanceSFC += tokenSFC;
                parent.totalSFC += tokenSFC;
                uint tokenJTL = (reward * 1e18) / _priceJTL;
                parent.balanceJTL += tokenJTL;
                parent.totalJTL += tokenJTL;
                emit Actions(
                    refer,
                    21,
                    reward,
                    tokenSFC,
                    parent.balanceSFC,
                    parent.totalSFC
                );
                emit Actions(
                    refer,
                    22,
                    reward,
                    tokenJTL,
                    parent.balanceJTL,
                    parent.totalJTL
                );
            }
            parentR.lastTime = block.timestamp;
            {
                if (i < _inviteRates.length) {
                    uint reward = (amount * _inviteRates[i]) / 1000;
                    if (reward > 0 && (parentR.amount > 0)) {
                        if (parentR.rewardTotal + reward > parentR.rewardMax) {
                            reward = parentR.rewardMax - parentR.rewardTotal;
                        }
                        parent.balanceUSDF += reward;
                        parentR.rewardTotal += reward;
                        parentR.rewardInvite += reward;
                        parentR.rewardBalance += reward;
                        totalReward.rewardInvite += reward;
                        emit Actions(
                            refer,
                            12,
                            reward,
                            parent.balanceUSDF,
                            parentR.rewardInvite,
                            parentR.rewardTotal
                        );
                    }
                }
            }
            {
                if (parent.level == currLevel && currReward > 0) {
                    uint reward = (currReward * _teamEqualeRate) / 1000;
                    currReward = 0;
                    if (reward > 0 && parentR.amount > 0) {
                        if (parentR.rewardTotal + reward > parentR.rewardMax) {
                            reward = parentR.rewardMax - parentR.rewardTotal;
                        }
                        parent.balanceUSDF += reward;
                        parentR.rewardTotal += reward;
                        parentR.rewardTeam += reward;
                        parentR.rewardBalance += reward;
                        totalReward.rewardTeam += reward;
                        emit Actions(
                            refer,
                            13,
                            reward,
                            parent.balanceUSDF,
                            parentR.rewardTeam,
                            parentR.rewardTotal
                        );
                    }
                } else if (
                    parent.level > currLevel &&
                    parent.level <= _teamRates.length &&
                    _teamRates[parent.level - 1] > currRate
                ) {
                    uint reward = ((_teamRates[parent.level - 1] - currRate) *
                        amount) / 1000;
                    if (parentR.rewardTotal + reward > parentR.rewardMax) {
                        reward = parentR.rewardMax - parentR.rewardTotal;
                    }
                    currReward = reward;
                    currLevel = parent.level;
                    currRate = _teamRates[parent.level - 1];
                    if (reward > 0 && parentR.amount > 0) {
                        parent.balanceUSDF += reward;
                        parentR.rewardTotal += reward;
                        parentR.rewardTeam += reward;
                        parentR.rewardBalance += reward;
                        totalReward.rewardTeam += reward;
                        emit Actions(
                            refer,
                            14,
                            reward,
                            parent.balanceUSDF,
                            parentR.rewardTeam,
                            parentR.rewardTotal
                        );
                    }
                }
            }
            if (parent.level == _teamRates.length && currReward == 0) {
                break;
            }
            refer = users[refer].refer;
        }
    }

    function _deposit(address account, bool isSFC, uint amount) private {
        {
            stakeTotal++;
            stakes[stakeTotal] = StakeInfo({
                isValid: true,
                isSFC: isSFC,
                index: stakeTotal,
                userIndex: userStakes[account] + 1,
                amount: amount,
                actual: amount,
                rate: _stakeRate,
                startTime: block.timestamp,
                lastTime: block.timestamp,
                reward: 0,
                rewardMax: isSFC ? amount : (amount * 12) / 10,
                owner: account
            });
            userStakes[account]++;
            userStakeIndex[account][userStakes[account]] = stakeTotal;
            userRewards[account].amount += amount;
            userRewards[account].rewardMax += (
                isSFC ? amount : (amount * 12) / 10
            );
            emit Actions(
                account,
                1,
                isSFC ? 1 : 0,
                amount,
                isSFC ? getSwapAmount(amount) : 0,
                userRewards[account].amount
            );
        }
        totalInfo.totalStake += amount;
    }

    function _random(uint256 lenth) private view returns (uint256) {
        return
            uint256(
                keccak256(
                    abi.encodePacked(block.timestamp, msg.sender, tx.origin)
                )
            ) % lenth;
    }

    function _randomWithSeed(
        uint256 lenth,
        uint256 seed
    ) private view returns (uint256) {
        return
            uint256(
                keccak256(
                    abi.encodePacked(
                        block.timestamp,
                        msg.sender,
                        seed,
                        tx.origin
                    )
                )
            ) % lenth;
    }
}