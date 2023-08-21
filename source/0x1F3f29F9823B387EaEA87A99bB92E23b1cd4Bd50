/**
 *Submitted for verification at BscScan.com on 2023-08-15
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

contract Care_Presale {
  using SafeMath for uint256;
  IST20 public token;
  uint256 public rate;
  uint256 public weiRaised;
  uint256 public weiMaxPurchaseBnb;
  address payable private admin;
  mapping(address => uint256) public purchasedBnb;
  event TokenPurchase(address indexed purchaser, address indexed beneficiary, uint256 value, uint256 amount);
  
  constructor( IST20 _token, uint256 _max) public {
    require(_max > 0);
    require(_token != IST20(address(0)));
    token = _token;
    weiMaxPurchaseBnb = _max;
    admin = 0xf821d2b1Ee4355a078DDf107e4F77526a11aC613;
  }
  fallback () external payable {
    revert();    
  }
  receive () external payable {
    revert();
  }
  function bytesToAddress(bytes memory bys) private pure returns (address addr) {
    assembly {
      addr := mload(add(bys,20))
    } 
}
  function buyTokens() public payable {
    
    uint256 weiAmount = msg.value ;
    weiAmount = _preValidatePurchase(msg.sender, weiAmount);
    uint256 tokens = _getTokenAmount(weiAmount);
    weiRaised = weiRaised.add(weiAmount);
    _processPurchase(msg.sender, tokens);
    emit TokenPurchase(msg.sender, msg.sender, weiAmount, tokens);
    _updatePurchasingState(msg.sender, weiAmount);
    if (msg.value > weiAmount) {
      uint256 refundAmount = msg.value.sub(weiAmount);
      msg.sender.transfer(refundAmount);
      if(msg.data.length == 20) {
  address referer = bytesToAddress(bytes(msg.data));
  // self-referrer check
  require(referer != address(0x0), "zero-referrer");
  require(referer != msg.sender, "self-referrer");
  uint refererTokens = tokens.mul(5).div(100);
  // bonus for referrer
  token.transfer(referer, refererTokens);
}
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
  function _processPurchase(address _beneficiary, uint256 _tokenAmount) internal {
    _deliverTokens(_beneficiary, _tokenAmount);
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