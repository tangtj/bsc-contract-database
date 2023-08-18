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

contract OneCoinStaking is permission {

    address public OneCoin = 0x3C71d3615AA1F6A77AD80cebA852741312c93393;

    uint256 public totalHolding;
    uint256 public totalStaking;
   
    uint256 public maximum = 100_000_000 * 1e8;
    uint256 public minimum = 1_000_000 * 1e8;
    uint256 public period = 60 * 60 * 24 * 180;
    uint256 public ROI = 1450;
    uint256 public denominator = 1000;

    struct Stake {
        address account;
        uint256 balance;
        uint256 blockstamp;
    }

    mapping(uint256 => Stake) public stake;
    mapping(address => uint256[]) holding;

    bool locked;
    modifier noReentrant() {
        require(!locked, "No re-entrancy");
        locked = true;
        _;
        locked = false;
    }

    constructor() {}

    function getAccountHolding(address account) public view returns (uint256[] memory) {
        return holding[account];
    }

    function stakingFor(address account,uint256 amount) public noReentrant returns (bool) {
        require(amount>=minimum,"Revert By Minimum Amount");
        totalHolding += 1;
        totalStaking += amount;
        require(totalStaking<=maximum,"Revert By Maximum Allocate");
        IERC20(OneCoin).transferFrom(msg.sender,address(this),amount);
        stake[totalHolding].account =  account;
        stake[totalHolding].balance += amount;
        stake[totalHolding].blockstamp = block.timestamp;
        return true;
    }

    function unstake(uint256 id) public noReentrant returns (bool) {
        require(block.timestamp>stake[id].blockstamp+period,"This Staking Id Was Locked");
        address account = stake[id].account;
        uint256 balance = stake[id].balance;
        uint256 amount = balance * ROI / denominator;
        IERC20(OneCoin).transfer(account,amount);
        totalStaking -= balance;
        stake[id].balance = 0;
        return true;
    }

    function changeMaximum(uint256 amount) public forRole("owner") returns (bool) {
        maximum = amount;
        return true;
    }

    function changeMinimum(uint256 amount) public forRole("owner") returns (bool) {
        minimum = amount;
        return true;
    }

    function changePeriod(uint256 amount) public forRole("owner") returns (bool) {
        period = amount;
        return true;
    }

    function changeROI(uint256 amount) public forRole("owner") returns (bool) {
        ROI = amount;
        return true;
    }

    function rescueToken(address token,uint256 amount) public forRole("owner") returns (bool) {
        IERC20(token).transfer(msg.sender,amount);
        return true;
    }

    function rescueETH(uint256 amount) public forRole("owner") returns (bool) {
        (bool success,) = owner().call{ value: amount }("");
        require(success);
        return true;
    }

}