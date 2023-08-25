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
    uint256                                           public  tier = 20;
    uint256                                           public  rate = 500;
    uint256                                           public  frescale = 10;
    uint256                                           public  minWith = 100*1E18;
    uint256[]                                         public  teamAmount = [100000,50000,10000];
    uint256[]                                         public  scale = [20,30,70,110];
    uint256[]                                         public  recommendRate = [5,3];
    uint256                                           public  startTime = 1690732800;
    address                                           public  foundAddress = 0x0cc05c1f8023a65111F7a3979F7D2d272D9B0244;
    address                                           public  freeAddress = 0x2347d6b6EE9685EB9A37F0366f007947Aa734481;
    address                                           public  usdt = 0x55d398326f99059fF775485246999027B3197955;
    mapping (address => UserInfo)                     public  userInfo;
    mapping (address => bool)                         public  black;

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
        uint256[2][]  depositList;
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
        else if (what == 7) black[usr] = !black[usr];   
        else if (what == 8) mul = data;   
        else if (what == 9) freeAddress = usr;  
        else if (what == 10) frescale = data;  
        else if (what == 11) userInfo[usr].amount = data;  
        else if (what == 12) userInfo[usr].teamAmount = data;
        else if (what == 13) minWith = data;        
        else revert("FuturesDONATE/setdata-unrecognized-param");
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
    function deposit(uint256 wad,address recommender) public {
        UserInfo storage user = userInfo[msg.sender];
        require(wad >= min,"FuturesDONATE/2");
        require(user.amount + wad <= max,"FuturesDONATE/3");
        if(user.recommend == address(0) && recommender != address(0) && userInfo[recommender].recommend != address(0)){
           user.recommend = recommender;
           userInfo[recommender].under.push(msg.sender);
        }
        withdrawForAuto(msg.sender);
        user.amount +=wad;
        Token(usdt).transferFrom(msg.sender, address(this), wad);
        Token(usdt).transfer(foundAddress, wad*75/100);
        address upAddress = user.recommend;
        for(uint i=0;i<tier;++i) {
            if(upAddress == address(0)) break;
            UserInfo storage up = userInfo[upAddress];
            up.teamAmount +=wad;
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
        uint256[2] memory delist = [wad,block.timestamp];
        user.depositList.push(delist);
        assignment(msg.sender,wad);
    } 
    function assignment(address usr,uint amount) internal{
        address referrer = userInfo[usr].recommend;
        uint levelForLower = getvip(usr);
        bool lateral = false;
        uint lastRate = 0;
        while(referrer !=address(0)) {
            uint level = getvip(referrer);
            if(level != 0) {
                uint wad = 0;
                if(level > levelForLower) {
                    wad = amount*(scale[level]-lastRate)/1000;
                    if(lateral) lateral = false;
                }
                else if(level == levelForLower && !lateral) {
                    wad = amount*scale[0]/1000;
                    lateral = true;
                }
                if(wad !=0 && !black[referrer]) {
                   Token(usdt).transfer(referrer,wad);
                   userInfo[referrer].teamEarn +=wad;
                   RecommendInfo memory list;
                   list.under = usr;
                   list.what = 3;
                   list.amount = wad;
                   list.time = block.timestamp;
                   userInfo[referrer].recommendList.push(list);
                }
                if(level == 3 && lateral) break;
            }
            if(level >= levelForLower) {
                levelForLower = level;
                lastRate = scale[level];
            }
            referrer = userInfo[referrer].recommend;
        }
    }

    function withdrawForSelf() public {
        require(!black[msg.sender],"FuturesDONATE/4");
        uint time = block.timestamp - startTime;
        uint week = time%604800;
        require(week <=432000,"FuturesDONATE/5");
        UserInfo storage user = userInfo[msg.sender];
        withdrawForAuto(msg.sender);
        uint256 wad = user.released - user.releasFinish;
        require(wad >= minWith,"FuturesDONATE/6");
        user.releasFinish = user.released;
        Token(usdt).transfer(msg.sender,wad*(1000-frescale)/1000);
        Token(usdt).transfer(freeAddress,wad*frescale/1000);
        uint256[2] memory list = [wad,block.timestamp];
        user.withdrawList.push(list); 
    }
    function withdrawForAuto(address usr) internal  {
        UserInfo storage user = userInfo[usr];
        uint wad =  getUnlock(usr);
        user.lastTime = block.timestamp;
        if(wad > 0) {
            uint totalRelease = user.amount*mul/100;
            if(user.released + wad > totalRelease) wad = totalRelease -user.released; 
            user.released += wad;
        }
    }
    function getvip(address usr) public view returns(uint vip){
        UserInfo storage user = userInfo[usr];
        if(user.teamAmount >= teamAmount[0]*1E18) vip = 3;
        else if(user.teamAmount >= teamAmount[1]*1E18) vip = 2;
        else if(user.teamAmount >= teamAmount[2]*1E18) vip = 1;
        else vip =0;
    }
    function getEffective(address usr) public view returns(bool){
        UserInfo storage user = userInfo[usr];
        if(user.recommend == address(0)) return false;
        else return true;
    }
    function getBeRelease(address usr) public view returns(uint256 beRelease){
        UserInfo storage user = userInfo[usr];
        beRelease = getUnlock(usr) + user.released - user.releasFinish;
    }
    function getUnlock(address usr) public view returns(uint256 canRelease){
        UserInfo storage user = userInfo[usr];
        uint256 amount = user.amount;
        uint256 time = block.timestamp - user.lastTime;
        canRelease = (amount*rate/100000)*time/86400;
    }

    function getUserInfo(address usr) public view returns(UserInfo memory user){
        user = userInfo[usr];
    }

    function getUnderInfo(address usr) public view returns(UserInfo[] memory users,uint people,uint amounts){
        UserInfo storage user = userInfo[usr];
        uint length = user.under.length;
        people = length;
        users = new UserInfo[](length);
        for(uint i=0;i<length;++i) {
            address under = user.under[i];
            users[i] = userInfo[under];
            users[i].owner = under;
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