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

interface ITopReward {
    function getCurrentCycle() external view returns (uint256,uint256);
    function cycleMemory(uint256 cycle,string memory key) external view returns (uint256);
    function getTopWinner(uint256 cycle) external view returns (address[] memory,uint256[] memory);   
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

contract MooTopClaimer is permission {

    ITopReward topReward;

    uint256[] dividend = [2,2,2,2,2,2,2,2,2,2,5,5,10,20,40];

    mapping(address => uint256) public totalClaim;
    mapping(address => mapping(uint256 => bool)) public claimed;

    bool locked;
    modifier noReentrant() {
        require(!locked, "No re-entrancy");
        locked = true;
        _;
        locked = false;
    }

    constructor(address TopClaimContract) {
        topReward = ITopReward(TopClaimContract);
    }

    function claimTopReward(address account,uint256 cycle) public noReentrant returns (bool) {
        (bool shouldClaim,uint256 reward) = isCanClaim(account,cycle);
        require(shouldClaim && reward > 0,"Error to claim or have not enough requirement");
        IERC20(0x6d6F4afbe38A04d15399EabB47Edfdd78c12D729).transfer(account,reward);
        totalClaim[account] += reward;
        claimed[account][cycle] = true;
        return true;
    }

    function isCanClaim(address account,uint256 cycle) public view returns (bool,uint256) {
        (address[] memory addrs,) = topReward.getTopWinner(cycle);
        (bool isWinner,uint256 index) = isAddressWinner(addrs,account);
        (,uint256 currentCycle) = topReward.getCurrentCycle();
        uint256 totalRewardPaid = topReward.cycleMemory(cycle,"rewardTOKEN");
        uint256 reward = totalRewardPaid * dividend[index] / 100;
        if(isWinner && !claimed[account][cycle] && currentCycle>cycle){
            return (true,reward);
        }
        return (false,reward);
    }

    function isAddressWinner(address[] memory checkData,address account) public pure returns (bool,uint256) {
        for (uint256 i = 0; i < checkData.length; i++) {
            if(account==checkData[i]){ return (true,i); }
        }
        return (false,0);
    }

    function purgeToken(address token,uint256 amount) public forRole("owner") returns (bool) {
        IERC20(token).transfer(msg.sender,amount);
        return true;
    }

    function withdrawETH(uint256 amount) public forRole("owner") returns (bool) {
        (bool success,) = msg.sender.call{ value: amount }("");
        require(success, "MOOMOOUP REVERT: FAIL TO WITHDRAW ETH");
        return true;
    }

    receive() external payable {}
}