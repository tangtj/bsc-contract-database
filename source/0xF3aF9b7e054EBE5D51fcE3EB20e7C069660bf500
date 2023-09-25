pragma solidity ^0.8.18;


// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)
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
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
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
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

interface IRewardPool {
    

    function claim(uint256 rewardType,address user,uint256 amount,bytes memory signature)external;

    function withdraw(address to,uint256 amount) external;

    function getClaimed(address user,uint256 rewardType)external view returns(uint256);

    function getTotalClaimed()external view returns(uint256);

    function getUserTotalClaimed(address user)external view returns(uint256);

}

// SPDX-License-Identifier: UNLICENSED
contract DepositMergeT {

    mapping(address => bool) private  owners;
    mapping(address => address) public ref;

    uint256[] private sysLevelPercents;
    uint256[] private levelUpAmount;
    Sys internal sys;

    mapping(address => NodeUser) nodes;
    address[] nodesArr;

    uint256 public minDeposit;
    uint256 public amountMax;
    uint256 public xPower;

    address public mask;

    
    error AlreadyBound();
    error CantBeMyself();
    error UnRegister();
    error RefUnRegister();
    error MaximumLimitExceeded();
    error MinimumLimitExceeded();
    error WrongQuantity();
    error NotYetSettlementTime();
    error NoStaticReward();
    error NotEOA();
    error NoReward();
    error NoContributionValue();

    struct NodeUser {
        address addr;
        uint256 balance;
        bool isNode;
        bool    isDelete;
        uint256 lv;
        uint256 timestamp;
    }

    struct Sys {
        uint256 totalRegister;
        uint256 totalDepositUsers;
        uint256 totalAmount;
        uint256 totalAmountX;
        uint256 startTimes;
        uint256 nowTimes;
        uint256 lastTimes;
        uint256 amountMax;
        uint256 minDeposit;
        uint256 xPower;
    }

    struct User {
        address addr;
        address ref;
        uint256 depositCount;
        uint256 refCount;
        uint256 level;
        uint256 createdTime;
        uint256 hisDeposit;
        uint256 hisDepositX;
        uint256 totalTeamDeposit;
        uint256 totalLevel101Deposit;
        uint256 otherTeamDeposit;
        address maxTeamAddr;
        uint256 maxTeamDeposit;
        uint256 thisWeekAmount;
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    modifier onlyRegister(){
        if(ref[msg.sender] == address(0)) revert UnRegister();
        _;
    }

    constructor(address _mask){
        mask = _mask;
        owners[msg.sender] = true;

        levelUpAmount = [
        0,
        700e18,
        3500e18,
        17000e18,
        50000e18,
        130000e18,
        260000e18,
        500000e18
        ];

        sysLevelPercents.push(5);//M1
        sysLevelPercents.push(3);//M2
        sysLevelPercents.push(3);//M3
        sysLevelPercents.push(3);//M4
        sysLevelPercents.push(2);//M5
        sysLevelPercents.push(2);//M6
        sysLevelPercents.push(2);//M7

        sys.xPower = 200;
        sys.amountMax = ~uint256(0);
        minDeposit = 30e18;
        sys.minDeposit = minDeposit;
        amountMax = ~uint256(0);
    }






    function _checkOwner() internal view virtual {
        require(owners[msg.sender], "Ownable: caller is not the owner");
    }
    function setOwner(address owner,bool auth) public onlyOwner {
        owners[owner] = auth;
    }
    function isOwner(address owner) public view returns (bool) {
        return owners[owner];
    }


    function adminSetMaxDeposit(uint256 max) external onlyOwner {
        sys.amountMax = max;
        amountMax = max;
    }
    function adminSetMinDeposit(uint256 min) external onlyOwner {
        minDeposit = min;
        sys.minDeposit = min;
    }

    function adminSetSysX(uint256 _xPower) external onlyOwner {
        sys.xPower = _xPower;
    }

    function adminSetSysLevelPercent(uint256[] memory lvPercent_) external onlyOwner {
        require(lvPercent_.length == sysLevelPercents.length,"length error");
        sysLevelPercents = lvPercent_;
    }

    function adminSetLevelUpAmount(uint256 lv, uint256 amount) external onlyOwner {
        levelUpAmount[lv] = amount;
    }
    
    function adminSetLevelUpAmounts(uint256[] memory amounts) external onlyOwner {
        require(amounts.length == 8,"length 8");
        levelUpAmount = amounts;
    }

    function adminSetNodeUser(address[] memory addrs) external onlyOwner {
        for(uint256 i = 0;i < addrs.length; i++){
            NodeUser storage node = nodes[addrs[i]];
            if(node.addr != address(0)){
                node.isNode = true;
                node.isDelete = false;
            }else{
                node.addr = addrs[i];
                node.isNode = true;
                nodesArr.push(addrs[i]);
            }
        }
    }

    function adminRemoveNodeUser(address[] memory addrs) external onlyOwner {
        for(uint256 i = 0;i < addrs.length; i++){
            NodeUser storage node = nodes[addrs[i]];
            node.addr = addrs[i];
            node.isNode = false;
        }
    }


    function getSysLevelPercent() external view returns (uint256[] memory) {
        return sysLevelPercents;
    }
    function getLevelUpAmount() external view returns(uint256[] memory) {
        return levelUpAmount;
    }
    function getSys() external view returns (Sys memory){
        return sys;
    }
    function getNodes() external view returns (NodeUser[] memory) {
        NodeUser[] memory arr = new NodeUser[](nodesArr.length);
        for(uint256 i = 0;i < nodesArr.length; i++){
            arr[i] = nodes[nodesArr[i]];
        }
        return arr;
    }

    function getUser(address addr) external view returns(User memory user) {
        
        return User({
            addr: addr,
            ref: ref[addr],
            depositCount: 0,
            refCount: 0,
            level: 0,
            createdTime: block.timestamp,
            hisDeposit: 0,
            hisDepositX: 0,
            totalTeamDeposit: 0,
            totalLevel101Deposit: 0,
            otherTeamDeposit: 0,
            maxTeamAddr: address(0),
            maxTeamDeposit: 0,
            thisWeekAmount: 0
        });
    }




    /**************************************** upgrade code ***************************************************/
    address public rewardPool;
    address constant public receiveAddress = 0xc8254F21589122d83E5b72509e14c4e6a5D8F957;
    address constant public defaultAddress = 0x6a7D8a382dC3Ae603E23B34698996b6e09AafbC1;

    event Register(address user,address ref);
    event Deposit(address user,uint256 amount);
    event SetRewardPool(address newRewardPool);

    function adminSetRewardPool(address rewardPoolAddr) external onlyOwner{
        rewardPool = rewardPoolAddr;
        emit SetRewardPool(rewardPoolAddr);
    }

    function withdraw(uint256 amount)external onlyOwner{
        IRewardPool(rewardPool).withdraw(receiveAddress,amount);
    }

    function claim(uint256 rewardType,uint256 amount,bytes memory signature) external {
        IRewardPool(rewardPool).claim(rewardType,msg.sender,amount,signature);
    }

    function getClaimed(address user,uint256 rewardType)external view returns(uint256){
        return IRewardPool(rewardPool).getClaimed(user,rewardType);
    }

    function getUserTotalClaimed(address user)external view returns(uint256){
        return IRewardPool(rewardPool).getUserTotalClaimed(user);
    } 

    function register(address superior_) external {
       if(ref[msg.sender] != address(0)) revert AlreadyBound();
        if(superior_ == msg.sender) revert CantBeMyself();
        if(ref[superior_] == address(0) && superior_ != defaultAddress) revert RefUnRegister();
        ref[msg.sender] = superior_;
    }

    function deposit(uint256 amount) external onlyRegister {
        require(
            amount >= minDeposit && amount <= amountMax,
            "The amount must be greater than 30"
        );
        IERC20(mask).transferFrom(msg.sender, rewardPool, amount);
        emit Deposit(msg.sender, amount);
    }

    




}