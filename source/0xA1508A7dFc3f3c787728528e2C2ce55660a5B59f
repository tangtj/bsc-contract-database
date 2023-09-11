// SPDX-License-Identifier: MIT

pragma solidity =0.8.15;

library SafeMath {

    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }

    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

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

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

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

    constructor() {
        _transferOwnership(_msgSender());
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface IERC20 {

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

abstract contract ReentrancyGuard {

    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        _status = _NOT_ENTERED;
    }
}

contract DeenCoinStaking is Ownable, ReentrancyGuard {

    using SafeMath for uint256;
    IERC20 public DeenCoin;

    uint256 public SAPY = 1500;
    uint256 public PoolStartTime;
    uint256 public totalStaked;
    uint256 public totalUnstaked;
    uint256 public totalDistributed;
    uint256 public totalAmountDistributed;
    uint256 public StakersCount;

    struct StakedRec {
        uint256 _amount;
        uint256 _apy;
        uint256 _startTime;
        uint256 _endTime;    //in seconds
        uint256 _perSecReward;
        uint256 _perSecAmount;
        uint256 _claimedReward;
        uint256 _claimedAmount;
        uint256 _totalReward;
    }

    struct pool {
        address _user;
        uint256 _totalStaked;
        uint256 _totalWithdraw;
        uint256 _totalClaimed;
        uint256 _totalAmounts;
        StakedRec[] _Ledger;
    }
    mapping (address => pool) public _stakers;
    bool public paused;
    bool public wpaused;
    bool public without_staked = true;

    struct Account {
        address referrer;
        uint reward;
        uint referredCount;
    }

    struct ReferralHistory{
        address from;
        uint256 depositAmount;
        uint256 referralAmount;
        uint256 timestamp;
    }
      
    uint256[] levelRate = [1000,300,200];
    uint public minReward = 1;
    mapping(address => Account) public accounts;
    mapping(address => mapping( uint256 => ReferralHistory[])) public userReferrals;
    address public defaultRef =  0xcA1827068d04501bcFc6B4EcF2c9B8C30c316226;
    mapping(address => mapping (address => bool)) private teamMember;
    mapping(uint => mapping(address => uint)) public team;

    constructor() {
        PoolStartTime = block.timestamp;
        DeenCoin = IERC20(0x532A252F457BB04127e01cEBe9bF394b93283999);
    }

    receive() external payable {}

    event RegisteredReferer(address referee, address referrer);
    event RegisteredRefererFailed(address referee, address referrer, string reason);
    event PaidReferral(address from, address to, uint amount, uint level);
    event Staked (
        address indexed _user,
        uint256 indexed _amount,
        uint256 _timestamp,
        uint256 _apy
    );

    event Claimed (
        address indexed _user,
        uint256 indexed _amount,
        uint256 _timestamp
    );

    function stake(uint _amount,uint _duration,address _referrer) public nonReentrant() {
        require(!paused,"Error: Staking is Currently Paused Now!!");
        address account = msg.sender;
        DeenCoin.transferFrom(account, address(this), _amount);
        uint staked = userTotalStaked(account);
        if(staked == 0) {
            _stakers[account]._user = account;
            StakersCount++;
        }
        if(!hasReferrer(msg.sender)) {
            uint ref_staked = userTotalStaked(_referrer);
            if(_referrer == address(0)){ 
                addReferrer(defaultRef);
            }
            else if(ref_staked == 0 && without_staked){
                addReferrer(_referrer);
            }
            else if(ref_staked > 0){
                addReferrer(_referrer);
            }
            else{
                addReferrer(defaultRef);
            }
        }

        payReferral(_amount);
        uint256 _runningApy = getApy();
        uint endT = block.timestamp.add(_duration);
        (uint aPersec , uint rPersec , uint _tr) = perSecR(_amount, _runningApy, _duration);
        StakedRec memory _rec = 
                StakedRec(
                    _amount,
                    _runningApy,
                    block.timestamp,
                    endT,
                    rPersec,
                    aPersec,
                    0,
                    0,
                    _tr
                );
        _stakers[account]._Ledger.push(_rec);
        _stakers[account]._totalStaked += _amount;
        totalStaked += _amount;
        emit Staked(account,_amount,block.timestamp,_runningApy);
    }

    function claim(uint _index) public nonReentrant() {
        require(!wpaused,"Error: Staking is Currently Paused Now!!");
        address account = msg.sender;

        errorCatch(account,_index);

        uint length = _stakers[account]._Ledger.length;
        StakedRec memory arr = _stakers[account]._Ledger[_index];

        uint reward = 0;
        uint amount = 0;
        uint total = 0;
        if(block.timestamp >= arr._endTime){
            reward =  arr._totalReward.sub(arr._claimedReward);
            amount = arr._amount.sub(arr._claimedAmount);
            total = amount.add(reward);
            totalUnstaked += amount;
            _stakers[account]._totalAmounts += amount;
            _stakers[account]._totalClaimed += reward;
            _stakers[account]._totalWithdraw += arr._amount;
            _stakers[account]._Ledger[_index] = _stakers[account]._Ledger[length - 1];
            _stakers[account]._Ledger.pop();
        }
        else{
            uint timeTill = calTime(arr._startTime);
            reward =  (timeTill.mul(arr._perSecReward)).sub(arr._claimedReward);
            amount =  (timeTill.mul(arr._perSecAmount)).sub(arr._claimedAmount);
            uint minimumR = arr._amount.mul(minReward).div(10000);
            total = amount.add(reward);
            require( total >= minimumR , "claim not allowed, minimum reward needed.");
            _stakers[account]._Ledger[_index]._claimedAmount += amount;
            _stakers[account]._Ledger[_index]._claimedReward += reward;
            _stakers[account]._totalAmounts += amount;
            _stakers[account]._totalClaimed += reward;
        }

        totalAmountDistributed += amount;
        totalDistributed += reward;

        DeenCoin.transfer(account, total);
        emit Claimed(account,total,block.timestamp);
    }


    function errorCatch(address account, uint _index) internal view {
        uint length = _stakers[account]._Ledger.length;
        if(_stakers[account]._totalStaked == 0) {
            revert("Error: No Amount Staked at this time!!");
        }
        if(_index > length - 1) {
            revert("Error: Invalid Index!");
        }
    }

    function slotRevenue(address _adr, uint _index) external view returns (uint,bool) {
        uint length = _stakers[_adr]._Ledger.length;
        if(length == 0) revert("Error: No Record Found");
        StakedRec memory arr = _stakers[_adr]._Ledger[_index];
        uint reward = 0;
        uint amount = 0;
        uint total = 0;
        bool allowed = false;
        if(block.timestamp >= arr._endTime){
            reward =  arr._totalReward.sub(arr._claimedReward);
            amount = arr._amount.sub(arr._claimedAmount);
            total = amount.add(reward);
            allowed = true;
        }
        else{
            uint timeTill = calTime(arr._startTime);
            reward =  (timeTill.mul(arr._perSecReward)).sub(arr._claimedReward);
            amount =  (timeTill.mul(arr._perSecAmount)).sub(arr._claimedAmount);
            uint minimumR = arr._amount.mul(minReward).div(10000);
            total = amount.add(reward);
            if(total >= minimumR){
                allowed = true;
            }
        }

        return (total,allowed);
    }

    function userTotalStaked(address _user) internal view returns (uint){
        uint total =  _stakers[_user]._totalStaked - _stakers[_user]._totalWithdraw;
        return total;
    }

    function userReferral(address _address , uint _level) external view returns(ReferralHistory[] memory){
        uint length = userReferrals[_address][_level].length;
        ReferralHistory[] memory rh = new ReferralHistory[](length);
        for(uint i=0;i<length;i++){
            rh[i] = userReferrals[_address][_level][i];
        }

        return rh;
    } 

    function getList(address _account) external view returns (StakedRec[] memory _rec, uint _size) {
        return (_stakers[_account]._Ledger, _stakers[_account]._Ledger.length);
    }

    function getLedgerIndex(address _account,uint _index) external view returns (StakedRec memory _rec) {
        uint length = _stakers[_account]._Ledger.length;
        if(length == 0) revert("Error: No Record Found");
        if(_index > length - 1) {
            revert("Error: Invalid Index!");
        }
        StakedRec memory arr = _stakers[_account]._Ledger[_index];
        return arr;
    }

    function calTime(uint _timer) internal view returns (uint) {
        return _timer == 0 ? 0 : block.timestamp.sub(_timer);
    }

    function perSecR(uint _amount, uint apy, uint _stakeTime) internal view returns (uint,uint,uint) {
        uint totalReward = (_amount.mul(apy).mul(_stakeTime)).div(365*86400*10000);
        uint aPersec = _amount.div(_stakeTime);
        uint rPersec = totalReward.div(_stakeTime);
        return (aPersec , rPersec , totalReward);
    }

    function getApy() public view returns (uint256) {
        return SAPY;
    }

    function getBalance() external view returns (uint256) {
        return DeenCoin.balanceOf(address(this));
    }

    function setDeenCoin(address _token) external onlyOwner {
        DeenCoin = IERC20(_token);
    }

    function setWithoutstaked(bool _without_staked) external onlyOwner {
        without_staked = _without_staked;
    }

    function setPauser(bool _status) external onlyOwner {
        require(paused != _status,"Error: Not Changed!");
        paused = _status;
    }

    function setWPauser(bool _status) external onlyOwner {
        require(wpaused != _status,"Error: Not Changed!");
        wpaused = _status;
    }

    function rescueFunds() external onlyOwner {
        (bool os,) = payable(owner()).call{value: address(this).balance}("");
        require(os);
    }

    function rescueToken(address _token) external onlyOwner {
        uint balance = IERC20(_token).balanceOf(address(this));
        IERC20(_token).transfer(owner(),balance);
    }

    function setSAPY(uint256 _apy) public onlyOwner{
        SAPY = _apy;
    }

    function setLevelRate(uint256[] memory _levelRate) public onlyOwner{
        levelRate = _levelRate;
    }

    function setminReward(uint256 _minReward) public onlyOwner{
        minReward = _minReward;
    }

    function setDefaultRef(address _refAddress) public onlyOwner{
        defaultRef = _refAddress;
    }

    function sum(uint[] memory data) public pure returns (uint) {
        uint S;
        for(uint i;i < data.length;i++) {
            S += data[i];
        }
        return S;
    }

    function hasReferrer(address addr) public view returns(bool){
        return accounts[addr].referrer != address(0);
    }

    function getTime() public view returns(uint256) {
        return block.timestamp; // solium-disable-line security/no-block-members
    }

    function isCircularReference(address referrer, address referee) internal view returns(bool){
        address parent = referrer;

        for (uint i; i < levelRate.length; i++) {
            if (parent == address(0)) {
                break;
            }

            if (parent == referee) {
                return true;
            }

            parent = accounts[parent].referrer;
        }

        return false;
    }

    function addReferrer(address referrer) internal returns(bool){
        if (referrer == address(0)) {
            emit RegisteredRefererFailed(msg.sender, referrer, "Referrer cannot be 0x0 address");
            return false;
        } else if (isCircularReference(referrer, msg.sender)) {
            emit RegisteredRefererFailed(msg.sender, referrer, "Referee cannot be one of referrer uplines");
            return false;
        } else if (accounts[msg.sender].referrer != address(0)) {
            emit RegisteredRefererFailed(msg.sender, referrer, "Address have been registered upline");
            return false;
        }

        Account storage userAccount = accounts[msg.sender];
        Account storage parentAccount = accounts[referrer];
        userAccount.referrer = referrer;
        parentAccount.referredCount = parentAccount.referredCount.add(1);

        emit RegisteredReferer(msg.sender, referrer);
        return true;
    }

   
    function payReferral(uint256 value) internal returns(uint256){
       Account memory userAccount = accounts[msg.sender];
        uint totalReferal;

        for (uint i; i < levelRate.length; i++) {
            address parent = userAccount.referrer;
            Account storage parentAccount = accounts[userAccount.referrer];

            if (parent == address(0)) {
                break;
            }

            uint c = value.mul(levelRate[i]).div(10000);
            totalReferal = totalReferal.add(c);
            
            parentAccount.reward = parentAccount.reward.add(c);
            if(!teamMember[parent][msg.sender]){
                team[i][parent] += 1; 
            }
            userReferrals[parent][i].push(ReferralHistory(
                msg.sender,
                value,
                c,
                block.timestamp
            ));
            IERC20(DeenCoin).transfer(parent,c);
            emit PaidReferral(msg.sender, parent, c, i + 1);
            userAccount = parentAccount;
        }
        
        return totalReferal;
    }
}