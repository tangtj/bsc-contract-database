pragma solidity >= 0.5.0;
//SPDX-License-Identifier: UNLICENCED
interface IBEP20 {
  function totalSupply() external view returns (uint256);
  function balanceOf(address who) external view returns (uint256);
  function allowance(address owner, address spender) external view returns (uint256);
  function transfer(address to, uint256 value) external returns (bool);
  function approve(address spender, uint256 value) external returns (bool);
  
  function transferFrom(address from, address to, uint256 value) external returns (bool);
  function burn(uint256 value) external returns (bool);
  event Transfer(address indexed from,address indexed to,uint256 value);
  event Approval(address indexed owner,address indexed spender,uint256 value);
}
contract CranetStarPro{
  
    event Registration(address indexed user, address indexed referrer,string referrerId,uint jpackage);
    event BuyMatrix(address indexed user, uint256 matrixAmt,uint256 matrix);
    event UpgradeLevel(address indexed user, uint256 levelAmt,uint256 level,address indexed sponsor,address indexed promoter);
    event MagicalMatrix(address indexed user,uint256 matrixAmt, uint256 matrix);
    event BuyPool(address indexed user, uint256 poolAmt,string trType); 
    event UserIncome(address indexed sender, address indexed receiver, uint level, uint256 levelAmt,string IncomeType,uint256 incomeAmt);
  
  using SafeMath for uint256;
  address payable public owner;
  uint256 public adminPer=10;  
  address payable adminWallet;
  address payable magicalWallet;
  address payable matrixWallet;
  address payable authAdmin;
  mapping(uint8 => uint256) public levelPrice;
  mapping(uint8 => uint256) public magicalMatrix;
  mapping(uint8 => uint256) public matrix;
  IBEP20 private BUSD; 
     
  constructor(address payable ownerAddress,IBEP20 _BUSD,address payable _adminWallet,address payable _magicalWallet,address payable _matrixWallet,address payable _authAdmin) public {
    owner = ownerAddress;
    BUSD=_BUSD;
    adminWallet=_adminWallet;  
    magicalWallet=_magicalWallet;
    matrixWallet=_matrixWallet;
    authAdmin=_authAdmin;

        levelPrice[1] = 10*1e18 ;
        levelPrice[2] = 15 *1e18;
        levelPrice[3] = 30 *1e18;
        levelPrice[4] = 60 *1e18;
        levelPrice[5] = 120 *1e18;
        levelPrice[6] = 240 *1e18;
        levelPrice[7] = 480 *1e18;
        levelPrice[8] = 960 *1e18;
        levelPrice[9] = 1920 *1e18;
        levelPrice[10] = 3840 *1e18;

        matrix[1]=25 *1e18;
        matrix[2]=50 *1e18;
        matrix[3]=100 *1e18; 
        matrix[4]=200*1e18;  
        matrix[5]=400 *1e18;
        matrix[6]=800*1e18 ;
        matrix[7]=1600*1e18 ;
        matrix[8]=3200*1e18 ;
        matrix[9]=6400*1e18 ;

        magicalMatrix[1]=50*1e18;
        magicalMatrix[2]=100*1e18;
        magicalMatrix[3]=200*1e18 ;
        magicalMatrix[4]=400*1e18  ;
        magicalMatrix[5]=800*1e18  ;
        magicalMatrix[6]=1600*1e18 ; 
        magicalMatrix[7]=3200*1e18 ; 
        magicalMatrix[8]=6400*1e18  ;

  }
    
  function NewRegistration(string memory sponcer_id,address payable referrerAddress,uint256 _amount) public payable
	{
       
        require(_amount>=levelPrice[1],"Invalid Pakcage Amount");
        require(BUSD.balanceOf(msg.sender) >= _amount,"Low token Balance");
        require(BUSD.allowance(msg.sender,address(this)) >= _amount,"Invalid allowance");
        BUSD.transferFrom(msg.sender ,address(this), _amount);
        uint256 adminAmt=_amount*adminPer/100;
        BUSD.transfer(adminWallet ,adminAmt);
        uint256 referralAmt=levelPrice[1]-adminAmt;
        BUSD.transfer(referrerAddress ,referralAmt);
        emit UserIncome(msg.sender, referrerAddress, 1, levelPrice[1],"REFERRAL INCOME",referralAmt);
        emit Registration(msg.sender, referrerAddress,sponcer_id,levelPrice[1]);
	}

  function buyLevel(address payable referrerAddress,address payable promoterAddress,uint8 level) public payable
	{
        uint256 busdAmt=levelPrice[level];
        require(BUSD.balanceOf(msg.sender) >= busdAmt,"Low token Balance");
        require(BUSD.allowance(msg.sender,address(this)) >= busdAmt,"Invalid allowance");
        BUSD.transferFrom(msg.sender ,address(this), busdAmt);
        uint256 adminAmt=busdAmt*adminPer/100;
        uint256 SpincomeAmt=(busdAmt-adminAmt)*60/100;
        uint256 PrncomeAmt=(busdAmt-adminAmt)*40/100;
        BUSD.transfer(adminWallet,adminAmt);
        BUSD.transfer(referrerAddress,SpincomeAmt);
        BUSD.transfer(promoterAddress,PrncomeAmt);
        emit UpgradeLevel(msg.sender, levelPrice[level],level,referrerAddress,promoterAddress);
 	      emit UserIncome(msg.sender, referrerAddress, level, levelPrice[level],"SPONSOR INCOME",SpincomeAmt);
        emit UserIncome(msg.sender, promoterAddress, level, levelPrice[level],"PLACEMENT UPLINE INCOME",PrncomeAmt);
	}

    function buyMatrix(uint8 level) public payable
	{
        uint256 busdAmt=matrix[level];
        require(BUSD.balanceOf(msg.sender) >= busdAmt,"Low token Balance");
        require(BUSD.allowance(msg.sender,address(this)) >= busdAmt,"Invalid allowance");
        BUSD.transferFrom(msg.sender ,address(this), busdAmt);
        uint256 adminAmt=busdAmt*adminPer/100;
        uint256 matrixAmt=busdAmt*(100-adminPer)/100;
        BUSD.transfer(adminWallet,adminAmt);
        BUSD.transfer(matrixWallet,matrixAmt);
        emit BuyMatrix(msg.sender, matrix[level],level);
	}
    function buyMagicalMatrix(uint8 level) public payable
	{
        uint256 busdAmt=magicalMatrix[level];
        require(BUSD.balanceOf(msg.sender) >= busdAmt,"Low token Balance");
        require(BUSD.allowance(msg.sender,address(this)) >= busdAmt,"Invalid allowance");
        BUSD.transferFrom(msg.sender ,address(this), busdAmt);
        uint256 adminAmt=busdAmt*adminPer/100;
        uint256 matrixAmt=busdAmt*(100-adminPer)/100;
        BUSD.transfer(adminWallet,adminAmt);
        BUSD.transfer(matrixWallet,matrixAmt);
        emit MagicalMatrix(msg.sender, matrix[level],level);
	}

    function buypool(uint256 pool,string memory  trType) public payable
	{
        BUSD.transferFrom(msg.sender,owner,pool);
        emit BuyPool(msg.sender, pool,trType); 
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