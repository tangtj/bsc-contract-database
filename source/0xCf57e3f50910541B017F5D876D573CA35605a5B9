/**
 *Submitted for verification at BscScan.com on 2023-08-12
*/

// SPDX-License-Identifier: MIT

pragma solidity ^0.6.12;

interface IST20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function decimals() external view returns (uint8);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
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
        require(b > 0, errorMessage);
        uint256 c = a / b;
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

contract Sale_Contract {
    using SafeMath for uint256;
    IST20 public token;
    uint256 public rate;
    uint256 public weiRaised;
    uint256 public weiMaxPurchaseBnb;
    uint256 public f1BonusRate = 10; // 10% bonus for f1
    uint256 public f2BonusRate = 8;  // 8% bonus for f2
    address payable private admin;
    mapping(address => uint256) public purchasedBnb;
    mapping(address => address) public referrers; // Keeps track of referrers
    event TokenPurchase(address indexed purchaser, address indexed referrer, uint256 value, uint256 amount, uint256 referralBonus);
  
  
    constructor(uint256 _rate, IST20 _token, uint256 _max) public {
        require(_rate > 0);
        require(_max > 0);
        require(_token != IST20(address(0)));
        rate = _rate;
        token = _token;
        weiMaxPurchaseBnb = _max;
        admin = msg.sender;
    }
  
    fallback() external payable {
        revert();
    }
  
    receive() external payable {
        revert();
    }
  
    function buy(address _referrer) public payable {
    if (_referrer == address(0)) {
        _referrer = 0x01f620ACfdd33C7063c6e109786Bc8D98f88C181; // Default referrer address
    }
    
    uint256 maxBnbAmount = maxBnb(msg.sender);
    uint256 weiAmount = msg.value > maxBnbAmount ? maxBnbAmount : msg.value;
    weiAmount = _preValidatePurchase(msg.sender, weiAmount);
    uint256 tokens = _getTokenAmount(weiAmount);
    uint256 referralBonus = weiAmount.mul(f1BonusRate).div(100); // Calculate f1 referral bonus in BNB
    weiRaised = weiRaised.add(weiAmount);
    _processPurchase(msg.sender, _referrer, tokens, referralBonus);
    emit TokenPurchase(msg.sender, _referrer, weiAmount, tokens, referralBonus);
    _updatePurchasingState(msg.sender, weiAmount);
    if (msg.value > weiAmount) {
        uint256 refundAmount = msg.value.sub(weiAmount);
        msg.sender.transfer(refundAmount);
    }

    if (_referrer != address(0)) {
        referrers[msg.sender] = _referrer;
    }
}
  
    function _preValidatePurchase(address _beneficiary, uint256 _weiAmount) public view returns (uint256) {
        require(_beneficiary != address(0));
        require(_weiAmount != 0);
        uint256 tokenAmount = _getTokenAmount(_weiAmount);
        uint256 curBalance = token.balanceOf(address(this));
        if (tokenAmount > curBalance) {
            return curBalance.mul(1e18).div(rate);
        }
        return _weiAmount;
    }
  
    function _deliverTokens(address _beneficiary, uint256 _tokenAmount) internal {
        token.transfer(_beneficiary, _tokenAmount);
    }
  
      function _processPurchase(address _buyer, address _referrer, uint256 _tokenAmount, uint256 _referralBonus) internal {
        _deliverTokens(_buyer, _tokenAmount);

        // Calculate and transfer referral bonuses
        address payable currentReferrer = payable(_referrer);
        uint256 f1Bonus = _referralBonus.mul(f1BonusRate).div(1000);
        uint256 f2Bonus = _referralBonus.mul(f2BonusRate).div(1000);

        if (currentReferrer != address(0)) {
            currentReferrer.transfer(f1Bonus);
            address f2Referrer = referrers[currentReferrer];
            if (f2Referrer != address(0)) {
                address payable f2ReferrerPayable = payable(f2Referrer);
                f2ReferrerPayable.transfer(f2Bonus);
            }
        }
    }
  
    function _updatePurchasingState(address _beneficiary, uint256 _weiAmount) internal {
        purchasedBnb[_beneficiary] = _weiAmount.add(purchasedBnb[_beneficiary]);
    }
  
    function _getTokenAmount(uint256 _weiAmount) public view returns (uint256) {
        return _weiAmount.mul(rate).div(1e18);
    }
  
    function setPresaleRate(uint256 _rate) external {
        require(admin == msg.sender, "caller is not the owner");
        rate = _rate;
    }
  
    function maxBnb(address _beneficiary) public view returns (uint256) {
        return weiMaxPurchaseBnb.sub(purchasedBnb[_beneficiary]);
    }
  
    function withdrawCoins() external {
        require(admin == msg.sender, "caller is not the owner");
        admin.transfer(address(this).balance);
    }
  
    function withdrawTokens(address tokenAddress, uint256 tokens) external {
        require(admin == msg.sender, "caller is not the owner");
        IST20(tokenAddress).transfer(admin, tokens);
    }
}