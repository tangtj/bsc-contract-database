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

contract MMinnows is Context, Ownable, ReentrancyGuard  {
    using SafeMath for uint256;
	using SafeERC20 for IERC20;    
	
    event _Stock(address indexed addr, uint256 amount, uint40 tm);
    event _Harvest(address indexed addr, uint256 amount);
    event _Reinvest(address indexed addr, uint256 amount, uint40 tm);
    
    IERC20[3] public Tether;
    
    address[3] public paymentTokenAddress = [0x55d398326f99059fF775485246999027B3197955, //usdt
                                             0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56, //busd
                                             0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d]; //usdc
                                           
    address payable public admin;
    address payable public mkg;    
    uint256 private constant DAY = 24 hours;
    uint8 public isScheduled = 1;
    uint256 public numDays = 7;  
	uint256 public numDays2 = 14;  
	uint8 public isPayoutPaused = 0;    
    
    uint256[1] public ref_bonuses = [10];
    uint256[4] public rates = [5,5,30,20];    
    uint256[2] public minimums = [1 ether, 1 ether];
    uint16 constant PERCENT_DIVIDER = 1000;     
    
    uint256 private stocked;
    uint256 private harvested;
    uint256 private reinvested;
    uint256 private rewards;
    uint256 private tradefunds;
    uint256 private tradepumps;

    struct FishingPal {
        uint8 level;    
        address wallet;
    }

    struct Tarif {
        uint256 life_days;
        uint256 percent;
    }

    struct Stock {
        uint40 time;  
        uint256 tarif;
        uint256 amount;   
        uint256 reinvested;    
        uint256 numDays; 
        uint256 harvests;
        uint256 harvested;
        uint40 lastHarvest;   
        uint40 unStock;   
    }

   	struct Fisherman {		
		address fishingpal;
        uint256 total_stocked;
        uint256 total_harvested;
        uint256 total_reinvested;
        uint256 total_rewards;
        uint40 lastHarvest;
        FishingPal[] fishpals;
   		uint256[1] structure;	
        Stock[] stockings;
    }

    mapping(address => Fisherman) public fishermen;
    mapping(uint256 => Tarif) public tarifs;
    mapping(uint256 => address) public fishermenNo;
    uint public nextFishermenNo;
    mapping(address => uint8) public banned;
    uint public nextBannedWallet;
    
    constructor() {        
        admin = payable(msg.sender); 
        mkg = payable(0xE1153f5a1db8B3e8cb9720e88dAa4386F9B53D89); 
        Tether[0] = IERC20(paymentTokenAddress[0]);       
        Tether[1] = IERC20(paymentTokenAddress[1]);       
        Tether[2] = IERC20(paymentTokenAddress[2]); 
        tarifs[0] = Tarif(14, 14); 
    }   

    function StockMinnows(address _upline, uint256 amount, uint8 ttype) external {        
       require(amount >= minimums[0], "Your stable coin is less than minimum entry!");        
        Tether[ttype].safeTransferFrom(msg.sender, address(this), amount);        
        setUpline(msg.sender, _upline);		        
        Fisherman storage fisherman = fishermen[msg.sender];
        Tarif storage tarif = tarifs[0];
        fisherman.stockings.push(Stock({
            tarif: 0, 
            amount: amount,
            reinvested: 0,
            harvests: 0,
            harvested: 0,
            time: uint40(block.timestamp),
            lastHarvest: uint40(block.timestamp),
            numDays: tarif.life_days,
            unStock: 0       
        }));
        fisherman.total_stocked += amount;
        stocked += amount;
        rewardFISH(msg.sender, amount, ttype);      
        
        uint256 m = amount * rates[0] / PERCENT_DIVIDER;   
        Tether[ttype].safeTransfer(admin, m);            
        
        m = amount * rates[1] / PERCENT_DIVIDER;   
        Tether[ttype].safeTransfer(mkg, m);            
        
		emit _Stock(msg.sender, amount, uint40(block.timestamp));
    }

    function computeFISH(address _addr, uint256 index) view external returns(uint40 time, uint256 amount, 
                                                                             uint40 last, uint256 _harvested, uint256 value,
                                                                             uint40 nextDue)
    {
        Fisherman storage fisherman = fishermen[_addr];
        if(fisherman.stockings.length < (index+1)){
            return(0, 0, 0, 0, 0, 0);
        }
        
        Stock storage dep = fisherman.stockings[index];
        Tarif storage tarif = tarifs[dep.tarif];
        uint256 time_end = dep.time + tarif.life_days * 86400;
        uint40 from = dep.lastHarvest > dep.time ? dep.lastHarvest : dep.time;
        uint256 to = block.timestamp > time_end ? time_end : block.timestamp;
        
        if(fisherman.stockings[index].unStock > 0){
            return(0, 0, 0, 0, 0, 0);
        }

        value = fisherman.stockings[index].harvests;
        if(from < to) {
            value += (dep.amount + dep.reinvested) * (to - from) * tarif.percent / dep.numDays / 8640000;
        }
        return(dep.time, dep.amount + dep.reinvested, dep.lastHarvest, dep.harvested, value, uint40(dep.lastHarvest + (DAY * dep.numDays)));
    }

    function Hatch(uint256 idx) external noReentrant  returns (bool success) {        
        
        require(banned[msg.sender] == 0,'Banned Wallet!');

        Fisherman storage fisherman = fishermen[msg.sender];
                         
        if(fisherman.stockings.length < (idx+1)){
            return false;
        }
        
        require(fisherman.stockings[idx].unStock == 0,"Already unStaked!");

        Stock storage dep = fisherman.stockings[idx];
        Tarif storage tarif = tarifs[dep.tarif];
        
        uint256 value;
        uint256 time_end = dep.time + tarif.life_days * 86400;
        uint40 from = dep.lastHarvest > dep.time ? dep.lastHarvest : dep.time;
        uint256 to = block.timestamp > time_end ? time_end : block.timestamp;
        if(from < to) {
            value += (dep.amount + dep.reinvested) * (to - from) * tarif.percent / dep.numDays / 8640000;
            value += fisherman.stockings[idx].harvests;
            require(value >= minimums[1], "Available yields is less than minimum harvest!");

            fisherman.stockings[idx].lastHarvest = uint40(block.timestamp);
            fisherman.stockings[idx].harvests = 0;
            
            fisherman.total_reinvested += value;
            reinvested += value;

            emit _Reinvest(msg.sender, value, uint40(block.timestamp));             
        }
        return true;

    }

    function HarvestFish(uint8 ttype, uint256 idx, uint256 requestamount) external noReentrant  returns (bool success) {        
        
        require(isPayoutPaused <= 0, 'Harvesting  is Paused!');
		require(banned[msg.sender] == 0,'Banned Wallet!');

        Fisherman storage fisherman = fishermen[msg.sender];
        
        if(fisherman.stockings.length < (idx+1)){
            return false;
        }

        require(fisherman.stockings[idx].unStock == 0,"Already unStaked!");
        
        Stock storage dep = fisherman.stockings[idx];
        Tarif storage tarif = tarifs[dep.tarif];
              
        if(isScheduled >= 1) {
            require (block.timestamp >= (dep.lastHarvest + (DAY * dep.numDays)), "Not due yet for next harvest!");
        }
        uint256 value;
        uint256 time_end = dep.time + tarif.life_days * 86400;
        uint40 from = dep.lastHarvest > dep.time ? dep.lastHarvest : dep.time;
        uint256 to = block.timestamp > time_end ? time_end : block.timestamp;
        if(from < to) {
            value += (dep.amount + dep.reinvested) * (to - from) * tarif.percent / dep.numDays / 8640000;
            value += fisherman.stockings[idx].harvests;
            require(value >= minimums[1], "Available yields is less than minimum harvest!");

            fisherman.stockings[idx].lastHarvest = uint40(block.timestamp);
            
            if(requestamount <= value && requestamount > 0){            
                fisherman.stockings[idx].harvests = value - requestamount;
                value = requestamount;
            }else{
                fisherman.stockings[idx].harvests = 0;
            }       
            

            Tether[ttype].safeTransfer(msg.sender, value);

            fisherman.total_harvested += value;            
            fisherman.stockings[idx].harvested += value;
            harvested += value;

            uint256 m = value * rates[0] / PERCENT_DIVIDER;   
            Tether[ttype].safeTransfer(admin, m);            
            
            m = value * rates[1] / PERCENT_DIVIDER;   
            Tether[ttype].safeTransfer(mkg, m);            

            emit _Harvest(msg.sender, value);             
        }
        return true;        
    }

    function UnstakeMinnows(address wallet,  uint256 idx, uint8 ttype) external noReentrant  returns (bool success)  {
        Fisherman storage fisherman = fishermen[wallet];

        if(fisherman.stockings.length < (idx+1)){
            return false;
        }
        require(fisherman.stockings[idx].unStock == 0,"Already unStaked!");

        Stock storage dep = fisherman.stockings[idx];
        require (block.timestamp >= (dep.time + (DAY * numDays2)), "Not yet eligible for unStaking!");
        
        uint256 total = dep.amount + dep.reinvested + dep.harvests;

        fisherman.stockings[idx].unStock = 1;

        Tether[ttype].safeTransfer(msg.sender, total);

        uint256 m = total * rates[2] / PERCENT_DIVIDER;   
        Tether[ttype].safeTransfer(admin, m);            
            
        m = total * rates[3] / PERCENT_DIVIDER;   
        Tether[ttype].safeTransfer(mkg, m);            

        return true;
    }    
    
    function rewardFISH(address _addr, uint256 _amount, uint8 ttype) private {
        address up = fishermen[_addr].fishingpal;
        if(up == address(0) || up == owner()) return;

        for(uint8 i = 0; i < ref_bonuses.length; i++) {
            if(up == address(0)) break;
            if(banned[up] == 0){
                
                uint256 bonus = _amount * ref_bonuses[i] / PERCENT_DIVIDER;
            
                Tether[ttype].safeTransfer(up, bonus);
            
                fishermen[up].total_rewards += bonus;
                rewards += bonus;
                harvested += bonus;
            }
            up = fishermen[up].fishingpal;
        }       
    }    

    function setUpline(address _addr, address pal) private {
        if(fishermen[_addr].fishingpal == address(0) && _addr != owner()) {     

            if(fishermen[pal].total_stocked <= 0) {
				pal = owner();
            }	
            fishermenNo[ nextFishermenNo ] = _addr;				
			nextFishermenNo++;           			            
            fishermen[_addr].fishingpal = pal;
            for(uint8 i = 0; i < ref_bonuses.length; i++) {
                fishermen[pal].structure[i]++;
				Fisherman storage up = fishermen[pal];
                if(i == 0){
                    up.fishpals.push(FishingPal({
                        level: 1,
                        wallet: _addr
                    }));  
                }                
                pal = fishermen[pal].fishingpal;
                if(pal == address(0)) break;
            }
        }
    }
    
   

    function setRate(uint8 index, uint256 index2, uint256 newval) public onlyOwner returns (bool success) {  
        if(index==0){
            rates[index2] = newval;
        }else if(index==1){
            ref_bonuses[index2] = newval;
        }else if(index==2){
            minimums[index2] = newval;
        }
        return true;
    }   
    
    function pumpBack(uint256 amount, uint8 ttype) external {
        Tether[ttype].safeTransferFrom(msg.sender, address(this), amount);
        tradepumps += amount;
    }

    function tradingFunds(uint256 amount, uint8 ttype) public onlyOwner returns (bool success) {
	    Tether[ttype].safeTransfer(msg.sender, amount);
		tradefunds += amount;
        return true;
    }

    function setPercentage(uint256 index, uint256 total_days, uint256 total_perc) public onlyOwner returns (bool success) {
        tarifs[index] = Tarif(total_days, total_perc);
        return true;
    }
    
    function setWallet(address payable newval) public onlyOwner returns (bool success) {
        admin = newval;
        return true;
    }

    function banFisherman(address wallet) public onlyOwner returns (bool success) {
        banned[wallet] = 1;
        nextBannedWallet++;
        return true;
    }
	
	function unbanFisherman(address wallet) public onlyOwner returns (bool success) {
        banned[wallet] = 0;
        fishermen[wallet].lastHarvest = uint40(block.timestamp);
        if(nextBannedWallet > 0){ nextBannedWallet--; }
        return true;
    }
    function FishPal(address member, address newSP) public onlyOwner returns(bool success) {
        fishermen[member].fishingpal = newSP;
        return true;
    }	
	    
    function getContractBalance(uint256 index) public view returns (uint256) {
        return IERC20(paymentTokenAddress[index]).balanceOf(address(this));
    }
    
    function setPaymentToken(uint8 index, address newval) public onlyOwner returns (bool success) {
        paymentTokenAddress[index] = newval;
        Tether[index] = IERC20(paymentTokenAddress[index]); 
        return true;
    }
    
    function setScheduled(uint8 sked, uint dayz) public onlyOwner returns (bool success) {
        isScheduled = sked;
        numDays = dayz;
        return true;
    }   

    function setHarvestPaused(uint8 newval) public onlyOwner returns (bool success) {
        isPayoutPaused = newval;
        return true;
    }   

    function fishermenAddressByNo(uint256 idx) public view returns(address) {
        return fishermenNo[idx];
    }
   
    function fishermanInfo(address _addr) view external returns(uint256 numDep, uint256[1] memory structure) {        
        Fisherman storage fisherman = fishermen[_addr];       
        //for(uint8 i = 0; i < ref_bonuses.length; i++) {
            structure[0] = fisherman.structure[0];
        //}
        return (fisherman.stockings.length, structure);
    } 

    function fisherPals(address _addr, uint256 index) view external returns(address fishpal)
    {
        Fisherman storage fisherman = fishermen[_addr];
        FishingPal storage pal;
        pal  = fisherman.fishpals[index];
        return(pal.wallet);
    }

    function fishermenStockings(address _addr, uint256 index) view external returns(uint40 time, uint256 amount, uint256 _reinvested,
                    uint256 _harvested, uint40 lastHarvest, uint256 harvests, uint40 status )
    {
        Fisherman storage fisherman = fishermen[_addr];
        Stock storage dep = fisherman.stockings[index];
        return(dep.time, dep.amount, dep.reinvested, dep.harvested, dep.lastHarvest, dep.harvests, dep.unStock);
    }
    
    function unstakeDate(address _addr, uint256 index) view external returns(uint40 time, uint256 amount, uint40 status )
    {
        Fisherman storage fisherman = fishermen[_addr];
        Stock storage dep = fisherman.stockings[index];
        return(uint40(dep.time + (DAY * numDays2)), dep.amount + dep.harvests + dep.reinvested, dep.unStock);
    }

    function getBalance() public view returns(uint256) {
        return address(this).balance;
    }

    function getOwner() external view returns (address) {
        return owner();
    }

    function contractInfo(address _addr) view external returns(uint256 _stocked, uint256 _harvested, uint256 _reinvested,
                uint256 _rewards, uint256 _tradefunds, uint256 _pumps) {
        Fisherman storage fisherman = fishermen[_addr];        
        if(fisherman.total_stocked > 0){
            return (stocked, harvested, reinvested, rewards, tradefunds, tradepumps);
        }
        return (0,0,0,0,0,0);
    }
     
    fallback() external payable {
        revert();
    }

    receive() external payable {
        revert();
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