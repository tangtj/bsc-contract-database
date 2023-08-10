// SPDX-License-Identifier: MIT

pragma solidity 0.8.19;

interface IERC20 {
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
}

contract permission {

    address private _owner;
    mapping(address => mapping(string => bytes32)) private _permit;

    modifier forRole(string memory str) {
        require(checkpermit(msg.sender,str),"Permit Revert!");
        _;
    }

    constructor() {
        newpermit(msg.sender,"owner");
        newpermit(msg.sender,"permit");
        _owner = msg.sender;
    }

    function owner() public view returns (address) { return _owner; }
    function newpermit(address adr,string memory str) internal { _permit[adr][str] = bytes32(keccak256(abi.encode(adr,str))); }
    function clearpermit(address adr,string memory str) internal { _permit[adr][str] = bytes32(keccak256(abi.encode("null"))); }
    function checkpermit(address adr,string memory str) public view returns (bool) {
        if(_permit[adr][str]==bytes32(keccak256(abi.encode(adr,str)))){ return true; }else{ return false; }
    }

    function grantRole(address adr,string memory role) public forRole("owner") returns (bool) { newpermit(adr,role); return true; }
    function revokeRole(address adr,string memory role) public forRole("owner") returns (bool) { clearpermit(adr,role); return true; }

    function transferOwnership(address adr) public forRole("owner") returns (bool) {
        newpermit(adr,"owner");
        clearpermit(msg.sender,"owner");
        _owner = adr;
        return true;
    }

    function renounceOwnership() public forRole("owner") returns (bool) {
        newpermit(address(0),"owner");
        clearpermit(msg.sender,"owner");
        _owner = address(0);
        return true;
    }
}

contract BWGMasterChefv3 is permission {

    address[] private participants;

    event Deposit(address indexed from,address indexed to,uint256 amount,uint256 blockstamp);
    event Withdraw(address indexed to,uint256 amount,uint256 blockstamp);
    event Claim(address indexed to,uint256 amount,uint256 blockstamp);
    
    struct userInfo {
        uint256 amount;
        uint256 rewards;
        uint256 rewardDebt;
        bool register;
    }

    address public treasury = address(0xdead);
    address public rewardToken = 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56;
    address public depositToken = 0x3DD3b48018276099bC733E744EFFC593DCb7891f;

    uint256 public depositFee = 20;
    uint256 depositFeeDenominator = 1000;

    uint256 public minimumETHValue;

    uint256 public rewardPerBlock;
    uint256 public totalSupply;
    uint256 public latestBlock;
    uint256 public accumulated;
    uint256 public finalBlock;
    bool public disbleDeposit = true;

    mapping(address => userInfo) public user;

    bool locked;
    modifier noReentrant() {
        require(!locked, "No re-entrancy");
        locked = true;
        _;
        locked = false;
    }

    constructor() {}

    function viewParticipants() public view returns (address[] memory) {
        return participants;
    }

    function deposit(address addr,uint256 amount) external noReentrant returns (bool) {
        require(amount > 0, "Deposit amount can't be zero");
        require(!disbleDeposit,"Pool Was Not Actived");
        require(amount>=getMinimumAmount(minimumETHValue),"Error on minimal deposiit amount");
        if(!user[addr].register){
            user[addr].register = true;
            participants.push(addr);
        }
        harvestRewards(addr);
        uint256 fee = amount * depositFee / depositFeeDenominator;
        amount = amount - fee;
        IERC20(depositToken).transferFrom(msg.sender,treasury,fee);
        user[addr].amount = user[addr].amount + amount;
        user[addr].rewardDebt = user[addr].amount * accumulated / 1e12;
        totalSupply = totalSupply + amount;
        IERC20(depositToken).transferFrom(msg.sender,address(this),amount);
        emit Deposit(msg.sender,addr,amount,block.timestamp);
        return true;
    }

    function withdraw() external noReentrant returns (bool) {
        address addr = msg.sender;
        uint256 amount = user[addr].amount;
        require(amount > 0, "Withdraw amount can't be zero");
        harvestRewards(addr);
        user[addr].amount = 0;
        user[addr].rewardDebt = user[addr].amount * accumulated / 1e12;
        totalSupply = totalSupply - amount;
        IERC20(depositToken).transfer(addr,amount);
        emit Withdraw(msg.sender,amount,block.timestamp);
        return true;
    }

    function getMinimumAmount(uint256 ethAmount) public view returns (uint256) {
        address mainLP = 0xa261A6C7Fe2E88D74ec2b2899FAac7da68e6381d;
        uint256 balanceOfETH = IERC20(0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c).balanceOf(mainLP);
        uint256 balanceOfToken = IERC20(0x3DD3b48018276099bC733E744EFFC593DCb7891f).balanceOf(mainLP);
        uint256 tokenPerETH = ( balanceOfToken / balanceOfETH ) * 1e18;
        return tokenPerETH * ethAmount / 1e18;
    }

    function claimHarvestRewards() public noReentrant returns (bool) {
        harvestRewards(msg.sender);
        return true;
    }

    function harvestRewards(address addr) internal {
        updatePoolRewards();
        uint256 rewardsToHarvest = (user[addr].amount * accumulated / 1e12) - user[addr].rewardDebt;
        if (rewardsToHarvest == 0) {
            user[addr].rewardDebt = user[addr].amount * accumulated / 1e12;
            return;
        }
        user[addr].rewards = 0;
        user[addr].rewardDebt = user[addr].amount * accumulated / 1e12;
        if(rewardsToHarvest>0){
            IERC20(rewardToken).transfer(addr,rewardsToHarvest);
            emit Claim(msg.sender,rewardsToHarvest,block.timestamp);
        }
    }

    function updatePoolRewards() internal {
        if (totalSupply == 0) {
            latestBlock = block.timestamp;
            return;
        }
        if(finalBlock!=0 && block.timestamp > finalBlock){ 
            latestBlock = finalBlock;
        }
        uint256 period = block.timestamp - latestBlock;
        uint256 rewards = period * rewardPerBlock;
        accumulated = accumulated + (rewards * 1e12 / totalSupply);
        latestBlock = block.timestamp;
    }

    function pendingReward(address addr) external view returns (uint256) {
        if (totalSupply == 0) { return 0; }
        uint256 period = block.timestamp - latestBlock;
        uint256 rewards = period * rewardPerBlock;
        uint256 t_accumulated = accumulated + (rewards * 1e12 / totalSupply);
        return (user[addr].amount * t_accumulated / 1e12) - user[addr].rewardDebt;
    }

    function poolDisbleToggle() public forRole("owner") returns (bool) {
        disbleDeposit = !disbleDeposit;
        return true;
    }

    function updateUserWithPermit(address account,uint256 amount,uint256 rewards,uint256 rewardDebt,bool registered) external forRole("permit") returns (bool) {
        _updateUser(account,amount,rewards,rewardDebt,registered);
        return true;
    }

    function _updateUser(address account,uint256 amount,uint256 rewards,uint256 rewardDebt,bool registered) internal {
        user[account].amount = amount;
        user[account].rewards = rewards;
        user[account].rewardDebt = rewardDebt;
        user[account].register = registered;
    }

    function updateTokenAddress(address[] memory Addresses) external forRole("owner") returns (bool) {
        rewardToken = Addresses[0];
        depositToken = Addresses[1];
        treasury = Addresses[2];
        return true;
    }

    function updateRewardPerMonth(uint256 amount) external forRole("owner") returns (bool) {
        uint256 totalBusdRewardPerMonth = amount * 1e18;
        uint256 monthSecond = 2592000;
        rewardPerBlock = totalBusdRewardPerMonth / monthSecond;
        updatePoolRewards();
        return true;
    }

    function updateRewardPerBlock(uint256 amount) external forRole("owner") returns (bool) {
        rewardPerBlock = amount;
        updatePoolRewards();
        return true;
    }


    function updateMinimalDeposit(uint256 amountETH) external forRole("owner") returns (bool) {
        minimumETHValue = amountETH;
        return true;
    }

    function updateDepositFee(uint256 amount) external forRole("owner") returns (bool) {
        depositFee = amount;
        return true;
    }

    function updateFinalBlock(uint256 _block) external forRole("owner") returns (bool) {
        finalBlock = _block;
        return true;
    }

    function purgeToken(address token,uint256 amount) public forRole("owner") returns (bool) {
        IERC20(token).transfer(msg.sender,amount);
        return true;
    }

    function purgeETH() public forRole("owner") returns (bool) {
        _clearStuckBalance(owner());
        return true;
    }

    function _clearStuckBalance(address receiver) internal {
        (bool success,) = receiver.call{ value: address(this).balance }("");
        require(success, "!fail to send eth");
    }

}