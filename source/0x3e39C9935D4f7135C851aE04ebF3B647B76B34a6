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

contract BMXP_SWAP{
  
     using SafeMath for uint256;
    IERC20 private BMXP; 
    IERC20 private BUSD;
    IERC20 private MLK;
    address payable public owner;
    uint256 public bmxpUsdPrice = 97*1e14;    //0.0097  =8 paisa
    uint256 public mlkUsdPrice =  97*1e14;      //0.0097  =8 paisa
    uint256 public mlkBmxpPrice = 97*1e14; 
    uint256 public  MINIMUM_BUY = 10*1e18 ;
    uint public  MAXIMUM_BUY = 100000*1e18 ;
	  uint public  MINIMUM_SALE = 10*1e18 ;
    uint public  MAXIMUM_SALE = 10000*1e18 ;
	  uint public sale_status = 1;
    uint public BuyFee = 1*100;
    uint public SellFee = 2*100;
    event Swap(address user,string fromtoken,string totoken,uint256 tokenQty,uint256 tokenPrice,uint256 totalAmt );
   
    constructor(address payable ownerAddress,IERC20 _MLK,IERC20 _BUSD,IERC20 _BMXP) public
    {
        owner = ownerAddress;  
        MLK = _MLK;
        BUSD=_BUSD;
        BMXP=_BMXP;
    }
    
  function SwapBUSDToBMXP(uint256 tokenQty) public payable
	{
		require(tokenQty>=MINIMUM_BUY,"Invalid minimum quatity");
		require(tokenQty<=MAXIMUM_BUY,"Invalid maximum quatity");
    require(BMXP.balanceOf(address(this))>=tokenQty,"Low Token Balance In Contract");
    uint256 BUSD_amt=(tokenQty*bmxpUsdPrice)/1e18; 
    uint256 FeeAmt=BUSD_amt*(BuyFee/100)/100;  
    uint256 NetBusdAmt=BUSD_amt+FeeAmt;
    require(NetBusdAmt>0,"Invalid buy amount");
    require(BUSD.balanceOf(msg.sender)>=NetBusdAmt,"Low BUSD Balance In Wallet");
		BUSD.transferFrom(msg.sender,address(this), NetBusdAmt);
	 	BMXP.transfer(msg.sender , tokenQty);
        // Swap(address user,string fromtoken,string totoken,uint256 tokenQty,uint256 tokenPrice,uint256 totalAmt )
        emit Swap(msg.sender,"BUSD","BMXP",tokenQty,bmxpUsdPrice,NetBusdAmt);
	}
  function SwapBUSdToMLK(uint256 tokenQty) public payable
	{
		require(tokenQty>=MINIMUM_BUY,"Invalid minimum quatity");
		require(tokenQty<=MAXIMUM_BUY,"Invalid maximum quatity");
    require(MLK.balanceOf(address(this))>=tokenQty,"Low Token Balance In Contract");
    uint256 BUSD_amt=(tokenQty*mlkUsdPrice)/1e18; 
    uint256 FeeAmt=BUSD_amt*(BuyFee/100)/100;  
    uint256 NetBUSDAmt=BUSD_amt+FeeAmt;
    require(NetBUSDAmt>0,"Invalid buy amount");
    require(BUSD.balanceOf(msg.sender)>=NetBUSDAmt,"Low BUSD Balance In Wallet");
		BUSD.transferFrom(msg.sender,address(this), NetBUSDAmt);
		MLK.transfer(msg.sender , tokenQty);
  emit Swap(msg.sender,"BUSD","MLK",tokenQty,mlkUsdPrice,NetBUSDAmt);
	}
    
function SwapBMXPtoBUSD(uint256 tokenQty) public payable
	{
    require(sale_status>=1,"Transaction Not Allow");
		require(tokenQty>=MINIMUM_SALE,"Invalid minimum quatity");
		require(tokenQty<=MAXIMUM_SALE,"Invalid maximum quatity");
    require(BMXP.balanceOf(msg.sender)>=tokenQty,"Low Token Balance In Wallet");
    uint256 BUSD_amt=(tokenQty*bmxpUsdPrice)/1e18; 
    uint256 FeeAmt=BUSD_amt*(SellFee/100)/100;  
    uint256 NetBusdAmt=BUSD_amt-FeeAmt;
    require(NetBusdAmt>0,"Invalid buy amount");
    require(BUSD.balanceOf(address(this))>=tokenQty,"Low BUSD Balance In Contract");
		BMXP.transferFrom(msg.sender,address(this), tokenQty);
		BUSD.transfer(msg.sender , NetBusdAmt);
         emit Swap(msg.sender,"BMXP","BUSD",tokenQty,bmxpUsdPrice,NetBusdAmt);
	}

  function SwapMLKToBUSD(uint256 tokenQty) public payable
	{
    require(sale_status>=1,"Transaction Not Allow");
		require(tokenQty>=MINIMUM_SALE,"Invalid minimum quatity");
		require(tokenQty<=MAXIMUM_SALE,"Invalid maximum quatity");
    require(MLK.balanceOf(msg.sender)>=tokenQty,"Low Token Balance In Wallet");
    uint256 BUSD_amt=(tokenQty*mlkUsdPrice)/1e18; 
    uint256 FeeAmt=BUSD_amt*(SellFee/100)/100;  
    uint256 NetBusdAmt=BUSD_amt-FeeAmt;
    require(NetBusdAmt>0,"Invalid buy amount");
    require(BUSD.balanceOf(address(this))>=tokenQty,"Low BUSD Balance In Contract");
		MLK.transferFrom(msg.sender,address(this), tokenQty);
		BUSD.transfer(msg.sender , NetBusdAmt);
         emit Swap(msg.sender,"MLK","BUSD",tokenQty,mlkUsdPrice,NetBusdAmt);
 	}

  function SwapBMXPtoMLK(uint256 tokenQty) public payable
	{
    require(tokenQty>=MINIMUM_SALE,"Invalid minimum quatity");
		require(tokenQty<=MAXIMUM_SALE,"Invalid maximum quatity");
    require(BMXP.balanceOf(msg.sender)>=tokenQty,"Low Token Balance In Wallet");
    uint256 MLK_amt=(tokenQty*mlkBmxpPrice)/1e18; 
    uint256 FeeAmt=MLK_amt*(SellFee/100)/100;  
    uint256 NetMLKAmt=MLK_amt-FeeAmt;
    require(NetMLKAmt>0,"Invalid buy amount");
    require(MLK.balanceOf(address(this))>=tokenQty,"Low BUSD Balance In Contract");
		BMXP.transferFrom(msg.sender,address(this), tokenQty);
		MLK.transfer(msg.sender , NetMLKAmt);
     emit Swap(msg.sender,"BMXP","MLK",tokenQty,mlkBmxpPrice,NetMLKAmt);
	}

  function SwapMLKtoBMXP(uint256 tokenQty) public payable
	{
    require(tokenQty>=MINIMUM_SALE,"Invalid minimum quatity");
		require(tokenQty<=MAXIMUM_SALE,"Invalid maximum quatity");
    require(MLK.balanceOf(msg.sender)>=tokenQty,"Low Token Balance In Wallet");
    uint256 BMXP_amt=(tokenQty*mlkBmxpPrice)/1e18; 
    uint256 FeeAmt=BMXP_amt*(SellFee/100)/100;  
    uint256 NetBMXPAmt=BMXP_amt-FeeAmt;
    require(NetBMXPAmt>0,"Invalid buy amount");
    require(BMXP.balanceOf(address(this))>=tokenQty,"Low BUSD Balance In Contract");
		MLK.transferFrom(msg.sender,address(this), tokenQty);
		BMXP.transfer(msg.sender , NetBMXPAmt);
     emit Swap(msg.sender,"MLK","BMXP",tokenQty,mlkBmxpPrice,NetBMXPAmt);
	}

  
  function Buy_setting(uint min_buy, uint max_buy, uint min_sell,uint max_sell) public payable
  {
      require(msg.sender==owner,"Only Owner");
        MINIMUM_BUY = min_buy ;
        MAXIMUM_BUY = max_buy;
        MINIMUM_SALE = min_sell ;
        MAXIMUM_SALE = max_sell;

  }

    function Price_setting(uint256 _bmxpPrice,uint256 _mlkPrice,uint256 _mlkBmxpPrice) public payable
    {
        require(msg.sender==owner,"Only Owner");
        bmxpUsdPrice=_bmxpPrice;
        mlkUsdPrice=_mlkPrice;
        mlkBmxpPrice=_mlkBmxpPrice;
    }
    function Fee_setting(uint256 _buyFee,uint256 _sellFee) public payable
    {
        require(msg.sender==owner,"Only Owner");
        BuyFee=_buyFee;
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
        _TOKEN.transfer(owner,(QtyAmt*1e18));
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