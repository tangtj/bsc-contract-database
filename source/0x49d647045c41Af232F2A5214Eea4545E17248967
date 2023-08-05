// ██████  ███████ ██    ██  ██████  ██      ██    ██ ███████ ██  ██████  ███    ██                   
// ██   ██ ██      ██    ██ ██    ██ ██      ██    ██     ██  ██ ██    ██ ████   ██                  
// ██████  █████   ██    ██ ██    ██ ██      ██    ██   ██    ██ ██    ██ ██ ██  ██                   
// ██   ██ ██       ██  ██  ██    ██ ██      ██    ██  ██     ██ ██    ██ ██  ██ ██                   
// ██   ██ ███████   ████    ██████  ███████  ██████  ███████ ██  ██████  ██   ████     

// CONTRACT DEVELOPED BY REVOLUZION

//Revoluzion Ecosystem
//WEB: https://revoluzion.io
//DAPP: https://revoluzion.app

// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

/********************************************************************************************
  INTERFACE
********************************************************************************************/

interface IERC20 {
    
    // EVENT 

    event Transfer(address indexed from, address indexed to, uint256 value);
    
    event Approval(address indexed owner, address indexed spender, uint256 value);

    // FUNCTION

    function name() external view returns (string memory);
    
    function symbol() external view returns (string memory);
    
    function decimals() external view returns (uint8);
    
    function totalSupply() external view returns (uint256);
    
    function balanceOf(address account) external view returns (uint256);
    
    function transfer(address to, uint256 amount) external returns (bool);
    
    function allowance(address owner, address spender) external view returns (uint256);
    
    function approve(address spender, uint256 amount) external returns (bool);
    
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

interface IRouter {

    // FUNCTION

    function WETH() external pure returns (address);
        
    function factory() external pure returns (address);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(uint256 amountIn, uint256 amountOutMin, address[] calldata path, address to, uint256 deadline) external;
    
    function swapExactETHForTokensSupportingFeeOnTransferTokens(uint256 amountOutMin, address[] calldata path, address to, uint256 deadline) external payable;

    function addLiquidityETH(address token, uint256 amountTokenDesired, uint256 amountTokenMin, uint256 amountETHMin, address to, uint256 deadline) external payable returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);
}

interface IAuthError {

    // ERROR

    error InvalidOwner(address account);

    error UnauthorizedAccount(address account);

    error InvalidAuthorizedAccount(address account);

    error CurrentAuthorizedState(address account, bool state);
}

interface ICommonError {

    // ERROR

    error CannotUseCurrentAddress(address current);

    error CannotUseCurrentValue(uint256 current);

    error CannotUseCurrentState(bool current);

    error InvalidAddress(address invalid);

    error InvalidValue(uint256 invalid);
}

interface IStaking {

    // DATA

    struct Leaderboard {
        uint256 stakeAmount;
        address user;
    }

    // FUNCTION

    function isWDYMStaking() external pure returns (bool);

    function stake(uint256 amount, uint256 stakeTypeId) external;

    function unstake(uint256 amount, uint256 stakeId) external;

    function userTotalStakes(address user) external returns (uint256);

    function getStakeTypeRewardMaxRatio() external view returns (uint256);
    
    function getLeaderboard() external view returns (Leaderboard[] memory);
    
    function deposit() external payable;

    function depositStuckedBNB() external;

    function depositBUSD(uint256 amount) external;

    function depositStuckedBUSD(uint256 amount) external;
}

interface IWDYM is IERC20 {

    // FUNCTION

    function isWDYM() external pure returns (bool);
}

/********************************************************************************************
  ACCESS
********************************************************************************************/

abstract contract Auth is IAuthError{
    
    // DATA

    address private _owner;

    // MAPPING

    mapping(address => bool) public authorization;

    // MODIFIER

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    modifier authorized() {
        _checkAuthorized();
        _;
    }

    // CONSTRUCCTOR

    constructor(address initialOwner) {
        _transferOwnership(initialOwner);
        authorize(initialOwner);
        if (initialOwner != msg.sender) {
            authorize(msg.sender);
        }
    }

    // EVENT
    
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    event UpdateAuthorizedAccount(address authorizedAccount, address caller, bool state, uint256 timestamp);

    // FUNCTION
    
    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        if (owner() != msg.sender) {
            revert UnauthorizedAccount(msg.sender);
        }
    }

    function _checkAuthorized() internal view virtual {
        if (!authorization[msg.sender]) {
            revert UnauthorizedAccount(msg.sender);
        }
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        if (newOwner == address(0)) {
            revert InvalidOwner(address(0));
        }
        _transferOwnership(newOwner);
    }

    function authorize(address account) public virtual onlyOwner {
        if (account == address(0) || account == address(0xdead)) {
            revert InvalidAuthorizedAccount(account);
        }
        _authorization(account, msg.sender, true);
    }

    function unauthorize(address account) public virtual onlyOwner {
        if (account == address(0) || account == address(0xdead)) {
            revert InvalidAuthorizedAccount(account);
        }
        _authorization(account, msg.sender, false);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }

    function _authorization(address account, address caller, bool state) internal virtual {
        if (authorization[account] == state) {
            revert CurrentAuthorizedState(account, state);
        }
        authorization[account] = state;
        emit UpdateAuthorizedAccount(account, caller, state, block.timestamp);
    }
}

/********************************************************************************************
  SECURITY
********************************************************************************************/

abstract contract Pausable {

    // DATA

    bool private _paused;

    // ERROR

    error EnforcedPause();

    error ExpectedPause();

    // MODIFIER

    modifier whenNotPaused() {
        _requireNotPaused();
        _;
    }

    modifier whenPaused() {
        _requirePaused();
        _;
    }

    // CONSTRUCTOR

    constructor() {
        _paused = false;
    }

    // EVENT
    
    event Paused(address account);

    event Unpaused(address account);

    // FUNCTION

    function paused() public view virtual returns (bool) {
        return _paused;
    }

    function pause() external virtual whenNotPaused {
        _pause();
    }

    function unpause() external virtual whenPaused {
        _unpause();
    }

    function _requireNotPaused() internal view virtual {
        if (paused()) {
            revert EnforcedPause();
        }
    }

    function _requirePaused() internal view virtual {
        if (!paused()) {
            revert ExpectedPause();
        }
    }

    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(msg.sender);
    }

    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(msg.sender);
    }
}

/********************************************************************************************
  STAKING
********************************************************************************************/

contract WhatDoYouMemeStaking is Auth, Pausable, ICommonError, IStaking {

    // DATA

    struct RewardPool {
        uint256 id;
        uint256 amountAdded;
        uint256 amountClaimed;
        uint256 rewardsPerStake;
        uint256 timestamp;
    }

    struct Stake {
        uint256 stakeType;
        uint256 amount;
        uint256 timestamp;
        uint256 startPoolIndex;
        uint256 totalExcluded;
        uint256 totalRealised;
    }

    IRouter public router;

    IERC20 public constant REWARD = IERC20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56);
    IWDYM public constant TOKEN = IWDYM(0x769c6F0C5c2BcD1B76638BD58e5350f5c94128F3);

    RewardPool[] public rewardPools;

    uint256 public constant DENOMINATOR = 10_000;

    uint256 public immutable rewardsPerStakeAccuracyFactor;

    uint256 public amountToRedistribute = 0;
    uint256 public penaltyPercentage = 0;
    uint256 public totalStaked = 0;
    uint256 public totalRewards = 0;
    uint256 public totalDistributed = 0;
    uint256 public rewardsPerStake = 0;
    uint256 public currentPoolIndex = 0;
    uint256 public stakeType = 3;

    uint256[] public rewardRatio = [100, 150, 300];

    address private constant DEAD = address(0xdead);
    address private constant ZERO = address(0);

    address public penaltyReceiver;

    address[] public stakers;

    bool private constant ISWDYM_STAKING = true;

    bool public takePenalty = false;
    bool public emergencyWithdraw = false;

    // MAPPING
        
    mapping(uint256 => uint256) public stakeTypeToDuration;
    mapping(address => uint256) public stakerIndexes;
    mapping(address => uint256) public userTotalStakes;
    mapping(address => uint256) public userStaking;
    mapping(address => uint256) public userActiveStaking;
    mapping(address => uint256) public userInactiveStaking;
    mapping(address => mapping(uint256 => Stake)) public userStakes;

    // ERROR

    error InvalidRewardPoolId();

    error InvalidStakeId();

    error EmergencyWithdrawDisabled();

    error NoStakingRewards();

    error NotTimeToUnstake(uint256 unstakeTime, uint256 timeNow);

    error ExceedUserStakeId(uint256 maxIdValue);

    error ExceedStakeBalance(uint256 balance);

    error ExceedStakeTypeRange(uint256 maxRange);

    error ExceedStuckedBalance(uint256 balance);

    // CONSTRUCTOR

    constructor(
        address routerAddress,
        address penaltyReceiverAddress
    ) Auth(msg.sender) {
        if (penaltyReceiverAddress == ZERO || penaltyReceiverAddress == DEAD) { revert InvalidAddress(penaltyReceiverAddress); }
        router = IRouter(routerAddress);
        penaltyReceiver = penaltyReceiverAddress;
        rewardsPerStakeAccuracyFactor = 1 ether * 1 ether;
        stakeTypeToDuration[1] = 90 days;
        stakeTypeToDuration[2] = 180 days;
        stakeTypeToDuration[3] = 365 days;
    }

    // EVENT

    event UpdateRouter(IRouter oldRouter, IRouter newRouter, address sender, uint256 timestamp);

    event UpdatePenaltyReceiver(address oldReceiver, address newReceiver, address sender, uint256 timestamp);

    event UpdatePenalty(uint256 oldPenalty, uint256 newPenalty, address sender, uint256 timestamp);
    
    event UpdateTakePenalty(bool oldState, bool newState, address sender, uint256 timestamp);

    event UpdateEmergencyWithdraw(bool oldState, bool newState, address sender, uint256 timestamp);

    event RewardDistribution(uint256 balance, uint256 distributed, address sender, uint256 timestamp);

    event RewardRescued(uint256 balance, uint256 distributed, address sender, uint256 timestamp);

    event Staked(address indexed staker, uint256 amount);

    event Unstaked(address indexed staker, uint256 amount);

    // FUNCTION

    /* General */

    receive() external payable {}

    function isWDYMStaking() external pure returns (bool) {
        return ISWDYM_STAKING;
    }

    /* Update */

    function addStakeType(uint256 duration, uint256 ratio) external authorized {
        if (duration <= 0) { revert InvalidValue(duration); }
        stakeType += 1;
        stakeTypeToDuration[stakeType] = duration;
        rewardRatio.push(ratio);
    }

    function removeStakeType(uint256 amount) external authorized {
        if (amount <= 0) { revert InvalidValue(amount); }
        if (amount > stakeType) { revert ExceedStakeTypeRange(stakeType); }
        for (uint256 i = stakeType; i > stakeType - amount; i--) {
            stakeTypeToDuration[i] = 0;
            rewardRatio.pop();
        }
        stakeType -= amount;
    }

    function modifyStakeType(uint256 index, uint256 duration, uint256 ratio) external authorized {
        if (duration <= 0) { revert InvalidValue(duration); }
        if (index <= 0) { revert InvalidValue(index); }
        if (index > stakeType) { revert ExceedStakeTypeRange(stakeType); }
        stakeTypeToDuration[index] = duration;
        rewardRatio[index - 1] = ratio;
    }

    function updateRouter(IRouter newRouter) external authorized {
        if (newRouter == router) { revert CannotUseCurrentAddress(address(newRouter)); }
        IRouter oldRouter = router;
        router = newRouter;
        emit UpdateRouter(oldRouter, newRouter, msg.sender, block.timestamp);
    }

    function updatePenaltyReceiver(address newReceiver) external authorized {
        if (newReceiver == penaltyReceiver) { revert CannotUseCurrentAddress(newReceiver); }
        if (newReceiver == ZERO || newReceiver == DEAD) { revert InvalidAddress(newReceiver); }
        address oldReceiver = penaltyReceiver;
        penaltyReceiver = newReceiver;
        emit UpdatePenaltyReceiver(oldReceiver, newReceiver, msg.sender, block.timestamp);
    }

    function updatePenalty(uint256 newPenalty) external authorized {
        if (newPenalty == penaltyPercentage) { revert CannotUseCurrentValue(newPenalty); }
        if (penaltyPercentage > 2_000) { revert InvalidValue(newPenalty); }
        uint256 oldPenalty = penaltyPercentage;
        penaltyPercentage = newPenalty;
        emit UpdatePenalty(oldPenalty, newPenalty, msg.sender, block.timestamp);
    }

    function updateTakePenalty(bool newState) external authorized {
        if (newState == takePenalty) { revert CannotUseCurrentState(newState); }
        bool oldState = takePenalty;
        takePenalty = newState;
        emit UpdateTakePenalty(oldState, newState, msg.sender, block.timestamp);
    }

    function updateEmergencyWithdraw(bool newState) external authorized {
        if (newState == emergencyWithdraw) { revert CannotUseCurrentState(newState); }
        bool oldState = emergencyWithdraw;
        emergencyWithdraw = newState;
        emit UpdateEmergencyWithdraw(oldState, newState, msg.sender, block.timestamp);
    }

    /* Check */

    function getStakeTypeRewardMaxRatio() public override view returns (uint256) {
        uint256 maxRatio = 0;
        for (uint256 i = 0; i < rewardRatio.length; i++) {
            if (maxRatio < rewardRatio[i]) {
                maxRatio = rewardRatio[i];
            }
        }
        return maxRatio;
    }

    function getLeaderboard() external override view returns (Leaderboard[] memory) {
        Leaderboard[] memory board = new Leaderboard[](stakers.length);
        for (uint256 i = 0; i < stakers.length; i++) {
            address user = stakers[i];
            Leaderboard memory item = Leaderboard({
                stakeAmount: userTotalStakes[user],
                user: user
            });
            board[i]= item;
        }

        for (uint256 i = 0; i < board.length - 1; i++) {
            for (uint256 j = 0; j < board.length - i - 1; j++) {
                if (board[j].stakeAmount <= board[j + 1].stakeAmount) {
                    Leaderboard memory temp = board[j];
                    board[j] = board[j + 1];
                    board[j + 1] = temp;
                }
            }
        }
        return board;
    }

    function getCumulativeRewards(uint256 amount, uint256 poolId) public view returns (uint256) {
        _verifyPoolID(poolId);
        return amount * rewardPools[poolId - 1].rewardsPerStake / rewardsPerStakeAccuracyFactor;
    }
    
    function getUnpaidEarnings(address staker, uint256 stakeId, uint256 poolId) public view returns (uint256) {
        _verifyPoolID(poolId);
        _verifyStakeID(staker, stakeId);
        
        if (userTotalStakes[staker] == 0) {
            return 0;
        }

        uint256 stakerTotalRewards = getCumulativeRewards(userStakes[staker][stakeId].amount, poolId);
        uint256 stakerTotalExcluded = userStakes[staker][stakeId].totalExcluded;

        if (stakerTotalRewards <= stakerTotalExcluded) {
            return 0;
        }

        return stakerTotalRewards - stakerTotalExcluded;
    }
    
    function getStakeTypeSpecificUnpaidEarnings(address staker, uint256 stakeId, uint256 poolId) public view returns (uint256) {
        return getUnpaidEarnings(staker, stakeId, poolId) * rewardRatio[userStakes[msg.sender][stakeId].stakeType - 1] / getStakeTypeRewardMaxRatio();
    }
    
    function getUserTotalUnpaidEarnings(address staker) public view returns (uint256) {
        uint256 unpaidEarning = 0;
        uint256 totalUserStakes = userStaking[staker];

        if (userStaking[staker] > 0 && rewardPools.length > 0) {
            for (uint256 i = 1; i <= totalUserStakes; i++) {
                for (uint256 j = 1; j <= rewardPools.length; j++) {
                    if (
                        userStakes[staker][i].amount > 0 &&
                        userStakes[staker][i].startPoolIndex <= j &&
                        rewardPools[j - 1].amountAdded > rewardPools[j - 1].amountClaimed
                    ) {
                        unpaidEarning += getStakeTypeSpecificUnpaidEarnings(staker, i, j);
                    }
                }
            }
        }

        return unpaidEarning;
    }

    /* Staking */

    function stake(uint256 amount, uint256 stakeTypeId) external override whenNotPaused {
        if (stakeTypeId <= 0 || stakeTypeId > stakeType) { revert InvalidValue(stakeTypeId); }
        if (userTotalStakes[msg.sender] == 0) {
            _addStaker(msg.sender);
        }

        uint256 unpaidEarning = _handleDistribution(msg.sender);

        uint256 index = userStaking[msg.sender];
        uint256 excluded = 0;

        if (rewardPools.length > 0) {
            excluded = getCumulativeRewards(amount, rewardPools.length);
        }

        Stake memory newStake = Stake ({
            stakeType: stakeTypeId,
            amount: amount,
            timestamp: block.timestamp,
            startPoolIndex: rewardPools.length + 1,
            totalExcluded: excluded,
            totalRealised: 0
        });

        userActiveStaking[msg.sender] += 1;
        userStaking[msg.sender] = index + 1;
        userStakes[msg.sender][index + 1] = newStake;
        userTotalStakes[msg.sender] += amount;
        totalStaked += amount;

        emit Staked(msg.sender, amount);
        
        if (unpaidEarning > 0) {
            totalDistributed += unpaidEarning;
            require(REWARD.transfer(msg.sender, unpaidEarning), "Stake: There's something wrong with the transfer.");
        }

        require(TOKEN.transferFrom(msg.sender, address(this), amount), "Stake: There's something wrong with the transfer.");
    }

    function unstake(uint256 amount, uint256 stakeId) external override whenNotPaused {
        uint256 unstakeTime = userStakes[msg.sender][stakeId].timestamp + stakeTypeToDuration[userStakes[msg.sender][stakeId].stakeType];
        if (
            unstakeTime > block.timestamp
        ) { revert NotTimeToUnstake(unstakeTime, block.timestamp); }
        _unstake(amount, stakeId, false);
    }

    function emergencyWithdrawStaking(uint256 amount, uint256 index) external {
        if (!emergencyWithdraw) { revert EmergencyWithdrawDisabled(); }
        _unstake(amount, index, takePenalty);
    }

    function _unstake(uint256 amount, uint256 stakeId, bool penalty) internal {
        _verifyStakeID(msg.sender, stakeId);
        if (amount <= 0) { revert InvalidValue(amount); }
        if (userStakes[msg.sender][stakeId].amount < amount) { revert ExceedStakeBalance(userStakes[msg.sender][stakeId].amount); }

        if (rewardPools.length > 0) {
            for (uint256 i = 1; i <= rewardPools.length; i++) {
                if (getUnpaidEarnings(msg.sender, stakeId, i) > 0) {
                    claimRewards(stakeId, i);
                }
            }
        }

        userStakes[msg.sender][stakeId].amount -= amount;
        userTotalStakes[msg.sender] -= amount;            
        totalStaked -= amount;

        if (userStakes[msg.sender][stakeId].amount == 0) {
            userActiveStaking[msg.sender] -= 1;
            userInactiveStaking[msg.sender] += 1;
        }

        if (userTotalStakes[msg.sender] == 0) {
            _removeStaker(msg.sender);
        }

        emit Unstaked(msg.sender, amount);
        
        uint256 penaltyAmount = 0;

        if (penaltyPercentage > 0) {
            penaltyAmount = amount * penaltyPercentage / DENOMINATOR;
        }

        if (penalty) {
            require(TOKEN.transfer(msg.sender, amount - penaltyAmount), "Unstake: There's something wrong with the transfer.");
            require(TOKEN.transfer(penaltyReceiver, penaltyAmount), "Unstake: There's something wrong with the penalty transfer.");
        } else {
            require(TOKEN.transfer(msg.sender, amount), "Unstake: There's something wrong with the transfer.");
        }
    }

    /* Claim */

    function claimRewards(uint256 stakeId, uint256 poolId) public {
        _verifyPoolID(poolId);
        _verifyStakeID(msg.sender, stakeId);
        if (userTotalStakes[msg.sender] <= 0) { revert NoStakingRewards(); }
        
        
        uint256 unpaidEarning = 0;

        if (userStakes[msg.sender][stakeId].amount > 0 && rewardPools[poolId - 1].amountAdded != rewardPools[poolId - 1].amountClaimed) {
            unpaidEarning = _calculateEarnings(msg.sender, stakeId, poolId);
        }

        if (unpaidEarning > 0) {
            totalDistributed += unpaidEarning;
            require(REWARD.transfer(msg.sender, unpaidEarning), "Claim Rewards: There's something wrong with the transfer.");
        } else {
            revert NoStakingRewards();
        }
    }

    function claimAllRewards() external {
        if (userStaking[msg.sender] > 0 && rewardPools.length > 0) {
            for (uint256 i = 1; i <= userStaking[msg.sender]; i++) {
                for (uint256 j = 1; j <= rewardPools.length; j++) {
                    if (getUnpaidEarnings(msg.sender, i, j) > 0) {
                        claimRewards(i, j);
                    }
                }
            }
        } else {
            revert NoStakingRewards();
        }
    }

    /* Deposit */

    function deposit() external override payable whenNotPaused {
        _handleDeposits(msg.value);
    }
    
    function depositStuckedBNB() external override whenNotPaused {
        uint256 amount = address(this).balance;
        _handleDeposits(amount);
    }
    
    function depositBUSD(uint256 amount) external override whenNotPaused {
        uint256 initialRewardBalance = REWARD.balanceOf(address(this));
        
        require(REWARD.transferFrom(msg.sender, address(this), amount), "Deposit BUSD: There's something wrong with the transfer.");

        uint256 added = REWARD.balanceOf(address(this)) - initialRewardBalance;
        _updateRewardInfo(added);
    }

    function depositStuckedBUSD(uint256 amount) external override whenNotPaused {
        if (amount > amountToRedistribute) { revert ExceedStuckedBalance(amountToRedistribute); }
        if (amountToRedistribute <= 0) { revert InvalidValue(amountToRedistribute); }
        if (amount <= 0) { revert InvalidValue(amount); }
        _updateRewardInfo(amount);
        amountToRedistribute -= amount;
        emit RewardDistribution(amountToRedistribute, amountToRedistribute - amount, msg.sender, block.timestamp);
    }

    /* Rescue */

    function wTokens(uint256 amount, address tokenAddress) external onlyOwner {
        uint256 wAmount = amount;

        if (tokenAddress == ZERO) {
            if (amount == 0) {
                wAmount = address(this).balance;
            }
            require(wAmount <= address(this).balance, "WithdrawTokens: Insufficient balance.");
            payable(msg.sender).transfer(wAmount);
            return;
        }

        if (amount == 0) {
            if (IERC20(tokenAddress) == REWARD) {
                wAmount = amountToRedistribute;
            } else {
                wAmount = IERC20(tokenAddress).balanceOf(address(this));
            }
        }

        if (IERC20(tokenAddress) == REWARD) {
            require(wAmount <= amountToRedistribute, "WithdrawTokens: Insufficient balance to redistribute.");
            amountToRedistribute -= wAmount;
            emit RewardRescued(amountToRedistribute, wAmount, msg.sender, block.timestamp);
        }
        require(wAmount <= IERC20(tokenAddress).balanceOf(address(this)), "WithdrawTokens: Insufficient balance.");
        
        require(
            IERC20(tokenAddress).transfer(
                msg.sender,
                wAmount
            ),
            "WithdrawTokens: Transfer transaction might fail."
        );
    }

    /* Internal */

    function _handleDistribution(address staker) internal returns (uint256) {
        uint256 unpaidEarning = 0;
        uint256 totalUserStakes = userStaking[staker];

        if (userStaking[staker] > 0 && rewardPools.length > 0) {
            for (uint256 i = 1; i <= totalUserStakes; i++) {
                for (uint256 j = 1; j <= rewardPools.length; j++) {
                    if (
                        userStakes[staker][i].amount > 0 &&
                        userStakes[staker][i].startPoolIndex <= j &&
                        rewardPools[j - 1].amountAdded > rewardPools[j - 1].amountClaimed
                    ) {
                        unpaidEarning += _calculateEarnings(staker, i, j);
                    }
                }
            }
        }

        return unpaidEarning;
    }

    function _calculateEarnings(address staker, uint256 stakeId, uint256 poolId) internal returns (uint256) {
        uint256 unpaidEarningRaw = getUnpaidEarnings(staker, stakeId, poolId);
        uint256 unpaid = getStakeTypeSpecificUnpaidEarnings(staker, stakeId, poolId);
        uint256 unpaidEarningRedistribute = unpaidEarningRaw - unpaid;
        amountToRedistribute += unpaidEarningRedistribute;
        userStakes[staker][stakeId].totalRealised += unpaid;
        userStakes[staker][stakeId].totalExcluded = getCumulativeRewards(userStakes[staker][stakeId].amount, poolId);
        rewardPools[poolId - 1].amountClaimed += unpaid;
        return unpaid;
    }

    function _handleDeposits(uint256 amount) internal {
        address[] memory path = new address[](2);
        path[0] = router.WETH();
        path[1] = address(REWARD);

        uint256 initialRewardBalance = REWARD.balanceOf(address(this));
        
        router.swapExactETHForTokensSupportingFeeOnTransferTokens {
            value: amount
        } (0, path, address(this), block.timestamp);
        
        uint256 added = REWARD.balanceOf(address(this)) - initialRewardBalance;
        _updateRewardInfo(added);
    }

    function _updateRewardInfo(uint256 added) internal {
        if (added > 0) {
            totalRewards += added;
            rewardsPerStake += rewardsPerStakeAccuracyFactor * added / totalStaked;
            currentPoolIndex += 1;
            RewardPool memory newPool = RewardPool({
                id: currentPoolIndex,
                amountAdded: added,
                amountClaimed: 0,
                rewardsPerStake: rewardsPerStake,
                timestamp: block.timestamp
            });
            rewardPools.push(newPool);
        }
    }

    function _addStaker(address staker) internal {
        stakerIndexes[staker] = stakers.length;
        stakers.push(staker);
    }
    
    function _removeStaker(address staker) internal {
        if (stakers.length > 1) {
            stakers[stakerIndexes[staker]] = stakers[stakers.length - 1];
            stakerIndexes[stakers[stakers.length - 1]] = stakerIndexes[staker];
        }
        stakers.pop();
    }

    function _verifyPoolID(uint256 poolId) internal view {
        if (poolId <= 0) { revert InvalidValue(poolId); }
        if (poolId > rewardPools.length) { revert InvalidRewardPoolId(); }
    }

    function _verifyStakeID(address staker, uint256 stakeId) internal view {
        if (stakeId <= 0) { revert InvalidValue(stakeId); }
        if (stakeId > userStaking[staker]) { revert ExceedUserStakeId(userStaking[staker]); }
    }

}