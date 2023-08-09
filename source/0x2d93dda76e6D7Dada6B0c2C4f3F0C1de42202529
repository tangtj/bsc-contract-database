pragma solidity >= 0.5.0;
//SPDX-License-Identifier: UNLICENCED
contract REIN_FORCE{
  
    event Registration(address indexed user, address indexed referrer,string referrerId,uint package,uint bnbAmt);
    event BuyMatrix(address indexed user, uint256 matrixAmt,uint256 matrix,uint256 bnbAmt);
    event UpgradeLevel(address indexed user, uint256 levelAmt,uint256 level,address indexed sponsor,address indexed promoter,uint256 bnbAmt);
    event MagicalMatrix(address indexed user, uint256 matrix,uint256 matrixAmt,uint256 bnbAmt);
    event BuyPool(address indexed user, uint256 poolAmt,string trType,uint256 bnbAmt); 
    event UserIncome(address indexed sender, address indexed receiver, uint level, uint256 IncomeAmt,string IncomeType,uint256 bnbAmt);
  
  using SafeMath for uint256;
  address payable public owner;
  uint256 public adminPer=5;  
  address payable adminWallet;
  address payable magicalWallet;
  address payable matrixWallet;
  address payable authAdmin;
  uint256 public usdtPrice=100000000000000;    //0.0041 BNB   
   mapping(uint8 => uint256) public levelPrice;
   mapping(uint8 => uint256) public magicalMatrix;
   mapping(uint8 => uint256) public matrix;
     
  constructor(address payable ownerAddress,address payable _adminWallet,address payable _magicalWallet,address payable _matrixWallet,address payable _authAdmin) public {
    owner = ownerAddress;
    adminWallet=_adminWallet;  
    magicalWallet=_magicalWallet;
    matrixWallet=_matrixWallet;
    authAdmin=_authAdmin;

        levelPrice[1] = 20 ;
        levelPrice[2] = 30 ;
        levelPrice[3] = 40 ;
        levelPrice[4] = 60 ;
        levelPrice[5] = 100;
        levelPrice[6] = 200 ;
        levelPrice[7] = 500 ;
        levelPrice[8] = 1000 ;
        levelPrice[9] = 5000 ;
        levelPrice[10] = 10000 ;

        matrix[1]=50;
        matrix[2]=100;
        matrix[3]=200; 
        matrix[4]=400;  
        matrix[5]=800;  
        matrix[6]=1600; 
        matrix[7]=3200; 
        matrix[8]=6400; 
        matrix[9]=12800; 

        magicalMatrix[1]=100;
        magicalMatrix[2]=200;
        magicalMatrix[3]=400; 
        magicalMatrix[4]=800;  
        magicalMatrix[5]=1600;  
        magicalMatrix[6]=3200;  

  }
    
  function NewRegistration(string memory sponcer_id,address payable referrerAddress) public payable
	{
        uint256 bnbAmt=levelPrice[1]*usdtPrice;
        require(msg.value>=bnbAmt,"Invalid BNB Amount");
        uint256 adminAmt=bnbAmt*adminPer/100;
        uint256 referralbnb=bnbAmt*(100-adminPer)/100;
        if(adminAmt>0)
        {
            adminWallet.transfer(adminAmt);
        }
        referrerAddress.transfer(referralbnb);
 		    emit Registration(msg.sender, referrerAddress,sponcer_id,levelPrice[1],bnbAmt);
        emit UserIncome(msg.sender, referrerAddress, 1, levelPrice[1],"REFERRAL INCOME",referralbnb);
	}

    function buyLevel(address payable referrerAddress,address payable promoterAddress,uint8 level) public payable
	{
        uint256 bnbAmt=levelPrice[level]*usdtPrice;
        require(msg.value>=bnbAmt,"Invalid BNB Amount");
        uint256 adminAmt=bnbAmt*adminPer/100;
        uint256 incomeAmt=(bnbAmt-adminAmt)/2;
        if(adminAmt>0)
        {
            adminWallet.transfer(adminAmt);
        }
        referrerAddress.transfer(incomeAmt);
        promoterAddress.transfer(incomeAmt);
        emit UpgradeLevel(msg.sender, levelPrice[level],level,referrerAddress,promoterAddress,bnbAmt);
 	    emit UserIncome(msg.sender, referrerAddress, level, levelPrice[level],"SPONSOR INCOME",incomeAmt);
        emit UserIncome(msg.sender, referrerAddress, level, levelPrice[level],"PLACEMENT UPLINE INCOME",incomeAmt);
	}

    function buyMatrix(uint8 level) public payable
	{
        uint256 bnbAmt=matrix[level]*usdtPrice;
        require(msg.value>=bnbAmt,"Invalid BNB Amount");
        uint256 adminAmt=bnbAmt*adminPer/100;
        uint256 matrixAmt=bnbAmt*(100-adminPer)/100;
        if(adminAmt>0)
        {
            adminWallet.transfer(adminAmt);
        }
        matrixWallet.transfer(matrixAmt);
        emit BuyMatrix(msg.sender, matrix[level],level,bnbAmt);
	}
    function buyMagicalMatrix(uint8 level) public payable
	{
        uint256 bnbAmt=magicalMatrix[level]*usdtPrice;
        require(msg.value>=bnbAmt,"Invalid BNB Amount");
        uint256 adminAmt=bnbAmt*adminPer/100;
        uint256 matrixAmt=bnbAmt*(100-adminPer)/100;
        if(adminAmt>0)
        {
            adminWallet.transfer(adminAmt);
        }
        magicalWallet.transfer(matrixAmt);
        emit BuyMatrix(msg.sender, matrix[level],level,bnbAmt);
	}

    function buypool(uint8 pool,string memory  trType) public payable
	{
        uint256 bnbAmt=pool*usdtPrice;
        require(msg.value>=bnbAmt,"Invalid BNB Amount");
        owner.transfer(bnbAmt);
        emit BuyPool(msg.sender, pool,trType,bnbAmt); 
	}

    function multisendBNB(address payable[]  memory  _contributors, uint256[] memory _balances) public payable {
        uint256 total = msg.value;
        uint256 i = 0;
        for (i; i < _contributors.length; i++) {
            require(total >= _balances[i] );
            total = total.sub(_balances[i]);
            _contributors[i].transfer(_balances[i]);
          }
	
    }
    
   
  function withdrawLostBNBFromBalance() public {
        owner.transfer(address(this).balance);
  }
 
  function transferOwnerShip(address payable newOwner) external {
        require(msg.sender==owner,'Permission denied');
        owner = newOwner;
    }

    function changeAdminWallet(address payable _newAdmin) external {
        require(msg.sender==owner,'Permission denied');
        adminWallet = _newAdmin;
    }
	
    function changematrixWallet(address payable _newMatrix) external {
        require(msg.sender==owner,'Permission denied');
        matrixWallet = _newMatrix;
    }
    function changeMagicalMatrix(address payable _magicalWallet) external {
        require(msg.sender==owner,'Permission denied');
        magicalWallet = _magicalWallet;
    }
    
    function perSetting(uint256 _adminPer) external {
        require(msg.sender==owner,'Permission denied');
        adminPer=_adminPer;
     
    }
    function priceSetting(uint256 _usdtPrice) external {
        require(msg.sender==authAdmin,'Permission denied');
        usdtPrice=_usdtPrice;
    }
    
}


/**     
 * @title SafeMath
 * @dev Math operations with safety checks that throw on error
 */
library SafeMath {

  /**
  * @dev Multiplies two numbers, throws on overflow.
  */
  function mul(uint256 a, uint256 b) internal pure returns (uint256) {
    if (a == 0) {
      return 0;
    }
    uint256 c = a * b;
    assert(c / a == b);
    return c;
  }

  /**
  * @dev Integer division of two numbers, truncating the quotient.
  */
  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    // assert(b > 0); // Solidity automatically throws when dividing by 0
    uint256 c = a / b;
    // assert(a == b * c + a % b); // There is no case in which this doesn't hold
    return c;
  }

  /**
  * @dev Substracts two numbers, throws on overflow (i.e. if subtrahend is greater than minuend).
  */
  function sub(uint256 a, uint256 b) internal pure returns (uint256) {
    assert(b <= a);
    return a - b;
  }

  /**
  * @dev Adds two numbers, throws on overflow.
  */
  function add(uint256 a, uint256 b) internal pure returns (uint256) {
    uint256 c = a + b;
    assert(c >= a); 
    return c;
  }
}