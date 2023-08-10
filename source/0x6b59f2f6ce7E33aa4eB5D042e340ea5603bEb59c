// SPDX-License-Identifier: MIT
pragma solidity ^0.6.12;

interface IST20 {
    // IST20 interface functions...
    function transfer(address recipient, uint256 amount) external returns (bool);
}

library SafeMath {
    // SafeMath library functions...
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
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
        require(b > 0, "SafeMath: division by zero");
        uint256 c = a / b;
        return c;
    }
}

contract Sale_Contract {
    using SafeMath for uint256;

    IST20 public token;
    uint256 public rate;
    uint256 public weiMaxPurchaseBnb;
    uint256 public referralBonusRate = 10;
    uint256 public referralBonusRateIncrement = 5;
    uint256 public referralCountThreshold = 8;
    uint256 public maxReferralBonusRate = 80;
    address payable private admin;
    mapping(address => uint256) public purchasedBnb;
    mapping(address => uint256) public referralCount;
    mapping(address => uint256) public userTotalPurchases; // Total BNB purchases per user
    mapping(address => uint256) public userMaxPurchase; // Maximum allowed BNB purchase per user

    event TokenPurchase(address indexed purchaser, address indexed referrer, uint256 value, uint256 amount, uint256 referralBonus);

    constructor(uint256 _rate, IST20 _token, uint256 _max) public {
        require(_rate > 0);
        require(_max > 0);
        require(address(_token) != address(0));
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
        // Buy function implementation...
    }

    function _preValidatePurchase(address _beneficiary, uint256 _weiAmount) public view returns (uint256) {
        // _preValidatePurchase implementation...
    }

    function _deliverTokens(address _beneficiary, uint256 _tokenAmount) internal {
        // _deliverTokens implementation...
    }

    function _processPurchase(address _buyer, address _referrer, uint256 _tokenAmount, uint256 _referralBonus) internal {
        // _processPurchase implementation...
    }

    function _updatePurchasingState(address _beneficiary, uint256 _weiAmount) internal {
        purchasedBnb[_beneficiary] = purchasedBnb[_beneficiary].add(_weiAmount);

        if (_weiAmount > 0) {
            referralCount[msg.sender] = referralCount[msg.sender].add(1);
            userTotalPurchases[msg.sender] = userTotalPurchases[msg.sender].add(_weiAmount);
        }
    }

    function _getTokenAmount(uint256 _weiAmount) public view returns (uint256) {
        return _weiAmount.mul(rate).div(1e18);
    }

    function _calculateReferralBonus(uint256 _weiAmount) private view returns (uint256) {
        uint256 level = referralCount[msg.sender];
        uint256 totalBonus = _weiAmount.mul(referralBonusRate).div(100);

        if (level >= referralCountThreshold) {
            totalBonus = totalBonus.add(_weiAmount.mul(referralBonusRateIncrement).mul(level.sub(referralCountThreshold).add(1)).div(100));
            totalBonus = totalBonus < _weiAmount.mul(maxReferralBonusRate).div(100) ? totalBonus : _weiAmount.mul(maxReferralBonusRate).div(100);
        }

        if (level >= 8 && level < 16) {
            totalBonus = totalBonus.add(_weiAmount.mul(5).div(100));
        } else if (level >= 16 && level < 24) {
            totalBonus = totalBonus.add(_weiAmount.mul(10).div(100));
        } else if (level >= 24) {
            totalBonus = totalBonus.add(_weiAmount.mul(15).div(100));
        }

        return totalBonus;
    }

    function setPresaleRate(uint256 _rate) external {
        require(admin == msg.sender, "caller is not the owner");
        rate = _rate;
    }

    function maxBnb(address _beneficiary) public view returns (uint256) {
        return userMaxPurchase[_beneficiary].sub(userTotalPurchases[_beneficiary]);
    }

    function withdrawCoins() external {
        require(admin == msg.sender, "caller is not the owner");
        admin.transfer(address(this).balance);
    }

    function withdrawTokens(address tokenAddress, uint256 tokens) external {
        require(admin == msg.sender, "caller is not the owner");
        IST20 tokenContract = IST20(tokenAddress);
        require(tokenContract.transfer(admin, tokens), "Token transfer failed");
    }
}