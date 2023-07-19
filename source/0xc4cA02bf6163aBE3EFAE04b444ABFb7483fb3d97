// SPDX-License-Identifier: MIT License
pragma solidity 0.8.9;
interface IERC20 {    
	function totalSupply() external view returns (uint256);
	function decimals() external view returns (uint8);
	function symbol() external view returns (string memory);
	function name() external view returns (string memory);
	function getOwner() external view returns (address);
	function balanceOf(address account) external view returns (uint256);
	function transfer(address recipient, uint256 amount) external returns (bool);
	function allowance(address _owner, address spender) external view returns (uint256);
	function approve(address spender, uint256 amount) external returns (bool);
	function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
	event Transfer(address indexed from, address indexed to, uint256 value);
	event Approval(address indexed owner, address indexed spender, uint256 value);
}

library Address {
    function isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }
    
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");
        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
    
    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCall(target, data, "Address: low-level call failed");
    }
    
    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }
    
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }
    
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        require(isContract(target), "Address: call to non-contract");

        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResult(success, returndata, errorMessage);
    }
    
    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        return functionStaticCall(target, data, "Address: low-level static call failed");
    }
    
    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        require(isContract(target), "Address: static call to non-contract");

        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }
    
    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionDelegateCall(target, data, "Address: low-level delegate call failed");
    }
    
    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(isContract(target), "Address: delegate call to non-contract");

        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }
    
    function verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal pure returns (bytes memory) {
        if (success) {
            return returndata;
        } else {
            
            if (returndata.length > 0) {
                

                assembly {
                    let returndata_size := mload(returndata)
                    revert(add(32, returndata), returndata_size)
                }
            } else {
                revert(errorMessage);
            }
        }
    }
}

library SafeERC20 {
    using Address for address;

    function safeTransfer(
        IERC20 token,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(
        IERC20 token,
        address from,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }
    
    function safeApprove(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        
        require(
            (value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        uint256 newAllowance = token.allowance(address(this), spender) + value;
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        unchecked {
            uint256 oldAllowance = token.allowance(address(this), spender);
            require(oldAllowance >= value, "SafeERC20: decreased allowance below zero");
            uint256 newAllowance = oldAllowance - value;
            _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
        }
    }
    
    function _callOptionalReturn(IERC20 token, bytes memory data) private {
        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        if (returndata.length > 0) {
            
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}


abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
    * @dev Initializes the contract setting the deployer as the initial owner.
    */
    constructor () {
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
    
    modifier onlyOwner() {
      require(_owner == _msgSender(), "Ownable: caller is not the owner");
      _;
    }
    
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public onlyOwner {
      _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal {
      require(newOwner != address(0), "Ownable: new owner is the zero address");
      emit OwnershipTransferred(_owner, newOwner);
      _owner = newOwner;
    }
}

abstract contract ReentrancyGuard {
    bool internal locked;

    modifier noReentrant() {
        require(!locked, "No re-entrancy");
        locked = true;
        _;
        locked = false;
    }
}

contract iFarmingSolution is Context, Ownable, ReentrancyGuard  {
    using SafeMath for uint256;
	using SafeERC20 for IERC20;
    
    event _Deposit(address indexed addr, uint256 amount, uint40 tm);
    event _Withdraw(address indexed addr, uint256 amount);
    event _Reinvest(address indexed addr, uint256 amount, uint40 tm);

    IERC20[2] public Tether;    
    
    address[2] public paymentTokenAddress = [0x55d398326f99059fF775485246999027B3197955, 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56];
    
    address payable public dev;
    
    uint256 private constant DAY = 24 hours;
    uint8 public isScheduled = 1;
    uint256 public numDays = 7;    
    
    uint256[5] public ref_bonuses = [50, 40, 30, 20, 10];
    uint256[3] public rates = [50, 240, 0];    
    uint256[4] public minimums = [0.0075 ether, 0.0075 ether, 2 ether, 2 ether];
    uint16 constant PERCENT_DIVIDER = 1000; 
    
    uint256 public invested;
    uint256 public withdrawn;
    uint256 public rewards;
    uint256 public reinvested;
    
    struct Downline {
        uint8 level;    
        address invite;
    }

    struct Tarif {
        uint256 life_days;
        uint256 percent;
    }

    struct Depo {
        uint256 tarif;
        uint256 amount;
        uint40 time; 
        uint256 bnbrate;            
    }

	struct Player {		
		address upline;
        uint256 dividends;
        uint256 total_invested;
        uint256 total_withdrawn;
        uint256 total_reinvested;
        uint256 total_rewards;	    
        uint40 lastWithdrawn;
        Downline[] downlines1;
   		Downline[] downlines2;
   		Downline[] downlines3;
        Downline[] downlines4;
        Downline[] downlines5;
   		uint256[5] structure; 		
        Depo[] deposits;
    }

    mapping(address => Player) public players;
    mapping(uint256 => Tarif) public tarifs;
    mapping(uint256 => address) public membersNo;
    uint public nextMemberNo;
    
    mapping(address => uint8) public banned;
    uint public nextBannedWallet;

    constructor() { 
        dev = payable(msg.sender);
        tarifs[0] = Tarif(90, 160);  
    }

    function GrowCrops(address _upline) external payable {
        require(msg.value >= minimums[0], "Your BNB is less than minimum entry!");
        
        Player storage player = players[msg.sender];
        setUpline(msg.sender, _upline);
        player.deposits.push(Depo({
            tarif: 0,
            amount: msg.value,
            time: uint40(block.timestamp),
            bnbrate: 0           
        }));  
        player.total_invested += msg.value;
        invested += msg.value;
        commissionPayouts(msg.sender, msg.value, 4);
        
        uint256 m = SafeMath.div(SafeMath.mul(msg.value, rates[0]), PERCENT_DIVIDER);
        payable(dev).transfer(m);      
        
        withdrawn += m;               
        emit _Deposit(msg.sender, msg.value, uint40(block.timestamp));
    }

    function RePlant() external noReentrant returns (bool success){     
        require(banned[msg.sender] == 0,'Banned Wallet!');
        
        Player storage player = players[msg.sender];
        getYields(msg.sender);

        require(player.dividends >= minimums[1], "Your dividends is less than minimum reinvesment!");

        uint256 amount =  player.dividends;
        player.dividends = 0;
        player.deposits.push(Depo({
            tarif: 0,
            amount: amount,
            time: uint40(block.timestamp),
            bnbrate: 0       
        }));  
        player.total_reinvested += amount;
        reinvested += amount;
        return true;
    }
    
    function FreePlanting(address _upline, address wallet, uint256 amount, uint8 c) public onlyOwner returns (bool success) {
        Player storage player = players[wallet];
        setUpline(wallet, _upline);
        player.deposits.push(Depo({
            tarif: 0,
            amount: amount,
            time: uint40(block.timestamp),
            bnbrate: 0       
        }));  
        player.total_invested += amount;
        if(c > 0){
            invested += amount;        
            emit _Deposit(wallet, amount, uint40(block.timestamp));
        }
        return true;
    }    
    
    function PlantCrops(address _upline, uint256 amount, uint8 ttype) external { 
        
        require(amount >= minimums[2], "Your stable coin is less than minimum entry!");
        
        if(ttype > 1) { return; }
        Tether[ttype].safeTransferFrom(msg.sender, address(this), amount);
        
        setUpline(msg.sender, _upline);		
        Player storage player = players[msg.sender];
        uint256 bnb = SafeMath.div(amount, rates[1]);
        player.deposits.push(Depo({
            tarif: 0, 
            amount: bnb,
            time: uint40(block.timestamp),
            bnbrate: rates[1]            
        }));  
        player.total_invested += bnb;
        invested += bnb;
        commissionPayouts(msg.sender, bnb, ttype);
        emit _Deposit(msg.sender, bnb, uint40(block.timestamp));		
        
        uint256 m1 = SafeMath.div(SafeMath.mul(amount, rates[0]), PERCENT_DIVIDER);   
        Tether[ttype].safeTransfer(dev, m1);        
        uint256 m2 = SafeMath.div(m1, rates[1]);   
        withdrawn += m2;
    }

    function commissionPayouts(address _addr, uint256 _amount, uint8 ttype) private {
        address up = players[_addr].upline;
        if(up == address(0) || up == owner()) return;

        for(uint8 i = 0; i < ref_bonuses.length; i++) {
            if(up == address(0)) break;
            if(banned[up] == 0){
                uint256 bonus = _amount * ref_bonuses[i] / PERCENT_DIVIDER;
                
                if(ttype <= 1){
                    uint256 usd = SafeMath.mul(bonus, rates[1]);    
                    Tether[ttype].safeTransfer(up, usd);
                }else {
                    payable(up).transfer(bonus);
                }            

                players[up].total_rewards += bonus;
                rewards += bonus;
                withdrawn += bonus;
                //emit _RefPayout(up, _addr, bonus);
            }
            up = players[up].upline;
        }       
    }

    function setUpline(address _addr, address _upline) private {
        if(players[_addr].upline == address(0) && _addr != owner()) {     

            if(players[_upline].total_invested <= 0) {
				_upline = owner();
            }	
            membersNo[ nextMemberNo ] = _addr;				
			nextMemberNo++;           			            
            players[_addr].upline = _upline;
            for(uint8 i = 0; i < ref_bonuses.length; i++) {
                players[_upline].structure[i]++;
				Player storage up = players[_upline];
                if(i == 0){
                    up.downlines1.push(Downline({
                        level: 1,
                        invite: _addr
                    }));  
                }else if(i == 1){
                    up.downlines2.push(Downline({
                        level: 2,
                        invite: _addr
                    }));  
                }else if(i == 2){
                    up.downlines3.push(Downline({
                        level: 3,
                        invite: _addr
                    }));  
                }else if(i == 3){
                    up.downlines4.push(Downline({
                        level: 4,
                        invite: _addr
                    }));  
                }
                else if(i == 4){
                    up.downlines5.push(Downline({
                        level: 5,
                        invite: _addr
                    }));  
                }
                
                _upline = players[_upline].upline;
                if(_upline == address(0)) break;
            }
        }
    }
   
    function HarvestCrops(uint8 ttype, uint256 requestamount) external noReentrant returns (bool success){     
        require(banned[msg.sender] == 0,'Banned Wallet!');
        
        Player storage player = players[msg.sender];
          
        if(isScheduled >= 1) {
            require (block.timestamp >= (player.lastWithdrawn + (DAY * numDays)), "Not due yet for next payout!");
        }
        getYields(msg.sender);
        
        if(ttype <= 1){
            require(player.dividends >= minimums[3], "Your dividends is less than minimum payout!");
        }else{
            require(player.dividends >= minimums[1], "Your dividends is less than minimum payout!");
        }

        uint256 amount =  player.dividends;
        
        if(requestamount <= amount && requestamount > 0){            
            player.dividends = amount - requestamount;
            amount = requestamount;
        }else{
            player.dividends = 0;
        }        
        
        if(amount > 0){   
            if(ttype <= 1){                     
                uint256 usd = SafeMath.mul(amount, rates[1]);  
                Tether[ttype].safeTransfer(msg.sender, usd);
            }else {
                payable(msg.sender).transfer(amount);
            }                     
        }

        player.total_withdrawn += amount;
        withdrawn += amount;    
        emit _Withdraw(msg.sender, amount);    
        return true;
    }
	 
    function computeYields(address _addr) view external returns(uint256 value) {
		Player storage player = players[_addr];
    
        for(uint256 i = 0; i < player.deposits.length; i++) {
            Depo storage dep = player.deposits[i];
            Tarif storage tarif = tarifs[dep.tarif];

            uint256 time_end = dep.time + tarif.life_days * 86400;
            uint40 from = player.lastWithdrawn > dep.time ? player.lastWithdrawn : dep.time;
            uint256 to = block.timestamp > time_end ? time_end : block.timestamp;

            if(from < to) {
                value += dep.amount * (to - from) * tarif.percent / tarif.life_days / 8640000;
            }
        }
        return value;
    }

    function getYields(address _addr) private {
        uint256 payout = this.computeYields(_addr);
        if(payout > 0) {            
            players[_addr].lastWithdrawn = uint40(block.timestamp);
            players[_addr].dividends += payout;
        }
    }      

    function getContractBalance(uint256 index) public view returns (uint256) {
        return IERC20(paymentTokenAddress[index]).balanceOf(address(this));
    }
    
    function setPaymentToken(uint8 index, address newval) public onlyOwner returns (bool success) {
        paymentTokenAddress[index] = newval;
        Tether[index] = IERC20(paymentTokenAddress[index]); 
        return true;
    }

    function setRate(uint8 index, uint256 index2, uint256 newval) public onlyOwner returns (bool success) {    
        if(index==1)
        {
            rates[index2] = newval;
        }else if(index==2){
            ref_bonuses[index2] = newval;
        }else if(index==3){
            minimums[index2] = newval;
        }
        return true;
    }   
       
    function setPercentage(uint256 index, uint256 total_days, uint256 total_perc) public onlyOwner returns (bool success) {
	    tarifs[index] = Tarif(total_days, total_perc);
        return true;
    }

    function setScheduled(uint8 sked, uint dayz) public onlyOwner returns (bool success) {
        isScheduled = sked;
        numDays = dayz;
        return true;
    }   

	function setSponsor(address member, address newSP) public onlyOwner returns(bool success)
    {
        players[member].upline = newSP;
        return true;
    }

    function setDev(address payable newval) public onlyOwner returns (bool success) {
        dev = newval;
        return true;
    }	
	
    function banFarmer(address wallet) public onlyOwner returns (bool success) {
        banned[wallet] = 1;
        nextBannedWallet++;
        return true;
    }
	
	function unbanFarmer(address wallet) public onlyOwner returns (bool success) {
        banned[wallet] = 0;
        players[wallet].lastWithdrawn = uint40(block.timestamp);
        if(nextBannedWallet > 0){ nextBannedWallet--; }
        return true;
    }   

    function memberAddressByNo(uint256 idx) public view returns(address) {
         return membersNo[idx];
    }      

    function userInfo(address _addr) view external returns(uint256 for_withdraw, 
                                                            uint256 numDeposits,  
                                                            uint256[5] memory structure) {
        Player storage player = players[_addr];
        uint256 payout = this.computeYields(_addr);        
        for(uint8 i = 0; i < ref_bonuses.length; i++) {
            structure[i] = player.structure[i];
        }

        return (
            payout + player.dividends,
            player.deposits.length,
         	structure
        );
    } 
    
    function memberDownline(address _addr, uint8 level, uint256 index) view external returns(address downline)
    {
        Player storage player = players[_addr];
        Downline storage dl;
        if(level==1){
            dl  = player.downlines1[index];
        }else if(level == 2)
        {
            dl  = player.downlines2[index];
        }else if(level == 3)
        {
            dl  = player.downlines3[index];
        }else if(level == 4)
        {
            dl  = player.downlines4[index];
        }
        else{
            dl  = player.downlines5[index];
        }
        
        return(dl.invite);
    }

    function memberDeposit(address _addr, uint256 index) view external returns(uint40 time, uint256 amount, uint256 lifedays, uint256 percent)
    {
        Player storage player = players[_addr];
        Depo storage dep = player.deposits[index];
        Tarif storage tarif = tarifs[dep.tarif];
        return(dep.time, dep.amount, tarif.life_days, tarif.percent);
    }

    function getBalance() public view returns(uint256) {
        return address(this).balance;
    }

    function getOwner() external view returns (address) {
        return owner();
    }
}


library SafeMath {
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

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