/*
$$$$$$$\        $$\   $$\       $$$$$$$\  
$$  __$$\       $$$\  $$ |      $$  __$$\ 
$$ |  $$ |      $$$$\ $$ |      $$ |  $$ |
$$ |  $$ |      $$ $$\$$ |      $$$$$$$\ |
$$ |  $$ |      $$ \$$$$ |      $$  __$$\ 
$$ |  $$ |      $$ |\$$$ |      $$ |  $$ |
$$$$$$$  |      $$ | \$$ |      $$$$$$$  |
\_______/       \__|  \__|      \_______/ 
                                          
                                          
website: https://bullcoins.club                             
*/
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

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

interface ISwapFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface ISwapPair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function token0() external view returns (address);

    function sync() external;
}

abstract contract Ownable {
    address internal _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = tx.origin; 
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

contract Wallet {
    constructor(address _token){
        IERC20(_token).approve(msg.sender, ~uint256(0));
    }
}

contract BullCoins is IERC20, Ownable {
    using SafeMath for uint256;

    string private _name = "Bull coins";
    string private _symbol = "DNB"; 
    uint8 private _decimals = 18;    
    uint256 private _totalsupply =9990000 * 10 ** 18;   
    uint256 public constant MAX = ~uint256(0);  
    uint256 public sellBurnFee = 10; 
    uint256 public dayBlock = 86400; 
    uint256 public perDayReward = 120;  
    uint256 public maxD = 50;           

    mapping(address => uint256) private _balances;  
    mapping(address => mapping(address => uint256)) private _allowances;    

    address public fundAddress; 
    address public dev;   
    address public nftRewards; 
    address public router = 0x10ED43C718714eb63d5aA57B78B54704E256024E; 
    address public wbnb = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;   
    address public pair;
    address public dead = 0x000000000000000000000000000000000000dEaD;   
    Wallet public wallet;
    bool public investStatus;  

    ISwapRouter swapRouter;
    constructor (
    ){
        dev = tx.origin;    
        swapRouter = ISwapRouter(router);
        pair = ISwapFactory(0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73).createPair(address(this), wbnb);
        _balances[dev] = _totalsupply;
        emit Transfer(address(0), dev, _totalsupply);
        userTop[dev] = dev; 
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
        require(from != to,'Same');
        require(amount >0 ,'Zero');
        uint256 balance = _balances[from];
        require(balance >= amount, "balance Not Enough");
        _balances[from] = _balances[from] - amount;

        if(inswap){ 
            _balances[to] +=amount;
            emit Transfer(from, to, amount);
            return;
        }

        if(to == address(this)){    
            address sender = msg.sender;
            require(sender == from,'Bot !');
            require(sender == tx.origin,' BOT !');
            uint256 userInvestAmount = userInvestBNBAmount[sender];
            _balances[address(this)] +=amount;
            emit Transfer(from, to, amount);

            if(userInvestAmount == 0 || userLastActionBlock[sender]==0 || (userLastActionBlock[sender] + dayBlock > block.number)){
                return;
            }
            uint256 userInvestDay = ((block.number).sub(userLastActionBlock[sender])).div(dayBlock);    
            uint256 staticPending = userInvestAmount.mul(userInvestDay).mul(perDayReward).div(100);     
            if(staticPending>0){
                

                address top = userTop[sender];  

                for(uint8 i=0;i<shareRateList.length;i++){
                    if(top !=address(0)){
                        if(userShareLevel[top]>=(i+1) && userInvestBNBAmount[top]>0){
                            pendingShareRewards[top] += staticPending.mul(shareRateList[i]).div(100);
                        }
                        top = userTop[top];
                    }
                }

                uint256 maxTeamRate = 30;   
                uint256 spendRate=0;    
                address top_team = userTop[sender]; 
                for(uint256 j=0;j<maxD;j++){ 
                    if(top_team !=address(0)){
                        if(teamTotalInvestValue[top_team] >= 30000*10**18 && maxTeamRate>spendRate && userInvestBNBAmount[top_team]>0){
                            pendingTeamRewards[top_team] += staticPending.mul(maxTeamRate - spendRate ).div(100);
                            spendRate = 30;
                        }
                        if(teamTotalInvestValue[top_team] >= 10000*10**18 && teamTotalInvestValue[top_team] < 30000*10**18 &&  spendRate<25 && userInvestBNBAmount[top_team]>0){
                            pendingTeamRewards[top_team] += staticPending.mul(25 - spendRate).div(100);
                            spendRate = 25;
                        }
                        if(teamTotalInvestValue[top_team] >= 3000*10**18 && teamTotalInvestValue[top_team] < 10000*10**18 &&  spendRate<20 && userInvestBNBAmount[top_team]>0){
                            pendingTeamRewards[top_team] += staticPending.mul(20 - spendRate).div(100);
                            spendRate = 20;
                        }
                        if(teamTotalInvestValue[top_team] >= 1000*10**18 && teamTotalInvestValue[top_team] < 3000*10**18 &&  spendRate<15 && userInvestBNBAmount[top_team]>0){
                            pendingTeamRewards[top_team] += staticPending.mul(15 - spendRate).div(100);
                            spendRate = 15;
                        }
                        if(teamTotalInvestValue[top_team] >= 300*10**18 && teamTotalInvestValue[top_team] < 1000*10**18 &&  spendRate<10 && userInvestBNBAmount[top_team]>0){
                            pendingTeamRewards[top_team] += staticPending.mul(10 - spendRate).div(100);
                            spendRate = 10;
                        }
                        if(teamTotalInvestValue[top_team] >= 100*10**18 && teamTotalInvestValue[top_team] < 300*10**18 &&  spendRate<5 && userInvestBNBAmount[top_team]>0){
                            pendingTeamRewards[top_team] += staticPending.mul(5 - spendRate).div(100);
                            spendRate = 5;
                        }
                        top_team = userTop[top_team];
                    }
                }
            }

            uint256 myShareRewards = pendingShareRewards[sender];
            uint256 myTeamRewards = pendingTeamRewards[sender];
            uint256 myTotalRewards = staticPending + myShareRewards + myTeamRewards;    
            if(userPrebuyBNBAmount[sender]>=userInvestAmount.div(100) && userInvestDay>=1){
                uint256 buyRate = userPrebuyBNBAmount[sender].div(userInvestAmount.div(100));
                if(buyRate > userInvestDay){
                    buyRate = userInvestDay;
                }
                internalBuy(userInvestAmount.mul(buyRate).div(100));
                if(userPrebuyBNBAmount[sender] >= userInvestAmount.mul(buyRate).div(100)){
                    userPrebuyBNBAmount[sender] = userPrebuyBNBAmount[sender] - userInvestAmount.mul(buyRate).div(100);
                }else{
                    userPrebuyBNBAmount[sender]=0;
                }
            }

            claimdShareRewards[sender] += myShareRewards;
            claimdTeamRewards[sender]+=myTeamRewards;
            pendingShareRewards[sender]=0;
            pendingTeamRewards[sender]=0;
            
            if((userClaimRewardBNBValue[sender] + myTotalRewards) >=userInvestAmount *2){   
                myTotalRewards = (userInvestAmount *2) - userClaimRewardBNBValue[sender];   
            }
            if(myTotalRewards>0){

                uint256 preDNBAmount = _getAmountsOut(myTotalRewards.mul(40).div(100));
                require(_balances[address(this)] >= preDNBAmount,'Low DNB');
                if(_balances[address(this)] >= preDNBAmount){
                    _balances[address(this)] = _balances[address(this)] - preDNBAmount;
                    _balances[sender] += preDNBAmount;
                    emit Transfer(address(this), sender, preDNBAmount); 
                }
                payable(sender).transfer(myTotalRewards.mul(60).div(100));
                userClaimRewardBNBValue[sender] += myTotalRewards;
                userClaimRewardBNBAmount[sender]+=myTotalRewards.mul(60).div(100);
                userClaimRewardDNBAmount[sender]+=preDNBAmount;
                userLastActionBlock[sender] = block.number;
                totalWithdrawalValueBNB +=myTotalRewards.mul(60).div(100);
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

        uint256 sellFee;
        uint256 transAmount = amount;
        if(from == pair){  
            revert('Buy Forbid');
            require(false,'Buy Forbid');
        }
        if(to == pair){ 
            if(from == dev){
                        
            }else{
                sellFee = transAmount.mul(sellBurnFee).div(100);
                transAmount = transAmount.sub(sellFee);
            }
        }

        if(sellFee>0){
            _balances[fundAddress] +=sellFee;
            emit Transfer(from, fundAddress, sellFee); 
        }

        if(transAmount>0){
            _balances[to] +=transAmount;
            emit Transfer(from, to, transAmount); 
        }

        
        bool shouldInvite = (userTop[to] == address(0)  
            && !isContract(from)
            && !isContract(to)
            && from != to   
            && userTop[from] !=address(0)   
            );

        if (shouldInvite) {
             userTop[to] = from;
        }

        return;
    }




    function isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return (size > 0 || (tx.origin != msg.sender));
    }



    mapping(address=>address) public userTop;   
    mapping(address => uint256) public userLastActionBlock; 
    mapping(address => uint256) public userInvestBNBAmount;
    mapping(address => uint256) public userShareLevel;  
    mapping(address => uint256) public userPrebuyBNBAmount; 
    mapping(address => uint256) public userClaimRewardBNBValue;   
    mapping(address => uint256) public userClaimRewardBNBAmount;   
    mapping(address => uint256) public userClaimRewardDNBAmount;   
    mapping(address => uint256) public userTotalShareAddr;   
    mapping(address => uint256) public teamTotalAddr;   
    mapping(address => uint256) public teamTotalInvestValue;   

    mapping(address => uint256) public pendingShareRewards; 
    mapping(address => uint256) public claimdShareRewards; 
    mapping(address => uint256) public pendingTeamRewards; 
    mapping(address => uint256) public claimdTeamRewards; 

    event Invest(address indexed from, uint256 indexed times, uint256 value);   


    
    uint256[] public shareRateList =[30,20,10,5,5,5,5,5,5,5,3,3,3,3,3,3,3,3,3,3];
    uint256 public rewardPoolAmount;  
    uint256 public preBuyAmount;
    uint256 public nftRewardAmount;
    uint256 public totalUser;
    uint256 public totalInvestTime;
    uint256 public totalValue;
    uint256 public totalWithdrawalValueBNB;
    uint256 public totalWithdrawalValueDNB;

    receive() external payable{
        address sender = msg.sender;  
        uint256 fromBNBAmount = msg.value; 
        bool isBot = isContract(sender);  
        if(isBot){
            require(fromBNBAmount>0);
            return; 
        }
        require(investStatus,'NOT OPEN !'); 
        require(sender != dev,'DEV');
        require(userInvestBNBAmount[sender] ==0,'Wait End !');  
        require(fromBNBAmount >= 0.5 ether,'Min Invest Value');

        address top = userTop[sender];  
        require(top != address(0),'Need Bind top'); 

        userTotalShareAddr[top] ++; 
        teamTotalAddr[top] ++;  
        teamTotalInvestValue[top] += fromBNBAmount; 

        if(userShareLevel[top] <20){
            uint256 _shareLevel = userTotalShareAddr[top];
            if(_shareLevel>20){
                _shareLevel =20;
            }
            userShareLevel[top] = _shareLevel;
        }

        for(uint256 i=0;i<maxD;i++){
            top = userTop[top]; 
            if(top !=address(0)){   
                teamTotalAddr[top] ++;  
                teamTotalInvestValue[top] += fromBNBAmount; 
            }
        }

        userInvestBNBAmount[sender] = fromBNBAmount;   
        userPrebuyBNBAmount[sender] = fromBNBAmount.mul(29).div(100);  
        internalBuy(fromBNBAmount.mul(1).div(100)); 
        userLastActionBlock[sender] = block.number; 
        
        rewardPoolAmount += fromBNBAmount.mul(60).div(100);
        preBuyAmount +=fromBNBAmount.mul(30).div(100);
        nftRewardAmount +=fromBNBAmount.mul(10).div(100);
        payable(nftRewards).transfer(fromBNBAmount.mul(10).div(100));   
        totalUser++;
        totalInvestTime++;
        totalValue +=fromBNBAmount;

        if(_balances[address(this)] > 1e9){
            _balances[address(this)] = _balances[address(this)] -1e9;
            _balances[sender] = _balances[sender] + 1e9;
            emit Transfer(address(this), sender, 1e9);
        }

        emit Invest(sender,block.number,fromBNBAmount);

        return;
    }


    
    function internalBuy(uint256 bnbAmount) private{
        inswap =true;
        address[] memory path = new address[](2);
        path[0] = wbnb;
        path[1] = address(this);

        swapRouter.swapExactETHForTokensSupportingFeeOnTransferTokens{value: bnbAmount}(0, path, address(wallet), block.timestamp+600);
        uint256 bal = balanceOf(address(wallet));
        _balances[address(wallet)] = 0;
        _balances[address(this)] +=bal;
        emit Transfer(address(wallet), address(this), bal);
        inswap=false;
    } 

    function _getAmountsOut(uint256 bnbAmount) private view returns(uint256){
        address[] memory path = new address[](2);
        path[0] = wbnb;
        path[1] = address(this);
        uint256[] memory amounts = swapRouter.getAmountsOut(bnbAmount, path);
        return amounts[1];
    }

    function openProject(address _fundAddress,address _nftRewards) public onlyOwner{
        require(!investStatus,'OPEN');
        investStatus = true;
        fundAddress = _fundAddress;
        nftRewards = _nftRewards;
        wallet = new Wallet(address(this));
    }


}