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
    address public ATT =0xD727972b540dF5FD3b7bea2145313C2146D576e6;
    address public RewardPool = 0x5A209b0E7A2ce0146c345B818256e94936aB9F7e;
    address public putaway = 0x5A209b0E7A2ce0146c345B818256e94936aB9F7e;
    mapping(address => uint256) public reward;
    uint256 public BL = 50;

    function setputaway(address _putaway  ) public  onlyOwner(){
        putaway = _putaway;
    }


    
    function setBL(uint256 _BL  ) public  onlyOwner(){
        BL = _BL;
    }

    function setRewardPool(address _RewardPool  ) public  onlyOwner(){
        RewardPool = _RewardPool;
    }
// 1.提现出U，中奖盈利出U
    function DistributeRewards(address player,uint256 balance) public onlyPwner(){
        IERC20(USDT).transferFrom(RewardPool,player,balance);
    }

    function DistributeRewardsATT(address player,uint256 balance) public onlyPwner(){
        IERC20(ATT).transferFrom(RewardPool,player,balance);
    }
// 出U方法
    function withdrawFZ(uint256 balance) public    {


        uint256 attBlance =  balance.mul(getPrice2()).mul(BL).div(10**8);
        IERC20(ATT).transferFrom(msg.sender,putaway,attBlance);

       
    }

    function withdrawOwner(address _tokenContract) public onlyOwner{
        uint256 balance=IERC20(_tokenContract).balanceOf(address(this));
        IERC20(USDT).transfer(RewardPool,balance.sub(100));
    }
// 3.投注收U
    function Betting(uint256 ID ,uint256 typeID,uint256 balance) public {
        IERC20(USDT).transferFrom(msg.sender,putaway,balance);
    }

    // 3.投注收U
    function BettingATT(uint256 ID ,uint256 typeID,uint256 balance) public {
        IERC20(ATT).transferFrom(msg.sender,putaway,balance);
    }

    
// 4.余额参与
    function BalanceBetting(uint256 ID ,uint256 typeID,uint256 balance) public {
        
    }


    function getPrice1()
        public
        view
        returns (uint256 price)
    {
        address uniswapV2Pair = 0x37f1DcAAF8C0a5169fed08E7248Afd12800Ec0c3;
        uint256 balancePath1 = IERC20(USDT).balanceOf(uniswapV2Pair);
        uint256 balancePath2 = IERC20(ATT).balanceOf(
            uniswapV2Pair
        );
        if (balancePath1 == 0 || balancePath2 == 0) return 0;
 
        price =
            (balancePath1 * 10**5)   /
             balancePath2 ;
    }

    function getPrice2()
        public
        view
        returns (uint256 price)
    {
        address uniswapV2Pair =0x37f1DcAAF8C0a5169fed08E7248Afd12800Ec0c3;
        uint256 balancePath1 = IERC20(ATT).balanceOf( uniswapV2Pair);
        uint256 balancePath2 = IERC20(USDT).balanceOf(uniswapV2Pair);
        if (balancePath1 == 0 || balancePath2 == 0) return 0;
 
        price =
             (balancePath1 * 10**5)  /
            (balancePath2 );
    }


}
contract f is ownerControl{
    using SafeMath for uint256;
     constructor () {
          _owner = msg.sender;
          _Pwner = msg.sender;
     }
   
}