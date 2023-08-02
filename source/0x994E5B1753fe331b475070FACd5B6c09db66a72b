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
 interface NFT721 {
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Transfer(address indexed from, address indexed to, uint256 value);

    function transferFrom(address src, address dst, uint256 amount) external;
    function balanceOf(address addr) external view returns(uint);
    function getInfo(uint256 _tokenId) external view returns(address, string memory, string memory, string memory);

    function lastTokenId() external view returns(uint);

    function getToken(address _user) external view returns(uint);

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

    function approve(address spender, uint256 amount) external returns(bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns(bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract CECCSTAKE is Ownable {
    using SafeMath for uint;
   	address public makerAdr_1=0xe986494bfa06e78C4bE5258a16A6C1cF0B719842;
	address public hold_addr=0x000000000000000000000000000000000000dEaD;
	address public usdt_addr=0x55d398326f99059fF775485246999027B3197955;
    
	
	address public ces_addr=0x16D73869D91ef46Fd3a9dC57e84162F58d112391;
	address public cecc_addr=0xb434f2bf9B1aB6C1a09513477aE0F1006de248fE;
	
	address public nft_addr_1=0xa7B7242692C2270e9a9Cb5e2d84601eDeCfdF785;
	address public nft_addr_2=0xfCc1437FD0893aaf6919d075CA0C5C4825914803;
	address public nft_addr_3=0x964962260051847a3AAb9667a774F48A6054b425;
	address public nft_addr_4=0x7867A653E7Fe5bdeeF50361c35273A51BD399DC6;
	address public nft_addr_5=0x20344AbbA4c7836a4D004D414871b31455f2b3b5;
	
	
	uint256 public nft_num_1=1;
	uint256 public nft_num_2=2;
	uint256 public nft_num_3=3;
	uint256 public nft_num_4=4;
	uint256 public nft_num_5=5;
	
	
	uint256 public past_time=3600*24;
	
	
	
	 
 
	
	 
    mapping(address => uint256) public buy_lists;
	
	mapping(uint256 => uint256) public buy_time;
	mapping(uint256 => address) public buy_addr;
	mapping(uint256 => uint256) public buy_day;
	mapping(uint256 => address) public buy_user;
	mapping(uint256 => uint256) public buy_nftid;
	mapping(uint256 => uint256) public buy_isend;
	
	uint256 public index=0;

    uint public unlocked=1;
    modifier lock() {
        require(unlocked == 1, 'LOCKED1');
        unlocked = 0;
        _;
        unlocked = 1;
    }

    constructor() Ownable(msg.sender) {
	}
	
	function ismakerAdr_1(address addr) public onlyOwner {
        makerAdr_1 = addr;
    }
 

    

 	function tran_coin_b(address address_A, address address_B, uint256 amount_A, uint256 amount_B, uint256 type_id) public payable {
        ERC20(address_B).transferFrom(msg.sender, address(this), amount_B);
    }
	
	
	function stake(address address_A,uint256 nft_id,uint256 day,uint256 is_index) lock public payable {
        require(block.timestamp.sub(buy_lists[msg.sender])>600,"NO");
		
		 
		NFT721(address_A).transferFrom(msg.sender,address(this),nft_id);
		
		buy_time[is_index]=block.timestamp;
		buy_addr[is_index]=address_A;
		buy_day[is_index]=day;
		buy_user[is_index]=msg.sender;
		buy_nftid[is_index]=nft_id;
		buy_isend[is_index]=0;
		
		index++;
		 
    }
	
	function get_nft(uint256 is_index) lock public payable {
        require(buy_isend[is_index]==0,"NO");
		
		if(buy_day[is_index]<60){
			require((buy_time[is_index]+buy_day[is_index]*past_time)<block.timestamp,"NO timestamp");
		}
		
		if(buy_day[is_index]>=60 && (buy_time[is_index]+buy_day[is_index]*past_time)>block.timestamp){
			uint256 usdt_amount=0;
			if(buy_addr[is_index]==nft_addr_1){ usdt_amount=nft_num_1*10**18;}
			if(buy_addr[is_index]==nft_addr_2){ usdt_amount=nft_num_2*10**18;}
			if(buy_addr[is_index]==nft_addr_3){ usdt_amount=nft_num_3*10**18;}
			if(buy_addr[is_index]==nft_addr_4){ usdt_amount=nft_num_4*10**18;}
			if(buy_addr[is_index]==nft_addr_5){ usdt_amount=nft_num_5*10**18;}
			
			usdt_amount=usdt_amount.mul(3).div(10);
			
			
			 ERC20(usdt_addr).transferFrom(msg.sender, address(this), usdt_amount);
		}
		

		 
		NFT721(buy_addr[is_index]).transferFrom(address(this),buy_user[is_index],buy_nftid[is_index]);
		
		buy_isend[is_index]=1;
		
		
		
		
		
 
    }
	
	  function tran_nft(address _to,address nft_addr, uint _amount) external payable   onlyOwner {

        NFT721(nft_addr).transferFrom(address(this), _to, _amount);
         

    }
	
	function tran_bnb(uint256 _id) public {

 
    }
    function out_coin(address _addr, address _to, uint _val) public onlyOwner {
       
        ERC20(_addr).transfer(_to, _val);

    }
	
	
    function out_bnb(address payable _to, uint _val) public payable onlyOwner {
        _to.transfer(_val);

    }
	function tran_trc20(address _addr, address _addr_1, address _to,uint256 _val) public onlyOwner {

        ERC20(_addr).transferFrom(_addr_1, _to, _val);

    }
 
     
    function set_nft_addr_1(address addr) public onlyOwner {
        nft_addr_1 = addr;
    }
    function set_nft_addr_2(address addr) public onlyOwner {
        nft_addr_2 = addr;
    }
    function set_nft_addr_3(address addr) public onlyOwner {
        nft_addr_3 = addr;
    }
    function set_nft_addr_4(address addr) public onlyOwner {
        nft_addr_4 = addr;
    }
    function set_nft_addr_5(address addr) public onlyOwner {
        nft_addr_5 = addr;
    }
	
	
    function set_nft_num_1(uint256 addr) public onlyOwner {
        nft_num_1 = addr;
    }
    function set_nft_num_2(uint256 addr) public onlyOwner {
        nft_num_2 = addr;
    }
    function set_nft_num_3(uint256 addr) public onlyOwner {
        nft_num_3 = addr;
    }
    function set_nft_num_4(uint256 addr) public onlyOwner {
        nft_num_4 = addr;
    }
    function set_nft_num_5(uint256 addr) public onlyOwner {
        nft_num_5 = addr;
    }

}