// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.6.0) (token/ERC20/IERC20.sol)

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


// OpenZeppelin Contracts v4.4.1 (security/Pausable.sol)

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
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        require(!paused(), "Pausable: paused");
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
        require(paused(), "Pausable: not paused");
        _;
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


// OpenZeppelin Contracts v4.4.1 (access/Ownable.sol)

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

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
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

// File: contracts/Stakes/StakeGfamBusd.sol


pragma solidity ^0.8.4;




contract StakeGfamBusd is Ownable, Pausable {

    mapping ( address => bool ) private  allowedContracts;
    
    IERC20 public gfamToken;
    IERC20 public busdToken; 
    ITokenTrade public tokenTrade;
    address public feesWallet;

    uint public rewardRate;
    uint public commissionRate;
    uint public feesRate;
    uint public earningDate;
    uint public offset;

    uint public totalGfam;
    uint public globalGfamValue;
    uint public totaRequestedBusd;
    uint public rewardBusdPool;
    uint public rewardDistributionBalance;
    uint public ReferralClaimsBalance;

    mapping(address => Stake[]) public stakes;
    mapping(address => uint) public stakesPerUser;
    mapping(address => address) public referrals;
    mapping(address => uint) public referralClaims;
    uint public currentStakes = 0;
    bool public lockedUnstake;
    bool public lockedWithdrawReward;
    bool public allowRef;


    constructor() {
        feesWallet = 0x09C6FC900bfDF6702FaF72535d7dc5f75f23E852;
        gfamToken = IERC20(0x772b609D3A8F2Ebe1c1b8F87EBda2e462eC475F8);
        busdToken = IERC20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56);
        tokenTrade = ITokenTrade(0x98a3B6e493E24C5A28576dced763C742DEC06cB8);
        rewardRate = 25;
        commissionRate = 10;
        feesRate = 5;
        lockedUnstake = true;
        lockedWithdrawReward = false;
        allowRef = true;
        offset = 1;

    }

    struct Stake {
        uint256 initialAmount;
        uint256 gfamValue;
        uint256 stakeValue;
        uint256 rewardGoal;
        uint256 rewardGoalNet;
        uint256 startDay;
        uint256 lastClaimDay;
        uint256 rewardClaimed;
        uint256 rewardClaimedNet;
        bool ended;
    }

    struct TokensData {
        uint totalGfam;
        uint rewardBusdPool;
        uint rewardDistributionBalance;
        uint totaRequestedBusd;
        uint ReferralClaimsBalance;
    }

    event stakeEvent(address _user, uint _amount, uint _gfamValue);
    event claimEvent(address _user, uint _amount, uint _goal);
    event restakeEvent(address _user, uint _amount, uint _gfamValue);
    event claimRewardEvent(address _user, uint _amount, uint _gfamValue);

    //STAKE
    function stake(uint256 _amount, address _referral) public whenNotPaused {
        address finRef;
        if(referrals[msg.sender] == address(0) && _referral != address(0)){referrals[msg.sender] = _referral;}
        if(referrals[msg.sender] != address(0)){finRef = referrals[msg.sender];}else{finRef = feesWallet;}
        if(allowRef){
            referralClaims[finRef] += _amount*commissionRate/100;
            ReferralClaimsBalance +=  _amount*commissionRate/100;
        }
        gfamToken.transferFrom(msg.sender, address(this), _amount);
        Stake memory _stake = __stake(_amount);
        stakes[msg.sender].push(_stake);
        stakesPerUser[msg.sender] += 1;
        //GLOBAL
        totaRequestedBusd += _stake.rewardGoal;
        totalGfam += _stake.initialAmount;
        currentStakes += 1;
        globalGfamValue += _stake.stakeValue;
        emit stakeEvent(msg.sender, _amount, getPrice(0));
    }
    function restake(uint _stakeIndex) public whenNotPaused {
        Stake storage _stake = stakes[msg.sender][_stakeIndex];
        require(_stake.rewardClaimedNet >= _stake.rewardGoalNet, "Claim your reward before restaking");
        uint gfamValue = getPrice(0);
        uint _amount = _stake.stakeValue*1e18/gfamValue;
        _stake.initialAmount = _amount;
        _stake.gfamValue = gfamValue;
        _stake.startDay = block.timestamp;
        _stake.rewardClaimed = 0;
        _stake.rewardClaimedNet = 0;
        _stake.ended = false;
        //GLOBAL
        totaRequestedBusd += _stake.rewardGoal;
        emit restakeEvent(msg.sender, _amount, gfamValue);

    }
    function __stake(uint256 _amount) internal view returns(Stake memory) {
        uint gfamValue = getPrice(_amount);
        uint stakeValue = _amount*gfamValue/1e18;
        uint goal = (stakeValue*rewardRate)/100;
        uint _rewardGoalNet = (goal*100)/(100-feesRate);
        Stake memory newStake = Stake({
            initialAmount: _amount,
            gfamValue: gfamValue,
            stakeValue: stakeValue,
            startDay: block.timestamp,
            rewardGoal: goal,
            rewardGoalNet: _rewardGoalNet,
            lastClaimDay: block.timestamp,
            rewardClaimed: 0,
            rewardClaimedNet: 0,
            ended: false
        });
        return newStake;
    }
    
    function claim(uint256 _stakeIndex) public whenNotPaused {
        require(_stakeIndex < stakes[msg.sender].length, "Invalid stake index");
        require(lockedWithdrawReward, "Locked");
        Stake storage _stake = stakes[msg.sender][_stakeIndex];
        require(!_stake.ended, "Stake Ended");
        require(_stake.startDay>earningDate, "Earning didn't start");
        require( block.timestamp + (1 days * offset) > _stake.startDay , "Waiting to start");
        _stake.lastClaimDay = block.timestamp;
        uint availableReward = getRewardPatial(_stake.rewardGoalNet);
        if(availableReward >= (_stake.rewardGoalNet - _stake.rewardClaimed)){
            availableReward = _stake.rewardGoalNet - _stake.rewardClaimed;
            _stake.ended = true;
        }
        uint _fees = (availableReward * feesRate)/100;
        _stake.rewardClaimed = availableReward - _fees;
        _stake.rewardClaimedNet = availableReward;
        busdToken.transfer(msg.sender, (availableReward - _fees));
        busdToken.transfer(feesWallet, _fees);
        //GLOBAL
        totaRequestedBusd -= availableReward;
        emit claimEvent(msg.sender, availableReward - _fees, _stake.rewardGoalNet);
    }
    function unstake(address _user, uint _stakeIndex) public onlyOwner {
        require(!lockedUnstake, "Locked");
        Stake storage _stake = stakes[msg.sender][_stakeIndex];
        require(_stake.rewardClaimedNet >= _stake.rewardGoalNet, "Claim your reward before unstaking");
        totalGfam -= _stake.initialAmount;
        currentStakes -= 1;
        globalGfamValue -= _stake.gfamValue;
        _removeStake(_user, _stakeIndex);
        gfamToken.transfer(_user, _stake.initialAmount * _stake.gfamValue / getPrice(0));
    }

    function _removeStake (address _user, uint _stakeIndex) private {
        removeItemFromArray(stakes[_user], _stakeIndex);
        if(stakesPerUser[_user]>0) stakesPerUser[_user] -= 1;
        currentStakes -= 1;
    }
    
    function getRewardPatial (uint _totalReward) public view returns (uint) {
        return (_totalReward * rewardBusdPool) / totaRequestedBusd;
    }
    //

    //REFERRAL
    function claimReferralCom() public {
        require(referralClaims[msg.sender] == 0, "Nothing to claim");
        Stake memory _stake = __stake(referralClaims[msg.sender]);
        stakes[msg.sender].push(_stake);
        stakesPerUser[msg.sender] += 1;
        //GLOBAL
        totaRequestedBusd += _stake.rewardGoal;
        totalGfam += _stake.initialAmount;
        currentStakes += 1;
        globalGfamValue += _stake.stakeValue;
        referralClaims[msg.sender] = 0;
        ReferralClaimsBalance -= _stake.initialAmount;
        emit claimRewardEvent(msg.sender, referralClaims[msg.sender], _stake.gfamValue);
    }
    
    function getTokensData() public view returns (TokensData memory){
        return TokensData(
            totalGfam,
            rewardBusdPool,
            rewardDistributionBalance,
            totaRequestedBusd,
            ReferralClaimsBalance
        );
    }

    function getCurrentDay() public view returns (uint){
        return block.timestamp / 1 days;
    }
    
    function getPrice(uint _amount) public view returns(uint){
        return tokenTrade.getPrice(_amount);
    }

    function removeItemFromArray(Stake[] storage array, uint256 index) internal {
        require(index < array.length, "Invalid index");
        if (index < array.length - 1) {array[index] = array[array.length - 1];}
        array.pop();
    }   

    //ADMIN
    function setTokenTradeContract (address _addr) public onlyOwner {tokenTrade = ITokenTrade(_addr);}
    function setFeesWallet (address _addr) public onlyOwner {feesWallet = _addr;}
    function setRewardRate (uint _value) public onlyOwner {rewardRate = _value;}
    function setFeesRate (uint _value) public onlyOwner {feesRate = _value;}
    function setCommissionRate (uint _value) public onlyOwner {commissionRate = _value;}
    function setReferral (address _user, address _ref) public onlyOwner {referrals[_user] = _ref;}
    function setLockedRewardWithdraw (bool _value) public onlyOwner {lockedWithdrawReward = _value;}
    function setLockedUnstake (bool _value) public onlyOwner {lockedUnstake = _value;}
    function setAllowRef (bool _value) public onlyOwner {allowRef = _value;}
    function setUserRef (address _user, address _ref) public onlyOwner {referrals[_user] = _ref;}
    function setOffset (uint _value) public onlyOwner {offset = _value;}
    function addToRewardPool (uint _value) public onlyOwner {
        rewardBusdPool += _value;
        earningDate = block.timestamp;
    }
    function substractFromRewardPool (uint _value) public onlyOwner {rewardBusdPool -= _value;}
    //ADMIN
    function busdBalance () public view returns(uint){return gfamToken.balanceOf(address(this));}
    function gfamBalance () public view returns(uint){return busdToken.balanceOf(address(this));}
    function withdrawGfam (uint _amount) public onlyOwner {gfamToken.transfer(msg.sender, _amount);}
    function withdrawBusd (uint _amount) public onlyOwner {busdToken.transfer(msg.sender, _amount);}
    function withdrawAllGfam () public onlyOwner {gfamToken.transfer(msg.sender, gfamBalance());}
    function withdrawAllBusd () public onlyOwner {busdToken.transfer(msg.sender, busdBalance());}
    //ALLOW
    function pause() public onlyOwner {_pause();}
    function unpause() public onlyOwner {_unpause();}
    modifier onlyAllowedContract (){require(allowedContracts[msg.sender] == true);_;}
    function allowContract ( address _addr ) onlyOwner public  {allowedContracts[_addr ] = true;}    
    function disallowContract ( address _addr ) onlyOwner public  {allowedContracts[_addr ] = false;}    
}

interface ITokenTrade {
    function getPrice(uint256 _amount) external view returns (uint256);
}