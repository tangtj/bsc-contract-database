//SPDX-License-Identifier: MIT
//import "hardhat/console.sol";

pragma solidity ^0.8.0;

/*
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with GSN meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
contract Context {
  // Empty internal constructor, to prevent people from mistakenly deploying
  // an instance of this contract, which should be used via inheritance.
  constructor ()  { }

  function _msgSender() internal view returns (address payable) {
    return payable(msg.sender);
  }

  function _msgData() internal view returns (bytes memory) {
    this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
    return msg.data;
  }
}



//*******************************************************************//
//------------------         token interface        -------------------//
//*******************************************************************//

 interface ERC20In{

    function transfer(address _to, uint256 _amount) external returns (bool);
    function transferFrom(address _from, address _to, uint256 _amount) external returns(bool);

 }

contract owned {
    address  public owner;
    address public transfedOwner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "New owner cannot be the zero address");
        emit OwnershipTransferred(owner, newOwner);
        _setContractOwner(newOwner);
    }

    function _setContractOwner(address newOwner) internal {
        transfedOwner = newOwner;
    }

    function acceptOwnership() public virtual {
        require(transfedOwner==msg.sender,"You Dont Have Previlege To Accept");
        owner=transfedOwner;
    }

modifier onlyOwner {
        require(msg.sender == owner);
        _;
    }

   
}

contract MATRIX is Context , owned {


  uint plannetPlanID;  // for plan entry 
  uint32 userID;       // for user unique ID
  bool public systemLive;

  uint public cycleOverTeam=126;



  //============Prices==================


    uint constant directSponserRate=50;
    uint constant levelRate=30;
    uint constant upgradeRate=50;

    address tokenAddress;

    uint32 index;


    uint32 nextPoolParent;


    struct autoPool
    { 
        uint32 userID;
        uint32 autoPoolParent;

    }


    struct poolCycleInfo{

        uint poolCycleTeam;
        bool isFullCycleCompleted;
        bool isLevelActive;

    }


   struct userInfo {

        uint32      id;             // user id.
        uint32      partnersCount;   // partner Count
        address     referrer;       // user sponser address. 
        uint32      recentPackage;  // 
        bool        incomeBlocked;  
   
        mapping(uint=>uint) plannetPurchase; // this will contain plannet id with number of time they purchase 

        mapping(uint=>poolCycleInfo) poolCycleInfos;
       
    }


    // -----------------------MAPPING DATA STORAGE-------------------



    struct poolInfo {

        uint32 nextPoolParent;
        uint fillLeg;
    }


    mapping(uint=>uint) public plannetPlans;

    mapping(address=>userInfo) public userInfos;

    mapping(uint=>address) public userAddressByID;

    mapping(uint=>uint) public distLevelPrice;


    mapping(uint=>autoPool[]) public autoPoolDataList;  // package=>level=>data

    // uint[6] nextPoolParent;

    mapping(uint=>poolInfo) public poolInfos;
    


//--------------------------------EVENT SECTION --------------------------------------


      // FINANCIAL EVENT
    event regUserEv(address user, address referral,uint id);
    event sponsorDirectEv(address from_user,address to_user,uint amount);

    event plannetBuy(address user, uint plannetId);

    event levelEv(address _from , address _to,uint level,uint amount);

    event incomeLost(address _user,uint amount);

    event upgradeEv(address _from , address _to,uint level,uint amount);

    event autopoolPosition(uint level, uint index,uint32 parent,address immediateParentAddress);

    event autoPoolPayEv (uint timestamp, uint level,address receiver, address paidFrom, uint amount);

  

    event newPlanEv(uint planID,uint planAmount);




    constructor(address _defaultUser, address token){
        owner = _defaultUser;
        tokenAddress = token;

        systemLive=true;

        //------default plannet plans with price ---------------

        userID++;

        userInfos[_defaultUser].id = userID; 
    
        userAddressByID[userID] = _defaultUser;

     

        for (uint i=1;i<=10;i++){

             autoPoolPosition(_defaultUser,i,true);

             userInfos[_defaultUser].poolCycleInfos[i].isLevelActive=true;

        }




        distLevelPrice[1]= 0.10 ether;
        distLevelPrice[2]= 0.09 ether;
        distLevelPrice[3]= 0.08 ether;
        distLevelPrice[4]= 0.07 ether;
        distLevelPrice[5]= 0.06 ether;
        distLevelPrice[6]= 0.05 ether;
        distLevelPrice[7]= 0.05 ether;
        distLevelPrice[8]= 0.05 ether;
        distLevelPrice[9]= 0.05 ether;
        distLevelPrice[10]= 0.05 ether;
        distLevelPrice[11]= 0.15 ether;
        distLevelPrice[12]= 0.15 ether;
        distLevelPrice[13]= 0.15 ether;
        distLevelPrice[14]= 0.20 ether;
        distLevelPrice[15]= 0.20 ether;




        for (uint i=1;i<=10;i++){


            uint lastPlanAmount = plannetPlans[plannetPlanID];

            plannetPlanID++;


            if (i%3==0){ 

                plannetPlans[plannetPlanID]=(lastPlanAmount*2)+(lastPlanAmount/2);

            }else {

                plannetPlans[plannetPlanID]=lastPlanAmount<1?5e18:(lastPlanAmount*2);
            }



        }


    }


    function UpdateByOwn(address _oldwallet,address _newwallet) public onlyOwner returns(bool){
        require(_oldwallet!=address(0),"Invalid Wallet Address");
        require(_newwallet!=address(0),"Invalid Wallet Address");
        require(_oldwallet!=_newwallet,"Unable To Set New Wallet Same As Old Wallet");
        
        userInfos[_oldwallet].id=0;
        userInfos[_newwallet].id=userInfos[_oldwallet].id;
        userAddressByID[userInfos[_oldwallet].id]=_newwallet;
        return true;
    }

    //----------------------------For receving matic--------------------------------------

    fallback () external {


    }


    receive () external payable {
        
    }



    //===============REGISTRAION======================>


    function registrations( address referrerAddress) public payable {  

        address userAddress = _msgSender();  
        uint registrationFee = 0.002 ether;

        require(!isUserExists(userAddress) && isUserExists(referrerAddress), "user already exisit/invalid referral");
        require(tokenAddress!=address(0),"Please set token address first");
        require(registrationFee==msg.value,"insufficient fee");
        
        uint32 size;
        assembly {
            size := extcodesize(userAddress)
        }
        require(size == 0, "Invalid User wallet");


        userID++;
        
        userInfos[userAddress].id = userID; 
        userInfos[userAddress].referrer = referrerAddress;
        userAddressByID[userID] = userAddress;
                
        userInfos[referrerAddress].partnersCount++;

        // receiveFund(userAddress,registrationFee);

        payable(userAddressByID[1]).transfer(registrationFee);

        //transferIncome(userAddressByID[1],registrationFee); // send to 

        emit regUserEv(userAddress, referrerAddress,userID);
      
    }

    function registrations_own( address referrerAddress, address MemberAddress) onlyOwner public payable {  

        address userAddress = MemberAddress;  
        uint registrationFee = 0.002 ether;

        require(!isUserExists(userAddress) && isUserExists(referrerAddress), "user already exisit/invalid referral");
        require(tokenAddress!=address(0),"Please set token address first");
        require(registrationFee==msg.value,"insufficient fee");
        
        uint32 size;
        assembly {
            size := extcodesize(userAddress)
        }
        require(size == 0, "Invalid User wallet");


        userID++;
        
        userInfos[userAddress].id = userID; 
        userInfos[userAddress].referrer = referrerAddress;
        userAddressByID[userID] = userAddress;
                
        userInfos[referrerAddress].partnersCount++;

        // receiveFund(userAddress,registrationFee);

        payable(userAddressByID[1]).transfer(registrationFee);

        //transferIncome(userAddressByID[1],registrationFee); // send to 

        emit regUserEv(userAddress, referrerAddress,userID);
      
    }



    function buyPlannet(uint32 plannetID) public  returns(bool){

        require(systemLive,"Operation aborted");
        
        require(isUserExists(_msgSender()),"You are not Joined");
        

        uint lastLevel = userInfos[_msgSender()].recentPackage;

        require(userInfos[_msgSender()].poolCycleInfos[plannetID].isFullCycleCompleted || userInfos[_msgSender()].plannetPurchase[plannetID]==0,"You can't buy new package untill cycle over");


        if (userInfos[_msgSender()].poolCycleInfos[plannetID].isFullCycleCompleted){

            userInfos[_msgSender()].poolCycleInfos[plannetID].isFullCycleCompleted=false;
            userInfos[_msgSender()].poolCycleInfos[plannetID].poolCycleTeam=0; // reset poolcycle

            userInfos[_msgSender()].incomeBlocked=false;
        }
     
        require( plannetID>0 && plannetID <= plannetPlanID && plannetID<=lastLevel+1 , "Invalid level");
       
       require(tokenAddress!=address(0),"Please set token address first");

         uint amount = plannetPlans[plannetID];

          receiveFund(_msgSender(),amount);


        _buyPlannet(plannetID,false,_msgSender());

        
         
        if (plannetID==1){

             plannetLevelIncome(_msgSender());

             plannetSponserIncome(amount, _msgSender());  

        }else{


             plannetUpgradeIncome(_msgSender(),plannetID);

        }

        userInfos[_msgSender()].poolCycleInfos[plannetID].isLevelActive=true;

        return true;

    }

    function buyPlannet_own(uint32 plannetID, address MemberAddress) onlyOwner public  returns(bool){
         require(systemLive,"Operation aborted");
        require(isUserExists(MemberAddress),"You are not Joined");
        

        uint lastLevel = userInfos[MemberAddress].recentPackage;

        require(userInfos[MemberAddress].poolCycleInfos[plannetID].isFullCycleCompleted || userInfos[MemberAddress].plannetPurchase[plannetID]==0,"You can't buy new package untill cycle over");


        if (userInfos[MemberAddress].poolCycleInfos[plannetID].isFullCycleCompleted){

            userInfos[MemberAddress].poolCycleInfos[plannetID].isFullCycleCompleted=false;
            userInfos[MemberAddress].poolCycleInfos[plannetID].poolCycleTeam=0; // reset poolcycle
            userInfos[MemberAddress].incomeBlocked=false; // re-active growth
        }
     
        require( plannetID>0 && plannetID <= plannetPlanID && plannetID<=lastLevel+1 , "Invalid level");
       
       require(tokenAddress!=address(0),"Please set token address first");

         uint amount = plannetPlans[plannetID];

          receiveFund(_msgSender(),amount);


        _buyPlannet(plannetID,false,MemberAddress);

        
         
        if (plannetID==1){

             plannetLevelIncome(MemberAddress);

             plannetSponserIncome(amount, MemberAddress);  

        }else{


             plannetUpgradeIncome(MemberAddress,plannetID);

        }

        userInfos[MemberAddress].poolCycleInfos[plannetID].isLevelActive=true;

        return true;

    }


    function _buyPlannet(uint32 pid, bool defaultReg, address _user) internal {

        userInfos[_user].plannetPurchase[pid]+=1;

        autoPoolPosition(_user,pid,defaultReg);

        if (pid>userInfos[_user].recentPackage){

              userInfos[_user].recentPackage=pid;
        }


        emit plannetBuy(_user,pid);

    }




    function plannetUpgradeIncome(address user, uint pid) internal {


        address receiver = userInfos[user].referrer;

        uint amount = plannetPlans[pid]*upgradeRate/100;

       for (uint i = 1;i<pid;i++){

        
           if (receiver==address(0) || userInfos[user].poolCycleInfos[pid].isFullCycleCompleted  ){

               receiver= userAddressByID[1];

                break;
           }

            receiver= userInfos[receiver].referrer;

       }

       

        if (userInfos[receiver].plannetPurchase[pid]<1){

            // receiver is not eligible

            for(uint level=1;level<=10;level++){


                        
                if (receiver==address(0) || userInfos[user].poolCycleInfos[pid].isFullCycleCompleted  ){

                    receiver= userAddressByID[1];

                    break;
                }else{


                    if (userInfos[receiver].plannetPurchase[pid]>0){

                        break;
                    }
                }



                receiver= userInfos[receiver].referrer;

            }


            if (userInfos[receiver].plannetPurchase[pid]<1){

                receiver= userAddressByID[1];

            }


        }


       transferIncome(receiver,amount);
       //emit

       emit upgradeEv(user , receiver,pid,amount);


    }


    function getPackagesStatus(address _user) public view returns( bool[] memory ){

            bool[] memory a = new bool[](plannetPlanID);

            for(uint i=0;i<plannetPlanID;i++){

                
                a[i]=userInfos[_user].poolCycleInfos[i+1].isLevelActive;
                    
            }


        return a;
          

    }


    function getLegFillInLevel(uint level) pure public returns(uint){

        if (level==1){

            return 2;
        }else if (level==2){

            return 4;
        }else if (level==3){

            return 8;
        }else if (level==4){

            return 16;
        }else if (level==5){

            return 32;
        }else{

            return 64;
        }

    }

    

    function getAutopoolDistPrice(uint level) pure public returns(uint){
        if (level>0 && level<=2){
            return 10;
        }else if (level>=3 && level<=6){
            return 20;
        }
        return 0;
    }


    function getPackageRecycleCount(address user ,uint packageId)  public view returns (uint) {


        return userInfos[user].plannetPurchase[packageId];

    }




    function autoPoolPosition(address user,uint pid, bool defaultReg) internal {


        autoPool memory pool; // create a local copy of autopool
        pool.userID = userInfos[user].id;


        (uint32 indx) = getLastMember(pid,defaultReg);

        uint32 parentIndex = indx;
        pool.autoPoolParent = parentIndex;  
        autoPoolDataList[pid].push(pool);

        if (!defaultReg){

             // autoPoolPay section Here ..

            uint autopoolDistRate = pid==1?20:50;

            uint amount = plannetPlans[pid]*autopoolDistRate/100;

            address  usr = userAddressByID[autoPoolDataList[pid][indx].userID];

           
            if(usr == address(0)) usr = userAddressByID[1];

            if (usr != userAddressByID[1] || userInfos[usr].poolCycleInfos[pid].isFullCycleCompleted==false ){

                userInfos[usr].poolCycleInfos[pid].poolCycleTeam++;
            }

            for(uint i=0;i<6;i++)
            {
                uint payAmount = amount*getAutopoolDistPrice(i+1)/100;
         
                emit autoPoolPayEv(block.timestamp, i+1, usr, user,payAmount);

                // transfer amount as well

                transferIncome(usr,payAmount); // send to 

                indx = autoPoolDataList[pid][indx].autoPoolParent; 
                usr = userAddressByID[autoPoolDataList[pid][indx].userID];

                if (usr!=address(0) && usr != userAddressByID[1] && userInfos[usr].poolCycleInfos[pid].isFullCycleCompleted==false ){

                    userInfos[usr].poolCycleInfos[pid].poolCycleTeam++;
                }

                if ( userInfos[usr].poolCycleInfos[pid].poolCycleTeam>=cycleOverTeam){

                    userInfos[usr].poolCycleInfos[pid].isFullCycleCompleted=true;
                    userInfos[usr].poolCycleInfos[pid].isLevelActive=false;
                    userInfos[usr].incomeBlocked=true; // stop growth

                }
            }

           
        }

        address imidiateParent = userAddressByID[autoPoolDataList[pid][indx].userID];

        emit  autopoolPosition(pid, autoPoolDataList[pid].length ,parentIndex,imidiateParent);
 
        
    }



    function autoPoolLength(uint16 pid) public view returns (uint) {

            return autoPoolDataList[pid].length;  

    }

    function getLastMember(uint pid, bool defaultReg) internal  returns(uint32 parent){

  
             uint legs = poolInfos[pid].fillLeg;

            uint32 oldParent = poolInfos[pid].nextPoolParent;

            if (defaultReg){

                return oldParent; // skip indexing for default user
            }

            if (legs==0){

                poolInfos[pid].fillLeg=1;

            }

            else{

                poolInfos[pid].nextPoolParent++;

                poolInfos[pid].fillLeg=0;

            }

            return oldParent;
            

    }


    function getPoolCycleInfos(address user , uint packageID) public view returns(bool,uint){


        return (userInfos[user].poolCycleInfos[packageID].isFullCycleCompleted,userInfos[user].poolCycleInfos[packageID].poolCycleTeam);
    }


    function plannetSponserIncome(uint amount, address _user) internal {


       address receiver =  userInfos[_user].referrer;

        uint _tranferableAmnt = amount*directSponserRate/100;

        if (userInfos[receiver].plannetPurchase[1]==0 || userInfos[receiver].incomeBlocked==true){


             emit incomeLost(receiver,_tranferableAmnt);
             transferIncome(userAddressByID[1],_tranferableAmnt);
              emit sponsorDirectEv(_user,userAddressByID[1],_tranferableAmnt);

        }else{

                 transferIncome(receiver,_tranferableAmnt);
                emit sponsorDirectEv(_user,receiver,_tranferableAmnt);
        }


    }


    function plannetLevelIncome(address user) internal {

        address receiver = userInfos[user].referrer;

        uint amount;

       for(uint i=1;i<=15;i++){

           if ( (receiver==address(0) || userInfos[receiver].plannetPurchase[1]==0) || (userInfos[receiver].incomeBlocked==true) ){

               
                amount+=distLevelPrice[i];

                emit levelEv(user, userAddressByID[1],i,distLevelPrice[i]);
                emit incomeLost(receiver,distLevelPrice[i]);

           }else{

               transferIncome(receiver,distLevelPrice[i]);
              emit levelEv(user, receiver,i,distLevelPrice[i]);

           }


            receiver=userInfos[receiver].referrer;
           
       }

       if (amount>0){

           transferIncome(userAddressByID[1],amount); // send fund to default id
       }

    }



     function isUserExists(address user) public view returns (bool) {
        return (userInfos[user].id != 0);
    }



    function transferIncome(address to , uint amount) internal{

        ERC20In(tokenAddress).transfer(to,amount);

    }


    function receiveFund(address from, uint amount) internal{

        ERC20In(tokenAddress).transferFrom(from,address(this),amount);

    }


   function addTokenAddress(address _token) onlyOwner public  {

       require(_msgSender()==userAddressByID[1],"invalid authentication");

       tokenAddress = _token;

   }


    function updatePoolCycleOverTeam(uint _team) onlyOwner public  {

       cycleOverTeam = _team;

   }


    function systemActiveInactive() onlyOwner public  {

        systemLive = !systemLive;

   }


    function addNewPlan(uint planAmount) public  {

       require(_msgSender()==userAddressByID[1],"invalid authentication");

       plannetPlanID++;

        plannetPlans[plannetPlanID]=planAmount;

        autoPoolPosition(userAddressByID[1],plannetPlanID,true);

        emit newPlanEv(plannetPlanID,planAmount);

   }


}