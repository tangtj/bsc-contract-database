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

interface IHelper {
    function current_price() external view returns (uint256);
    function thisClaimed(address account) external view returns (uint256);
    function presaleClaimed(address account) external view returns (uint256);
    function presaleUSD2MOO(uint256 amount) external view returns (uint256);
    function getPresaleBalanceUSD(address account) external view returns (uint256);
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

contract MooMoohHelperV4 is permission {

    address public owner;
    address public depositor = 0xf08133A1a2a0301832A03DF8D8F0C4406E39c5E6;
    address public userCA = 0x91EdA823eeddc18d89Ec5124517bA7F14820Ad86;

    address public tokenMOO = 0x6d6F4afbe38A04d15399EabB47Edfdd78c12D729;
    address public tokenTracker = 0xe740DbBC235b876EdE13D7427574cf46b06F8d66;
    address public tokenRenew = 0x00aC5cc0DEAFbbba47c2B4A35Ff45d636b1abb83;

    address public tokenPair = 0xC47B852b6138A5579340Dbe6fe94cE18F62100fd;
    address public bnbPair = 0x16b9a82891338f9bA80E2D6970FddA79D1eb0daE;
    address public wbnb = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
    address public usdt = 0x55d398326f99059fF775485246999027B3197955;

    address public mooHelperV3 = 0x5c026E9E6213af5c3af6A48B9Fb43e102E1282e5;

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

    function current_price() public view returns (uint256) {
        return IHelper(mooHelperV3).current_price();
    }

    function thisClaimed(address account) public view returns (uint256) {
        return IHelper(mooHelperV3).thisClaimed(account);
    }

    function presaleUSD2MOO(uint256 amount) public view returns (uint256) {
        return IHelper(mooHelperV3).presaleUSD2MOO(amount);
    } 

    function getPresaleBalanceUSD(address account) public view returns (uint256) {
        return IHelper(mooHelperV3).getPresaleBalanceUSD(account);
    }

    function presaleClaimed(address account) public view returns (uint256) {
        return IHelper(mooHelperV3).presaleClaimed(account);
    }

    function getBNBPerToken() public view returns (uint256) {
        uint256 bnb = IERC20(wbnb).balanceOf(tokenPair) * 1e18;
        uint256 token = IERC20(tokenMOO).balanceOf(tokenPair) * 1e12;
        return bnb / token;
    }

    function getBNBPrice() public view returns (uint256) {
        uint256 wbnbPair = IERC20(wbnb).balanceOf(bnbPair);
        uint256 usdtPair = IERC20(usdt).balanceOf(bnbPair);
        return ( usdtPair * 1e18 ) / wbnbPair;
    }

    function getMooPrice() public view returns (uint256) {
        return getBNBPerToken() * getBNBPrice() / 1e24;
    }

    function getMooPriceAmount(uint256 amount) public view returns (uint256) {
        return getMooPrice() * amount;
    }


    function depositWithToken(address account,address referral,uint256 amount) public noReentrant returns (bool) {
        uint256 usdMooPrice = getMooPriceAmount(amount);
        IERC20(tokenMOO).transferFrom(msg.sender,address(this),amount);
        IDepositor(depositor).depositWithPermit(account,referral,userCA,amount,usdMooPrice,false);
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