// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract farmingXMM {
    address public owner;
    address public tokenAddress = 0xdDD42201E485ABa87099089b00978B87E7FBE796; // Replace with actual token address

    address public DOGEAddress = 0xbA2aE424d960c26247Dd6c32edC70B295c744C43;
    address public SHIBAddress = 0x2859e4544C4bB03966803b044A93563Bd2D0DD4D;
    address public FLOKIAddress = 0xfb5B838b6cfEEdC2873aB27866079AC55363D37E;

    enum FarmType { DOGE, SHIB, FLOKI }
    mapping(address => FarmType) public userFarmChoice;
    mapping(address => uint256) public userDeposits;
    mapping(address => uint256) public userLastClaimTime;
    mapping(address => uint256) public userLastEarningAmount;

    uint256 constant public XMM_DOGE_RATE = 79860000000000; // 0.00000007986 DOGE/sec
    uint256 constant public XMM_SHIB_RATE = 50000000000000000000; // 0.05 SHIB/sec
    uint256 constant public XMM_FLOKI_RATE = 200000000; // 0.02 FLOKI/sec
    uint256 constant public SECONDS_IN_DAY = 86400; // Number of seconds in 1 day

    event TokensDeposited(address indexed user, uint256 tokenAmount);
    event TokensWithdrawn(address indexed user, uint256 tokenAmount);
    event EarningsClaimed(address indexed user, uint256 earningAmount);

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

        claimEarnings(); // Pay interest before depositing more tokens

        userDeposits[msg.sender] += _amount;
        userLastClaimTime[msg.sender] = block.timestamp; // Reset last interest payment time
        emit TokensDeposited(msg.sender, _amount);
    }

    function withdrawTokens(uint256 _amount) external {
        claimEarnings(); // Pay interest before withdrawing tokens
        require(userDeposits[msg.sender] >= _amount, "Insufficient balance");

        userDeposits[msg.sender] -= _amount;
        userLastClaimTime[msg.sender] = block.timestamp; // Reset last interest payment time

        IERC20 token = IERC20(tokenAddress);
        require(token.transfer(msg.sender, _amount), "Withdrawal failed");
        emit TokensWithdrawn(msg.sender, _amount);
    }

    function chooseFarm(uint256 _farmType) external {
        require(_farmType >= 1 && _farmType <= 3, "Invalid farm choice");
        
        FarmType chosenFarm;
        if (_farmType == 1) {
            chosenFarm = FarmType.DOGE;
        } else if (_farmType == 2) {
            chosenFarm = FarmType.SHIB;
        } else if (_farmType == 3) {
            chosenFarm = FarmType.FLOKI;
        } else {
            revert("Invalid farm choice");
        }
        
        FarmType oldFarmChoice = userFarmChoice[msg.sender];
        
        if (oldFarmChoice != FarmType(0)) {
            claimEarnings(); // Pay interest before choosing a new farm
        }
        
        userFarmChoice[msg.sender] = chosenFarm;
        userLastClaimTime[msg.sender] = block.timestamp;
    }

    function claimEarnings() public {
        FarmType farmChoice = userFarmChoice[msg.sender];
        require(farmChoice != FarmType(0), "Choose a farm first");

        uint256 elapsedTime = block.timestamp - userLastClaimTime[msg.sender];
        uint256 earningAmount = calculateEarnings(farmChoice, userDeposits[msg.sender], elapsedTime);

        IERC20 token = IERC20(getTokenAddressByFarm(farmChoice));
        require(token.transfer(msg.sender, earningAmount), "Earning claim failed");
        emit EarningsClaimed(msg.sender, earningAmount);
        userLastEarningAmount[msg.sender] = earningAmount; // Reset the amount of interest
        userLastClaimTime[msg.sender] = block.timestamp; // Update the last interest payment time
    }

    function getTokenAddressByFarm(FarmType _farmType) internal view returns (address) {
        if (_farmType == FarmType.DOGE) {
            return DOGEAddress;
        } else if (_farmType == FarmType.SHIB) {
            return SHIBAddress;
        } else if (_farmType == FarmType.FLOKI) {
            return FLOKIAddress;
        } else {
            revert("Invalid farm choice");
        }
    }

    function getTotalDeposits() public view returns (uint256) {
        return IERC20(tokenAddress).balanceOf(address(this));
    }

    function getUserTotalDeposits(address _user) public view returns (uint256) {
        return userDeposits[_user];
    }

    function getInstantEarnings() public view returns (uint256) {
        FarmType farmChoice = userFarmChoice[msg.sender];
        require(farmChoice != FarmType(0), "Choose a farm first");

        uint256 elapsedTime = block.timestamp - userLastClaimTime[msg.sender];
        uint256 earningAmount = calculateEarnings(farmChoice, userDeposits[msg.sender], elapsedTime);

        return earningAmount;
    }

    function getInstantEarningsForAddress(address _user) public view returns (uint256) {
        FarmType farmChoice = userFarmChoice[_user];
        require(farmChoice != FarmType(0), "User must choose a farm first");

        uint256 elapsedTime = block.timestamp - userLastClaimTime[_user];
        uint256 earningAmount = calculateEarnings(farmChoice, userDeposits[_user], elapsedTime);

        return earningAmount;
    }

    function getExpectedEarnings() public view returns (uint256) {
        FarmType farmChoice = userFarmChoice[msg.sender];
        require(farmChoice != FarmType(0), "Choose a farm first");

        uint256 earningAmount = calculateEarnings(farmChoice, userDeposits[msg.sender], SECONDS_IN_DAY);

        return earningAmount;
    }

    function calculateEarnings(FarmType _farmType, uint256 _depositAmount, uint256 _timeElapsed) internal pure returns (uint256) {
    uint256 earningRate;
    
    if (_farmType == FarmType.DOGE) {
        earningRate = XMM_DOGE_RATE;
    } else if (_farmType == FarmType.SHIB) {
        earningRate = XMM_SHIB_RATE;
    } else if (_farmType == FarmType.FLOKI) {
        earningRate = XMM_FLOKI_RATE;
    } else {
        revert("Invalid farm choice");
    }

    uint256 earningAmount = (_depositAmount * _timeElapsed * earningRate) / SECONDS_IN_DAY;
    return earningAmount;
    }
}