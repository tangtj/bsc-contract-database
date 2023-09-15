/*
$$$$$$$\        $$$$$$$$\       $$$$$$$\        
$$  __$$\       \__$$  __|      $$  __$$\       
$$ |  $$ |         $$ |         $$ |  $$ |      
$$$$$$$\ |         $$ |         $$$$$$$\ |      
$$  __$$\          $$ |         $$  __$$\       
$$ |  $$ |         $$ |         $$ |  $$ |      
$$$$$$$  |         $$ |         $$$$$$$  |      
\_______/          \__|         \_______/       
                                                
                                                                  

WEBSITE HTTPS://BTBCOIN.CLUB
*/
// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

interface IERC20 {
    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface ISwapRouter {
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
}

abstract contract Ownable {
    address internal _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = msg.sender; 
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == msg.sender, "!owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "new is 0");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}


contract BTBCoin is IERC20, Ownable{
    string private _name = "Btb Coin";
    string private _symbol = "BTB"; 
    uint8 private _decimals = 18;    
    uint256 private _totalsupply =1000000 * 10 ** 18;   
    uint256 public constant MAX = ~uint256(0);  
    uint256 public dayTimes = 86400; 
    uint256 public perDayReward = 30;  
    uint256 public maxD = 50;    

    uint256[] public shareRateList =[30,20,10,5,5,5,5,5,5,5];
    uint256 public rewardPoolAmount;  
    uint256 public preBuyAmount;
    uint256 public nftRewardAmount;
    uint256 public totalUser;
    uint256 public totalInvestTime;
    uint256 public totalValue;
    uint256 public totalWithdrawalValueBNB;
    uint256 public totalWithdrawalValueDNB;       

    mapping(address => uint256) private _balances;  
    mapping(address => mapping(address => uint256)) private _allowances;    

    address public dev = 0xC5855b54F15A6833e896d2187f5524A0E9C9A10c;   
    address public fund = 0x50A52956D3b3686FBFb532274b156a8985530004;
    address public nftRewards = 0x499894380130753c5EEba72bB60E80B25b2393a0;
    address public DEVfEES = 0x860Cf107DcB7DD5b3cD54ff1d3bab8B8b0d2Eb8c;
    address public factory = 0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73;
    address public router = 0x10ED43C718714eb63d5aA57B78B54704E256024E; 
    address public wbnb = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;   
    address public pair;
    address public wallet = 0xbBbBBBBbbBBBbbbBbbBbbbbBBbBbbbbBbBbbBBbB;
    bool public investStatus;  

    mapping(address=>address) public userTop;   
    mapping(address => uint256) public userLastActionBlock; 
    mapping(address => uint256) public userInvestBNBAmount;
    mapping(address => uint256) public userShareLevel;  
    mapping(address => uint256) public userPrebuyBNBAmount; 
    mapping(address => uint256) public userClaimRewardBNBValue;   
    mapping(address => uint256) public userClaimRewardBNBAmount;   
    mapping(address => uint256) public userClaimRewardDNBAmount;
    mapping(address => uint256) public userInviteTotalAddr; 
    mapping(address => uint256) public userInviteAddr;   
    mapping(address => uint256) public userTotalShareAddr;   
    mapping(address => uint256) public teamTotalAddr;   
    mapping(address => uint256) public teamTotalInvestValue;   

    mapping(address => uint256) public pendingShareRewards; 
    mapping(address => uint256) public claimdShareRewards; 
    mapping(address => uint256) public pendingTeamRewards; 
    mapping(address => uint256) public claimdTeamRewards; 

    event Invest(address indexed from, uint256 indexed times, uint256 value); 
    
    constructor (
    ){
        _balances[dev] = _totalsupply;
        emit Transfer(address(0), dev, _totalsupply);
        userTop[dev] = address(1); 
    }

    function symbol() external view override returns (string memory) {
        return _symbol;
    }

    function name() external view override returns (string memory) {
        return _name;
    }

    function decimals() external view override returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _totalsupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        if (_allowances[sender][msg.sender] != MAX) {
            _allowances[sender][msg.sender] = _allowances[sender][msg.sender] - amount;
        }
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    
    bool inswap;    
    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private{
        require(from != to,"Same");
        require(amount >0 ,"Zero");
        uint256 balance = _balances[from];
        require(balance >= amount, "balance Not Enough");
        _balances[from] = _balances[from] - amount;

        if(inswap){ 
            _balances[to] +=amount;
            emit Transfer(from, to, amount);
            return;
        }

        if(from == pair){
            revert('Buy Forbid');
        }

        if(to == address(this)){    
            address sender = msg.sender;
            require(sender == from,"Bot");
            _balances[address(this)] +=amount;
            emit Transfer(from, to, amount);   

            uint256 userInvestAmount = userInvestBNBAmount[sender];
            if(sender != tx.origin || userInvestAmount == 0 || userLastActionBlock[sender]==0 ||  (userLastActionBlock[sender] + dayTimes ) > block.timestamp){
                return;
            }


            uint256 userInvestDay = (block.timestamp - userLastActionBlock[sender]) / dayTimes;
            uint256 staticPending = ((userInvestAmount * userInvestDay) * perDayReward)/1000;
            if(staticPending>0){
                address top = userTop[sender]; 

                for(uint8 i=0;i<shareRateList.length;i++){
                    if(top !=address(0)){
                        if((userShareLevel[top] >= (i+1)) && (userInvestBNBAmount[top]>0)){
                            uint256 shareRewars = (staticPending * shareRateList[i]) / 100;
                            pendingShareRewards[top] += shareRewars;
                        }
                        top = userTop[top];
                    }
                }

                uint256 maxTeamRate = 60;   
                uint256 spendRate=0;    
                address top_team = userTop[sender]; 
                for(uint256 j=0;j<maxD;j++){ 
                    if(top_team !=address(0)){
                        if(teamTotalInvestValue[top_team] >= 30000*10**18 && maxTeamRate>spendRate && userInvestBNBAmount[top_team]>0){
                            pendingTeamRewards[top_team] += ((staticPending * (maxTeamRate - spendRate)) / 100);
                            spendRate = 60;
                        }
                        if(teamTotalInvestValue[top_team] >= 10000*10**18 && teamTotalInvestValue[top_team] < 30000*10**18 &&  spendRate<50 && userInvestBNBAmount[top_team]>0){
                            pendingTeamRewards[top_team] += ((staticPending * (50 - spendRate)) / 100);
                            spendRate = 50;
                        }
                        if(teamTotalInvestValue[top_team] >= 3000*10**18 && teamTotalInvestValue[top_team] < 10000*10**18 &&  spendRate<40 && userInvestBNBAmount[top_team]>0){
                            pendingTeamRewards[top_team] += ((staticPending * (40 - spendRate)) / 100);
                            spendRate = 40;
                        }
                        if(teamTotalInvestValue[top_team] >= 1000*10**18 && teamTotalInvestValue[top_team] < 3000*10**18 &&  spendRate<30 && userInvestBNBAmount[top_team]>0){
                            pendingTeamRewards[top_team] += ((staticPending * (30 - spendRate)) / 100);
                            spendRate = 30;
                        }
                        if(teamTotalInvestValue[top_team] >= 300*10**18 && teamTotalInvestValue[top_team] < 1000*10**18 &&  spendRate<20 && userInvestBNBAmount[top_team]>0){
                            pendingTeamRewards[top_team] += ((staticPending * (20 - spendRate)) / 100);
                            spendRate = 20;
                        }
                        if(teamTotalInvestValue[top_team] >= 100*10**18 && teamTotalInvestValue[top_team] < 300*10**18 &&  spendRate<10 && userInvestBNBAmount[top_team]>0){
                            pendingTeamRewards[top_team] += ((staticPending * (10 - spendRate)) / 100);
                            spendRate = 10;
                        }
                        top_team = userTop[top_team];
                    }
                }
            }

            uint256 myShareRewards = pendingShareRewards[sender];
            uint256 myTeamRewards = pendingTeamRewards[sender];
            uint256 myTotalRewards = staticPending + myShareRewards + myTeamRewards;

            if(userPrebuyBNBAmount[sender] >0 && (userPrebuyBNBAmount[sender] >= (userInvestAmount/100)) && (userInvestDay >= 1)){
                uint256 buyRate =userPrebuyBNBAmount[sender] * 100 /userInvestAmount; 
                if(buyRate > userInvestDay){ 
                    buyRate = userInvestDay;
                }
                uint256 needBuyValues = (userInvestAmount * buyRate) / 100;
                internalBuy(needBuyValues);
                
                if(userPrebuyBNBAmount[sender] >= needBuyValues){
                    uint256 endPre = userPrebuyBNBAmount[sender] - needBuyValues;
                    userPrebuyBNBAmount[sender] = endPre;
                }else{
                    userPrebuyBNBAmount[sender]=0;
                }
            }

            claimdShareRewards[sender] += myShareRewards;
            claimdTeamRewards[sender]+=myTeamRewards;
            pendingShareRewards[sender]=0;
            pendingTeamRewards[sender]=0;
            
            if((userClaimRewardBNBValue[sender] + myTotalRewards) >= userInvestAmount *2){
                
                myTotalRewards = (userInvestAmount *2) - userClaimRewardBNBValue[sender];   
            }

            if(myTotalRewards>0){

                uint256 dnbValue = myTotalRewards * 10 / 100;
                uint256 preDNBAmount = _getAmountsOut(dnbValue);
                require(_balances[address(this)] >= preDNBAmount,"Low BTB");
                uint256 endSendDnbAmount = _balances[address(this)] - preDNBAmount;
                _balances[address(this)] = endSendDnbAmount;
                _balances[sender] += preDNBAmount;
                emit Transfer(address(this), sender, preDNBAmount);
                
                uint256 bnbValue = myTotalRewards * 90 / 100;

                payable(sender).transfer(bnbValue);
                
                userClaimRewardBNBValue[sender] += myTotalRewards;
                userClaimRewardBNBAmount[sender]+=bnbValue;
                userClaimRewardDNBAmount[sender]+=preDNBAmount;
                userLastActionBlock[sender] = block.timestamp;
                totalWithdrawalValueBNB +=bnbValue;
                totalWithdrawalValueDNB+=preDNBAmount;
            }

            if(userClaimRewardBNBValue[sender]>=userInvestAmount *2){
                userInvestBNBAmount[sender] =0;
                userLastActionBlock[sender] = 0;
                userPrebuyBNBAmount[sender] =0;
                userClaimRewardBNBValue[sender]=0;
                userClaimRewardBNBAmount[sender]=0;
                userClaimRewardDNBAmount[sender]=0;
                pendingShareRewards[sender]=0;
                claimdShareRewards[sender]=0;
                pendingTeamRewards[sender]=0;
                claimdTeamRewards[sender]=0;
                totalUser--;
            }
            return;
        }

        uint256 sellBurnAmount;
        uint256 sellFundAmount;
        if(to == pair){ 
            if(from != dev){
                sellFundAmount = amount *3/100;
                sellBurnAmount = amount *2 /100;
            }
        }


        if(sellFundAmount>0){
            _balances[fund] +=sellFundAmount;
            emit Transfer(from, fund, sellFundAmount); 
        }
        if(sellBurnAmount>0){
            _balances[address(0)] +=sellBurnAmount;
            emit Transfer(from, address(0), sellBurnAmount); 
        }

        uint256 transAmount = amount -sellFundAmount- sellBurnAmount;
        _balances[to] +=transAmount;
        emit Transfer(from, to, transAmount); 
        
        bool shouldInvite = (userTop[to] == address(0)  
            && !isContract(from)
            && !isContract(to)
            && from != to   
            && userTop[from] !=address(0)   
        );

        if (shouldInvite) {
            userTop[to] = from;
            userInviteAddr[from] ++;
        }
        return;
    }


    function isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }

    

    receive() external payable{
        address sender = msg.sender;  
        uint256 fromBNBAmount = msg.value; 
        bool isBot = isContract(sender);  
        if(isBot  || (tx.origin != sender)){
            return;
        }
        require(investStatus,"NOT OPEN !"); 
        require(sender != dev,"DEV Forbid");
        require(userInvestBNBAmount[sender] ==0,"Wait End !");  
        require(fromBNBAmount >= 0.5 ether,"Min Invest Value");

        address top = userTop[sender];  
        require(top != address(0),"Need Bind top"); 

        userTotalShareAddr[top] ++; 

        if(userShareLevel[top] <10){
            uint256 _shareLevel = userTotalShareAddr[top];
            if(_shareLevel>10){
                _shareLevel =10;
            }
            userShareLevel[top] = _shareLevel;
        }

        for(uint256 i=0;i<maxD;i++){
            if(top !=address(0)){
                teamTotalAddr[top] ++;  
                teamTotalInvestValue[top] += fromBNBAmount; 
                top = userTop[top];
            }
        }

        userInvestBNBAmount[sender] = fromBNBAmount;
        userPrebuyBNBAmount[sender] = fromBNBAmount*19/100;
        uint256 firstBuyValue = fromBNBAmount/100;
        internalBuy(firstBuyValue); 
        userLastActionBlock[sender] = block.timestamp; 
        
        rewardPoolAmount += (fromBNBAmount*70/100);
        preBuyAmount +=(fromBNBAmount*20/100);
        nftRewardAmount +=(fromBNBAmount*10/100);
        payable(nftRewards).transfer(fromBNBAmount*9/100);   
        payable(DEVfEES).transfer(fromBNBAmount*1/100);
        totalUser++;
        totalInvestTime++;
        totalValue +=fromBNBAmount;

        if(_balances[address(this)] > 1e14){
            uint256 endBal = _balances[address(this)] -1e14;
            _balances[address(this)] = endBal;
            _balances[sender] = _balances[sender] + 1e14;
            emit Transfer(address(this), sender, 1e14);
        }
        emit Invest(sender,block.timestamp,fromBNBAmount);
        return;
    }

    function internalBuy(uint256 bnbAmount) private{
        require(!inswap,"inSwap");
        inswap =true;
        address[] memory path = new address[](2);
        path[0] = wbnb;
        path[1] = address(this);

        ISwapRouter(router).swapExactETHForTokensSupportingFeeOnTransferTokens{value: bnbAmount}(0, path, wallet, block.timestamp+600);
        uint256 bal = balanceOf(wallet);
        _balances[wallet] = 0;
        _balances[address(this)] +=bal;
        emit Transfer(wallet, address(this), bal);
        inswap=false;
    } 

    function _getAmountsOut(uint256 bnbAmount) private view returns(uint256){
        address[] memory path = new address[](2);
        path[0] = wbnb;
        path[1] = address(this);
        uint256[] memory amounts = ISwapRouter(router).getAmountsOut(bnbAmount, path);
        return amounts[1];
    }

    function openProject(address _pair) public onlyOwner{
        require(!investStatus,"OPEN");
        investStatus = true;
        pair = _pair;

    }

}