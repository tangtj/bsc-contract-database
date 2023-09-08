// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// IERC20 Interface
interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

// Ownable Contract
contract Ownable {
    address public owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        owner = msg.sender;
        emit OwnershipTransferred(address(0), owner);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function");
        _;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Invalid new owner address");
        emit OwnershipTransferred(owner, newOwner);
        owner = newOwner;
    }
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
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        return a / b;
    }
}

contract Presale is Ownable {
    using SafeMath for uint256;

    IERC20 public token;
    address public beneficiary;
    uint256 public rate;
    uint256 public minPurchase;
    uint256 public maxPurchase;
    uint256 public startTime;
    uint256 public endTime;
    bool public isPresaleActive = false;

    event TokensPurchased(address indexed buyer, uint256 amount, uint256 cost);

    constructor(
        address _token,
        address _beneficiary
    ) {
        token = IERC20(_token);
        beneficiary = _beneficiary;
        rate = 323475862; // 150 SPAI for 1 BNB
        minPurchase = 0.00463 ether; // Minimum purchase is set to 0.1 BNB
        maxPurchase = 50 ether; // Maximum purchase is set to 50 BNB
        startTime = 1694070740; // September 4, 2023, 12:00 PM (noon) UTC
        endTime = 1696615320;
    }

    modifier onlyDuringPresale() {
        require(isPresaleActive, "Presale is not active");
        require(block.timestamp >= startTime && block.timestamp <= endTime, "Presale is not open");
        _;
    }

    function startPresale() external onlyOwner {
        isPresaleActive = true;
    }

    function endPresale() external onlyOwner {
        isPresaleActive = false;
    }

    function purchaseTokens() external payable onlyDuringPresale {
        require(msg.value >= minPurchase && msg.value <= maxPurchase, "Invalid purchase amount");

        // Calculate the token amount based on the received BNB
        uint256 tokenAmount = msg.value.mul(rate);
        require(tokenAmount <= token.balanceOf(address(this)), "Not enough tokens in the contract");

        // Transfer tokens to the buyer
        token.transfer(msg.sender, tokenAmount);

        // Transfer BNB to the beneficiary
        payable(beneficiary).transfer(msg.value);

        emit TokensPurchased(msg.sender, tokenAmount, msg.value);
    }

    function withdrawTokens() external onlyOwner {
        token.transfer(owner, token.balanceOf(address(this)));
    }

    function withdrawBNB() external onlyOwner {
        payable(owner).transfer(address(this).balance);
    }
}