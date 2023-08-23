// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}          



contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
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


    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }

}


abstract contract ReentrancyGuard {
   
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

   
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

interface IERC20 {

    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

}



contract LppStake is ReentrancyGuard, Context, Ownable {
 

    event TokensPurchased(address  purchaser, uint256 value, IERC20 token);
    event Purchased(address  purchaser, uint256 value);
    event TokenWithdrawed(address  reciever, uint256 value, IERC20 token);
    event Withdrawed(address  reciever, uint256 value);
    event RewardsWithdrawn(address  reciever, uint256 value);

    uint256 private constant REWARD_PERCENTAGE = 2;
    uint256 private constant REWARD_DURATION = 1500 days;
    uint256 private constant MIN_STAKING_DURATION = 600 days;

    address public tokenAddress;

    struct UserStakingInfo {
        uint256 totalStakedAmount;
        uint256 stakingTimestamp;
    }

    mapping(address => UserStakingInfo) private userStaking;

    constructor ()  {
        tokenAddress = address(0xF14fE8ea9D35cCEA2545985d7541CcAd02cb6112);
    }   

    function stake(uint256 amount, IERC20 _token) external nonReentrant {
        uint256 weiAmount = amount;
        require(_token == IERC20(tokenAddress),"Token Address doesnt match");
        require(_token.balanceOf(msg.sender) >= amount, "Balance is Low");
        require(_token.allowance(msg.sender, address(this)) >= amount, "Allowance not given for Buying Token");
        require(_token.transferFrom(msg.sender, address(this), amount), "Couldnt Transfer Amount");

        // Record the user's staking information
        userStaking[msg.sender].totalStakedAmount += amount;
        userStaking[msg.sender].stakingTimestamp = block.timestamp;

        emit TokensPurchased(msg.sender, weiAmount, _token);
    }

    function getUserStakingInfo(address userAddress) external view returns (uint256 totalStaked, uint256 rewardsEarned) {
        UserStakingInfo memory info = userStaking[userAddress];
        uint256 stakingDuration = block.timestamp - info.stakingTimestamp;
        
        if (stakingDuration >= MIN_STAKING_DURATION) {
            rewardsEarned = (info.totalStakedAmount * REWARD_PERCENTAGE * stakingDuration) / REWARD_DURATION;
        }
        
        totalStaked = info.totalStakedAmount;
    }

    function withdrawRewards() external nonReentrant {
        IERC20 token_ = IERC20(tokenAddress);
        UserStakingInfo storage info = userStaking[msg.sender];
        uint256 stakingDuration = block.timestamp - info.stakingTimestamp;
        
        require(stakingDuration >= MIN_STAKING_DURATION, "Rewards can be claimed after 600 days");

        uint256 rewardsEarned = (info.totalStakedAmount * REWARD_PERCENTAGE * stakingDuration) / REWARD_DURATION;

        // Transfer the rewards to the user
        require(rewardsEarned > 0, "No rewards to withdraw");
        require(address(this).balance >= rewardsEarned, "Insufficient contract balance");

        info.stakingTimestamp = block.timestamp; // Reset the staking timestamp
        token_.transfer(msg.sender,rewardsEarned);

        emit RewardsWithdrawn(msg.sender, rewardsEarned);
    }


    function removeBnbStuck() external nonReentrant onlyOwner {
        payable(owner()).transfer(address(this).balance);
    }

    function replaceTokenAddress(address _tokenAddress) external onlyOwner{
        tokenAddress = _tokenAddress;
    }
    
    function foreignTokens(IERC20 _tokenAddress) external nonReentrant onlyOwner{
        IERC20 token_ = _tokenAddress;
        uint256 tokenAmt = token_.balanceOf(address(this));
        require(tokenAmt > 0, 'BEP-20 balance is 0');
        token_.transfer(owner(), tokenAmt);
    }

    // External view function to fetch staking information for a user
    function getUserStakingInformation(address userAddress) external view returns (uint256, uint256) {
        UserStakingInfo memory stakingInfo = userStaking[userAddress];
        return (stakingInfo.totalStakedAmount, stakingInfo.stakingTimestamp);
    }


}