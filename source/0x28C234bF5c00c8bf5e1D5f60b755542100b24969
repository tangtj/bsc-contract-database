// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract Farm {
    address public owner;
    address public tokenAddress = 0xdDD42201E485ABa87099089b00978B87E7FBE796;
    
    address public DOGEAddress = 0xbA2aE424d960c26247Dd6c32edC70B295c744C43;
    address public SHIBAddress = 0x2859e4544C4bB03966803b044A93563Bd2D0DD4D;
    address public FLOKIAddress = 0xfb5B838b6cfEEdC2873aB27866079AC55363D37E;

    enum FarmType { DOGE, SHIB, FLOKI }
    mapping(address => FarmType) public userFarmChoice;
    mapping(address => uint256) public userDeposits;
    mapping(address => uint256) public userDepositTimes;

    event TokensDeposited(address indexed user, uint256 tokenAmount, FarmType farm);
    event TokensWithdrawn(address indexed user, uint256 tokenAmount, FarmType farm);
    event EarningsClaimed(address indexed user, uint256 earningAmount, FarmType farm);

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
        userDeposits[msg.sender] += _amount;
        emit TokensDeposited(msg.sender, _amount, userFarmChoice[msg.sender]);
    }

    function withdrawTokens(uint256 _amount) external {
        require(userDeposits[msg.sender] >= _amount, "Insufficient balance");
        IERC20 token = IERC20(tokenAddress);
        require(token.transfer(msg.sender, _amount), "Withdrawal failed");
        userDeposits[msg.sender] -= _amount;
        emit TokensWithdrawn(msg.sender, _amount, userFarmChoice[msg.sender]);
    }

    function getTotalDeposits() public view returns (uint256) {
        return IERC20(tokenAddress).balanceOf(address(this));
    }

    function getUserTotalDeposits(address _user) public view returns (uint256) {
        return userDeposits[_user];
    }

    function chooseFarm(FarmType _farmType) external {
        require(_farmType == FarmType.DOGE || _farmType == FarmType.SHIB || _farmType == FarmType.FLOKI, "Invalid farm choice");
        userFarmChoice[msg.sender] = _farmType;
    }

    function updateDepositTime() external {
        userDepositTimes[msg.sender] = block.timestamp;
    }

    function claimEarnings() external {
        FarmType farmChoice = userFarmChoice[msg.sender];
        require(farmChoice != FarmType(0), "Choose a farm first");
        uint256 elapsedTime = block.timestamp - userDepositTimes[msg.sender];
        uint256 earningAmount;

        if (farmChoice == FarmType.DOGE) {
            earningAmount = (userDeposits[msg.sender] * elapsedTime * 69) / 1 days;
        } else if (farmChoice == FarmType.SHIB) {
            earningAmount = (userDeposits[msg.sender] * elapsedTime * 500000) / 1 days;
        } else if (farmChoice == FarmType.FLOKI) {
            earningAmount = (userDeposits[msg.sender] * elapsedTime * 200000) / 1 days;
        }

        IERC20 token = IERC20(tokenAddress);
        require(token.transfer(msg.sender, earningAmount), "Earning claim failed");
        emit EarningsClaimed(msg.sender, earningAmount, farmChoice);
        userDepositTimes[msg.sender] = block.timestamp; // Reset deposit time
    }
}