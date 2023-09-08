// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
}

contract HerpesTokenSale {
    address public owner;
    IERC20 public herpesToken;
    address public beneficiary;

    uint256 public constant TOKENS_PER_BNB = 1000000000;
    uint256 public constant MAX_BNB = 2 ether; 
    uint256 public constant MIN_BNB = 0.01 ether; // Minimum BNB purchase

    uint256 public firstInstallment = 50; 
    uint256 public secondInstallment = 50; 

    uint256 public startTime;
    uint256 public endTime;
    uint256 public softCap;
    uint256 public hardCap;

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
        require(msg.value >= MIN_BNB, "Sent BNB below the minimum threshold");
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
            amountToClaim = (buyer.amountSent * TOKENS_PER_BNB * firstInstallment) / 100;
        } else {
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

    function removeForeignTokens(address _token, address _recipient) external onlyOwner {
        require(_token != address(herpesToken), "Cannot remove HERPES tokens using this function");
        IERC20(_token).transfer(_recipient, IERC20(_token).balanceOf(address(this)));
    }

    function returnUnusedHerpesTokens(address _recipient, uint256 _amount) external onlyOwner {
        require(_amount <= herpesToken.balanceOf(address(this)), "Not enough HERPES tokens in contract");
        herpesToken.transfer(_recipient, _amount);
    }

     function manualSendTokens(address _buyer, uint256 _amount) external onlyOwner {
        require(_buyer != address(0), "Invalid address");
        require(_amount > 0, "Amount should be greater than 0");

        BuyerInfo storage buyer = buyers[_buyer];
        require(buyer.tokensClaimed + _amount <= buyer.amountSent * TOKENS_PER_BNB, "Exceeds allowable claim");

        buyer.tokensClaimed += _amount;
        herpesToken.transfer(_buyer, _amount);
    }
}