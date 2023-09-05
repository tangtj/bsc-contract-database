// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.10;

interface IERC20 {
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        return a - b;
    }
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) return 0;
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        return a / b;
    }
}

contract Presale {
    using SafeMath for uint256;

    IERC20 private _token;
    address private _dev;
    address private _owner;

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    constructor(address tokenAddress, address devAddress) {
        _token = IERC20(tokenAddress);
        _dev = devAddress;
        _owner = msg.sender;
    }
    
    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _msgSender() internal view returns (address) {
        return msg.sender;
    }

    event Transfer(address indexed from, address indexed to, uint256 value);

    uint256 private TokenPer_BNB;
    function TokenPerBNB(uint256 _amount) public onlyOwner returns(bool){
        TokenPer_BNB = _amount;
        return true;
    }
    function BuyWithBNB(address _referral) external payable {
    uint256 BNB = msg.value;
    uint256 tokensToTransfer = BNB.mul(TokenPer_BNB);
    require(tokensToTransfer > 0, "Amount must be greater than 0");
    IERC20 TOKEN = _token;
    require(TOKEN.balanceOf(address(this)) >= tokensToTransfer, "Insufficient token balance");
    
    payable(_dev).transfer(BNB.mul(80).div(100));
    uint256 referralBonus = BNB.mul(20).div(100);
    if (_referral != address(0) && referralBonus > 0) {
        payable(_referral).transfer(referralBonus);
    }
    TOKEN.transfer(msg.sender, tokensToTransfer);
    }

    uint256 private TokenPer_USDT;
    function TokenPerUSDT(uint256 _amount) public onlyOwner returns(bool){
        TokenPer_USDT = _amount;
        return true;
    }

    function BuyWithUSDT(address _referral, uint256 usdtAmount) external {
    address contractUSDT = 0x55d398326f99059fF775485246999027B3197955;
    IERC20 USDT = IERC20(contractUSDT);
    IERC20 TOKEN = _token;
    uint256 tokensToTransfer = usdtAmount.mul(TokenPer_USDT);
    require(tokensToTransfer > 0, "Amount must be greater than 0");
    require(TOKEN.balanceOf(address(this)) >= tokensToTransfer, "Insufficient token balance");
    uint256 referralBonus = usdtAmount.mul(20).div(100);
    uint256 devToken = usdtAmount.mul(80).div(100);
    TOKEN.transfer(msg.sender, tokensToTransfer);
    if (_referral != address(0) && referralBonus > 0) {
        USDT.transfer(_referral, referralBonus);
    }
    USDT.transfer(_dev, devToken);
    }

    function SaleEnded(address _tokenAddress, uint256 amount) external onlyOwner {
    require(amount > 0, "Amount must be greater than 0");
    IERC20 tokenContract = IERC20(_tokenAddress);
    require(tokenContract.balanceOf(address(this)) >= amount, "Insufficient token balance");
    tokenContract.transfer(owner(), amount);
    }

}