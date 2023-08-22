/**
    ░█████╗░██╗░░░██╗░█████╗░██╗░░░░░██╗██╗░░██╗
    ██╔══██╗╚██╗░██╔╝██╔══██╗██║░░░░░██║╚██╗██╔╝
    ██║░░╚═╝░╚████╔╝░██║░░╚═╝██║░░░░░██║░╚███╔╝░
    ██║░░██╗░░╚██╔╝░░██║░░██╗██║░░░░░██║░██╔██╗░
    ╚█████╔╝░░░██║░░░╚█████╔╝███████╗██║██╔╝╚██╗
    ░╚════╝░░░░╚═╝░░░░╚════╝░╚══════╝╚═╝╚═╝░░╚═╝
*/



//SPDX-License-Identifier: UNLICENSED


pragma solidity 0.8.11;

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


    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
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

abstract contract ReentrancyGuard {
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    modifier nonReentrant() {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");
        _status = _ENTERED;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}

library SafeMath {
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a);
        uint256 c = a - b;
        return c;
    }
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a);
        return c;
    }
}

contract Cyclix is ReentrancyGuard, Ownable {

    // Plan Settings

    using SafeMath for uint256;
    uint256 public constant decimalNumber = 1000000000000000000;
    uint256 public constant packageAmount = 25;
    uint256 public constant directIncome = 20;
    uint256 public constant poolIncome = 5;

    // events 

    event Register(
        address indexed _registerAddress,
        address indexed _sponsorAddress,
        uint256 indexed _packageAmount,
        uint256 _timestamp
    );

    event DirectIncome(
        address indexed _registerAddress,
        address indexed _receiverAddress,
        uint256 indexed _receiveAmount,
        uint256 _timestamp
    );

    event PoolIncome(
        address indexed _receiverAddress,
        uint256 indexed _count,
        uint256 indexed _receiveAmount,
        uint256 _timestamp
    );

    event WithdrawLostTokens(
        address indexed _tokenAddress,
        address indexed _receiverAddress,
        uint256 indexed _receiveAmount,
        uint256 _timestamp
    );

    event lockAdminAdd(address indexed _account);

    // end events



    // struct

   

    struct User {
        uint256 id;
        address userAddress;
        address sponsorAddress;
        uint256 packageAmount;
        uint256 directs;
        uint256 created_at;
    }

    struct matrixPool {
        uint256 id;
        address userAddress;
        address uplineAddress;
        uint256 count;
        uint256 created_at;
    }
    

    mapping(address => User) public users;
    User[] public usersArray;

    mapping(address => matrixPool) public poolUsers;
    matrixPool[] public poolUsersArray;

    uint256 public userCount;

    // end struct
    


    IERC20 public token;

    mapping(address => bool) public lockAdmin;

    modifier isOwner(address _account) {
        require(lockAdmin[_account] == true);
        _;
    }

    constructor(IERC20 _token) {
        token = _token;   
        setLockAdmin(msg.sender);
    }

    function setLockAdmin(address _account) public onlyOwner {
        lockAdmin[_account] = true;
        emit lockAdminAdd(_account);
    }

    function unsetLockAdmin(address _account) public onlyOwner {
        lockAdmin[_account] = false;
        emit lockAdminAdd(_account);
    }

    //// Code

    function totalUsers() public view returns (uint256) {
        return userCount;
    }
    

    function createUser(address _first_user) public isOwner(msg.sender) {
        require(userCount <= 0, "First user already created!");
        User memory newUser = User(userCount, _first_user, address(0), packageAmount, 0, block.timestamp);
        users[_first_user] = newUser;
        usersArray.push(newUser);

        matrixPool memory poolUser = matrixPool(userCount,_first_user, address(0), 0, block.timestamp);
        poolUsers[_first_user] = poolUser;
        poolUsersArray.push(poolUser);

        userCount++;
        
    }

    function register(address _sponsor_id) public {
        require(userCount > 0, "First user not created yet!");
        require(_sponsor_id != address(0), "Invaild sponsor wallet address!");
        require(msg.sender != address(0), "Invaild wallet address!");
        require(check_user(_sponsor_id) != false, "Sponser wallet address Invaild or not registered with us!");
        require(check_user(msg.sender) != true, "Wallet address already registered with us!");
        require(token.allowance(msg.sender, address(this)) >= packageAmount*decimalNumber, "The contract does not have enough of an allowance to escrow!");
        // phase 2
        uint256 totalPackage = packageAmount*decimalNumber;
        User memory newUser = User(userCount, msg.sender, _sponsor_id, packageAmount, 0, block.timestamp);
        users[msg.sender] = newUser;
        usersArray.push(newUser);

        // directs
        User storage my_sponser = users[_sponsor_id];
        my_sponser.directs += 1;
        usersArray[my_sponser.id].directs += 1;
        address sponserWalletAddress;
        if(my_sponser.directs % 3 == 0){
            User storage my_sponser3 = users[my_sponser.userAddress];
            if(my_sponser3.sponsorAddress != address(0)){
                sponserWalletAddress = my_sponser3.sponsorAddress;
            }else{
                sponserWalletAddress = owner();
            }
        }else{
            if(my_sponser.userAddress != address(0)){
                sponserWalletAddress = my_sponser.userAddress;
            }else{
                sponserWalletAddress = owner();
            }
        }

        token.transferFrom(msg.sender, sponserWalletAddress, directIncome*decimalNumber);
        emit DirectIncome(msg.sender, sponserWalletAddress, directIncome, block.timestamp);

        totalPackage = totalPackage.sub(directIncome*decimalNumber);



        // pool 
        uint256 _id = getPoolUpline();
        address upline_id = poolUsersArray[_id].userAddress;
        matrixPool storage my_pool = poolUsers[upline_id];
        matrixPool memory poolUser = matrixPool(userCount,msg.sender, upline_id, 0, block.timestamp);
        poolUsersArray[_id].count += 1;
        my_pool.count += 1;

        token.transferFrom(msg.sender, upline_id, poolIncome*decimalNumber);
        totalPackage = totalPackage.sub(poolIncome*decimalNumber);
        emit PoolIncome(upline_id, my_pool.count, poolIncome, block.timestamp);

        if(totalPackage > 0){
            token.transferFrom(msg.sender, owner(), totalPackage);
        }
        poolUsers[msg.sender] = poolUser;
        poolUsersArray.push(poolUser);
        userCount++;
        emit Register(msg.sender, _sponsor_id, packageAmount, block.timestamp);
    }

    function check_user(address _userAddress) public view returns (bool) {
        User storage registerUser = users[_userAddress];
        if(registerUser.userAddress == _userAddress){
            return true;
        }else{
            return false;
        }
    }

    function getUpline(address _userAddress) public view returns (bool) {
        User storage registerUser = users[_userAddress];
        if(registerUser.userAddress == _userAddress){
            return true;
        }else{
            return false;
        }
    }

    function getPoolUpline() public view returns (uint256 _id) {
        uint256 id;
        for(uint i=0; i < poolUsersArray.length; i++){
            if(poolUsersArray[i].count < 10){
                    id = i;
                    break;
                }
        }
        return (id);
    }


    function isUserExists(address user) public view returns (bool) {
        return (users[user].id != 0);
    }

    function withdrawLostTokens(address tokenAddress, address _receiver) public isOwner(msg.sender)  { 
       bool transferSuccess = IERC20(tokenAddress).transfer(_receiver, IERC20(tokenAddress).balanceOf(address(this)));
       require(transferSuccess, "Failed to send lost fund");
        emit WithdrawLostTokens(tokenAddress, _receiver, IERC20(tokenAddress).balanceOf(address(this)), block.timestamp);
    }

}