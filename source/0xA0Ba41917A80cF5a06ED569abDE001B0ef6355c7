// SPDX-License-Identifier: UNLICENSE
pragma solidity ^0.8.17;

interface IBEP20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract DOBEINUStake{

    struct Deposit {
        uint amount;
        uint at;
        uint withdrawn;
        bool active;
    }       

    struct ReferralsCount {
        uint referrals_tier1;
        uint referrals_tier2;
        uint referrals_tier3;
        uint referrals_tier4;
    }

    struct ReferralsTotalDeposit{
        uint referrals_tier1;
        uint referrals_tier2;
        uint referrals_tier3;
        uint referrals_tier4;
    }

    struct Investor {
        bool registered;
        address referrer;
        uint balanceRef;
        uint totalRef;
        uint totalBonusRef;
        uint totalDepositedByRefs;
        Deposit[] deposits;
        uint totalDep;
        uint invested;
        uint paidAt;
        uint withdrawn;
    }
    
    uint DAY = 28800; 
    uint PERIOD = DAY * 300;
    uint PERCENT = 450;
      
    uint[] public refRewards;
    uint public totalInvestors;
    uint public totalInvested;
    uint public totalRefRewards;

    mapping(address => Investor) public investors;
    mapping(address => ReferralsTotalDeposit) public refDeposits;
    mapping(address => ReferralsCount) public refCountDeposits;

    address public tokenAddress;
    IBEP20 private token;

    event DepositAt(address user, uint amount);
    event Withdraw(address user, uint amount);

    constructor(address _tokenAddress) {        
        tokenAddress = _tokenAddress;
        token = IBEP20(tokenAddress);

        refRewards.push(30);
        refRewards.push(10);
        refRewards.push(10);
        refRewards.push(10);
        refRewards.push(10);
        refRewards.push(8);
        refRewards.push(8);
        refRewards.push(8);
        refRewards.push(8);
        refRewards.push(8);
        refRewards.push(5);
        refRewards.push(5);
        refRewards.push(5);
        refRewards.push(5);
        refRewards.push(5);
    }
  
    function register(uint _amount, address referrer) internal {
        if(!investors[msg.sender].registered) {
            
            investors[msg.sender].registered = true;
            totalInvestors++;
            
            if(referrer != msg.sender && referrer!=address(0)){
                investors[msg.sender].referrer = referrer;

                address rec = referrer;

                for (uint i = 0; i < refRewards.length; i++) {

                    if (!investors[rec].registered) 
                        break;

                    if (i == 0){ 
                        refCountDeposits[rec].referrals_tier1++;
                        refDeposits[rec].referrals_tier1 += _amount;
                    }

                    if (i >= 1 && i <= 4){ 
                        refCountDeposits[rec].referrals_tier2++;
                        refDeposits[rec].referrals_tier2 += _amount;
                    }

                    if (i >= 5 && i <= 9){ 
                        refCountDeposits[rec].referrals_tier3++;
                        refDeposits[rec].referrals_tier3 += _amount;
                    }

                    if (i >= 10){ 
                        refCountDeposits[rec].referrals_tier4++;
                        refDeposits[rec].referrals_tier4 += _amount;
                    } 

                    rec = investors[rec].referrer;
                }
            }
        }else{
            if(investors[msg.sender].referrer != address(0)){
                referrer = investors[msg.sender].referrer;

                address rec = referrer;

                for (uint i = 0; i < refRewards.length; i++) {
                    
                    if (!investors[rec].registered) 
                        break;

                    if (i == 0){ 
                        refCountDeposits[rec].referrals_tier1++;
                        refDeposits[rec].referrals_tier1 += _amount;
                    }

                    if (i >= 1 && i <= 4){ 
                        refCountDeposits[rec].referrals_tier2++;
                        refDeposits[rec].referrals_tier2 += _amount;
                    }

                    if (i >= 5 && i <= 9){ 
                        refCountDeposits[rec].referrals_tier3++;
                        refDeposits[rec].referrals_tier3 += _amount;
                    }

                    if (i >= 10){ 
                        refCountDeposits[rec].referrals_tier4++;
                        refDeposits[rec].referrals_tier4 += _amount;
                    }                   

                    rec = investors[rec].referrer;
                }
            }            
        }
        
        if(investors[investors[msg.sender].referrer].registered){
            uint a = _amount / 20;
            investors[investors[msg.sender].referrer].balanceRef += a;
            investors[investors[msg.sender].referrer].totalRef += a;
            investors[investors[msg.sender].referrer].totalDepositedByRefs += _amount;
            totalRefRewards += a;
        }
    }
  
    function rewardReferrers(uint amount, address referrer) internal {
        address rec = referrer;
        
        for (uint i = 0; i < refRewards.length; i++) {
          if (!investors[rec].registered) 
            break;
          
          uint a = amount * refRewards[i] / 100;
          investors[rec].balanceRef += a;
          investors[rec].totalBonusRef += a;
          totalRefRewards += a;
          
          rec = investors[rec].referrer;
        }
    }
      
    function maxPayoutOf(uint256 _amount) pure external returns(uint256) {
        return _amount * 45 / 10;
    } 

    function deposit(uint _amount, address referrer) external{
        require(token.allowance(msg.sender, address(this)) >= _amount, "Insufficient allowance");
        require(token.balanceOf(msg.sender) >= _amount);

        if(investors[msg.sender].registered)
            require( investors[msg.sender].deposits.length < 10, "The maximum number of deposits (10) per wallet has been reached");

        token.transferFrom(msg.sender, address(this), _amount);

        register(_amount, referrer); 
            
        investors[msg.sender].invested += _amount;
        totalInvested += _amount;
            
        investors[msg.sender].deposits.push(Deposit(_amount, block.number, 0, true));
        investors[msg.sender].totalDep++;

        if (investors[msg.sender].paidAt == 0) 
            investors[msg.sender].paidAt = block.number;
            
        emit DepositAt(msg.sender, _amount);
    }
  
    function withdrawable(address user) public view returns (uint amount) {
        Investor storage investor = investors[user];
        
        for (uint i = 0; i < investor.deposits.length; i++) {
            Deposit storage dep = investor.deposits[i];
            
            uint max_payout_depo = this.maxPayoutOf(dep.amount);
            
            if(dep.active == true && max_payout_depo > dep.withdrawn){
                uint finish = dep.at + PERIOD;
                uint since = investor.paidAt > dep.at ? investor.paidAt : dep.at;
                uint till = block.number > finish ? finish : block.number;
                uint _amount = 0;
                
                if (since < till){ 
                    _amount = dep.amount * (till - since) * PERCENT / PERIOD / 100;
                }
                
                if((dep.withdrawn + _amount) > max_payout_depo)
                    _amount = max_payout_depo - dep.withdrawn;
                
                amount += _amount;
            }
        }
    }
  
    function profit() internal returns (uint) {
        Investor storage investor = investors[msg.sender];

        uint amount = 0;
        
        for (uint i = 0; i < investor.deposits.length; i++) {
            Deposit storage dep = investor.deposits[i];
            
            uint max_payout_depo = this.maxPayoutOf(dep.amount);
            
            if(dep.active == true && max_payout_depo > dep.withdrawn){
                uint finish = dep.at + PERIOD;
                uint since = investor.paidAt > dep.at ? investor.paidAt : dep.at;
                uint till = block.number > finish ? finish : block.number;
                uint _amount = 0;
                
                if (since < till){ 
                    _amount = dep.amount * (till - since) * PERCENT / PERIOD / 100;
                }
                
                if((dep.withdrawn + _amount) > max_payout_depo){
                    _amount = max_payout_depo - dep.withdrawn;
                    investor.deposits[i].active = false;
                }
                    
                investor.deposits[i].withdrawn += _amount;
                amount += _amount;
            }
        }
        
        amount += investor.balanceRef;
        investor.balanceRef = 0;
        investor.paidAt = block.number;
        
        return amount;
    }
  
    function withdraw() external {
        uint amount = profit();
            
        rewardReferrers(amount, investors[msg.sender].referrer);
        
        if(amount > token.balanceOf(address(this)))
            amount = token.balanceOf(address(this));
        
        token.transfer(msg.sender, amount);
        investors[msg.sender].withdrawn += amount;
            
        emit Withdraw(msg.sender, amount);
    }
}