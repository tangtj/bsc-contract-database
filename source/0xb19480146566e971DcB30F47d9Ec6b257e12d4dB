// SPDX-License-Identifier: MIT

pragma solidity 0.8.16;

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
interface IBEP20 {
    function totalSupply() external view returns (uint256);

    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function getOwner() external view returns (address);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

abstract contract Context {
    function _msgSender() internal view returns (address) {
        return msg.sender;
    }

    function _msgData() internal view returns (bytes memory) {
        this;
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }
}

contract LiquiYield_LP_STAKE is Context, IBEP20, Ownable, ReentrancyGuard {
    using SafeMath for uint256;

    address private LP_TOKEN_ADDRESS;

    address private NATIVE_TOKEN_ADDRESS = 0x14004310BB4ecEE69A965131836650EBBA3CeD6B;

    address WITHDRAWAL_ADDRESS = 0x7Fd87261154EC68575d284D14d645D0DD93F3a0B;
    
    address private DEAD = 0x000000000000000000000000000000000000dEaD;
    
    IBEP20 private LP_TOKEN;
    IBEP20 private NATIVE_TOKEN;

    bool lp_updated = false;

    
    //----------- BEP20 Variables -----------//
    mapping(address => uint256) private _balances;
    uint256 private _totalSupply;
    uint256 private _maxSupply;
    uint8 private _decimals;
    string private _symbol;
    string private _name;
    //---------------------------------------//
    //---------------------------------------//

    constructor() {
        _name = "LiquiYield LP Stake";
        _symbol = "LQY_LP_STAKE";
        _decimals = 18;
        _totalSupply = 0;
        
        // LP_TOKEN = IBEP20(LP_TOKEN_ADDRESS);
        NATIVE_TOKEN = IBEP20(NATIVE_TOKEN_ADDRESS);
    }

    function getLPTokenAddress() public view returns (address) {
        return LP_TOKEN_ADDRESS;
    }

    function setLPTokenAddress(address _newAddress) public onlyOwner {
        require(!lp_updated, "LQY: You cannot change LP Address after it has been set");
        require(_newAddress != address(0), "LQY: You cannot change LP Address to the Zero Address");
        require(_newAddress != DEAD, "LQY: You cannot change LP Address to the DEAD Address");

        LP_TOKEN_ADDRESS = _newAddress;
        LP_TOKEN = IBEP20(_newAddress);

        lp_updated = true;
    }

    function getNativeTokenAddress() public view returns (address) {
        return NATIVE_TOKEN_ADDRESS;
    }

    function setNativeTokenAddress(address _newAddress) public onlyOwner {
        NATIVE_TOKEN_ADDRESS = _newAddress;
        NATIVE_TOKEN = IBEP20(_newAddress);
    }

    //----------- BEP20 Functions -----------//

    function getOwner() external view override returns (address) {
        return owner();
    }
    function decimals() external view override returns (uint8) {
        return _decimals;
    }
    function symbol() external view override returns (string memory) {
        return _symbol;
    }
    function name() external view override returns (string memory) {
        return _name;
    }
    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }
    function balanceOf(address account) external view override returns (uint256) {
        return _balances[account];
    }
    function transfer(address recipient, uint256 amount)
        external
        pure
        override
        returns (bool)
    {
        require(recipient != address(0) && amount > 0, "MBP: Recipient and amount required");
        return false;
    }
    function transferFrom(address sender, address recipient, uint256 amount ) external pure override returns (bool) {
        require(sender != address(0), "MBP: Sender required");
        require(recipient != address(0) && amount > 0, "MBP: Recipient and amount required");
        return false;
    }
    function _mintStakeTokens(address account, uint256 amount) internal {
        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
        emit Transfer(address(0), account, amount);
    }
    function _burnStakeTokens(address account, uint256 amount) internal {
        require(account != address(0), "BEP20: burn from the zero address");
        _balances[account] = _balances[account].sub(
            amount,
            "BEP20: burn amount exceeds balance"
        );
        _totalSupply = _totalSupply.sub(amount);
        emit Transfer(account, address(0), amount);
    }
    //---------------------------------------//
    //---------------------------------------//
    
    uint32 constant SECONDS_IN_DAY = 86400 seconds;

    mapping(address => uint256) private AMOUNT_STAKED;

    mapping(address => uint256) private STAKE_DAYS;

    mapping(address => bool) private LP_STAKED;

    uint256 private TOTAL_STAKERS = 0;
    uint256 private TOTAL_STAKED;

    uint64 private MINIMUM_DAYS_TO_UNSTAKE = 10;

    mapping(address => uint256) private LAST_CLAIM_AMOUNT;
    mapping(address => uint256) private LAST_CLAIM_TIME;
    mapping(address => uint256) public LAST_CLAIM_PERIOD;
    uint256 public currentClaimPeriod = 1;
    uint64 private CLAIM_INTERVAL = 7;
    uint256 lastBlock = 1693066907;
    uint256 currentRewardTotal;
    uint256 currentStakeTotal;
    uint256 currentRewardClaimed;
    uint256 totalRewardClaimed;
    mapping(address => uint256) private ELIGIBLE_FROM_ROUND;
    
    event Staked(address indexed user, uint256 amount, uint256 totalStaked);
    event Unstaked(address indexed user, uint256 amount, uint256 totalStaked);
    event RewardClaimed(address account, uint256 reward);

    function getMinimumDaysToUnStake() external view returns (uint64) {
        return MINIMUM_DAYS_TO_UNSTAKE;
    }

    function setMinimumDaysToUnStake(uint64 _minimumDaysToUnstake) external onlyOwner {
        require(
            _minimumDaysToUnstake > 0 && _minimumDaysToUnstake <= 30,
            "LQY: Must be greater than 0 and less than 31"
        );
        MINIMUM_DAYS_TO_UNSTAKE = _minimumDaysToUnstake;
    }

    function getClaimInterval() external view returns (uint64) {
        return CLAIM_INTERVAL;
    }

    function setClaimInterval(uint64 _claimInterval) external onlyOwner {
        require(
            _claimInterval > 0 && _claimInterval <= 30,
            "LQY: Must be greater than 0 and less than 31"
        );
        CLAIM_INTERVAL = _claimInterval;
    }

    function getWithdrawalAddress() external view returns (address) {
        return WITHDRAWAL_ADDRESS;
    }

    function setWithdrawalAddress(address _addr) external onlyOwner {
        WITHDRAWAL_ADDRESS = _addr;
    }


    function stakeLP(uint256 _lpTokenAmount) external nonReentrant {
        require(LP_TOKEN_ADDRESS != address(0), "LQY: LP Address not yet initialised");
        require(
            LP_TOKEN.transferFrom(msg.sender, address(this), _lpTokenAmount),
            "BEP20: Error in token transfer"
        );

        if(!LP_STAKED[msg.sender]){
            LP_STAKED[msg.sender] = true;
            TOTAL_STAKERS = TOTAL_STAKERS.add(1);
        }

        ELIGIBLE_FROM_ROUND[msg.sender] = currentClaimPeriod + 1;
        
        STAKE_DAYS[msg.sender] = block.timestamp;

        TOTAL_STAKED = TOTAL_STAKED + _lpTokenAmount;

        AMOUNT_STAKED[msg.sender] = AMOUNT_STAKED[msg.sender].add(_lpTokenAmount);

        _mintStakeTokens(msg.sender, _lpTokenAmount);

        emit Staked(msg.sender, _lpTokenAmount, TOTAL_STAKED);
    }

    function unstakeLP(uint256 _lpTokenAmount) external nonReentrant {
        require(LP_TOKEN_ADDRESS != address(0), "LQY: LP Address not yet initialised");
        require(LP_STAKED[msg.sender], "MBP: You didn't stake LP Tokens");
        require(_lpTokenAmount <= AMOUNT_STAKED[msg.sender], "MBP: Amount exceed stake");
        
        require(
            ((block.timestamp - STAKE_DAYS[msg.sender]) / SECONDS_IN_DAY) >= MINIMUM_DAYS_TO_UNSTAKE, 
            "MBP: Your stake days isn't up to minimum to unstake"
        );

        if(_lpTokenAmount == AMOUNT_STAKED[msg.sender]){
            STAKE_DAYS[msg.sender] = 0;
            LP_STAKED[msg.sender] = false;
            TOTAL_STAKERS = TOTAL_STAKERS.sub(1);
        }

        TOTAL_STAKED = TOTAL_STAKED - _lpTokenAmount;

        AMOUNT_STAKED[msg.sender] = AMOUNT_STAKED[msg.sender].sub(_lpTokenAmount);

        require(
            LP_TOKEN.transfer(msg.sender, _lpTokenAmount),
            "BEP20: Error in token transfer"
        );

        _burnStakeTokens(msg.sender, _lpTokenAmount);
        
        emit Unstaked(msg.sender, _lpTokenAmount, TOTAL_STAKED);
    }

    function claimReward() external nonReentrant {
        require(LP_TOKEN_ADDRESS != address(0), "LQY: LP Address not yet initialised");
        require(LP_STAKED[msg.sender], "You have no staked LP Tokens");

        uint256 nextClaimBlock = lastBlock.add(CLAIM_INTERVAL * SECONDS_IN_DAY);

        if(block.timestamp > lastBlock && block.timestamp.sub(lastBlock) > (CLAIM_INTERVAL * SECONDS_IN_DAY)){
            lastBlock = block.timestamp.sub(CLAIM_INTERVAL * SECONDS_IN_DAY);
            nextClaimBlock = lastBlock.add(CLAIM_INTERVAL * SECONDS_IN_DAY);
        }

        if (block.timestamp >= nextClaimBlock) {
            lastBlock = nextClaimBlock; 
            currentRewardTotal = NATIVE_TOKEN.balanceOf(address(this));
            currentStakeTotal = TOTAL_STAKED;
            currentRewardClaimed = 0;
            currentClaimPeriod++;
        }
        
        require(ELIGIBLE_FROM_ROUND[msg.sender] <= currentClaimPeriod, "You Cannot claim in this round");

        require(LAST_CLAIM_PERIOD[msg.sender] < currentClaimPeriod, "You already claimed for this period");

        uint256 userPercentage = AMOUNT_STAKED[msg.sender].mul(100).div(currentStakeTotal);
        uint256 rewardToClaim = currentRewardTotal.mul(userPercentage).div(100);

        LAST_CLAIM_PERIOD[msg.sender] = currentClaimPeriod;
        LAST_CLAIM_AMOUNT[msg.sender] = rewardToClaim;
        LAST_CLAIM_TIME[msg.sender] = block.timestamp;

        currentRewardClaimed = currentRewardClaimed.add(rewardToClaim);
        totalRewardClaimed = totalRewardClaimed.add(rewardToClaim);

        require(NATIVE_TOKEN.transfer(msg.sender, rewardToClaim), "Error in transferring reward");

        emit RewardClaimed(msg.sender, rewardToClaim);
    }

    function LP_StakeDaysCount(address _addr) external view returns (uint64) {
        if(LP_STAKED[_addr]){
            return uint64((block.timestamp - STAKE_DAYS[_addr]) / SECONDS_IN_DAY);
        } else {
            return 0;
        }
    }

    function getUserLPAmountStaked(address _addr) external view returns (uint256) {
        return AMOUNT_STAKED[_addr];
    }

    function getUserPercentage(address _addr) external view returns (uint256) {
        if (AMOUNT_STAKED[_addr] == 0) {
            return 0;
        }
        return (AMOUNT_STAKED[_addr] / TOTAL_STAKED) * 100;
    }

    function getUserLastClaim(address _addr) external view returns (uint256) {
        return LAST_CLAIM_AMOUNT[_addr];
    }

    function getUserLastClaimTime(address _addr) external view returns (uint256) {
        if (LAST_CLAIM_TIME[_addr] == 0) {
            return 0;
        }
        return (block.timestamp - LAST_CLAIM_TIME[_addr]) / SECONDS_IN_DAY;
    }

    function getGeneralLastClaimTime() external view returns (uint256) {
        return (block.timestamp - lastBlock) / SECONDS_IN_DAY;
    }

    function getCurrentRewardTotal() external view returns (uint256) {
        return currentRewardTotal;
    }

    function getCurrentStakeTotal() external view returns (uint256) {
        return currentStakeTotal;
    }

    function getCurrentRewardClaimed() external view returns (uint256) {
        return currentRewardClaimed;
    }

    function getTotalRewardClaimed() external view returns (uint256) {
        return totalRewardClaimed;
    }

    function getTotalLPAmountStaked() external view returns (uint256) {
        return LP_TOKEN.balanceOf(address(this));
    }

    function getTotalStakers() external view returns (uint256) {
        return TOTAL_STAKERS;
    }

    function getCurrentClaimPeriod() external view returns (uint256) {
        return currentClaimPeriod;
    }

    function getUserLastClaimPeriod(address _addr) external view returns (uint256) {
        return LAST_CLAIM_PERIOD[_addr];
    }

    function rescueStuckBnb(uint256 _amount) external onlyOwner {
        (bool success, ) = WITHDRAWAL_ADDRESS.call{value: _amount}("");
        require(success);
    } 

    function rescueStuckTokens(address _tokenAddress, uint256 _amount) external onlyOwner {
        IBEP20 BEP20token = IBEP20(_tokenAddress);
        BEP20token.transfer(WITHDRAWAL_ADDRESS, _amount);
    }

}