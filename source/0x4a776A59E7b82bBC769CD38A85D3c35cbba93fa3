// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    function transfer(address to, uint256 _amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this;
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _setOwner(_msgSender());
    }

    function Owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(Owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

contract IGTStaking is Ownable {
    IERC20 public IGTToken;

    event Staked(address indexed user, uint256 amount);
    event claim_(address indexed user, uint256 amount, uint256 rewards);
    address[] private nonClaimbleAddresses;

    uint256 public TotalPlan;
    bool public FreezClaim = false;

    struct PlanRecord {
        uint256 TotalDeposit;
        uint256 TotalWithdrawal;
        uint256 startTime;
        uint256 totalmember;
        uint256 Amount;
        uint256 PlanId;
        bool isConcluded;
    }
 mapping(uint256 => PlanRecord) public plan;
    
       struct Stakedrecord {
        uint256 amount;
        uint256 ClaimAmount;
        uint256 lastUpdatetime;
        uint256 startTime;
        uint256 TotalPlanBuy;
    }

    mapping(address => mapping(uint256 => Stakedrecord)) public Stakedrecords;

    uint256 public dailyRewardPercentage = 5;


    constructor(address _IGTToken) {
        IGTToken = IERC20(_IGTToken);
    }

    function CreatePlan(uint256 PlanID, uint256 Amount) external onlyOwner {
        uint256 SerialNumber = PlanID;
        plan[SerialNumber].PlanId = PlanID;
        plan[SerialNumber].Amount = Amount;
        plan[SerialNumber].startTime = block.timestamp;
        TotalPlan += 1;
        plan[SerialNumber].isConcluded = true;
    }
    function TerminateThePlan(uint256 PlanId, bool status) external onlyOwner {
        plan[PlanId].isConcluded = status;
    }

    function Stake(uint256 PlanId) external {
        uint256 _amount = plan[PlanId].Amount;
        require(plan[PlanId].isConcluded, "Plan is terminated");
        require(_amount > 0, "Amount must be greater than 0");
        require(plan[PlanId].Amount <= IGTToken.balanceOf(_msgSender()), "Please enter valid amount");
        require(_amount % 100 == 0, "Amount must be multiple of 100");
        IGTToken.transferFrom(msg.sender, address(this), _amount);
        Stakedrecords[_msgSender()][PlanId].amount += _amount;
        Stakedrecords[_msgSender()][PlanId].lastUpdatetime = block.timestamp;
        Stakedrecords[_msgSender()][PlanId].startTime = block.timestamp;
        Stakedrecords[_msgSender()][PlanId].TotalPlanBuy += 1;
        plan[PlanId].TotalDeposit +=_amount;
        plan[PlanId].totalmember += 1;
        emit Staked(msg.sender, _amount);
    }

    function CheckClaim(address staked, uint256 PlanId)  public view returns (uint256 reward)
    {
        require(Stakedrecords[staked][PlanId].amount * 5 >= Stakedrecords[staked][PlanId].ClaimAmount);
        uint256 stakedAmount = Stakedrecords[staked][PlanId].amount;
        if (stakedAmount == 0) {
            return 0;
        }
        uint256 add;
        uint256 stakingDays = (block.timestamp -
            Stakedrecords[staked][PlanId].lastUpdatetime) / 1 days;
        uint256 rewards = (stakedAmount * 5) / 1000;

        if (
            block.timestamp - Stakedrecords[staked][PlanId].lastUpdatetime >
            1 days
        ) {
            for (uint256 i = 0; i < stakingDays; i++) {
                add += rewards;
            }
            return add;
        } else if (
            block.timestamp - Stakedrecords[staked][PlanId].lastUpdatetime ==
            1 days
        ) {
            return rewards;
        } else {
            return 0;
        }
    }

    function claim(uint256 PlanID) external {
        require(!FreezClaim,"At a movement claim is stop by the deployer");
        require(!isNonClaimbleAddress(_msgSender()),"You're not Cliam reward at this movement");
        uint256 stakedAmount = Stakedrecords[_msgSender()][PlanID].amount;
        require(stakedAmount > 0, "No staked amount");
        require( Stakedrecords[_msgSender()][PlanID].amount * 5 >=Stakedrecords[_msgSender()][PlanID].ClaimAmount);
        require(block.timestamp - Stakedrecords[_msgSender()][PlanID].lastUpdatetime >=1 days,"Can only claim once per day"
        );

        uint256 rewards = CheckClaim(msg.sender, PlanID);

        if (rewards > 0) {
            uint256 remainingRewards = (Stakedrecords[_msgSender()][PlanID]
                .amount * 5) - Stakedrecords[_msgSender()][PlanID].ClaimAmount;

            if (rewards > remainingRewards) {
                rewards = remainingRewards;
            }

            Stakedrecords[_msgSender()][PlanID].ClaimAmount += rewards;

            IGTToken.transfer(_msgSender(), rewards);
            plan[PlanID].TotalWithdrawal += rewards;

            if (
                Stakedrecords[_msgSender()][PlanID].amount * 5 <=
                Stakedrecords[_msgSender()][PlanID].ClaimAmount
            ) {

                Stakedrecords[_msgSender()][PlanID].amount = 0;
                Stakedrecords[_msgSender()][PlanID].ClaimAmount = 0;
            }

            Stakedrecords[_msgSender()][PlanID].lastUpdatetime = block.timestamp;
            emit claim_(msg.sender, stakedAmount, rewards);
        }
    }

    function disableAddressForClaim(address EnterAddress) external onlyOwner {
        require(EnterAddress != address(0), "Invalid address");
        nonClaimbleAddresses.push(EnterAddress);
    }

    function removeNonClaimblebleAddress(address _address) external onlyOwner {
        for (uint256 i = 0; i < nonClaimbleAddresses.length; i++) {
            if (nonClaimbleAddresses[i] == _address) {
                nonClaimbleAddresses[i] = nonClaimbleAddresses[
                    nonClaimbleAddresses.length - 1
                ];
                nonClaimbleAddresses.pop();
                break;
            }
        }
    }

    function isNonClaimbleAddress(address _address) public view returns (bool) {
        for (uint256 i = 0; i < nonClaimbleAddresses.length; i++) {
            if (nonClaimbleAddresses[i] == _address) {
                return true;
            }
        }
        return false;
    }
    function getPlans() external view returns (uint256[] memory planIds, uint256[] memory planAmounts) {
        uint256[] memory ids = new uint256[](TotalPlan);
        uint256[] memory amounts = new uint256[](TotalPlan);
        uint256 index = 0;

        for (uint256 i = 1; i <= TotalPlan; i++) {
            if (plan[i].isConcluded) {
                ids[index] = plan[i].PlanId;
                amounts[index] = plan[i].Amount;
                index++;
            }
        }
        assembly {
            mstore(ids, index)
            mstore(amounts, index)
        }

        return (ids, amounts);
    }

    function RescueToken(address reciver , uint256 Amount)external onlyOwner{
        require(IGTToken.balanceOf(address(this)) >= Amount,"Amount enter the greater than to the contract balance");
        IGTToken.transfer(reciver, Amount);
    }
    function _FreezClaim(bool status)external onlyOwner{
        FreezClaim = status;

    }
}