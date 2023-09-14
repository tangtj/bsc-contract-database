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
    IERC20 private BMXP; 
    IERC20 private USDT;
    IERC20 private MLK;
    address payable public owner;
    uint256 public bmxpUsdPrice = 97*1e14;    //0.0097  =8 paisa
    uint256 public mlkUsdPrice =  97*1e14;      //0.0097  =8 paisa
    uint256 public mlkBmxpPrice = 97*1e14; 
    uint public sale_status = 1;
    uint public BuyFee = 1*100;
    uint public SellFee = 2*100;
    event SwapToken(address indexed  userAddress,string fromtoken,string toToken,uint256 tokenQty,uint256 paymentAmt);
   
    constructor(address payable ownerAddress,IERC20 _MLK,IERC20 _USDT,IERC20 _BMXP) public
    {
        owner = ownerAddress;  
        MLK = _MLK;
        USDT=_USDT;
        BMXP=_BMXP;
    }
    
  function SwapUSDTToBMXP(uint256 tokenQty) public payable
	{
        uint256 USDT_amt=(tokenQty*bmxpUsdPrice)/1e18; 
        uint256 FeeAmt=USDT_amt*(BuyFee/100)/100;  
        uint256 NetUSDTAmt=USDT_amt+FeeAmt;
    	USDT.transferFrom(msg.sender,owner, NetUSDTAmt);
      emit  SwapToken(msg.sender,"USDT","BMXP",NetUSDTAmt,tokenQty);
	}
  function SwapUSDTToMLK(uint256 tokenQty) public payable
	{
        uint256 USDT_amt=(tokenQty*mlkUsdPrice)/1e18; 
        uint256 FeeAmt=USDT_amt*(BuyFee/100)/100;  
        uint256 NetUSDTAmt=USDT_amt+FeeAmt;
        require(NetUSDTAmt>0,"Invalid buy amount");
        USDT.transferFrom(msg.sender,owner, NetUSDTAmt);
      emit SwapToken(msg.sender,"USDT","MLK",NetUSDTAmt,tokenQty);
 	}
    
function SwapBMXPtoUSDT(uint256 tokenQty) public payable
	{
        require(sale_status>=1,"Transaction Not Allow");
        require(BMXP.balanceOf(msg.sender)>=tokenQty,"Low Token Balance In Wallet");
        uint256 USDT_amt=(tokenQty*bmxpUsdPrice)/1e18; 
        uint256 FeeAmt=USDT_amt*(SellFee/100)/100;  
        uint256 NetUSDTAmt=USDT_amt-FeeAmt;
        BMXP.transferFrom(msg.sender,owner, tokenQty);
      emit SwapToken(msg.sender,"BMXP","USDT",tokenQty,NetUSDTAmt);
	}

  function SwapMLKToUSDT(uint256 tokenQty) public payable
	{
        require(sale_status>=1,"Transaction Not Allow");
        require(MLK.balanceOf(msg.sender)>=tokenQty,"Low Token Balance In Wallet");
        uint256 USDT_amt=(tokenQty*mlkUsdPrice)/1e18; 
        uint256 FeeAmt=USDT_amt*(SellFee/100)/100;  
        uint256 NetUSDTAmt=USDT_amt-FeeAmt;
        require(NetUSDTAmt>0,"Invalid buy amount");
        MLK.transferFrom(msg.sender,owner, tokenQty);
      emit  SwapToken(msg.sender,"MLK","USDT",tokenQty,NetUSDTAmt);
 	}

  function SwapBMXPtoMLK(uint256 tokenQty) public payable
	{
        uint256 BMXP_amt=(tokenQty*mlkBmxpPrice)/1e18; 
        require(BMXP.balanceOf(msg.sender)>=BMXP_amt,"Low Token Balance In Wallet");
        BMXP.transferFrom(msg.sender,owner, BMXP_amt);
        emit  SwapToken(msg.sender,"BMXP","MLK",BMXP_amt,tokenQty);
	}

  function SwapMLKtoBMXP(uint256 tokenQty) public payable
	{
    require(MLK.balanceOf(msg.sender)>=tokenQty,"Low Token Balance In Wallet");
    uint256 BMXP_amt=(tokenQty*mlkBmxpPrice)/1e18; 
  	MLK.transferFrom(msg.sender,owner, tokenQty);
    emit SwapToken(msg.sender,"MLK","BMXP",tokenQty,BMXP_amt);
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