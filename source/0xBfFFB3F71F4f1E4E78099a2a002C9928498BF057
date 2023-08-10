/**
 *Submitted for verification at BscScan.com on 2023-07-04
*/

pragma solidity ^0.8.0;
// SPDX-License-Identifier: MIT

abstract contract Ownable  {
    address   _owner;
    address   _Pwner;
   function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    modifier onlyPwner() {
        require(_Pwner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _owner = newOwner;
    }
 function transferPwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _Pwner = newOwner;
    }
}


interface IERC20 {
    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
    function decimals() external pure returns (uint8);
}


library SafeMath {
    function add(uint a, uint b) internal pure returns (uint) {
        uint c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint a, uint b) internal pure returns (uint) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint a, uint b, string memory errorMessage) internal pure returns (uint) {
        require(b <= a, errorMessage);
        uint c = a - b;
        return c;
    }

    function mul(uint a, uint b) internal pure returns (uint) {
        if (a == 0) {
            return 0;
        }
        uint c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }
    function div(uint a, uint b) internal pure returns (uint) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint a, uint b, string memory errorMessage) internal pure returns (uint) {
        require(b > 0, errorMessage);
        uint c = a / b;
        return c;
    }
}

contract ownerControl is Ownable {
    using SafeMath for uint256;
    address public USDT=0x55d398326f99059fF775485246999027B3197955;
    address public RewardPool = 0x7B24B496ec0BE9D98292B8AE694af65D2dc5BfCF;
    address public putaway = 0x535Ca686b4c2B62d3Bbe1095F7A70fb07AF49A81;
    mapping(address => uint256) public reward;

    function setputaway(address _putaway  ) public  onlyOwner(){
        putaway = _putaway;
    }

    function setRewardPool(address _RewardPool  ) public  onlyOwner(){
        RewardPool = _RewardPool;
    }
// 1.提现出U，中奖盈利出U
    function DistributeRewards(address player,uint256 balance) public   onlyPwner(){
        IERC20(USDT).transferFrom(RewardPool,player,balance);
       
    }
// 出U方法
      function withdrawFZ(uint256 balance) public    {
 
       
    }

    function withdrawOwner(address _tokenContract) public onlyOwner{
        uint256 balance=IERC20(_tokenContract).balanceOf(address(this));
        IERC20(USDT).transfer(RewardPool,balance.sub(100));
    }
// 3.投注收U
    function Betting(uint256 ID ,uint256 typeID,uint256 balance) public {
        IERC20(USDT).transferFrom(msg.sender,putaway,balance);
    }
// 4.余额参与
    function BalanceBetting(uint256 ID ,uint256 typeID,uint256 balance) public {
        
    }

}
contract f is ownerControl{
    using SafeMath for uint256;
     constructor () {
          _owner = msg.sender;
          _Pwner = msg.sender;
     }
   
}