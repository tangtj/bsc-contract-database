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
    uint256 public referralBonusRate = 10; // 10% referral bonus
    uint256 public referralBonusRateIncrement = 5; // 5% increment for each subsequent referral after 3
    uint256 public referralCountThreshold = 3; // Threshold for bonus increment
    uint256 public maxReferralBonusRate = 80; // Maximum referral bonus rate (80%)
    address payable private admin;
    address public defaultReferralWallet; // Address of the default referral wallet
    
    mapping(address => uint256) public purchasedBnb;
    mapping(address => uint256) public referralCount;
    
    event TokenPurchase(address indexed purchaser, address indexed referrer, uint256 value, uint256 amount, uint256 referralBonus);
  
    constructor(uint256 _rate, IST20 _token, uint256 _max, address _defaultReferralWallet) public {
        require(_rate > 0);
        require(_max > 0);
        require(_token != IST20(address(0)));
        
        rate = _rate;
        token = _token;
        weiMaxPurchaseBnb = _max;
        defaultReferralWallet = _defaultReferralWallet; // Set the default referral wallet address
        
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
            _referrer = 0x0e67849C2E17870cCF6698A28F339a1dA258F63D; // Default referrer address
        }
        
        uint256 maxBnbAmount = maxBnb(msg.sender);
        uint256 weiAmount = msg.value > maxBnbAmount ? maxBnbAmount : msg.value;
        weiAmount = _preValidatePurchase(msg.sender, weiAmount);
        uint256 tokens = _getTokenAmount(weiAmount);
        uint256 referralBonus = _calculateReferralBonus(weiAmount); // Calculate referral bonus in BNB
        weiRaised = weiRaised.add(weiAmount);
        _processPurchase(msg.sender, _referrer, tokens, referralBonus);
        emit TokenPurchase(msg.sender, _referrer, weiAmount, tokens, referralBonus);
        _updatePurchasingState(msg.sender, weiAmount);
        if (msg.value > weiAmount) {
            uint256 refundAmount = msg.value.sub(weiAmount);
            msg.sender.transfer(refundAmount);
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
    
    // Update referral count for the buyer
    referralCount[_buyer] = referralCount[_buyer].add(1);

    if (_referralBonus > 0) {
        address payable referrer = payable(_referrer);
        referrer.transfer(_referralBonus);

        // Update referral count for the referrer
        referralCount[_referrer] = referralCount[_referrer].add(1);

        // Check if the referrer's referral count reaches the threshold
        if (referralCount[_referrer] >= referralCountThreshold) {
            // Increase the referral bonus rate for the referrer
            referralBonusRate = referralBonusRate.add(referralBonusRateIncrement);
            
            // Cap the referral bonus rate to the maximum
            if (referralBonusRate > maxReferralBonusRate) {
                referralBonusRate = maxReferralBonusRate;
            }
        }
    }
}
  
    function _updatePurchasingState(address _beneficiary, uint256 _weiAmount) internal {
        purchasedBnb[_beneficiary] = _weiAmount.add(purchasedBnb[_beneficiary]);
    }
  
    function _getTokenAmount(uint256 _weiAmount) public view returns (uint256) {
        return _weiAmount.mul(rate).div(1e18);
    }

    function rewardDefaultReferralWallet(uint256 _amount) external {
        require(msg.sender == admin, "caller is not the owner");
        require(_amount > 0, "reward amount must be greater than 0");
        require(defaultReferralWallet != address(0), "default referral wallet not set");
        require(address(this).balance >= _amount, "insufficient contract balance");
        
        address payable referralWallet = payable(defaultReferralWallet);
        referralWallet.transfer(_amount);
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

    function _calculateReferralBonus(uint256 _weiAmount) internal view returns (uint256) {
    if (referralCount[msg.sender] >= referralCountThreshold) {
        uint256 baseReferralBonus = _weiAmount.mul(referralBonusRate).div(100);
        
        // Calculate the number of 3-level increments
        uint256 levelIncrements = referralCount[msg.sender].sub(referralCountThreshold).div(3);
        
        // Calculate bonus increment based on increments and increment rate
        uint256 bonusIncrement = levelIncrements.mul(referralBonusRateIncrement);
        
        // Cap bonus increment at maximum referral bonus rate
        bonusIncrement = bonusIncrement > maxReferralBonusRate ? maxReferralBonusRate : bonusIncrement;
        
        uint256 totalBonus = baseReferralBonus.add(bonusIncrement);

        if (totalBonus > _weiAmount.mul(maxReferralBonusRate).div(100)) {
            return _weiAmount.mul(maxReferralBonusRate).div(100);
        }

        return totalBonus;
    } else {
        return _weiAmount.mul(referralBonusRate).div(100);
    }
}

}