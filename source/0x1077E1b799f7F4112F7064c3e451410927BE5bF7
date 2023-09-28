pragma solidity >= 0.5.0;

interface IERC20 {
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

contract MYLUCKSWAP{
  
    using SafeMath for uint256;

    struct User {
        uint id;
        uint256 fromDate;
        uint256 toDate;
        uint256 balanceAmt;
     }
   
    mapping(address => User) public users;
    mapping(uint => address) public idToAddress;
   
    uint public lastUserId = 2;

    IERC20 private USDT;
    IERC20 private MLK;
    address payable public owner;
    uint256 public mlkUsdPrice =  97*1e14;      //0.0097  =8 paisa
    uint public sale_status = 1;
    uint public SellFee = 2*100;
    uint256 public maxSellQty=100000*1e18;
    event SwapToken(address indexed  userAddress,string fromtoken,string toToken,uint256 tokenQty,uint256 paymentAmt);
   
    constructor(address payable ownerAddress,IERC20 _MLK,IERC20 _USDT) public
    {
        owner = ownerAddress;  
        MLK = _MLK;
        USDT=_USDT;
         User memory user = User({
            id:1,
            fromDate: uint(0),
            toDate:uint(0),
            balanceAmt:uint(0)
        });
        
        users[ownerAddress] = user;
        idToAddress[1] = ownerAddress;
    }
  function userReg(address userAddress) private
  {
      User memory user = User({
            id: lastUserId,
            fromDate: block.timestamp,
            toDate:block.timestamp + 1 days,
            balanceAmt:maxSellQty
        });
        users[userAddress] = user;
        idToAddress[lastUserId] = userAddress;
        lastUserId++;
  }
   function resetDailyLimit(address userAddress) private {
        if (block.timestamp - users[userAddress].fromDate >= 1 days) {
            users[userAddress].fromDate = block.timestamp;
            users[userAddress].toDate = block.timestamp+1 days;
            users[userAddress].balanceAmt=maxSellQty;
        }
  
    }
  function isUserExists(address user) public view returns (bool) 
  {
        return (users[user].id != 0);
  }
  function BuyMLK(uint256 USDTQty) public payable
	{
      uint256 MlkQty=(USDTQty/mlkUsdPrice)*1e18; 
      require(MLK.balanceOf(address(this))>=MlkQty,"Low Token Balance In Contract");
      require(USDT.balanceOf(msg.sender)>=USDTQty,"Low Token Balance In Wallet");
      USDT.transferFrom(msg.sender,address(this), USDTQty);
      MLK.transfer(msg.sender,MlkQty);
      emit SwapToken(msg.sender,"USDT","MLK",USDTQty,MlkQty);
 	}
    
 function SellToken(uint256 MLKQty) public payable
	{
      require(sale_status==1,'Sell Not Allow');
     
      if(!isUserExists(msg.sender))
      {
        userReg(msg.sender);
      }
      else
      {
        resetDailyLimit(msg.sender);
      }
      uint256 balanceSell=users[msg.sender].balanceAmt;
      require(balanceSell>=MLKQty, "Daily transaction limit exceeded");
      uint256 USDTQty=(MLKQty*mlkUsdPrice)/1e18; 
      uint256 FeeAmt=USDTQty-(USDTQty*SellFee/10000);
      uint256 NetUSDTAmt=USDTQty-FeeAmt;
      require(USDT.balanceOf(address(this))>=NetUSDTAmt,"Low Token Balance In Contract");
      require(MLK.balanceOf(msg.sender)>=MLKQty,"Low Token Balance In Wallet");
      MLK.transferFrom(msg.sender,address(this), MLKQty);
      USDT.transfer(msg.sender,NetUSDTAmt);
      users[msg.sender].balanceAmt-=MLKQty;
      emit SwapToken(msg.sender,"MLK","USDT",NetUSDTAmt,MLKQty);
 	}
  function getBalance(address userAddress) public view returns(uint256)
  {
    uint256 balAmt=0;
    if(isUserExists(userAddress))
    {
         balAmt= users[userAddress].balanceAmt;
    }
    else{
       balAmt=maxSellQty;
    }
     return balAmt;
  } 
   function Price_setting(uint256 _mlkPrice) public payable
    {
        require(msg.sender==owner,"Only Owner");
        mlkUsdPrice=_mlkPrice;
  
    }
    function sellLimit(uint256 _maxSellQty) public payable
    {
        require(msg.sender==owner,"Only Owner");
        maxSellQty=_maxSellQty*1e18;
  
    }
    function Fee_setting(uint256 _sellFee) public payable
    {
        require(msg.sender==owner,"Only Owner");
        SellFee=_sellFee;
    }
	 function sale_setting(uint start_sale) public payable
    {
        require(msg.sender==owner,"Only Owner");
        sale_status=start_sale;
    }
        
     function withdrawLost(uint256 WithAmt) public {
        require(msg.sender == owner, "onlyOwner");
        owner.transfer(WithAmt*1e18);
    }
    
  
	function withdrawLostTokenFromBalance(uint QtyAmt,IERC20 _TOKEN) public 
	{
        require(msg.sender == owner, "onlyOwner");
        _TOKEN.transfer(owner,(QtyAmt));
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