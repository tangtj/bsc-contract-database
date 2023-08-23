// SPDX-License-Identifier: MIT

pragma solidity ^0.7.6;

/*

------------------------------------ 
 CONTRACT MANAGEMENT:
------------------------------------

10% direct referral 👨  
3% referred level 2 👨🏽‍👨🏽‍  
1% referred level 3 👨🏽‍👨🏽‍👨🏽‍  
1% referred level 4 👨🏽‍👨🏽‍ 👨🏽‍ 👨🏽‍   
1% referred level 5 👨🏽‍👨🏽‍👨🏽‍ 👨🏽‍ 👨🏽‍ 
------------------------------------

*/


interface IBEP20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}


abstract contract Ownable is Context {
    address private _owner;
    address private _previousOwner;
    uint256 private _lockTime;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
        * @dev Initializes the contract setting the deployer as the initial owner.
        */
    constructor ()  {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
        * @dev Returns the address of the current owner.
        */
    function owner() public view returns (address) {
        return _owner;
    }

    /**
        * @dev Throws if called by any account other than the owner.
        */
    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

        /**
        * @dev Leaves the contract without owner. It will not be possible to call
        * `onlyOwner` functions anymore. Can only be called by the current owner.
        *
        * NOTE: Renouncing ownership will leave the contract without an owner,
        * thereby removing any functionality that is only available to the owner.
        */
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
        * @dev Transfers ownership of the contract to a new account (`newOwner`).
        * Can only be called by the current owner.
        */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }

    function geUnlockTime() public view returns (uint256) {
        return _lockTime;
    }

    //Locks the contract for owner for the amount of time provided
    function lock(uint256 time) public virtual onlyOwner {
        _previousOwner = _owner;
        _owner = address(0);
        _lockTime = block.timestamp + time;
        emit OwnershipTransferred(_owner, address(0));
    }
    
    //Unlocks the contract for owner when _lockTime is exceeds
    function unlock() public virtual {
        require(_previousOwner == msg.sender, "You don't have permission to unlock the token contract");
        require(block.timestamp > _lockTime , "Contract is locked until 7 days");
        emit OwnershipTransferred(_owner, _previousOwner);
        _owner = _previousOwner;
    }
}

contract ksrtairdrop is  Context, Ownable {
    using SafeMath for uint256;
    uint public totalPlayers;
    bool private locked;   
    bool private isstart;  
    address public _tokenAddress = 0x7e358413038Ea38b2946dC33f2551B5630Ac6398;

    uint256 lvl0 = 5;
    uint256 lvl1 = 1;
    uint256 lvl2 = 1;
    uint256 lvl3 = 1;
    uint256 lvl4 = 1;
    uint256 lvl5 = 1;
    uint256 mul = 1000000;

    struct Player {
        uint active;
        uint time;
        string hash;
        address affFrom;
        uint256 aff1sum; 
        uint256 aff2sum;
        uint256 aff3sum;
        uint256 aff4sum;
        uint256 aff5sum;
        uint256 w_token;      
    }

    struct Hash {
        address user;
        uint active;
    }

    modifier nonReentrant() {
    require(!locked, "No re-entrancy");
    locked = true;
    _;
    locked = false;
    }



    constructor(){		
        players[owner()].active = 1;
        isstart = false;
	}


    mapping(address => Player) public players;
    mapping(string => Hash) public hashs;

    event Newbie(address indexed user, address indexed _referrer, uint _time);  
    receive() external payable {}

    function setAllFeePercent(uint256 _lvl0 ,uint256 _lvl1,uint256 _lvl2,uint256 _lvl3,uint256 _lvl4,uint256 _lvl5,uint256 _mul) external onlyOwner() {    
        lvl0 = _lvl0;
        lvl1 = _lvl1;
        lvl2 = _lvl2;
        lvl3 = _lvl3;
        lvl4 = _lvl4;
        lvl5 = _lvl5;
        mul = _mul;
    }

    function setisstart(bool _isstart) external onlyOwner() {    
        isstart = _isstart;
    }

    function settokenAddress(address __tokenAddress) external onlyOwner() {    
        _tokenAddress = __tokenAddress;
    }

    function claim(address _affAddr, string memory _hash) public payable {
        
        Player storage player = players[msg.sender];
        Hash storage hash = hashs[_hash]; 
        require(player.active == 0, "No Reapeat");
        require(hash.active == 0, "No Reapeat");

        totalPlayers++;
        

        if(_affAddr != address(0) && players[_affAddr].active != 0){
            emit Newbie(msg.sender, _affAddr, block.timestamp);
            register(msg.sender, _affAddr, _hash);
        }
        else{
            emit Newbie(msg.sender, owner(), block.timestamp);
            register(msg.sender, owner(), _hash);
        }



    }

    function getvel(address _Addr) public view returns (uint256) { 
		uint256 token = getlevel(_Addr,6);
		uint256 vel = token - players[_Addr].w_token;
		return vel;
	}

    function getlevel(address _Addr , uint256 _lvl) public view returns (uint256) { 
        Player storage player = players[_Addr];
	
		uint256 vel = 0;
        if(_lvl == 0){
            vel = lvl0 * mul;
        }else if(_lvl == 1){
           vel = player.aff1sum * lvl1 * mul;
        }else if(_lvl == 2){
            vel = player.aff2sum * lvl2 * mul;
        }else if(_lvl == 3){
            vel = player.aff3sum * lvl3 * mul;
        }else if(_lvl == 4){
            vel = player.aff4sum * lvl4 * mul;
        }else if(_lvl == 5){
            vel = player.aff5sum * lvl5 * mul;
        }else if(_lvl == 6){
            vel = ( (lvl0) + (player.aff1sum * lvl1) + (player.aff2sum * lvl2) + (player.aff3sum * lvl3) + ( player.aff4sum * lvl4) + (player.aff5sum  * lvl5) ) * mul;
        }

		return vel;
	}

    

    

    function withdraw() public {        
        require(isstart == true, "Not started");
        uint256 vel = getvel(msg.sender);
        require(players[msg.sender].active == 1, "Not active");
        require(vel > 0, "Not active");
        players[msg.sender].w_token = players[msg.sender].w_token.add(vel);

        IBEP20(_tokenAddress).transfer(msg.sender, vel * 1 ether);
    
        //transfer token to user
        
    }

     function recoverBEP20(address tokenAddress, uint256 tokenAmount) public onlyOwner {
        // do not allow recovering self token
        require(tokenAddress != address(this), "Self withdraw");
        IBEP20(tokenAddress).transfer(owner(), tokenAmount);
    }
    function recoverBEP(address Address, uint256 tokenAmount) public onlyOwner {
        payable(Address).transfer(tokenAmount);
    }





    function register(address _addr, address _affAddr,string memory _hash) private{

      Player storage player = players[_addr];
      Hash storage hash = hashs[_hash]; 
      player.active = 1;
      player.affFrom = _affAddr;
      player.hash = _hash;
      hash.user = _addr;
      hash.active = 1;

      address _affAddr1 = _affAddr;
      address _affAddr2 = players[_affAddr1].affFrom;
      address _affAddr3 = players[_affAddr2].affFrom;
      address _affAddr4 = players[_affAddr3].affFrom;
      address _affAddr5 = players[_affAddr4].affFrom;

      players[_affAddr1].aff1sum = players[_affAddr1].aff1sum.add(1);
      players[_affAddr2].aff2sum = players[_affAddr2].aff2sum.add(1);
      players[_affAddr3].aff3sum = players[_affAddr3].aff3sum.add(1);
      players[_affAddr4].aff4sum = players[_affAddr4].aff4sum.add(1);
      players[_affAddr5].aff5sum = players[_affAddr5].aff5sum.add(1);     
     
    }

}

library SafeMath {

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b);

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0);
        uint256 c = a / b;

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a);
        uint256 c = a - b;

        return c;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a);

        return c;
    }

}