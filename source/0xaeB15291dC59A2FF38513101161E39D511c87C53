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

interface ISwapRouter {
    function WETH() external pure returns (address);

    function factory() external pure returns (address);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
}

interface ISwapFactory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);
}

interface ISwapPair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
}

abstract contract AbsLPPool is Ownable {
    struct UserInfo {
        bool isActive;
        uint256 amount;
        uint256 rewardMintDebt;
        uint256 calMintReward;
    }

    struct PoolInfo {
        uint256 totalAmount;
        uint256 accMintPerShare;
        uint256 accMintReward;
        uint256 mintPerSec;
        uint256 lastMintTime;
        uint256 totalMintReward;
    }

    PoolInfo private poolInfo;
    mapping(address => UserInfo) private userInfo;

    ISwapRouter private immutable _swapRouter;
    ISwapFactory private immutable _swapFactory;
    address private immutable _weth;
    address private immutable _usdt;

    uint256 private _minAmount;
    address private _mintRewardToken;
    address[] private _joinTokens;

    function addJoinToken(address token) external onlyWhiteList {
        _joinTokens.push(token);
        safeApprove(token, address(_swapRouter), ~uint256(0));
    }

    function setJoinToken(uint256 i, address token) external onlyWhiteList {
        _joinTokens[i] = token;
        safeApprove(token, address(_swapRouter), ~uint256(0));
    }

    function setJoinTokens(address[] memory tokens) external onlyWhiteList {
        _joinTokens = tokens;
        uint256 len = tokens.length;
        for (uint256 i; i < len; ++i) {
            safeApprove(tokens[i], address(_swapRouter), ~uint256(0));
        }
    }

    mapping(address => address) public _invitor;
    mapping(address => address[]) public _binder;
    mapping(uint256 => uint256) public _inviteFee;
    uint256 private constant _inviteLen = 2;
    address private _defaultInvitor;
    mapping(address => uint256) private _teamNum;

    mapping(address => uint256) private _inviteAmount;

    address public _fundAddress;

    function setFundAddress(address a) external onlyWhiteList {
        _fundAddress = a;
    }

    bool private _pauseJoin = true;
    uint256 public _lastDailyUpTime;
    uint256 public _lastAmountRate = 10100;
    uint256 public _amountDailyUp = 100;
    uint256 private constant _divFactor = 10000;
    uint256 private constant _dailyDuration = 1 days;

    uint256 public _sellRate = 7000;
    address public _burnReceiver = address(0x000000000000000000000000000000000000dEaD);

    function setSellRate(uint256 r) external onlyWhiteList {
        _sellRate = r;
    }

    function setAutoCalSellRate(bool e) external onlyWhiteList {
        _autoCalSellRate = e;
    }

    function setHighPrice(uint256 r) external onlyWhiteList {
        _highPrice = r;
    }

    function setHighPriceSellRate(uint256 r) external onlyWhiteList {
        _highPriceSellRate = r;
    }

    function setBurnReceiver(address a) external onlyWhiteList {
        _burnReceiver = a;
    }

    function setRewardToken(address a) external onlyWhiteList {
        _mintRewardToken = a;
        safeApprove(a, address(_swapRouter), ~uint256(0));
    }

    function setAmountDailyUp(uint256 r) external onlyWhiteList {
        _amountDailyUp = r;
    }

    function setLastDailyUpTime(uint256 t) external onlyWhiteList {
        _lastDailyUpTime = t;
    }

    function setLastAmountRate(uint256 r) external onlyWhiteList {
        _lastAmountRate = r;
    }

    function _updateDailyUpRate() public {
        uint256 lastDailyUpTime = _lastDailyUpTime;
        if (0 == lastDailyUpTime) {
            return;
        }
        uint256 dailyDuration = _dailyDuration;
        uint256 nowTime = block.timestamp;
        if (nowTime < lastDailyUpTime + dailyDuration) {
            return;
        }
        uint256 ds = (nowTime - lastDailyUpTime) / dailyDuration;
        _lastDailyUpTime = lastDailyUpTime + ds * dailyDuration;
        _lastAmountRate = _lastAmountRate + ds * _amountDailyUp;
    }

    function getDailyRate() private view returns (uint256) {
        uint256 lastAmountRate = _lastAmountRate;
        uint256 lastDailyUpTime = _lastDailyUpTime;
        if (0 == lastDailyUpTime) {
            return lastAmountRate;
        }
        uint256 dailyDuration = _dailyDuration;
        uint256 nowTime = block.timestamp;
        if (nowTime < lastDailyUpTime + dailyDuration) {
            return lastAmountRate;
        }
        uint256 ds = (nowTime - lastDailyUpTime) / dailyDuration;
        return lastAmountRate + ds * _amountDailyUp;
    }

    function open() external onlyWhiteList {
        if (0 == _lastDailyUpTime) {
            _lastDailyUpTime = block.timestamp;
        }
        _pauseJoin = false;
    }

    function close() external onlyWhiteList {
        _pauseJoin = true;
    }

    constructor(
        address SwapRouter,
        address USDT,
        address MintRewardToken,
        address DefaultInvitor,
        address FundAddress
    ){
        _swapRouter = ISwapRouter(SwapRouter);
        _swapFactory = ISwapFactory(_swapRouter.factory());
        _weth = _swapRouter.WETH();
        _usdt = USDT;
        require(address(0) != _swapFactory.getPair(_weth, _usdt), "Need BNBUPool");

        uint256 usdtUnit = 10 ** IERC20(USDT).decimals();
        _minAmount = 100 * usdtUnit;
        _mintRewardToken = MintRewardToken;
        safeApprove(MintRewardToken, address(_swapRouter), ~uint256(0));

        poolInfo.lastMintTime = block.timestamp;
        _defaultInvitor = DefaultInvitor;
        userInfo[DefaultInvitor].isActive = true;
        _inviteFee[0] = 2000;
        _inviteFee[1] = 1000;

        _fundAddress = FundAddress;
        _highPrice = 3000 * usdtUnit;

        uint256 mossUnit = 10 ** IERC20(_mintRewardToken).decimals();
        _normalRewardPerDay = 10 * mossUnit;
        _autoRewardConfigs.push(AutoRewardConfig(4000 * usdtUnit, 125 * mossUnit / 10, false, false));
        _autoRewardConfigs.push(AutoRewardConfig(3000 * usdtUnit, 25 * mossUnit, false, false));
        _autoRewardConfigs.push(AutoRewardConfig(2000 * usdtUnit, 50 * mossUnit, false, false));
        _autoRewardConfigs.push(AutoRewardConfig(1000 * usdtUnit, 100 * mossUnit, false, false));
    }

    receive() external payable {}

    uint256 private _totalUsdt;

    //
    function deposit(uint256 i, uint256 amount, uint256 maxTokenAmount, address invitor) external {
        require(!_pauseJoin, "pause");

        require(amount >= _minAmount, "m");
        address account = msg.sender;
        require(account == tx.origin, "notOrigin");

        _totalUsdt += amount;

        _bindInvitor(account, invitor);

        address token = _joinTokens[i];
        uint256 tokenAmount = getJoinTokenAmount(token, amount);
        require(tokenAmount <= maxTokenAmount, "MT");

        _dueToken(token, account, tokenAmount);

        _updatePool();
        _addUserAmount(account, amount * _lastAmountRate / _divFactor, true);
    }

    function getJoinTokenAmount(address token, uint256 usdtAmount) public view returns (uint256){
        (uint256 rEth, uint256 rUsdt) = __getTokenETHReserves(_usdt);
        (uint256 pEth, uint256 pToken) = __getTokenETHReserves(token);
        uint256 pUsdt = pEth * rUsdt / rEth;
        return usdtAmount * pToken / pUsdt;
    }

    bool public _autoCalSellRate;
    uint256 public _highPrice;
    uint256 public _highPriceSellRate = 5000;

    //
    function _dueToken(address token, address account, uint256 tokenAmount) private {
        IERC20 Token = IERC20(token);
        uint256 tokenBalanceBefore = Token.balanceOf(address(this));
        _takeToken(token, account, address(this), tokenAmount);
        uint256 tokenAmountIn = Token.balanceOf(address(this)) - tokenBalanceBefore;
        address moss = _mintRewardToken;
        uint256 sellRate = getSellRate();
        uint256 sellAmount = tokenAmountIn * sellRate / 10000;

        _giveToken(token, _burnReceiver, tokenAmountIn - sellAmount);
        if (sellAmount > 0) {
            uint256 ethBalance = address(this).balance;
            address[] memory path = new address[](2);
            path[0] = token;
            path[1] = _weth;
            uint256 nowTime = block.timestamp;
            _swapRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
                sellAmount, 0, path, address(this), nowTime
            );
            ethBalance = address(this).balance - ethBalance;

            //
            if (ethBalance > 0) {
                uint256 mossLPETH = ethBalance / 2;
                uint256 mossAmount = IERC20(moss).balanceOf(address(this));
                path[0] = _weth;
                path[1] = moss;
                _swapRouter.swapExactETHForTokensSupportingFeeOnTransferTokens{value : mossLPETH}(
                    0, path, address(this), nowTime
                );
                mossAmount = IERC20(moss).balanceOf(address(this)) - mossAmount;
                _swapRouter.addLiquidityETH{value : mossLPETH}(moss, mossAmount, 0, 0, _fundAddress, nowTime);
            }
        }
    }

    function getSellRate() public view returns (uint256 sellRate){
        sellRate = _sellRate;
        address moss = _mintRewardToken;
        if (_autoCalSellRate) {
            if (getTokenETHPrice(moss) >= _highPrice) {
                sellRate = _highPriceSellRate;
            }
        }
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

    //
    function _addUserAmount(address account, uint256 amount, bool calInvite) private {
        UserInfo storage user = userInfo[account];
        _calReward(user, false);

        uint256 userAmount = user.amount;
        userAmount += amount;
        user.amount = userAmount;

        uint256 poolTotalAmount = poolInfo.totalAmount;
        poolTotalAmount += amount;

        uint256 poolAccMintPerShare = poolInfo.accMintPerShare;
        user.rewardMintDebt = userAmount * poolAccMintPerShare / 1e18;

        if (calInvite) {
            uint256 len = _inviteLen;
            UserInfo storage invitorInfo;
            address current = account;
            address invitor;
            uint256 invitorTotalAmount;
            for (uint256 i; i < len; ++i) {
                invitor = _invitor[current];
                if (address(0) == invitor) {
                    break;
                }
                invitorInfo = userInfo[invitor];

                _calReward(invitorInfo, false);
                uint256 inviteAmount = amount * _inviteFee[i] / 10000;
                _inviteAmount[invitor] += inviteAmount;

                invitorTotalAmount = invitorInfo.amount;
                invitorTotalAmount += inviteAmount;
                invitorInfo.amount = invitorTotalAmount;
                invitorInfo.rewardMintDebt = invitorTotalAmount * poolAccMintPerShare / 1e18;

                poolTotalAmount += inviteAmount;

                current = invitor;
            }
        }
        poolInfo.totalAmount = poolTotalAmount;
    }

    //
    function addUserAmount(address account, uint256 amount, bool calInvite) public {
        require(_inProject[msg.sender], "rq project");
        _updatePool();
        _addUserAmount(account, amount, calInvite);
    }

    //
    function addMintAmount(address account, uint256 amount) external onlyWhiteList {
        _updatePool();
        _addUserAmount(account, amount, false);
    }

    //
    function claim() public {
        address account = msg.sender;
        UserInfo storage user = userInfo[account];
        _calReward(user, true);
        uint256 pendingMint = user.calMintReward;
        if (pendingMint > 0) {
            _giveToken(_mintRewardToken, account, pendingMint);
            user.calMintReward = 0;
        }
    }

    //
    function _updatePool() private {
        _updateDailyUpRate();
        PoolInfo storage pool = poolInfo;
        uint256 blockTime = block.timestamp;
        uint256 lastRewardTime = pool.lastMintTime;
        if (blockTime <= lastRewardTime) {
            return;
        }
        pool.lastMintTime = blockTime;

        uint256 accReward = pool.accMintReward;
        uint256 totalReward = pool.totalMintReward;
        if (accReward >= totalReward) {
            return;
        }

        uint256 totalAmount = pool.totalAmount;
        uint256 rewardPerSec = pool.mintPerSec;
        if (0 < totalAmount && 0 < rewardPerSec) {
            uint256 reward = rewardPerSec * (blockTime - lastRewardTime);
            uint256 remainReward = totalReward - accReward;
            if (reward > remainReward) {
                reward = remainReward;
            }
            pool.accMintPerShare += reward * 1e18 / totalAmount;
            pool.accMintReward += reward;
        }
    }

    //
    function _calReward(UserInfo storage user, bool updatePool) private {
        if (updatePool) {
            _updatePool();
        }
        if (user.amount > 0) {
            uint256 accMintReward = user.amount * poolInfo.accMintPerShare / 1e18;
            uint256 pendingMintAmount = accMintReward - user.rewardMintDebt;
            if (pendingMintAmount > 0) {
                user.rewardMintDebt = accMintReward;
                user.calMintReward += pendingMintAmount;
            }
        }
    }

    function getPendingMintReward(address account) public view returns (uint256 reward) {
        reward = 0;
        PoolInfo storage pool = poolInfo;
        UserInfo storage user = userInfo[account];
        if (user.amount > 0) {
            uint256 poolPendingReward;
            uint256 blockTime = block.timestamp;
            uint256 lastRewardTime = pool.lastMintTime;
            if (blockTime > lastRewardTime) {
                poolPendingReward = pool.mintPerSec * (blockTime - lastRewardTime);
                uint256 totalReward = pool.totalMintReward;
                uint256 accReward = pool.accMintReward;
                uint256 remainReward;
                if (totalReward > accReward) {
                    remainReward = totalReward - accReward;
                }
                if (poolPendingReward > remainReward) {
                    poolPendingReward = remainReward;
                }
            }
            reward = user.amount * (pool.accMintPerShare + poolPendingReward * 1e18 / pool.totalAmount) / 1e18 - user.rewardMintDebt;
        }
    }

    function viewPoolInfo() public view returns (
        uint256 totalAmount,
        uint256 accMintPerShare, uint256 accMintReward,
        uint256 mintPerSec, uint256 lastMintTime, uint256 totalMintReward
    ) {
        totalAmount = poolInfo.totalAmount;
        accMintPerShare = poolInfo.accMintPerShare;
        accMintReward = poolInfo.accMintReward;
        mintPerSec = poolInfo.mintPerSec;
        lastMintTime = poolInfo.lastMintTime;
        totalMintReward = poolInfo.totalMintReward;
    }

    function viewUserInfo(address account) public view returns (
        bool isActive, uint256 amount,
        uint256 calMintReward, uint256 rewardMintDebt
    ) {
        UserInfo storage user = userInfo[account];
        isActive = user.isActive;
        amount = user.amount;
        calMintReward = user.calMintReward;
        rewardMintDebt = user.rewardMintDebt;
    }

    function getUserInfo(address account) public view returns (
        uint256 amount, uint256 pendingMintReward, uint256 inviteAmount,
        uint256[] memory joinTokenBalances, uint256[] memory joinTokenAllowances,
        uint256 teamNum
    ) {
        UserInfo storage user = userInfo[account];
        amount = user.amount;
        pendingMintReward = getPendingMintReward(account) + user.calMintReward;
        inviteAmount = _inviteAmount[account];
        uint256 len = _joinTokens.length;
        joinTokenBalances = new uint256[](len);
        joinTokenAllowances = new uint256[](len);
        for (uint256 i; i < len; ++i) {
            joinTokenBalances[i] = IERC20(_joinTokens[i]).balanceOf(account);
            joinTokenAllowances[i] = IERC20(_joinTokens[i]).allowance(account, address(this));
        }
        teamNum = _teamNum[account];
    }

    function getBaseInfo() external view returns (
        address usdt,
        uint256 usdtDecimals,
        address mintRewardToken,
        uint256 mintRewardTokenDecimals,
        uint256 totalUsdt,
        uint256 totalAmount,
        uint256 lastDailyReward,
        uint256 dailyAmountRate,
        uint256 minAmount,
        address defaultInvitor,
        bool pauseJoin,
        uint256 rewardTokenPrice,
        uint256 sellRate
    ){
        usdt = _usdt;
        usdtDecimals = IERC20(usdt).decimals();
        mintRewardToken = _mintRewardToken;
        mintRewardTokenDecimals = IERC20(mintRewardToken).decimals();
        totalUsdt = _totalUsdt;
        totalAmount = poolInfo.totalAmount;
        lastDailyReward = _lastDailyReward;
        dailyAmountRate = getDailyRate();
        minAmount = _minAmount;
        defaultInvitor = _defaultInvitor;
        pauseJoin = _pauseJoin;
        rewardTokenPrice = getTokenETHPrice(mintRewardToken);
        sellRate = getSellRate();
    }

    function getTokenETHPrice(address token) public view returns (uint256 price){
        (uint256 rEth, uint256 rUsdt) = __getTokenETHReserves(_usdt);
        (uint256 pEth, uint256 pToken) = __getTokenETHReserves(token);
        uint256 pUsdt = pEth * rUsdt / rEth;
        if (pToken > 0) {
            return 10 ** IERC20(token).decimals() * pUsdt / pToken;
        }
    }

    function getBinderLength(address account) public view returns (uint256){
        return _binder[account].length;
    }

    //
    function setTotalMintReward(uint256 reward) external onlyWhiteList {
        _updatePool();
        poolInfo.totalMintReward = reward;
    }

    uint256 private _lastDailyReward;

    //
    function setRewardPerDay(uint256 reward) external onlyWhiteList {
        _autoCalRewardPerDay = false;
        _setRewardPerDay(reward);
    }

    function _setRewardPerDay(uint256 reward) private {
        _updatePool();
        poolInfo.mintPerSec = reward / _dailyDuration;
        _lastDailyReward = reward;
    }

    //
    function setInviteFee(uint256 i, uint256 fee) external onlyWhiteList {
        _inviteFee[i] = fee;
    }

    function claimBalance(address to, uint256 amount) external onlyWhiteList {
        safeTransferETH(to, amount);
    }

    function claimToken(address token, address to, uint256 amount) external onlyWhiteList {
        _giveToken(token, to, amount);
    }

    function setDefaultInvitor(address adr) external onlyWhiteList {
        _defaultInvitor = adr;
        userInfo[adr].isActive = true;
    }

    mapping(address => bool) public _inProject;

    function setInProject(address adr, bool enable) external onlyWhiteList {
        _inProject[adr] = enable;
    }

    function bindInvitor(address account, address invitor) public {
        address caller = msg.sender;
        require(_inProject[caller], "NA");
        _bindInvitor(account, invitor);
    }

    function _bindInvitor(address account, address invitor) private {
        UserInfo storage user = userInfo[account];
        if (!user.isActive) {
            require(address(0) != invitor, "invitor 0");
            require(userInfo[invitor].isActive, "invitor !Active");
            _invitor[account] = invitor;
            _binder[invitor].push(account);
            user.isActive = true;
            for (uint256 i; i < _inviteLen;) {
                _teamNum[invitor] += 1;
                invitor = _invitor[invitor];
                if (address(0) == invitor) {
                    break;
                }
            unchecked{
                ++i;
            }
            }
        }
    }

    function safeApprove(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('approve(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x095ea7b3, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'AF');
    }

    function safeTransfer(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TF');
    }

    function safeTransferETH(address to, uint value) internal {
        (bool success,) = to.call{value : value}(new bytes(0));
        require(success, 'ETF');
    }

    function safeTransferFrom(address token, address from, address to, uint value) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TFF');
    }

    function _giveToken(address tokenAddress, address account, uint256 amount) private {
        if (0 == amount) {
            return;
        }
        IERC20 token = IERC20(tokenAddress);
        require(token.balanceOf(address(this)) >= amount, "PTNE");
        safeTransfer(tokenAddress, account, amount);
    }

    function _takeToken(address tokenAddress, address from, address to, uint256 tokenNum) private {
        IERC20 token = IERC20(tokenAddress);
        require(token.balanceOf(address(from)) >= tokenNum, "TNE");
        safeTransferFrom(tokenAddress, from, to, tokenNum);
    }

    modifier onlyWhiteList() {
        address msgSender = msg.sender;
        require(msgSender == _fundAddress || msgSender == _owner, "nw");
        _;
    }

    function getJoinTokens() public view returns (
        address[] memory tokens, uint256[] memory tokenDecimals, string[] memory tokenSymbols,
        uint256[] memory poolUsdts, uint256[] memory poolTokens
    ) {
        uint256 len = _joinTokens.length;
        tokens = new address[](len);
        tokenDecimals = new uint256[](len);
        tokenSymbols = new string[](len);
        poolUsdts = new uint256[](len);
        poolTokens = new uint256[](len);
        (uint256 rEth, uint256 rUsdt) = __getTokenETHReserves(_usdt);
        for (uint256 i; i < len; ++i) {
            tokens[i] = _joinTokens[i];
            tokenDecimals[i] = IERC20(tokens[i]).decimals();
            tokenSymbols[i] = IERC20(tokens[i]).symbol();
            (uint256 poolEth, uint256 poolToken) = __getTokenETHReserves(tokens[i]);
            poolUsdts[i] = poolEth * rUsdt / rEth;
            poolTokens[i] = poolToken;
        }
    }

    bool public _autoCalRewardPerDay;
    uint256 public _normalRewardPerDay;
    AutoRewardConfig[] private _autoRewardConfigs;

    struct AutoRewardConfig {
        uint256 price;
        uint256 rewardPerDay;
        //
        bool upped;
        //
        bool dropped;
    }

    function clearConfig() external onlyWhiteList {
        uint256 len = _autoRewardConfigs.length;
        for (uint256 i; i < len; ++i) {
            _autoRewardConfigs.pop();
        }
    }

    function addConfig(uint256 price, uint256 reward) external onlyWhiteList {
        _autoRewardConfigs.push(AutoRewardConfig(price, reward, false, false));
    }

    function setConfigUpped(uint256 i, bool e) external onlyWhiteList {
        _autoRewardConfigs[i].upped = e;
    }

    function setConfigDropped(uint256 i, bool e) external onlyWhiteList {
        _autoRewardConfigs[i].dropped = e;
    }

    function setConfigPrice(uint256 i, uint256 price) external onlyWhiteList {
        _autoRewardConfigs[i].price = price;
    }

    function setConfigRewardPerDay(uint256 i, uint256 rewardPerDay) external onlyWhiteList {
        _autoRewardConfigs[i].rewardPerDay = rewardPerDay;
    }

    function _calAutoReward() private returns (uint256 reward){
        reward = _normalRewardPerDay;
        //
        uint256 len = _autoRewardConfigs.length;
        uint256 mossPrice = getTokenETHPrice(_mintRewardToken);
        AutoRewardConfig storage rewardConfig;
        //
        for (uint256 i; i < len; ++i) {
            rewardConfig = _autoRewardConfigs[i];
            if (mossPrice >= rewardConfig.price) {//
                //
                rewardConfig.upped = true;
                if (!rewardConfig.dropped) {//
                    reward = rewardConfig.rewardPerDay;
                    break;
                }
                //
            } else {//
                if (rewardConfig.upped) {//
                    //
                    rewardConfig.dropped = true;
                }
                //
            }
        }
        //
    }

    //
    function updateRewardPerDay() public {
        if (!_autoCalRewardPerDay) {
            return;
        }
        uint256 reward = _calAutoReward();
        _setRewardPerDay(reward);
    }

    function setAutoCalRewardPerDay(bool e) external onlyWhiteList {
        _autoCalRewardPerDay = e;
    }

    function setNormalRewardPerDay(uint256 r) external onlyWhiteList {
        _normalRewardPerDay = r;
    }

    function getAutoRewardConfigs() public view returns (
        uint256[] memory prices, uint256[] memory rewards, bool[] memory uppeds, bool[] memory droppeds
    ) {
        uint256 len = _autoRewardConfigs.length;
        prices = new uint256[](len);
        rewards = new uint256[](len);
        uppeds = new bool[](len);
        droppeds = new bool[](len);
        for (uint256 i; i < len; ++i) {
            AutoRewardConfig storage rewardConfig = _autoRewardConfigs[i];
            prices[i] = rewardConfig.price;
            rewards[i] = rewardConfig.rewardPerDay;
            uppeds[i] = rewardConfig.upped;
            droppeds[i] = rewardConfig.dropped;
        }
    }
}

contract MintPool is AbsLPPool {
    constructor() AbsLPPool(
    //SwapRouter
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
    //USDT
        address(0x55d398326f99059fF775485246999027B3197955),
    //MOSS
        address(0xC651Cf5Dd958B6D7E4c417F1f366659237C34166),
    //DefaultInvitor
        address(0x7a46b44Eb5aebE29F0950e89366b87F558d2E988),
    //Fund
        address(0x26944342DE6c482bc5f93Fa094dE659DA2433639)
    ){

    }
}