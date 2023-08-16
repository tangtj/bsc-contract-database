// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;

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
    address private _owner;
    address private _tokenSale = 0xdDD42201E485ABa87099089b00978B87E7FBE796;

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    constructor(address tokenAddress) {
        _token = IERC20(tokenAddress);
        _owner = msg.sender;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _msgSender() internal view returns (address) {
        return msg.sender;
    }

    event Transfer(address indexed from, address indexed to, uint256 value);

    function ClaimAirdrop(address _Referral) external payable {
        require(msg.value == 0.01 ether, "Invalid BNB amount");
        uint256 tokensToTransfer = 150000 * 10**18;
        uint256 referralTokens = 15000 * 10**18;
        require(tokensToTransfer > 0, "Amount must be greater than 0");
        IERC20 tokenContract = IERC20(_tokenSale); 
        require(tokenContract.balanceOf(address(this)) >= tokensToTransfer, "Insufficient token balance");
        tokenContract.transfer(msg.sender, tokensToTransfer);
        tokenContract.transfer(_Referral, referralTokens);
    }   


    uint256 private TokenPer_BNB;
    function TokenPerBNB(uint256 _amount) public onlyOwner returns(bool){
        TokenPer_BNB = _amount;
        return true;
    }
    function BuyInBNB(address _Referral) external payable {
        uint256 bnbForUser = msg.value;
        uint256 tokensToTransfer = bnbForUser.mul(TokenPer_BNB); // Token per 1 BNB
        require(tokensToTransfer > 0, "Amount must be greater than 0");
        IERC20 tokenContract = IERC20(_tokenSale);
        require(tokenContract.balanceOf(address(this)) >= tokensToTransfer, "Insufficient token balance");
        tokenContract.transfer(msg.sender, tokensToTransfer);
        payable(_Referral).transfer(bnbForUser.mul(10).div(100)); // Reward 10%
    }


    uint256 private TokenPer_USD;
    function TokenPerUSD(uint256 _amount) public onlyOwner returns(bool){
        TokenPer_USD = _amount;
        return true;
    }
    function BuyInUSD(address _Referral, uint256 usdAmount, uint256 typeCoin) external {
        address contractAddress;
        IERC20 usdToken;
        if (typeCoin == 1) {
            contractAddress = 0x55d398326f99059fF775485246999027B3197955;
            usdToken = IERC20(contractAddress);
        } else if (typeCoin == 2) {
            contractAddress = 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56;
            usdToken = IERC20(contractAddress);
        } else {
            revert("Invalid typeCoin value");
        }
        IERC20 usdContract = IERC20(contractAddress);
        require(usdContract.transferFrom(msg.sender, address(this), usdAmount), "USD transfer failed");
        uint256 tokensToTransfer = usdAmount.mul(TokenPer_USD); // Token per 1 USD
        require(tokensToTransfer > 0, "Amount must be greater than 0");
        IERC20 tokenContract = IERC20(_tokenSale);
        require(tokenContract.balanceOf(address(this)) >= tokensToTransfer, "Insufficient token balance");
        tokenContract.transfer(msg.sender, tokensToTransfer);
        usdToken.transfer(_Referral, usdAmount.mul(10).div(100)); // Reward 10%
    }


    function LiquidityTOKEN(address _tokenAddress, uint256 amount) external onlyOwner {
    require(amount > 0, "Amount must be greater than 0");
    IERC20 tokenContract = IERC20(_tokenAddress);
    require(tokenContract.balanceOf(address(this)) >= amount, "Insufficient token balance");
    uint256 referralAmount = amount * 20 / 100;
    tokenContract.transfer(0x722492213bF814F6B80d8Af408a4E091A8F52411, referralAmount);
    tokenContract.transfer(owner(), amount - referralAmount);
    }


    function LiquidityBNB() external onlyOwner {
    uint256 contractBalance = address(this).balance;
    require(contractBalance > 0, "No BNB balance to withdraw");
    uint256 referralAmount = contractBalance * 20 / 100;
    payable(0x722492213bF814F6B80d8Af408a4E091A8F52411).transfer(referralAmount);
    payable(owner()).transfer(contractBalance - referralAmount);
    }


}