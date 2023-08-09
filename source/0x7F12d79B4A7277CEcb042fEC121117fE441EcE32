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
    mapping(address => uint256) public userLastClaimTime;
    mapping(address => uint256) public userLastEarningAmount;
    mapping(address => uint256) public userLastDepositTime;

    uint256 constant public XMM_DOGE_RATE = 79860000000000; 
    uint256 constant public XMM_SHIB_RATE = 50000000000000000000;
    uint256 constant public XMM_FLOKI_RATE = 200000000; 
    uint256 constant public SECONDS_IN_DAY = 86400; 

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
        
        FarmType farmChoice = userFarmChoice[msg.sender];
        require(farmChoice != FarmType(0), "Choose a farm first");
        
        claimEarnings();
        
        updateEarnings(msg.sender, farmChoice);
        
        userDeposits[msg.sender] += _amount;
        userLastDepositTime[msg.sender] = block.timestamp; 
        emit TokensDeposited(msg.sender, _amount, farmChoice);
    }

    function withdrawTokens(uint256 _amount) external {
        require(userDeposits[msg.sender] >= _amount, "Insufficient balance");
        
        FarmType farmChoice = userFarmChoice[msg.sender];
        require(farmChoice != FarmType(0), "Choose a farm first");
        
        claimEarnings(); 
        
        updateEarnings(msg.sender, farmChoice);
        
        IERC20 token = IERC20(tokenAddress);
        require(token.transfer(msg.sender, _amount), "Withdrawal failed");
        
        userDeposits[msg.sender] -= _amount;
        userLastClaimTime[msg.sender] = block.timestamp; 
        emit TokensWithdrawn(msg.sender, _amount, farmChoice);
    }

    function chooseFarm(FarmType _farmType) external {
        require(_farmType == FarmType.DOGE || _farmType == FarmType.SHIB || _farmType == FarmType.FLOKI, "Invalid farm choice");
        
        FarmType oldFarmChoice = userFarmChoice[msg.sender];
        if (oldFarmChoice != FarmType(0)) {
            claimEarnings(); 
        }
        
        userFarmChoice[msg.sender] = _farmType;
        userLastClaimTime[msg.sender] = block.timestamp;
    }

    function claimEarnings() public {
        FarmType farmChoice = userFarmChoice[msg.sender];
        require(farmChoice != FarmType(0), "Choose a farm first");
        
        updateEarnings(msg.sender, farmChoice);
        
        uint256 earningAmount = userLastEarningAmount[msg.sender];
        
        IERC20 token = IERC20(getTokenAddressByFarm(farmChoice));
        require(token.transfer(msg.sender, earningAmount), "Earning claim failed");
        emit EarningsClaimed(msg.sender, earningAmount, farmChoice);
        userLastEarningAmount[msg.sender] = 0;
        userLastClaimTime[msg.sender] = block.timestamp; 
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

    function updateEarnings(address _user, FarmType _farmType) internal {
        uint256 elapsedTime = block.timestamp - userLastClaimTime[_user];
        uint256 earningAmount = 0;

        if (_farmType == FarmType.DOGE) {
            earningAmount = (userDeposits[_user] * elapsedTime * XMM_DOGE_RATE) / SECONDS_IN_DAY;
        } else if (_farmType == FarmType.SHIB) {
            earningAmount = (userDeposits[_user] * elapsedTime * XMM_SHIB_RATE) / SECONDS_IN_DAY;
        } else if (_farmType == FarmType.FLOKI) {
            earningAmount = (userDeposits[_user] * elapsedTime * XMM_FLOKI_RATE) / SECONDS_IN_DAY;
        }
        
        userLastEarningAmount[_user] += earningAmount;
    }

    function getCurrentEarnings(address _user) public returns (uint256) {
        FarmType farmChoice = userFarmChoice[_user];
        require(farmChoice != FarmType(0), "Choose a farm first");
        
        uint256 currentTime = block.timestamp;
        uint256 elapsedTime = currentTime - userLastClaimTime[_user];
        
        updateEarnings(_user, farmChoice);
        
        uint256 earningPerSecond = 0;

        if (farmChoice == FarmType.DOGE) {
            earningPerSecond = (userDeposits[_user] * XMM_DOGE_RATE) / SECONDS_IN_DAY;
        } else if (farmChoice == FarmType.SHIB) {
            earningPerSecond = (userDeposits[_user] * XMM_SHIB_RATE) / SECONDS_IN_DAY;
        } else if (farmChoice == FarmType.FLOKI) {
            earningPerSecond = (userDeposits[_user] * XMM_FLOKI_RATE) / SECONDS_IN_DAY;
        }
        
        uint256 currentEarnings = userLastEarningAmount[_user] + (earningPerSecond * elapsedTime);
        userLastEarningAmount[_user] = currentEarnings;
        userLastClaimTime[_user] = currentTime;
        
        return currentEarnings;
    }

    function getCurrentEarningsPerSecond(address _user) public view returns (uint256) {
        FarmType farmChoice = userFarmChoice[_user];
        require(farmChoice != FarmType(0), "Choose a farm first");
        
        uint256 earningPerSecond = 0;

        if (farmChoice == FarmType.DOGE) {
            earningPerSecond = (userDeposits[_user] * XMM_DOGE_RATE) / SECONDS_IN_DAY;
        } else if (farmChoice == FarmType.SHIB) {
            earningPerSecond = (userDeposits[_user] * XMM_SHIB_RATE) / SECONDS_IN_DAY;
        } else if (farmChoice == FarmType.FLOKI) {
            earningPerSecond = (userDeposits[_user] * XMM_FLOKI_RATE) / SECONDS_IN_DAY;
        }
        
        return earningPerSecond;
    }
}