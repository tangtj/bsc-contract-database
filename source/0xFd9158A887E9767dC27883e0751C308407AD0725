// SPDX-License-Identifier: MIT

pragma solidity ^0.8.9;

interface IBEP20 {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address who) external view returns (uint256);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function transfer(address to, uint256 value) external returns (bool);

    function approve(address spender, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

contract Ownable {
    address private _owner;

    event OwnershipRenounced(address indexed previousOwner);
    event TransferOwnerShip(address indexed previousOwner);

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _owner = msg.sender;
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(msg.sender == _owner, "Not owner");
        _;
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipRenounced(_owner);
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public onlyOwner {
        emit TransferOwnerShip(newOwner);
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal {
        require(newOwner != address(0), "Owner can not be 0");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract TokenFarming is Ownable {
    uint256 public minimumStakingAmount;
    uint256 public maximumStakingAmount;
    uint256 public lockDays = 365;

    uint256 public stakingLevel1RangeStart;
    uint256 public stakingLevel1RangeEnd;
    uint256 public stakingLevel1RewardPercentage = 6; // this is the reward percent (in 1000) per day

    uint256 public stakingLevel2RangeStart;
    uint256 public stakingLevel2RangeEnd;
    uint256 public stakingLevel2RewardPercentage = 8; // this is the reward percent (in 1000) per day

    uint256 public stakingLevel3RangeStart;
    uint256 public stakingLevel3RangeEnd;
    uint256 public stakingLevel3RewardPercentage = 10; // this is the reward percent (in 1000) per day

    uint256[] public referralLevelRewardPercentage = [
        0,
        10,
        20,
        30,
        40,
        50,
        60,
        70,
        80,
        90,
        100
    ];

    IBEP20 public stakingToken;

    bool lock_ = false;
    bool public stakingActive = true;

    mapping(address => uint256) public stakingBalance;
    mapping(address => uint256) public rewardsClaimed;
    mapping(address => uint256) public totalRewardsClaimed;
    mapping(address => address) public referralMapping;
    mapping(address => uint256) public referralClaimableRewards;
    mapping(address => uint256) public totalReferralRewardsClaimed;
    mapping(address => uint256) public userUnlockTime;

    modifier lock() {
        require(!lock_, "Process is locked");
        lock_ = true;
        _;
        lock_ = false;
    }

    constructor() {
        // stakingToken = IBEP20(0x4B5C23cac08a567ecf0c1fFcA8372A45a5D33743);
        stakingToken = IBEP20(0x031b41e504677879370e9DBcF937283A8691Fa7f);
        minimumStakingAmount = 1000000000000000000000;
        maximumStakingAmount = 20000000000000000000000;

        uint8 _decimals = stakingToken.decimals();

        stakingLevel1RangeStart = 1000 * (10**_decimals);
        stakingLevel1RangeEnd = 5000 * (10**_decimals);
        stakingLevel2RangeStart = 5001 * (10**_decimals);
        stakingLevel2RangeEnd = 10000 * (10**_decimals);
        stakingLevel3RangeStart = 10001 * (10**_decimals);
        stakingLevel3RangeEnd = 20000 * (10**_decimals);
    }

    // staking function
    function stake(uint256 _amount, address _referral) public lock {
        require(stakingActive, "Staking is not active");
        require(
            _amount >= minimumStakingAmount,
            "Amount is less than minimum staking amount"
        );
        require(
            _amount <= maximumStakingAmount,
            "Amount is more than maximum staking amount"
        );
        require(
            stakingToken.transferFrom(msg.sender, address(this), _amount),
            "Transfer failed"
        );
        stakingBalance[msg.sender] += _amount;
        if (
            _referral != address(0) &&
            _referral != msg.sender &&
            stakingBalance[_referral] >= minimumStakingAmount &&
            referralMapping[msg.sender] == address(0)
        ) {
            referralMapping[msg.sender] = _referral;
        }
        userUnlockTime[msg.sender] = block.timestamp + (lockDays * 1 days);
        rewardsClaimed[msg.sender] = 0;

        // transfer tokens to this address
        stakingToken.transfer(address(this), _amount);
    }

    // claimable rewards function
    function claimableRewards(address _user) public view returns (uint256) {
        uint256 _stakingAmount = stakingBalance[_user];
        uint256 _userLockTime = userUnlockTime[_user] - (lockDays * 1 days);
        uint256 _days = (block.timestamp - _userLockTime) / (1 days);

        if(_days > lockDays){
            _days = lockDays;
        }

        uint256 _rewardPercentage = 0;
        if (
            _stakingAmount >= stakingLevel1RangeStart &&
            _stakingAmount <= stakingLevel1RangeEnd
        ) {
            _rewardPercentage = stakingLevel1RewardPercentage;
        } else if (
            _stakingAmount >= stakingLevel2RangeStart &&
            _stakingAmount <= stakingLevel2RangeEnd
        ) {
            _rewardPercentage = stakingLevel2RewardPercentage;
        } else if (
            _stakingAmount >= stakingLevel3RangeStart &&
            _stakingAmount <= stakingLevel3RangeEnd
        ) {
            _rewardPercentage = stakingLevel3RewardPercentage;
        }
        uint256 _amount = ((_stakingAmount * _rewardPercentage * _days) /
            (1000)) - rewardsClaimed[_user];
        return _amount;
    }

    // claimable referral rewards
    function claimableReferralRewards(address _user)
        public
        view
        returns (uint256)
    {
        return referralClaimableRewards[_user];
    }

    // claim rewards function
    function claimRewards() public lock {
        require(stakingBalance[msg.sender] > 0, "No staking balance");
        uint256 _amount = claimableRewards(msg.sender);
        require(_amount > 0, "No rewards");

        rewardsClaimed[msg.sender] += _amount;
        totalRewardsClaimed[msg.sender] += _amount;

        // transfer tokens to this address
        stakingToken.transfer(msg.sender, _amount);

        // referral rewards
        address _referral = referralMapping[msg.sender];
        if (_referral != address(0)) {
            uint256 _referralAmount = userReferralShare(_referral, _amount, 1);
            referralClaimableRewards[_referral] += _referralAmount;
        }
        _referral = referralMapping[_referral];
        if (_referral != address(0)) {
            uint256 _referralAmount = userReferralShare(_referral, _amount, 2);
            referralClaimableRewards[_referral] += _referralAmount;
        }
        _referral = referralMapping[_referral];
        if (_referral != address(0)) {
            uint256 _referralAmount = userReferralShare(_referral, _amount, 3);
            referralClaimableRewards[_referral] += _referralAmount;
        }
        _referral = referralMapping[_referral];
        if (_referral != address(0)) {
            uint256 _referralAmount = userReferralShare(_referral, _amount, 4);
            referralClaimableRewards[_referral] += _referralAmount;
        }
        _referral = referralMapping[_referral];
        if (_referral != address(0)) {
            uint256 _referralAmount = userReferralShare(_referral, _amount, 5);
            referralClaimableRewards[_referral] += _referralAmount;
        }
        _referral = referralMapping[_referral];
        if (_referral != address(0)) {
            uint256 _referralAmount = userReferralShare(_referral, _amount, 6);
            referralClaimableRewards[_referral] += _referralAmount;
        }
        _referral = referralMapping[_referral];
        if (_referral != address(0)) {
            uint256 _referralAmount = userReferralShare(_referral, _amount, 7);
            referralClaimableRewards[_referral] += _referralAmount;
        }
        _referral = referralMapping[_referral];
        if (_referral != address(0)) {
            uint256 _referralAmount = userReferralShare(_referral, _amount, 8);
            referralClaimableRewards[_referral] += _referralAmount;
        }
        _referral = referralMapping[_referral];
        if (_referral != address(0)) {
            uint256 _referralAmount = userReferralShare(_referral, _amount, 9);
            referralClaimableRewards[_referral] += _referralAmount;
        }
        _referral = referralMapping[_referral];
        if (_referral != address(0)) {
            uint256 _referralAmount = userReferralShare(_referral, _amount, 10);
            referralClaimableRewards[_referral] += _referralAmount;
        }
    }

    function userReferralShare(
        address _user,
        uint256 _amount,
        uint256 _referralLevel
    ) public view returns (uint256) {
        uint256 _stakingAmount = stakingBalance[_user];
        if (
            _stakingAmount >= stakingLevel1RangeStart &&
            _stakingAmount <= stakingLevel1RangeEnd
        ) {
            if (_referralLevel > 5) return 0;
            return
                (_amount * referralLevelRewardPercentage[_referralLevel]) /
                1000;
        } else if (
            _stakingAmount >= stakingLevel2RangeStart &&
            _stakingAmount <= stakingLevel2RangeEnd
        ) {
            if (_referralLevel > 7) return 0;
            return
                (_amount * referralLevelRewardPercentage[_referralLevel]) /
                1000;
        } else if (
            _stakingAmount >= stakingLevel3RangeStart &&
            _stakingAmount <= stakingLevel3RangeEnd
        ) {
            if (_referralLevel > 10) return 0;
            return
                (_amount * referralLevelRewardPercentage[_referralLevel]) /
                1000;
        }
        return 0;
    }

    // claim referral rewards function
    function claimReferralRewards() public lock {
        require(
            referralClaimableRewards[msg.sender] > 0,
            "No referral rewards"
        );
        uint256 _amount = claimableReferralRewards(msg.sender);
        referralClaimableRewards[msg.sender] = 0;
        totalReferralRewardsClaimed[msg.sender] += _amount;

        // transfer tokens to this address
        stakingToken.transfer(msg.sender, _amount);
    }

    // all onlyOwner functions here

    // set staking token
    function setStakingToken(address _stakingToken) public onlyOwner {
        stakingToken = IBEP20(_stakingToken);
    }

    // set minimum staking amount
    function setMinimumStakingAmount(uint256 _minimumStakingAmount)
        public
        onlyOwner
    {
        minimumStakingAmount = _minimumStakingAmount;
    }

    // set maximum staking amount
    function setMaximumStakingAmount(uint256 _maximumStakingAmount)
        public
        onlyOwner
    {
        maximumStakingAmount = _maximumStakingAmount;
    }

    // set lock days
    function setLockDays(uint256 _lockDays) public onlyOwner {
        lockDays = _lockDays;
    }

    // set staking level 1 range
    function setStakingLevel1Range(
        uint256 _stakingLevel1RangeStart,
        uint256 _stakingLevel1RangeEnd
    ) public onlyOwner {
        stakingLevel1RangeStart = _stakingLevel1RangeStart;
        stakingLevel1RangeEnd = _stakingLevel1RangeEnd;
    }

    // set staking level 1 reward percentage
    function setStakingLevel1RewardPercentage(
        uint256 _stakingLevel1RewardPercentage
    ) public onlyOwner {
        stakingLevel1RewardPercentage = _stakingLevel1RewardPercentage;
    }

    // set staking level 2 range
    function setStakingLevel2Range(
        uint256 _stakingLevel2RangeStart,
        uint256 _stakingLevel2RangeEnd
    ) public onlyOwner {
        stakingLevel2RangeStart = _stakingLevel2RangeStart;
        stakingLevel2RangeEnd = _stakingLevel2RangeEnd;
    }

    // set staking level 2 reward percentage
    function setStakingLevel2RewardPercentage(
        uint256 _stakingLevel2RewardPercentage
    ) public onlyOwner {
        stakingLevel2RewardPercentage = _stakingLevel2RewardPercentage;
    }

    // set staking level 3 range
    function setStakingLevel3Range(
        uint256 _stakingLevel3RangeStart,
        uint256 _stakingLevel3RangeEnd
    ) public onlyOwner {
        stakingLevel3RangeStart = _stakingLevel3RangeStart;
        stakingLevel3RangeEnd = _stakingLevel3RangeEnd;
    }

    // set staking level 3 reward percentage
    function setStakingLevel3RewardPercentage(
        uint256 _stakingLevel3RewardPercentage
    ) public onlyOwner {
        stakingLevel3RewardPercentage = _stakingLevel3RewardPercentage;
    }

    // set referral level reward percentage
    function setReferralLevelRewardPercentage(
        uint256 _referralLevel,
        uint256 _referralLevelRewardPercentage
    ) public onlyOwner {
        referralLevelRewardPercentage[
            _referralLevel
        ] = _referralLevelRewardPercentage;
    }

    // set staking active
    function setStakingActive(bool _stakingActive) public onlyOwner {
        stakingActive = _stakingActive;
    }

    // withdraw tokens
    function withdrawTokens(address _token, uint256 _amount) public onlyOwner {
        IBEP20(_token).transfer(msg.sender, _amount);
    }
}