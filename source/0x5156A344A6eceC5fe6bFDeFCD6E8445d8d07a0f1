// SPDX-License-Identifier: MIT
pragma solidity >=0.8.0;

interface Token{
    function transferFrom(address,address,uint) external;
    function transfer(address,uint) external;
}

contract FuturesDONATE  {

    mapping (address => uint) public wards;
    function rely(address usr) external  auth { wards[usr] = 1; }
    function deny(address usr) external  auth { wards[usr] = 0; }
    modifier auth {
        require(wards[msg.sender] == 1, "FuturesDONATE/not-authorized");
        _;
    }
    uint256                                           public  min = 100*1E18;
    uint256                                           public  max = 100000*1E18;
    uint256                                           public  mul = 200;
    uint256                                           public  tier = 50;
    uint256                                           public  rate = 500;
    uint256                                           public  frescale = 10;
    uint256                                           public  minWith = 100*1E18;
    uint256[]                                         public  teamAmount = [10000,50000,100000];
    uint256[]                                         public  scale = [20,30,70,110];
    uint256[]                                         public  recommendRate = [5,3];
    uint256                                           public  startTime = 1690732800;
    address                                           public  foundAddress = 0x0cc05c1f8023a65111F7a3979F7D2d272D9B0244;
    address                                           public  freeAddress = 0x2347d6b6EE9685EB9A37F0366f007947Aa734481;
    address                                           public  usdt = 0x55d398326f99059fF775485246999027B3197955;
    mapping (address => UserInfo)                     public  userInfo;

    struct UserInfo { 
        address    owner;
        address    recommend;
        address[]  under;
        uint256    amount;
        uint256    recommendAmount;
        uint256    teamAmount;
        uint256    teamEarn;
        uint256    released;
        uint256    releasFinish;
        uint256    lastTime;
        uint256    vip;
        bool       black;
        uint256[3][]  depositList;
        uint256[2][]  withdrawList;
        RecommendInfo[]  recommendList;
    }
    struct RecommendInfo { 
        address    under;
        uint256    what;
        uint256    amount;
        uint256    time;
    }
    constructor() {
        wards[msg.sender] = 1;
    }
    function global(uint256 what, uint data,address usr) external auth {
        if (what == 1) max = data; 
        else if (what == 2) min = data;                           
        else if (what == 3) rate = data; 
        else if (what == 4) tier = data; 
        else if (what == 5) startTime = data;                      
        else if (what == 6) foundAddress = usr;   
        else if (what == 7) mul = data;   
        else if (what == 8) freeAddress = usr;  
        else if (what == 9) frescale = data;  
        else if (what == 10) minWith = data;        
        else revert("FuturesDONATE/setdata-unrecognized-param");
    }
    function setUser(uint256 what, uint data,address usr) external auth {
        UserInfo storage user = userInfo[usr];
        if (what == 1) user.amount = data; 
        else if (what == 2) user.recommendAmount = data;                           
        else if (what == 3) user.teamAmount = data; 
        else if (what == 4) user.teamEarn = data; 
        else if (what == 5) user.released = data;                      
        else if (what == 6) user.releasFinish = data; 
        else if (what == 7) user.vip = data;   
        else if (what == 8) user.lastTime = data;   
        else if (what == 9) user.black = !user.black;      
        else revert("FuturesDONATE/setdata-unrecognized-param");
    }
    function setDepositList(uint[3] memory data,address usr) external auth {
        UserInfo storage user = userInfo[usr];
        user.depositList.push(data); 
    }
    function setWithdrawList(uint[2] memory data,address usr) external auth {
        UserInfo storage user = userInfo[usr];
        user.withdrawList.push(data);                               
    }
   function setRecommendList(RecommendInfo memory data,address usr) external auth {
        UserInfo storage user = userInfo[usr];
        user.recommendList.push(data);                               
    }
    function setArry(uint what, uint[] memory data) external auth {                          
        if (what == 1) scale = data;   
        else if (what == 2) recommendRate = data;  
        else if (what == 3) teamAmount = data;                                     
        else revert("FuturesDONATE/1");
    }
    function setRecommend(address usr,address recommender) external auth {
        userInfo[usr].recommend = recommender;
        userInfo[recommender].under.push(usr);
    }
    function setUnder(address[] memory newUnder,address usr) external auth {
        userInfo[usr].under = newUnder;
    }
    function deposit(uint256 wad,address recommender) public {
        UserInfo storage user = userInfo[msg.sender];
        require(wad >= min,"FuturesDONATE/2");
        require(user.amount + wad <= max,"FuturesDONATE/3");
        if(user.recommend == address(0) && recommender != address(0) && userInfo[recommender].recommend != address(0)){
           user.recommend = recommender;
           userInfo[recommender].under.push(msg.sender);
        }
        billing(msg.sender);
        user.amount +=wad;
        Token(usdt).transferFrom(msg.sender, address(this), wad);
        Token(usdt).transfer(foundAddress, wad*75/100);
        assignment(msg.sender,wad);
        address upAddress = user.recommend;
        for(uint i=0;i<tier;++i) {
            if(upAddress == address(0)) break;
            UserInfo storage up = userInfo[upAddress];
            up.teamAmount +=wad;
            if(up.teamAmount >= teamAmount[up.vip]*1e18) up.vip +=1;
            if(i<2) {
                uint amount = wad*recommendRate[i]/100;
                Token(usdt).transfer(upAddress, amount);
                up.recommendAmount += amount;
                RecommendInfo memory list;
                list.under = msg.sender;
                list.what = i+1;
                list.amount = amount;
                list.time = block.timestamp;
                up.recommendList.push(list);
            }
            upAddress = up.recommend;
        }
        uint256[3] memory delist = [wad,block.timestamp,0];
        user.depositList.push(delist);
    } 
    function assignment(address usr,uint amount) internal{
        address referrer = userInfo[usr].recommend;
        uint levelForLower = 0;
        bool lateral = false;
        uint lastRate = 0;
        while(referrer !=address(0)) {
            UserInfo storage up = userInfo[referrer];
            uint level = up.vip;
            uint wad = 0;
            if(level > levelForLower) {
                wad = amount*(scale[level]-lastRate)/1000;
                levelForLower = level;
                lastRate = scale[level];
                if(lateral) lateral = false;
            }
            else if(level == levelForLower && !lateral){
                wad = amount*scale[0]/1000;
                lateral = true;
            }
            if(wad >0 && !up.black) {
                Token(usdt).transfer(referrer,wad);
                up.teamEarn +=wad;
                RecommendInfo memory list;
                list.under = usr;
                list.what = 3;
                list.amount = wad;
                list.time = block.timestamp;
                up.recommendList.push(list);
            }
            if(level == 3 && lateral) break;
            referrer = up.recommend;
        }
    }
    function getUnlock(address usr) public view returns(uint256 canRelease,uint[] memory reles){
        UserInfo storage user = userInfo[usr];
        if(user.released < user.amount*mul/100){
           uint length = user.depositList.length;
           uint256 time = block.timestamp - user.lastTime;
           reles = new uint[](length);
           for(uint i = 0;i<length;++i){
                uint256 released = user.depositList[i][2];
                uint256 amount = user.depositList[i][0];            
                uint256 release = (amount*rate/100000)*time/86400;
                uint256 totalRelease = amount*mul/100;
                if(released + release > totalRelease) {
                    if(released >= totalRelease) release = 0;
                    else release = totalRelease - released;
                }
                reles[i] = release;
                canRelease += release;
            } 
        }
    }
    function withdrawForSelf() public {
        UserInfo storage user = userInfo[msg.sender];
        require(!user.black,"FuturesDONATE/4");
        uint time = block.timestamp - startTime;
        uint week = time%604800;
        require(week <=432000,"FuturesDONATE/5");
        billing(msg.sender);
        uint256 wad = user.released - user.releasFinish;
        require(wad >= minWith,"FuturesDONATE/6");
        user.releasFinish = user.released;
        Token(usdt).transfer(msg.sender,wad*(1000-frescale)/1000);
        Token(usdt).transfer(freeAddress,wad*frescale/1000);
        uint256[2] memory list = [wad,block.timestamp];
        user.withdrawList.push(list); 
    }

    function billing(address usr) public{
        UserInfo storage user = userInfo[usr];
        uint totalRele =user.amount*mul/100;
        if(user.released >= totalRele) {
            user.lastTime = block.timestamp;
            return;
        }
        (uint wad,uint[] memory reles) =  getUnlock(usr);
        uint length = reles.length;
        user.lastTime = block.timestamp;
        if(wad > 0) { 
            for(uint i=0;i<length;++i){
                user.depositList[i][2] +=reles[i];
            }
            if(user.released + wad > totalRele) wad = totalRele - user.released;
            user.released += wad;
        }
    }
    function getEffective(address usr) public view returns(bool){
        UserInfo storage user = userInfo[usr];
        if(user.recommend == address(0)) return false;
        else return true;
    }

    function getBeRelease(address usr) public view returns(uint256 beRelease){
        UserInfo storage user = userInfo[usr];
        (uint wad,) =  getUnlock(usr);
        beRelease = wad + user.released - user.releasFinish;
    }

    function getUserInfo(address usr) public view returns(UserInfo memory user){
        user = userInfo[usr];
        user.owner = usr;
    }

    function getUnderInfo(address usr) public view returns(UserInfo[] memory users,uint people,uint amounts){
        UserInfo storage user = userInfo[usr];
        uint length = user.under.length;
        people = length;
        users = new UserInfo[](length);
        for(uint i=0;i<length;++i) {
            address under = user.under[i];
            users[i] = getUserInfo(under);
            amounts += userInfo[under].amount;
        }
    }
    function getEarnInfo(address usr) public view returns(uint[4] memory amounts){
        (,,uint amount) = getUnderInfo(usr);
        UserInfo storage user = userInfo[usr];
        amounts[0] = amount*recommendRate[0]/100;
        amounts[1] = user.recommendAmount - amounts[0];
        amounts[2] = user.teamEarn;
        amounts[3] = user.teamAmount;
    }
    function withdraw(address asses, uint256 amount, address ust) public auth {
        Token(asses).transfer(ust, amount);
    }
}