// SPDX-License-Identifier: Unlicensed
pragma solidity 0.8.18;

abstract contract Context 
{
    function _msgSender() internal view virtual returns (address) 
    {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) 
    {
        return msg.data;
    }
}


interface IBEP20 
{
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
}


library SafeMath {

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }


    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }

}


abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _setOwner(msg.sender);
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }


    function transferOwnership(address newOwner) external virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private 
    {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }

}



contract  LUMIVest is Ownable 
{
    using SafeMath for uint256; 
    mapping(address => uint256) private _vestedbalances;
    mapping(address => uint256) private _vestingPlan;
    mapping(address => uint256) private _totalwithdrawals;
    mapping(address => string) private _description;

    struct Plan {
        uint256 periodLengthInDays; 
        uint256 tokensToBeReleasedPerPeriod;
        uint256 immediateRelease;
    }

     Plan[] public Plans;


    struct Member
    {
        address _address;
        uint256 indicator;
    }

    /*

        0   -  indicator means that member is not agreen for any action by owner
        1   -  indicator means that member is agreen to withdraw unvested tokens  from contract by owner

    **/

    Member[2] public members;

    modifier onlyMember() {
        require(members[0]._address == _msgSender() || members[1]._address == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    address[] public beneficiariesAddresses;

    IBEP20 lumiToken;

    uint256 public totalVestedLockAmount = 0;
    uint256 public totalVestedWithdrawnAmount = 0;
    uint256 public requestedTokensToWithdraw = 0;
    uint256 public deploymentTime = 0;

    constructor()
    {    

        lumiToken = IBEP20(0x9d6DF568D4D3E619B99a5f988ac7b2bCc3408753); // LUMIShare token address
        //             (Seconds        PeriodicRelease     ImmediateRelease)                         
        Plans.push(Plan(30*24*60*60,    3975848*10**18,     7951696*10**18));      // Plan   0
        Plans.push(Plan(60*24*60*60,    16698563*10**18,    16698563*10**18));     // Plan   1    
        Plans.push(Plan(30*24*60*60,    7951697*10**18,     7951697*10**18));      // Plan   2  
        Plans.push(Plan(30*24*60*60,    58047385*10**18,    0));                   // Plan   3
        Plans.push(Plan(90*24*60*60,    4230303*10**18,     0));                   // Plan   4
        Plans.push(Plan(90*24*60*60,    4230303*10**18,     1590339*10**18));      // Plan   5
        Plans.push(Plan(90*24*60*60,    5287878*10**18,     1192754*10**18));      // Plan   6
        Plans.push(Plan(90*24*60*60,    1057576*10**18,     0));                   // Plan   7
        Plans.push(Plan(60,             10*10**18,          5*10**18));             // Plan   8
        // Members 
        members[0] = Member(0xA21DD7b442A9d24468ACcF774F0504446ee3024d, 0);  // partner 1
        members[1] = Member(0xE19A799F4aeeeE3aCb38c6588d30F1cdCeFbEFe2, 0);  // partner 2
        deploymentTime = block.timestamp;
    }


    function addNewPlan(uint256 periodInSeconds, uint256 periodicReleaseTokens, uint256 immediateReleaseTokens) public onlyOwner 
    {
        Plans.push(Plan(periodInSeconds, periodicReleaseTokens, immediateReleaseTokens));
    }


    function getPlanDetailByIndex(uint256 index) public view returns(uint256 periodLengthInDays, uint256 tokensToBeReleasedPerPeriod, uint256 immediateRelease)
    {
        Plan memory p = Plans[index];
        periodLengthInDays = p.periodLengthInDays;
        tokensToBeReleasedPerPeriod = p.tokensToBeReleasedPerPeriod;
        immediateRelease =  p.immediateRelease;
        return (periodLengthInDays, tokensToBeReleasedPerPeriod, immediateRelease);
    }


    function getBeneficiaryAddressByIndex(uint256 index) public view returns(address)
    {
        return beneficiariesAddresses[index];
    }      


    function availTokensForVesting() public view returns(uint256)
    {
        uint256 availBalance = (lumiToken.balanceOf(address(this))).sub(totalVestedLockAmount);
        return availBalance;
    }
    

    event BeneficiaryAdded(address beneficiary, uint256 amount, uint256 timestamp);
    function addBeneficiary(uint256 _vestingAmount, address beneficiary, uint256 plan, string memory description) public onlyOwner 
    {
        uint256 availBalance = availTokensForVesting();
        require(availBalance>_vestingAmount, "Exceeding available balance");
        require(plan<Plans.length, "Invalid Plan");
        totalVestedLockAmount = totalVestedLockAmount.add(_vestingAmount);
        _vestedbalances[beneficiary] += _vestingAmount;
        _vestingPlan[beneficiary] =  plan;
        beneficiariesAddresses.push(beneficiary);
        _description[beneficiary] = description;
        emit BeneficiaryAdded(beneficiary, _vestingAmount, block.timestamp);
    }



    function getBeneficiary(address beneficiaryAddress) public view 
    returns(uint256 periodLengthInDays, uint256 tokensToBeReleasedPerPeriod, uint256 vestedBalance, uint256 withdrawnTokens, uint256 immediateRelease, uint256 _plan, string memory description)
    {
        uint256 planIndex = _vestingPlan[beneficiaryAddress];
        _plan = planIndex;
        Plan memory plan = Plans[planIndex];
        periodLengthInDays = plan.periodLengthInDays;
        tokensToBeReleasedPerPeriod = plan.tokensToBeReleasedPerPeriod;
        immediateRelease = plan.immediateRelease;
        vestedBalance = _vestedbalances[beneficiaryAddress];
        description = _description[beneficiaryAddress];
        withdrawnTokens = _totalwithdrawals[beneficiaryAddress];
        return (periodLengthInDays, tokensToBeReleasedPerPeriod, vestedBalance, withdrawnTokens, immediateRelease, _plan, description);
    }


    function addBeneficiariesInBulk(uint256[] memory vestingAmounts, address[] memory beneficiaries, uint256[] memory plans, string[] memory description) public onlyOwner 
    {
        uint256 len1 = vestingAmounts.length;
        uint256 len2 = beneficiaries.length;
        uint256 len3 = plans.length;
        uint256 len4 = description.length;
        require(len1==len2 && len2==len3 && len3==len4, "All three arrays must be of same size.");
        for(uint256 i=0; i<beneficiaries.length; i++)
        {
            addBeneficiary(vestingAmounts[i], beneficiaries[i], plans[i], description[i]);
        }
    }


    function availableVestedTokensForWidthrawal(address beneficiary) public view returns(uint256)
    {
        uint256 lapseOfTime = block.timestamp-deploymentTime;
        uint256 beneficiaryVestingPlan = _vestingPlan[beneficiary];
        Plan memory plan = Plans[beneficiaryVestingPlan];
        uint256 periodLengthInDays = plan.periodLengthInDays;
        uint256 tokensToBeReleasedPerPeriod = plan.tokensToBeReleasedPerPeriod;
        uint256 immediateRelease = plan.immediateRelease;
        uint256 periods = (lapseOfTime).div(periodLengthInDays);

        uint256 withdrawl = _totalwithdrawals[beneficiary];

        uint256 totalCalculatedVestingTokens = (periods.mul(tokensToBeReleasedPerPeriod)).add(immediateRelease).sub(withdrawl);
        
        uint256 balance = _vestedbalances[beneficiary];

        if(totalCalculatedVestingTokens > balance)
        {
            totalCalculatedVestingTokens = balance;
        }

        return  totalCalculatedVestingTokens;

    }



    event VestedTokensWithdraw(address beneficiary, uint256 amountWithdrawn, uint256 timestamp);
    function withdrawVestingTokens() public 
    {
        uint256 availableForWidthrawal = availableVestedTokensForWidthrawal(msg.sender);
        require(availableForWidthrawal>0, "No Balance");
        
        uint256 balance = _vestedbalances[msg.sender];

        _vestedbalances[msg.sender] = balance.sub(availableForWidthrawal);

        uint256 withdrawl = _totalwithdrawals[msg.sender];
        _totalwithdrawals[msg.sender] = withdrawl.add(availableForWidthrawal);

        lumiToken.transfer(msg.sender, availableForWidthrawal);

        totalVestedLockAmount = totalVestedLockAmount.sub(availableForWidthrawal);

        totalVestedWithdrawnAmount = totalVestedWithdrawnAmount.add(availableForWidthrawal);

        emit VestedTokensWithdraw(msg.sender, availableForWidthrawal, block.timestamp);

    }


    function totalBeneficiaries() public view returns(uint256)
    {
        return beneficiariesAddresses.length;
    }


    function setIndicator(uint256 _indicator) public onlyMember
    {
        if(msg.sender==members[0]._address) 
        {
            members[0].indicator =  _indicator;
        }

        if(msg.sender==members[1]._address) 
        {
            members[1].indicator =  _indicator;
        }
    }



    // Owner to request an amount of tokens withdraw, members will agree, then owner will be able to withdraw.
    function initWithdrawTokens(uint256 amount) public onlyOwner
    {
        uint256 availBalance = availTokensForVesting();
         require(requestedTokensToWithdraw<availBalance, "Exceeding available balance");
        requestedTokensToWithdraw = amount;
    }


    // Two members must be agree then owner can withdraw requested amount. 
    function withdrawTokens() public onlyOwner
    {   
        uint256 availBalance = availTokensForVesting();
        require(requestedTokensToWithdraw<availBalance, "Exceeding available balance");
        require(requestedTokensToWithdraw>0, "Zero token cannot be withdrawn");
        // Indicator one mean that members are agree to widthraw amount.
        require(members[0].indicator==1 &&  members[1].indicator==1, "All members are not agree to withdraw amount");
        lumiToken.transfer(msg.sender, requestedTokensToWithdraw);
        requestedTokensToWithdraw = 0;
        members[0].indicator=0;
        members[1].indicator=0;
    }
}