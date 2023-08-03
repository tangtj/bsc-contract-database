// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;
/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount) external returns (bool);



    /**
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

  
}

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

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner account will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
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

library SafeMath {
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
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
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
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    /**
     * @dev Returns the integer division of two unsigned integers. Reverts on
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
        return div(a, b, "SafeMath: division by zero");
    }

    /**
     * @dev Returns the integer division of two unsigned integers. Reverts with custom message on
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
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts when dividing by zero.
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
        return mod(a, b, "SafeMath: modulo by zero");
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts with custom message when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

contract PledgeMining is Ownable{

    using SafeMath for uint256;

    address public lpAddress = address(0x0d25895E3743DaD1f6D4A3a8F0f4070A6De5b34A);
    address public awardTokenAddress = address(0xEa265c66992D359C0f86ba55B271483634134320);

    address[] public pledgeUser; //质押用户列表
    mapping(address => bool) private havePush;

    mapping(address => uint256) private pledgeAmout; //质押数量
    mapping(address => uint256) private awardAmount; //奖励数量
    uint public totalPledgeAmout; //总质押数量 
    uint public dayAwardAmount; //日产数量

    struct Log { 
        uint rTime;
        uint rValue;
    }

    mapping(address => Log[]) public _pledgeLogList; //质押记录
    mapping(address => Log[]) public _releaseLogList; //解押记录
    mapping(address => Log[]) public _rewardsLogList; //奖励记录
    mapping(address => Log[]) public _drawAewardsLogList; //提取记录

    event Pledge(address indexed account, uint indexed amount);
    event Release(address indexed account, uint indexed amount);
    event IssueAward(address indexed account, uint indexed amount);
    event DrawAward(address indexed account, uint indexed amount);

    constructor(){
        dayAwardAmount = 20 * 10**18;
    }

    function setDayAwardAmount(uint amount) external onlyOwner{
        dayAwardAmount = amount;
    }

    function setLpAddress(address lp) external onlyOwner{
        lpAddress = lp;
    }

    function setAwardTokenAddress(address token) external onlyOwner{
        awardTokenAddress = token;
    }

    function pledge(uint amount) external{

        require(0 < amount, 'Amount: must be > 0');

        address sender = _msgSender();

        IERC20(lpAddress).transferFrom(sender, address(this), amount);

        pledgeAmout[sender] = pledgeAmout[sender].add(amount);

        _pledgeLogList[sender].push(Log(block.timestamp, amount));

        totalPledgeAmout = totalPledgeAmout.add(amount);

        addPledgeUser(sender);

        emit Pledge(sender, amount);
    }

    function release(uint amount) external {
        
        require(0 < amount, 'Amount: must be > 0');

        address sender = _msgSender();

        require(pledgeAmout[sender] >= amount, 'Insufficient pledge balance');

        IERC20(lpAddress).transfer(sender, amount);

        pledgeAmout[sender] = pledgeAmout[sender].sub(amount);

        _releaseLogList[sender].push(Log(block.timestamp, amount));

        totalPledgeAmout = totalPledgeAmout.sub(amount);


        emit Release(sender, amount);
    }

    function issueAwards() external onlyOwner {
        if (totalPledgeAmout == 0){
            return;
        }

        uint rate;
        uint amount;
        address user;

        for(uint i=0; i<pledgeUser.length; i++){
            user = pledgeUser[i];
            rate = pledgeAmout[user].mul(1000000).div(totalPledgeAmout);

            if(rate>0){
                amount = dayAwardAmount.mul(rate).div(1000000);
                awardAmount[user] = awardAmount[user].add(amount);
                _rewardsLogList[user].push(Log(block.timestamp, amount));
                emit IssueAward(user, amount);
            }
        }
    }

    function drawAward(uint amount) external {
        
        require(0 < amount, 'Amount: must be > 0');

        address sender = _msgSender();

        require(awardAmount[sender] >= amount, 'Insufficient award balance');

        IERC20(awardTokenAddress).transfer(sender, amount);
 
        awardAmount[sender] = awardAmount[sender].sub(amount);

        _drawAewardsLogList[sender].push(Log(block.timestamp, amount));       

        emit DrawAward(sender, amount);
    }

    function addPledgeUser(address user) internal {

        if(havePush[user]){
            return;
        }

        havePush[user] = true;

        pledgeUser.push(user);
    }

    function getPledgeAmout()  external view returns(uint){

        return pledgeAmout[_msgSender()];

    }

    function getAwardAmount()  external view returns(uint){

        return awardAmount[_msgSender()];

    }

    function getPledgeLogs() external view returns(Log[] memory){

        return _pledgeLogList[_msgSender()];

    }

    function getReleaseLogs() external view returns(Log[] memory){

        return _releaseLogList[_msgSender()];

    }

    function getRewardsLogs() external view returns(Log[] memory){

        return _rewardsLogList[_msgSender()];

    }

    function getDrawAewardsLogs() external view returns(Log[] memory){

        return _drawAewardsLogList[_msgSender()];

    }        
}