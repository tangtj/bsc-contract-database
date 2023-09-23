// SPDX-License-Identifier: MIT
pragma solidity >=0.7.0 <0.9.0;

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
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
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
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract Staking is Ownable, ReentrancyGuard {
    uint256 public stakeFee = 0; // stake fee by %
    uint256 public unstakeFee = 20; // unstake fee by %
    address public FeeReceiver = 0x5D9ACfa059EA6063d8133e5a0BcffdcFBC389DB9;
    uint256 public constant WAITING_TIME_TO_UNSTAKE = 1 days;

    enum HistoryType {
        STAKE,
        UNSTAKE,
        CLAIM
    }

    struct UserStakeHistory {
        address user;
        uint256 amount;
        uint256 created_at;
        HistoryType history_type;
    }

    struct Stake {
        uint256 duration;
        uint256 apy; // 1% == 0.01
    }

    struct UserStake {
        address staker;
        uint256 stakeId;
        uint256 amount;
        uint256 start;
        uint256 claimTime; // default 0: claim stake when staking is ended: timestamp: claim stake time when staking is running
        bool claimed;
    }

    event NewStake(uint256 indexed stakeId);
    event UpdatedStake(uint256 indexed stakeId);
    event NewStaking(uint256 indexed id, address indexed stakinger);
    event Claimed(uint256 indexed id);

    UserStakeHistory[] userStakeHistory;

    IERC20 public tokenStaking;

    uint256 public totalStake;
    mapping(uint256 => Stake) public stakes;
    uint256 public totalUserStake;
    mapping(uint256 => UserStake) public userStakes;

    mapping(address => uint256[]) private userStakeIdList;

    mapping(address => uint256) public earned;

    uint256 public totalStakedAmount;

    mapping(address => uint256) public balanceOf;

    constructor(address _stakingToken) {
        tokenStaking = IERC20(_stakingToken);
    }

    modifier stakeExists(uint256 _id) {
        require(_id < totalStake, "Invalid stakeId");
        _;
    }

    modifier userStakeExists(uint256 _id) {
        require(_id < totalUserStake, "Invalid stakeId");
        _;
    }

    function _setStake(uint256 _duration, uint256 _apy, uint256 _id) private onlyOwner {
        stakes[_id].duration = _duration;
        stakes[_id].apy = _apy;
    }

    function addStake(uint256 _duration, uint256 _apy) external onlyOwner {
        _setStake(_duration, _apy, totalStake);
        emit NewStake(totalStake);
        totalStake++;
    }

    function updateStake(uint256 _duration, uint256 _apy, uint256 _id) external onlyOwner stakeExists(_id) {
        _setStake(_duration, _apy, _id);
        emit UpdatedStake(_id);
    }

    function setTokenStaking(address _tokenStaking) external onlyOwner {
        tokenStaking = IERC20(_tokenStaking);
    }

    function updateStakeFee(uint256 _newStakeFee) public onlyOwner {
        require(_newStakeFee <= 100 && _newStakeFee > 0, "Invalid fee!");

        stakeFee = _newStakeFee;
    }

    function updateUnstakeFee(uint256 _newUnstakeFee) public onlyOwner {
        require(_newUnstakeFee <= 100 && _newUnstakeFee > 0, "Invalid fee!");

        unstakeFee = _newUnstakeFee;
    }

    function updateFeeReceiver(address _newFeeReceiver) public onlyOwner {
        FeeReceiver = _newFeeReceiver;
    }

    function withdraw() external onlyOwner {
        uint256 balance = tokenStaking.balanceOf(address(this));
        tokenStaking.transfer(msg.sender, balance);
    }

    function getUserStakeIdList(address _address) public view returns (uint256[] memory) {
        return userStakeIdList[_address];
    }

    function getAllUserStakeHistory() public view returns (UserStakeHistory[] memory) {
        return userStakeHistory;
    }

    function stake(uint256 _stakeId, uint256 _amount) external stakeExists(_stakeId) {
        address sender = _msgSender();

        require(_amount > 0, "Invalid amount!");

        require(tokenStaking.transferFrom(sender, address(this), _amount));

        // save to the userStakes mapping with new value (without claimed and claimTime => claimed = false & claimTime = 0)
        userStakes[totalUserStake].staker = sender;
        userStakes[totalUserStake].stakeId = _stakeId;
        userStakes[totalUserStake].amount = _amount;
        userStakes[totalUserStake].start = block.timestamp;

        userStakeIdList[sender].push(totalUserStake);

        userStakeHistory.push(UserStakeHistory(sender, _amount, block.timestamp, HistoryType.STAKE));

        totalUserStake++;
        totalStakedAmount += _amount;

        emit NewStaking(totalUserStake, sender);
    }

    function claim(uint256 _id) external userStakeExists(_id) nonReentrant {
        address sender = _msgSender();

        UserStake memory uStake_ = userStakes[_id];

        require(sender == uStake_.staker, "Unauthorized!");

        require(!uStake_.claimed, "User has already claimed!");

        Stake memory stake_ = stakes[uStake_.stakeId];

        if (uStake_.claimTime == 0) {
            // UNSTAKE: CALC WAITING TIME TO EXECUTE
            if (block.timestamp < uStake_.start + stake_.duration) {
                userStakes[_id].claimTime = block.timestamp + WAITING_TIME_TO_UNSTAKE;
            }
            // CLAIM
            else {
                uint256 reward_ = uStake_.amount + pendingReward(_id);

                require(tokenStaking.transfer(sender, reward_));

                userStakes[_id].claimed = true;

                totalStakedAmount -= uStake_.amount;

                userStakeHistory.push(UserStakeHistory(sender, reward_, block.timestamp, HistoryType.CLAIM));

                emit Claimed(_id);
            }
        }
        // RETRIEVE AFTER UNSTAKE WAITING TIME ENDED
        else {
            require(block.timestamp >= uStake_.claimTime, "Unable to claim yet!");

            uint256 realAmount = (uStake_.amount * (100 - unstakeFee)) / 100;

            require(tokenStaking.transfer(sender, realAmount));

            require(tokenStaking.transfer(FeeReceiver, uStake_.amount - realAmount));

            userStakes[_id].claimed = true;

            totalStakedAmount -= uStake_.amount;

            userStakeHistory.push(UserStakeHistory(sender, realAmount, block.timestamp, HistoryType.UNSTAKE));

            emit Claimed(_id);
        }
    }

    function pendingReward(uint256 _id) public view returns (uint256) {
        UserStake memory uStake_ = userStakes[_id];

        if (uStake_.claimed) return 0;

        Stake memory stake_ = stakes[uStake_.stakeId];

        uint256 time_ = block.timestamp < uStake_.start + stake_.duration
            ? block.timestamp - uStake_.start
            : stake_.duration;

        return (uStake_.amount * stake_.apy * time_) / (100 * 365 days);
    }
}