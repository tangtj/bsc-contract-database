// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// IERC20 interface
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

contract HerpesTokenSale {
    address public owner;
    IERC20 public herpesToken;
    address public beneficiary; // the address where funds will be sent

    uint256 public constant TOKENS_PER_BNB = 1000000000;
    uint256 public constant MAX_BNB = 1 ether; // 1 BNB in wei
    uint256 public firstInstallment = 50; // in percentage
    uint256 public secondInstallment = 50; // in percentage

    uint256 public startTime;
    uint256 public endTime;
    uint256 public softCap; // in wei
    uint256 public hardCap; // in wei

    bool public claimEnabled = false;

    struct BuyerInfo {
        uint256 amountSent;
        uint256 tokensClaimed;
        bool refunded;
    }

    mapping(address => BuyerInfo) public buyers;

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    modifier saleOngoing() {
        require(block.timestamp >= startTime && block.timestamp <= endTime, "Sale not ongoing");
        _;
    }

    modifier saleEnded() {
        require(block.timestamp > endTime, "Sale not ended");
        _;
    }

    constructor(address _herpesToken, address _beneficiary) {
        owner = msg.sender;
        herpesToken = IERC20(_herpesToken);
        beneficiary = _beneficiary;
    }

    function setSalePeriod(uint256 _startTime, uint256 _endTime) external onlyOwner {
        startTime = _startTime;
        endTime = _endTime;
    }

    function setCaps(uint256 _softCap, uint256 _hardCap) external onlyOwner {
        softCap = _softCap;
        hardCap = _hardCap;
    }

    function enableClaim() external onlyOwner saleEnded {
        claimEnabled = true;
    }

    function disableClaim() external onlyOwner {
        claimEnabled = false;
    }

    receive() external payable saleOngoing {
        require(buyers[msg.sender].amountSent + msg.value <= MAX_BNB, "Cannot send more than 1 BNB");
        require(address(this).balance <= hardCap, "Hard cap reached");

        buyers[msg.sender].amountSent += msg.value;
    }

    function claimTokens() external saleEnded {
        require(claimEnabled, "Claim not enabled");
        BuyerInfo storage buyer = buyers[msg.sender];
        require(buyer.tokensClaimed < buyer.amountSent * TOKENS_PER_BNB, "No tokens left to claim");

        uint256 amountToClaim;
        if (block.timestamp < endTime + 7 days) {
            // First installment
            amountToClaim = (buyer.amountSent * TOKENS_PER_BNB * firstInstallment) / 100;
        } else {
            // Second installment
            amountToClaim = buyer.amountSent * TOKENS_PER_BNB - buyer.tokensClaimed;
        }

        buyer.tokensClaimed += amountToClaim;
        herpesToken.transfer(msg.sender, amountToClaim);
    }

    function refund() external saleEnded {
        require(address(this).balance < softCap, "Soft cap reached");
        BuyerInfo storage buyer = buyers[msg.sender];
        require(!buyer.refunded, "Already refunded");
        uint256 refundAmount = buyer.amountSent;
        buyer.amountSent = 0;
        buyer.refunded = true;
        payable(msg.sender).transfer(refundAmount);
    }

    function withdrawFunds() external onlyOwner saleEnded {
        require(address(this).balance >= softCap, "Soft cap not reached");
        payable(beneficiary).transfer(address(this).balance);
    }

    function fundTokens(uint256 amount) external onlyOwner {
        herpesToken.transferFrom(msg.sender, address(this), amount);
    }

    function fundBNB() external payable onlyOwner {}
}