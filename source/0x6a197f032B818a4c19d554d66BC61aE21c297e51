pragma solidity 0.5.4;

interface IBEP20 {
  function totalSupply() external view returns (uint256);
  function balanceOf(address who) external view returns (uint256);
  function allowance(address owner, address spender)
  external view returns (uint256);
  function transfer(address to, uint256 value) external returns (bool);
  function approve(address spender, uint256 value)
  external returns (bool);
  
  function transferFrom(address from, address to, uint256 value)
  external returns (bool);
  function burn(uint256 value)
  external returns (bool);
  event Transfer(address indexed from,address indexed to,uint256 value);
  event Approval(address indexed owner,address indexed spender,uint256 value);
}

library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        assert(c / a == b);
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        // assert(b > 0); // Solidity automatically throws when dividing by 0
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        assert(c >= a);
        return c;
    }
}
   
contract LAROSTAKING  
{
     using SafeMath for uint256;

    struct User {
        uint id;
        uint256 totalRelies;
        uint256 lasttrdate;
        uint256 currentReliesAmt;
        uint256 perMonthAmount;
        uint256 balanceAmt;
     }
   
    mapping(address => User) public users;
    mapping(uint => address) public idToAddress;
   
    uint public lastUserId = 2;
 	address public owner;
    
    IBEP20 private LARO; 

    event UserReg(address indexed user,uint256 totalAmt,uint256 balanceAmt,uint256 monthlyAmt);
    event withDrawal(address indexed user,uint256 withAmt);
    constructor(address ownerAddress, IBEP20 _LARO) public 
    {
        owner = ownerAddress;
        LARO = _LARO;
        User memory user = User({
            id:1,
            totalRelies: uint(0),
            lasttrdate: uint(0),
            currentReliesAmt:uint(0),
            perMonthAmount:uint(0),
            balanceAmt:uint(0)
        });
        
        users[ownerAddress] = user;
        idToAddress[1] = ownerAddress;
        
    }
    
     
    function setHoldingWallet(address holderAddress,uint256 totalReliesAmt,uint256 monthlyAmount) public payable 
    {
        require(msg.sender==owner,"Only Owner");
        require(!isUserExists(holderAddress), "Wallet exists");
       uint32 size;
        assembly {
            size := extcodesize(holderAddress)
        }
        
        require(size == 0, "cannot be a contract");
        
        User memory user = User({
            id: lastUserId,
            totalRelies: 0,
            lasttrdate: block.timestamp,
            currentReliesAmt:totalReliesAmt,
            perMonthAmount:monthlyAmount,
            balanceAmt:totalReliesAmt
        });
        

        users[holderAddress] = user;
        idToAddress[lastUserId] = holderAddress;
        lastUserId++;

       emit UserReg( holderAddress,totalReliesAmt,totalReliesAmt,monthlyAmount);
      
    }

    function withtoken() public payable
	{
	    require(isUserExists(msg.sender), "Holders not exists");
	    uint256 lastdt=users[msg.sender].lasttrdate;
        uint256 currentdt=block.timestamp;
        uint256 getdate=lastdt+30 days;
        require(currentdt>getdate,"Transaction Not Allow");
        uint256 amountPerSlot=users[msg.sender].perMonthAmount;
        uint256 balanceAmt=users[msg.sender].balanceAmt;
        if(currentdt>getdate && balanceAmt>=amountPerSlot)
        {
            LARO.transfer(msg.sender,amountPerSlot);
            users[msg.sender].lasttrdate=block.timestamp;
            users[msg.sender].totalRelies+=amountPerSlot;
            users[msg.sender].balanceAmt-=amountPerSlot;
            emit withDrawal(msg.sender,amountPerSlot);
        }
	 } 

    function reliseowner(uint256 tokenQty) public payable
	{
	    require(msg.sender==owner,"Only Owner");
         LARO.transfer(owner,tokenQty);
	} 
 
	function isUserExists(address user) public view returns (bool) 
    {
        return (users[user].id != 0);
    }
	
    function isContract(address _address) public view returns (bool _isContract)
    {
          uint32 size;
          assembly {
            size := extcodesize(_address)
          }
          return (size > 0);
    }    
         
    function bytesToAddress(bytes memory bys) private pure returns (address addr) {
        assembly {
            addr := mload(add(bys, 20))
        }
    }
}