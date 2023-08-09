// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract farmingXMM {
    address public owner;
    address public tokenAddress = 0xdDD42201E485ABa87099089b00978B87E7FBE796; // Replace with the actual token address

    mapping(address => uint256) public userDeposits;
    mapping(address => uint256) public userLastClaimTime;
    mapping(address => uint256) public userAccumulatedRewardsDOGE;
    mapping(address => uint256) public userAccumulatedRewardsSHIB;
    mapping(address => uint256) public userAccumulatedRewardsFLOKI;

    uint256 private secondsInDay = 24 * 60 * 60;

    event TokensDeposited(address indexed user, uint256 tokenAmount);
    event TokensWithdrawn(address indexed user, uint256 tokenAmount);

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function depositTokens(uint256 _amount) external {
        IERC20 token = IERC20(tokenAddress);
        require(token.transferFrom(msg.sender, address(this), _amount), "Deposit failed");

        uint256 accumulatedDOGE;
        uint256 accumulatedSHIB;
        uint256 accumulatedFLOKI;

        if (userDeposits[msg.sender] > 0) {
            (accumulatedDOGE, accumulatedSHIB, accumulatedFLOKI) = calculateAccumulatedRewards(msg.sender, block.timestamp);
            userAccumulatedRewardsDOGE[msg.sender] += accumulatedDOGE;
            userAccumulatedRewardsSHIB[msg.sender] += accumulatedSHIB;
            userAccumulatedRewardsFLOKI[msg.sender] += accumulatedFLOKI;
        }

        userDeposits[msg.sender] += _amount;
        userLastClaimTime[msg.sender] = block.timestamp;

        emit TokensDeposited(msg.sender, _amount);
    }

    function withdrawTokens(uint256 _amount) external {
        require(userDeposits[msg.sender] >= _amount, "Insufficient balance");
        IERC20 token = IERC20(tokenAddress);
        require(token.transfer(msg.sender, _amount), "Withdrawal failed");
        userDeposits[msg.sender] -= _amount;
        emit TokensWithdrawn(msg.sender, _amount);
    }

    function calculateAccumulatedRewards(address _user, uint256 _currentTime) internal view returns (uint256 dogeRewards, uint256 shibRewards, uint256 flokiRewards) {
        uint256 timeSinceLastClaim = _currentTime - userLastClaimTime[_user];

        uint256 dailyRewards = (userDeposits[_user] * 2) / 100;
        dogeRewards = (dailyRewards * timeSinceLastClaim * 69) / secondsInDay;
        shibRewards = (dailyRewards * timeSinceLastClaim * 500000) / secondsInDay;
        flokiRewards = (dailyRewards * timeSinceLastClaim * 200000) / secondsInDay;
    }

    function claimRewards(uint256 _tokenType) external {
    require(_tokenType >= 1 && _tokenType <= 3, "Invalid token type");

    uint256 currentTime = block.timestamp;
    uint256 accumulatedDOGE;
    uint256 accumulatedSHIB;
    uint256 accumulatedFLOKI;

    (accumulatedDOGE, accumulatedSHIB, accumulatedFLOKI) = calculateAccumulatedRewards(msg.sender, currentTime);

    if (_tokenType == 1) {
        userAccumulatedRewardsDOGE[msg.sender] += accumulatedDOGE;
    } else if (_tokenType == 2) {
        userAccumulatedRewardsSHIB[msg.sender] += accumulatedSHIB;
    } else if (_tokenType == 3) {
        userAccumulatedRewardsFLOKI[msg.sender] += accumulatedFLOKI;
    }

    userLastClaimTime[msg.sender] = currentTime;
}

    function getTotalDeposits() public view returns (uint256) {
        return IERC20(tokenAddress).balanceOf(address(this));
    }

    function getUserTotalDeposits(address _user) public view returns (uint256) {
        return userDeposits[_user];
    }
}

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}