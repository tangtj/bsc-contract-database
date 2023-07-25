// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

abstract contract IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) public virtual returns (bool);
    function transfer(address recipient, uint256 amount) public virtual returns (bool);
}

contract StakingPool {
    struct Stake {
        uint256 amount;
        uint256 startTime;
        uint256 endTime;
        uint256 lastClaimTime;
    }

    mapping(address => Stake[]) public stakes;
    uint256 public totalSupply;
    uint256 public APR;
    uint256 public duration;
    address public owner;
    IERC20 public Token;

    event Transfer(address indexed from, address indexed to, uint256 value);

    constructor(address _stakingToken) {
        owner = msg.sender;
        Token = IERC20(_stakingToken);
    }

    modifier onlyOwner {
        require(msg.sender == owner, "Caller is not the owner");
        _;
    }

    function trasfeOwnership(address _newOwner) public onlyOwner{
        owner = _newOwner;
    }

    function setAPR (uint256 _Apr) external onlyOwner{
        APR = _Apr;
    }
    function setDuration(uint256 _month) external onlyOwner {
        duration = _month;
    }

    function setStakingToken(address _stakingToken) external onlyOwner {
        Token = IERC20(_stakingToken);
    }

    function stake(uint256 _amount) external {
        require(_amount > 0, "Staking amount must be greater than 0");
        require(Token.transferFrom(msg.sender, address(this), _amount), "Transfer error!");

        totalSupply += _amount;

        stakes[msg.sender].push(Stake({
            amount: _amount,
            startTime: block.timestamp,
            endTime: block.timestamp + (duration * 30 days),
            lastClaimTime: block.timestamp
        }));
    }

    function unstake(uint256 _index) external {
        Stake storage userStake = stakes[msg.sender][_index];
        require(userStake.endTime <= block.timestamp, "Staking period not yet ended");
        uint256 _amount = userStake.amount;

        uint256 reward = calculateReward(_amount, userStake.startTime, userStake.endTime);
        userStake.lastClaimTime = block.timestamp;
        uint256 total = _amount + reward;
        Token.transfer(msg.sender, total);
        totalSupply -= _amount;

        userStake.amount -= _amount;
        if(userStake.amount == 0) {
            delete stakes[msg.sender][_index];
        }
    }

    function calculateReward(uint256 _amount, uint256 _startTime, uint256 _endTime) public view returns (uint256) {
        uint256 timeDiff = _endTime - _startTime;
        uint256 reward = (_amount * APR * timeDiff) / (365 days * 100);
        return reward;
    }
    
    function getAllStakes(address _user) public view returns (Stake[] memory,uint256) {
        return (stakes[_user], stakes[_user].length);
    }
}