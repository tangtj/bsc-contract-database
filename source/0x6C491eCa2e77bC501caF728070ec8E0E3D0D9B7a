// File: @openzeppelin/contracts/security/ReentrancyGuard.sol


// OpenZeppelin Contracts (last updated v4.9.0) (security/ReentrancyGuard.sol)

pragma solidity ^0.8.0;

/**
 * @dev Contract module that helps prevent reentrant calls to a function.
 *
 * Inheriting from `ReentrancyGuard` will make the {nonReentrant} modifier
 * available, which can be applied to functions to make sure there are no nested
 * (reentrant) calls to them.
 *
 * Note that because there is a single `nonReentrant` guard, functions marked as
 * `nonReentrant` may not call one another. This can be worked around by making
 * those functions `private`, and then adding `external` `nonReentrant` entry
 * points to them.
 *
 * TIP: If you would like to learn more about reentrancy and alternative ways
 * to protect against it, check out our blog post
 * https://blog.openzeppelin.com/reentrancy-after-istanbul/[Reentrancy After Istanbul].
 */
abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _status will be _NOT_ENTERED
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Returns true if the reentrancy guard is currently set to "entered", which indicates there is a
     * `nonReentrant` function in the call stack.
     */
    function _reentrancyGuardEntered() internal view returns (bool) {
        return _status == _ENTERED;
    }
}

// File: @openzeppelin/contracts/utils/Address.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/Address.sol)

pragma solidity ^0.8.1;

/**
 * @dev Collection of functions related to the address type
 */
library Address {
    /**
     * @dev Returns true if `account` is a contract.
     *
     * [IMPORTANT]
     * ====
     * It is unsafe to assume that an address for which this function returns
     * false is an externally-owned account (EOA) and not a contract.
     *
     * Among others, `isContract` will return false for the following
     * types of addresses:
     *
     *  - an externally-owned account
     *  - a contract in construction
     *  - an address where a contract will be created
     *  - an address where a contract lived, but was destroyed
     *
     * Furthermore, `isContract` will also return true if the target contract within
     * the same transaction is already scheduled for destruction by `SELFDESTRUCT`,
     * which only has an effect at the end of a transaction.
     * ====
     *
     * [IMPORTANT]
     * ====
     * You shouldn't rely on `isContract` to protect against flash loan attacks!
     *
     * Preventing calls from contracts is highly discouraged. It breaks composability, breaks support for smart wallets
     * like Gnosis Safe, and does not provide security since it can be circumvented by calling from a contract
     * constructor.
     * ====
     */
    function isContract(address account) internal view returns (bool) {
        // This method relies on extcodesize/address.code.length, which returns 0
        // for contracts in construction, since the code is only stored at the end
        // of the constructor execution.

        return account.code.length > 0;
    }

    /**
     * @dev Replacement for Solidity's `transfer`: sends `amount` wei to
     * `recipient`, forwarding all available gas and reverting on errors.
     *
     * https://eips.ethereum.org/EIPS/eip-1884[EIP1884] increases the gas cost
     * of certain opcodes, possibly making contracts go over the 2300 gas limit
     * imposed by `transfer`, making them unable to receive funds via
     * `transfer`. {sendValue} removes this limitation.
     *
     * https://consensys.net/diligence/blog/2019/09/stop-using-soliditys-transfer-now/[Learn more].
     *
     * IMPORTANT: because control is transferred to `recipient`, care must be
     * taken to not create reentrancy vulnerabilities. Consider using
     * {ReentrancyGuard} or the
     * https://solidity.readthedocs.io/en/v0.8.0/security-considerations.html#use-the-checks-effects-interactions-pattern[checks-effects-interactions pattern].
     */
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    /**
     * @dev Performs a Solidity function call using a low level `call`. A
     * plain `call` is an unsafe replacement for a function call: use this
     * function instead.
     *
     * If `target` reverts with a revert reason, it is bubbled up by this
     * function (like regular Solidity function calls).
     *
     * Returns the raw returned data. To convert to the expected return value,
     * use https://solidity.readthedocs.io/en/latest/units-and-global-variables.html?highlight=abi.decode#abi-encoding-and-decoding-functions[`abi.decode`].
     *
     * Requirements:
     *
     * - `target` must be a contract.
     * - calling `target` with `data` must not revert.
     *
     * _Available since v3.1._
     */
    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, "Address: low-level call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`], but with
     * `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but also transferring `value` wei to `target`.
     *
     * Requirements:
     *
     * - the calling contract must have an ETH balance of at least `value`.
     * - the called Solidity function must be `payable`.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    /**
     * @dev Same as {xref-Address-functionCallWithValue-address-bytes-uint256-}[`functionCallWithValue`], but
     * with `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a static call.
     *
     * _Available since v3.3._
     */
    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        return functionStaticCall(target, data, "Address: low-level static call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a static call.
     *
     * _Available since v3.3._
     */
    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a delegate call.
     *
     * _Available since v3.4._
     */
    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionDelegateCall(target, data, "Address: low-level delegate call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a delegate call.
     *
     * _Available since v3.4._
     */
    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }

    /**
     * @dev Tool to verify that a low level call to smart-contract was successful, and revert (either by bubbling
     * the revert reason or using the provided one) in case of unsuccessful call or if target was not a contract.
     *
     * _Available since v4.8._
     */
    function verifyCallResultFromTarget(
        address target,
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        if (success) {
            if (returndata.length == 0) {
                // only check isContract if the call was successful and the return data is empty
                // otherwise we already know that it was a contract
                require(isContract(target), "Address: call to non-contract");
            }
            return returndata;
        } else {
            _revert(returndata, errorMessage);
        }
    }

    /**
     * @dev Tool to verify that a low level call was successful, and revert if it wasn't, either by bubbling the
     * revert reason or using the provided one.
     *
     * _Available since v4.3._
     */
    function verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal pure returns (bytes memory) {
        if (success) {
            return returndata;
        } else {
            _revert(returndata, errorMessage);
        }
    }

    function _revert(bytes memory returndata, string memory errorMessage) private pure {
        // Look for revert reason and bubble it up if present
        if (returndata.length > 0) {
            // The easiest way to bubble the revert reason is using memory via assembly
            /// @solidity memory-safe-assembly
            assembly {
                let returndata_size := mload(returndata)
                revert(add(32, returndata), returndata_size)
            }
        } else {
            revert(errorMessage);
        }
    }
}

// File: @openzeppelin/contracts/utils/Context.sol


// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)

pragma solidity ^0.8.0;

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

// File: @openzeppelin/contracts/security/Pausable.sol


// OpenZeppelin Contracts (last updated v4.7.0) (security/Pausable.sol)

pragma solidity ^0.8.0;


/**
 * @dev Contract module which allows children to implement an emergency stop
 * mechanism that can be triggered by an authorized account.
 *
 * This module is used through inheritance. It will make available the
 * modifiers `whenNotPaused` and `whenPaused`, which can be applied to
 * the functions of your contract. Note that they will not be pausable by
 * simply including this module, only once the modifiers are put in place.
 */
abstract contract Pausable is Context {
    /**
     * @dev Emitted when the pause is triggered by `account`.
     */
    event Paused(address account);

    /**
     * @dev Emitted when the pause is lifted by `account`.
     */
    event Unpaused(address account);

    bool private _paused;

    /**
     * @dev Initializes the contract in unpaused state.
     */
    constructor() {
        _paused = false;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        _requireNotPaused();
        _;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is paused.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    modifier whenPaused() {
        _requirePaused();
        _;
    }

    /**
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Throws if the contract is paused.
     */
    function _requireNotPaused() internal view virtual {
        require(!paused(), "Pausable: paused");
    }

    /**
     * @dev Throws if the contract is not paused.
     */
    function _requirePaused() internal view virtual {
        require(paused(), "Pausable: not paused");
    }

    /**
     * @dev Triggers stopped state.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }

    /**
     * @dev Returns to normal state.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
    }
}

// File: @openzeppelin/contracts/access/Ownable.sol


// OpenZeppelin Contracts (last updated v4.9.0) (access/Ownable.sol)

pragma solidity ^0.8.0;


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
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
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
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
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

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

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
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

// File: contracts/PresaleBSCV1.sol


pragma solidity ^0.8.19;







interface Aggregator {
  function latestRoundData() external view returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound);
}

contract PresaleBSCV1 is Ownable, Pausable, ReentrancyGuard {
  address public paymentWallet;
  uint256 public tokensPresaleSupply;
  uint256 public totalTokensSold;
  address public saleToken;
  uint256 public baseDecimals;
  uint256 public maxTokensToBuy;
  uint256 public minTokensToBuy;
  uint256 public currentRound;
  uint256[][5] internal rounds;
  uint256 public roundTokensTracker;
  uint256 public usdRaised;
  uint256 public claimStart;
  uint256 public currentVestingRound;
  uint256[][3] internal vestingRounds;
  uint256 public vestingRoundTokensTracker;
  bool public whitelistClaimOnly;

  IERC20 public USDTInterface;
  IERC20 public BUSDInterface;
  Aggregator public AggregatorV3BNBInterface;
  Aggregator public AggregatorV3BUSDInterface;
  mapping(address => uint256) public userDeposits;
  mapping(address => bool) public hasClaimed;
  mapping(address => bool) public isBlacklisted;
  mapping(address => bool) public isWhitelisted;

  constructor (
    address _paymentWallet,
    uint256 _baseDecimals,
    uint256 _maxTokensToBuy,
    uint256 _minTokensToBuy,
    address _USDTInterface,
    address _AggregatorV3BNBInterface,
    address _AggregatorV3BUSDInterface,
    address _BUSDInterface,
    uint256[] memory _roundNos,
    uint256[] memory _noOfTokens,
    uint256[] memory _timeStarts
  ) {
    paymentWallet = _paymentWallet;
    baseDecimals = _baseDecimals;
    maxTokensToBuy = _maxTokensToBuy;
    minTokensToBuy = _minTokensToBuy;
    USDTInterface = IERC20(_USDTInterface);
    AggregatorV3BNBInterface = Aggregator(_AggregatorV3BNBInterface);
    AggregatorV3BUSDInterface = Aggregator(_AggregatorV3BUSDInterface);
    BUSDInterface = IERC20(_BUSDInterface);
    for (uint256 i = 0; i < _roundNos.length; i++) {
      vestingRounds[0].push(_roundNos[i]);
      vestingRounds[1].push(_noOfTokens[i]);
      vestingRounds[2].push(_timeStarts[i]);
    }
  }

  event NewRoundCreated(uint256 _roundNo, uint256 _newNoOfTokens, uint256 _newPrice, uint256 _newTimeStart, uint256 _newTimeEnd, uint256 timestamp);
  event RoundUpdated(uint256 _roundNo, uint256 _newNoOfTokens, uint256 _newPrice, uint256 _newTimeStart, uint256 _newTimeEnd, uint256 timestamp);
  event NewRoundStarted(uint256 _prevRoundNo, uint256 _roundNo, uint256 timestamp);
  event TokensBought(address indexed user, uint256 indexed tokensBought, address indexed purchaseToken, uint256 amountPaid, uint256 usdEq, uint256 timestamp);
  event TokensAdded(address indexed token, uint256 noOfTokens, uint256 timestamp);
  event TokensClaimed(address indexed user, uint256 amount, uint256 timestamp);
  event ClaimStartUpdated(uint256 prevValue, uint256 newValue, uint256 timestamp);
  event VestingRoundUpdated(uint256 _roundNo, uint256 _newTimeStart, uint256 timestamp);
  event NewVestingRoundStarted(uint256 _prevRoundNo, uint256 _roundNo, uint256 timestamp);
  event MaxTokensUpdated(uint256 prevValue, uint256 newValue, uint256 timestamp);
  event MinTokensUpdated(uint256 prevValue, uint256 newValue, uint256 timestamp);

  function pause() external onlyOwner {
    _pause();
  }

  function unpause() external onlyOwner {
    _unpause();
  }

  function addNewRound(uint256 _roundNo, uint256 _newNoOfTokens, uint256 _newPrice, uint256 _newTimeStart, uint256 _newTimeEnd) external onlyOwner returns(bool){
    require(_newNoOfTokens > 0,"invalid no of tokens");
    require(_newPrice > 0,"invalid new price");
    require(_newTimeStart > 0,"invalid new start time");
    require(_newTimeEnd > 0,"invalid new end time");
    rounds[0].push(_roundNo);
    rounds[1].push(_newNoOfTokens);
    rounds[2].push(_newPrice);
    rounds[3].push(_newTimeStart);
    rounds[4].push(_newTimeEnd);
    if(_roundNo == currentRound){
      roundTokensTracker = _newNoOfTokens;
    }
    emit NewRoundCreated(_roundNo, _newNoOfTokens, _newPrice, _newTimeStart, _newTimeEnd, block.timestamp);
    return true;
  }

  function changeIndividualRoundData(uint256 _roundNo, uint256 _newNoOfTokens, uint256 _newPrice, uint256 _newTimeStart, uint256 _newTimeEnd) external onlyOwner returns(bool){
    require(_roundNo - 1 <= rounds[0].length, "invalid No of round");
    if(_newNoOfTokens > 0){
      uint256 _prevNoOfTokens = rounds[1][_roundNo - 1];
      rounds[1][_roundNo - 1] = _newNoOfTokens;
      if(_roundNo == currentRound){
        uint256 _diffNoOfTokens = _newNoOfTokens - _prevNoOfTokens;
        uint256 _newRoundTokensTracker = roundTokensTracker + _diffNoOfTokens;
        roundTokensTracker = _newRoundTokensTracker;
      }
    }
    if(_newPrice > 0){
      rounds[2][_roundNo - 1] = _newPrice;
    }
    if(_newTimeStart > 0){
      rounds[3][_roundNo - 1] = _newTimeStart;
    }
    if(_newTimeEnd > 0){
      rounds[4][_roundNo - 1] = _newTimeEnd;
    }
    emit RoundUpdated(_roundNo, _newNoOfTokens, _newPrice, _newTimeStart, _newTimeEnd, block.timestamp);
    return true;
  }

  function changeCurrentRound(uint256 _roundNo) external onlyOwner {
    require(_roundNo > 0, 'Wrong round number');
    uint256 _prevRoundNo = currentRound;
    currentRound = _roundNo;
    roundTokensTracker = rounds[1][_roundNo - 1];
    emit NewRoundStarted(_prevRoundNo, _roundNo, block.timestamp);
  }

  function IndividualRoundData(uint256 _roundNo) public view returns (uint256[] memory) {
    require(_roundNo > 0 && _roundNo <= rounds[0].length, "Invalid round number");
    uint256[] memory result = new uint256[](5);
    for (uint256 i = 0; i < 5; i++) {
      result[i] = rounds[i][_roundNo - 1];
    }
    return result;
  }

  function changeIndividualVestingRoundData(uint256 _roundNo, uint256 _newTimeStart) external onlyOwner returns(bool){
    require(_roundNo - 1 <= vestingRounds[0].length, "invalid No of round");
    if(_newTimeStart > 0){
      vestingRounds[2][_roundNo - 1] = _newTimeStart;
    }
    emit VestingRoundUpdated(_roundNo, _newTimeStart, block.timestamp);
    return true;
  }

  function changeCurrentVestingRound(uint256 _roundNo) external onlyOwner {
    require(_roundNo > 0, 'Wrong round number');
    uint256 _prevRoundNo = currentVestingRound;
    currentVestingRound = _roundNo;
    uint256 _newVestingRoundTokensTracker = vestingRoundTokensTracker + vestingRounds[1][_roundNo - 1];
    vestingRoundTokensTracker = _newVestingRoundTokensTracker;
    emit NewVestingRoundStarted(_prevRoundNo, _roundNo, block.timestamp);
  }

  function IndividualVestingRoundData(uint256 _roundNo) public view returns (uint256[] memory) {
    require(_roundNo > 0 && _roundNo <= vestingRounds[0].length, "Invalid round number");
    uint256[] memory result = new uint256[](3);
    for (uint256 i = 0; i < 3; i++) {
      result[i] = vestingRounds[i][_roundNo - 1];
    }
    return result;
  }

  function changePaymentWallet(address _newPaymentWallet) external onlyOwner {
    require(_newPaymentWallet != address(0), 'address cannot be zero');
    paymentWallet = _newPaymentWallet;
  }

  function changeMaxTokensToBuy(uint256 _maxTokensToBuy) external onlyOwner {
    require(_maxTokensToBuy > 0, 'Zero max tokens to buy value');
    uint256 prevValue = maxTokensToBuy;
    maxTokensToBuy = _maxTokensToBuy;
    emit MaxTokensUpdated(prevValue, _maxTokensToBuy, block.timestamp);
  }

  function changeMinTokensToBuy(uint256 _minTokensToBuy) external onlyOwner {
    require(_minTokensToBuy > 0, 'Zero min tokens to buy value');
    uint256 prevValue = minTokensToBuy;
    minTokensToBuy = _minTokensToBuy;
    emit MinTokensUpdated(prevValue, _minTokensToBuy, block.timestamp);
  }

  function calculatePrice(uint256 _amount) public view returns (uint256) {
    uint256 USDTAmount;
    require(_amount <= maxTokensToBuy, 'Amount exceeds max tokens to buy');
    require(_amount >= minTokensToBuy, 'Amount exceeds min tokens to buy');
    USDTAmount = (_amount * rounds[2][currentRound - 1]) / baseDecimals;
    return USDTAmount;
  }

  modifier checkSaleState(uint256 amount) {
    require(block.timestamp >= rounds[3][currentRound - 1] && block.timestamp <= rounds[4][currentRound - 1], 'Invalid time for buying');
    require(amount > 0, 'Invalid sale amount');
    require(amount >= minTokensToBuy, 'Amount is below the minimum tokens to buy');
    require(amount <= maxTokensToBuy, 'Amount is higher than the maximum tokens to buy');
    _;
  }

  function buyWithBNB(uint256 amount) external payable checkSaleState(amount) whenNotPaused nonReentrant returns (bool) {
    uint256 usdtAmount = calculatePrice(amount);
    uint256 bnbAmount = (usdtAmount * baseDecimals) / getLatestBNBPrice();
    require(msg.value >= bnbAmount, 'Less payment');
    require(amount <= maxTokensToBuy - userDeposits[_msgSender()], string(abi.encodePacked('Total amount per account exceeds the max tokens to buy. Remaining: ', maxTokensToBuy - userDeposits[_msgSender()])));
    require(amount <= roundTokensTracker, string(abi.encodePacked('Exceeds the remaining tokens. Remaining: ', roundTokensTracker)));
    uint256 excess = msg.value - bnbAmount;
    sendValue(payable(paymentWallet), bnbAmount);
    totalTokensSold += amount;
    roundTokensTracker -= amount;
    userDeposits[_msgSender()] += amount;
    usdRaised += usdtAmount;
    if (excess > 0) sendValue(payable(_msgSender()), excess);
    emit TokensBought(_msgSender(), amount, address(0), bnbAmount, usdtAmount, block.timestamp);
    return true;
  }

  function buyWithUSDT(uint256 amount) external checkSaleState(amount) whenNotPaused nonReentrant returns (bool) {
    uint256 usdtAmount = calculatePrice(amount);
    require(amount <= maxTokensToBuy - userDeposits[_msgSender()], string(abi.encodePacked('Total amount per account exceeds the max tokens to buy. Remaining: ', maxTokensToBuy - userDeposits[_msgSender()])));
    require(amount <= roundTokensTracker, string(abi.encodePacked('Exceeds the remaining tokens. Remaining: ', roundTokensTracker)));
    uint256 ourAllowance = USDTInterface.allowance(_msgSender(), address(this));
    require(usdtAmount <= ourAllowance, 'Make sure to add enough allowance');
    require(USDTInterface.balanceOf(_msgSender()) >= usdtAmount, 'Insufficient USDT balance');
    bool success = USDTInterface.transferFrom(_msgSender(), paymentWallet, usdtAmount);
    require(success, 'USDT transfer failed');
    totalTokensSold += amount;
    roundTokensTracker -= amount;
    userDeposits[_msgSender()] += amount;
    usdRaised += usdtAmount;
    emit TokensBought(_msgSender(), amount, address(USDTInterface), usdtAmount, usdtAmount, block.timestamp);
    return true;
  }

  function buyWithBUSD(uint256 amount) external checkSaleState(amount) whenNotPaused nonReentrant returns (bool) {
    uint256 usdtAmount = calculatePrice(amount);
    uint256 busdAmount = (usdtAmount * baseDecimals) / getLatestBUSDPrice();
    require(amount <= maxTokensToBuy - userDeposits[_msgSender()], string(abi.encodePacked('Total amount per account exceeds the max tokens to buy. Remaining: ', maxTokensToBuy - userDeposits[_msgSender()])));
    require(amount <= roundTokensTracker, string(abi.encodePacked('Exceeds the remaining tokens. Remaining: ', roundTokensTracker)));
    uint256 ourAllowance = BUSDInterface.allowance(_msgSender(), address(this));
    require(busdAmount <= ourAllowance, 'Make sure to add enough allowance');
    require(BUSDInterface.balanceOf(_msgSender()) >= busdAmount, 'Insufficient BUSD balance');
    bool success = BUSDInterface.transferFrom(_msgSender(), paymentWallet, busdAmount);
    require(success, 'BUSD transfer failed');
    totalTokensSold += amount;
    roundTokensTracker -= amount;
    userDeposits[_msgSender()] += amount;
    usdRaised += usdtAmount;
    emit TokensBought(_msgSender(), amount, address(BUSDInterface), busdAmount, usdtAmount, block.timestamp);
    return true;
  }

  function bnbBuyHelper(uint256 amount) external view returns (uint256 bnbAmount) {
    uint256 usdAmount = calculatePrice(amount);
    bnbAmount = (usdAmount * baseDecimals) / getLatestBNBPrice();
  }

  function usdtBuyHelper(uint256 amount) external view returns (uint256 usdAmount) {
    usdAmount = calculatePrice(amount);
  }

  function busdBuyHelper(uint256 amount) external view returns (uint256 busdAmount) {
    uint256 usdAmount = calculatePrice(amount);
    busdAmount = (usdAmount * baseDecimals) / getLatestBUSDPrice();
  }

  function sendValue(address payable recipient, uint256 amount) internal {
    require(address(this).balance >= amount, 'Low balance');
    (bool success, ) = recipient.call{value: amount}('');
    require(success, 'BNB Payment failed');
  }

  function getLatestBNBPrice() internal view returns (uint256) {
    (, int256 price, , , ) = AggregatorV3BNBInterface.latestRoundData();
    price = (price * (10 ** 10));
    return uint256(price);
  }

  function getLatestBUSDPrice() internal view returns (uint256) {
    (, int256 price, , , ) = AggregatorV3BUSDInterface.latestRoundData();
    price = (price * (10 ** 10));
    return uint256(price);
  }

  function changeBaseDecimals(uint256 value) external onlyOwner {
    require(value >= 0, 'Base decimals a negative value');
    baseDecimals = value;
  }

  function changeTokensPresaleSupply(uint256 value) external onlyOwner {
    require(value >= 0, 'Tokens presale supply a negative value');
    tokensPresaleSupply = value;
  }

  function changeUSDTInterface(address _USDTInterface) external onlyOwner {
    require(_USDTInterface != address(0), 'cannot be zero');
    USDTInterface = IERC20(_USDTInterface);
  }

  function changeAggregatorV3BNBInterface(address _AggregatorV3BNBInterface) external onlyOwner {
    require(_AggregatorV3BNBInterface != address(0), 'cannot be zero');
    AggregatorV3BNBInterface = Aggregator(_AggregatorV3BNBInterface);
  }

  function changeAggregatorV3BUSDInterface(address _AggregatorV3BUSDInterface) external onlyOwner {
    require(_AggregatorV3BUSDInterface != address(0), 'cannot be zero');
    AggregatorV3BUSDInterface = Aggregator(_AggregatorV3BUSDInterface);
  }

  function startClaim(uint256 _claimStart, uint256 noOfTokens, address _saleToken) external onlyOwner returns (bool) {
    require(_claimStart > rounds[3][currentRound - 1] && _claimStart > block.timestamp, 'Invalid claim start time');
    require(claimStart == 0, 'Claim already set');
    claimStart = _claimStart;
    saleToken = _saleToken;
    bool success = IERC20(_saleToken).transferFrom(_msgSender(), address(this), noOfTokens);
    require(success, 'Token transfer failed');
    emit TokensAdded(saleToken, noOfTokens, block.timestamp);
    return true;
  }

  function changeClaimStart(uint256 _claimStart) external onlyOwner returns (bool) {
    require(claimStart > 0, 'Initial claim data not set');
    require(_claimStart > rounds[4][currentRound - 1], 'Sale in progress');
    require(_claimStart > block.timestamp, 'Claim start in past');
    uint256 prevValue = claimStart;
    claimStart = _claimStart;
    emit ClaimStartUpdated(prevValue, _claimStart, block.timestamp);
    return true;
  }

  function changeSaleToken(address _saleToken) external onlyOwner {
    require(_saleToken != address(0), 'cannot be zero');
    saleToken = _saleToken;
  }

  function claim() external whenNotPaused returns (bool) {
    require(saleToken != address(0), 'Sale token not added');
    require(block.timestamp >= claimStart  && currentVestingRound > 0, 'Claim has not started yet');
    require(!isBlacklisted[_msgSender()], 'This Address is Blacklisted');
    if (whitelistClaimOnly) {
      require(isWhitelisted[_msgSender()], 'User not whitelisted for claim');
    }
    require(!hasClaimed[_msgSender()], 'Already claimed');
    uint256 amount = userDeposits[_msgSender()];
    require(amount > 0 && vestingRoundTokensTracker > 0, 'Nothing to claim');
    if(amount > vestingRoundTokensTracker){
      uint256 _remainingAmount = vestingRoundTokensTracker;
      bool success = IERC20(saleToken).transfer(_msgSender(), _remainingAmount);
      require(success, 'Token transfer failed');
      userDeposits[_msgSender()] -= _remainingAmount;
      vestingRoundTokensTracker -= _remainingAmount;
      emit TokensClaimed(_msgSender(), _remainingAmount, block.timestamp);
    }else{
      bool success = IERC20(saleToken).transfer(_msgSender(), amount);
      require(success, 'Token transfer failed');
      hasClaimed[_msgSender()] = true;
      delete userDeposits[_msgSender()];
      vestingRoundTokensTracker -= amount;
      emit TokensClaimed(_msgSender(), amount, block.timestamp);
    }
    return true;
  }

  function blacklistUsers(address[] calldata _usersToBlacklist) external onlyOwner {
    for (uint256 i = 0; i < _usersToBlacklist.length; i++) {
      isBlacklisted[_usersToBlacklist[i]] = true;
    }
  }

  function removeFromBlacklist(address[] calldata _userToRemoveFromBlacklist) external onlyOwner {
    for (uint256 i = 0; i < _userToRemoveFromBlacklist.length; i++) {
      isBlacklisted[_userToRemoveFromBlacklist[i]] = false;
    }
  }

  function whitelistUsers(address[] calldata _usersToWhitelist) external onlyOwner {
    for (uint256 i = 0; i < _usersToWhitelist.length; i++) {
      isWhitelisted[_usersToWhitelist[i]] = true;
    }
  }

  function removeFromWhitelist(address[] calldata _userToRemoveFromWhitelist) external onlyOwner {
    for (uint256 i = 0; i < _userToRemoveFromWhitelist.length; i++) {
      isWhitelisted[_userToRemoveFromWhitelist[i]] = false;
    }
  }

  function setClaimWhitelistStatus(bool _status) external onlyOwner {
    whitelistClaimOnly = _status;
  }
}