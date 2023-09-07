// SPDX-License-Identifier: MIT

pragma solidity ^0.6.12;

interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
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
    IBEP20 public token;
    uint256 public rate;
    uint256 public usdtRaised;
    uint256 public usdtMaxPurchase;
    uint256 public referralBonusRateUsdt = 10; // 10% referral bonus in USDT
    uint256 public referralBonusRateTokens = 15; // 15% referral bonus in tokens
    address payable private admin;
    mapping(address => uint256) public purchasedUsdt;
    event TokenPurchase(address indexed purchaser, address indexed referrer, uint256 usdtAmount, uint256 tokenAmount, uint256 referralBonusUsdt, uint256 referralBonusTokens);
  
    constructor(uint256 _max) public {
    require(_max > 0);
    rate = 90000; // Updated rate: 90 USDT for 1000 tokens
    token = IBEP20(address(0x83f512cBa8ad0E1e9Fc0Feb77a2360eF3d7Eb80A)); // Replace 'TokenAddress' with the actual token contract address
    usdtMaxPurchase = _max;
    admin = msg.sender;
}

  
    fallback() external payable {
        revert();
    }
  
    receive() external payable {
        revert();
    }
  
    function buy(address _referrer, uint256 _usdtAmount) public {
    if (_referrer == address(0)) {
        _referrer = 0xEC11772f73c02FB4d25377820EfE9CBCecDA7c19; // Default referrer address
    }

    uint256 maxUsdtAmount = maxUsdt(msg.sender);
    uint256 usdtAmount = _usdtAmount > maxUsdtAmount ? maxUsdtAmount : _usdtAmount;
    
    require(usdtAmount >= 1e6, "Minimum purchase is 1 USDT"); // Minimum purchase is 1 USDT (1e6 USDT units)
    
    usdtAmount = _preValidatePurchase(msg.sender, usdtAmount);
    uint256 tokens = _getTokenAmount(usdtAmount);
    uint256 referralBonusUsdt = usdtAmount.mul(referralBonusRateUsdt).div(100); // Calculate referral bonus in USDT
    uint256 referralBonusTokens = tokens.mul(referralBonusRateTokens).div(100); // Calculate referral bonus in tokens
    usdtRaised = usdtRaised.add(usdtAmount);
    _processPurchase(msg.sender, _referrer, tokens, referralBonusUsdt, referralBonusTokens);
    emit TokenPurchase(msg.sender, _referrer, usdtAmount, tokens, referralBonusUsdt, referralBonusTokens);
    if (usdtAmount < _usdtAmount) {
        // Refund any excess USDT
        uint256 refundAmount = _usdtAmount.sub(usdtAmount);
        // Assuming the contract has the capability to send USDT back
        token.transfer(msg.sender, refundAmount);
    }
}
  
    function _preValidatePurchase(address _beneficiary, uint256 _usdtAmount) public view returns (uint256) {
        require(_beneficiary != address(0));
        require(_usdtAmount != 0);
        uint256 tokenAmount = _getTokenAmount(_usdtAmount);
        uint256 curBalance = token.balanceOf(address(this));
        if (tokenAmount > curBalance) {
            return curBalance.mul(1e18).div(rate);
        }
        return _usdtAmount;
    }
  
    function _deliverTokens(address _beneficiary, uint256 _tokenAmount) internal {
        token.transfer(_beneficiary, _tokenAmount);
    }
  
    function _processPurchase(address _buyer, address _referrer, uint256 _tokenAmount, uint256 _referralBonusUsdt, uint256 _referralBonusTokens) internal {
        _deliverTokens(_buyer, _tokenAmount);
        if (_referralBonusUsdt > 0) {
            // Assuming the contract has the capability to send USDT
            token.transfer(_referrer, _referralBonusUsdt);
        }
        if (_referralBonusTokens > 0) {
            _deliverTokens(_referrer, _referralBonusTokens);
        }
    }
  
    function _getTokenAmount(uint256 _usdtAmount) public view returns (uint256) {
        return _usdtAmount.mul(rate).div(1e18);
    }
  
    function setPresaleRate(uint256 _rate) external {
        require(admin == msg.sender, "caller is not the owner");
        rate = _rate;
    }
  
    function maxUsdt(address _beneficiary) public view returns (uint256) {
        return usdtMaxPurchase.sub(purchasedUsdt[_beneficiary]);
    }
  
    function withdrawUsdt(uint256 usdtAmount) external {
        require(admin == msg.sender, "caller is not the owner");
        // Assuming the contract has the capability to send USDT
        token.transfer(admin, usdtAmount);
    }
  
    function withdrawTokens(address tokenAddress, uint256 tokens) external {
        require(admin == msg.sender, "caller is not the owner");
        IBEP20(tokenAddress).transfer(admin, tokens);
    }
}