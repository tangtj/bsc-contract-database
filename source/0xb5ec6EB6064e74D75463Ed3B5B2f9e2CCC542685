pragma solidity ^ 0.8.0;

// SPDX-License-Identifier: UNLICENSED

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

library SafeMath {
  
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }


    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

   
    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }

   
    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

   
    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

  
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

 
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

  
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

   
    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

   
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}

contract BITMAXPRO_STAKING {
    using SafeMath for uint256;
    event Multisended(uint256 value , address indexed sender);
    event WithDraw(address indexed  investor,uint256 WithAmt);
    event MemberPayment(address indexed  investor,uint netAmt,uint256 Withid);
    event Deposit(address indexed investor,uint256 package,uint256 tokenQty);
    event Registration(address indexed user,string referrer,string referrerId,uint256 package,uint256 tokenQty);
    event Payment(uint256 NetQty);
	

    IBEP20 private MLK; 
    address public owner;
    uint256 public tokenRate=2e16;  //0.02 BUSD
    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }

    constructor(address ownerAddress,IBEP20 _MLK) {
        owner = ownerAddress; 
        MLK = _MLK;
    }
    function registration(uint256 _amount,string memory refadd,string memory _referrerId ) external {
        uint256 MlkAmt=(_amount/tokenRate)*1e18;
        require(MLK.balanceOf(msg.sender) >= MlkAmt,"Low  Balance");
        require(MLK.allowance(msg.sender,address(this)) >= MlkAmt,"Invalid allowance");
        emit Registration(msg.sender,refadd,_referrerId,MlkAmt,_amount);
        emit Deposit(msg.sender,MlkAmt,_amount);
        MLK.transferFrom(msg.sender, owner, MlkAmt);
   	}

    function _Invest(uint256 _amount) external {
        uint256 MlkAmt=(_amount/tokenRate)*1e18;
        require(MLK.balanceOf(msg.sender) >= MlkAmt,"Low token Balance");
        require(MLK.allowance(msg.sender,address(this)) >= MlkAmt,"Invalid allowance");
        MLK.transferFrom(msg.sender, owner, MlkAmt);
        emit Deposit(msg.sender,_amount,MlkAmt);
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

    function withdrawToken(IBEP20 _token ,uint256 _amount) external onlyOwner {
        _token.transfer(owner,_amount);
    }

    function withdraw(uint256 _amount) external onlyOwner {
        payable(owner).transfer(_amount);
    }
  function changeRate(uint256 _tokenRate) external onlyOwner {
       tokenRate=_tokenRate;
    }
  
    function ChangeOwner(address _ownerAddress) external onlyOwner {
        owner=_ownerAddress;
    }
}