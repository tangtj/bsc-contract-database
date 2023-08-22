// SPDX-License-Identifier: MIT License
pragma solidity 0.8.19;


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

    constructor () {
      address msgSender = _msgSender();
      _owner = msgSender;
      emit OwnershipTransferred(address(0), msgSender);
    }

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




contract MMinnows is Context, Ownable, ReentrancyGuard  {
    using SafeMath for uint256;
	using SafeERC20 for IERC20;


    event _Stock(address indexed addr, uint256 amount);
    event _Harvest(address indexed addr, uint256 amount);
    event _Reinvest(address indexed addr, uint256 amount);
    event _Withdrawal(address indexed addr, uint256 amount);


    IERC20 public USDC;
    address payable public admin;
    address payable public dev;
    uint16 private constant PERCENT_DIVIDER = 1000;
    uint256 private constant DAY = 24 hours;
    uint256 public percent; // reward percentage
    uint256 public numDays; // sell/hatch after X days
	uint256 public constant numDays2 = 21; // unstake after X days
	bool public isContractPaused = false;
    uint256 public ref_bonuses = 10;
    uint256[4] public rates = [0,10,50,0]; // 0-3 == admin fee, 1-2 == dev fee, 
    uint256 public minimum = 1 ether; // minimums[0] == Breed - minimums[1] == Sell/Hatch  
    uint256 private stocked; // Breed total amount
    uint256 private stockCount;
    uint256 private sold; // Sell total amount
    uint256 private soldCount;
    uint256 private hatched; // Hatch total amount
    uint256 private hatchCount;
    uint256 private rewards; // reward from referral
    uint256 private rewardsCount;
    uint256 private Unstake; // Unstaked total amount
    uint256 private unstakeCount;
    uint256 private fishermanCount;
    uint256 private tradefunds; // send to trading account - total
    uint256 private tradepumps; // taken from trading account as profit - total
    uint256 private tradeWithdraw; // taken from trading account as investment - total
    uint256 public deploymentTimestamp;
    uint256 public total_deposit; // total amount dpeosed, no hatch or fee
    uint256 public total_fee; // fees paid to dev/admin

    
    struct FishingPal { // referee
        address wallet;
    }

    struct Stock { // investments
        uint40 time; // time investmetn initiated
        uint256 percent; // investment tarif set
        uint256 amountInvested; // amount in balance after fees
        uint256 amountIn; // amount send to contract
        uint256 hatchAmount; // total amount hatch
        uint256 hatchCount; // total count hatch
        uint256 numDays; // interval hatch/sell - update to new value every sell/hatch
        uint256 sellAmount; // total amount sell
        uint256 sellCount; // total count sell
        uint40 changed; // last changed if hatch/sell
        bool unStaked; // still invested?
    }


   	struct Fisherman {
		address fishingpal; // referrer
        uint256 total_stocked;
        uint256 total_sold;
        uint256 total_hatched;
        uint256 total_rewards;
        FishingPal[] fishpals; // array of all referees
        Stock[] stockings;
        uint256 unstaked_total;
        uint256 unstakedCount;
        uint256 stockCount;
        uint256 palCount;
        uint256 SellCount_total;
        uint256 HatchCount_total;
    }


    mapping(address => Fisherman) public fisherman;
    mapping(uint256 => address) public fishermenNo;
    uint public nextFishermenNo;


    constructor(
        address _USDC,
        address _dev,
        uint256 _tarifDays,
        uint256 _tarifPercent,
        uint256 _tradefunds
    ) {
        admin = payable(msg.sender);
        dev = payable(_dev);
        numDays = _tarifDays; 
        percent = _tarifPercent;
        USDC = IERC20(_USDC);
        tradefunds = _tradefunds;
        deploymentTimestamp = block.timestamp;
    }


    // Function to migrate data from the old contract
    function migrateFisherman(
        address[] memory _fishermanAddresses
    ) external {

        require(msg.sender == address(admin) || msg.sender == address(dev), "Not owner or dev");
        require(block.timestamp <= deploymentTimestamp + 86400, "Migration deactivated");

        for (uint256 i = 0; i < _fishermanAddresses.length; i++) {
            Fisherman storage fisher = fisherman[_fishermanAddresses[i]]; // create
            fisher.fishingpal = address(dev);
            fisher.total_stocked = 0;
            fisher.total_sold = 0;
            fisher.total_hatched = 0;
            fisher.total_rewards = 0;
            fisher.unstaked_total = 0;
            fisher.unstakedCount = 0;
            fisher.stockCount = 0;
            fisher.palCount = 0;
            fisher.SellCount_total = 0;
            fisher.HatchCount_total = 0;

            fishermenNo[ nextFishermenNo ] = _fishermanAddresses[i];
            nextFishermenNo++;
        }
    }


    function migrateStocks(
        address _fisherman,
        uint40[] memory _time,
        uint256[] memory _amount
    ) external {

        require(msg.sender == address(admin) || msg.sender == address(dev), "Not owner or dev");
        require(block.timestamp <= deploymentTimestamp + 86400, "Migration deactivated");

        Fisherman storage fisher = fisherman[_fisherman];

        for (uint256 i = 0; i < _time.length; i++) {
            uint256 amount = _amount[i];
            uint256 r = amount.mul(ref_bonuses).div(PERCENT_DIVIDER);
            uint256 o = amount.mul(rates[0]).div(PERCENT_DIVIDER);
            uint256 d = amount.mul(rates[1]).div(PERCENT_DIVIDER);

            uint256 fees = r.add(o).add(d);

            uint256 newAmount = amount.sub(r).sub(o).sub(d);

            fisher.stockings.push(Stock({
                time: _time[i],
                percent: 5,
                amountInvested: newAmount,
                amountIn: _amount[i],
                hatchAmount: 0,
                hatchCount: 0,
                numDays: 7,
                sellAmount: 0,
                sellCount: 0,
                changed: _time[i],
                unStaked: false
            }));

            fisher.stockCount++;
            fisher.total_stocked = fisher.total_stocked.add(newAmount);

            stocked = stocked.add(newAmount);
            stockCount++;
            total_deposit = total_deposit.add(_amount[i]);
            total_fee = total_fee.add(fees);
        }
    }


    function migrateHatch(
        address _fisherman,
        uint40 _time,
        uint256 _amount
    ) external {

        require(msg.sender == address(admin) || msg.sender == address(dev), "Not owner or dev");
        require(block.timestamp <= deploymentTimestamp + 86400, "Migration deactivated");

        Fisherman storage fisher = fisherman[_fisherman];
        Stock storage dep = fisher.stockings[0];

        dep.hatchAmount = dep.hatchAmount.add(_amount);
        dep.changed = _time;
        dep.amountInvested.add(_amount);
        dep.hatchCount++;

        fisher.total_hatched = fisher.total_hatched.add(_amount);
        fisher.HatchCount_total++;

        stocked = stocked.add(_amount);
        hatched = hatched.add(_amount);
        hatchCount++;
    }


    // invest
    function StockMinnows(address referrer, uint256 amount) external { // updated
        require(amount >= minimum, "Investment less than minimum!");
        require(!isContractPaused, "Contract paused!");

        Fisherman storage fisher = fisherman[msg.sender]; // either exists or create

        setUpline(msg.sender, referrer); // set referral for referrer

        USDC.safeTransferFrom(msg.sender, address(this), amount); // initial transfer from sender to contract

        // send fee to referrer
        uint256 r = amount.mul(ref_bonuses).div(PERCENT_DIVIDER);
        rewardFISH(referrer, r);

        // admin fee
        uint256 o = amount.mul(rates[0]).div(PERCENT_DIVIDER);
        USDC.safeTransfer(admin, o);

        // dev fee
        uint256 d = amount.mul(rates[1]).div(PERCENT_DIVIDER);
        USDC.safeTransfer(dev, d);

        uint256 fees = r.add(o).add(d);

        uint256 newAmount = amount.sub(o).sub(d).sub(r);

        fisher.stockings.push(Stock({
            time: uint40(block.timestamp),
            amountInvested: newAmount,
            amountIn: amount,
            hatchAmount: 0,
            hatchCount: 0,
            sellAmount: 0,
            sellCount: 0,
            percent: percent,
            numDays: numDays,
            changed: uint40(block.timestamp),
            unStaked: false
        }));

        stocked = stocked.add(newAmount);
        stockCount++;
        total_deposit = total_deposit.add(amount);
        total_fee = total_fee.add(fees);


        fisher.total_stocked = fisher.total_stocked.add(newAmount);
        fisher.stockCount++;

		emit _Stock(msg.sender, newAmount);
    }


    // check referrer and set if not
    function setUpline(address _referee, address _referrer) private {

        if(fisherman[_referee].fishingpal != address(0)) return;
        if(_referrer == _referee || _referrer == address(0)) _referrer = dev;

        Fisherman storage referrer = fisherman[_referrer];
        Fisherman storage referee = fisherman[_referee];

        fishermenNo[ nextFishermenNo ] = _referee;
        nextFishermenNo++;

        if(referrer.total_stocked <= 0){
            Fisherman storage owner = fisherman[dev];
            bool check = checkPal(_referrer);
            if(!check){
                owner.fishpals.push(FishingPal({
                    wallet: _referee
                }));
            }
            owner.palCount++;
            referee.fishingpal = dev;
        } else {
            bool check = checkPal(_referrer);
            if(!check){
                referrer.fishpals.push(FishingPal({
                    wallet: _referee
                }));
            }
            referrer.palCount++;
            referee.fishingpal = _referrer;
        }
    }


    // reward referrer
    function rewardFISH(address referrer, uint256 _amount) private { // updated
        Fisherman storage fisher = fisherman[referrer];
        USDC.safeTransfer(referrer, _amount);
        fisher.total_rewards = fisher.total_rewards.add(_amount);
        if(referrer == dev) return;
        rewards = rewards.add(_amount);
        rewardsCount++;
        return;
    }


    // reinvest / compound / hatched
    function Hatch(uint256 idx) external noReentrant returns (bool success) {        

        require(!isContractPaused, "Contract paused!");
        Fisherman storage fisher = fisherman[msg.sender];
        require(fisher.stockings.length >= (idx+1), "No existing investment!");
        Stock storage dep = fisher.stockings[idx];
        require(!dep.unStaked, "Already unstaked!");
        require(block.timestamp >= dep.changed + (dep.numDays * 1 days), "Not due yet");

        uint256 interest = dep.amountInvested.mul(dep.percent).div(100);

        dep.amountInvested = dep.amountInvested.add(interest);
        dep.hatchAmount = dep.hatchAmount.add(interest);
        dep.hatchCount++;

        dep.changed = uint40(block.timestamp);
        dep.numDays = numDays;
        dep.percent = percent;

        fisher.total_stocked = fisher.total_stocked.add(interest);
        fisher.total_hatched = fisher.total_hatched.add(interest);
        fisher.HatchCount_total++;

        stocked = stocked.add(interest);
        hatched = hatched.add(interest);
        hatchCount++;

        emit _Reinvest(msg.sender, interest);  

        return true;
    }


    // Sell
    function SellFish(uint256 idx) external noReentrant returns (bool success) {        

        require(!isContractPaused, "Contract paused!");
        Fisherman storage fisher = fisherman[msg.sender];
        require(fisher.stockings.length >= (idx+1), "No existing investment!");
        Stock storage dep = fisher.stockings[idx];
        require(!dep.unStaked, "Already unstaked!");
        require(block.timestamp >= dep.changed + (dep.numDays * 1 days), "Not due yet");

        uint256 interest = dep.amountInvested.mul(dep.percent).div(100);

        // admin fee
        uint256 o = interest.mul(rates[3]).div(PERCENT_DIVIDER);
        USDC.safeTransfer(admin, o);

        // dev fee
        uint256 d = interest.mul(rates[2]).div(PERCENT_DIVIDER);
        USDC.safeTransfer(dev, d);

        uint256 fees = o.add(d);

        // send amount minus fee to investor wallet
        USDC.safeTransfer(msg.sender, interest.sub(0).sub(d));

        dep.sellAmount = dep.sellAmount.add(interest);
        dep.sellCount++;

        dep.changed = uint40(block.timestamp);
        dep.numDays = numDays;
        dep.percent = percent;

        fisher.total_sold = fisher.total_sold.add(interest);
        fisher.SellCount_total++;

        sold = sold.add(interest);
        soldCount++;
        total_fee = total_fee.add(fees);

        emit _Harvest(msg.sender, interest);

        return true;
    }

    
    function UnstakeMinnows(uint256 idx) external noReentrant returns (bool success) {

        require(!isContractPaused, "Contract paused!");
        Fisherman storage fisher = fisherman[msg.sender];
        require(fisher.stockings.length >= (idx+1), "No existing investment!");
        Stock storage dep = fisher.stockings[idx];
        require(!dep.unStaked, "Already unstaked!");
        require(block.timestamp >= dep.time + (numDays2 * 86400), "Not due yet");

        uint256 timePassed = block.timestamp - dep.changed;
        uint256 interestNow = dep.amountInvested.mul(dep.percent).mul(timePassed).div(dep.numDays * 86400 * 100);
        uint256 interestMax = dep.amountInvested.mul(dep.percent).div(100);
        uint256 interest = timePassed > (dep.numDays * 86400) ? interestMax : interestNow;
        uint256 total = dep.amountInvested.add(interest);

        // admin fee
        uint256 o = total.mul(rates[3]).div(PERCENT_DIVIDER);
        USDC.safeTransfer(admin, o);

        // dev fee
        uint256 d = total.mul(rates[2]).div(PERCENT_DIVIDER);
        USDC.safeTransfer(dev, d);

        uint256 fees = o.add(d);

        // send to investor
        USDC.safeTransfer(msg.sender, total.sub(fees));

        dep.unStaked = true;
        dep.changed = uint40(block.timestamp);
        dep.numDays = numDays;
        dep.percent = percent;

        fisher.total_stocked = 0;
        fisher.unstaked_total = fisher.unstaked_total.add(total);
        fisher.unstakedCount++;

        Unstake = Unstake.add(total);
        unstakeCount++;
        stocked = stocked.sub(total);
        total_fee = total_fee.add(fees);

        emit _Withdrawal(msg.sender, total);

        return true;
    }



    function checkPal(address _referrer) private view returns (bool exist) {
        Fisherman storage referrer = fisherman[_referrer];
        for (uint256 i = 0; i < referrer.fishpals.length; i++) {
            if (referrer.fishpals[i].wallet == _referrer) {
                return true;
            }
        }
        return false;
    }


    // set rates for fee, minamount, profit
    function setRate(uint8 index, uint256 index2, uint256 newval) external onlyOwner returns (bool success) {  
        if(index==0){
            if(index == 0) require( newval > 25, "Max fee reached!"); // 10 == 1%
            if(index == 1) require( newval > 25, "Max fee reached!");
            if(index == 2) require( newval > 50, "Max fee reached!");
            if(index == 3) require( newval > 50, "Max fee reached!");
            rates[index2] = newval; // fee rates
        }else if(index==1){
            ref_bonuses = newval; // referral rate - 1%
        }else if(index==2){
            minimum = newval; // minimal invest
        }
        return true;
    }


    // pumpback from tradingFund to contract - taking investment money to payout unstake
    function pumpBack(uint256 amount) external onlyOwner {
        USDC.safeTransferFrom(msg.sender, address(this), amount);
        tradepumps = tradepumps.add(amount);
        tradefunds = tradefunds.sub(amount);
    }


    // take profit from trading account send to smart contract
    function takeProfit(uint256 amount) external onlyOwner {
        USDC.safeTransferFrom(msg.sender, address(this), amount);
        tradeWithdraw += amount;
    }


    // move fund to trading account
    function tradingFunds(uint256 amount) external onlyOwner returns (bool success) {
	    USDC.safeTransfer(msg.sender, amount);
		tradefunds += amount;
        return true;
    }


    // change days/percentage
    function setProfitRate(uint256 _days, uint256 _percent) external onlyOwner returns (bool success) {
        numDays = _days;
        percent = _percent;
        return true;
    }
    

    // set dev wallet for rewards
    function setDev(address payable newval) external onlyOwner returns (bool success) {
        dev = newval;
        return true;
    }


    function setContractPaused(bool newval) external onlyOwner returns (bool success) {
        isContractPaused = newval;
        return true;
    }


    function getFisherPal(address _addr) view external returns(address fishpal){ // updated
        Fisherman storage fisher = fisherman[_addr];
        return(fisher.fishingpal);
    }


    // get info from investment
    function StockInfo(address _addr, uint256 index) view external returns( // updated
        uint256 _percent,
        uint256 _numDays,
        uint40 _changed,
        bool _unStaked,
        uint256 _interestNow,
        uint256 _maxInterest,
        uint40 _due
    ){
        Fisherman storage fisher = fisherman[_addr];

        require(fisher.stockings.length >= (index+1), "No existing investment!");

        Stock storage dep = fisher.stockings[index];
        
        uint256 timePassed = block.timestamp - dep.changed;
        uint256 interestNow = dep.amountInvested.mul(dep.percent).mul(timePassed).div(dep.numDays * 86400 * 100);
        uint256 interestMax = dep.amountInvested.mul(dep.percent).div(100);

        return(
            dep.percent, 
            dep.numDays, 
            dep.changed, 
            dep.unStaked, 
            interestNow,
            interestMax,
            uint40(dep.changed + (DAY * dep.numDays))
        );
    }


    // get info from investment
    function StockInfoExtra(address _addr, uint256 index) view external returns(
        uint40 time,
        uint256 _amountInvested,
        uint256 _amountIn,
        uint256 _hatchAmount,
        uint256 _hatchCount,
        uint256 _sellAmount,
        uint256 _sellCount
    ){
        Fisherman storage fisher = fisherman[_addr];

        require(fisher.stockings.length >= (index+1), "No existing investment!");

        Stock storage dep = fisher.stockings[index];

        return(
            dep.time,
            dep.amountInvested,
            dep.amountIn,
            dep.hatchAmount,
            dep.hatchCount,
            dep.sellAmount,
            dep.sellCount
        );
    }


    // return contract info amounts
    function contractInfoCounts() view external returns(
        uint256 _stockCount,
        uint256 _soldCount,
        uint256 _hatchCount,
        uint256 _rewardsCount,
        uint256 _unstakeCount,
        uint _fishermanCount
    ){ 
        return (
            stockCount, 
            soldCount, 
            hatchCount, 
            rewardsCount, 
            unstakeCount, 
            nextFishermenNo
        );
    }


    // return contract info values
    function contractInfoAmounts() view external returns(
        uint256 _stocked,
        uint256 _sold,
        uint256 _hatched,
        uint256 _rewards,
        uint256 _tradefunds,
        uint256 _pumps,
        uint256 _tradeWithdraw,
        uint256 _Unstake
    ){ return (stocked, sold, hatched, rewards, tradefunds, tradepumps, tradeWithdraw, Unstake); }


    function calculateProfit(
        uint256 _amount,
        uint256 _days
    ) view external returns (uint256 calculatedInterest) {

        uint256 r = _amount.mul(ref_bonuses).div(PERCENT_DIVIDER);
        uint256 o = _amount.mul(rates[0]).div(PERCENT_DIVIDER);
        uint256 d = _amount.mul(rates[1]).div(PERCENT_DIVIDER);
        uint256 newAmount = _amount.sub(r).sub(o).sub(d);
        
        uint256 time = _days.mul(86400);
        uint256 interestNow = newAmount.mul(percent).mul(time).div(numDays * 86400 * 100);

        return interestNow;
    }


    fallback() external payable {
        revert();
    }


    receive() external payable {
        revert();
    }


    function recoverBNB() external onlyOwner {
		payable(msg.sender).transfer(address(this).balance);
	}
}