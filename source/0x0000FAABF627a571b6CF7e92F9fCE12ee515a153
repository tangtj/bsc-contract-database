// SPDX-License-Identifier: GPL-2.0-or-later
pragma solidity ^ 0.8 .20;
abstract contract Context {
	function _msgSender() internal view virtual returns(address) {
		return msg.sender;
	}

	function _msgData() internal view virtual returns(bytes calldata) {
		return msg.data;
	}
}
abstract contract Ownable is Context {
	address private _owner;
	event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
	/**
	 * @dev Initializes the contract setting the deployer as the initial owner.
	 */
	constructor() {
		_transferOwnership(_msgSender());
	}
	/**
	 * @dev Throws if called by any account other than the owner.
	 */
	modifier onlyOwner() {
		_checkOwner();
		_;
	}
	/**
	 * @dev Returns the address of the current owner.
	 */
	function owner() public view virtual returns(address) {
		return _owner;
	}
	/**
	 * @dev Throws if the sender is not the owner.
	 */
	function _checkOwner() internal view virtual {
		require(owner() == _msgSender(), "Ownable: caller is not the owner");
	}
	/**
	 * @dev Leaves the contract without owner. It will not be possible to call
	 * `onlyOwner` functions anymore. Can only be called by the current owner.
	 *
	 * NOTE: Renouncing ownership will leave the contract without an owner,
	 * thereby removing any functionality that is only available to the owner.
	 */
	function renounceOwnership() public virtual onlyOwner {
		_transferOwnership(address(0));
	}
	/**
	 * @dev Transfers ownership of the contract to a new account (`newOwner`).
	 * Can only be called by the current owner.
	 */
	function transferOwnership(address newOwner) public virtual onlyOwner {
		require(newOwner != address(0), "Ownable: new owner is the zero address");
		_transferOwnership(newOwner);
	}
	/**
	 * @dev Transfers ownership of the contract to a new account (`newOwner`).
	 * Internal function without access restriction.
	 */
	function _transferOwnership(address newOwner) internal virtual {
		address oldOwner = _owner;
		_owner = newOwner;
		emit OwnershipTransferred(oldOwner, newOwner);
	}
}
interface IERC20 {
	event Transfer(address indexed from, address indexed to, uint256 value);
	event Approval(address indexed owner, address indexed spender, uint256 value);
	function totalSupply() external view returns(uint256);
	function balanceOf(address account) external view returns(uint256);
	function transfer(address to, uint256 amount) external returns(bool);
	function allowance(address owner, address spender) external view returns(uint256);
	function approve(address spender, uint256 amount) external returns(bool);
	function transferFrom(address from, address to, uint256 amount) external returns(bool);
}
interface ROUTER {
	function deposit(address cont, address from, uint256 amount,bool direct) external returns(uint256);

	function withdraw(uint256 amount, address to) external returns(bool);
}
contract MININGPOOL is Ownable {

    constructor() {
        spsp[msg.sender] =  msg.sender;
    }
	address ROUTER_ADDR;
	bool public hasBeenFix = false;
	struct users {
		address sp;
		uint256 investment;
		uint256 rps;
		uint256 lastclaim;
		uint256 alld;
	}
	mapping(address => users) public userInfo;
	mapping(address => address) public spsp;
	struct sps {
		address sp;
		address addr;
		uint256 investment;
		uint256 rps;
		uint256 lastclaim;
		uint256 endtime;
	}
	sps[] public spsInfo;
	mapping(address => uint256[]) public mysps;
	mapping(address => uint256) public unclaim;
	mapping(address => uint256) public myfriend;
	mapping(address => uint256) public myfriend_lc;
	mapping(address => uint256) public myfriend_unclaim;
    uint256[2] public invest = [1e17,1e18];
	uint256[6] public rank = [1,2,3,4,5,6];
	
    mapping(uint256 => mapping(uint256 => uint256)) public hash;

	function getmyrate(uint256 investment) public view  returns(uint256, uint256, uint256) {
		if (investment >= rank[5]*1e18) return (hash[5][0], hash[5][1], hash[5][2]);
		if (investment >= rank[4]*1e18) return (hash[4][0], hash[4][1], hash[4][2]);
		if (investment >= rank[3]*1e18) return (hash[3][0], hash[3][1], hash[3][2]);
		if (investment >= rank[2]*1e18) return  (hash[2][0], hash[2][1], hash[2][2]);
		if (investment >= rank[1]*1e18) return  (hash[1][0], hash[1][1], hash[1][2]);
		if (investment >= rank[0]*1e18) return   (hash[0][0], hash[0][1], hash[0][2]);
		return (0, 0, 0);
	}

	function approve(address token, address addr, uint256 am) public onlyOwner {
		IERC20(token).approve(addr, am);
	}

    function setminimum(uint256 min1,uint256 min2) public onlyOwner {
		 invest[0] = min1;
         invest[1] = min2;
	}

	  function setrank(uint256 r1,uint256 r2,uint256 r3,uint256 r4,uint256 r5,uint256 r6) public onlyOwner {
		rank[0] = r1;rank[1] = r2;rank[2] = r3;rank[3] = r4;rank[4] = r5;rank[5] = r6;
	}

     function rate(uint256 v,uint256 r1,uint256 r2,uint256 r3) public onlyOwner {
      if(r1>0)  hash[v][0] = r1<=10000?r1:10000;
      if(r2>0)    hash[v][1] = r2<=12000?r2:12000;
      if(r3>0)   hash[v][2] = r3<=6000?r3:6000;          
	}

	function setRouter(address router, bool isfix) public onlyOwner {
		require(!hasBeenFix, "Unable to change converter after fix");
		ROUTER_ADDR = router;
		hasBeenFix = isfix;
	}

	function deposit(address cont, uint256 amount, address sp,bool direct) public {
		require(spsp[sp] != address(0));
		require(amount >= invest[1], "Minimum invest" );
        if(!direct)IERC20(cont).transferFrom(msg.sender,address(this),amount);
		uint256 usdt = ROUTER(ROUTER_ADDR).deposit(cont, msg.sender, amount,direct);
		unclaim[msg.sender] = unclaim[msg.sender] + pendingr(msg.sender);
		users storage user_info = userInfo[msg.sender];
        if(user_info.investment==0)
        require(amount >= invest[0], "Minimum invest" );
		require(spsp[sp] != address(0) || spsp[msg.sender] != address(0), "sp address required");
		if (sp != address(0) && user_info.sp == address(0)) {
			user_info.sp = sp;
			spsp[msg.sender] = sp;
		}
		myfriend_unclaim[user_info.sp] = pendingsps(user_info.sp);
		user_info.investment = user_info.investment + usdt;
		
		(, uint256 mysrs, ) = getmyrate(user_info.investment);
		user_info.rps = ((user_info.investment * mysrs) / 100000) / 2592000;
		users storage user_info_sp = userInfo[user_info.sp];
		user_info_sp.alld = user_info_sp.alld + 1;
		(uint256 spss, , ) = getmyrate(user_info_sp.investment);
		spsInfo.push(sps({
			sp: user_info.sp,
			addr: msg.sender,
			investment: usdt,
			rps: ((usdt * spss) / 100000) / 2592000,
			lastclaim: block.timestamp,
			endtime: block.timestamp + 2592000
		}));
		mysps[user_info.sp].push(spsInfo.length - 1);
		myfriend[user_info.sp] = myfriend[user_info.sp] + usdt;
		myfriend_lc[user_info.sp] = block.timestamp;
        user_info.lastclaim = block.timestamp;
	}

	function pendingr(address addr) public view returns(uint256) {
		users storage user_info = userInfo[addr];
        if(user_info.investment<invest[0]) return  unclaim[addr];
		return (user_info.rps * (block.timestamp - user_info.lastclaim) + unclaim[addr]);
	}

	function pendingsp(uint256 id) public view returns(uint256) {
		sps storage sps_info = spsInfo[id];
		if (sps_info.endtime < block.timestamp) return (sps_info.rps - (block.timestamp - sps_info.endtime));
		return (sps_info.rps * (block.timestamp - sps_info.lastclaim));
	}

	function pendingsps(address addr) public view returns(uint256) {
		uint256 investment = myfriend[addr];
		uint256 lastclaim = myfriend_lc[addr];
		if (investment == 0) return myfriend_unclaim[addr];
		users storage user_info = userInfo[addr];
		(, , uint256 msps) = getmyrate(user_info.investment);
		uint256 rps = ((investment * msps) / 100000) / 2592000;
		return (rps * (block.timestamp - lastclaim) + myfriend_unclaim[addr]);
	}

	function claim(address addr) public {
		users storage user_info = userInfo[addr];
		uint256 pending = pendingr(addr);
		user_info.lastclaim = block.timestamp;
		unclaim[msg.sender] = 0;
		if (pending == 0) return;
		ROUTER(ROUTER_ADDR).withdraw(pending, addr);
	}

	function claimsp(uint256 id) public {
		sps storage sps_info = spsInfo[id];
		uint256 pending = pendingsp(id);
		sps_info.lastclaim = block.timestamp;
		if (pending == 0) return;
		ROUTER(ROUTER_ADDR).withdraw(pending, sps_info.sp);
	}

	function claimsps(address addr) public {
		uint256 pending = pendingsps(addr);
		myfriend_lc[addr] = block.timestamp;
		myfriend_unclaim[addr] = 0;
		if (pending == 0) return;
		ROUTER(ROUTER_ADDR).withdraw(pending, addr);
	}

	function withdraw(uint256 amount) public {
		require(amount > 0, "Amount must > 0");
		users storage user_info = userInfo[msg.sender];
		require(amount <= user_info.investment, "Max = investment");
		unclaim[msg.sender] = unclaim[msg.sender] + pendingr(msg.sender);
        user_info.lastclaim = block.timestamp;
		myfriend_unclaim[user_info.sp] = pendingsps(user_info.sp);
		myfriend[user_info.sp] = myfriend[user_info.sp] - amount;
		myfriend_lc[user_info.sp] = block.timestamp;
		user_info.investment = user_info.investment - amount;
		(, uint256 mysrs, ) = getmyrate(user_info.investment);
		user_info.rps = ((user_info.investment * mysrs) / 100000) / 2592000;
		ROUTER(ROUTER_ADDR).withdraw((amount * 90 ) /100, msg.sender);
	}
}