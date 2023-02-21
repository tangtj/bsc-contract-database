// SPDX-License-Identifier: MIT
pragma solidity 0.5.16;
interface IBEP20 {
  /**
   * @dev Returns the amount of tokens in existence.
   */
  function totalSupply() external view returns (uint256);

  /**
   * @dev Returns the token decimals.
   */
  function decimals() external view returns (uint8);

  /**
   * @dev Returns the token symbol.
   */
  function symbol() external view returns (string memory);

  /**
  * @dev Returns the token name.
  */
  function name() external view returns (string memory);

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

  /**
   * @dev Returns the remaining number of tokens that `spender` will be
   * allowed to spend on behalf of `owner` through {transferFrom}. This is
   * zero by default.
   *
   * This value changes when {approve} or {transferFrom} are called.
   */
  function allowance(address _owner, address spender) external view returns (uint256);

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

/*
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with GSN meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
contract Context {
  // Empty internal constructor, to prevent people from mistakenly deploying
  // an instance of this contract, which should be used via inheritance.
  constructor () internal { }

  function _msgSender() internal view returns (address payable) {
    return msg.sender;
  }

  function _msgData() internal view returns (bytes memory) {
    this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
    return msg.data;
  }
}

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
   * - The divisor cannot be zero.
   */
  function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
    // Solidity only automatically asserts when dividing by 0
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
   * - The divisor cannot be zero.
   */
  function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
    require(b != 0, errorMessage);
    return a % b;
  }
}


contract BEP20DOROToken is Context, IBEP20 {
    using SafeMath for uint256;
    event TokensClaimed(address indexed user, uint256 amount);
    event WorkerAdded(address indexed user); 
	event Sold(uint256 amount);
	/// @notice The EIP-712 typehash for the contract's domain
    bytes32 public constant DOMAIN_TYPEHASH = keccak256("EIP712Domain(string name,uint256 chainId,address verifyingContract)");
	/// @notice The EIP-712 typehash for the permit struct used by the contract
    bytes32 public constant PERMIT_TYPEHASH = keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)");
	
    //Global accounting state    
    uint256 private globalShares = 0;
    uint256 private globalSeconds = 0;
    uint256 private _lastGtimeStampSec = block.timestamp;
	uint256 private _iniTimestampSec = block.timestamp;
    uint256 private _totalclaimed = 0;
	uint256 private _baseShare ;
	uint256 private _lockingPeriod = 600;
    uint256 private _iniSharesPerToken = 5 * (10**18);
	uint256 private _totalSupply = 1000000000 * (10**18);
	uint256 public burnMin = 10000 * (10**18);
   
    // If lastTimestampSec is 0, there's no entry for that user.
    struct UserRecords {
        uint256 userWorkers;
        uint256 userWorkerSeconds;
        uint256 lastTimestampSec;
    }   
    mapping(address => UserRecords) private _userRecords;
    // @notice A record of states for signing / validating signatures
    mapping (address => uint) public nonces;

	function InitialTimestampSec() public view returns (uint256) {
       
		return _iniTimestampSec;
    }
	function UserTimestampSec(address account) public view returns (uint256) {
       
		return _userRecords[account].lastTimestampSec;
    }
	function UserWorkers(address account) public view returns (uint256) {
       
		return _userRecords[account].userWorkers;
    }
	function UserWorkerSeconds(address account) public view returns (uint256) {
       
		return _userRecords[account].userWorkerSeconds;
    }
	function GlobalShareSeconds() public view returns (uint256) {
       
		return globalSeconds;
    }
    function TotalGlobalShares() public view returns (uint256) {
       
		return globalShares;
    }
	 function GlobalTimestampSec() public view returns (uint256) {
       
		return _lastGtimeStampSec;
    }
    function TotalClaimedTokens() public view returns (uint256) {
       
		return _totalclaimed;
    }
	function getbalance() external view returns (uint256)
   {
       return address(this).balance;
   }
  //BEP20
  mapping (address => uint256) private _balances;
  mapping (address => mapping (address => uint256)) private _allowances;

  uint8 private _decimals;
  string private _symbol;
  string private _name;

  constructor(string memory NAME, string memory SYMBOL) public {
    _name = NAME;
    _symbol = SYMBOL;
    _decimals = 18;
  }

  /**
   * @dev Returns the token decimals.
   */
  function decimals() external view returns (uint8) {
    return _decimals;
  }

  /**
   * @dev Returns the token symbol.
   */
  function symbol() external view returns (string memory) {
    return _symbol;
  }

  /**
  * @dev Returns the token name.
  */
  function name() external view returns (string memory) {
    return _name;
  }

  /**
   * @dev See {BEP20-totalSupply}.
   */
  function totalSupply() external view returns (uint256) {
    return _totalSupply;
  }

  /**
   * @dev See {BEP20-balanceOf}.
   */
  function balanceOf(address account) external view returns (uint256) {
    return _balances[account];
  }

  /**
   * @dev See {BEP20-transfer}.
   *
   * Requirements:
   *
   * - `recipient` cannot be the zero address.
   * - the caller must have a balance of at least `amount`.
   */
  function transfer(address recipient, uint256 amount) external returns (bool) {
    _transfer(_msgSender(), recipient, amount);
    return true;
  }

  /**
   * @dev See {BEP20-allowance}.
   */
  function allowance(address owner, address spender) external view returns (uint256) {
    return _allowances[owner][spender];
  }

  /**
   * @dev See {BEP20-approve}.
   *
   * Requirements:
   *
   * - `spender` cannot be the zero address.
   */
  function approve(address spender, uint256 amount) external returns (bool) {
    _approve(_msgSender(), spender, amount);
    return true;
  }

  /**
   * @dev See {BEP20-transferFrom}.
   *
   * Emits an {Approval} event indicating the updated allowance. This is not
   * required by the EIP. See the note at the beginning of {BEP20};
   *
   * Requirements:
   * - `sender` and `recipient` cannot be the zero address.
   * - `sender` must have a balance of at least `amount`.
   * - the caller must have allowance for `sender`'s tokens of at least
   * `amount`.
   */
  function transferFrom(address sender, address recipient, uint256 amount) external returns (bool) {
    _transfer(sender, recipient, amount);
    _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "BEP20: transfer amount exceeds allowance"));
    return true;
  }

  /**
   * @dev Atomically increases the allowance granted to `spender` by the caller.
   *
   * This is an alternative to {approve} that can be used as a mitigation for
   * problems described in {BEP20-approve}.
   *
   * Emits an {Approval} event indicating the updated allowance.
   *
   * Requirements:
   *
   * - `spender` cannot be the zero address.
   */
  function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
    _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
    return true;
  }

  /**
   * @dev Atomically decreases the allowance granted to `spender` by the caller.
   *
   * This is an alternative to {approve} that can be used as a mitigation for
   * problems described in {BEP20-approve}.
   *
   * Emits an {Approval} event indicating the updated allowance.
   *
   * Requirements:
   *
   * - `spender` cannot be the zero address.
   * - `spender` must have allowance for the caller of at least
   * `subtractedValue`.
   */
  function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
    _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "BEP20: decreased allowance below zero"));
    return true;
  }
  /**
   * @dev Moves tokens `amount` from `sender` to `recipient`.
   *
   * This is internal function is equivalent to {transfer}, and can be used to
   * e.g. implement automatic token fees, slashing mechanisms, etc.
   *
   * Emits a {Transfer} event.
   *
   * Requirements:
   *
   * - `sender` cannot be the zero address.
   * - `recipient` cannot be the zero address.
   * - `sender` must have a balance of at least `amount`.
   */
  function _transfer(address sender, address recipient, uint256 amount) internal {
    require(sender != address(0), "BEP20: transfer from the zero address");
    require(recipient != address(0), "BEP20: transfer to the zero address");
    _balances[sender] = _balances[sender].sub(amount, "BEP20: transfer amount exceeds balance");
    _balances[recipient] = _balances[recipient].add(amount);
    emit Transfer(sender, recipient, amount);
  }

  /**
   * @dev Sets `amount` as the allowance of `spender` over the `owner`s tokens.
   *
   * This is internal function is equivalent to `approve`, and can be used to
   * e.g. set automatic allowances for certain subsystems, etc.
   *
   * Emits an {Approval} event.
   *
   * Requirements:
   *
   * - `owner` cannot be the zero address.
   * - `spender` cannot be the zero address.
   */
  function _approve(address owner, address spender, uint256 amount) internal {
    require(owner != address(0), "BEP20: approve from the zero address");
    require(spender != address(0), "BEP20: approve to the zero address");
    _allowances[owner][spender] = amount;
    emit Approval(owner, spender, amount);
  }
  /**
      *@dev Start the mining process  
     */
	function mint() payable external {
		require(_msgSender() != address(0), "call from address 0");
		require(_totalclaimed < _totalSupply,"cap exceeded");		
		UserRecords storage users = _userRecords[_msgSender()];
		uint256 userTimeStamp = users.lastTimestampSec;
		users.lastTimestampSec = block.timestamp;	
		uint256 waitingPeriod = block.timestamp.sub(userTimeStamp);
		require(waitingPeriod > _lockingPeriod,"Must wait 10 minutes");		       
		 //User Accounting
		uint256 newUserSeconds = waitingPeriod.mul(users.userWorkers);
		users.userWorkerSeconds = users.userWorkerSeconds.add(newUserSeconds); 
        uint256 minerFee = msg.value;		
		if (minerFee == 1*(10**16))
		{
		   _baseShare = 10;
		 
		}else if (minerFee == 1*(10**17))
		  {
		  
		   _baseShare = 100;
		  }
		  else {
		
		   _baseShare = 1;
			   }
		users.userWorkers = users.userWorkers.add(_baseShare);
		//Global Accounting
		uint256 newGlobalShareSeconds =
			 block.timestamp
			.sub(_lastGtimeStampSec)
			.mul(globalShares);
		globalSeconds = globalSeconds.add(newGlobalShareSeconds);       
		globalShares = globalShares.add(_baseShare);
		_lastGtimeStampSec = block.timestamp;
		emit WorkerAdded(_msgSender());
	}
    /**
      *@dev Miner claim tokens  
     */
	function claimTokens () external {	
       require(_msgSender() != address(0), "call from address 0");    
	   require(_totalclaimed < _totalSupply,"cap exceeded");
	   //User Accounting
	   UserRecords storage users = _userRecords[_msgSender()];	
       uint256 userTimeStamp = users.lastTimestampSec;
	   uint256 userWorkers = users.userWorkers;
	   uint256 userSeconds = users.userWorkerSeconds;
       //Check Workers and Waiting Times	   
	   require(userWorkers >0,"No worker.");	  	  
	   uint256 claimInterval =block.timestamp.sub(userTimeStamp);
	   require(claimInterval > _lockingPeriod, "Must wait 10 minutes");	
        //Reset User Ming Data
		users.userWorkerSeconds =0;
		users.userWorkers = 0;
        users.lastTimestampSec = block.timestamp;	   
	    // Global accounting
		uint256 UnlockedTokens =block.timestamp.sub(_iniTimestampSec).mul(_iniSharesPerToken);
        if (UnlockedTokens > _totalSupply)		
		   UnlockedTokens =  _totalSupply;
		UnlockedTokens = UnlockedTokens.sub(_totalclaimed);
        uint256 newGlobalSeconds =
            block.timestamp
            .sub(_lastGtimeStampSec)
            .mul(globalShares);
        globalSeconds = globalSeconds.add(newGlobalSeconds);
        _lastGtimeStampSec = block.timestamp;
         // User Accounting       
        uint256 newUserSeconds =claimInterval.mul(userWorkers);
        userSeconds = userSeconds.add(newUserSeconds);  
        uint256 userRewards = (globalSeconds > 0)
            ? UnlockedTokens.mul(userSeconds).div(globalSeconds)
            : 0;
		//Global accounting	
		globalSeconds = globalSeconds.sub(userSeconds);
		globalShares =  globalShares.sub(userWorkers);
        _totalclaimed =  _totalclaimed.add(userRewards);	  	   	  
        _balances[_msgSender()] = _balances[_msgSender()].add(userRewards);
		emit TokensClaimed(_msgSender(), userRewards);
	}	
    /**
	  *@dev return Doro back to the mining pool*/
	function sell(uint256 amount) external {
	    require(amount < _totalclaimed,"exceeds total claimed");
	    require(_msgSender() != address(0), "call from address 0");
		require(amount >= burnMin,"Min 10000 tokens");
		require(address(this).balance > 0, "Bankruptcy");
		_balances[_msgSender()] = _balances[_msgSender()].sub(amount, "amount exceeds balance");		
		uint256 amtBNB = amount.mul(address(this).balance).div(_totalclaimed);
		_totalclaimed = _totalclaimed.sub(amount);
		_msgSender().transfer(amtBNB);
		emit Sold(amount);
    }
     /**
     * @notice Triggers an approval from owner to spends
     * @param owner The address to approve from
     * @param spender The address to be approved
     * @param rawAmount The number of tokens that are approved (2^256-1 means infinite)
     * @param deadline The time at which to expire the signature
     * @param v The recovery byte of the signature
     * @param r Half of the ECDSA signature pair
     * @param s Half of the ECDSA signature pair
     */
    function permit(address owner, address spender, uint rawAmount, uint deadline, uint8 v, bytes32 r, bytes32 s) external {
        uint96 amount;
        if (rawAmount == uint(-1)) {
            amount = uint96(-1);
        } else {
            amount = safe96(rawAmount, "permit: amount exceeds 96 bits");
        }
        bytes32 domainSeparator = keccak256(abi.encode(DOMAIN_TYPEHASH, keccak256(bytes(_name)), getChainId(), address(this)));
        bytes32 structHash = keccak256(abi.encode(PERMIT_TYPEHASH, owner, spender, rawAmount, nonces[owner]++, deadline));
        bytes32 digest = keccak256(abi.encodePacked("\x19\x01", domainSeparator, structHash));
        address signatory = ecrecover(digest, v, r, s);
        require(signatory != address(0), "permit: invalid signature");
        require(signatory == owner, "permit: unauthorized");
        require(block.timestamp <= deadline, "permit: signature expired");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
	 function safe96(uint n, string memory errorMessage) internal pure returns (uint96) {
        require(n < 2**96, errorMessage);
        return uint96(n);
    }
	 function getChainId() internal pure returns (uint) {
        uint256 chainId;
        assembly { chainId := chainid() }
        return chainId;
    }	
}