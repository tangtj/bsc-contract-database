// SPDX-License-Identifier: MIT

pragma solidity 0.8.19;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender,address recipient,uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner,address indexed spender,uint256 value);
}

interface IMLMUser {
    function getUserRegistered(address account) external view returns (bool);
    function getUserUpline(address account,uint256 level) external view returns (address[] memory);
    function register(address referree,address referral) external returns (bool);
    function pushUserReferreeWithPermit(address referree,address referral,uint256 level) external returns (bool);
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

contract StakingData is permission {

    IMLMUser public user;

    address[] staker;

    struct Stake {
        uint256 balance;
        uint256 claimed;
        mapping(uint256 => uint256) matching;
        uint256 genesisBlock;
        uint256 latestBlock;
    }

    mapping(address => Stake) public stake;

    address public depositToken;
    address public rewardToken;
    address public feeReceiver;

    uint256 public returnROI = 10;
    uint256 public depositFee = 20;
    uint256 public denominator = 1000;
    uint256[] matchingAmount = [150,50,30,10,10,10,10,10,10,10];

    uint256 public totalStaked;
    uint256 public totalPaid;

    uint256 day = 86400;

    bool public disableToggle;

    bool locked;
    modifier noReentrant() {
        require(!locked, "No re-entrancy");
        locked = true;
        _;
        locked = false;
    }

    constructor(address userContract) {
        user = IMLMUser(userContract);
    }

    function getStaker() public view returns (address[] memory) {
        return staker;
    }

    function getMatchinAmount() public view returns (uint256[] memory) {
        return matchingAmount;
    }

    function getAccountMatching(address account) public view returns (uint256[] memory) {
        uint256[] memory result = new uint256[](10);
        for(uint256 i=0; i < 10; i++){
            result[i] = stake[account].matching[i];
        }
        return result;
    }

    function depositStaking(address account,uint256 amount,address referral) public noReentrant returns (bool) {
        require(!disableToggle,"Staking Error: Staking Was Not Open");
        address[] memory upline = new address[](10);
        if(!user.getUserRegistered(account)){
            require(account!=referral,"Staking Error: Referral Address Must Not Be Same");
            require(user.getUserRegistered(referral),"Staking Error: Referral Address Must Be Registered");
            user.register(account,referral);
            upline = user.getUserUpline(account,10);
            for(uint256 i=0; i < 10; i++){
                if(upline[i]!=address(0)){
                    user.pushUserReferreeWithPermit(account,upline[i],i);
                }
            }
        }
        if(stake[account].genesisBlock==0){
            stake[account].genesisBlock = block.timestamp;
            staker.push(account);
        }
        internalClaim(account);
        uint256 fee = amount * depositFee / denominator;
        uint256 amountAfterFee = amount - fee;
        IERC20(depositToken).transferFrom(msg.sender,address(this),amountAfterFee);
        IERC20(depositToken).transferFrom(msg.sender,feeReceiver,fee);
        stake[account].balance += amountAfterFee;
        totalStaked += amountAfterFee;
        return true;
    }

    function withdrawStaking(address account,uint256 amount) public noReentrant returns (bool) {
        require(!disableToggle,"Staking Error: Staking Was Not Open");
        internalClaim(account);
        stake[account].balance -= amount;
        totalStaked -= amount;
        IERC20(depositToken).transfer(account,amount);
        return true;
    }

    function Claim(address account) public returns (bool) {
        require(!disableToggle,"Staking Error: Staking Was Not Open");
        internalClaim(account);
        return true;
    }

    function internalClaim(address account) internal {
        uint256 reward = getCurrentReward(account);
        if(reward>0){
            IERC20(rewardToken).transfer(account,reward);
            stake[account].claimed += reward;
            totalPaid += reward;
            address[] memory upline = user.getUserUpline(account,10);
            for(uint256 i=0; i < 10; i++){
                if(upline[i]!=address(0)){
                    uint256 matching = reward * matchingAmount[i] / denominator;
                    IERC20(rewardToken).transfer(upline[i],matching);
                    stake[upline[i]].matching[i] += matching;
                }
            }
        }
        stake[account].latestBlock = block.timestamp;
    }

    function getCurrentReward(address account) public view returns (uint256) {
        if(stake[account].latestBlock==0){ return 0; }
        uint256 period = block.timestamp - stake[account].latestBlock;
        uint256 reward = stake[account].balance * returnROI / denominator;
        return reward / day * period;
    }

    function toggleDisabled() public forRole("owner") returns (bool) {
        disableToggle = !disableToggle;
        return true;
    }

    function updateDepositToken(address tokenAddress) public forRole("owner") returns (bool) {
        depositToken = tokenAddress;
        return true;
    }

    function updateRewardToken(address tokenAddress) public forRole("owner") returns (bool) {
        rewardToken = tokenAddress;
        return true;
    }

    function updateFeeRecevier(address receiverAddress) public forRole("owner") returns (bool) {
        feeReceiver = receiverAddress;
        return true;
    }

    function updateDepositROI(uint256 roi,uint256 fee,uint256 deno)public forRole("owner") returns (bool) {
        returnROI = roi;
        depositFee = fee;
        denominator = deno;
        return true;
    }

    function updateMatchingAmount(uint256[] memory amounts)public forRole("owner") returns (bool) {
        matchingAmount = amounts;
        return true;
    }

    function updateUserContract(address userContract) public forRole("owner") returns (bool) {
        user = IMLMUser(userContract);
        return true;
    }

    function withdrawToken(address to,address tokenAddress,uint256 tokenAmount) public forRole("owner") returns (bool) {
        IERC20(tokenAddress).transfer(to,tokenAmount);
        return true;
    }

    function withdrawETH(address to,uint256 amountETH) public forRole("owner") returns (bool) {
        (bool success,) = to.call{ value: amountETH }("");
        require(success);
        return true;
    }

}