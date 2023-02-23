// File: @openzeppelin/contracts/token/ERC20/IERC20.sol

// SPDX-License-Identifier: MIT

pragma solidity ^0.6.0;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount) external returns (bool);
    function mint(address account, uint amount) external;
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
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

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
}

// File: contracts/libraries/ERC20Detailed.sol

pragma solidity 0.6.12;


/**
 * @title ERC20Detailed token
 * @dev The decimals are only for visualization purposes.
 * All the operations are done using the smallest and indivisible token unit,
 * just as on Ethereum all the operations are done in wei.
 */
abstract contract ERC20Detailed is IERC20 {
    string private _name;
    string private _symbol;
    uint8 private _decimals;

    constructor (string memory name, string memory symbol, uint8 decimals) public {
        _name = name;
        _symbol = symbol;
        _decimals = decimals;
    }

    /**
     * @return the name of the token.
     */
    function name() public view returns (string memory) {
        return _name;
    }

    /**
     * @return the symbol of the token.
     */
    function symbol() public view returns (string memory) {
        return _symbol;
    }

    /**
     * @return the number of decimals of the token.
     */
    function decimals() public view returns (uint8) {
        return _decimals;
    }
}

// File: contracts/interfaces/Governance.sol

pragma solidity 0.6.12;

contract Governance {

    address public _governance;

    constructor() public {
        _governance = tx.origin;
    }

    event GovernanceTransferred(address indexed previousOwner, address indexed newOwner);

    modifier onlyGovernance {
        require(msg.sender == _governance, "not governance");
        _;
    }

    function setGovernance(address governance)  public  onlyGovernance
    {
        require(governance != address(0), "new governance the zero address");
        emit GovernanceTransferred(_governance, governance);
        _governance = governance;
    }


}

// File: @openzeppelin/contracts/math/SafeMath.sol

pragma solidity ^0.6.0;

/**
 * @dev Wrappers over Solidity's arithmetic operations with added overflow
 * checks.
 *
 * Arithmetic operations in Solidity wrap on overflow. This can easily result
 * in bugs, because programmers usually assume that an overflow raises an
 * error, which is the standard behavior in high level programming languages.
 * `SafeMath` restores this intuition by reverting the transaction when an
 * operation overflows.
 *
 * Using this library instead of the unchecked operations eliminates an entire
 * class of bugs, so it's recommended to use it always.
 */
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
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

// File: contracts/Nar.sol

pragma solidity 0.6.12;




contract NarToken is Governance,ERC20Detailed{

    using SafeMath for uint256;

    //events
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Mint(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event eventSetRate(uint256 burn_rate, uint256 reward_rate);
    event eventRewardPool(address rewardPool);
    event eventTeamWallet(address teamWallet);
   
    // for minters
    mapping (address => bool) public _minters;
    address [] public _minterAddresses;

    //token base data
    uint256 internal _totalSupply;
    mapping(address => uint256) public _balances;
    mapping (address => mapping (address => uint256)) public _allowances;

    /// Constant token specific fields
    uint256 public  _maxSupply = 0;

    ///
    bool public _openTransfer = false;

    // init limit rate
    uint256 public constant _maxValueRate = 2000; 
    uint256 public constant _minValueRate = 10;  
    uint256 public constant _rateBase = 10000; 

    uint256 public  _burnRate = 250;       
    uint256 public  _rewardRate = 250;   

    uint256 public _totalBurnToken = 0;
    uint256 public _totalRewardToken = 0;

    //todo reward pool!
    address public _rewardPool;
    //todo team wallet!
    address public _teamWallet;

    uint256 public _teamMinted = 0;

    address public _burnPool = 0x7777777777777777777777777777777777777777;

    /**
     * CONSTRUCTOR
     *
     * @dev Initialize the Nar Token
     */

    constructor () public ERC20Detailed("NAR Token", "NAR", 18) {
         _maxSupply = 21000000 * (10**18);
    }
    
    /**
    * @dev Approve the passed address to spend the specified amount of tokens on behalf of msg.sender.
    * @param spender The address which will spend the funds.
    * @param amount The amount of tokens to be spent.
    */
    function approve(address spender, uint256 amount) override external 
    returns (bool) 
    {
        require(msg.sender != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);

        return true;
    }

    /**
    * @dev Function to check the amount of tokens than an owner _allowed to a spender.
    * @param owner address The address which owns the funds.
    * @param spender address The address which will spend the funds.
    * @return A uint256 specifying the amount of tokens still available for the spender.
    */
    function allowance(address owner, address spender) override external view 
    returns (uint256) 
    {
        return _allowances[owner][spender];
    }

    /**
    * @dev Gets the balance of the specified address.
    * @param owner The address to query the the balance of.
    * @return An uint256 representing the amount owned by the passed address.
    */
    function balanceOf(address owner) override external view 
    returns (uint256) 
    {
        return _balances[owner];
    }

    /**
    * @dev return the token total supply
    */
    function totalSupply() override external view 
    returns (uint256) 
    {
        return _totalSupply;
    }

     /**
    * @dev transfer token for a specified address
    * @param to The address to transfer to.
    * @param value The amount to be transferred.
    */
    function transfer(address to, uint256 value) override external 
    returns (bool)  
    {
        return _transfer(msg.sender,to,value);
    }

    /**
    * @dev Transfer tokens from one address to another
    * @param from address The address which you want to send tokens from
    * @param to address The address which you want to transfer to
    * @param value uint256 the amount of tokens to be transferred
    */
    function transferFrom(address from, address to, uint256 value) override external 
    returns (bool) 
    {
        uint256 allow = _allowances[from][msg.sender];
        _allowances[from][msg.sender] = allow.sub(value);
        
        return _transfer(from,to,value);
    }

      /**
    * @dev for mint function
    */
    function mint(address account, uint256 amount)  override external 
    {
        require(account != address(0), "ERC20: mint to the zero address");
        require(_minters[msg.sender] || _teamWallet == msg.sender , "!minter");

        uint256 curMintSupply = _totalSupply.add(_totalBurnToken);
        uint256 newMintSupply = curMintSupply.add(amount);
        require( newMintSupply <= _maxSupply,"supply is max!");

        if(_teamWallet == msg.sender){
             require(_teamMinted.add(amount) <= curMintSupply.div(100) ,"more than 1 %");
             _teamMinted = _teamMinted.add(amount);
        }

        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
      
        emit Mint(address(0), account, amount);
        emit Transfer(address(0), account, amount);
    }

    /**
    * @dev Transfer tokens with fee
    * @param from address The address which you want to send tokens from
    * @param to address The address which you want to transfer to
    * @param value uint256s the amount of tokens to be transferred
    */
    function _transfer(address from, address to, uint256 value) internal 
    returns (bool) 
    {
        require(_openTransfer || from == _governance, "transfer closed");

        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        uint256 sendAmount = value;
        uint256 burnFee = (value.mul(_burnRate)).div(_rateBase);
        if (burnFee > 0) {
            //to burn
            _balances[_burnPool] = _balances[_burnPool].add(burnFee);
            _totalSupply = _totalSupply.sub(burnFee);
            sendAmount = sendAmount.sub(burnFee);

            _totalBurnToken = _totalBurnToken.add(burnFee);

            emit Transfer(from, _burnPool, burnFee);
        }

        uint256 rewardFee = (value.mul(_rewardRate)).div(_rateBase);
        if (rewardFee > 0) {
           //to reward
            _balances[_rewardPool] = _balances[_rewardPool].add(rewardFee);
            sendAmount = sendAmount.sub(rewardFee);

            _totalRewardToken = _totalRewardToken.add(rewardFee);

            emit Transfer(from, _rewardPool, rewardFee);
        }

        _balances[from] = _balances[from].sub(value);
        _balances[to] = _balances[to].add(sendAmount);

        emit Transfer(from, to, sendAmount);

        return true;
    }

    /**
    * @dev set the token transfer switch
    */
    function enableOpenTransfer() public onlyGovernance  
    {
        _openTransfer = true;
    }

    /**
    * @dev for get numbers of minter
    */
    function minterAddressesLength() external view returns (uint256) {
        return _minterAddresses.length;
    }

    function addMinter(address _minter) public onlyGovernance 
    {
        _minters[_minter] = true;
        _minterAddresses.push(_minter);
        
    }
    
    function removeMinter(address _minter) public onlyGovernance 
    {
        _minters[_minter] = false;
        for (uint i=0; i < _minterAddresses.length ;i++){
            if(_minterAddresses[i] == _minter){
                _minterAddresses[i] = _minterAddresses[_minterAddresses.length -1];
                _minterAddresses.pop();
                break;
            }
        }
    }

    receive () external payable {
      revert();
    }

    /**
    * @dev for govern value
    */
    function setRate(uint256 burn_rate, uint256 reward_rate) public 
        onlyGovernance 
    {
        require(_maxValueRate >= burn_rate && burn_rate >= _minValueRate,"invalid burn rate");
        require(_maxValueRate >= reward_rate && reward_rate >= _minValueRate,"invalid reward rate");

        _burnRate = burn_rate;
        _rewardRate = reward_rate;

        emit eventSetRate(burn_rate, reward_rate);
    }

    /**
    * @dev for set teamWallet
    */

    function setTeamWallet(address teamWallet) public 
        onlyGovernance 
    {
        require(teamWallet != address(0x0));

        _teamWallet = teamWallet;

        emit eventTeamWallet(_teamWallet);
    }

    /**
    * @dev for set reward
    */
    
    function setRewardPool(address rewardPool) public 
        onlyGovernance 
    {
        require(rewardPool != address(0x0));

        _rewardPool = rewardPool;

        emit eventRewardPool(_rewardPool);
    }
}