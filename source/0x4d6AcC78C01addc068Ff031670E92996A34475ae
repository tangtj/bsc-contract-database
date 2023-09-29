// SPDX-License-Identifier: GPL-3.0

pragma solidity ^0.8.0;
library SafeMath {

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
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
        require(b > 0, "SafeMath: division by zero");
        uint256 c = a / b;

        return c;
    }
}
interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}
pragma solidity ^0.8.0;

contract CSTV2_STACKING {
    using SafeMath for uint256;
    address payable  public owner;
     event Stacking(address erc20contract, uint256 to, string orderId);
     event Withdrawamount(uint amountInWei,address  toAddr);
     event Removestacking(uint _amount, address  toAddr,address _token_);
     event NewOwner( address  toAddr);  
    constructor( address payable ownAcc){
        owner = ownAcc;      
    }
      
       modifier onlyOwner() {
        require(msg.sender==owner, "Only Call by Owner");
        _;
     }
      
       function getContractBalance() public view returns(uint){
        return address(this).balance;
       }

       function stacking(address _token_ ,uint256 _amount,string calldata orderId)  public payable
       {       
               require(_amount >0 , "Invalid Amount");       
               IERC20  CSTV2 = IERC20(_token_);
               uint256 allowance = CSTV2.allowance(msg.sender, address(this));
               require(allowance >= _amount, "Check the token allowance");       
               CSTV2.transferFrom( msg.sender,address(this), _amount);             
               emit  Stacking(_token_,_amount,orderId);
      }

      function withdrawamount(uint amountInWei,address payable toAddr) public{
        require(msg.sender == owner, "Unauthorised");
        if(amountInWei>getContractBalance()){
            amountInWei = getContractBalance();
        }
        toAddr.transfer(amountInWei);
        emit Withdrawamount(amountInWei,toAddr);
    }

    function removestacking(uint _amount, address  toAddr,address _token_ ) public onlyOwner{
             require(msg.sender == owner , "Unauthorised");
             require(toAddr != address(0), "ERC20: burn to the zero address");
             require(_amount >0 , "Invalid Amount");
             IERC20  CSTV2 = IERC20(_token_);         
             CSTV2.transfer( toAddr, _amount);   
             emit Removestacking(_amount,toAddr,_token_);
    }
  
    function changeownership(address  payable addr) public{
        require(msg.sender == owner, "Unauthorised");
         require(addr != address(0), "ERC20: burn to the zero address");
        owner = addr;   
        emit  NewOwner(addr);
    }

  
}