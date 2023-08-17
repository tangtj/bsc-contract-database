// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;


interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
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

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _transferOwnership(_msgSender());
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

 
  
contract Privatesale is Ownable {
    address USDT = 0x55d398326f99059fF775485246999027B3197955;
    address ORG  = 0x1767d519A11eE0A0073BE1941125Dc459F655B2E;
     
    struct user {
                address addr;
                uint256 amount;
                uint256 claimed;
                uint256 lastclaim;
                address upline; 
                uint256 endtime;
            }
   
    mapping(address => address) public upline;
    mapping(address => user) public users;
    uint256 public allocation = 20000e18;
    uint256 public soldout = 0;
    uint256 public claimed = 0;

  
    constructor() {
        upline[msg.sender] = msg.sender;
    }

 
     function buy( uint256 amount,address _upline) external {
            require(amount>0,"Invalid amount");
            require(allocation>= soldout+amount,"Has been soldout");
            require(_upline != msg.sender,"Self affiliate not allowed");
            require(upline[_upline] != address(0),"Invalid upline");

            IERC20(USDT).transferFrom(address(msg.sender),owner(),amount*9);
            IERC20(USDT).transferFrom(address(msg.sender),_upline,amount*1);

            claim(msg.sender);
            user storage nftdata = users[msg.sender];   
            nftdata.addr =  msg.sender;
            nftdata.amount =   nftdata.amount + amount;
            nftdata.claimed =  nftdata.claimed;
            nftdata.lastclaim =  block.timestamp;
            upline[msg.sender] =_upline;
            nftdata.upline =  _upline;
            nftdata.endtime =  block.timestamp + 26300000;
            soldout = soldout + amount;
          
    }


    function pending(address addr) public view  returns(uint256){
         user memory nftdata = users[addr];   
         if(nftdata.amount <=  nftdata.claimed) return(0);
         uint256 unclaimed = nftdata.amount -  nftdata.claimed ;
         uint256 endtolast = nftdata.endtime - nftdata.lastclaim;
         uint256 persecond = unclaimed / endtolast;
         uint256 lasttonow = block.timestamp - nftdata.lastclaim;
         uint256 pend = lasttonow * persecond;
         if(pend>unclaimed) pend = unclaimed;
         return pend;

    }


    function claim(address addr) public {
        uint256 pend = pending(addr);
        if(pend>0){
        IERC20(ORG).transfer(addr,pend);
        user storage nftdata = users[msg.sender];  
        nftdata.claimed =  nftdata.claimed + pend;
        nftdata.lastclaim =  block.timestamp;
        claimed = claimed + pend;
        } 
    }

    //withdraw unsold
    function unsold(uint256 amount) public onlyOwner {
       uint256 balance =  IERC20(ORG).balanceOf(address(this));
       if(balance > (soldout * 11 )/10 + amount)
       IERC20(ORG).transfer(owner(),amount);
        
    }

  
}