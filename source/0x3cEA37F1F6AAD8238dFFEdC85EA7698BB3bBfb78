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

interface IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);  // Ajoutez cette ligne
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

contract bts_Presale {
    using SafeMath for uint256;
    IST20 public token;
    IERC20 public usdt;
    IERC20 public busd;
    uint256 public rate;
    uint256 public weiRaised;
    uint256 public weiMaxPurchaseBnb;
    address payable private admin;
    mapping(address => uint256) public purchasedBnb;
  
    event TokenPurchase(address indexed purchaser, address indexed beneficiary, uint256 value, uint256 amount);
  

    constructor(uint256 _rate, IST20 _token, uint256 _max, address _usdtAddress, address _busdAddress) public {
        require(_rate > 0);
        require(_max > 0);
        require(_token != IST20(address(0)));
        rate = _rate;
        token = _token;
        weiMaxPurchaseBnb = _max;
        usdt = IERC20(_usdtAddress);
        busd = IERC20(_busdAddress);
        admin = 0x808bC87248eD2C43273d496328d87955c718EF92;
    }



    fallback () external payable {
        revert();    
    }

    receive () external payable {
        revert();
    }

    function buyTokens(address _beneficiary) public payable {
        uint256 weiAmount = msg.value;
        weiAmount = _preValidatePurchase(_beneficiary, weiAmount);
        uint256 tokens = _getTokenAmount(weiAmount);
        weiRaised = weiRaised.add(weiAmount);
        _processPurchase(_beneficiary, tokens);
        emit TokenPurchase(msg.sender, _beneficiary, weiAmount, tokens);
        _updatePurchasingState(_beneficiary, weiAmount);
    }
  
    function buyTokensWithUsdt(uint256 _usdtAmount) public {
        require(usdt.transferFrom(msg.sender, address(this), _usdtAmount), "Transfer failed");
        uint256 tokens = _getTokenAmount(_usdtAmount);
        _processPurchase(msg.sender, tokens);
        emit TokenPurchase(msg.sender, msg.sender, _usdtAmount, tokens);
    }

    function buyTokensWithBusd(uint256 _busdAmount) public {
        require(busd.transferFrom(msg.sender, address(this), _busdAmount), "Transfer failed");
        uint256 tokens = _getTokenAmount(_busdAmount);
        _processPurchase(msg.sender, tokens);
        emit TokenPurchase(msg.sender, msg.sender, _busdAmount, tokens);
    }



    function _preValidatePurchase(address _beneficiary, uint256 _weiAmount) internal view returns (uint256) {
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

    function _getTokenAmount(uint256 _weiAmount) internal view returns (uint256) {
        return _weiAmount.mul(rate).div(1e18);
    }

    function setPresaleRate(uint256 _rate) external {
        require(admin == msg.sender, "caller is not the owner");
        rate = _rate;
    }

    function withdrawCoins() external {
        require(admin == msg.sender, "caller is not the owner");
        admin.transfer(address(this).balance);
    }

    function withdrawTokens(address tokenAddress, uint256 tokens) external {
        require(admin == msg.sender, "caller is not the owner");
        IST20(tokenAddress).transfer(admin, tokens);
    }

    function withdrawUsdtTo(uint256 amount, address to) external {
        require(admin == msg.sender, "caller is not the owner");
        require(to != address(0), "Cannot transfer to the zero address");
        require(usdt.transfer(to, amount), "Transfer failed");
    }

    function withdrawBusdTo(uint256 amount, address to) external {
        require(admin == msg.sender, "caller is not the owner");
        require(to != address(0), "Cannot transfer to the zero address");
        require(busd.transfer(to, amount), "Transfer failed");
    }

}