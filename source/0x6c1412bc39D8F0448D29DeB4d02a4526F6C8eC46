// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

 interface IERC20 {
        function transfer(address recipient, uint256 amount) external returns (bool);
        function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    }

contract GTHERPES_ICO {

    address public owner;
    address public tokenAddress;
    uint256 public rate = 1000000000; // HERPES tokens per BNB
    uint256 public firstInstallment = rate / 2;
    uint256 public secondInstallment = rate / 2;
    uint256 public startTime;
    uint256 public endTime;
    bool public claimEnabled = false;
    uint256 public softCap;
    uint256 public hardCap;
    uint256 public totalReceived;
    uint256 public minimumBNB;
    mapping(address => uint256) public contributions;
    mapping(address => bool) public firstClaimDone;
    
    constructor(address _tokenAddress) {
        owner = msg.sender;
        tokenAddress = _tokenAddress;
    }
    
    modifier onlyOwner() {
        require(msg.sender == owner, "Not the owner");
        _;
    }
    
    modifier saleActive() {
        require(block.timestamp >= startTime && block.timestamp <= endTime, "Sale is not active");
        _;
    }
    
    function setSalePeriod(uint256 _startTime, uint256 _endTime) external onlyOwner {
        startTime = _startTime;
        endTime = _endTime;
    }
    
    function enableClaim(bool _status) external onlyOwner {
        claimEnabled = _status;
    }
    
    function setSoftCap(uint256 _softCap) external onlyOwner {
        softCap = _softCap;
    }
    
    function setHardCap(uint256 _hardCap) external onlyOwner {
        hardCap = _hardCap;
    }
    
    function setMinimumBNB(uint256 _minimumBNB) external onlyOwner {
        minimumBNB = _minimumBNB;
    }

    function fundWithHERPES(uint256 amount) external onlyOwner {
        IERC20(tokenAddress).transferFrom(msg.sender, address(this), amount);
    }

    function fundWithBNB() external payable onlyOwner {}

    function buyTokens() external payable saleActive {
        require(msg.value >= minimumBNB, "Sent BNB is below the minimum required amount");
        require(contributions[msg.sender] + msg.value <= 2 ether, "Can't contribute more than 2 BNB");
        require(totalReceived + msg.value <= hardCap, "Hard cap reached");
        
        contributions[msg.sender] += msg.value;
        totalReceived += msg.value;
    }
    
    function claim() external {
        require(claimEnabled, "Claiming is not enabled");
        uint256 amount = contributions[msg.sender] * firstInstallment;
        if (firstClaimDone[msg.sender]) {
            require(block.timestamp >= endTime + 7 days, "Second claim not yet available");
            amount = contributions[msg.sender] * secondInstallment;
        } else {
            firstClaimDone[msg.sender] = true;
        }
        
        IERC20(tokenAddress).transfer(msg.sender, amount);
    }

    function manualTokenSend(address recipient, uint256 amount) external onlyOwner {
        IERC20(tokenAddress).transfer(recipient, amount);
    }

    function refund() external {
        require(block.timestamp > endTime && totalReceived < softCap, "Refund is not available");
        uint256 amount = contributions[msg.sender];
        contributions[msg.sender] = 0;
        payable(msg.sender).transfer(amount);
    }
    
    function withdrawBNB(address payable _to) external onlyOwner {
        _to.transfer(address(this).balance);
    }
    
    function withdrawHERPES(uint256 amount) external onlyOwner {
        IERC20(tokenAddress).transfer(msg.sender, amount);
    }
    
    function withdrawOtherTokens(address _token, uint256 amount) external onlyOwner {
        IERC20(_token).transfer(msg.sender, amount);
    }
    
   
}