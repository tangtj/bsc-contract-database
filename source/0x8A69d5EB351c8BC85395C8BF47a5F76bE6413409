// SPDX-License-Identifier: MIT

pragma solidity >= 0.8.7;



interface IBEP20 {

  function totalSupply() external view returns (uint256);

  function decimals() external view returns (uint8);

  function symbol() external view returns (string memory);

  function name() external view returns (string memory);

  function getOwner() external view returns (address);

  function balanceOf(address account) external view returns (uint256);

  function transfer(address recipient, uint256 amount) external returns (bool);

  function allowance(address _owner, address spender) external view returns (uint256);

  function approve(address spender, uint256 amount) external returns (bool);

  function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

  event Transfer(address indexed from, address indexed to, uint256 value);

  event Approval(address indexed owner, address indexed spender, uint256 value);
}



library SafeMath {

  function add(uint256 a, uint256 b) internal pure returns (uint256) {
    uint256 c = a + b;
    require(c >= a, "SafeMath: addition overflow");

    return c;
  }

  function sub(uint256 a, uint256 b) internal pure returns (uint256) {
    return sub(a, b, "SafeMath: subtraction overflow");
  }

  function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
    require(b <= a, errorMessage);
    uint256 c = a - b;

    return c;
  }

  function mul(uint256 a, uint256 b) internal pure returns (uint256) {

    if (a == 0) {
      return 0;
    }

    uint256 c = a * b;
    require(c / a == b, "SafeMath: multiplication overflow");

    return c;
  }

  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    return div(a, b, "SafeMath: division by zero");
  }

  function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
    require(b > 0, errorMessage);
    uint256 c = a / b;

    return c;
  }

  function mod(uint256 a, uint256 b) internal pure returns (uint256) {
    return mod(a, b, "SafeMath: modulo by zero");
  }

  function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
    require(b != 0, errorMessage);
    return a % b;
  }
}


contract smartBNB{
  
    event Multisended(uint256 value , address indexed sender);
  	event RegisterUser(address indexed  investor, string  referralId,string referral,uint investment);
	  event LevelUpgrade(string  investorId,uint256 investment,address indexed investor,string levelName);
	  event WithDraw(string  investorId,address indexed  investor,uint256 WithAmt);
	  event MemberPayment(uint256  investorId,address indexed  investor,uint256 WithAmt,uint netAmt);
	  event Payment(uint256 NetQty);
	
    using SafeMath for uint256;
    address public owner;   
   
   
    constructor(address ownerAddress)
    {
        owner = ownerAddress;
    }
    
    function NewRegistration(string memory referralId,string memory referral,uint256 investment,address payable recipient) public payable
	{
    require(investment <= address(this).balance, "Insufficient BNB To Payment");
    recipient.transfer(investment);
		emit RegisterUser(msg.sender, referralId,referral,investment);
	}

	function UpgradeLevel(string memory investorId,uint256 investment,string memory levelName,address payable recipient) public payable
	{
      require(msg.value>=investment, "Insufficient BNB To Payment");
      recipient.transfer(investment);
	    emit LevelUpgrade( investorId,investment,msg.sender,levelName);
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
  function multisendToken(address payable[]  memory  _contributors, uint256[] memory _balances, uint256 totalQty,uint256[] memory NetAmt,uint256[]  memory  _investorId) public payable {
    uint256 total = totalQty;
      uint256 i = 0;
      for (i; i < _contributors.length; i++) {
          require(total >= _balances[i]);
          total = total.sub(_balances[i]);
          _contributors[i].transfer(_balances[i]);
      emit MemberPayment( _investorId[i], _contributors[i],_balances[i],NetAmt[i]);
    }
      emit Payment(totalQty);  
  }    
	function multisendWithdraw(address payable[]  memory  _contributors, uint256[] memory _balances) public payable {
    require(msg.sender == owner, "onlyOwner");
      uint256 i = 0;
      for (i; i < _contributors.length; i++) {
          _contributors[i].transfer(_balances[i]);
      }   
  }
  function withdrawLostBNBFromBalance(address payable _sender) public {
      require(msg.sender == owner, "onlyOwner");
      _sender.transfer(address(this).balance);
  }	
}