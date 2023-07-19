// SPDX-License-Identifier: MIT
pragma solidity 0.8.17;


// CAUTION
// This version of SafeMath should only be used with Solidity 0.8 or later,
// because it relies on the compiler's built in overflow checks.
library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }
}


// safe transfer
library TransferHelper {
    function safeApprove(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('approve(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x095ea7b3, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: APPROVE_FAILED');
    }

    function safeTransfer(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FAILED');
    }

    function safeTransferFrom(address token, address from, address to, uint value) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }

    function safeTransferETH(address to, uint value) internal {
        (bool success,) = to.call{value:value}(new bytes(0));
        // (bool success,) = to.call.value(value)(new bytes(0));
        require(success, 'TransferHelper: ETH_TRANSFER_FAILED');
    }
}



// Interface of the ERC20 standard as defined in the EIP.
interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address from, address to) external view returns (uint256);
    function approve(address to, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}


// linkedin inferface
interface ILinkedin {
    function mySuper(address user) external view returns (address);
    function myJuniors(address user) external view returns (address[] memory);
    function getSuperList(address user, uint256 list) external view returns (address[] memory);
}

interface IEternityV1 {
    function poolInfo(uint256 index) external view returns (PoolInfoV1 memory);
    function userInfo(uint256 pid, address account) external view returns (UserInfoV1 memory);
    function juniorMsg(uint256 pid, address account) external view returns (JuniorMsgV1 memory);
}
struct PoolInfoV1 {
    address token;                    // token address.
    uint256 totalDepositAmount;       // token deposit in pool.
    uint256 totalStaked;              // total staked. The current dividend amount shall be calculated once a day.
    uint256 leastDepositAmount;       // least deposit amount.
    uint256 luckyPoolEarnLimitAmount; // lucky pool earn limit.
    uint256 nextRewardTime;           // next share time. The time of the next dividend, once a day
    uint256 teamEarnCount;            // tean earn count.
    uint256 teamEarnAmount;           // team earn amount.
    uint256 teamEarnLimitAmount;      // team earn limit amount.
    uint256 juniorAmount;             // junior amount condition.
    uint256 userTokenPerShare;        // Accumulated Token per share, times SCALING_FACTOR. Check code.
    bool isOpen;                      // is open.
}
struct UserInfoV1 {
    uint256 depositAmount;        // deposit amount.
    uint256 takedAmount;          // already taked amount.
    uint256 takeLimitAmount;      // taked amount limit.
    uint256 rewardDebt;           // reward debt. Accumulated Token per share, times SCALING_FACTOR. Check code.
    uint256 residueAmount;        // residue amount. release residue.
    uint256 releaseStartTime;     // release start time.
    uint256 releaseEndTime;       // release end time.
}
struct JuniorMsgV1 {
    address oneJunior;    // 1-3 juniro and amount.
    address twoJunior;
    address threeJunior;
    uint256 oneAmount;
    uint256 twoAmount;
    uint256 threeAmount;
}

    

// owner
abstract contract Ownable {
    address public owner = address(0);
    address public oneAddress;

    constructor() {
        oneAddress = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == oneAddress, 'one address error');
        _;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        if (newOwner != address(0)) {
            oneAddress = newOwner;
        }
    }
}


// non reentrant
abstract contract ReentrancyGuard {
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;
    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    modifier nonReentrant() {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");
        _status = _ENTERED;
        _;
        _status = _NOT_ENTERED;
    }
}


// non contract
abstract contract ContractGuard {

    constructor() {}

    modifier nonContract() {
        require(!isContract(msg.sender), "ContractGuard: not user1 error");
        require(tx.origin == msg.sender, "ContractGuard: not user2 error");
        _;
    }

    function isContract(address account) internal view returns (bool) {
        return account.code.length > 0;
    }
}


// eternity V3.
contract EternityV3 is Ownable, ReentrancyGuard, ContractGuard {
    using SafeMath for uint256;
    PoolInfoV1 public poolInfoV1;
    UserInfoV1 public userInfoV1;
    JuniorMsgV1 public juniorMsgV1;


    // pid is deposit pool.
    struct PoolInfo {
        address token;                    // token address.
        address earnToken;                // earn token.
        uint256 totalDepositAmount;       // token deposit in pool.
        uint256 totalStaked;              // total staked. The current dividend amount shall be calculated once a day.
        uint256 leastDepositAmount;       // least deposit amount.
        uint256 luckyPoolEarnLimitAmount; // lucky pool earn limit.
        uint256 nextRewardTime;           // next share time. The time of the next dividend, once a day.
        uint256 teamEarnCount;            // tean earn count.
        uint256 teamEarnAmount;           // team earn amount.
        uint256 teamEarnLimitAmount;      // team earn limit amount.
        uint256 juniorAmount;             // junior amount condition.
        uint256 userTokenPerShare;        // Accumulated Token per share, times SCALING_FACTOR. Check code.
        bool isOpen;                      // is open.
    }
    PoolInfo[] public poolInfo;   // (index pid 0 start)
    // user deposit. poolID => ID => msg.
    mapping(uint256 => mapping(uint256 => UserInfo)) public userInfo;
    struct UserInfo {
        address ownerOf;              // account
        uint256 depositAmount;        // deposit amount.
        uint256 takedAmount;          // already taked amount.
        uint256 takeLimitAmount;      // taked amount limit.
        uint256 rewardDebt;           // reward debt. Accumulated Token per share, times SCALING_FACTOR. Check code.
        uint256 residueAmount;        // residue amount. release residue.
        uint256 releaseStartTime;     // release start time.
        uint256 releaseEndTime;       // release end time.
    }
    // 30 day release dividend.
    uint256 public releaseTime;
    bool public allIsOpen = true;

    // lucky pool
    uint256 private constant LUCKY_SEATS = 50;        // Seats limit, default 50.
    uint256 private immutable ONE_DAY_SECOND_NUMBER;  // mainnet=86400, testnet=60.
    uint256 public luckyIntervalTime;                 // interval time
    struct LuckyPool {
        uint256 earnTotalAmount;                      // earn total amount.
        uint256 expireTime;                           // expire time.
        uint256 isState;                              // 1=underway, 2=poised, 3=complete.
        uint256 luckyNextIndex;                       // next index, (0 start)
    }
    // (luck pool id 1 start)      
    mapping(uint256 => mapping(uint256 => LuckyPool)) public luckyPool;                        // lucky pool.         pid => round => lucky pool infor
    mapping(uint256 => mapping(uint256 => address[LUCKY_SEATS])) public luckyPoolAccounts;     // lucky address.      pid => round => luckyaddress
    mapping(uint256 => uint256) public luckyPoolCount;                                         // lucky pool count.   pid => luck pool count
    uint256 public luckyLastAddrssRatio = 3000;                                                // last address earn ratio. default. %%

    // 0 = super ratio. 1 = super super ratio. 2 = super super super ratio. %%
    uint256[3] public superRatio = [2000, 1000, 500];
    // mining pool ratio. lucky pool ratio. team pool ratio. leader address ratio.  %%
    uint256 public miningPoolRatio = 4000;
    uint256 public luckyPoolRatio = 500;
    uint256 public teamPoolRatio = 1300;
    uint256 public leaderRatio = 700;
    uint256 private constant RATIO_DENOMINATOR = 10000;   // denominator
    uint256 private constant SCALING_FACTOR = 1e18;       // Scaling this up increases support for high supply tokens
    // 2 mul earn limit.
    uint256 public earnMul = 2;

    // super earn condition.
    mapping(uint256 => mapping(address => JuniorMsg)) public juniorMsg;   // pid => account => junior msg.
    struct JuniorMsg {
        address oneJunior;    // 1-3 juniro and amount.
        address twoJunior;
        address threeJunior;
        uint256 oneAmount;
        uint256 twoAmount;
        uint256 threeAmount;
    }

    // linkedin. teamEarn. leader. leaderLucky.
    address public linkedin;
    address public leader;
    address public leaderLucky;
    // stLTC, coin
    address public stLTC;
    address public constant ETH = 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE;

    // twoAddress
    address public twoAddress;                      // sign address.
    mapping(uint256 => bool) public nonceUsed;      // nonce only one.

    mapping(address => bool) public blackList;                          // black list.
    uint256 public totalID;                                             // total ID.
    mapping(uint256 => mapping(address => uint256)) public accountID;   // account ID. (if is 0 add ID, else default)
    bool public isCanMoveData = false;                                   // is can move data
    address public EternityV1;                                          // Eternity v1
    struct PoolInfoThen {
        address recycleToken;             // recycle token.
        uint256 limitDepositAmount;       // limit deposit amount.
        bool isOpen;
    }
    mapping(uint256 => PoolInfoThen) public poolInfoThen; // pool info then.
    

    constructor(
        uint256 oneDaySecondNumber_,
        address linkedin_,
        address leader_,
        address leaderLucky_,
        address stLTC_,
        address twoAddress_,
        address EternityV1_
    ) {
        ONE_DAY_SECOND_NUMBER = oneDaySecondNumber_;     // mainnet=86400, testnet=60.
        releaseTime = 30 * ONE_DAY_SECOND_NUMBER;        // default 30 day.
        luckyIntervalTime = 3 * ONE_DAY_SECOND_NUMBER;   // default 3 day.

        linkedin = linkedin_;
        leader = leader_;
        leaderLucky = leaderLucky_;
        stLTC = stLTC_;

        twoAddress = twoAddress_;
        EternityV1 = EternityV1_;
    }


    event Mining(uint256 pid, address token, address account, uint256 amount, uint256 surplus);
    event UserTake(uint256 pid, address token, address account, uint256 amount);
    event TeamEarn(uint256 pid, address token, uint256 count, uint256 amount);
    event SuperEarn(uint256 pid, address token, address account, address superAddress, uint256 amount);
    event BackendTransferToken(uint256 pid, address token, address user, uint256 amount, uint256 nonce);

    event CreatedAccountID(address account, uint256 userID);
    

    // set releaseTime.
    function setReleaseTime(uint256 dayNumber) external onlyOwner {
        require(dayNumber > 0 && dayNumber <= 100, "Eternity-setReleaseTime: day number error");
        releaseTime = dayNumber * ONE_DAY_SECOND_NUMBER;
    }

    // set allIsOpen.
    function setAllIsOpen(bool newAllIsOpen) external onlyOwner {
        allIsOpen = newAllIsOpen;
    }

    // add pool.
    function addPool(
        address token, 
        address earnToken,
        uint256 leastDepositAmount,
        uint256 luckyPoolEarnLimitAmount,
        uint256 teamEarnLimitAmount,
        uint256 juniorAmount
    ) external onlyOwner {
        require(token != address(0), "Eternity-addPool: token is zaro address error");
        require(earnToken != address(0), "Eternity-addPool: earn token is zaro address error");
        require(leastDepositAmount > 0, "Eternity-addPool: least depoist amount is zero error");
        require(luckyPoolEarnLimitAmount > 0, "Eternity-addPool: lucky pool earn limit amount is zero error");
        _startNewLuckPool(poolInfo.length, 0, address(0));

        // add pool
        poolInfo.push(PoolInfo({
            token: token,
            earnToken: earnToken,
            totalDepositAmount: 0,
            totalStaked: 0,
            leastDepositAmount: leastDepositAmount,
            luckyPoolEarnLimitAmount: luckyPoolEarnLimitAmount,
            nextRewardTime: block.timestamp.add(ONE_DAY_SECOND_NUMBER),
            teamEarnCount: 0,
            teamEarnAmount: 0,
            teamEarnLimitAmount: teamEarnLimitAmount,
            juniorAmount: juniorAmount,
            userTokenPerShare: 0,
            isOpen: true
        }));
    }

    // change pool.
    function changePool(
        uint256 pid, 
        address token, 
        address earnToken,
        uint256 leastDepositAmount, 
        uint256 luckyPoolEarnLimitAmount, 
        uint256 teamEarnLimitAmount, 
        uint256 juniorAmount,
        bool isOpen2
    ) external onlyOwner {
        PoolInfo storage _PoolInfo = poolInfo[pid];
        _PoolInfo.token = token;
        _PoolInfo.earnToken = earnToken;

        _PoolInfo.leastDepositAmount = leastDepositAmount;
        _PoolInfo.luckyPoolEarnLimitAmount = luckyPoolEarnLimitAmount;
        _PoolInfo.teamEarnLimitAmount = teamEarnLimitAmount;
        _PoolInfo.juniorAmount = juniorAmount;
        _PoolInfo.isOpen = isOpen2;
    }
    function changePool2(
        uint256 pid, 
        uint256 totalDepositAmount, 
        uint256 totalStaked,
        uint256 nextRewardTime, 
        uint256 teamEarnCount, 
        uint256 teamEarnAmount, 
        uint256 userTokenPerShare
    ) external onlyOwner {
        PoolInfo storage _PoolInfo = poolInfo[pid];

        _PoolInfo.totalDepositAmount = totalDepositAmount;
        _PoolInfo.totalStaked = totalStaked;
        _PoolInfo.nextRewardTime = nextRewardTime;
        _PoolInfo.teamEarnCount = teamEarnCount;
        _PoolInfo.teamEarnAmount = teamEarnAmount;
        _PoolInfo.userTokenPerShare = userTokenPerShare;
    }

    function setPoolThen(uint256 pid, address recycleToken, uint256 limitDepositAmount, bool isOpen2) external onlyOwner {
        require(recycleToken != address(0), "0 address error");
        require(limitDepositAmount > 0, "0 amount error");

        poolInfoThen[pid].recycleToken = recycleToken;
        poolInfoThen[pid].limitDepositAmount = limitDepositAmount;
        poolInfoThen[pid].isOpen = isOpen2;
    }

    // new a luck pool.
    function _startNewLuckPool(uint256 pid, uint256 amount, address firstAccount) private {
        luckyPoolCount[pid]++;
        LuckyPool storage _LuckyPool = luckyPool[pid][luckyPoolCount[pid]];
        _LuckyPool.earnTotalAmount = amount;
        _LuckyPool.expireTime = block.timestamp.add(luckyIntervalTime);
        _LuckyPool.isState = 1;
        if(firstAccount != address(0)) {
            luckyPoolAccounts[pid][luckyPoolCount[pid]][0] = firstAccount;
            _LuckyPool.luckyNextIndex = 1;
        }
    }

    // get all pool
    function getAllPool() external view returns(PoolInfo[] memory) {
        return poolInfo;
    }

    // git pid all lucky pool
    function getAllLuckyPool(uint256 pid) external view returns(LuckyPool[] memory) {
        uint256 _length = luckyPoolCount[pid];
        LuckyPool[] memory _LuckyPool = new LuckyPool[](_length);
        for(uint256 i = 0; i < _length; i++) {
            _LuckyPool[i] = luckyPool[pid][i+1];
        }
        return _LuckyPool;
    }

    // set luckyIntervalTime.
    function setLuckyIntervalTime(uint256 dayNumber) external onlyOwner {
        require(dayNumber > 0 && dayNumber <= 30, "Eternity-setLuckyIntervalTime: day number error");
        luckyIntervalTime = dayNumber * ONE_DAY_SECOND_NUMBER;
    }

    // set luckyLastAddrssRatio.
    function setLuckyLastAddrssRatio(uint256 newLuckyLastAddrssRatio) external onlyOwner {
        require(newLuckyLastAddrssRatio > 0 && newLuckyLastAddrssRatio < 10000, "Eternity-setLuckyLastAddrssRatio: ratio error");
        luckyLastAddrssRatio = newLuckyLastAddrssRatio;
    }

    // set all ratio.
    function setAllRatio(uint256[7] memory allRatio) external onlyOwner {
        uint256 _total;
        for(uint256 i = 0; i < 7; i++) {
            _total += allRatio[i];
        }
        require(_total == 10000, "Eternity-setAllRatio: ratio total error");
        superRatio[0] = allRatio[0];
        superRatio[1] = allRatio[1];
        superRatio[2] = allRatio[2];
        miningPoolRatio = allRatio[3];
        luckyPoolRatio = allRatio[4];
        teamPoolRatio = allRatio[5];
        leaderRatio = allRatio[6];
    }

    // set earnMul.
    function setEarnMul(uint256 newEarnMul) external onlyOwner {
        require(newEarnMul > 0, "Eternity-setEarnMul: earnMul error");
        earnMul = newEarnMul;
    }

    // set linkedin.
    function setLinkedin(address newLinkedin) external onlyOwner {
        require(newLinkedin != address(0), "Eternity-setLinkedin: address error");
        linkedin = newLinkedin;
    }

    // set leader.
    function setLeader(address newLeader) external onlyOwner {
        require(newLeader != address(0), "Eternity-setLeader: address error");
        leader = newLeader;
    }

    // set leader lucky.
    function setLeaderLucky(address newLeaderLucky) external onlyOwner {
        require(newLeaderLucky != address(0), "Eternity-newLeaderLucky: address error");
        leaderLucky = newLeaderLucky;
    }

    // set stLTC.
    function setStLTC(address newStLTC) external onlyOwner {
        require(newStLTC != address(0), "Eternity-setStLTC: address error");
        stLTC = newStLTC;
    }

    // set blackList
    function setBlackList(address account, bool isBlack) external onlyOwner {
        require(account != address(0), "Eternity-setStLTC: address error");
        blackList[account] = isBlack;
    }

    // set isCanMoveData
    function setIsCanMoveData(bool newIsCanMoveData) external onlyOwner {
        isCanMoveData = newIsCanMoveData;
    }
    
    // git pid lucky pool account.
    function getLuckyPoolAccount(uint256 pid, uint256 luckyPoolID) external view returns(address[LUCKY_SEATS] memory) {
        return luckyPoolAccounts[pid][luckyPoolID];
    }

    // user deposit.
    function deposit(uint256 pid, uint256 amount) external payable nonReentrant nonContract {
        if(isCanMoveData) {
            // if can move data. then only move data.
            _moveData(pid);
            return;
        }

        address msg_sender = msg.sender;
        PoolInfo storage _PoolInfo = poolInfo[pid];
        require(!blackList[msg_sender], "Eternity-deposit: is black list");
        require(allIsOpen, "Eternity-deposit: not all open");
        require(_PoolInfo.isOpen, "Eternity-deposit: not open");
        require(amount >= _PoolInfo.leastDepositAmount, "Eternity-deposit: amount least error");

        if(_PoolInfo.token == ETH) {
            // is bnb.
            require(msg.value == amount, "Eternity-deposit: amount and value is error");
        }else {
            // is token.
            TransferHelper.safeTransferFrom(_PoolInfo.token, msg_sender, address(this), amount);
        }

        // is created order
        if(accountID[pid][msg_sender] == 0) _createdAccountID(pid, msg_sender);

        // amount .
        uint256 minging_amount = amount.mul(miningPoolRatio).div(RATIO_DENOMINATOR);
        uint256 lucky_amount = amount.mul(luckyPoolRatio).div(RATIO_DENOMINATOR);
        uint256 team_amount = amount.mul(teamPoolRatio).div(RATIO_DENOMINATOR);
        uint256 leader_amount = amount.mul(leaderRatio).div(RATIO_DENOMINATOR);

        // user.
        _userEarn(pid, msg_sender, amount, minging_amount);
        // mining.
        _updatePool(pid, minging_amount); // 0.5
        // lucky.
        _luckyPool(pid, lucky_amount, msg_sender);
        // team.
        _teamEarn(pid, team_amount);

        // super.
        if(_PoolInfo.token != stLTC) _superEarn(pid, msg_sender, amount);

        // leader.
        if(leader_amount > 0) _transferTokenOrETH(_PoolInfo.earnToken, leader, leader_amount);
    }

    // mass update pools
    function massUpdatePools() external nonReentrant nonContract {
        uint256 _length = poolInfo.length;
        for (uint256 i = 0; i < _length; i++) {
            _updatePool(i, 0);
        }
    }

    // update pool
    function updatePool(uint256 pid) external nonReentrant nonContract {
        _updatePool(pid, 0);
    }

    function _updatePool(uint256 pid, uint256 amount) private {
        PoolInfo storage _PoolInfo = poolInfo[pid];

        amount = amount / 2;
        if (block.timestamp <= _PoolInfo.nextRewardTime) {
            // not reward time || not reward
            _PoolInfo.totalStaked += amount;
            _PoolInfo.totalDepositAmount += amount;
            return;
        }
        if(_PoolInfo.totalStaked != 0 && _PoolInfo.totalDepositAmount != 0) {
            uint256 add_per = _PoolInfo.totalStaked.mul(SCALING_FACTOR).div(_PoolInfo.totalDepositAmount);
            _PoolInfo.userTokenPerShare += add_per;
        }
        _PoolInfo.totalStaked = amount;
        _PoolInfo.totalDepositAmount += amount;
        _PoolInfo.nextRewardTime = block.timestamp.add(ONE_DAY_SECOND_NUMBER);
    }

    // pending token
    function pendingToken(uint256 pid, address account) public view returns (uint256, uint256) {
        PoolInfo memory _PoolInfo = poolInfo[pid];
        uint256 _ID = accountID[pid][account];
        UserInfo memory _UserInfo = userInfo[pid][_ID];
        if(_UserInfo.depositAmount == 0) return (0, 0);
        uint256 amount = _UserInfo.depositAmount.mul(_PoolInfo.userTokenPerShare).div(SCALING_FACTOR).sub(_UserInfo.rewardDebt);
        amount = amount + _UserInfo.residueAmount;
        if(amount == 0 ) return (0, 0);

        // 30 day release dividend.
        if(block.timestamp >= _UserInfo.releaseEndTime) {
            return (amount, amount);
        }
        uint256 _totalReleaseTime = _UserInfo.releaseEndTime - _UserInfo.releaseStartTime;
        uint256 _perTimeAmount = amount.mul(1e10).div(_totalReleaseTime);
        uint256 take_amount = block.timestamp.sub(_UserInfo.releaseStartTime).mul(_perTimeAmount).div(1e10);
        // amount=total eran. take_amount=now can take earn.
        return (amount, take_amount);
    }

    function _userTake(uint256 pid, address account) private {
        uint256 _ID = accountID[pid][account];
        UserInfo storage _UserInfo = userInfo[pid][_ID];
        (uint256 _amount, uint256 take_amount) = pendingToken(pid, account);
        take_amount = _UserInfo.takedAmount + take_amount > _UserInfo.takeLimitAmount ? _UserInfo.takeLimitAmount - _UserInfo.takedAmount : take_amount;

        if(take_amount > 0) _transferTokenOrETH(poolInfo[pid].earnToken, account, take_amount);
        _UserInfo.takedAmount += take_amount;
        _UserInfo.residueAmount = _amount.sub(take_amount);
        emit UserTake(pid, poolInfo[pid].earnToken, account, take_amount);
    }

    // user earn.
    function _userEarn(uint256 pid, address account, uint256 amount, uint256 depositAmount) private {
        _userTake(pid, account);

        uint256 _ID = accountID[pid][account];
        UserInfo storage _UserInfo = userInfo[pid][_ID];
        _UserInfo.takeLimitAmount += amount.mul(earnMul);
        uint256 _partAmount = depositAmount / 2;
        _UserInfo.depositAmount += _partAmount;
        _UserInfo.residueAmount += _partAmount;
        _UserInfo.rewardDebt = _UserInfo.depositAmount.mul(poolInfo[pid].userTokenPerShare).div(SCALING_FACTOR);
        _UserInfo.releaseStartTime = block.timestamp;
        _UserInfo.releaseEndTime = block.timestamp + releaseTime;

        emit Mining(pid, poolInfo[pid].token, account, depositAmount, _UserInfo.depositAmount);
        // verify limit depost.
        require(_UserInfo.depositAmount <= poolInfoThen[pid].limitDepositAmount, "deposit limit amount");
    }

    // lucky pool.
    function _luckyPool(uint256 pid, uint256 amount, address account) private {
        LuckyPool storage _LuckyPool = luckyPool[pid][luckyPoolCount[pid]];

        // is expire.
        if(block.timestamp > _LuckyPool.expireTime) {
            // now lucky pool is poised. start new lucky pool.
            _LuckyPool.isState = 2;
            _startNewLuckPool(pid, amount, account);
        }else {
            // add pool amount. lengthen time. add lucky account address.
            _LuckyPool.earnTotalAmount = _LuckyPool.earnTotalAmount.add(amount);
            _LuckyPool.expireTime = block.timestamp.add(luckyIntervalTime);
            uint256 _nowLuckyIndex = _LuckyPool.luckyNextIndex >= LUCKY_SEATS ? 0 : _LuckyPool.luckyNextIndex;
            luckyPoolAccounts[pid][luckyPoolCount[pid]][_nowLuckyIndex] = account;
            _LuckyPool.luckyNextIndex = _nowLuckyIndex + 1;
            if(_LuckyPool.earnTotalAmount >= poolInfo[pid].luckyPoolEarnLimitAmount) {
                // take 50%.
                uint256 _newEarnTotalAmount = _LuckyPool.earnTotalAmount.div(2);
                if(_newEarnTotalAmount > 0) _transferTokenOrETH(poolInfo[pid].earnToken, leaderLucky, _newEarnTotalAmount);
                _LuckyPool.earnTotalAmount = _newEarnTotalAmount;
            }
        }
    }

    // team earn.
    function _teamEarn(uint256 pid, uint256 amount) private {
        PoolInfo storage _PoolInfo = poolInfo[pid];
        _PoolInfo.teamEarnAmount += amount;
        if(_PoolInfo.teamEarnAmount >= _PoolInfo.teamEarnLimitAmount) {
            _PoolInfo.teamEarnCount++;
            emit TeamEarn(pid, _PoolInfo.earnToken, _PoolInfo.teamEarnCount, _PoolInfo.teamEarnAmount);
            _PoolInfo.teamEarnAmount = 0;
        }
    }

    // super earn.
    function _superEarn(uint256 pid, address account, uint256 amount) private {
        address[] memory super_three  = ILinkedin(linkedin).getSuperList(account, 3);
        require(super_three.length == 3, "Eternity-_superEarn: super list error");
        address _earnToken = poolInfo[pid].earnToken;

        // update condition.
        _juniorRecord(pid, account, super_three[0]);

        uint256 super_amount;
        for(uint256 i = 0; i < 3; i++) {
            super_amount =  amount.mul(superRatio[i]).div(RATIO_DENOMINATOR);
            if(super_three[i] != address(0)) {
                if(_superTakeVerify(pid, super_three[i], i)) {
                    uint256 _ID = accountID[pid][super_three[i]];
                    UserInfo storage _UserInfo = userInfo[pid][_ID];
                    super_amount = _UserInfo.takedAmount + super_amount > _UserInfo.takeLimitAmount ? _UserInfo.takeLimitAmount - _UserInfo.takedAmount : super_amount;
                    _UserInfo.takedAmount = _UserInfo.takedAmount + super_amount;
                    if(super_amount > 0) _transferTokenOrETH(_earnToken, super_three[i], super_amount);
                    emit SuperEarn(pid, _earnToken, account, super_three[i], super_amount);
                }
            }
        }
    }

    // is min.
    function _isMin(uint256 a, uint256 b, uint256 c) private pure returns(uint256) {
        if (a <= b && a <= c) {
            return a;
        } else if (b <= a && b <= c) {
            return b;
        } else {
            return c;
        }
    }

    // junior record.
    function _juniorRecord(uint256 pid, address account, address superAddress) private {
        uint256 _ID = accountID[pid][account];
        UserInfo memory _UserInfo = userInfo[pid][_ID];
        JuniorMsg storage _JuniorMsg = juniorMsg[pid][superAddress];

        uint256 min_number = _isMin(_JuniorMsg.oneAmount, _JuniorMsg.twoAmount, _JuniorMsg.threeAmount);
        if(_UserInfo.depositAmount <= min_number) return;
        if(account == _JuniorMsg.oneJunior) {
            _JuniorMsg.oneAmount = _UserInfo.depositAmount;
        }else if(account == _JuniorMsg.twoJunior) {
            _JuniorMsg.twoAmount = _UserInfo.depositAmount;
        }else if(account == _JuniorMsg.threeJunior) {
            _JuniorMsg.threeAmount = _UserInfo.depositAmount;
        }else {
            if(_JuniorMsg.oneAmount == min_number) {
                _JuniorMsg.oneJunior = account;
                _JuniorMsg.oneAmount = _UserInfo.depositAmount;
            }else if(_JuniorMsg.twoAmount == min_number) {
                _JuniorMsg.twoJunior = account;
                _JuniorMsg.twoAmount = _UserInfo.depositAmount;
            }else {
                _JuniorMsg.threeJunior = account;
                _JuniorMsg.threeAmount = _UserInfo.depositAmount;
            }
        }
    }

    // super take verify.
    // rank is 0-2, corresponding 1-3.
    function _superTakeVerify(uint256 pid, address superAddress, uint256 rankIndex) private view returns(bool) {
        PoolInfo memory _PoolInfo = poolInfo[pid];
        JuniorMsg memory _JuniorMsg = juniorMsg[pid][superAddress];
        uint256 _amount = _PoolInfo.juniorAmount;
        uint256 _count = 0;
        if(_JuniorMsg.oneAmount >= _amount) {
            _count++;
        }
        if(_JuniorMsg.twoAmount >= _amount) {
            _count++;
        }
        if(_JuniorMsg.threeAmount >= _amount) {
            _count++;
        }

        if(rankIndex == 0) {
            return _count >= 1;
        }else if(rankIndex == 1) {
            return _count >= 2;
        }else {
            return _count >= 3;
        }
    }

    // user withdraw.
    function withdraw(uint256 pid) external nonReentrant nonContract {
        address msg_sender = msg.sender;
        require(allIsOpen, "Eternity-withdraw: not all open");
        require(!blackList[msg_sender], "Eternity-withdraw: is black list");
        _userTake(pid, msg_sender);
        uint256 _ID = accountID[pid][msg_sender];
        UserInfo storage _UserInfo = userInfo[pid][_ID];
        
        _UserInfo.rewardDebt = _UserInfo.depositAmount.mul(poolInfo[pid].userTokenPerShare).div(SCALING_FACTOR);
        _UserInfo.releaseStartTime = block.timestamp;
    }

    // anybody can give lucky pool earn
    function anybodyLuckyPoolEarn(uint256 pid, uint256 luckyPoolID) external nonReentrant nonContract {
        LuckyPool storage _LuckyPool = luckyPool[pid][luckyPoolID];
        require(_LuckyPool.isState == 2, "Eternity-anybodyLuckyPoolEarn: state error");
        require(_LuckyPool.earnTotalAmount > 0, "Eternity-anybodyLuckyPoolEarn: amount1 error");
        _LuckyPool.isState = 3;

        address _earnToken = poolInfo[pid].earnToken;
        // luckyLastAddrssRatio
        uint256 _soLuckyIndex = _LuckyPool.luckyNextIndex == 0 ? LUCKY_SEATS - 1 : _LuckyPool.luckyNextIndex - 1;
        uint256 _soLuckyAmount = _LuckyPool.earnTotalAmount.mul(luckyLastAddrssRatio).div(RATIO_DENOMINATOR);
        uint256 _luckAmount = _LuckyPool.earnTotalAmount.sub(_soLuckyAmount).div((LUCKY_SEATS - 1));
        require(_soLuckyAmount > 0, "Eternity-anybodyLuckyPoolEarn: amount2 error");
        require(_luckAmount > 0, "Eternity-anybodyLuckyPoolEarn: amount3 error");

        address[LUCKY_SEATS] memory _luckyAccounts = luckyPoolAccounts[pid][luckyPoolID];
        for(uint256 i = 0; i < _luckyAccounts.length; i++) {
            if(i == _soLuckyIndex) {
                if(_luckyAccounts[i] != address(0) && _soLuckyAmount > 0) _transferTokenOrETH(_earnToken, _luckyAccounts[i], _soLuckyAmount);
            }else {
                if(_luckyAccounts[i] != address(0) && _luckAmount > 0) _transferTokenOrETH(_earnToken, _luckyAccounts[i], _luckAmount);
            }
        }
    }
    

    // set twoAddress_.
    function setTwoAddress(address newTwoAddress_) public onlyOwner {
        require(newTwoAddress_ != address(0), "0 address error");
        twoAddress = newTwoAddress_;
    }

    // set nonce is true
    function setNonceUsed(uint256 nonce) public onlyOwner {
        nonceUsed[nonce] = true;
    }

    // sign take token.
    // data: (this contract address, pid, token address, user address, token amount, random nonce).
    function backendTransferToken(uint256 _pid,address _token,address _to,uint256 _amount,uint256 _nonce,bytes memory _signature) external nonReentrant nonContract {
        require(allIsOpen, "not is open");
        // only myself
        require(msg.sender == _to, "not you");
        //require(!blackList[_to], "Eternity-backendTransferToken: is black list");

        bytes32 hashData = keccak256(abi.encodePacked(address(this),_pid,_token,_to,_amount,_nonce));
        bytes32 messageHash = keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hashData));
        address signer = recoverSigner(messageHash, _signature);
        require(signer == twoAddress, "Signer is not two address");
        require(signer != address(0), "Signer is 0 address");
        require(!nonceUsed[_nonce], "nonce used");
        nonceUsed[_nonce] = true;

        // transfer
        _transferTokenOrETH(_token, _to, _amount);
        // emit
        emit BackendTransferToken(_pid, _token, _to, _amount, _nonce);

        // amount limit
        uint256 _ID = accountID[_pid][_to];
        UserInfo storage _UserInfo = userInfo[_pid][_ID];
        require(_UserInfo.takeLimitAmount >= _UserInfo.takedAmount.add(_amount), "Eternity-backendTransferToken: earm limit error");
        _UserInfo.takedAmount = _UserInfo.takedAmount.add(_amount);
    }

    function recoverSigner(bytes32 message, bytes memory sig) internal pure returns (address) {
        (uint8 v, bytes32 r, bytes32 s) = splitSignature(sig);
        return ecrecover(message, v, r, s);
    }

    function splitSignature(bytes memory sig) internal pure returns (uint8 v, bytes32 r, bytes32 s) {
        require(sig.length == 65);
        assembly {
            r := mload(add(sig, 32))
            s := mload(add(sig, 64))
            v := byte(0, mload(add(sig, 96)))
        }
        return (v, r, s);
    }

    // take token.
    function takeToken(address _token, address _to , uint256 _value) external onlyOwner {
        require(_to != address(0), "zero address error");
        require(_value > 0, "value zero error");
        TransferHelper.safeTransfer(_token, _to, _value);
    }

    // receive eth
    receive() external payable {}

    // take eth.
    function takeETH(address _to, uint256 _value) public onlyOwner {
        require(_to != address(0), "zero address error");
        require(_value > 0, "value zero error");
        TransferHelper.safeTransferETH(_to, _value);
    }

    // transfer 
    function _transferTokenOrETH(address _token, address _to, uint256 _value) private {
        if(_token == ETH) {
            // if 0xeeee.. transfer bnb
            TransferHelper.safeTransferETH(_to, _value);
        }else {
            TransferHelper.safeTransfer(_token, _to, _value);
        }
    }

    // created account ID
    function _createdAccountID(uint256 _pid, address _account) private returns(uint256) {
        totalID++;
        accountID[_pid][_account] = totalID;
        userInfo[_pid][totalID].ownerOf = _account;

        emit CreatedAccountID(_account, totalID);
        return totalID;
    }

    // account ID transfer
    function transferAccountID(uint256 _pid, address _newOwnerOf) public nonReentrant nonContract {
        require(!blackList[msg.sender], "Eternity-transferAccountID: is black list");
        require(allIsOpen, "Eternity-transferAccountID: not all open");
        

        address msg_sender = msg.sender;
        uint256 _ID = accountID[_pid][msg_sender];
        UserInfo storage _UserInfo = userInfo[_pid][_ID];
        require(_ID != 0, "Eternity-transferAccountID: zero error");
        require(_UserInfo.ownerOf == msg_sender, "Eternity-transferAccountID: not your");

        uint256 _ID2 = accountID[_pid][_newOwnerOf];
        require(_ID2 == 0, "Eternity-transferAccountID: zero error 2"); // new owner must not order

        // transfer account ID
        userInfo[_pid][_ID].ownerOf = _newOwnerOf;
        accountID[_pid][_newOwnerOf] = _ID;
        delete accountID[_pid][msg_sender];
    }

    // recycleToken
    function recycleOrder(uint256 _pid) public nonReentrant nonContract {
        require(!blackList[msg.sender], "Eternity-recycleOrder: is black list");
        require(allIsOpen, "Eternity-recycleOrder not all open");

        address msg_sender = msg.sender;
        uint256 _ID = accountID[_pid][msg_sender];
        UserInfo storage _UserInfo = userInfo[_pid][_ID];
        require(_ID != 0, "Eternity-recycleOrder: zero error");
        require(_UserInfo.ownerOf == msg_sender, "Eternity-recycleOrder: not your");

        // (uint256 _amount, ) = pendingToken(_pid, msg_sender);
        uint256 _amount = _UserInfo.takeLimitAmount - _UserInfo.takedAmount;
        // pool sub.
        PoolInfo storage _PoolInfo = poolInfo[_pid];
        _PoolInfo.totalDepositAmount = _PoolInfo.totalDepositAmount - _UserInfo.depositAmount;
        // delete order
        delete userInfo[_pid][_ID];
        delete accountID[_pid][msg_sender];

        // transfer
        PoolInfoThen memory _PoolInfoThen = poolInfoThen[_pid];
        require(_PoolInfoThen.isOpen, "Eternity-recycleOrder: not open then");
        require(_PoolInfoThen.recycleToken != address(0), "Eternity-recycleOrder: zero address then");
        _transferTokenOrETH(_PoolInfoThen.recycleToken, msg_sender, _amount);
    }

    // owner set account order.
    function setAccountOrder(uint256 _pid, address _account, uint256 _takeLimitAmount, uint256 _residueAmount, uint256 _releaseEndTime) public onlyOwner {
        require(_account != address(0), "zero address error");
        uint256 _ID = accountID[_pid][_account];
        UserInfo storage _UserInfo = userInfo[_pid][_ID];
        require(_ID != 0, "Eternity-setAccountOrder: zero error");
        require(_UserInfo.ownerOf == _account, "Eternity-setAccountOrder: not your");
        require(_releaseEndTime >= block.timestamp, "Eternity-setAccountOrder: time error");
        
        _UserInfo.takeLimitAmount  = _takeLimitAmount;
        _UserInfo.residueAmount = _residueAmount;
        _UserInfo.releaseEndTime = _releaseEndTime;
    }

    // move data
    function _moveData(uint256 pid) private {
        require(isCanMoveData, "Eternity-moveData: not move");
        require(poolInfo[pid].isOpen, "Eternity-moveData: pool not open");
        address msg_sender = msg.sender;
        UserInfoV1 memory _UserInfoV1 = IEternityV1(EternityV1).userInfo(pid, msg_sender);

        uint256 _ID = accountID[pid][msg_sender];
        require(_ID == 0, "Eternity-moveData: must not id");
        uint256 _newID = _createdAccountID(pid, msg_sender);
        // deposit
        UserInfo storage _UserInfo = userInfo[pid][_newID];
        _UserInfo.depositAmount = _UserInfoV1.depositAmount;
        _UserInfo.takedAmount = _UserInfoV1.takedAmount;
        _UserInfo.takeLimitAmount = _UserInfoV1.takeLimitAmount;
        _UserInfo.rewardDebt = _UserInfoV1.rewardDebt;
        _UserInfo.residueAmount = _UserInfoV1.residueAmount;
        _UserInfo.releaseStartTime = _UserInfoV1.releaseStartTime;
        _UserInfo.releaseEndTime = _UserInfoV1.releaseEndTime;

        // junior msg
        JuniorMsgV1 memory _JuniorMsgV1 = IEternityV1(EternityV1).juniorMsg(pid, msg_sender);
        JuniorMsg storage _JuniorMsg = juniorMsg[pid][msg_sender];
        _JuniorMsg.oneJunior = _JuniorMsgV1.oneJunior;
        _JuniorMsg.twoJunior = _JuniorMsgV1.twoJunior;
        _JuniorMsg.threeJunior = _JuniorMsgV1.threeJunior;
        _JuniorMsg.oneAmount = _JuniorMsgV1.oneAmount;
        _JuniorMsg.twoAmount = _JuniorMsgV1.twoAmount;
        _JuniorMsg.threeAmount = _JuniorMsgV1.threeAmount;
    } 

}