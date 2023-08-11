/**
 *Submitted for verification at BscScan.com on 2023-02-10
*/

// SPDX-License-Identifier: UNLICENSED

pragma solidity 0.8.15;

interface IERC20 {

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

interface IDaoStake {
    function userInfo(address _user) external view returns (uint256, uint256, uint256);
    function stake(address _to, uint256 _amount) external;
    function unstake(address _to, uint256 _amount) external;
    function cooldown() external;
    function claimRewards(address _to) external;
    function pendingReward(address _to) external view returns (uint256);
    function LVL() external view returns (IERC20);
    function LGO() external view returns (IERC20);
    function COOLDOWN_SECONDS() external view returns (uint256);
    function UNSTAKE_WINDOWN() external view returns (uint256);
}

interface IYieldStake {
    function userInfo(address _user) external view returns (uint256, uint256);
    function pendingRewards(uint256 _epoch, address _user) external view returns (uint256);
    function stake(address _to, uint256 _amount) external;
    function unstake(address _to, uint256 _amount) external;
    function claimRewards(uint256 _epoch, address _to) external;
}


/// @title Level Dao Vesting
/// @author Level
/// @notice Hold investor LVL to stake to LevelStake contract. These LVL will be unlocked after a certain period
contract LevelDaoVestingV2 {

    IERC20 public LVL;
    IERC20 public LGO;
    IDaoStake public DAO_STAKE;
    IYieldStake public YIELD_STAKE;

    uint256 public locked_duration;
    uint256 public vesting_duration;
    uint256 public start;

    uint256 public lvl_amount;
    uint256 public claimed_amount;
    address public investor;
    address public owner;

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner");
        _;
    }

    modifier onlyInvestor() {
        require(msg.sender == investor, "Only Investor");
        _;
    }

    constructor(
        address _daoStake,
        address _yieldStake,
        uint256 _start,
        uint256 _locked_duration,
        uint256 _vesting_duration) {
        DAO_STAKE = IDaoStake(_daoStake);
        YIELD_STAKE = IYieldStake(_yieldStake);
        LVL = DAO_STAKE.LVL();
        LGO = DAO_STAKE.LGO();
        start = _start;
        locked_duration = _locked_duration;
        vesting_duration = _vesting_duration;
        investor = msg.sender;
        owner = msg.sender;
    }

    function get_vesting_speed() public view returns (uint256) {
        if (vesting_duration == 0) {
            return 0;
        }
        return lvl_amount / vesting_duration; // number of LVL per second
    }

    function get_vesting_end() public view returns (uint256) {
        return start + locked_duration + vesting_duration;
    }

    function get_vested_duration() public view returns (uint256) {
        uint256 _now = block.timestamp;
        if (_now <= start + locked_duration) {
            return 0;
        } else if (_now >= get_vesting_end()) {
            return vesting_duration;
        } else {
            return _now - start - locked_duration;
        }
    }

    /* ========== VIEW FUNCTIONS ========== */

    /// @notice calculate amount of LVL can be claimed
    function claimable_LVL() public view returns (uint256) {
        uint256 _vested_duration = get_vested_duration();
        uint256 _vesting_speed = get_vesting_speed();
        uint256 _vested_amount = _vesting_speed * _vested_duration;
        return _vested_amount - claimed_amount;
    }

    function claimable_LGO() public view returns (uint256) {
        return DAO_STAKE.pendingReward(address(this)) + LGO.balanceOf(address(this));
    }

    function claimable_yield(uint256 _epoch) public view returns (uint256) {
        return YIELD_STAKE.pendingRewards(_epoch, address(this));
    }

    function lvl_staked_dao() external view returns (uint256) {
        (uint256 _amount, ,) = DAO_STAKE.userInfo(address(this));
        return _amount;
    }

    function lvl_staked_yield() external view returns (uint256) {
        (uint256 _amount, ) = YIELD_STAKE.userInfo(address(this));
        return _amount;
    }

    /* ========== PUBLIC FUNCTIONS ========== */

    function claim_LVL(uint256 _amount) external onlyInvestor {
        require(
            (_amount != 0 && _amount <= claimable_LVL()),
            "LevelDaoVesting::claim_LVL: invalid amount"
        );
        claimed_amount += _amount;
        LVL.transfer(investor, _amount);
        emit Withdrawn(investor, _amount);
    }

    function stake_dao(uint256 _amount) external onlyInvestor {
        _stake_dao(_amount);
    }

    function unstake_dao(uint256 _amount) external onlyInvestor {
        require(_amount > 0 , "LevelDaoVesting::unstake: invalid amount");
        DAO_STAKE.unstake(address(this), _amount);
        emit DaoUnstaked(_amount);
    }

    function claim_LGO() external onlyInvestor {
        DAO_STAKE.claimRewards(address(this));
        uint256 _amount = LGO.balanceOf(address(this));
        LGO.transfer(investor, _amount);
        emit LGOClaimed(investor, _amount);
    }

    function stake_yield(uint256 _amount) external onlyInvestor {
        _stake_yield(_amount);
    }

    function unstake_yield(uint256 _amount) external onlyInvestor {
        require(_amount > 0 , "LevelDaoVesting::unstake: invalid amount");
        YIELD_STAKE.unstake(address(this), _amount);
        emit YieldUnstaked(_amount);
    }

    function claim_yield(uint256 _epoch) external onlyInvestor {
        YIELD_STAKE.claimRewards(_epoch, investor);
    }

    function emergency_withdraw() external onlyOwner {
        require(investor != address(0), "Invalid investor address");
        uint256 _amount = LVL.balanceOf(address(this));
        claimed_amount += _amount;
        LVL.transfer(investor, _amount);
        emit Withdrawn(investor, _amount);
    }

    function transfer_ownership(address _newOwner) external onlyOwner {
        require(_newOwner != address(0), "Invalid address");
        owner = _newOwner;
    }

    function transfer_investor(address _investor) external onlyInvestor {
        require(_investor != address(0), "Invalid address");
        investor = _investor;
    }

    /* ========== INTERNAL FUNCTIONS ========== */

    function _stake_dao(uint256 _amount) internal {
        LVL.approve(address(DAO_STAKE), 0);
        LVL.approve(address(DAO_STAKE), _amount);
        DAO_STAKE.stake(address(this), _amount);
        emit DaoStaked(_amount);
    }

    function _stake_yield(uint256 _amount) internal {
        LVL.approve(address(YIELD_STAKE), 0);
        LVL.approve(address(YIELD_STAKE), _amount);
        YIELD_STAKE.stake(address(this), _amount);
        emit YieldStaked(_amount);
    }

    /* ========== EVENTS ========== */

    event DaoStakedForInvestor(uint256 _amount);
    event DaoStaked(uint256 _amount);
    event DaoUnstaked(uint256 _amount);
    event YieldStaked(uint256 _amount);
    event YieldUnstaked(uint256 _amount);
    event Withdrawn(address indexed _receiver, uint256 _amount);
    event LGOClaimed(address indexed _receiver, uint256 _amount);
}