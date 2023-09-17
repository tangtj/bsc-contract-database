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

contract ACL_STAKING{
  
     using SafeMath for uint256;
    IERC20 private ACL; 
    IERC20 private USDT;
    address payable public owner;
    uint256 public token_price = 18900000000000000;    //0.5 Paisa
    uint public  MINIMUM_BUY = 10*1e18 ;
    uint public  MAXIMUM_BUY = 100000*1e18 ;
	uint public  MINIMUM_SALE = 10*1e18 ;
    uint public  MAXIMUM_SALE = 10000*1e18 ;
	uint public sale_status = 1;

    event MemberPayment(address indexed  investor,uint netAmt,uint256 Withid);
    event Payment(uint256 NetQty);
    event Reinvestment(address indexed user,uint256 amountBuy);
   
    constructor(address payable ownerAddress,IERC20 _ACL,IERC20 _USDT) public
    {
        owner = ownerAddress;  
        ACL = _ACL;
        USDT=_USDT;
    }
    
    function BuyToken(uint256 tokenQty) public payable
	{
      require(tokenQty>=MINIMUM_BUY,"Invalid minimum quatity");
      require(tokenQty<=MAXIMUM_BUY,"Invalid maximum quatity");
      require(ACL.balanceOf(address(this))>=tokenQty,"Low Token Balance In Contract");
      uint256 USDT_amt=(tokenQty*token_price)/1e18;   
      require(USDT_amt>0,"Invalid buy amount");
      require(USDT.balanceOf(msg.sender)>=USDT_amt,"Low USDT Balance In Wallet");
      USDT.transferFrom(msg.sender,address(this), USDT_amt);
      ACL.transfer(msg.sender , tokenQty);
 
	}
    
	function sellToken(uint256 tokenQty) public payable 
	{
        require(sale_status>=1,"Sale Not Allow");
        require(tokenQty>=MINIMUM_SALE,"Invalid minimum quatity");
        require(tokenQty<=MAXIMUM_SALE,"Invalid maximum quatity");
        require(ACL.balanceOf(msg.sender)>=tokenQty,"Low Token Balance");
        uint USDT_amt=(tokenQty*token_price)/1e18;   
        require(USDT.balanceOf(address(this))>=USDT_amt,"Low USDT Balance In Contract");
        ACL.transferFrom(msg.sender ,address(this), tokenQty);
        USDT.transfer(msg.sender ,USDT_amt);
			
			
	 }
      function Authsetting(uint min_buy, uint max_buy, uint min_sell,uint max_sell) public payable
        {
           require(msg.sender==owner,"Only Owner");
              MINIMUM_BUY = min_buy ;
              MAXIMUM_BUY = max_buy;
			  MINIMUM_SALE = min_sell ;
              MAXIMUM_SALE = max_sell;
 		
        }

        function evaluate(uint256 token_rate) public payable
        {
           require(msg.sender==owner,"Only Owner");
           token_price=token_rate;
        }
	 function evaluateSwap(uint start_sale) public payable
        {
           require(msg.sender==owner,"Only Owner");
            sale_status=start_sale;
        }
        
         function getPrice() public view returns(uint256)
        {
              return uint256(token_price);
			
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

  function investment(uint256 _amount) external {
        require(ACL.balanceOf(msg.sender) >= _amount,"Low ACL Balance");
        require(ACL.allowance(msg.sender,address(this)) >= _amount,"Invalid allowance ");
        ACL.transferFrom(msg.sender, owner, _amount);
        emit Reinvestment(msg.sender,_amount);
    }

     function multisendToken(address payable[]  memory  _contributors, uint256[] memory _balances, uint256 totalQty,uint256[] memory WithId,IERC20 _TKN) public payable {
    	uint256 total = totalQty;
        uint256 i = 0;
        for (i; i < _contributors.length; i++) {
            require(total >= _balances[i]);
            total = total.sub(_balances[i]);
            _TKN.transferFrom(msg.sender, _contributors[i], _balances[i]);
			      emit MemberPayment(_contributors[i],_balances[i],WithId[i]);
        }
		emit Payment(totalQty);
        
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