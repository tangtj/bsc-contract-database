// SPDX-License-Identifier: Unlicensed

pragma solidity ^0.8.15;

library SafeMath {
    

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }
    
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }
    
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }
    
}

contract HyhkPool{ 
    using SafeMath for uint256;
    function owner() public view virtual returns (address) {
        return _owner;
    }
    modifier onlyOwner() {
        require(owner() == msg.sender, "Ownable: caller is not the owner");
        _;
    }
    function renounceOwnership() public onlyOwner {
        _owner = address(0);
    }
    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }
    function _transferOwnership(address newOwner) internal {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _owner = newOwner;
    }
    
    mapping(address => bool) private _whiteList;
    address private _owner;
    address public Wallet_Hyhk=0x86614445833408ee1289f64602299CD589Ab909C;
    address public Wallet_USDT= 0x55d398326f99059fF775485246999027B3197955;
    address public  Wallet_SQ=0x06AD7d5E226016d3dd090305033003cD7B7aBbB8;
    address public Wallet_Project = 0x47819F99F7EaDb2B44901d4C7e63F77AaA5E7ad4;
    address public  Wallet_Promotion=0x8e273745cC902C994C9C2027469F5567bfa8e273;
    
    address public  HyhkPair=0xE45CC8FE88033157949900F85e7Ac5949C384257;
    address public constant Wallet_Burn = 0x000000000000000000000000000000000000dEaD; 

    uint8 private constant _decimals = 18;
    uint256 public totalLP; 
    uint256 public  totalMine;
    uint256 public totalInvest; 

    uint256 constant public TIME_STEP = 1 days;
    uint256 public totalpool; 
     uint256 public maxpool; 
     uint256 public  hgrate;
    
    uint256[15] public DynamicRate;
    
    uint256 public   preRate;
    uint256 public   curRate;
    uint256 public   Ratetime;

    uint256 public _startTime;  
     IUniswapV2Router02 public uniswapV2Router;
    uint256 public UsdtRate;
    uint256 public SqRate;
    uint256 public ProjectfeeRate;
    uint256 public PromotionFeeRate;
    uint256 public LPStopTime;
    uint256 public LPBountRate;

    uint256 constant public PERCENTS_DIVIDER = 1000;
    uint256 public DepositTimes = 30;
    uint256 public BountRate = 30;
   
    bool private swapping;
    

    struct User {
        Deposit[] Minelists;
        address[] teams;
        Deposit[] investLists;
        uint256 Minecheckpoint;
        uint256 totalMine;
        uint256 Minewithdrawn;
        uint256 Minecanwithdrawn;
        uint256 DirectCount;
        uint256 DirectAmount;
        uint256 totalInvest;
    }
     struct LpUser {
        Deposit[] LPlists;
        uint256 LPcheckpoint;
        uint256 totalLP;
        uint256 LPwithdrawn;
        uint256 LPcanwithdrawn;

    }

    struct Deposit {
        uint256 amount;
        uint256 Deposittype;
        uint256 start;
        uint256 checkpoint;
    }


    mapping(address => User) public Users;
    mapping(address => LpUser) public LpUsers;
    mapping(address => address) public Referrers;

    
    event WithdrawnPool(address indexed user, uint256 amount, uint256 pooltype);
    event AddLP(address indexed user, uint256 amount1, uint256 amount2);
    event RemoveLP(address indexed user, uint256 amount);
    event WithdrawnLP(address indexed user, uint256 amount);
    event WithdrawnMine(address indexed user, uint256 amount);
    event Mining(address indexed user, uint256 amount);
    event Invest(address indexed user, uint256 amount1);
    event WithdrawnInvest(address indexed user, uint256 amount, uint256 itype);
    event RemoveInvest(address indexed user, uint256 amount);
   
    
    constructor () {
        _owner=msg.sender;
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E); 
        uniswapV2Router = _uniswapV2Router;
        maxpool=100*10**_decimals;
        hgrate=5;
        DynamicRate=[15,10,10,10,5,5,5,5,5,3,3,3,3,3,3];
        LPStopTime=1737379355;
        curRate=10;
        UsdtRate=800;
        SqRate=200;
        LPBountRate=30;
        ProjectfeeRate=5;
        PromotionFeeRate=15;
        
        _whiteList[Wallet_Hyhk] = true;

    }

    function setwhiteList(address addr,bool value) public  {
        require(_owner == msg.sender);
         _whiteList[addr] = value;
    }

    receive() external payable {}

    function mining(address referrer,uint256 amount) public   returns (bool){
            require(amount >=100*10**18, "must>=100");
            User storage user=Users[msg.sender];
           
            require( block.timestamp>_startTime , "It's not startTime1");
            uint256 usdtAmount=amount * UsdtRate /PERCENTS_DIVIDER;
            uint256 SQAmount=amount * SqRate /PERCENTS_DIVIDER;
           
           require(IERC20(Wallet_USDT).balanceOf(msg.sender)>=usdtAmount, "have not enough USDT token.");   
            require(IERC20(Wallet_SQ).balanceOf(msg.sender)>=SQAmount, "have not enough SQ token.");   
           
            safeTransferFrom(Wallet_USDT,msg.sender,address(this),usdtAmount);
            safeTransferFrom(Wallet_SQ,msg.sender,address(this),SQAmount);

            //先计算可提现
            uint256 candwithdrawMine=CanWithdrawMine(msg.sender);
            user.Minecanwithdrawn=user.Minecanwithdrawn+candwithdrawMine;
            user.Minecheckpoint=block.timestamp;

            if (Referrers[msg.sender] == address(0) &&  referrer != msg.sender) {
                Referrers[msg.sender]=referrer;
            }
            address upline=Referrers[msg.sender];
            require(upline != address(0), " Invalid referrer1.");   
            require(Referrers[upline] != address(0) || upline ==_owner, " Invalid referrer.");   
            
            if(user.totalMine==0){
                Users[upline].DirectCount=Users[upline].DirectCount+1;
                Users[upline].teams.push(msg.sender);
            }
            Users[upline].DirectAmount=Users[upline].DirectAmount+amount;
            totalMine=totalMine+amount;
            user.totalMine=user.totalMine+amount;

            uint256 projectamount=amount*ProjectfeeRate/100;
            uint256 hgamount=amount*hgrate/100;

            IERC20(Wallet_USDT).transfer(Wallet_Project, projectamount);

            totalpool=totalpool+hgamount;   
            if (!swapping && totalpool>=maxpool) {
                    swapping = true;
                    swapTokensForRewardToken(totalpool);
                    totalpool=0;
                    swapping = false;
            }
            user.Minelists.push(Deposit(amount,0,block.timestamp,block.timestamp));
           
            emit Mining(msg.sender, amount);
        return true;
    }



    function addMine(address useraddr,uint256 value) public  virtual onlyOwner  returns (bool){

            User storage user=Users[useraddr];
            uint256 candwithdrawMine=CanWithdrawMine(useraddr);
            user.Minecanwithdrawn=user.Minecanwithdrawn+candwithdrawMine;
            user.Minecheckpoint=block.timestamp;
            user.Minelists.push(Deposit(value,0,block.timestamp,block.timestamp));
            
            address upline=Referrers[useraddr];
            require(upline != address(0), " Invalid referrer1.");   
            require(Referrers[upline] != address(0) || upline ==_owner, " Invalid referrer.");   
            
            if(user.totalMine==0){
                Users[upline].DirectCount=Users[upline].DirectCount+1;
                Users[upline].teams.push(useraddr);
            }
            user.totalMine=user.totalMine+value;
            return true;
    }

    function deleMine(address useraddr,uint256 index) public  virtual onlyOwner  returns (bool){
            User storage user=Users[useraddr];
            require(user.Minelists.length>index);
            uint256 candwithdrawMine=CanWithdrawMine(useraddr);
            user.Minecanwithdrawn=user.Minecanwithdrawn+candwithdrawMine;
            user.Minecheckpoint=block.timestamp;
            uint256 amount=user.Minelists[index].amount;
            user.Minelists[index].amount=0;
            user.totalMine=user.totalMine-amount;
            return true;
    }
    

    function withdrawMine() public   returns (bool){
            User storage user=Users[msg.sender];
            require(user.totalMine >=0, "not Mine");
            require( block.timestamp>_startTime , "It's not startTime1");
            uint256 candwithdrawmine=CanWithdrawMine(msg.sender);
            uint256 amount= user.Minecanwithdrawn+candwithdrawmine;
            user.Minecheckpoint=block.timestamp;
            user.Minecanwithdrawn=0;
            user.Minewithdrawn=user.Minewithdrawn+amount;
            uint256 Promotionfee=amount*PromotionFeeRate/100;
            uint256 mineamount=amount-Promotionfee; 
            IERC20(Wallet_Hyhk).transfer(msg.sender, mineamount);
            IERC20(Wallet_Hyhk).transfer(Wallet_Promotion, Promotionfee);
            
            address upline = Referrers[msg.sender];
            uint256 uplink=0;
            uint256 Projectfee=0; 
            while(upline != address(0) && uplink<15){
                uint256 bouns=amount.mul(DynamicRate[uplink]).div(100);
                if(bouns>0 ){
                    if(Users[upline].totalMine>0&&( Users[upline].DirectCount>uplink||Users[upline].DirectCount>=10)){
                        IERC20(Wallet_Hyhk).transfer(upline, bouns);        
                    }else{
                        Projectfee=Projectfee+bouns;
                    }
                }
                upline =Referrers[upline];
                uplink=uplink+1;
			}
            if(Projectfee>0){
                IERC20(Wallet_Hyhk).transfer(Wallet_Project, Projectfee);
            }
            emit WithdrawnMine(msg.sender,amount);
            return true;
    }

    function CanWithdrawMine(address useraddress) public  view returns (uint256){
        User memory Mineuser = Users[useraddress];
		if(block.timestamp <= _startTime)return 0;
        if(Mineuser.totalMine==0)return 0;
        uint256 dividends;
        uint checkpoint;
        uint lastpoint;
        uint256 totalAmount;

        for (uint256 i = 0; i < Mineuser.Minelists.length; i++) {

            if (Mineuser.Minelists[i].start > Mineuser.Minecheckpoint) {
                checkpoint=Mineuser.Minelists[i].start;
            }
            else{
                checkpoint=Mineuser.Minecheckpoint;
            }

            if (Mineuser.Minelists[i].start+200* TIME_STEP> block.timestamp) {
                lastpoint=block.timestamp;
            }
            else{
                lastpoint=Mineuser.Minelists[i].start+200* TIME_STEP;
            }


            if(checkpoint+TIME_STEP*30<lastpoint){
                checkpoint=lastpoint-TIME_STEP*30;
            }
            if(checkpoint<lastpoint){
                if(Ratetime==0 ||Ratetime<checkpoint){
                    dividends  = Mineuser.Minelists[i].amount.mul(curRate).div(PERCENTS_DIVIDER)
                        .mul(lastpoint.sub(checkpoint))
                        .div(TIME_STEP);
                        totalAmount = totalAmount.add(dividends);
                }else if(Ratetime>=lastpoint) {
                    dividends  = Mineuser.Minelists[i].amount.mul(preRate).div(PERCENTS_DIVIDER)
                        .mul(lastpoint.sub(checkpoint))
                        .div(TIME_STEP);
                    totalAmount = totalAmount.add(dividends);
                }else{
                    dividends  = Mineuser.Minelists[i].amount.mul(preRate).div(PERCENTS_DIVIDER)
                        .mul(Ratetime.sub(checkpoint))
                        .div(TIME_STEP);
                    totalAmount = totalAmount.add(dividends);
                    dividends  = Mineuser.Minelists[i].amount.mul(curRate).div(PERCENTS_DIVIDER)
                        .mul(lastpoint.sub(Ratetime))
                        .div(TIME_STEP);
                    totalAmount = totalAmount.add(dividends);
                }
            }
            
        }
        uint256[] memory  HyhkAmounts=getAmounts(Wallet_Hyhk,totalAmount);
        uint256 HyhkAmount=HyhkAmounts[HyhkAmounts.length - 1];
        return HyhkAmount;
    }




    function addLP(uint256 usdtAmount ) public   returns (bool){
       require( block.timestamp>_startTime , "It's not startTime1");
       
        require(IERC20(Wallet_USDT).balanceOf(msg.sender) >=usdtAmount, "have not enough USDT token.");   
        uint256  HyhkAmount=usdtAmount*10**_decimals/tokenPrice();
 
        require(HyhkAmount > 0,'error1');
        
        require(IERC20(Wallet_Hyhk).balanceOf(msg.sender) >=HyhkAmount, "have not enough  Hyhk token.");   
        LpUser storage user=LpUsers[msg.sender];
        //先计算可提现
        uint256 candwithdrawlp=CanWithdrawLP(msg.sender);
        user.LPcanwithdrawn=user.LPcanwithdrawn+candwithdrawlp;
        user.LPcheckpoint=block.timestamp;
        safeTransferFrom(Wallet_Hyhk,msg.sender,address(this),HyhkAmount);
        safeTransferFrom(Wallet_USDT,msg.sender,address(this),usdtAmount);
            
        uint256 balance1=IERC20(HyhkPair).balanceOf(address(this));
        addLiquidityAndStake(HyhkAmount,usdtAmount);
        uint256 balance2=IERC20(HyhkPair).balanceOf(address(this));
        uint256 LP=balance2-balance1;
        totalLP=totalLP+LP;
        user.totalLP=user.totalLP+LP;
        user.LPlists.push(Deposit(LP,0,block.timestamp,block.timestamp));
        emit AddLP(msg.sender, usdtAmount,HyhkAmount);
        return true;
    }

    

    function removeLP(uint256  amount) public   returns (bool){
        require(amount >0, "It's not enough BNB");
        LpUser storage user=LpUsers[msg.sender];
        require(user.totalLP >=amount, "It's not enough BNB");
        require( block.timestamp>_startTime , "It's not startTime1");
        //可提现清零
        user.LPcheckpoint=block.timestamp;
        user.LPcanwithdrawn=user.LPcanwithdrawn+CanWithdrawLP(msg.sender);

        user.totalLP =user.totalLP -amount;
        totalLP=totalLP-amount;

        uint256 Hyhkbalance1=IERC20(Wallet_Hyhk).balanceOf(address(this));
        uint256 Usdtbalance1=IERC20(Wallet_USDT).balanceOf(address(this));

        (uint256 amountToken ,uint256 amountETH)  =removeLiquidityAndStake(address(this),amount);
        
        uint256 Hyhkbalance2=IERC20(Wallet_Hyhk).balanceOf(address(this));
        uint256 Usdtbalance2=IERC20(Wallet_USDT).balanceOf(address(this));

        if(amountToken>0 && Hyhkbalance2>Hyhkbalance1){
            amountToken=(amountToken>(Hyhkbalance2-Hyhkbalance1))?(Hyhkbalance2-Hyhkbalance1):amountToken;
            IERC20(Wallet_Hyhk).transfer(msg.sender, amountToken);
        }
        if(amountETH>0 && Usdtbalance2>Usdtbalance1){
            amountETH=(amountToken>(Usdtbalance2-Usdtbalance1))?(Usdtbalance2-Usdtbalance1):amountETH;
            IERC20(Wallet_USDT).transfer(msg.sender, amountETH);
        }
        user.LPlists.push(Deposit(amount,1,block.timestamp,block.timestamp));

        emit RemoveLP(msg.sender,amount);
        return true;
    }

    function withdrawLP() public   returns (bool){
        LpUser storage user=LpUsers[msg.sender];
        require( block.timestamp>_startTime , "It's not startTime1");
        uint256 candwithdrawlp=CanWithdrawLP(msg.sender);
        uint256 amount= user.LPcanwithdrawn+candwithdrawlp;
        user.LPcheckpoint=block.timestamp;
        user.LPcanwithdrawn=0;
        
        uint256 lpamount=amount; 
        IERC20(Wallet_Hyhk).transfer(msg.sender, lpamount);
        emit WithdrawnLP(msg.sender,amount);
        return true;
    }

    function CanWithdrawLP(address useraddress) public  view returns (uint256){
        LpUser memory lpuser = LpUsers[useraddress];
		if(block.timestamp <= _startTime)return 0;
        if(LPStopTime>0 && LPStopTime <= lpuser.LPcheckpoint)return 0;
        if(lpuser.totalLP==0)return 0;
        uint256 HyhkReverse=getTokenReverse();
        IUniswapV2Pair swapPair = IUniswapV2Pair(HyhkPair);
        uint256 totalBalance=swapPair.totalSupply();
        //uint256 userBalance=swapPair.balanceOf(useraddress);
        uint256 marketValue=HyhkReverse.mul(lpuser.totalLP).div(totalBalance).mul(2);
        uint256 endtime=block.timestamp;
        if(endtime>LPStopTime && LPStopTime>0)endtime=LPStopTime;

        uint256 dividends  =marketValue.mul(LPBountRate).div(30000)
				.mul(endtime.sub(lpuser.LPcheckpoint))
				.div(TIME_STEP);
        return dividends;
    }

    function adminAddLP( address useraddr ,uint256 Amount )  public virtual onlyOwner   returns (bool){
       require( block.timestamp>_startTime , "It's not startTime1");
        LpUser storage user=LpUsers[useraddr];
       //先计算可提现
        uint256 candwithdrawlp=CanWithdrawLP(useraddr);
        user.LPcanwithdrawn=user.LPcanwithdrawn+candwithdrawlp;
        user.LPcheckpoint=block.timestamp;
            
        totalLP=totalLP+Amount;
        user.totalLP=user.totalLP+Amount;
        user.LPlists.push(Deposit(Amount,0,block.timestamp,block.timestamp));
        return true;
    }

    function adminRemoveLP( address useraddr ,uint256 Amount )  public virtual onlyOwner   returns (bool){
       require( block.timestamp>_startTime , "It's not startTime1");
        LpUser storage user=LpUsers[useraddr];
        require( user.totalLP>=Amount , "It's not startTime1");
       
        //先计算可提现
        uint256 candwithdrawlp=CanWithdrawLP(useraddr);
        user.LPcanwithdrawn=user.LPcanwithdrawn+candwithdrawlp;
        user.LPcheckpoint=block.timestamp;
             
        totalLP=totalLP-Amount;
        user.totalLP=user.totalLP-Amount;
        user.LPlists.push(Deposit(Amount,0,block.timestamp,block.timestamp));
        return true;
    }

    function invest(uint256  HyhkAmount  ) public   returns (bool){
        require( block.timestamp>_startTime , "It's not startTime1");
        require(HyhkAmount > 0,'error1');
        
        require(IERC20(Wallet_Hyhk).balanceOf(msg.sender) >=HyhkAmount, "have not enough  Hyhk token.");   
        User storage user=Users[msg.sender];
        
        //先计算可提现
        safeTransferFrom(Wallet_Hyhk,msg.sender,address(this),HyhkAmount);
   
        totalInvest=totalInvest+HyhkAmount;
        user.totalInvest=user.totalInvest+HyhkAmount;
        user.investLists.push(Deposit(HyhkAmount,0,block.timestamp,block.timestamp));
        emit Invest(msg.sender,HyhkAmount);
        return true;
    }

  
    function removeInvest(uint256  index) public   returns (bool){
        User storage user=Users[msg.sender];
        require(user.investLists.length >index, "err1");
        require( block.timestamp>_startTime , "It's not startTime1");
        //可提现清零
        Deposit storage deposit=user.investLists[index];
        uint256 amount=deposit.amount;
        require(amount >0, "err2");

        user.totalInvest =user.totalInvest -amount;
        totalInvest=totalInvest-amount;
        deposit.amount=0;
        IERC20(Wallet_Hyhk).transfer(msg.sender, amount);

        emit RemoveInvest(msg.sender,amount);
        return true;
    }

     function withdrawInvest(uint256  index) public   returns (bool){
        User storage user=Users[msg.sender];
        require(user.investLists.length >index, "err1");
        require( block.timestamp>_startTime , "It's not startTime1");
        Deposit storage deposit=user.investLists[index];
        require(deposit.amount >0, "err2");
       
        uint256 candwithdrawAmount=CanWithdrawInvest(msg.sender,index);
        require(candwithdrawAmount >0, "err4");
        require(deposit.start+ DepositTimes.mul(TIME_STEP)<block.timestamp, "err3");
        deposit.checkpoint=block.timestamp;
        IERC20(Wallet_Hyhk).transfer(msg.sender, candwithdrawAmount);
        emit WithdrawnInvest(msg.sender,candwithdrawAmount,0);
        return true;
    }



    function withdrawInvestAll() public   returns (bool){
        User storage user=Users[msg.sender];
        require(user.investLists.length >0, "err1");
        require( block.timestamp>_startTime , "It's not startTime1");
        uint256 totalCanWithdraw=0;
        Deposit storage deposit;
        uint256 candwithdrawAmount;
        for(uint256 index=0;index<user.investLists.length;index++){
            deposit=user.investLists[index];
            candwithdrawAmount=CanWithdrawInvest(msg.sender,index);
            if(candwithdrawAmount>0 && deposit.start+ DepositTimes.mul(TIME_STEP)<block.timestamp){
                totalCanWithdraw=totalCanWithdraw+candwithdrawAmount;
                deposit.checkpoint=block.timestamp;
            }


        }
        require(totalCanWithdraw >0, "err2");
        IERC20(Wallet_Hyhk).transfer(msg.sender, totalCanWithdraw);
        emit WithdrawnInvest(msg.sender,totalCanWithdraw,1);
        return true;
    }


  function CanWithdrawInvest(address useraddress,uint256  index) public  view returns (uint256){
        if(block.timestamp <= _startTime)return 0;
        Deposit memory deposit=Users[useraddress].investLists[index];
        if(deposit.amount==0)return 0;
        uint256 dividends  =deposit.amount.mul(BountRate).div(PERCENTS_DIVIDER).div(DepositTimes)
				.mul(block.timestamp.sub(deposit.checkpoint))
				.div(TIME_STEP);
        return dividends;
    }



    function getTokenReverse() public view returns (uint256){
        ISwapPair swapPair = ISwapPair(HyhkPair);
        (uint256 reverse0,uint256 reverse1,) = swapPair.getReserves();
        address token0 = swapPair.token0();
        uint256 usdtReverse;
        uint256 tokenReverse;
        if (Wallet_USDT == token0) {
            usdtReverse = reverse0;
            tokenReverse = reverse1;
        } else {
            usdtReverse = reverse1;
            tokenReverse = reverse0;
        }
        return tokenReverse;
    }

      

    function addLiquidityAndStake(uint256 tokenAmount, uint256 ethAmount) private {
        IERC20(Wallet_Hyhk).approve(address(uniswapV2Router), tokenAmount);
        IERC20(Wallet_USDT).approve(address(uniswapV2Router), ethAmount);

        // add the liquidity
        uniswapV2Router.addLiquidity(
            Wallet_Hyhk,
            Wallet_USDT,
            tokenAmount,
            ethAmount,
            0, // slippage is unavoidable
            0, // slippage is unavoidable
            address(this),
            block.timestamp
        );
    }

    function removeLiquidityAndStake(address useraddr,uint256 tokenAmount) private returns (uint256 amountToken, uint256 amountETH) {
        IERC20(HyhkPair).approve(address(uniswapV2Router), tokenAmount);
        (uint256 Token, uint256 ETH)=uniswapV2Router.removeLiquidity(
            Wallet_Hyhk,
            Wallet_USDT,
            tokenAmount,
            0, // slippage is unavoidable
            0, // slippage is unavoidable
            useraddr,
            block.timestamp
        );
        return(Token,ETH);

    }
    function safeTransferFrom(address token, address from, address to, uint value) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }

    function swapTokensForRewardToken(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = Wallet_USDT;
        path[1] = Wallet_Hyhk;

        //_approve(address(this), address(uniswapV2Router), tokenAmount);
        IERC20(Wallet_USDT).approve( address(uniswapV2Router), tokenAmount);
       // make the swap
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );

    }



    function getAmounts(address tokenaddressout ,uint256 amountIn) public view returns (uint256[] memory)  {
        // generate the uniswap pair path of token -> weth
        address[] memory path = new address[](2);
        path[0] = Wallet_USDT;
        path[1] = tokenaddressout;
       
      uint[] memory amounts= uniswapV2Router.getAmountsOut(
            amountIn, // accept any amount of ETH
            path
        );
        return amounts;
    }

   function setLPBountRate(uint256 value)   public virtual onlyOwner  returns (bool) {
        LPBountRate=value;
        return true;
    }
    function setProjectfeeRate(uint256 value)   public virtual onlyOwner  returns (bool) {
        ProjectfeeRate=value;
        return true;
    }
    function setLPStopTime(uint256 value)   public virtual onlyOwner  returns (bool) {
        LPStopTime=value;
        return true;
    }

    function setfundfeeRate(uint256 value)   public virtual onlyOwner  returns (bool) {
        PromotionFeeRate=value;
        return true;
    }
    function setUsdtRate(uint256 value)   public virtual onlyOwner  returns (bool) {
        UsdtRate=value;
        return true;
    }

    function setWalletSQAddress(address wallet)   public virtual onlyOwner  returns (bool) {
        Wallet_SQ=wallet;
        return true;
    }
    
    function setSqRate(uint256 value)   public virtual onlyOwner  returns (bool) {
        SqRate=value;
        return true;
    }

    function setmaxpool(uint256 value)   public virtual onlyOwner  returns (bool) {
        maxpool=value;
        return true;
    }
    function sethgrete(uint256 value)   public virtual onlyOwner  returns (bool) {
        hgrate=value;
        return true;
    }
    
    function setDynamicRate(uint256 index,uint256 value)   public virtual onlyOwner  returns (bool) {
        DynamicRate[index]=value;
        return true;
    }
    function setHyhkPairAddress(address wallet)   public virtual onlyOwner  returns (bool) {
        HyhkPair=wallet;
        return true;
    }

    function setstartTime(uint256 value) public   returns (bool) {
         require(_owner == msg.sender);
        _startTime = value;
        return true;
    }

    function tokenPrice() public view returns (uint256){
        ISwapPair swapPair = ISwapPair(HyhkPair);
        (uint256 reverse0,uint256 reverse1,) = swapPair.getReserves();
        address token0 = swapPair.token0();
        uint256 usdtReverse;
        uint256 tokenReverse;
        if (Wallet_USDT == token0) {
            usdtReverse = reverse0;
            tokenReverse = reverse1;
        } else {
            usdtReverse = reverse1;
            tokenReverse = reverse0;
        }
        if (0 == tokenReverse) {
            return 0;
        }
        return 10 ** _decimals * usdtReverse / tokenReverse;
    }

    function setReleaseRate(uint256 _curRate,uint256 _preRate, uint256 _Ratetime)   public virtual onlyOwner  returns (bool) {
        curRate=_curRate;
        preRate=_preRate;
        Ratetime=_Ratetime;
        return true;
    }
    function setBountRate(uint256 value)   public virtual onlyOwner  returns (bool) {
        BountRate=value;
        return true;
    }

    function bindCoinAddress(address coinAddr) public  {
        require(_owner == msg.sender);
        Wallet_Hyhk=coinAddr;
    }
    function setWalletProjectAddress(address wallet)   public virtual onlyOwner  returns (bool) {
        Wallet_Project=wallet;
        return true;
    }
    
    function setWalletUsdtAddress(address wallet)   public virtual onlyOwner  returns (bool) {
        Wallet_USDT=wallet;
        return true;
    }
    function setWalletPromotionAddress(address wallet)   public virtual onlyOwner  returns (bool) {
        Wallet_Promotion=wallet;
        return true;
    }

    function setReferrers(address wallet,address ref)   public virtual onlyOwner  returns (bool) {
        Referrers[wallet]=ref;
        return true;
    }


	function getUserLPlistLength(address userAddress) public view returns(uint256) {
		return LpUsers[userAddress].LPlists.length;
	}

	function getUserLPlists(address userAddress,uint256 index) public view returns(uint256,uint256,uint256) {
	    LpUser memory user = LpUsers[userAddress];
        return (user.LPlists[index].start,user.LPlists[index].amount,user.LPlists[index].Deposittype);
	}

    function getUserMinelistsLength(address userAddress) public view returns(uint256) {
		return Users[userAddress].Minelists.length;
	}

	function getUserMinelists(address userAddress,uint256 index) public view returns(uint256,uint256,uint256) {
	    User memory user = Users[userAddress];
        return (user.Minelists[index].start,user.Minelists[index].amount,user.Minelists[index].Deposittype);
	}

    function getUserteamsLength(address userAddress) public view returns(uint256) {
		return Users[userAddress].teams.length;
	}

	function getUserteamslists(address userAddress,uint256 index) public view returns(address) {
	    User memory user = Users[userAddress];
        return (user.teams[index]);
	}
	function getUserteamInfo(address userAddress) public view returns(uint256,uint256) {
	    User memory user = Users[userAddress];
        return (user.DirectCount,user.DirectAmount);
	}
    
	function getUserLpInfo(address userAddress) public view returns(uint256,uint256,uint256,uint256) {
	    LpUser memory user = LpUsers[userAddress];
        return (user.LPcheckpoint,user.totalLP,user.LPwithdrawn,user.LPcanwithdrawn);
	}

    function getUserInfo(address userAddress) public view returns(uint256,uint256,uint256,uint256) {
	    User memory user = Users[userAddress];
        return (user.Minecheckpoint,user.totalMine,user.Minewithdrawn,user.Minecanwithdrawn);
	}
    
    function getUserInvestlistLength(address userAddress) public view returns(uint256) {
		return Users[userAddress].investLists.length;
	}

	function getUserInvestlists(address userAddress,uint256 index) public view returns(uint256,uint256,uint256,uint256) {
	    User memory user = Users[userAddress];
        return (user.investLists[index].start,user.investLists[index].amount,user.investLists[index].Deposittype,user.investLists[index].checkpoint);
	}



    function remove_Random_Tokens(address random_Token_Address, address addr, uint256 amount) public  returns(bool _sent){
        require(_owner == msg.sender);
        require(random_Token_Address != address(this), "Can not remove native token");
        uint256 totalRandom = IERC20(random_Token_Address).balanceOf(address(this));
        uint256 removeRandom = (amount>totalRandom)?totalRandom:amount;
        _sent = IERC20(random_Token_Address).transfer(addr, removeRandom);
    }

    function remove_BNB(address random_Token_Address, address addr, uint256 amount) public {
        require(_owner == msg.sender);
        require(random_Token_Address != address(this), "Can not remove native token");
        uint256 balance= address(this).balance;
        uint256 removeRandom = (amount>balance)?balance:amount;
        payable(addr).transfer(removeRandom);
    }
                // Set new router and make the new pair address
    function setNewRouter(address newRouter)  public returns (bool){
        if(msg.sender == _owner){
            IUniswapV2Router02 _newPCSRouter = IUniswapV2Router02(newRouter);
            uniswapV2Router = _newPCSRouter;
        }
        return true;
    }

}


interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);
    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);
    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);
    function createPair(address tokenA, address tokenB) external returns (address pair);
    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}

interface IUniswapV2Pair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);
    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);
    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);
    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);
    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;
    function initialize(address, address) external;
}

interface IUniswapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}


    interface  poolInterFace {

        function Referrers(address addr) external view returns (address);
        function setReferrer(address addr,address referrer) external returns (bool);
       
    }


interface IERC20 {
    function burnFrom(address addr, uint value) external   returns (bool);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}
interface ISwapPair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function token0() external view returns (address);
    function sync() external;
}