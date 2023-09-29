// SPDX-License-Identifier: MIT

pragma solidity =0.8.15;

interface IUniswapV2Pair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);

    function mint(address to) external returns (uint liquidity);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;

    function initialize(address, address) external;
}

interface AggregatorV3Interface {
  function decimals() external view returns (uint8);

  function description() external view returns (string memory);

  function version() external view returns (uint256);

  function getRoundData(
    uint80 _roundId
  ) external view returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound);

  function latestRoundData()
    external
    view
    returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound);
}

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

contract FutureCoinStaking is Ownable, ReentrancyGuard {
    IUniswapV2Pair public pair;
    bool private reverse = true;
    AggregatorV3Interface public bnbFeed;
    using SafeMath for uint256;
    IERC20 public FutureCoin;
    uint256 public totalStaked;
    uint256 public totalStakedUsd;
    uint256 public totalUnstaked;
    uint256 public totalDistributed;
    uint256 public totalAmountDistributed;
    uint256 public StakersCount;
    uint public minReward = 500;

    struct StakedRec {
        uint256 _amount;
        uint256 _amountUsd;
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
         uint256 _totalStakedUsd;
        uint256 _totalWithdraw;
        uint256 _totalClaimed;
        uint256 _totalAmounts;
        StakedRec[] _Ledger;
    }
    mapping (address => pool) public _stakers;
    uint[5] public _apy = [2000,3000,4000,5000,6000];
    uint[5] public _time = [31536000,63072000,94608000,126144000,157680000];
    uint256[] public _stakeAmount = [100000000000000000000,300000000000000000000,600000000000000000000,1000000000000000000000,3000000000000000000000,5000000000000000000000,10000000000000000000000,50000000000000000000000];
    bool public paused;
    bool public wpaused;
    bool public without_staked = false;

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
      
    uint256[] public levelRate = [1000,700,500,200,50,50];
    mapping(address => Account) public accounts;
    mapping(address => mapping( uint256 => ReferralHistory[])) public userReferrals;
    address public defaultRef =  0xa30E24827D5Ca9CB3Af99D8186d3d12DF72285f5;
    mapping(address => mapping (address => bool)) private teamMember;
    mapping(uint => mapping(address => uint)) public team;

    constructor() {
        FutureCoin = IERC20(0xa293BcAb2510bc4DdA5430BB2CBA449AC3931F8D);
        bnbFeed = AggregatorV3Interface(0x0567F2323251f0Aab15c8dFb1967E4e8A7D42aeE);
        pair = IUniswapV2Pair(0x2F91e3510960Dac428B4D9A3977E3344f1514E49);
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

    function stake(uint _index,uint _duration,address _referrer) public {
        require(!paused,"Error: Staking is Currently Paused Now!!");
        require(_duration > 0 , "Error:No time found with stakes");
        require(_stakeAmount[_index] > 0 , "Error:No stake plan found with amounts");
        address account = msg.sender;
        uint _amount = _stakeAmount[_index];
        uint _tokenAmount = getAmountsOut(_amount);
        FutureCoin.transferFrom(account, address(this), _tokenAmount);
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
        
        payReferral(_tokenAmount);

         uint256 _runningApy = getAPY(_duration);
        uint endT = block.timestamp.add(_duration);
        (uint aPersec , uint rPersec , uint _tr) = perSecR(_tokenAmount, _runningApy, _duration);
        StakedRec memory _rec = 
                StakedRec(
                    _tokenAmount,
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
        _stakers[account]._totalStaked += _tokenAmount;
        _stakers[account]._totalStakedUsd += _amount;


        totalStaked += _tokenAmount;
        totalStakedUsd += _amount;
        emit Staked(account,_tokenAmount,block.timestamp,_runningApy);
    }

    function getAPY(uint _utime) public view returns(uint){
        if(_utime <= _time[0]){
            return _apy[0];
        }
        else if(_utime > _time[0] && _utime <= _time[1]){
            return _apy[1];
        }
        else if(_utime > _time[1] && _utime <= _time[2]){
            return _apy[2];
        }
        else if(_utime > _time[2] && _utime <= _time[3]){
            return _apy[3];
        }
        else if(_utime > _time[3]){
            return _apy[4];
        }
        else{
            return _apy[0];
        }
    }

    function perSecR(uint _amount, uint apy, uint _stakeTime) internal view returns (uint,uint,uint) {
        uint totalReward = (_amount.mul(apy).mul(_stakeTime)).div(365*86400*10000);
        uint aPersec = _amount.div(_stakeTime);
        uint rPersec = totalReward.div(_stakeTime);
        return (aPersec , rPersec , totalReward);
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

        FutureCoin.transfer(account, total);
        emit Claimed(account,total,block.timestamp);
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

    function calTime(uint _timer) internal view returns (uint) {
        return _timer == 0 ? 0 : block.timestamp.sub(_timer);
    }

    function userTotalStaked(address _user) internal view returns (uint){
        uint total =  _stakers[_user]._totalStaked - _stakers[_user]._totalWithdraw;
        return total;
    }

    function getAmounts() public view returns(uint[] memory){
        return _stakeAmount;
    }

    function userReferral(address _address , uint _level) external view returns(ReferralHistory[] memory){
        uint length = userReferrals[_address][_level].length;
        ReferralHistory[] memory rh = new ReferralHistory[](length);
        for(uint i=0;i<length;i++){
            rh[i] = userReferrals[_address][_level][i];
        }

        return rh;
    }

    function getTokenPrice() public view returns (uint256) {
        (uint256 reserve0, uint256 reserve1, ) = pair.getReserves();
        uint tokenpriceBNB;
        if(reverse){
            tokenpriceBNB = (reserve1 * (10**18)) / reserve0;
        }
        else{
            tokenpriceBNB = (reserve0 * (10**18)) / reserve1;
        }
        
        (, int price, , , ) = bnbFeed.latestRoundData();

        uint priceInDollars = (tokenpriceBNB * uint256(price)) / (10**8);

        return priceInDollars;
    }

    function getAmountsOut(uint _amount) public view returns (uint256){
        uint256 getPrice = getTokenPrice() ;
        return _amount * 10**18 / getPrice;
    }  

    function getList(address _account) external view returns (StakedRec[] memory _rec, uint _size) {
        return (_stakers[_account]._Ledger, _stakers[_account]._Ledger.length);
    }

    function setStakeAmout(uint256[] memory _stakearray) public onlyOwner{
        _stakeAmount = _stakearray;
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

    function setminReward(uint256 _minReward) public onlyOwner{
        minReward = _minReward;
    }

    function getBalance() external view returns (uint256) {
        return FutureCoin.balanceOf(address(this));
    }

    function setFutureCoin(address _token) external onlyOwner {
        FutureCoin = IERC20(_token);
    }

    function setWithoutstaked(bool _without_staked) external onlyOwner {
        without_staked = _without_staked;
    }

    function setPauser(bool _status) external onlyOwner {
        require(paused != _status,"Error: Not Changed!");
        paused = _status;
    }

    function setReverse(bool _value) public onlyOwner{
        reverse = _value;
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

    function setLevelRate(uint256[] memory _levelRate) public onlyOwner{
        levelRate = _levelRate;
    }

    function setDefaultRef(address _refAddress) public onlyOwner{
        defaultRef = _refAddress;
    }

    function setApy(uint[5] memory _apyvalue) public onlyOwner{
      _apy = _apyvalue;
    }

    function setTime(uint[5] memory _stime) public onlyOwner{
      _time = _stime;
    }

    function setPairAddress(address _pairAddress) public onlyOwner{
        pair = IUniswapV2Pair(_pairAddress);
    }

    //chain link feed Address
    function setBNBFees(address _address) public onlyOwner{
        bnbFeed = AggregatorV3Interface(_address);
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
            IERC20(FutureCoin).transfer(parent,c);
            emit PaidReferral(msg.sender, parent, c, i + 1);
            userAccount = parentAccount;
        }
        
        return totalReferal;
    }
}