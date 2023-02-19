// SPDX-License-Identifier: MIT

pragma solidity ^0.8.4;

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this;
        return msg.data;
    }
}

abstract contract Auth is Context{
    address owner;
    mapping (address => bool) private authorizations;

    constructor(address _owner) {
        owner = _owner;
        authorizations[_owner] = true;
    }

    modifier onlyOwner() {
        require(isOwner(msg.sender)); _;
    }

    modifier authorized() {
        require(isAuthorized(msg.sender)); _;
    }

    function authorize(address adr) public onlyOwner {
        authorizations[adr] = true;
        emit Authorized(adr);
    }

    function unauthorize(address adr) public onlyOwner {
        authorizations[adr] = false;
        emit Unauthorized(adr);
    }

    function isOwner(address account) public view returns (bool) {
        return account == owner;
    }

    function isAuthorized(address adr) public view returns (bool) {
        return authorizations[adr];
    }

    function transferOwnership(address payable adr) public onlyOwner {
        owner = adr;
        authorizations[adr] = true;
        emit OwnershipTransferred(adr);
    }

    event OwnershipTransferred(address owner);
    event Authorized(address adr);
    event Unauthorized(address adr);
}

interface IBEP20 {
    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

interface IDividendTracker {
    function updateRewards() external;
}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

contract DistributeTreasury is Auth {
    using SafeMath for uint256;

    address public busdToken = 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56;
    address public rfvTreasury = 0x94DC0b13E66ABa9450b3Cc44c2643BBb4C264BC7;

    address public referralTreasury;
    IDividendTracker public busdDividendTracker;

    uint256 public rfvRatio = 15 * 100000;
    uint256 public xLiberoRatio = 7 * 90909;
    uint256 public xLiberoReferralRatio = 7 * (100000 - 90909);

    uint256 public totalRatio = rfvRatio + xLiberoRatio + xLiberoReferralRatio;

    constructor() Auth(msg.sender){}

    receive() external payable {}

    function manualDistribute() external {
        uint256 balance = IBEP20(busdToken).balanceOf(address(this));

        if(balance >= 1 * 10 ** 18){
            uint256 rfvAmount = balance.mul(rfvRatio).div(totalRatio);
            uint256 xLiberoAmount = balance.mul(xLiberoRatio).div(totalRatio);
            uint256 xLiberoReferralAmount = balance.sub(rfvAmount).sub(xLiberoAmount);

            if(rfvAmount > 0){
                IBEP20(busdToken).transfer(rfvTreasury, rfvAmount);
            }

            if(xLiberoAmount > 0){
                IBEP20(busdToken).transfer(address(busdDividendTracker), xLiberoAmount);
                busdDividendTracker.updateRewards();
            }

            if(xLiberoReferralAmount > 0){
                IBEP20(busdToken).transfer(referralTreasury, xLiberoReferralAmount);
            }
        }
    }

    function setRatio(uint256 _rfvRatio, uint256 _xLiberoRatio, uint256 _xLiberoReferralRatio) external onlyOwner {
        rfvRatio = _rfvRatio;
        xLiberoRatio = _xLiberoRatio;
        xLiberoReferralRatio = _xLiberoReferralRatio;
        totalRatio = _rfvRatio + _xLiberoRatio + _xLiberoReferralRatio;
    }

    function setTreasuryWallet(address _rfvTreasury, address _referralTreasury, address _busdTracker) external onlyOwner {
        rfvTreasury = _rfvTreasury;
        referralTreasury = _referralTreasury;
        busdDividendTracker = IDividendTracker(_busdTracker);
    }

    function retrieveTokens(address token, uint amount) external onlyOwner {
        uint balance = IBEP20(token).balanceOf(address(this));

        if(amount > balance){
            amount = balance;
        }

        require(IBEP20(token).transfer(msg.sender, amount), "Transfer failed!");
    }

    function retrieveBNB(uint amount) external onlyOwner {

        uint balance = address(this).balance;

        if(amount > balance){
            amount = balance;
        }

        (bool success,) = payable(msg.sender).call{ value: amount }("");
        require(success, "Failed");
    }

    function claimTokens(address token, uint amount, address receiver) external onlyOwner {

        uint balance = IBEP20(token).balanceOf(address(this));

        if(amount > balance){
            amount = balance;
        }

        require(IBEP20(token).transfer(receiver, amount), "Transfer failed!");
    }

    function claimBNB(uint amount, address receiver) external onlyOwner {

        uint balance = address(this).balance;

        if(amount > balance){
            amount = balance;
        }

        (bool success,) = payable(receiver).call{ value: amount }("");
        require(success, "Failed");
    }
}