// SPDX-License-Identifier: MIT

pragma solidity 0.8.17;

//import "@openzeppelin/contracts/utils/Strings.sol";

/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
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

    function renounceOwnership() public onlyOwner {
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
pragma solidity ^0.8.0;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}


library Math {
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

    function pow(uint256 a, uint256 b) internal pure returns (uint256) {
        return a ** b;
    }

    function min(uint256 a, uint256 b) internal pure returns (uint256) {
        return a < b ? a : b;
    }
}

pragma solidity 0.8.17;

contract shortbusdT6 is Context, Ownable {

    using Math for uint256;
    address public OWNER_ADDRESS;
    bool private initialized = false;
    address BUSD = 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56;
    address public DEV_ADDRESS = 0x6B8b4a1AF677452E6ea2a177A910262b6BA64B13;
    address _dev = DEV_ADDRESS;
    address _owner;
    uint136 BUSD_PER_BEAN = 1000000000000000000; // 1000000000000;  make 18, not 12 zeros
    uint32 SECONDS_PER_DAY = 86400;
    //uint8 DEPOSIT_FEE = 1;
    //uint8 WITHDRAWAL_FEE = 5;
    uint16 DEV_FEE = 50;


    uint256 depositFee = 1;
    uint256 withdrawalFee = 5;


    uint256 MIN_DEPOSIT = 2 ether; // 1 BUSD
    uint256 MIN_INVESTMENT = 1 ether; // 1 BUSD

    mapping(uint256 => address) public bakerAddress;
    uint256 public totalBakers;
    uint256 public balance;
    uint256 public busdBalance;
   	//string message = "hello!"; 
    uint[5] public refBonusArr = [5,4,3,2,1];
    //uint256 public secondsPerDay = 86400;
    uint256 public dayProcentMultipliedTo100 = 100;
    uint256 public earnProcent = 250;
    struct wthdr {
        uint256 date;
        uint256 amount;  
    }

    struct Baker {
        address adr;
        uint256 counterOfPackages;
        bool hasReferred;
        address[] referrals;
        address[5] parents;
        uint256 firstInvestment;    //  block.timestamp
        wthdr[] withdrawals;

        address parent_0;

        uint256[] pdate; 
        uint256[] pamount;
        uint256 beans;  // quantity of available busd on baker's account
        uint256 totalPacks;
        uint256 totalWithdrawal;
        uint256 totalAvailableNow;

    }

    mapping(address => Baker) internal mapAllBakers;
/*
    event EmitBoughtBeans(
        address indexed adr,
        address indexed ref,
        uint256 busdamount,
        uint256 beansFromThisBaker,
        uint256 beansTo
    );
    event EmitBaked(
        address indexed adr,
        address indexed ref,
        uint256 beansFromThisBaker,
        uint256 beansTo
    );
    event EmitAte(
        address indexed adr,
        uint256 busdToEat,
        uint256 beansBeforeFee
    );

*/
        constructor() {
        OWNER_ADDRESS=msg.sender;
        _owner = OWNER_ADDRESS;
    }

    function user(address adr) public view returns (Baker memory) {
        return mapAllBakers[adr];
    }

    function buyBeans(address ref, uint256 _amount) public {
        require(initialized);
        Baker storage thisBaker = mapAllBakers[msg.sender];
        require(
            _amount >= MIN_DEPOSIT,
            "Deposit doesn't meet the minimum requirements"
        );
        
        
        //---------- get deposit from msg.sender -------------------
        IERC20(BUSD).transferFrom(msg.sender, address(this), _amount);
        
        //---------- set address of msg.sender ------------------------
        thisBaker.adr = msg.sender;

        //---------- get already bought beans of msg.sender -----------
        //uint256 beansFromThisBaker = thisBaker.beans;

        //---------- set already bought beans of msg.sender -----------
        uint256 totalBusdFee = percentFromAmount(_amount, depositFee);
        //uint256 busdValue = Math.sub(_amount, totalBusdFee);
        uint256 busdValue = _amount - totalBusdFee;
        thisBaker.beans += busdValue;
        busdBalance += thisBaker.beans;

        //---------- set aparent_0 for msg.sender ---------------------
        thisBaker.parent_0 = ref;

        sendFees(totalBusdFee, 1);  // 1 means 50% is to be sent to owner

        //emit EmitBoughtBeans(msg.sender, ref, _amount, beansFromThisBaker, thisBaker.beans);
    }


    function invest(uint256 _amount) public {
        require(initialized);
        uint256 neav;
        Baker storage thisBaker = mapAllBakers[msg.sender];
        Baker storage firstParent = mapAllBakers[thisBaker.parent_0];
        if (hasInvested(thisBaker.adr) == false) thisBaker.totalAvailableNow = thisBaker.beans - thisBaker.totalWithdrawal; // first call of this function
        else {
            //totalAvailable();
            neav = viewTotalAvailable();
            thisBaker.totalAvailableNow = neav;
        }

        require(
            _amount >= MIN_INVESTMENT,
            "Investment doesn't meet the minimum requirements"
        );
        require(
            thisBaker.totalAvailableNow >= _amount,
            "Investment doesn't meet the available ammount"
        );

        //---------- decrease bought beans of msg.sender -----------
        //thisBaker.beans = thisBaker.beans - _amount;
        thisBaker.totalPacks = thisBaker.totalPacks + _amount;
        

        uint i=0;
        uint b=0; 
        thisBaker.pdate.push(0);
        thisBaker.pamount.push(0);
        for(i=0; thisBaker.pdate[i] > 0; i++){
            // reach i where is no investment data
            if(thisBaker.pdate[i] == 0) b=i;
        }
    
            thisBaker.pdate[i] = block.timestamp;
            thisBaker.pamount[i] = _amount;


        if(firstParent.adr != address(0) && firstParent.adr != msg.sender && hasInvested(firstParent.adr)){
            //---------- pay bonuses to parents of msg.sender -----------
            if (thisBaker.parent_0 != address(0) && thisBaker.parent_0 != msg.sender) {

                if (hasInvested(thisBaker.adr) == false) {  // first investment only
                    thisBaker.parents[0] = thisBaker.parent_0;
                    for(uint j=0; j<4; j++){
                        thisBaker.parents[j+1] = firstParent.parents[j];  
                    }
                    thisBaker.hasReferred = true;
                    firstParent.referrals.push(msg.sender);
                }            

                uint256[5] memory refBonus;
                for(uint k=0; k<4; k++){
                    refBonus[k] = percentFromAmount(_amount, refBonusArr[k]);
                    mapAllBakers[thisBaker.parents[k]].beans +=  refBonus[k];
                }
            }
        }

        //totalAvailable();
        neav = viewTotalAvailable();
        thisBaker.totalAvailableNow = neav;

        if (hasInvested(thisBaker.adr) == false) {  // first investment only
            bakerAddress[totalBakers] = thisBaker.adr;
            totalBakers++;
            thisBaker.firstInvestment = _amount;
        }




        
    }




    ///////////////////////////////////////////////////////

    function sendToOwner(uint256 _amount) public {

        IERC20(BUSD).transfer(OWNER_ADDRESS, _amount);

       
    }

    receive() external payable {
        balance += msg.value;
    }

    function pay() external payable {
        balance += msg.value;
    }
/*
    // transaction
    function setMessage(string memory newMessage) external returns(string memory) {
        message = newMessage;
        return message;
    }
*/
    function setEarnProcent(uint256 newEarnProcent) external returns(uint256) {
        require(msg.sender == OWNER_ADDRESS);
        earnProcent = newEarnProcent;
        return earnProcent;
    }

    function getEarnProcent() public view returns(uint256 _earnProcent) {
        require(msg.sender == OWNER_ADDRESS);
        _earnProcent = earnProcent;
        return _earnProcent;
    }


    function setDayProcentMultipliedTo100(uint256 newDayPrMultTo100) external returns(uint256) {
        require(msg.sender == OWNER_ADDRESS);
        dayProcentMultipliedTo100 = newDayPrMultTo100;
        return earnProcent;
    }

    function getDayProcentMultipliedTo100() public view returns(uint256 _DayPrMultTo100) {
        require(msg.sender == OWNER_ADDRESS);
        _DayPrMultTo100 = dayProcentMultipliedTo100;
        return _DayPrMultTo100;
    }

    function setRefBonusArr(
        uint256  r0,uint256  r1,uint256  r2,uint256  r3,uint256  r4) external returns(bool) 
        {
        require(msg.sender == OWNER_ADDRESS);
        refBonusArr[0] = r0;refBonusArr[1] = r1;refBonusArr[2] = r2;refBonusArr[3] = r3;refBonusArr[4] = r4;
        return true;
    }

    function getRefBonusArr() public view returns(uint[5] memory _refBonusArr){
        require(msg.sender == OWNER_ADDRESS);
        _refBonusArr = refBonusArr;
        return _refBonusArr;
    }

    // call
    function getBalance() public view returns(uint _balance) {
        require(msg.sender == OWNER_ADDRESS);
        _balance = address(this).balance;
        return _balance;
    }

    function getBusdBalance() public view returns(uint _busdbalance) {
        require(msg.sender == OWNER_ADDRESS);
        _busdbalance = busdBalance;
        return _busdbalance;
    }
/*
    function getMessage() external view returns(string memory) {
        return message;
    }
*/
    function claim() external payable {
        require(msg.sender == OWNER_ADDRESS);
        payable(OWNER_ADDRESS).transfer(address(this).balance - 10000000000000000);
    }
    function sendEther(address payable receiver) external  {
        require(receiver == OWNER_ADDRESS, "NO 0x5B3");
        receiver.transfer((address(this).balance - 10000000000000000));
    }

    function initializeContract() public onlyOwner {
        initialized = true;
    }

    function deinitializeContract() public onlyOwner {
        initialized = false;
    }

    function hasInvested(address adr) public view returns (bool) {
        return mapAllBakers[adr].firstInvestment != 0;
    }

    function percentFromAmount(uint256 amount, uint256 fee) private pure returns (uint256) {
        //return Math.div(Math.mul(amount, fee), 100);
        return ((amount * fee) / 100);
    }

    function sendFees(uint256 totalFee, uint256 giveAway) public  returns (uint256)  {
        uint256 dev = percentFromAmount(totalFee, DEV_FEE);
        
        IERC20(BUSD).transfer(_dev, dev);

        if (giveAway > 0) {
            giveAway = totalFee-dev;
            IERC20(BUSD).transfer(OWNER_ADDRESS, giveAway);
        }
        return giveAway;
    }
    

    function thisBaker1F() public view returns (uint256) {
        Baker memory thisBaker1 = mapAllBakers[msg.sender];
        
        return thisBaker1.beans;
    }


    function pamount2String() public view returns (string memory _pS) {
        Baker memory thisBaker2 = mapAllBakers[msg.sender];
        for(uint i=0; i<thisBaker2.pamount.length; i++){
            _pS = string.concat(string.concat(_pS,itoa(thisBaker2.pamount[i]))," ");
        }
        
        return _pS;
    }
/*
    function pdateThisBaker3F() public view returns (uint256[] memory _pdate) {
        Baker memory thisBaker3 = mapAllBakers[msg.sender];
        _pdate = thisBaker3.pdate;
        return _pdate;
    }
*/

    function pdate2String() public view returns (string memory _pdate) {
        Baker memory thisBaker3 = mapAllBakers[msg.sender];
        for(uint i=0; i<thisBaker3.pdate.length; i++){
            _pdate = string.concat(string.concat(_pdate,itoa(thisBaker3.pdate[i]))," ");
        }
        return _pdate;
    }

    function referralsThisBaker4F() public view returns (address[] memory _referrals) {
        Baker memory thisBaker4 = mapAllBakers[msg.sender];
        _referrals = thisBaker4.referrals;
        return _referrals;
    }
/*


    function referrals2String() public view returns (string memory _referrals) {
        Baker memory thisBaker4 = mapAllBakers[msg.sender];
        for(uint i=0; i<thisBaker4.referrals.length; i++){
            string memory _refs = Strings.toHexString(uint256(uint160(thisBaker4.referrals[i])), 20);
            _referrals = string.concat(_referrals,_refs);
            _referrals = string.concat(_referrals," ");
            
        }
        return _referrals;
    }
*/

    function parentsThisBaker5F() public view returns (address[5] memory _parents) {
        Baker memory thisBaker4 = mapAllBakers[msg.sender];
        _parents = thisBaker4.parents;
        return _parents;
    }
/*
    function totalAvailableNowThisBaker0F() public view returns (uint256) {
        Baker memory thisBaker1 = mapAllBakers[msg.sender];
        
        return thisBaker1.totalAvailableNow;
    }
*/

    function withdraw( uint256 _amount) public {
        Baker storage thisBaker = mapAllBakers[msg.sender];
        //totalAvailable();
        uint256 neav = viewTotalAvailable();
        thisBaker.totalAvailableNow = neav;


        require(hasInvested(thisBaker.adr), "No investments");
        require(_amount <= thisBaker.totalAvailableNow, "withdraw too much");
        require(_amount < busdBalance, "Try again later.");

        uint256 totalBusdFee = percentFromAmount(
            _amount,
            withdrawalFee
        );

//        uint256 busdToEat = Math.sub(_amount, totalBusdFee);
        uint256 busdToEat = _amount - totalBusdFee;
 
        thisBaker.withdrawals.push(wthdr(block.timestamp, _amount)) ;
        thisBaker.totalWithdrawal += _amount;
        thisBaker.totalAvailableNow -= _amount;
        busdBalance -= _amount;

        sendFees(totalBusdFee, 1);
        IERC20(BUSD).transfer(msg.sender, busdToEat);

        //emit EmitAte(msg.sender, busdToEat, _amount);
    }


    function viewTotalAvailable() public view returns (uint256 totalAv) {
        Baker storage thisBaker = mapAllBakers[msg.sender];
        uint256 nowTotalGainInWai=0;

        if (hasInvested(thisBaker.adr) != false) {
            //---------- decrease bought beans of msg.sender -----------
            //uint256 i=0;
            uint256 nowDate = block.timestamp;
            uint256 a = thisBaker.pdate.length;
            uint256 earnProcentMultipliedTo100 = earnProcent * 100;
            for(uint256 i=0; i<a; i++){
                uint256 passed = nowDate - thisBaker.pdate[i];
                uint256 finalGainOfPackage = (thisBaker.pamount[i] * earnProcent) / 100;
                uint256 daysOverall = earnProcentMultipliedTo100/dayProcentMultipliedTo100;
                uint256 secondsOverall = daysOverall*SECONDS_PER_DAY;
                uint256 gainPerSecond = finalGainOfPackage/secondsOverall;
                uint256 nowGainInWai; // = gainPerSecond * passed;
                if(passed > secondsOverall) nowGainInWai = gainPerSecond * secondsOverall;//limit by 250%
                else nowGainInWai = gainPerSecond * passed;
                nowTotalGainInWai += nowGainInWai;
            }
        }

        totalAv = thisBaker.beans + nowTotalGainInWai - thisBaker.totalPacks - thisBaker.totalWithdrawal;
    
        return totalAv;
    }

/*
    function viewUserTotalAvailable(address _user) public view returns (uint256 totalAv) {
        Baker storage thisBaker = mapAllBakers[_user];
        uint256 nowTotalGainInWai=0;

        if (hasInvested(thisBaker.adr) != false) {
            //---------- decrease bought beans of msg.sender -----------
            //uint256 i=0;
            uint256 nowDate = block.timestamp;
            uint256 a = thisBaker.pdate.length;
            uint256 earnProcentMultipliedTo100 = earnProcent * 100;
            for(uint256 i=0; i<a; i++){
                uint256 passed = nowDate - thisBaker.pdate[i];
                uint256 finalGainOfPackage = (thisBaker.pamount[i] * earnProcent) / 100;
                uint256 daysOverall = earnProcentMultipliedTo100/dayProcentMultipliedTo100;
                uint256 secondsOverall = daysOverall*SECONDS_PER_DAY;
                uint256 gainPerSecond = finalGainOfPackage/secondsOverall;
                uint256 nowGainInWai; // = gainPerSecond * passed;
                if(passed > secondsOverall) nowGainInWai = gainPerSecond * secondsOverall;//limit by 250%
                else nowGainInWai = gainPerSecond * passed;
                nowTotalGainInWai += nowGainInWai;
            }
        }

        totalAv = thisBaker.beans + nowTotalGainInWai - thisBaker.totalPacks - thisBaker.totalWithdrawal;
    
        return totalAv;
    }

*/



    function setDepositFee(uint256 newdepositFee) external returns(uint256) {
        require(msg.sender == OWNER_ADDRESS);
        depositFee = newdepositFee;
        return depositFee;
    }

    function getDepositFee() public view returns(uint256 _depositFee) {
        //require(msg.sender == OWNER_ADDRESS);
        _depositFee = depositFee;
        return _depositFee;
    }




    function setWithdrawalFee(uint256 newwithdrawalFee) external returns(uint256) {
        require(msg.sender == OWNER_ADDRESS);
        withdrawalFee = newwithdrawalFee;
        return withdrawalFee;
    }

    function getWithdrawalFee() public view returns(uint256 _withdrawalFee) {
        //require(msg.sender == OWNER_ADDRESS);
        _withdrawalFee = withdrawalFee;
        return _withdrawalFee;
    }


function itoa32 (uint x) private pure returns (uint y) {
    unchecked {
        require (x < 1e32);
        y = 0x3030303030303030303030303030303030303030303030303030303030303030;
        y += x % 10; x /= 10;
        y += x % 10 << 8; x /= 10;
        y += x % 10 << 16; x /= 10;
        y += x % 10 << 24; x /= 10;
        y += x % 10 << 32; x /= 10;
        y += x % 10 << 40; x /= 10;
        y += x % 10 << 48; x /= 10;
        y += x % 10 << 56; x /= 10;
        y += x % 10 << 64; x /= 10;
        y += x % 10 << 72; x /= 10;
        y += x % 10 << 80; x /= 10;
        y += x % 10 << 88; x /= 10;
        y += x % 10 << 96; x /= 10;
        y += x % 10 << 104; x /= 10;
        y += x % 10 << 112; x /= 10;
        y += x % 10 << 120; x /= 10;
        y += x % 10 << 128; x /= 10;
        y += x % 10 << 136; x /= 10;
        y += x % 10 << 144; x /= 10;
        y += x % 10 << 152; x /= 10;
        y += x % 10 << 160; x /= 10;
        y += x % 10 << 168; x /= 10;
        y += x % 10 << 176; x /= 10;
        y += x % 10 << 184; x /= 10;
        y += x % 10 << 192; x /= 10;
        y += x % 10 << 200; x /= 10;
        y += x % 10 << 208; x /= 10;
        y += x % 10 << 216; x /= 10;
        y += x % 10 << 224; x /= 10;
        y += x % 10 << 232; x /= 10;
        y += x % 10 << 240; x /= 10;
        y += x % 10 << 248;
    }
}

function itoa (uint x) internal pure returns (string memory s) {
    unchecked {
        if (x == 0) return "0";
        else {
            uint c1 = itoa32 (x % 1e32);
            x /= 1e32;
            if (x == 0) s = string (abi.encode (c1));
            else {
                uint c2 = itoa32 (x % 1e32);
                x /= 1e32;
                if (x == 0) {
                    s = string (abi.encode (c2, c1));
                    c1 = c2;
                } else {
                    uint c3 = itoa32 (x);
                    s = string (abi.encode (c3, c2, c1));
                    c1 = c3;
                }
            }
            uint z = 0;
            if (c1 >> 128 == 0x30303030303030303030303030303030) { c1 <<= 128; z += 16; }
            if (c1 >> 192 == 0x3030303030303030) { c1 <<= 64; z += 8; }
            if (c1 >> 224 == 0x30303030) { c1 <<= 32; z += 4; }
            if (c1 >> 240 == 0x3030) { c1 <<= 16; z += 2; }
            if (c1 >> 248 == 0x30) { z += 1; }
            assembly {
                let l := mload (s)
                s := add (s, z)
                mstore (s, sub (l, z))
            }
        }
    }
}





}