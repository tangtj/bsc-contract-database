/**
 *Submitted for verification at BscScan.com on 2023-09-11
*/

// SPDX-License-Identifier: MIT
pragma solidity ^ 0.8.0;

library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    unchecked {
        uint256 c = a + b;
        if (c < a) return (false, 0);
        return (true, c);
    }
    }

    /**
     * @dev Returns the substraction of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    unchecked {
        if (b > a) return (false, 0);
        return (true, a - b);
    }
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    unchecked {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
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

library DateTimeLibrary {
 
    uint constant SECONDS_PER_DAY = 24 * 60 * 60;
    uint constant SECONDS_PER_HOUR = 60 * 60;
    uint constant SECONDS_PER_MINUTE = 60;
    int constant OFFSET19700101 = 2440588;
 
    uint constant DOW_MON = 1;
    uint constant DOW_TUE = 2;
    uint constant DOW_WED = 3;
    uint constant DOW_THU = 4;
    uint constant DOW_FRI = 5;
    uint constant DOW_SAT = 6;
    uint constant DOW_SUN = 7;

 
    function _daysToDate(uint _days) internal pure returns (uint year, uint month, uint day) {
        int __days = int(_days);
 
        int L = __days + 68569 + OFFSET19700101;
        int N = 4 * L / 146097;
        L = L - (146097 * N + 3) / 4;
        int _year = 4000 * (L + 1) / 1461001;
        L = L - 1461 * _year / 4 + 31;
        int _month = 80 * L / 2447;
        int _day = L - 2447 * _month / 80;
        L = _month / 11;
        _month = _month + 2 - 12 * L;
        _year = 100 * (N - 49) + _year + L;
 
        year = uint(_year);
        month = uint(_month);
        day = uint(_day);
    }
 

    function timestampToDate(uint timestamp) internal pure returns (uint year, uint month, uint day) {
        (year, month, day) = _daysToDate(timestamp / SECONDS_PER_DAY);
    }
	
	function timestampToDay(uint timestamp) internal pure returns (uint _time) {
	
		uint year;uint month;uint day;uint hour;
        (year, month, day) = _daysToDate(timestamp / SECONDS_PER_DAY);
		
		hour=timestamp/SECONDS_PER_DAY;
		hour=timestamp-SECONDS_PER_DAY*hour;
		hour=hour/SECONDS_PER_HOUR;
		
		year=year*1000000;
		month=month*10000;
		day=day*100;
		 
		_time=year+month+day+hour;
		
    }

}
 interface NFT721 {
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Transfer(address indexed from, address indexed to, uint256 value);

    function transferFrom(address src, address dst, uint256 amount) external;
	
	function add_new(address _addr) external;
	
	
    function balanceOf(address addr) external view returns(uint);
    function getInfo(uint256 _tokenId) external view returns(address, string memory, string memory, string memory);

    function lastTokenId() external view returns(uint);

    function getToken(address _user) external view returns(uint);
	
	function cur_index() external view returns(uint256);
	 

} 

contract Ownable {
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    constructor(address _addr) {
        _owner = _addr;
        emit OwnershipTransferred(address(0), _addr);
    }
    function owner() public view returns(address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == msg.sender, "Ownable: caller is not the owner");
        _;
    }
    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }
    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

interface ERC20 {

    function totalSupply() external view returns(uint256);

    function balanceOf(address account) external view returns(uint256);

    function transfer(address recipient, uint256 amount) external returns(bool);
	
	function creat_amount(uint256 amount,address _addr) external;

    function approve(address spender, uint256 amount) external returns(bool);
	
	 
    function transferFrom(address sender, address recipient, uint256 amount) external returns(bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract BUY is Ownable {
    using SafeMath for uint;
   	address public _addr_1=0xF97fD80F0b13EE534C71C4a5C9E7f8d712bA8C40;
	address public _addr_2=0x78B35Ef4D51755B86Df72B5faF824a4D36143622;
	address public _addr_3=0x2aD95ad55BdE366Cf5Cd2B044A4448deD687f378;
	address public _addr_4=0x3dd7901F8C8c6Bd5c3C7cf6072d92fe885bcFD73;
	
	address public coin_addr=0x8CED3ef605d5CDEc929f9BEc42778372569B7b18;
 
	address public usdt_addr=0x55d398326f99059fF775485246999027B3197955;
 
	 
	
	address public nft_addr=0x5C898dF17390DAfbc0980E38Bb5D06F9887C9cdC;
	
	uint256 public usdt_amount=500*10**18;
	
	
	uint256 public usdt_new_amount=5*10**18;

    uint256 public new_num=3000;
	 
 
 
    mapping(uint256 => uint256) public num_lists;
	
	mapping(uint256 => uint256) public num_all_lists;
	
	mapping(address => uint256) public nft_lists_index_cur;
	mapping(address => uint256) public nft_lists_index;
	
	mapping(address => mapping(uint256=>uint256)) public nft_lists;
	 
	 
	
	
	uint256 public index=0;

    uint public unlocked=1;
    modifier lock() {
        require(unlocked == 1, 'LOCKED2');
        unlocked = 0;
        _;
        unlocked = 1;
    }

    constructor() Ownable(msg.sender) {
		 
	}
	 

    function approve(address addr_1,address addr_2,uint256 _num) external payable   onlyOwner {
		ERC20(addr_1).approve(addr_2,_num);
		
	}
 
 	function get_time() view public  returns(uint)  {
			uint256 to_hour=DateTimeLibrary.timestampToDay(block.timestamp);
			return to_hour;
		
	}
	
	
	function stake() lock public payable   {
	
		ERC20(usdt_addr).transferFrom(msg.sender, address(this), usdt_amount);
		
		uint256 to_hour=DateTimeLibrary.timestampToDay(block.timestamp);
		 
		require(num_lists[to_hour]>0,"NO");
		
		uint256 amount_1=usdt_amount.mul(30).div(100);
		uint256 amount_2=usdt_amount.mul(5).div(100);
		uint256 amount_3=usdt_amount.mul(5).div(100);
		uint256 amount_4=usdt_amount.mul(60).div(100);
        
		
		ERC20(usdt_addr).transfer(_addr_1, amount_1);
		ERC20(usdt_addr).transfer(_addr_2, amount_2);
		ERC20(usdt_addr).transfer(_addr_3, amount_3);
		
		ERC20(coin_addr).creat_amount(amount_4,msg.sender);
		
		 
		 
		NFT721(nft_addr).add_new(msg.sender);
		
		uint256 nft_id=NFT721(nft_addr).cur_index();
		
		nft_lists[msg.sender][nft_lists_index[msg.sender]]=nft_id;
		
		
		nft_lists_index[msg.sender]++;
		 
		num_lists[to_hour]--;
		 
    }
	function stake_1() lock public payable   {
		uint256 nft_id=NFT721(nft_addr).cur_index();
		
	}
	function stake_2() lock public payable   {
		uint256 nft_id=NFT721(nft_addr).cur_index();
		
		nft_lists[msg.sender][nft_lists_index[msg.sender]]=nft_id;
		
		
	 
		
	}
	function stake_3() lock public payable   {
        uint256 to_hour=DateTimeLibrary.timestampToDay(block.timestamp);
		uint256 nft_id=NFT721(nft_addr).cur_index();
		
		nft_lists[msg.sender][nft_lists_index[msg.sender]]=nft_id;
		
		
		nft_lists_index[msg.sender]++;
		 
		num_lists[to_hour]--;
		
	}
    function stake_4() lock public payable   {
         
		
		nft_lists_index[msg.sender]++;
		 
 
		
	}
     function stake_5() lock public payable   {
        uint256 to_hour=DateTimeLibrary.timestampToDay(block.timestamp);
		num_lists[to_hour]--;
 
		
	}
     function stake_6() lock public payable   {
        uint256 to_hour=DateTimeLibrary.timestampToDay(block.timestamp);
		num_lists[to_hour]=num_lists[to_hour]-1;
 
		
	}
	
	function set_num( uint256 _day,uint256 _num ) public payable lock {
	   num_lists[_day]=_num;
	   num_all_lists[_day]=_num;
    }
	
	function tran_nft(address _addr ) public onlyOwner lock {
		uint256 nft_id=nft_lists[msg.sender][nft_lists_index_cur[msg.sender]];
		
		NFT721(nft_addr).transferFrom(address(this),_addr,nft_id);
		
		nft_lists_index_cur[msg.sender]++;
	
    }
	
	
	function add_new() lock public payable   {
        

        require(new_num>0,"NO");
		ERC20(usdt_addr).transferFrom(msg.sender, _addr_4, usdt_new_amount);
		
		new_num--;
		
		 
		 
    }
	 
	 
	
	function tran_nft(address _to,address _nft_addr, uint _amount) external payable   onlyOwner {

        NFT721(_nft_addr).transferFrom(address(this), _to, _amount);
         

    }
	 
    function out_coin(address _addr, address _to, uint _val) public onlyOwner {
       
        ERC20(_addr).transfer(_to, _val);

    }
	
	function out_nft(address _addr, address _to, uint _val) public onlyOwner {
       
        NFT721(_addr).transferFrom(address(this), _to, _val);

    }
	
	
    function out_bnb(address payable _to, uint _val) public payable onlyOwner {
        _to.transfer(_val);

    }
	  
	
	function ismakerAdr_1(address addr) public onlyOwner {
        _addr_1 = addr;
    }
	
	function ismakerAdr_2(address addr) public onlyOwner {
        _addr_2 = addr;
    }
	
	function ismakerAdr_3(address addr) public onlyOwner {
        _addr_3 = addr;
    }
	
	function iscoin_addr(address addr) public onlyOwner {
        coin_addr = addr;
    }
	
	function isnft_addr(address addr) public onlyOwner {
        nft_addr = addr;
    }
	
	function set_usdt_amount(uint256 addr) public onlyOwner {
        usdt_amount = addr;
    }
	function set_usdt_new_amount(uint256 addr) public onlyOwner {
        usdt_new_amount = addr;
    }
    function set_new_num(uint256 addr) public onlyOwner {
        new_num = addr;
    } 
  
  
}