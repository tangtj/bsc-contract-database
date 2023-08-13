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

contract PresaleRouter is permission {

    address[] wallet;
    uint256[] value;
    uint256 public split;
    uint256 denominator;

    bool public presaleStart;
    bool public presaleEnded;

    address public soldToken;
    address public depositToken;
    uint256 public pricePerToken;

    uint256 public airdropAmount;
    uint256 public airdropMax;

    uint256 public tokenSoldOut;
    uint256 public airdropPaid;
    uint256 airdropBlockNumber;

    address[] contributors;
    uint256[] amounts;
    uint256[] scores;

    struct Presale {
        uint256 id;
        uint256 amount;
        uint256 score;
        address referral;
        address[] referree;
        bool registerd;
    }

    mapping(address => Presale) presale;
    mapping(address => bool) public isClaimedAirdrop;

    constructor() {
        _register(address(this),address(0));
    }

    function startPresale(uint256 _pricePerToken,address _soldToken,address _depositToken) public forRole("owner") returns (bool) {
        require(!presaleStart,"Presale Was Start Already!");
        pricePerToken = _pricePerToken;
        soldToken = _soldToken;
        depositToken = _depositToken;
        presaleStart = true;
        return true;
    }

    function endingPresale() public forRole("owner") returns (bool) {
        require(!presaleEnded,"Presale Was ended Already!");
        presaleEnded = true;
        return true;
    }

    function getPeriodBlock(uint256 blockstamp) public view returns (uint256) {
        if(blockstamp>block.timestamp){ return blockstamp - block.timestamp; }
        return 0;
    }

    function getWalletValue() public view returns (address[] memory wallet_,uint256[] memory value_,uint256 denominator_) {
        return (wallet,value,denominator);
    }

    function updateWalletValue(address[] memory _wallet,uint256[] memory _value,uint256 _denominator) public forRole("owner") returns (bool) {
        wallet = _wallet;
        value = _value;
        denominator = _denominator;
        split = wallet.length;
        return true;
    }

    function updateAirdrop(uint256 _airdropAmount,uint256 _airdropMax) public forRole("owner") returns (bool) {
        airdropAmount = _airdropAmount;
        airdropMax = _airdropMax;
        return true;
    }

    function setDepositToken(address _depositToken) public forRole("owner") returns (bool) {
        depositToken = _depositToken;
        return true;
    }

    function setSoldToken(address _soldToken) public forRole("owner") returns (bool) {
        soldToken = _soldToken;
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

    function contribute(address referee,address referral,uint256 amount) public returns (bool) {
        require(presaleStart,"Presale Not Start Now!");
        require(!presaleEnded,"Presale Was Ended!");
        _register(referee,referral);
        address ref = presale[referee].referral;
        presale[referee].amount += amount;
        amounts[presale[referee].id] = presale[referee].amount;
        presale[ref].score += amount;
        scores[presale[ref].id] = presale[ref].score;
        uint256 soldOutAmount = amount / pricePerToken;
        tokenSoldOut += soldOutAmount;
        IERC20(depositToken).transferFrom(msg.sender,address(this),amount);
        IERC20(soldToken).transfer(msg.sender,soldOutAmount * 1e8);
        _distribute();
        return true;
    }

    function claimAirdrop(address referrer) public returns (bool) {
        require(!isClaimedAirdrop[msg.sender],"Reacts Address Was Claimed!");
        require(presale[referrer].registerd,"Referrer Address Must Be Registered!");
        require(airdropBlockNumber!=block.number,"Only One Reward Paid Per Block!");
        require(airdropPaid+airdropAmount<=airdropMax,"Amount Limit Max Airdrop!");
        require(!isContract(msg.sender),"Reacts By Contract Not Allowed!");
        airdropPaid += airdropAmount;
        isClaimedAirdrop[msg.sender] = true;
        airdropBlockNumber = block.number;
        IERC20(soldToken).transfer(msg.sender,airdropAmount);
        IERC20(soldToken).transfer(referrer,airdropAmount);
        return true;
    }

    function getContributors() public view returns (address[] memory) {
        return contributors;
    }

    function getAmounts() public view returns (uint256[] memory) {
        return amounts;
    }

    function getScores() public view returns (uint256[] memory) {
        return scores;
    }

    function getPresaleAccountData(address account) public view returns (uint256 id_,uint256 amount_,uint256 score_,address referral_,bool registerd_) {
        return (presale[account].id,presale[account].amount,presale[account].score,presale[account].referral,presale[account].registerd);
    }

    function getPresaleAccountReferree(address account) public view returns (address[] memory referree_) {
        return presale[account].referree;
    }

    function _register(address referree,address referral) internal {
        if(!presale[referree].registerd){
            require(referree!=referral,"Referree And Referral Must Not Be Same!");
            contributors.push(referree);
            amounts.push(0);
            scores.push(0);
            presale[referree].id = contributors.length-1;
            presale[referree].referral = referral;
            presale[referral].referree.push(referree);
            presale[referree].registerd = true;
        }
    }

    function _distribute() internal {
        uint256 amount = IERC20(depositToken).balanceOf(address(this));
        for(uint256 i=0; i < split; i++){
            IERC20(depositToken).transfer(wallet[i],amount * value[i] / denominator);
        }
    }

    function isContract(address account) internal view returns (bool){
    uint32 size;
    assembly {
        size := extcodesize(account)
    }
    return (size > 0);
    }
}