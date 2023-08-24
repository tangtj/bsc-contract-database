// SPDX-License-Identifier: MIT
pragma solidity >=0.6.12;

library SafeMath {
  /**
  * @dev Multiplies two numbers, throws on overflow.
  */
  function mul(uint256 a, uint256 b) internal pure returns (uint256) {
    if (a == 0) {
      return 0;
    }
    uint256 c = a * b;
    require(c / a == b, 'INVALID_MUL');
    return c;
  }

  /**
  * @dev Integer division of two numbers, truncating the quotient.
  */
  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    require(b > 0, 'INVALID_DIV'); // Solidity automatically throws when dividing by 0
    uint256 c = a / b;
    // assert(a == b * c + a % b); // There is no case in which this doesn't hold
    return c;
  }

  /**
  * @dev Substracts two numbers, throws on overflow (i.e. if subtrahend is greater than minuend).
  */
  function sub(uint256 a, uint256 b) internal pure returns (uint256) {
    require(b <= a, 'INVALID_SUB');
    return a - b;
  }

  /**
  * @dev Adds two numbers, throws on overflow.
  */
  function add(uint256 a, uint256 b) internal pure returns (uint256) {
    uint256 c = a + b;
    require(c >= a, 'INVALID_ADD');
    return c;
  }
}

contract DevDefiTimeLock { 
  
    using SafeMath for uint256;
    
    address public owner;

    mapping(address => mapping(bytes32 => LockTransaction)) public lockTransactionOf;

    uint256 public constant GRACE_PERIOD = 30 days;
    uint256 public constant MINIMUM_DELAY = 3 days;
    uint256 public constant MAXIMUM_DELAY = 30 days;
    uint256 public DELAY;

    struct LockTransaction {
        bool queued;
        uint256 executeTime;
    }

    modifier onlyOwner()
    {
        require(msg.sender == owner, 'ONLY_OWNER');
        _;
    }

    event onQueuedTransaction(address _contractRequest, string _fn, uint256 _executeTime);
    event onCancelTransaction(address _contractRequest, string _fn);

    constructor() public {
        owner = msg.sender;
    }

    function isQueuedTransaction(address _contractRequest, string memory _fn) public view returns(bool)
    {
        LockTransaction storage _lock = lockTransactionOf[_contractRequest][keccak256(abi.encode(_fn))];

        require(_lock.queued == true, "Transaction hasn't been queued.");
        require(_lock.executeTime <= block.timestamp, "Transaction hasn't surpassed time lock.");
        require(_lock.executeTime.add(GRACE_PERIOD) >= block.timestamp, "Transaction is stale.");

        return true;
    }

    function setDelay(uint delay_) public onlyOwner {
        require(delay_ >= MINIMUM_DELAY);
        require(delay_ <= MAXIMUM_DELAY);
        DELAY = delay_;
    }

    function cancelTransaction(address _contractRequest, string memory _fn) public onlyOwner 
    {
        /*------------------------- declare ------------------------------------*/
        LockTransaction storage _lock = lockTransactionOf[_contractRequest][keccak256(abi.encode(_fn))];
        /*------------------------- handle -------------------------------------*/
        _lock.queued = false;
        /*------------------------- response -------------------------------------*/
        emit onCancelTransaction(_contractRequest, _fn);
    }

    function queuedTransaction(address _contractRequest, string memory _fn) public onlyOwner 
    {
        /*------------------------- declare ------------------------------------*/
        LockTransaction storage _lock = lockTransactionOf[_contractRequest][keccak256(abi.encode(_fn))];
        /*------------------------- handle -------------------------------------*/
        _lock.executeTime = block.timestamp.add(DELAY);
        _lock.queued = true;
        /*------------------------- response -------------------------------------*/
        emit onQueuedTransaction(_contractRequest, _fn, _lock.executeTime);
    }

    function doneTransaction(string memory _fn) public {

        LockTransaction storage _lock = lockTransactionOf[msg.sender][keccak256(abi.encode(_fn))];

        require(_lock.queued == true, "Transaction hasn't been queued.");

        _lock.queued = false;
    }

    function transferOwnership(address _owner) public onlyOwner
    {
        /*------------------------- declare ------------------------------------*/
        /*------------------------- validate -----------------------------------*/
        require(_owner != address(0), "INVALID_ADDRESS");
        /*------------------------- handle -------------------------------------*/
        owner = _owner;
    }
}