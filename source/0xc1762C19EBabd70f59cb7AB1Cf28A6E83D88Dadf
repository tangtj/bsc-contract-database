// SPDX-License-Identifier: MIT

pragma solidity 0.8.19;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function decimals() external view returns (uint256);
    function symbol() external view returns (string memory);
    function name() external view returns (string memory);
    function getOwner() external view returns (address);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address _owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function mint(address to,uint256 amount) external returns (bool);
    function burn(address from,uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IUser {
    function getUserRegistered(address account) external view returns (bool);
    function register(address referree,address referral) external returns (bool);
    function getUserReferree(address account,uint256 level) external view returns (address[] memory);
    function getUserUpline(address account,uint256 level) external view returns (address[] memory);
    function getUserReferral(address account) external view returns (address);
    function pushUserReferreeWithPermit(address account,address referree,uint256 level) external returns (bool);
}

interface IHelper {
    function presaleClaimed(address account) external view returns (uint256);
}

interface IPresale {
    function balanceOf(address account,address token) external view returns (uint256);
}

interface IDepositor {
    function depositWithPermit(address account,address referral,address userCA,uint256 amountTOKEN,uint256 amountUSD,bool isblockReferralReward) external returns (bool);
    function claimReward(address account,address userCA,bool isblockReferralReward) external returns (bool);
}

contract permission {
    mapping(address => mapping(string => bytes32)) private permit;

    function newpermit(address adr,string memory str) internal { permit[adr][str] = bytes32(keccak256(abi.encode(adr,str))); }

    function clearpermit(address adr,string memory str) internal { permit[adr][str] = bytes32(keccak256(abi.encode("null"))); }

    function checkpermit(address adr,string memory str) public view returns (bool) {
        if(permit[adr][str]==bytes32(keccak256(abi.encode(adr,str)))){ return true; }else{ return false; }
    }

    modifier forRole(string memory str) {
        require(checkpermit(msg.sender,str),"Permit Revert!");
        _;
    }
}

contract MooMoohHelper is permission {
    
    address public owner;
    address public depositor = 0xf08133A1a2a0301832A03DF8D8F0C4406E39c5E6;
    address public presale = 0x6b9640F10790dad56c92C35dFbCFF5bbefdb14dc;
    address public userCA = 0x91EdA823eeddc18d89Ec5124517bA7F14820Ad86;

    address public tokenMOO = 0x6d6F4afbe38A04d15399EabB47Edfdd78c12D729;
    address public tokenTracker = 0xe740DbBC235b876EdE13D7427574cf46b06F8d66;
    address public tokenRenew = 0x00aC5cc0DEAFbbba47c2B4A35Ff45d636b1abb83;

    address BUSD = 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56;
    address USDT = 0x55d398326f99059fF775485246999027B3197955;

    address HelperV1 = 0xE36FaBe24cA57f6b88755c5E7d57960742d1D2d7;
    
    mapping(address => uint256) public thisClaimed;

    bool locked;
    modifier noReentrant() {
        require(!locked, "No re-entrancy");
        locked = true;
        _;
        locked = false;
    }

    constructor() {
        newpermit(msg.sender,"permit");
        newpermit(msg.sender,"owner");
        owner = msg.sender;
    }

    function presaleUSD2MOO(uint256 amount) public pure returns (uint256) {
        return amount / 120000;
    } 

    function getPresaleBalanceUSD(address account) public view returns (uint256) {
        IPresale p = IPresale(presale);
        return p.balanceOf(account,USDT) + p.balanceOf(account,BUSD);
    }

    function presaleClaimed(address account) public view returns (uint256) {
        return IHelper(HelperV1).presaleClaimed(account) + thisClaimed[account];
    }

    function depositWithPresale(address account,address referral) public noReentrant returns (bool) {
        if(getPresaleBalanceUSD(account)>presaleClaimed(account)){
            uint256 divUSD = getPresaleBalanceUSD(account) - presaleClaimed(account);
            uint256 tokenAmount = presaleUSD2MOO(divUSD);
            thisClaimed[account] += divUSD;
            IDepositor(depositor).depositWithPermit(account,referral,userCA,tokenAmount,divUSD,true);
        }
        return true;
    }

    function depositWithRenew(address account,address referral) public noReentrant returns (bool) {
        uint256 renewBalance = IERC20(tokenRenew).balanceOf(account);
        require(renewBalance>=200000000000000);
        IERC20(tokenRenew).burn(account,renewBalance);
        IDepositor(depositor).depositWithPermit(account,referral,userCA,renewBalance,renewBalance*100000,true);
        return true;
    }

    function claimReward(address account) public noReentrant returns (bool) {
        IDepositor(depositor).claimReward(account,userCA,false);
        return true;
    }

    function claimTracker(address account) public noReentrant returns (bool) {
        uint256 claimBalance = IERC20(tokenTracker).balanceOf(account);
        IERC20(tokenTracker).burn(account,claimBalance);
        IERC20(tokenMOO).transfer(account,claimBalance);
        return true;
    }

    function withdrawToken(address token,uint256 amount) public forRole("owner") returns (bool) {
        IERC20(token).transfer(msg.sender,amount);
        return true;
    }

    function withdrawETH(uint256 amount) public forRole("owner") returns (bool) {
        (bool success,) = msg.sender.call{ value: amount }("");
        require(success, "MOOMOOUP REVERT: FAIL TO WITHDRAW ETH");
        return true;
    }

    function grantRole(address adr,string memory role) public forRole("owner") returns (bool) {
        newpermit(adr,role);
        return true;
    }

    function revokeRole(address adr,string memory role) public forRole("owner") returns (bool) {
        clearpermit(adr,role);
        return true;
    }

    function transferOwnership(address adr) public forRole("owner") returns (bool) {
        newpermit(adr,"owner");
        clearpermit(msg.sender,"owner");
        owner = adr;
        return true;
    }

    receive() external payable {}
}