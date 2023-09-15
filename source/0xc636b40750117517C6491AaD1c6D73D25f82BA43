// SPDX-License-Identifier: MIT


pragma solidity ^0.8.0;

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

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
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
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
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
abstract contract ReentrancyGuard {
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;
    uint256 private _status;

    constructor() { _status = _NOT_ENTERED; }

    modifier nonReentrant() {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        _status = _ENTERED;
        _;
		_status = _NOT_ENTERED;
    }
}

library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        uint256 c = a + b;
        if (c < a) return (false, 0);
        return (true, c);
    }

    /**
     * @dev Returns the substraction of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        if (b > a) return (false, 0);
        return (true, a - b);
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) return (true, 0);
        uint256 c = a * b;
        if (c / a != b) return (false, 0);
        return (true, c);
    }

    /**
     * @dev Returns the division of two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        if (b == 0) return (false, 0);
        return (true, a / b);
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        if (b == 0) return (false, 0);
        return (true, a % b);
    }

    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     *
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        return a - b;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     *
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) return 0;
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        return a / b;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: modulo by zero");
        return a % b;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {trySub}.
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        return a - b;
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting with custom message on
     * division by zero. The result is rounded towards zero.
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {tryDiv}.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        return a / b;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting with custom message when dividing by zero.
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {tryMod}.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        return a % b;
    }
}


contract BNBWizardFarming is Ownable, ReentrancyGuard{
    using SafeMath for uint256;
 
    uint256 public devFee = 200;
    uint256 public ref = 900;
    uint256 public period = 1 days;
    uint256 public capitalPeriod = 60 * 60 * 24 * 30;
    uint256 public apr = 200;
    uint256 public _usersBonus = 2000;
    uint256 public percentRate = 10000;

    address private devWallet;
  
    uint256 public _currentDepositID = 0;   
    
    uint256 public Investors = 0;
    uint256 public totalReward = 0;
    uint256 public totalInvested = 0;

    uint256 public count;
    mapping(address => bool) private hasReceivedBonus; // Track whether each user has received the bonus


    mapping(address => uint256) public _bonus;
   
    struct DepositInfos{
        address investor;
        uint256 depositAmount;
        uint256 depositAt;
        uint256 claimedAmount;
        bool state;
    }

    struct InvestorInfos{
        address investor;
        address referrer;
        uint256 totalLocked;
        uint256 startTime;
        uint256 lastCalculationDate;
        uint256 claimableAmount;
        uint256 claimedAmount;
        uint256 referAmount;
    }

    event Deposit(
        uint256 id,
        address investor
    );

   
    mapping(uint256 => DepositInfos) public depositState;
    mapping(address => uint256[]) public ownedDeposits;
    mapping(address => InvestorInfos) public investors;



    constructor(address _feesWallet) {
        require(_feesWallet!=address(0),"Please provide a valid dev wallet address");
        devWallet = _feesWallet;
        count = 0;
    }
  
    function setWallet(address _feesWallet) public onlyOwner {
        require(_feesWallet!=address(0),"Please provide a valid address");
        devWallet = _feesWallet;
    }

    function _getNextDepositID() private view returns (uint256) {
        return _currentDepositID + 1;
    }

    function _incrementDepositID() private {
        _currentDepositID++;
    }

    function deposit(address _referrer) external payable nonReentrant{
        require(msg.value >= 10000000000000000, "Too low amount to deposit");

        uint256 _newAmount = msg.value;

        if(_referrer == msg.sender){
            _referrer = address(0);
        }
  
        uint256 _id = _getNextDepositID();
        _incrementDepositID();

        uint256 depositFee = (_newAmount * devFee).div(percentRate);
      
        payable(devWallet).transfer(depositFee);
            
        // Check if the deposit meets the condition
        if (_newAmount >= 50000000000000000) { // more than 0.05 BNB
            if (count < _usersBonus && !hasReceivedBonus[msg.sender]) {
                hasReceivedBonus[msg.sender] = true; // Mark the bonus as received for the user

                // Calculate 10% of the original sent value
                uint256 tenPercent = (_newAmount * 10) / 100;

                // Calculate the new value by adding 10% to the original value
                _newAmount = _newAmount + tenPercent;

                count++;
            }
        }

        uint256 _depositAmount = _newAmount - depositFee;

        depositState[_id].investor = msg.sender;
        depositState[_id].depositAmount = _depositAmount;
        depositState[_id].depositAt = block.timestamp;
        depositState[_id].state = true;

        if(investors[msg.sender].investor == address(0)){
            Investors = Investors.add(1);
            investors[msg.sender].investor = msg.sender;
            investors[msg.sender].startTime = block.timestamp;
            investors[msg.sender].lastCalculationDate = block.timestamp;
        }

        if(address(0) != _referrer && investors[msg.sender].referrer == address(0)) {
            investors[msg.sender].referrer = _referrer;
            
        }

        if(investors[msg.sender].referrer != address(0)){
            uint256 referrerAmount = (_newAmount * ref).div(percentRate);
            
            investors[investors[msg.sender].referrer].referAmount = investors[investors[msg.sender].referrer].referAmount.add(referrerAmount);
            payable(investors[msg.sender].referrer).transfer(referrerAmount);

        }
       

        uint256 lastRoiTime = block.timestamp - investors[msg.sender].lastCalculationDate;
        uint256 allClaimableAmount = (lastRoiTime *
            investors[msg.sender].totalLocked *
            apr).div(percentRate * period);

        investors[msg.sender].claimableAmount = investors[msg.sender].claimableAmount.add(allClaimableAmount);
        investors[msg.sender].totalLocked = investors[msg.sender].totalLocked.add(_depositAmount);
        investors[msg.sender].lastCalculationDate = block.timestamp;

        totalInvested = totalInvested.add(_newAmount);

        ownedDeposits[msg.sender].push(_id);
        emit Deposit(_id, msg.sender);
        
       
    }

    function stopAirdropRewards() public onlyOwner{
        // transfer not claimed airdrop to owner
        payable(_msgSender()).transfer(_bonus[msg.sender]);
	
    }

    function setAirdropRewards(address _addr, uint256 amount) public onlyOwner{
        _bonus[_addr] += amount;
    }

    function revokeAirdropRewards(address _addr, uint256 amount) public onlyOwner{
        _bonus[_addr] -= amount;
    }

    function moreUsers(uint256 _users) public onlyOwner{
        _usersBonus = _users;
    }

    function vApr(uint256 _newapr) public onlyOwner{
        apr = _newapr;
    }



    function claimRewards() public nonReentrant {
        require(ownedDeposits[msg.sender].length > 0, "you can deposit once at least");
        
        uint256 lastRoiTime = block.timestamp - investors[msg.sender].lastCalculationDate;
        uint256 allClaimableAmount = (lastRoiTime *
            investors[msg.sender].totalLocked *
            apr).div(percentRate * period);
         investors[msg.sender].claimableAmount = investors[msg.sender].claimableAmount.add(allClaimableAmount);

        uint256 amountToSend = investors[msg.sender].claimableAmount;
        
        if(getBalance()<amountToSend){
            amountToSend = getBalance();
        }
        
        investors[msg.sender].claimableAmount = investors[msg.sender].claimableAmount.sub(amountToSend);
        investors[msg.sender].claimedAmount = investors[msg.sender].claimedAmount.add(amountToSend);
        investors[msg.sender].lastCalculationDate = block.timestamp;
   
        payable(msg.sender).transfer(amountToSend);
        totalReward = totalReward.add(amountToSend);
    }
    
    
    function withdraw(uint256 id) public nonReentrant {
        require(
            depositState[id].investor == msg.sender,
            "only investor of this id can claim reward"
        );
        require(
            block.timestamp - depositState[id].depositAt > capitalPeriod,
            "withdraw lock time is not finished yet"
        );
        require(depositState[id].state, "you already withdrawed capital");
        
        uint256 claimableReward = getAllClaimableReward(msg.sender);

        require(
            depositState[id].depositAmount + claimableReward <= getBalance(),
            "no enough token in pool"
        );

       
        investors[msg.sender].claimableAmount = 0;
        investors[msg.sender].claimedAmount = investors[msg.sender].claimedAmount.add(claimableReward);
        investors[msg.sender].lastCalculationDate = block.timestamp;
        investors[msg.sender].totalLocked = investors[msg.sender].totalLocked.sub(depositState[id].depositAmount);

        uint256 amountToSend = depositState[id].depositAmount + claimableReward;

        payable(msg.sender).transfer(amountToSend);
        totalReward = totalReward.add(claimableReward);

        depositState[id].state = false;
    }

    function getOwnedDeposits(address investor) public view returns (uint256[] memory) {
        return ownedDeposits[investor];
    }

    function getAllClaimableReward(address _investor) public view returns (uint256) {
         uint256 lastRoiTime = block.timestamp - investors[_investor].lastCalculationDate;
         uint256 _apr = getApr();
          uint256 allClaimableAmount = (lastRoiTime *
            investors[_investor].totalLocked *
            _apr).div(percentRate * period);

         return investors[_investor].claimableAmount.add(allClaimableAmount);
    }

    function getApr() public view returns (uint256) {
        return apr;
    }

    function getBalance() public view returns(uint256) {
        return address(this).balance;
    }

    function getTotalRewards() public view returns (uint256) {
        return totalReward;
    }

    function getTotalInvests() public view returns (uint256) {
        return totalInvested;
    }

}