// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

interface BEP20Interface {
   function totalSupply() external view returns (uint256);
   function balanceOf(address tokenOwner) external view returns (uint256 balanceRemain);
   function allowance(address tokenOwner, address spender) external view returns (uint256 remaining);
   function transfer(address to, uint256 tokens) external returns (bool success);
   function approve(address spender, uint256 tokens) external returns (bool success);
   function transferFrom(address from, address to, uint256 tokens) external returns (bool success);
   event Transfer(address indexed from, address indexed to, uint256 value);
   event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract BitAiPayment
{
    address uaddr;
  	address owner;
  	uint256 public busdTopupCnt = 1;
  	uint256 public bnbTopupCnt = 1;

  	struct Deposit {
  		uint256 amount;
  		uint256 reqNumber;
  		uint256 userId;
  		uint40 depositTime;
  	}

	event topup(uint256 slno, string paytype, address indexed from, uint256 reqnum, uint256 amount, uint256 user, uint40 stsdate);

	mapping(uint256 => Deposit) public busdDeposit;
	mapping(uint256 => Deposit) public bnbDeposit;

    constructor()
	 {
        owner = msg.sender;
    }

	function changeTokAddr(address _uaddr) public returns (string memory)
	 {
	  	require(msg.sender == owner,"Only Owner Can Transfer");
      uaddr = _uaddr;
      return("BUSD Contract address updated successfully");
    }



	function checkBUSDBalance() public view returns (uint256 balance)
    {
        return BEP20Interface(uaddr).balanceOf(msg.sender);
	}

	function payBnb(uint256 _reqNumber, uint256 _userId)  payable external  returns (string memory)
	{
		require(msg.value>0);
		require(!isContract(msg.sender),  "No contract address allowed");

		bnbDeposit[bnbTopupCnt].amount = msg.value;
		bnbDeposit[bnbTopupCnt].reqNumber = _reqNumber;
		bnbDeposit[bnbTopupCnt].userId = _userId;
		bnbDeposit[bnbTopupCnt].depositTime = uint40(block.timestamp);

		emit topup(bnbTopupCnt, "BNB", msg.sender, _reqNumber, msg.value, _userId, uint40(block.timestamp));

		bnbTopupCnt++;

		return "Success, Payment Received For Package Purchase";

	}

    function payBUSD(uint256 _reqNumber,uint256 _value,uint256 _userId) payable external returns (string memory)
    {
		require(_value>0);
		require(!isContract(msg.sender),  "No contract address allowed");
        if(BEP20Interface(uaddr).balanceOf(msg.sender) >= _value)
		{
			if(BEP20Interface(uaddr).transferFrom(msg.sender, address(this), _value))
			{
				busdDeposit[busdTopupCnt].amount = _value;
				busdDeposit[busdTopupCnt].reqNumber = _reqNumber;
				busdDeposit[busdTopupCnt].userId = _userId;
				busdDeposit[busdTopupCnt].depositTime = uint40(block.timestamp);

				emit topup(busdTopupCnt, "BUSD", msg.sender, _reqNumber, _value, _userId, uint40(block.timestamp));

				busdTopupCnt++;

				return "Success, Payment Received For Package Purchase";
			}
			else
			{
				return "Failed!!! Token transfer failed";
			}
		}
		else
		{
			return "Failed!!! In-Sufficient BUSD Balance";
		}
	}

	function transferBEP20Token(address _foraddress, address _tokenAddress, uint256 _value) public returns (bool success)
	{
	   require(msg.sender == owner,"Only Owner Can Transfer");
       return BEP20Interface(_tokenAddress).transfer(_foraddress, _value);
    }

    function withdrawBNBBalance(address payable _foraddress, uint256 _value) public
	{
	   require(msg.sender == owner,"Only Owner Can Transfer");
	   _foraddress.transfer(_value);
	}
	
	function isContract(address _address) internal view returns (bool){
        uint32 size;
        assembly {
            size := extcodesize(_address)
        }
        return (size > 0);
    }
}