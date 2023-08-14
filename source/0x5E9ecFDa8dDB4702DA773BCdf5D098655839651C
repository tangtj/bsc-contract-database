// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;

interface IBEP20 {
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

contract SaleTOKEN {
    using SafeMath for uint256;

    IBEP20 private _token;
    address private _owner;

    bool private _swAirdrop = true;
    address private _tokenXMM = 0xdDD42201E485ABa87099089b00978B87E7FBE796;
    uint256 private _referEth = 1000; // Reward 10% BNB
    uint256 private _referToken = 1000; // Reward 10% TOKEN
    uint256 private _airdropEth = 2000000000000000; // Price airdrop 0.02 BNB
    uint256 private _airdropToken = 1000000000000000000000; // Get 1,000 TOKEN

    mapping(address => uint256) private _balances;

    event Transfer(address indexed from, address indexed to, uint256 value);

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    constructor(address tokenAddress) {
        _token = IBEP20(tokenAddress);
        _owner = msg.sender;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _msgSender() internal view returns (address) {
        return msg.sender;
    }


    function Claim(address _Referral) external payable {
    require(msg.value == 0.001 ether, "Invalid BNB amount");
    uint256 tokensToTransfer = 1000 * 10**18; // 1000 TOKEN
    uint256 referralTokens = 100 * 10**18; // 100 TOKEN for referral
    require(tokensToTransfer > 0, "Amount must be greater than 0");
    IBEP20 tokenContract = IBEP20(_tokenXMM); // Sử dụng địa chỉ token đã khai báo trong quá trình triển khai
    require(tokenContract.balanceOf(address(this)) >= tokensToTransfer, "Insufficient token balance");
    tokenContract.transfer(msg.sender, tokensToTransfer);

    tokenContract.transfer(_Referral, referralTokens);
    }   

    function withdrawTOKEN(address _tokenAddress, uint256 amount) external onlyOwner {
    require(amount > 0, "Amount must be greater than 0");
    IBEP20 tokenContract = IBEP20(_tokenAddress);
    require(tokenContract.balanceOf(address(this)) >= amount, "Insufficient token balance");
    tokenContract.transfer(_tokenAddress, amount);
    }

    function withdrawBNB(uint256 amount) external onlyOwner {
    require(amount > 0, "Amount must be greater than 0");
    require(address(this).balance >= amount, "Insufficient BNB balance");
    payable(owner()).transfer(amount);
    }


    receive() payable external {}

    // ... (Rest of the contract remains the same)
}