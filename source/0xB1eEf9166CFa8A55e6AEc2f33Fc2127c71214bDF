pragma solidity >= 0.5.0;

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

contract SaveGreenStaking{
  
  event  Registration(string  sponcer_id,address indexed sponsor,uint256 investment,uint256 tokenAmt,address indexed member_name,string position);
  event Staking(string  investorId,uint256 investment,uint256 tokenAmt,address indexed investor);
	event WithDraw(string  WithId,address indexed  investor,uint256 WithAmt);
	event WithDrawMulti(uint256  investorId,address indexed  investor,uint256 WithAmt,uint256 WithId);
	event MemberPayment(address indexed  investor,uint netAmt,uint256 Withid);
    event Payment(uint256 NetQty);

    using SafeMath for uint256;
    IBEP20 private GRV; 
    address public owner;
    uint256 public grvPrice=5*1e7;  //0.5 USDT
   
    constructor(address ownerAddress,IBEP20 _GRV) public
    {
        owner = ownerAddress;  
        GRV = _GRV;
     }
     	function userRegister(string memory referrerId,address referral,uint256 package,string memory position) public payable
	{
	  uint tokenAmt=(package/grvPrice)*1e8;
    require(GRV.balanceOf(msg.sender)>=tokenAmt);
		require(GRV.allowance(msg.sender,address(this))>=tokenAmt,"Approve Your Token First");
		GRV.transferFrom(msg.sender ,owner,tokenAmt);
		emit Registration(referrerId,referral,package,tokenAmt,msg.sender,position);
	}

 	function Invest(string memory investorId,uint256 package) public payable
	{
	  uint tokenAmt=(package/grvPrice)*1e8;
    require(GRV.balanceOf(msg.sender)>=tokenAmt);
		require(GRV.allowance(msg.sender,address(this))>=tokenAmt,"Approve Your Token First");
		GRV.transferFrom(msg.sender ,owner,tokenAmt);
		emit Staking( investorId,package,tokenAmt,msg.sender);
	}

  function getPrice() public view returns(uint256)
        {
              return uint256(grvPrice);
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
    
      function multisendToken(address payable[]  memory  _contributors, uint256[] memory _balances, uint256 totalQty,uint256[] memory WithId,IBEP20 _TKN) public payable {
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
    
    
    function withdrawLostBNBFromBalance(address payable _sender) public {
        require(msg.sender == owner, "onlyOwner");
        _sender.transfer(address(this).balance);
    }
    
    function withdrawincome(string memory WithId,address payable _userAddress,uint256 WithAmt) public {
         GRV.transferFrom(msg.sender,_userAddress, WithAmt);
        emit WithDraw(WithId,_userAddress,WithAmt);
    }
     
	function withdrawLostTokenFromBalance(uint QtyAmt,IBEP20 _TKN) public 
	{
        require(msg.sender == owner, "onlyOwner");
        _TKN.transfer(owner,QtyAmt);
	}
	
  function changePrice(uint256 _grvPrice) public 
	{
        require(msg.sender == owner, "onlyOwner");
        grvPrice=_grvPrice;
	}
	function changeowner(address _newowner) public 
	{
        require(msg.sender == owner, "onlyOwner");
		owner = _newowner;
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