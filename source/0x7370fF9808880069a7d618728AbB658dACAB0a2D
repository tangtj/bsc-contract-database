/**
 *Submitted for verification at BscScan.com on 2023-08-24
*/

/**
 *Submitted for verification at BscScan.com on 2023-08-19
*/

/* SPDX-License-Identifier: SimPL-2.0*/
pragma solidity >=0.6.2;

interface tokenRecipient { function receiveApproval(address _from, uint256 _value, address _token, bytes memory _extraData) external; }

contract Owner {
    address private owner;
    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor()  {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "only Owner");
        _;
    }
   
}	
library SafeMath {
    function add(uint256 x, uint256 y) internal pure returns(uint256 z) {
        require((z = x + y) >= x, 'ds-math-add-overflow');
    }

    function sub(uint256 x, uint256 y) internal pure returns(uint256 z) {
        require((z = x - y) <= x, 'ds-math-sub-underflow');
    }

    function mul(uint256 x, uint256 y) internal pure returns(uint256 z) {
        require(y == 0 || (z = x * y) / y == x, 'ds-math-mul-overflow');
    }
    function div(uint256 a, uint256 b) internal pure returns(uint256) {
        return div(a, b, "SafeMath: division by zero");
    }	

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns(uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
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

    function _daysToDate(uint _days) internal pure returns(uint year, uint month, uint day) {
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

    function timestampToDate(uint timestamp) internal pure returns(uint day_str) { 
        uint year;
        uint month;
        uint day;
		(year, month, day) = _daysToDate(timestamp / SECONDS_PER_DAY);
		
		day_str=year*100*100+month*100+day;
    }

}
interface IERC20 {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external view returns(string memory);
    function symbol() external view returns(string memory);
    function decimals() external view returns(uint8);
    function totalSupply() external view returns(uint);
    function balanceOf(address owner) external view returns(uint);
    function allowance(address owner, address spender) external view returns(uint);

    function approve(address spender, uint value) external returns(bool);
    function transfer(address to, uint value) external returns(bool);
    function transferFrom(address from, address to, uint value) external returns(bool);
} 
 
contract AnyantsAirdrop is Owner {
	 
    using SafeMath
    for uint;
    
	 
 	struct DataArr {
		address user_address;
        address coin_address;
        uint amount;
		uint apply_time_start;
		uint apply_time_end;
		uint release_time_start;
		uint release_time_end;
		uint pre_amount;
		uint is_white;
		uint end_type;
		uint auth_type;
    }
	struct ReleaseArr {
		uint DataId;
        uint time_start;
		uint pre_amount;
		 
    }
	struct UserAmountArr {
		uint DataId;
        address user_address;
        uint amount;
    }
	
	uint public DataId=0;
	uint ReleaseId=0;
	mapping(uint =>DataArr) public DataLists; 
	mapping(uint =>ReleaseArr) public ReleaseLists; 
	
	mapping(address => mapping(uint256 => uint256)) public DataAddressLists; 
	mapping(address => mapping(uint256 => uint256)) public DataCoinLists; 
	mapping(address => mapping(uint256 => uint256)) public DataUserLists; 
	
	
	mapping(uint => mapping(uint256 => address)) public AddressLists; 

    mapping(address => mapping(address => uint256)) public CoinAddressAmount; 

	mapping(uint256 => mapping(address => uint256)) public DataUserAmountById; 
	
	mapping(uint256 => uint256) public DataGetAmountById; 
	
	mapping(uint256 => uint256) public DataIsendById; 
    
	mapping(uint =>address) public ReturnAddressById; 

      

	
	mapping(address =>uint) public DataAddressLength; 
	mapping(address =>uint) public DataCoinLength; 
	mapping(address =>uint) public DataUserLength; 
	mapping(uint =>uint) public AddressLength; 
    mapping(uint =>uint) public AddressListsIndex; 

     
	
	
	uint public unlocked=1; 
    modifier lock() {
        require(unlocked == 1, 'LOCKED');
        unlocked = 0;
        _;
        unlocked = 1;
    }
	uint public unlocked_2=1;
	modifier lock_2() {
        require(unlocked_2 == 1, 'LOCKED');
        unlocked_2 = 0;
        _;
        unlocked_2 = 1;
    }
	
	function add(address coin_address,address return_address,uint data_id,uint[] memory num_lists)   external lock returns (bool success) {
		 
		 
		IERC20(coin_address).transferFrom(msg.sender,address(this), num_lists[0]);
		DataLists[DataId]=DataArr(msg.sender,coin_address,num_lists[0],num_lists[1],num_lists[2],num_lists[3],num_lists[4],num_lists[5],num_lists[6],num_lists[7],num_lists[8]);
		 
		
		uint _AddressLength=DataAddressLength[msg.sender];
		uint _CoinLength=DataCoinLength[coin_address];
		DataAddressLists[msg.sender][_AddressLength]=DataId;
		DataCoinLists[coin_address][_CoinLength]=DataId;
		
		
		ReturnAddressById[DataId]=return_address;
		
		 
		DataAddressLength[msg.sender]++;
		DataCoinLength[coin_address]++;
        DataId++;
        return true;
    }
	
		 
 	 
	
	
	function AddAddressLists(uint _DataId,address[] memory address_lists) external{
		DataArr memory _detail=DataLists[_DataId];
		
		require(msg.sender==_detail.user_address , 'Address NO');
		require(_detail.is_white==1 , 'IS_WHITE NO');
		
       uint StartIndex=AddressListsIndex[_DataId];
		for(uint i=0;i<address_lists.length;i++){ 
			AddressLists[_DataId][i+StartIndex]=address_lists[i];
		
		}
		AddressListsIndex[_DataId]=AddressListsIndex[_DataId]+address_lists.length;
		
	}
	
    function get_airdrop(uint256 _id,address coin_address) external view{
		
		
	} 
	function get_lists(uint256 max_id,uint256 min_id,uint256 amount) external view returns(uint[] memory,address[] memory) {
		uint[] memory lists=new uint[]((amount*9+1));
        address[] memory address_lists=new address[]((amount*2));
		uint last_id=0;
		uint ii=0;
        uint k=0;
        uint kk=0;
		if(max_id>(DataId-1)){max_id=DataId-1;}
		for(uint i=max_id;i>=min_id;i--){ 
			lists[k]=i;k++;
			address_lists[kk]=DataLists[i].user_address;kk++;
			address_lists[kk]=DataLists[i].coin_address;kk++;
			lists[k]=DataLists[i].amount;k++;
			lists[k]=DataLists[i].apply_time_start;k++;
			lists[k]=DataLists[i].apply_time_end;k++;
            lists[k]=DataLists[i].release_time_start;k++;
            lists[k]=DataLists[i].release_time_end;k++;
			lists[k]=DataLists[i].pre_amount;k++;
			lists[k]=DataLists[i].is_white;k++;
			lists[k]=DataLists[i].end_type;k++;
			lists[k]=DataLists[i].auth_type;k++;
			ii++;
			if(ii>=amount){last_id=i;break;} 
			if(i==0){break;	}
		}
		lists[k]=last_id;
		return (lists,address_lists);
	}
	function get_lists_by_user_address(uint256 max_id,uint256 min_id,uint256 amount,address _addr) external view returns(uint[] memory,address[] memory) {
		uint[] memory lists=new uint[]((amount*9+1));
        address[] memory address_lists=new address[]((amount*2));
		uint last_id=0;
		uint ii=0;
		uint _id=0;
        uint k=0;
        uint kk=0;
		 
		if(max_id>(DataAddressLength[_addr]-1)){max_id=DataAddressLength[_addr]-1;}

		for(uint i=max_id;i>=min_id;i--){
			_id=DataAddressLists[_addr][i];
			lists[k]=_id;k++;
		 
			lists[k]=i;k++;
			address_lists[kk]=DataLists[_id].user_address;kk++;
			address_lists[kk]=DataLists[_id].coin_address;kk++;
			lists[k]=DataLists[_id].amount;k++;
			lists[k]=DataLists[i].apply_time_start;k++;
			lists[k]=DataLists[i].apply_time_end;k++;
            lists[k]=DataLists[i].release_time_start;k++;
            lists[k]=DataLists[i].release_time_end;k++;
			lists[k]=DataLists[_id].pre_amount;k++;
			lists[k]=DataLists[_id].is_white;k++;
			lists[k]=DataLists[_id].end_type;k++;
			lists[k]=DataLists[i].auth_type;k++;
			
			ii++;
			if(ii>=amount){last_id=i;break;} 
			if(i==0){break;	}
		}
		lists[k]=last_id;
		return (lists,address_lists);
	}
	function get_lists_by_coin_address(uint256 max_id,uint256 min_id,uint256 amount,address _addr) external view returns(uint[] memory,address[] memory) {
		uint[] memory lists=new uint[]((amount*9+1));
        address[] memory address_lists=new address[]((amount*2));
		uint last_id=0;
		uint ii=0;
		uint _id=0;
		uint k=0; 
        uint kk=0;
		if(max_id>(DataCoinLength[_addr]-1)){max_id=DataCoinLength[_addr]-1;}
		
		for(uint i=max_id;i>=min_id;i--){
			_id=DataCoinLists[_addr][i];
			lists[k]=_id;k++;
			address_lists[kk]=DataLists[_id].user_address;kk++;
			address_lists[kk]=DataLists[_id].coin_address;kk++;
			lists[k]=DataLists[_id].amount;k++;
			lists[k]=DataLists[_id].apply_time_end;k++;
			lists[k]=DataLists[_id].apply_time_end;k++;
            lists[k]=DataLists[_id].release_time_start;k++;
            lists[k]=DataLists[_id].release_time_end;k++;
			lists[k]=DataLists[_id].pre_amount;k++;
			lists[k]=DataLists[_id].is_white;k++;
			lists[k]=DataLists[_id].end_type;k++;
			lists[k]=DataLists[i].auth_type;k++;
			
			ii++;
			if(ii>=amount){last_id=i;break;} 
			if(i==0){break;	}
		}
		lists[k]=last_id;
		return (lists,address_lists);
	}
	
	function end_airdrop(uint256 _DataId,uint256 _amount)  external  returns (uint) {
	 	
		require(DataIsendById[_DataId]==0, 'End NO');
		
		DataArr memory _detail=DataLists[_DataId];
		require(msg.sender==_detail.user_address , 'Address NO');
		
		uint cur_time=block.timestamp;
		require(_detail.release_time_end<cur_time , 'TIME NO');
		
	 	uint cur_amount=0; 
		
		cur_amount=DataLists[_DataId].amount.sub(DataGetAmountById[_DataId]);
		if(cur_amount>0){
			IERC20(_detail.coin_address).transfer(ReturnAddressById[_DataId],cur_amount);	
		}
		DataIsendById[_DataId]=1;
		return cur_amount;

    }
	
	
	 /*
	function get_user_amount_by_coinaddress(address _addr,address user_addr,uint256 max_id,uint256 min_id)  public view returns (uint) {
	 
		uint cur_time=block.timestamp;
		uint all_amount=0;
        uint _id=0;
        if(max_id>(DataCoinLength[_addr]-1)){max_id=DataCoinLength[_addr]-1;}
		
		for(uint i=max_id;i>=min_id;i--){
			_id=DataCoinLists[_addr][i];
			if(DataLists[_id].release_time_start<cur_time && DataLists[_id].release_time_end>=cur_time){
				all_amount=all_amount.add(DataLists[_id].amount);
			}
		}
		uint cur_amount=CoinAddressAmount[_addr][user_addr];
		cur_amount=all_amount.sub(cur_amount);
		return cur_amount;

    }
	 
	function withdraw_by_coinaddress(address _addr,uint256 max_id,uint256 min_id)   external lock  {
		
        uint cur_amount=get_user_amount_by_coinaddress(_addr,msg.sender,max_id,min_id);
        if(cur_amount>0){

        CoinAddressAmount[_addr][msg.sender]=CoinAddressAmount[_addr][msg.sender].add(cur_amount);
        IERC20(_addr).transfer(msg.sender, cur_amount);
        }

		uint cur_amount=CoinAddressAmount[_addr][msg.sender];
		cur_amount=all_amount.sub(cur_amount);
		if(cur_amount>0){
			CoinAddressAmount[_addr][msg.sender]=all_amount;
        	IERC20(_addr).transfer(msg.sender, cur_amount);
		}
    }
	*/
	function get_user_amount_by_id(uint256 _DataId,address _useraddr)  public view returns (uint) {
	 
		uint cur_time=block.timestamp;
		uint cur_amount=0; 
		uint all_amount=0; 
		DataArr memory _detail=DataLists[_DataId];
		if(_detail.release_time_start<cur_time && _detail.release_time_end>=cur_time){
			all_amount=DataLists[_DataId].pre_amount;
		}
		 
		cur_amount=DataUserAmountById[_DataId][_useraddr];
		cur_amount=all_amount.sub(cur_amount);
		
		
		if(DataLists[_DataId].amount<DataGetAmountById[_DataId].add(cur_amount)){
			cur_amount=0;
		}
		
		return cur_amount;

    }
	function withdraw_by_id(uint256 _DataId)   external lock  {
		
        uint cur_amount=get_user_amount_by_id(_DataId,msg.sender);
		DataArr memory _detail=DataLists[_DataId];

		require(_detail.auth_type==0, 'NO auth_type');
		require(cur_amount>0, 'NO amount');
		require(_detail.is_white==0 , 'IS_WHITE NO');
        
		if(cur_amount>0){
       		DataUserAmountById[_DataId][msg.sender]=DataUserAmountById[_DataId][msg.sender].add(cur_amount);
			
			DataGetAmountById[_DataId]=DataGetAmountById[_DataId].add(cur_amount);			
       		IERC20(_detail.coin_address).transfer(msg.sender, cur_amount);
        }

    }
	function withdraw_by_owner(uint256 _DataId,address _useraddress) onlyOwner  external lock  {
		
        uint cur_amount=get_user_amount_by_id(_DataId,_useraddress);
		DataArr memory _detail=DataLists[_DataId];

		require(_detail.auth_type>0, 'NO auth_type');
		require(cur_amount>0, 'NO amount');
		require(_detail.is_white==1 , 'IS_WHITE NO');
        
		if(cur_amount>0){
       		DataUserAmountById[_DataId][msg.sender]=DataUserAmountById[_DataId][msg.sender].add(cur_amount);
			
			DataGetAmountById[_DataId]=DataGetAmountById[_DataId].add(cur_amount);
       		IERC20(_detail.coin_address).transfer(_useraddress, cur_amount);
        }

    }
	
	
	 
}