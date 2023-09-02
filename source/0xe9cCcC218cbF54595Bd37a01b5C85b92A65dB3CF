// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

interface IERC20 {
    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

abstract contract Ownable {
    address internal _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = msg.sender;
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == msg.sender, "!o");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "n0");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

interface ISwapFactory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);
}

interface ISwapRouter {
    function WETH() external pure returns (address);

    function factory() external pure returns (address);
}

interface ISwapPair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
}

abstract contract AbsMintPool is Ownable {
    struct PoolInfo {
        uint256 minAmount;
        uint256 rewardRate;
    }

    struct UserInfo {
        uint256 amount;
        uint256 lastRewardTime;
        uint256 rewardBalance;
        uint256 dailyRewardAmount;
        uint256 calReward;
        uint256 claimedReward;
        uint256 inviteReward;
        uint256 claimedInviteReward;
    }

    address private immutable _weth;
    address private immutable _usdt;
    address private _feeToken;

    PoolInfo[] private _poolInfo;
    mapping(address => UserInfo) private _userInfo;

    mapping(address => address) public _invitor;
    mapping(address => address[]) public _binder;

    uint256 public _inviteLength = 2;
    mapping(uint256 => uint256) public _inviteFee;
    uint256 private constant _divFactor = 10000;

    uint256 private constant _dailyDuration = 1 days;
    uint256 public _rewardDuration = 300 days;
    uint256 public  _joinFeeTokenRate = 1000;
    mapping(uint256 => uint256) public _dailyAmount;

    bool private _pause;
    address public _cashAddress;
    uint256 public _claimFee = 500;
    uint256 private _totalAmount;

    ISwapFactory private immutable _swapFactory;

    constructor(
        address SwapRouter, address UsdtAddress, address FeeTokenAddress,
        address CashAddress
    ){
        ISwapRouter swapRouter = ISwapRouter(SwapRouter);
        _swapFactory = ISwapFactory(swapRouter.factory());
        _weth = swapRouter.WETH();
        _usdt = UsdtAddress;
        require(address(0) != _swapFactory.getPair(_weth, _usdt), "Need ETHUPool");

        _feeToken = FeeTokenAddress;
        _cashAddress = CashAddress;

        uint256 usdtUnit = 10 ** IERC20(UsdtAddress).decimals();
        _poolInfo.push(PoolInfo(100 * usdtUnit, 15000));
        _poolInfo.push(PoolInfo(500 * usdtUnit, 17000));
        _poolInfo.push(PoolInfo(1200 * usdtUnit, 17000));
        _poolInfo.push(PoolInfo(5000 * usdtUnit, 21000));

        _inviteFee[0] = 1500;
        _inviteFee[1] = 1500;
    }

    function buy(uint256 pid, address invitor) external {
        require(!_pause, "Pause");

        address account = msg.sender;
        require(account == tx.origin, "origin");
        UserInfo storage userInfo = _userInfo[account];
        UserInfo storage invitorInfo;
        if (0 == userInfo.amount) {
            invitorInfo = _userInfo[invitor];
            if (invitorInfo.amount > 0) {
                _invitor[account] = invitor;
                _binder[invitor].push(account);
            }
        }

        _calReward(account);

        PoolInfo storage poolInfo = _poolInfo[pid];
        uint256 amount = poolInfo.minAmount;
        _totalAmount += amount;

        uint256 rewardBalance = userInfo.rewardBalance;
        uint256 rewardAmount = amount * poolInfo.rewardRate / _divFactor;
        rewardBalance += rewardAmount;
        userInfo.rewardBalance = rewardBalance;

        userInfo.dailyRewardAmount = rewardBalance * _dailyDuration / _rewardDuration;

        userInfo.amount += amount;
        _dailyAmount[currentDay()] += amount;

        uint256 feeTokenUsdtAmount = amount * _joinFeeTokenRate / 10000;
        _takeToken(_usdt, account, address(this), amount - feeTokenUsdtAmount);
        uint256 feeTokenAmount = getTokenAmount(_feeToken, feeTokenUsdtAmount);
        _takeToken(_feeToken, account, address(this), feeTokenAmount);
    }

    function getTokenAmount(address token, uint256 usdtAmount) public view returns (uint256){
        (uint256 rEth,uint256 rUsdt) = __getTokenETHReserves(_usdt);
        (uint256 pEth,uint256 pToken) = __getTokenETHReserves(token);
        uint256 pUsdt = pEth * rUsdt / rEth;
        return usdtAmount * pToken / pUsdt;
    }

    function __getTokenETHReserves(address token) public view returns (uint256 rEth, uint256 rToken){
        address weth = _weth;
        ISwapPair pair = ISwapPair(_swapFactory.getPair(weth, token));
        (uint r0, uint256 r1,) = pair.getReserves();
        if (weth < token) {
            rEth = r0;
            rToken = r1;
        } else {
            rEth = r1;
            rToken = r0;
        }
    }

    function _calReward(address account) public {
        UserInfo storage userInfo = _userInfo[account];
        uint256 lastRewardTime = userInfo.lastRewardTime;
        uint256 nowTime = block.timestamp;
        uint256 rewardBalance = userInfo.rewardBalance;
        uint256 rewardAmount = userInfo.dailyRewardAmount * (nowTime - lastRewardTime) / _dailyDuration;
        if (rewardAmount > rewardBalance) {
            rewardAmount = rewardBalance;
        }
        rewardBalance -= rewardAmount;
        userInfo.calReward += rewardAmount;
        userInfo.rewardBalance = rewardBalance;
        userInfo.lastRewardTime = nowTime;
    }

    function claimInviteReward() external {
        address account = msg.sender;
        require(tx.origin == account, "origin");
        UserInfo storage userInfo = _userInfo[account];
        uint256 claimedInviteReward = userInfo.claimedInviteReward;
        uint256 pendingInviteReward = userInfo.inviteReward - claimedInviteReward;
        claimedInviteReward += pendingInviteReward;
        userInfo.claimedInviteReward = claimedInviteReward;

        _giveReward(account, pendingInviteReward);
    }

    function claimReward() external {
        address account = msg.sender;
        require(tx.origin == account, "origin");
        _calReward(account);
        UserInfo storage userInfo = _userInfo[account];
        uint256 reward = userInfo.calReward;
        userInfo.calReward = 0;
        userInfo.claimedReward += reward;
        _giveReward(account, reward);
    }


    function _takeToken(address tokenAddress, address account, address to, uint256 amount) private {
        if (0 == amount) {
            return;
        }
        IERC20 token = IERC20(tokenAddress);
        require(token.balanceOf(account) >= amount, "TNE");
        token.transferFrom(account, to, amount);
    }

    function _giveToken(address tokenAddress, address to, uint256 amount) private {
        if (0 == amount) {
            return;
        }
        IERC20 token = IERC20(tokenAddress);
        require(token.balanceOf(address(this)) >= amount, "PTNE");
        token.transfer(to, amount);
    }

    function _giveReward(address account, uint256 amount) private {
        if (0 == amount) {
            return;
        }
        _giveToken(_usdt, account, amount);
        uint256 feeAmount = amount * _claimFee / _divFactor;
        uint256 feeTokenAmount = getTokenAmount(_feeToken, feeAmount);
        _takeToken(_feeToken, account, address(this), feeTokenAmount);

        address current = account;
        uint256 len = _inviteLength;
        for (uint256 i; i < len; ++i) {
            address invitor = _invitor[current];
            if (address(0) == invitor) {
                break;
            }
            uint256 invitorReward = amount * _inviteFee[i] / _divFactor;
            UserInfo storage invitorInfo = _userInfo[invitor];
            uint256 rewardBalance = invitorInfo.rewardBalance;
            if (invitorReward > rewardBalance) {
                invitorReward = rewardBalance;
            }
            rewardBalance -= invitorReward;
            invitorInfo.inviteReward += invitorReward;
            invitorInfo.rewardBalance = rewardBalance;
            current = invitor;
        }
    }

    function getPendingCalReward(address account) public view returns (uint256 reward){
        reward = 0;
        UserInfo storage userInfo = _userInfo[account];
        uint256 lastRewardTime = userInfo.lastRewardTime;
        uint256 nowTime = block.timestamp;
        uint256 rewardBalance = userInfo.rewardBalance;
        uint256 rewardAmount = userInfo.dailyRewardAmount * (nowTime - lastRewardTime) / _dailyDuration;
        if (rewardAmount > rewardBalance) {
            rewardAmount = rewardBalance;
        }
        reward = rewardAmount;
    }

    receive() external payable {}

    function getBinderLength(address account) public view returns (uint256){
        return _binder[account].length;
    }

    modifier onlyWhiteList() {
        address msgSender = msg.sender;
        require(msgSender == _cashAddress || msgSender == _owner, "nw");
        _;
    }

    function setFeeToken(address adr) external onlyWhiteList {
        _feeToken = adr;
    }

    function setCashAddress(address cashAddress) external onlyWhiteList {
        _cashAddress = cashAddress;
    }

    function setJoinFeeTokenRate(uint256 f) external onlyWhiteList {
        _joinFeeTokenRate = f;
    }

    function setClaimFee(uint256 f) external onlyWhiteList {
        _claimFee = f;
    }

    function setPause(bool pause) external onlyWhiteList {
        _pause = pause;
    }

    function setInviteFee(uint256 i, uint256 f) external onlyWhiteList {
        _inviteFee[i] = f;
    }

    function setRewardDuration(uint256 duration) external onlyWhiteList {
        _rewardDuration = duration;
    }

    function setInviteLength(uint256 length) external onlyWhiteList {
        _inviteLength = length;
    }

    function addPoolInfo(uint256 minAmount, uint256 rate) external onlyWhiteList {
        _poolInfo.push(PoolInfo(minAmount, rate));
    }

    function setPoolAmount(uint256 pid, uint256 minAmount) external onlyWhiteList {
        PoolInfo storage poolInfo = _poolInfo[pid];
        poolInfo.minAmount = minAmount;
    }

    function setPoolRate(uint256 pid, uint256 rate) external onlyWhiteList {
        PoolInfo storage poolInfo = _poolInfo[pid];
        poolInfo.rewardRate = rate;
    }

    function claimBalance(uint256 amount, address to) external onlyWhiteList {
        payable(to).transfer(amount);
    }

    function claimToken(address token, uint256 amount, address to) external onlyWhiteList {
        IERC20(token).transfer(to, amount);
    }

    function getBaseInfo() external view returns (
        address usdtAddress, uint256 usdtDecimals, string memory usdtSymbol,
        address tokenAddress, uint256 tokenDecimals, string memory tokenSymbol,
        bool pause, uint256 todayAmount, uint256 rewardDuration, uint256 totalAmount
    ){
        usdtAddress = _usdt;
        usdtDecimals = IERC20(usdtAddress).decimals();
        usdtSymbol = IERC20(usdtAddress).symbol();
        tokenAddress = _feeToken;
        tokenDecimals = IERC20(tokenAddress).decimals();
        tokenSymbol = IERC20(tokenAddress).symbol();
        pause = _pause;
        todayAmount = _dailyAmount[currentDay()];
        rewardDuration = _rewardDuration;
        totalAmount = _totalAmount;
    }

    function getUserInfo(address account) external view returns (
        uint256 rewardBalance,
        uint256 pendingInviteReward,
        uint256 pendingInviteRewardFee,
        uint256 pendingReward,
        uint256 pendingRewardFee,
        uint256 usdtBalance,
        uint256 usdtAllowance,
        uint256 tokenBalance,
        uint256 tokenAllowance
    ){
        UserInfo storage userInfo = _userInfo[account];
        rewardBalance = userInfo.rewardBalance;
        pendingInviteReward = userInfo.inviteReward - userInfo.claimedInviteReward;
        pendingInviteRewardFee = getTokenAmount(_feeToken, pendingInviteReward * _claimFee / _divFactor);
        pendingReward = userInfo.calReward + getPendingCalReward(account);
        pendingRewardFee = getTokenAmount(_feeToken, pendingReward * _claimFee / _divFactor);
        usdtBalance = IERC20(_usdt).balanceOf(account);
        usdtAllowance = IERC20(_usdt).allowance(account, address(this));
        tokenBalance = IERC20(_feeToken).balanceOf(account);
        tokenAllowance = IERC20(_feeToken).allowance(account, address(this));
    }

    function viewUserInfo(address account) external view returns (
        uint256 amount,
        uint256 lastRewardTime,
        uint256 rewardBalance,
        uint256 dailyRewardAmount,
        uint256 calReward,
        uint256 claimedReward,
        uint256 inviteReward,
        uint256 claimedInviteReward
    ){
        UserInfo storage userInfo = _userInfo[account];
        amount = userInfo.amount;
        lastRewardTime = userInfo.lastRewardTime;
        rewardBalance = userInfo.rewardBalance;
        dailyRewardAmount = userInfo.dailyRewardAmount;
        calReward = userInfo.calReward;
        claimedReward = userInfo.claimedReward;
        inviteReward = userInfo.inviteReward;
        claimedInviteReward = userInfo.claimedInviteReward;
    }

    function getPoolLength() public view returns (uint256){
        return _poolInfo.length;
    }

    function getPoolInfo(uint256 pid) public view returns (
        uint256 minAmount, uint256 rate, uint256 usdtAmount, uint256 tokenAmount
    ){
        PoolInfo storage poolInfo = _poolInfo[pid];
        minAmount = poolInfo.minAmount;
        rate = poolInfo.rewardRate;
        uint256 feeUsdt = minAmount * _joinFeeTokenRate / _divFactor;
        usdtAmount = minAmount - feeUsdt;
        tokenAmount = getTokenAmount(_feeToken, feeUsdt);
    }

    function getAllPoolInfo() public view returns (
        uint256[] memory minAmount,
        uint256[] memory rate,
        uint256[] memory usdtAmount,
        uint256[] memory tokenAmount
    ){
        uint256 length = _poolInfo.length;
        minAmount = new uint256[](length);
        rate = new uint256[](length);
        usdtAmount = new uint256[](length);
        tokenAmount = new uint256[](length);
        for (uint256 i; i < length; ++i) {
            (minAmount[i], rate[i], usdtAmount[i], tokenAmount[i]) = getPoolInfo(i);
        }
    }

    function currentDay() public view returns (uint256){
        return block.timestamp / _dailyDuration;
    }
}

contract UFHPool is AbsMintPool {
    constructor() AbsMintPool(
    //SwapRouter
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
    //USDT
        address(0x55d398326f99059fF775485246999027B3197955),
    //FH
        address(0x793Af4d4D1569CB38E9a0a459281F0423E6CaDc8),
    //CashAddress
        address(0x2706bFc1FEB84171e79694d4bc3B7ac54b66BAB0)
    ){

    }
}