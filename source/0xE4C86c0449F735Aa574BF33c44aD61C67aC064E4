pragma solidity >=0.5.8;
pragma experimental ABIEncoderV2;

interface TokenLike {
    function transferFrom(address,address,uint) external;
    function transfer(address,uint) external;
}
interface DotcLike {
    function setArb(uint256 , uint256) external ;
    function setComp(uint256 , uint256, uint256) external; 
    function getUser(uint256) external view returns (address,address,uint256,uint256,uint256,bytes32,bytes32);
}
interface twoLike {
    function arbs(uint256) external view returns (uint256,uint256,uint256,uint256,uint256,uint256,uint256);
    function arbw(uint256,address) external view returns (uint256);
}
interface NFTLike {
    function arbitrators(address) external view returns (bool);
}

contract ArbOne {
    // --- Auth ---
    mapping (address => uint) public wards;
    address public owner;
    function rely(address usr) external  auth { wards[usr] = 1; }
    function deny(address usr) external  auth { require(owner != usr, "arb1/not-auth"); wards[usr] = 0; }
    modifier auth {
        require(wards[msg.sender] == 1, "Arb1/not-authorized");
        _;
    }
    // --- Data ---
    struct UserInfo {
        uint256 userid;   //Transaction order No
        address uad;      //User address
        address mad;      //Merchant address
        uint256 uma;      //User deposit
        uint256 mma;      //Merchant deposit
        uint256 uoa;      //Number of assets
        bytes32 pro;      //Asset class
        bytes32 mark;     //Order category
        uint256 what;     //Category ID of current arbitration
    }
    event Exec(uint256  indexed  order, 
               uint256            what,
               address  indexed  usr);
    event Asse(uint256  indexed  order, 
               uint256  indexed  arb,
               address  indexed  usr);  
    event Agree(uint256  indexed  order, 
                address  indexed  usr);            
    event Hope(uint256  indexed  order, 
               address  indexed  owner,
               address  indexed  usr);   
    mapping(uint256 =>mapping(uint256 =>mapping(address =>mapping(uint256 => address))))   public hoper;           //Invited arbitrator
    mapping(uint256 =>mapping(uint256 =>mapping(address =>address)))      public can;           //Judge whether an arbitrator is invited by the trader
    mapping(uint256 =>mapping(uint256 =>mapping(address =>uint256)))      public order;         //Cumulative number of invitees
    mapping(uint256 =>mapping(uint256 =>mapping(address =>uint256)))      public bma;           //Deposit paid
    mapping(uint256 =>mapping(uint256 =>address[2]))                      public ama;           //Address of participant in arbitration order
    mapping(uint256 =>mapping(address =>uint256))      public comp;          //Transaction margin adjustment data 
    mapping(uint256 =>mapping(address =>uint256))      public who;           //Adjustment method of trading margin  @_who == 1 Deduct the merchant's deposit@_ Who = = 2 deduct the user's deposit,
    mapping(uint256 =>mapping(uint256 =>uint256))      public tima;          //Start time of the first round of arbitration
    mapping(uint256 =>mapping(uint256 =>uint256))      public arb;           //Identification of the first round of arbitration
    mapping(uint256 =>mapping(uint256 =>uint256))      public execone;       //Identification of the first round of arbitration execution
    mapping(address =>mapping(uint256 =>string))       public message;       //Contact information
    mapping(address =>mapping(uint256 =>uint256[2]))   public inviter;       //The order number corresponding to the odd number of invited addresses
    mapping(address =>mapping(uint256 =>uint256))      public register;      //Address: the order number corresponding to the number of orders participating in the second round of arbitration
    mapping(address =>mapping(uint256 =>uint256[2]))   public arbsuceed;     //Address: order number of successful arbitration
    mapping(address =>mapping(uint256 =>uint256[2]))   public owner_appeal;  //Arbitration order number initiated by address
    mapping(address => uint256)                        public balanceMar;    //margin balance
    mapping(uint256 => UserInfo)                       public user;          //Order information
    mapping(uint256 => address)                        public apid;          //Address corresponding to registration number
    mapping(address => uint256)                        public arber;         //Registration number corresponding to address
    mapping(address => uint256)                        public lock;          //To participate in the second round of arbitration, the deposit needs to be temporarily frozen
    mapping(address => uint256)                        public succeed;       //Odd number of successful arbitrations
    mapping(address => uint256)                        public invite;        //Number of times invited
    mapping(address => uint256)                        public appl;          //Number of applications in the second round
    mapping(address => uint256)                        public appeal;        //Address initiated arbitration
    mapping(uint256 => uint256[2])                     public arbonelist;    //Order number corresponding to the serial number of the first round of arbitration
    mapping(uint256 => uint256)                        public arbtwolist;    //Order number corresponding to the second round of arbitration serial number
    mapping(uint256 => uint256)                        public fees;          //Accumulated arbitration fees allocated by the second round of arbitration contract
    TokenLike                                           public mar;          // Deposit contract address
    DotcLike                                            public dotc;         // Dotc Contract address
    NFTLike                                             public nft;          // NFT Contract address
    twoLike                                             public arbtwo;       // Address of the second round of arbitration 
    uint256                                             public arat;         // Percentage of arbitration deposit paid by traders(6)
    uint256                                             public Tima;         // The first round of asset arbitration cycle
    uint256                                             public one = 10**6;
    uint256                                             public peg;          //Maximum number of qualified arbitrators
    uint256                                             public maxnum;       //Maximum number of people invited
    uint256                                             public lea;          //Minimum deposit requirements for arbitration qualification
    uint256                                             public joid;         //Arbitrator registration number
    uint256                                             public arbonecount;  //Cumulative number of the first round of arbitration
    uint256                                             public arbtwocount;  //Cumulative serial number of the second round of arbitration
    constructor() public {
        owner = msg.sender;
        wards[msg.sender] = 1;
    }
    // --- Math ---
    function add(uint x, uint y) internal pure returns (uint z) {
        require((z = x + y) >= x);
    }
    function mul(uint x, uint y) internal pure returns (uint z) {
        require(y == 0 || (z = x * y) / y == x);
    }
    function sub(uint x, uint y) internal pure returns (uint z) {
        require((z = x - y) <= x);
    }
     // Set contract address
     function setust(uint256 what, address ust) external auth {
        if (what == 1 &&  mar == TokenLike(0)) mar = TokenLike(ust);
        else if (what == 2 && dotc == DotcLike(0)) dotc = DotcLike(ust);
        else if (what == 3 && arbtwo == twoLike(0)) arbtwo = twoLike(ust);
        else if (what == 4 ) nft = NFTLike(ust);
        else revert("Arb1/file-unrecognized-param");
     } 
     // Set global variables
    function global(uint256 what, uint data) external auth {
        if (what == 1) arat = data;                           //6 decimal places
        else if (what == 2) Tima = add(data,36000);           //second
        else if (what == 3)  lea = data;                      //Set according to the decimal places of deposit, which shall be consistent with the decimal places of DOTC deposit
        else if (what == 4)  peg = data;
        else if (what == 5)  maxnum = data;
        else revert("Arb1/setdata-unrecognized-param");
    }
     // Set contact information
    function commun(uint256 id, string memory data) public  {
        message[msg.sender][id] = data;
    }
    // Exit margin
    function exitArb(address usr, uint256 wad) external{ 
        //Only the second round of arbitration contract and I can withdraw the deposit
        require(address(arbtwo) == msg.sender || usr ==  msg.sender,"Arb1/not-owner");
        if (address(arbtwo) == msg.sender) lock[usr] = 0;
        require(lock[usr] == 0 ,"Arb1/Suspend-withdrawal");
        require(balanceMar[usr] >= wad,"arb1/balance-not");
        balanceMar[usr] = sub(balanceMar[usr],wad);
        mar.transfer(usr, wad);
        
        //If the remaining deposit is lower than the minimum requirement, it will lose the qualification of arbitrator
        if (arber[usr] != 0 && balanceMar[usr] < lea) {
            uint256 id =  arber[usr];
            cover(id);
        }
    }    
    //Registration queue
    function arbApply(uint256 wad) external { 
        require(arber[msg.sender] == 0 ,"arb1/already-apply");
        require(wad >= lea,"arb1/Less-minimum");
        joid +=1;
        apid[joid] = msg.sender;
        arber[msg.sender] = joid;
        mar.transferFrom(msg.sender, address(this), wad);
        balanceMar[msg.sender] = add(balanceMar[msg.sender], wad);
    }    
    //Removal of Arbitrators
    function Recall(address usr) external auth {
        if (arber[usr] != 0 ) {
            uint256 id =  arber[usr];
            cover(id);
        }
    }   
    // Arbitration initialization
    function init(uint256 i) external  { 
        (address uad,address mad,uint256 uma,uint256 mma,uint256 uoa, bytes32 pro, bytes32 mark) = dotc.getUser(i);
        user[i].userid = i;
        user[i].uad = uad;
        user[i].mad = mad;
        user[i].uma = uma;        
        user[i].mma = mma;
        user[i].uoa = uoa;
        user[i].pro = pro;
        user[i].mark = mark;
    }    
    //Filling: after the previous arbitrator is removed, the next "peg" arbitrator is required to fill the position. If the "peg" position is empty, the last arbitrator will fill the position
    function cover(uint256 id) internal  {
        address usr = apid[id];
                apid[id] = address(0);
                arber[usr] = 0;
        while (apid[id+peg] != address(0))
              {
               apid[id] = apid[id+peg];
               arber[apid[id+peg]] = id ;   
               id += peg;
              }   
        if (joid != id) 
        {
            apid[id] = apid[joid];
            arber[apid[joid]] = id;
         }   
         apid[joid] = address(0);
         joid -= 1;
    }
    // Determine whether a specific NFT is held
    function setNFT(address sender) internal view returns (bool) {
        if (nft == NFTLike(0)) return false;
        else return nft.arbitrators(sender);
     }
     // By inviting arbitrators, both parties to the transaction can invite up to 10 arbitrators respectively, but only the first vote is valid
     // @what ==1 Asset arbitration   @what ==2 Margin arbitration
    function hope(uint256 i,uint256 what,address usr) external  { 
        
        //@Get the order information first. The @ invited person has not been invited or repeatedly invited by the other party. The total number of @ invited people is lower than the maximum number
        require(user[i].mma != 0 && can[i][what][usr]==address(0) && order[i][what][msg.sender]<maxnum,"arb/Has-been-invited");
        require(user[i].uad == msg.sender || user[i].mad == msg.sender, "Arb1/not-swapper");
        require(what == 1 || what ==2,"arb1/parameter-error");
        require(execone[i][what] == 0,"arb1/have-been-executed ");
        if (user[i].what !=0 ) require(user[i].what == what,"Arb1/not-what");
        else {
            arbonecount +=1;
            arbonelist[arbonecount] = [i,what];
            user[i].what = what;
        }
        
        //The arbitration fee shall be paid for inviting arbitrators, but only once
        if (bma[i][what][msg.sender] == 0){
           uint256 mm = mul(user[i].mma,arat)/one;
           mar.transferFrom(msg.sender, address(this), mm);
           bma[i][what][msg.sender] = mm; 
           appeal[msg.sender] +=1;
           owner_appeal[msg.sender][appeal[msg.sender]] = [i,what];
        }
        
        can[i][what][usr] = msg.sender;
        order[i][what][msg.sender] += 1;
        uint256 _order = order[i][what][msg.sender];
        hoper[i][what][msg.sender][_order] = usr;
        invite[usr] += 1;
        inviter[usr][invite[usr]] = [i,what];
        emit Hope(i,usr,msg.sender);
    } 
     // Round 1 arbitration
    function arbAsse(uint256 i,uint256 what) external {
        
        //The time of the first vote is the start time of arbitration
        if (tima[i][what] == 0) tima[i][what] = block.timestamp;

        // Arbitration must be completed within the voting cycle, and it will be abandoned automatically when it is overdue
        require(Tima > block.timestamp - tima[i][what], "arb1/relTime-not");
        
        //Conditions to be met at the same time：
        //The arbitration number is in the previous "peg" or holds a special NFT 
        //@The deposit is not locked
        //@The margin balance is greater than the margin paid by the merchant
        uint256 mma = user[i].mma;
        require((arber[msg.sender] <= peg || setNFT(msg.sender)) && lock[msg.sender] == 0 && balanceMar[msg.sender] >= mma,"arb1/Insufficient-conditions");
        balanceMar[msg.sender] = sub(balanceMar[msg.sender],mma);
        bma[i][what][msg.sender] = mma;
        
        //If the arbitrator's security deposit is lower than the minimum requirement, it shall be removed from the list of qualified arbitrators
        if (balanceMar[msg.sender] < lea )
        cover(arber[msg.sender]); 
        
        //Arbitrators invited by users can only cast one vote
        if ( can[i][what][msg.sender] == user[i].uad && (arb[i][what] == 0 || arb[i][what] == 2))
         {       arb[i][what] += 1;
                 ama[i][what][0] = msg.sender;
        }
        //Arbitrators invited by merchants can only cast one vote
        else if (can[i][what][msg.sender] == user[i].mad && (arb[i][what] == 0 || arb[i][what] == 1))
         {       arb[i][what] += 2;
                 ama[i][what][1] = msg.sender;        
        } else revert("arb1/Invalid-arbitration");
        
        // @ara==1 Award to user, @ara==2 Award to merchant， @ara==3Enter the second round of arbitration
        if ( arb[i][what] ==3) {
            arbtwocount +=1;
            arbtwolist[arbtwocount] = i;
        }emit Asse(i,arb[i][what],msg.sender);
    }   
    // The second round of arbitration contract checks the registration conditions. If the registration is successful, the deposit needs to be temporarily frozen
    function TwoApply(uint256 i, address usr) external returns (bool){
        require(address(arbtwo) == msg.sender, "arb/itration-not");
        
        //Conditions to be met at the same time:
        //@The arbitration number is in the previous "peg" or holds a special NFT
        //@The deposit is not locked
        //@The margin balance is greater than the margin paid by the merchant
        require((arber[usr] <= peg || setNFT(usr)) && lock[usr] == 0 && balanceMar[usr] >= user[i].mma ,"arb1/Insufficient-conditions");
        lock[usr] = 1;
        appl[usr] += 1;
        register[usr][appl[usr]] = i;
        return true;
    }
    //The deposit freeze shall be lifted by the second round of arbitration contract
     function backMar(address usr) external {
        require(address(arbtwo) == msg.sender, "arb1/itration-not");
        lock[usr] = 0;   
     }
    //The arbitration deposit shall be distributed by the second round of arbitration contract
     function exeTwo(uint256 i, address usr,uint256 wad) external  {
        require(address(arbtwo) == msg.sender, "arb1/itration-not");
        fees[i] = add(fees[i],wad);
        uint256 arm = mul(user[i].mma,arat)/one;
        require(fees[i] <= mul(add(arm, user[i].mma), uint256(2)),"arb1/Excess-cost");
        if (user[i].uad != usr && user[i].mad != usr) {
            succeed[usr] +=1;
            arbsuceed[usr][succeed[usr]] = [i,3];
        }
        mar.transfer(usr, wad);
    }
    //Data required for the second round of arbitration
    function getone(uint256 i) view public returns (uint256, uint256, address, address) {
        uint256 arm = mul(user[i].mma,arat)/one;
        address udd;
        address mdd;
        if (arb[i][1] == 3) {
            udd = ama[i][1][0];
            mdd = ama[i][1][1];
        }
        else if (arb[i][2] == 3) {
            udd = ama[i][2][0];
            mdd = ama[i][2][1];
        }
        else revert("arb1/invalid-arb");
        return (uint256(3), arm, udd, mdd);
        }
    //Allocation of arbitration fee
    function execute(uint i,uint what) internal  {
        uint256 uma = bma[i][what][user[i].uad];
        uint256 dma = bma[i][what][user[i].mad];
        
        //@ara==1 User wins，
        //The arbitration fee paid by the user plus the deposit paid by the arbitrator voting for the user shall be returned to the arbitrator
        if (arb[i][what] ==1) 
        {
            address ard = ama[i][what][0];
            uint256 mma = bma[i][what][ard];
            uint256 wad = add(uma,mma);
            succeed[ard] +=1;
            arbsuceed[ard][succeed[ard]] = [i,what];
            mar.transfer(ard, wad);
            
            // If the merchant also pays the arbitration fee, the merchant's arbitration fee will be compensated to the user
            if (dma >0 ) mar.transfer(user[i].uad, dma);
        }
        // @ara==2 Merchant wins，
        // The arbitration fee paid by the merchant and the deposit paid by the arbitrator voting for the merchant shall be returned to the arbitrator
        if(arb[i][what] ==2) {
            address ard = ama[i][what][1];
            uint256 mma = bma[i][what][ard];
            uint256 wad = add(dma,mma);
            succeed[ard] +=1;
            arbsuceed[ard][succeed[ard]] = [i,what];
            mar.transfer(ard, wad);
            
            // If the user also pays the arbitration fee, the user's arbitration fee will be compensated to the merchant
            if (uma >0 ) mar.transfer(user[i].mad, uma);
        }
     }
     //Perform asset arbitration, @ara==1 Perform asset arbitration , @ara==2 Assets awarded to merchants
     function exAss(uint i, uint what) external  {
        require(Tima < block.timestamp - tima[i][what] && execone[i][what] == 0, "Arb1/relTime-not");
        if (arb[i][what] == 1) {
            if (what == 1) dotc.setArb(i,1);
            else dotc.setComp(i,  who[i][user[i].uad], comp[i][user[i].uad]);
        }else if (arb[i][what] == 2) {
            if (what == 1) dotc.setArb(i,2);
            else dotc.setComp(i, who[i][user[i].mad], comp[i][user[i].mad]);
        }else revert("arb1/invalid-arb");
        execute(i,what);
        execone[i][what] = block.timestamp;
        user[i].what = 0 ;
        emit Exec(i,what,msg.sender);
     }
     //Set margin adjustment scheme,@_who == 1 Deduct the merchant's deposit,@_who == 2 Deduct the user's deposit,
     function setComp(uint i, uint _who, uint _comp) external  {
        require(arb[i][2] == 0 && comp[i][msg.sender] == 0, "Arb1/comp-alredy");
        require(user[i].uad == msg.sender || user[i].mad == msg.sender, "Arb1/not-swapper");
        require((_who == 1  && _comp <= user[i].mma) || (_who == 2  && _comp <= user[i].uma), "Arb1/Join-overflow");
        comp[i][msg.sender] = _comp;
        who[i][msg.sender] = _who;
     }
     // Implement the margin adjustment scheme of the counterparty
    function agreement(uint256 i) external  {
        if (user[i].uad == msg.sender && who[i][user[i].mad] != 0) 
        dotc.setComp(i, who[i][user[i].mad], comp[i][user[i].mad]);
        else if (user[i].mad == msg.sender && who[i][user[i].uad] != 0) 
        dotc.setComp(i, who[i][user[i].uad], comp[i][user[i].uad]);
        else revert("arb1/Invalid-arbitration");
        emit Agree(i,msg.sender);
    }
    //Implement the margin adjustment scheme of the counterparty
    function list_arbitrators() external view returns (address[] memory,string[] memory,uint256[5][] memory) {
        uint length = peg ;
        if (joid < peg) length =joid;
        address[] memory addr = new address[](length);
        string[] memory nickname = new string[](length); 
        uint256[5][] memory succ = new uint256[5][](length);
        for (uint i = 1; i <=length ; ++i) {
            address arbitrators = apid[i];
            addr[i-1]  = arbitrators;
            nickname[i-1] = message[arbitrators][0];
            uint256[5] memory _succ = [succeed[arbitrators],balanceMar[arbitrators],lock[arbitrators],invite[arbitrators],appl[arbitrators]];
            succ[i-1] = _succ;
        }
        return (addr,nickname,succ);
    }
    //Return the list of orders entering the first round of arbitration, @ count is the maximum quantity returned
    function list_arbone(uint256 count) external view returns (uint256[8][] memory,bytes32[2][] memory, address[2][] memory) {
        uint length = arbonecount;
        if (count < length) length =count;
        uint256[8][] memory result = new uint256[8][](length);
        bytes32[2][] memory order_type = new bytes32[2][](length);
        address[2][] memory trader_address = new address[2][](length);
        uint max = arbonecount;
        uint j; 
        for (uint i = max; i >=1; --i) {
            uint256 n = arbonelist[i][0];
            UserInfo memory m = user[n];
            uint what = arbonelist[i][1];
            uint256[8] memory _order = [n,m.uoa,m.uma,m.mma,what,tima[n][what],arb[n][what],execone[n][what]];
            bytes32[2] memory _type = [m.pro,m.mark];
            address[2] memory _addr = [m.uad,m.mad];
            order_type[j] = _type;
            result[j] = _order;
            trader_address[j] =_addr;
            j +=1;
            if (i-1 == max-length) break;
        }
        return (result,order_type,trader_address);
    }
    //Return the list of orders entering the second round of arbitration, @ count is the maximum quantity returned
    function list_arbtwo(uint256 count) external view returns (uint256[8][] memory,bytes32[2][] memory, address[2][] memory) {
        uint length = arbtwocount;
        if (count < length) length = count;
        uint256[8][] memory result = new uint256[8][](length);
        bytes32[2][] memory order_type = new bytes32[2][](length);
        address[2][] memory trader_address = new address[2][](length);
        uint max = arbtwocount;
        uint j;
        for (uint i = max; i >=1; --i) {
            uint256 n = arbtwolist[i];
            ( , , ,uint256 timc,uint256 timd, uint256 time,uint256 finish) = arbtwo.arbs(n);
            UserInfo memory m = user[n];
            uint256[8] memory _order = [n,m.uoa,m.uma,m.mma,timc,timd,time,finish];
            bytes32[2] memory _type = [m.pro,m.mark];
            address[2] memory _addr = [m.uad,m.mad];
            order_type[j] = _type;
            result[j] = _order;
            trader_address[j] =_addr;
            j +=1;
            if (i-1 == max-length) break;
        }
        return (result,order_type,trader_address);
    }
    //Return invited my order list @ count maximum quantity returned
     function inviDeal(address usr, uint256 count) external view returns (uint256[8][] memory,bytes32[2][] memory, address[2][] memory) {
        uint length = invite[usr];
        if (count < length) length = count;
        uint256[8][] memory result = new uint256[8][](length);
        bytes32[2][] memory order_type = new bytes32[2][](length);
        address[2][] memory trader_address = new address[2][](length);
        uint max = invite[usr];
        uint j;
        for (uint i = max; i >=1 ; --i) {
            uint n = inviter[usr][i][0];
            uint what = inviter[usr][i][1];
            UserInfo memory m = user[n];
            uint256[8] memory _order = [n,m.uoa,m.uma,m.mma,what,tima[n][what],arb[n][what],execone[n][what]];
            bytes32[2] memory _type = [m.pro,m.mark];
            address[2] memory _addr = [m.uad,m.mad];
            order_type[j] = _type;
            result[j] = _order;
            trader_address[j] =_addr;
            j +=1;
            if (i-1 == max-length) break;
        }
        return (result,order_type,trader_address);
    }
    function arbw(uint256 i, address usr) public view returns (uint256) {
            uint256 _arbw = arbtwo.arbw(i,usr);
        return  _arbw;
    } 
     
      //Return the maximum quantity returned from the list @ count of orders I participated in the second round of arbitration
     function applDeal(address usr, uint256 count) external view returns (uint256[8][] memory,bytes32[2][] memory, address[2][] memory) {
        uint length = appl[usr];
        if (count < length) length = count;
        uint256[8][] memory result = new uint256[8][](length);
        bytes32[2][] memory order_type = new bytes32[2][](length);
        address[2][] memory trader_address = new address[2][](length);
        uint max = appl[usr];
        uint j;
        for (uint i = max; i >=1 ; --i) {
            uint n = register[usr][i];
            uint256 _arbw = arbw(n,usr);
            ( , , , ,uint256 timd, uint256 time,uint256 finish) = arbtwo.arbs(n);
            UserInfo memory m = user[n];
            uint256[8] memory _order = [n,m.uoa,m.uma,m.mma,_arbw,timd,time,finish];
            bytes32[2] memory _type = [m.pro,m.mark];
            address[2] memory _addr = [m.uad,m.mad];
            order_type[j] = _type;
            result[j] = _order;
            trader_address[j] =_addr;
            j +=1;
            if (i-1 == max-length) break;
        }
        return (result,order_type,trader_address);
    }
     //Return the list of orders I successfully arbitrated @ count the maximum quantity returned
     function arbsucc(address usr, uint256 count) external view returns (uint256[8][] memory,bytes32[2][] memory, address[2][] memory) {
        uint length = succeed[usr];
        if (count < length) length = count;
        uint256[8][] memory result = new uint256[8][](length);
        bytes32[2][] memory order_type = new bytes32[2][](length);
        address[2][] memory trader_address = new address[2][](length);
        uint max = succeed[usr];
        uint j;
        for (uint i = max; i >=1 ; --i) {
            uint n = arbsuceed[usr][i][0];
            uint256 what = arbsuceed[usr][i][1];
            uint256 _arb;
            if (what == 3)  _arb = 3;
            else  _arb = arb[n][what];
            ( , , , , , ,uint256 finish) = arbtwo.arbs(n);
            UserInfo memory m = user[n];
            uint256[8] memory _order = [n,m.uoa,m.uma,m.mma, _arb,what,execone[n][what],finish];
            bytes32[2] memory _type = [m.pro,m.mark];
            address[2] memory _addr = [m.uad,m.mad];
            order_type[j] = _type;
            result[j] = _order;
            trader_address[j] =_addr;
            j +=1;
            if (i-1 == max-length) break;
        }
        return (result,order_type,trader_address);
    }
    //Return the order list I initiated arbitration @ count the maximum quantity returned
    function ownerappeal(address usr, uint256 count) external view returns (uint256[10][] memory,bytes32[2][] memory, address[2][] memory) {
        uint length = appeal[usr];
        if (count < length) length = count;
        uint256[10][] memory result = new uint256[10][](length);
        bytes32[2][] memory order_type = new bytes32[2][](length);
        address[2][] memory trader_address = new address[2][](length);
        uint max = appeal[usr];
        uint j;
        for (uint i = max; i >=1 ; --i) {
            uint n = owner_appeal[usr][i][0];
            uint what = owner_appeal[usr][i][1];
            uint _arb =  arb[n][what];
            ( , , , , ,uint256 time,uint256 finish) = arbtwo.arbs(n);
            UserInfo memory m = user[n];
            uint256[10] memory _order = [n,m.uoa,m.uma,m.mma,what,_arb,tima[n][what],execone[n][what],time,finish];
            bytes32[2] memory _type = [m.pro,m.mark];
            address[2] memory _addr = [m.uad,m.mad];
            order_type[j] = _type;
            result[j] = _order;
            trader_address[j] =_addr;
            j +=1;
            if (i-1 == max-length) break;
        }
        return (result,order_type,trader_address);
    }
    //Return my arbitration information
    function ownermess(address usr) external view returns (string memory, uint256[7] memory) {
        uint256 _balance = balanceMar[usr];
        uint256 _arber = arber[usr];
        uint256 _appeal = appeal[usr];
        uint256 _invite = invite[usr];
        uint256 _appl = appl[usr];
        uint256 _succeed = succeed[usr];
        uint256 _lock = lock[usr];
        uint256[7] memory data = [_balance,_arber,_appeal,_invite,_appl,_succeed,_lock];
        return (message[usr][0],data);
    }
      //Return order arbitration information
    function arbwhat(uint256 i,uint256 what) external view returns (address, address, uint256[3] memory) {
        uint256 _tima = tima[i][what];
        uint256 _arb = arb[i][what];
        uint256 _execone = execone[i][what];
        address _uadd = ama[i][what][0];
        address _madd = ama[i][what][1];

        uint256[3] memory data = [_tima,_arb,_execone];
        return (_uadd,_madd,data);
    }
    //Return to the person I invited
    function hopers(address usr, uint256 i,uint256 what) external view returns (address[] memory, string[] memory) {
        uint length = order[i][what][usr];
        address[] memory addr = new address[](length);
        string[] memory nickname = new string[](length); 
        for (uint j = 1; j <=length ; ++j) {
            address arbitrators = hoper[i][what][usr][j];
            addr[j-1]  = arbitrators;
            nickname[j-1] = message[arbitrators][0];
        }
        return (addr,nickname);
    }
    //Batch return of arbitrator information
    function list_nicename(address[] calldata usr, uint256 n) external view returns (string[] memory) {
        string[] memory nickname = new string[](usr.length); 
        for (uint i = 0; i <usr.length ; ++i) {
            nickname[i] = message[usr[i]][n];
        }
        return nickname;
    }
}