/**
 *Submitted for verification at BscScan.com on 2023-04-17
*/

/**
 *Submitted for verification at BscScan.com on 2023-04-15
*/

/**
 *Submitted for verification at BscScan.com on 2023-04-06
*/

/**
 *Submitted for verification at BscScan.com on 2023-03-28
*/

/**
 *Submitted for verification at Etherscan.io on 2022-07-25
*/

/**
 *Submitted for verification at BscScan.com on 2022-07-16
*/

/**
 *Submitted for verification at BscScan.com on 2021-12-30
*/

/**
 *Submitted for verification at BscScan.com on 2021-12-29
*/

/**
 *Submitted for verification at BscScan.com on 2021-12-29
*/
pragma solidity 0.6.8;

 interface IBEP20 {
    function totalSupply() external view returns (uint);
    function balanceOf(address tokenOwner) external view returns (uint balance);
    function allowance(address tokenOwner, address spender) external view returns (uint remaining);
    function transfer(address to, uint tokens) external returns (bool success);
    function approve(address spender, uint tokens) external returns (bool success);
    function transferFrom(address from, address to, uint tokens) external returns (bool success);
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}


contract Nandi_swap{

    struct User {
        uint id;
        uint partnersCount;
    }

    uint public totalInvestors;
    uint public totalInvested;
    uint public lastUserId = 2;

    address private _owner;
    
    mapping (address => uint256) private _balances;

    mapping(address => User) public users;
    mapping(uint => address) public idToAddress;
    mapping(uint => address) public userIds;

    IBEP20 private BUSD; 
    address public owners;    

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    event Registration(address indexed user, uint indexed userId);

    constructor(address ownerAddress,IBEP20 _BUSD) public {
        owners = ownerAddress;  
        BUSD = _BUSD;

        _owner = msg.sender;
        //owner = _ownerAddress;
        User memory user = User({
            id: 1,
            partnersCount: uint(0)
        });
        users[msg.sender] = user;
        idToAddress[1] = msg.sender;
        userIds[1] = msg.sender;
    }

     function _msgSender() internal view returns (address payable) {
        return msg.sender;
    }
    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view  returns (address) {
        return _owner;
    }
    
	function registration(address _userAddress) private {
        uint32 size;
        assembly {
            size := extcodesize(_userAddress)
        }
        require(size == 0, "cannot be a contract");
        
        User memory user = User({
            id: lastUserId,            
            partnersCount: 0
        });
        users[_userAddress] = user;
        idToAddress[lastUserId] = _userAddress;
        //users[_userAddress].referrer = _referrerAddress;
        userIds[lastUserId] = _userAddress;
        lastUserId++;
       // users[_referrerAddress].partnersCount++;
        emit Registration(_userAddress, users[_userAddress].id);
    }


   function isUserExists(address _user) public view returns (bool) {
        return (users[_user].id != 0);
    }


    function bytesToAddress(bytes memory bys) private pure returns (address addr) {
        assembly {
            addr := mload(add(bys, 20))
        }
    }

    function Buypackage(uint investment) public payable returns(bool)
	{
	    BUSD.transferFrom(msg.sender ,owners, investment);
	    registration(msg.sender);
        return true;
	}
}