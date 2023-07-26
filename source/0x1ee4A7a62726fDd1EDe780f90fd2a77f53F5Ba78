// SPDX-License-Identifier: MIT
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
     * ====
     */
    function isContract(address account) internal view returns (bool) {
        // This method relies in extcodesize, which returns 0 for contracts in
        // construction, since the code is only stored at the end of the
        // constructor execution.

        uint256 size;
        // solhint-disable-next-line no-inline-assembly
        assembly { size := extcodesize(account) }
        return size > 0;
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
     * https://diligence.consensys.net/posts/2019/09/stop-using-soliditys-transfer-now/[Learn more].
     *
     * IMPORTANT: because control is transferred to `recipient`, care must be
     * taken to not create reentrancy vulnerabilities. Consider using
     * {ReentrancyGuard} or the
     * https://solidity.readthedocs.io/en/v0.5.11/security-considerations.html#use-the-checks-effects-interactions-pattern[checks-effects-interactions pattern].
     */
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{ value: amount }("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    /**
     * @dev Performs a Solidity function call using a low level `call`. A
     * plain`call` is an unsafe replacement for a function call: use this
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
      return functionCall(target, data, "Address: low-level call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`], but with
     * `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCall(address target, bytes memory data, string memory errorMessage) internal returns (bytes memory) {
        return _functionCallWithValue(target, data, 0, errorMessage);
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
    function functionCallWithValue(address target, bytes memory data, uint256 value, string memory errorMessage) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        return _functionCallWithValue(target, data, value, errorMessage);
    }

    function _functionCallWithValue(address target, bytes memory data, uint256 weiValue, string memory errorMessage) private returns (bytes memory) {
        require(isContract(target), "Address: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{ value: weiValue }(data);
        if (success) {
            return returndata;
        } else {
            // Look for revert reason and bubble it up if present
            if (returndata.length > 0) {
                // The easiest way to bubble the revert reason is using memory via assembly

                // solhint-disable-next-line no-inline-assembly
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

/**
 * @title SafeERC20
 * @dev Wrappers around ERC20 operations that throw on failure (when the token
 * contract returns false). Tokens that return no value (and instead revert or
 * throw on failure) are also supported, non-reverting calls are assumed to be
 * successful.
 * To use this library you can add a `using SafeERC20 for IERC20;` statement to your contract,
 * which allows you to call the safe operations as `token.safeTransfer(...)`, etc.
 */
library SafeERC20 {
    using SafeMath for uint256;
    using Address for address;

    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    /**
     * @dev Deprecated. This function has issues similar to the ones found in
     * {IERC20-approve}, and its usage is discouraged.
     *
     * Whenever possible, use {safeIncreaseAllowance} and
     * {safeDecreaseAllowance} instead.
     */
    function safeApprove(IERC20 token, address spender, uint256 value) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        // solhint-disable-next-line max-line-length
        require((value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).add(value);
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).sub(value, "SafeERC20: decreased allowance below zero");
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     */
    function _callOptionalReturn(IERC20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We use {Address.functionCall} to perform this call, which verifies that
        // the target address contains contract code and also asserts for success in the low-level call.

        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        if (returndata.length > 0) { // Return data is optional
            // solhint-disable-next-line max-line-length
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}

contract Forsage {

    struct User {
        uint id;
        address referrer;
        uint partnersCount;
    }

    mapping(address => User) public users;

    address public id1;
}

contract XQoreStorage {
    
    using SafeERC20 for IERC20;

    address payable public xQore;
    address public owner;
    

    event CannotSendETH(address indexed to, uint value);

    constructor(address ownerAddress) public {
        xQore = payable(msg.sender);
        owner = ownerAddress;
    }

    receive() external payable {
        require(msg.sender == xQore, "XQoreStorage: receive(). XQore only");
    }

    function transferTo(address to, uint value) public {
        require(msg.sender == xQore);
        (bool res , ) = to.call{value: value}("");
        if(!res) {
            emit CannotSendETH(to, value);
        }
    }

    function withdrawLostTokens(address tokenAddress) public {
        require(msg.sender == owner);        
        IERC20(tokenAddress).transfer(owner, IERC20(tokenAddress).balanceOf(address(this)));
    }
}

contract XQoreBasic {
    
    // -----DON'T REMOVE-----------
    address public impl;
    address public contractOwner;
    // -----------------------------

    uint8 public MAX_LEVEL;

    struct User {
        uint id;
        address referrer;
        bool exists;
        
        mapping(uint8 => bool) activeX6Levels;        
        mapping(uint8 => X6) x6Matrix;
    }
    
    struct X6 {
        address currentReferrerAddress;

        address[3] firstLevelReferrals;
        address[9] secondLevelReferrals;

        uint128 reinvestCount;
        bool blocked;
    }

    Forsage public forsage;
    XQoreStorage public xQoreStorage;
    
    mapping(address => User) public users;
    mapping(uint => address) public idToAddress;
    mapping(uint8 => uint) public levelPrice;

    address public owner;
    address public multisig;

    bool public locked;
    
    event Registration(address indexed user, address indexed referrer, uint indexed userId, uint referrerId);
    event Reinvest(address indexed user, address indexed currentReferrer, address indexed caller, uint8 level);
    event Upgrade(address indexed user, address indexed referrer, uint8 level);
    event NewUserPlace(address indexed user, address indexed referrer, uint8 level, uint8 place);
    event MissedEthReceive(address indexed receiver, address indexed from, uint8 level);
    event SentExtraEthDividends(address indexed _from, address indexed receiver, uint8 _level, uint _value);
    event StoredETH(address indexed from, address indexed to, uint8 level, uint value);
    event ReleasedETH(address indexed from, address indexed to, uint8 level, uint value);
    event CannotSendETH(address indexed to, uint value);
    event FeeSent(address indexed from, uint8 level, uint value);

    function receiveEth() external payable {}
}

abstract contract Proxy {
    /**
     * @dev Delegates the current call to `implementation`.
     * 
     * This function does not return to its internall call site, it will return directly to the external caller.
     */
    function _delegate(address implementation) internal {
        // solhint-disable-next-line no-inline-assembly
        assembly {
            // Copy msg.data. We take full control of memory in this inline assembly
            // block because it will not return to Solidity code. We overwrite the
            // Solidity scratch pad at memory position 0.
            calldatacopy(0, 0, calldatasize())

            // Call the implementation.
            // out and outsize are 0 because we don't know the size yet.
            let result := delegatecall(gas(), implementation, 0, calldatasize(), 0, 0)

            // Copy the returned data.
            returndatacopy(0, 0, returndatasize())

            switch result
            // delegatecall returns 0 on error.
            case 0 { revert(0, returndatasize()) }
            default { return(0, returndatasize()) }
        }
    }

    /**
     * @dev This is a virtual function that should be overriden so it returns the address to which the fallback function
     * and {_fallback} should delegate.
     */
    function _implementation() internal virtual view returns (address);

    /**
     * @dev Delegates the current call to the address returned by `_implementation()`.
     * 
     * This function does not return to its internall call site, it will return directly to the external caller.
     */
    function _fallback() internal {
        _beforeFallback();
        _delegate(_implementation());
    }

    /**
     * @dev Fallback function that delegates calls to the address returned by `_implementation()`. Will run if no other
     * function in the contract matches the call data.
     */
    fallback () payable external {
        _fallback();
    }

    /**
     * @dev Fallback function that delegates calls to the address returned by `_implementation()`. Will run if call data
     * is empty.
     */
    receive () payable external {
        _fallback();
    }

    /**
     * @dev Hook that is called before falling back to the implementation. Can happen as part of a manual `_fallback`
     * call, or as part of the Solidity `fallback` or `receive` functions.
     * 
     * If overriden should call `super._beforeFallback()`.
     */
    function _beforeFallback() internal virtual {
    }
}


contract XQoreProxy is Proxy {
    
    address public impl;
    address public contractOwner;

    modifier onlyContractOwner() { 
        require(msg.sender == contractOwner); 
        _; 
    }

    constructor(address _impl) public {
        impl = _impl;
        contractOwner = msg.sender;
    }
    
    function update(address newImpl) public onlyContractOwner {
        impl = newImpl;
    }

    function removeOwnership() public onlyContractOwner {
        contractOwner = address(0);
    }
    
    function _implementation() internal override view returns (address) {
        return impl;
    }
}

contract XQore is XQoreBasic {
    
    using SafeERC20 for IERC20;
    
    modifier onlyContractOwner() { 
        require(msg.sender == contractOwner, "onlyOwner"); 
        _; 
    }
    
    modifier onlyUnlocked() { 
        require(block.timestamp >= 1682607600, "waiting for start time");
        _; 
    }
 
    // FORSAGE - 0x5acc84a3e955Bdd76467d3348077d003f00fFB97
    function init(address _forsage, address _multisig) public onlyContractOwner() {
        require(forsage == Forsage(address(0)), "already inited");

        multisig = _multisig;

        MAX_LEVEL = 12;
        
        forsage = Forsage(_forsage);
        
        contractOwner = msg.sender;
        
        levelPrice[1] = 0.018e18;
        levelPrice[2] = 0.024e18;
        levelPrice[3] = 0.033e18;
        levelPrice[4] = 0.046e18;
        levelPrice[5] = 0.062e18;
        levelPrice[6] = 0.088e18;
        levelPrice[7] = 0.125e18;
        levelPrice[8] = 0.175e18;
        levelPrice[9] = 0.245e18;
        levelPrice[10] = 0.345e18;
        levelPrice[11] = 0.455e18;
        levelPrice[MAX_LEVEL] = 0.644e18;
        
        owner = forsage.id1();
        
        User memory user = User({
            id: 1,
            referrer: address(0),
            exists: true
        });
        
        users[owner] = user;
        idToAddress[1] = owner;
        
        for(uint8 i = 1; i <= MAX_LEVEL; i++) {
            users[owner].activeX6Levels[i] = true;
        }
    
        xQoreStorage = new XQoreStorage(contractOwner);
    }
    
    function changeLock() external onlyContractOwner() {
        locked = !locked;
    }

    receive() external payable onlyUnlocked {
        //avoid payment loop
        if(msg.sender == address(xQoreStorage)) {
            return;
        }

        require(gasleft() > uint(400000), "too low gas limit");
        uint8 _level = findRequestedLevel(msg.value);
        require(_level > 0, "invalid value");

        _buyNewLevel(msg.sender, _level);
    }

    function findRequestedLevel(uint value) public view returns(uint8 level) {
        for (uint8 i = 1; i <= MAX_LEVEL; i++) {
            if(levelPrice[i] == value) {
                return i;
            }
        }
    }

    function registrationExt() external payable onlyUnlocked() {
        require(gasleft() > uint(400000), "too low gas limit");
        _registration(msg.sender);
    }

    function registrationFor(address userAddress) external payable onlyUnlocked() {
        require(gasleft() > uint(400000), "too low gas limit");
        _registration(userAddress);
    }

    function buyNewLevel(uint8 level) external payable onlyUnlocked() {
        require(gasleft() > uint(400000), "too low gas limit");
        _buyNewLevel(msg.sender, level);
    }

    function buyNewLevelFor(address userAddress, uint8 level) external payable onlyUnlocked() {
        require(gasleft() > uint(400000), "too low gas limit");
        _buyNewLevel(userAddress, level);
    }

    function _buyNewLevel(address userAddress, uint8 level) internal {
        // require(msg.sender == tx.origin, "cannot be a contract");

        if (!isUserExists(userAddress)) {
            return _registration(userAddress);
        }

        require(level > 1 && level <= MAX_LEVEL, "invalid level");
        require(!users[userAddress].activeX6Levels[level], "level already activated");
        require(users[userAddress].activeX6Levels[level-1], "buy previous level first");

        require(msg.value == levelPrice[level], "invalid price"); // fee included;

        if (users[userAddress].x6Matrix[level-1].blocked) {
            users[userAddress].x6Matrix[level-1].blocked = false;
        }

        address freeX6Referrer = findFreeX6Referrer(userAddress, level);
        
        users[userAddress].activeX6Levels[level] = true;
        updateX6Referrer(userAddress, freeX6Referrer, level, levelPrice[level]);
        // controller.update(msg.sender, level, msg.value, 0);
        
        emit Upgrade(userAddress, freeX6Referrer, level);
    }
    
    function _registration(address userAddress) private {
    
        (uint _id ,address referrerAddress, ) = forsage.users(userAddress);

        require(referrerAddress != address(0), "register in Forsage first");
        require(msg.value == levelPrice[1], "registration cost 0.018 BNB");
        // depositToken.safeTransferFrom(msg.sender, address(this), levelPrice[1]);
        require(!isUserExists(userAddress), "user exists");
        // require(msg.sender == tx.origin, "cannot be a contract");
        
        User memory user = User({
            id: _id,
            referrer: referrerAddress,
            exists: true
        });

        users[userAddress] = user;

        idToAddress[_id] = userAddress;
        users[userAddress].activeX6Levels[1] = true;

        address currentReferrer = findFreeX6Referrer(userAddress, 1);

        updateX6Referrer(userAddress, currentReferrer, 1, levelPrice[1]);
        // controller.update(userAddress, 1, msg.value, 0);
        
        emit Registration(userAddress, referrerAddress, users[userAddress].id, users[referrerAddress].id);
    }

    function updateX6Referrer(address userAddress, address referrerAddress, uint8 level, uint ethValue) internal {
        uint8 n = 0;
        uint total2LevelUsers;

        while(n < 3) {
            if(users[referrerAddress].x6Matrix[level].firstLevelReferrals[n] == address(0)) {

                users[referrerAddress].x6Matrix[level].firstLevelReferrals[n] = userAddress;
                emit NewUserPlace(userAddress, referrerAddress, level, n);
                
                //set current level
                users[userAddress].x6Matrix[level].currentReferrerAddress = referrerAddress;
                // users[userAddress].x6Matrix[level].currentReferrerIndex = n;

                if (referrerAddress == owner) {
                    sendDividends(userAddress, referrerAddress, level, ethValue/2);
                    return sendDividends(userAddress, referrerAddress, level, ethValue/2);
                } else {
                    sendDividends(userAddress, referrerAddress, level, ethValue/2);
                }
                
                address ref = users[referrerAddress].x6Matrix[level].currentReferrerAddress;

                uint currentReferrerIndex;
                for(uint8 i = 0; i < 3; i++) {
                    if(users[ref].x6Matrix[level].firstLevelReferrals[i] == referrerAddress) {
                        currentReferrerIndex = i;
                        break;
                    }
                }

                if (currentReferrerIndex == 0) {
                    users[ref].x6Matrix[level].secondLevelReferrals[n] = userAddress;
                    emit NewUserPlace(userAddress, ref, level, n+3);
                } else if (currentReferrerIndex == 1) {
                    users[ref].x6Matrix[level].secondLevelReferrals[n+3] = userAddress;
                    emit NewUserPlace(userAddress, ref, level, n+6);
                } else if (currentReferrerIndex == 2) {
                    users[ref].x6Matrix[level].secondLevelReferrals[n+6] = userAddress;
                    emit NewUserPlace(userAddress, ref, level, n+9);
                }

                total2LevelUsers = 0;

                for(uint i = 0; i < 9; i++) {
                    if (users[ref].x6Matrix[level].secondLevelReferrals[i] != address(0)) {
                        total2LevelUsers++;
                    }
                }

                if(total2LevelUsers >= 8) {
                    storeTokens(userAddress, ref, level, ethValue/2);
                } else {
                    // if(ref == owner) {
                    //     sendOwnerETHDividends(userAddress, level);
                    // }

                    sendDividends(userAddress, ref, level,  ethValue/2);
                }

                if(total2LevelUsers == 9) {
                    return reinvest(msg.sender, ref, level);
                }

                return;
            }
    
            n++;
        }

        //---------------------------- SECOND LEVEL -------------------------------------

        n = 0;
        while(true) {
            if (users[referrerAddress].x6Matrix[level].secondLevelReferrals[n] == address(0)) {
                users[referrerAddress].x6Matrix[level].secondLevelReferrals[n] = userAddress;
                emit NewUserPlace(userAddress, referrerAddress, level, n+3);
                
                //move to bottom
                uint a;
                if(n < 3) {
                    a = 0;
                } else if (n < 6) {
                    a = 1;
                } else {
                    a = 2;
                }

                address currentReferrer = users[referrerAddress].x6Matrix[level].firstLevelReferrals[a];
                users[currentReferrer].x6Matrix[level].firstLevelReferrals[n%3] = userAddress; 

                //set current level
                users[userAddress].x6Matrix[level].currentReferrerAddress = currentReferrer;
                // uint currentReferrerIndex = n%3;
                emit NewUserPlace(userAddress, currentReferrer, level, n%3);

                // if (referrerAddress == owner) {
                //     return sendOwnerETHDividends(userAddress, level);
                // }

                sendDividends(userAddress, currentReferrer, level, ethValue/2);


                total2LevelUsers = 0;

                for (uint i = 0; i < 9; i++) {
                    if (users[referrerAddress].x6Matrix[level].secondLevelReferrals[i] != address(0)) {
                        total2LevelUsers++;
                    }
                }

                if (total2LevelUsers >= 8) {
                    storeTokens(userAddress, referrerAddress, level, ethValue/2);
                } else {
                    // if (referrerAddress == owner) {
                    //     return sendOwnerETHDividends(userAddress, level);
                    // }

                    sendDividends(userAddress, referrerAddress, level, ethValue/2);
                }

                if (total2LevelUsers == 9) {
                    return reinvest(msg.sender, referrerAddress, level);
                }

                return;
            }

            if(n == 0) {
                n = 3;
            } else if(n==3) {
                n = 6;
            } else if(n==6) {
                n = 1;
            } else if(n==1) {
                n = 4;
            } else if(n==4) {
                n = 7;
            } else if(n==7) {
                n = 2;
            } else if(n==2) {
                n = 5;
            } else if(n==5) {
                n = 8;
            } else {
                break;
            }
        }
    }

    function reinvest(address caller, address userAddress, uint8 level) internal {
        address referrer = findFreeX6Referrer(userAddress, level);

        address[3] memory a3;
        address[9] memory a9;
        
        users[userAddress].x6Matrix[level].currentReferrerAddress = referrer;
        users[userAddress].x6Matrix[level].firstLevelReferrals = a3;
        users[userAddress].x6Matrix[level].secondLevelReferrals = a9;
        users[userAddress].x6Matrix[level].reinvestCount++;

        if (userAddress == owner) {
            xQoreStorage.transferTo(multisig, levelPrice[level]);
            // controller.update(owner, level, levelPrice[level], 1);
            emit ReleasedETH(userAddress, owner, level, levelPrice[level]);
        } else {
            xQoreStorage.transferTo(address(this), levelPrice[level]);
            // controller.update(referrer, level, levelPrice[level], 1);
            emit ReleasedETH(userAddress, referrer, level, levelPrice[level]);
        }

        // controller.update(userAddress, level, levelPrice[level], 2);

        emit Reinvest(userAddress, referrer, caller, level);

        if (level != MAX_LEVEL && !users[userAddress].activeX6Levels[level+1]) {
            users[userAddress].x6Matrix[level].blocked = true;
        }

        if (referrer == address(0)) {
            return;
        }

        updateX6Referrer(userAddress, referrer, level, levelPrice[level]);
    }
        
    function findFreeX6Referrer(address userAddress, uint8 level) public view returns(address) {
        if (userAddress == owner) {
            return address(0);
        }

        address userBuf = userAddress;
        uint maxIterations = 50;
        uint iterations = 0;

        while(true) {
            ( , address ref, ) = forsage.users(userBuf);

            iterations++;
            if(iterations >= maxIterations || ref == owner) {
                return owner;
            }

            userBuf = ref;

            if(users[ref].referrer != address(0)) {
                if(users[userBuf].exists && users[userBuf].activeX6Levels[level]) {
                    return userBuf;
                }
            }
        }

        return owner;
    }

    function usersActiveX6Levels(address userAddress, uint8 level) public view returns(bool) {
        return users[userAddress].activeX6Levels[level];
    }

    function usersX6Matrix(address userAddress, uint8 level) public view returns(address currentReferrerAddress, address[3] memory firstLevelReferrals, address[9] memory secondLevelReferrals) {
        return (
            users[userAddress].x6Matrix[level].currentReferrerAddress,
            users[userAddress].x6Matrix[level].firstLevelReferrals,
            users[userAddress].x6Matrix[level].secondLevelReferrals
        );
    }
    
    function isUserExists(address user) public view returns (bool) {
        return (users[user].id != 0);
    }

    function findEthReceiver(address userAddress, address _from, uint8 level) private returns(address, bool) {
        address receiver = userAddress;
        bool isExtraDividends;

        uint8 iter = 0;
        while (true) {
            if (users[receiver].x6Matrix[level].blocked) {
                emit MissedEthReceive(receiver, _from, level);
                isExtraDividends = true;
                receiver = users[receiver].x6Matrix[level].currentReferrerAddress;

                iter++;
                if(iter >= 25) {
                    return (owner, isExtraDividends);
                }
            } else {
                return (receiver, isExtraDividends);
            }
        }

        return (owner, true);
    }


    function sendDividends(address _from, address _to, uint8 _level, uint _value) private {
        (address receiver, bool isExtraDividends) = findEthReceiver(_to, _from, _level);
        if(receiver == owner) {
            receiver = multisig;
        }
        // require(_value > 0, "smth wrong with value");
        // require(depositToken.balanceOf(address(this)) >= _value, "smth wrong with balance");
        (bool res, ) = receiver.call{value: _value}("");
        if(!res) {
            // revert("CannotSendETH");
            emit CannotSendETH(receiver, _value);
        }
        // depositToken.safeTransfer(receiver, _value);
        if(isExtraDividends) {
            emit SentExtraEthDividends(_from, receiver, _level, _value);
        }
    }
    
    function storeTokens(address from, address to, uint8 level, uint value) private {
        address addr = address(xQoreStorage);
        (bool res,) = payable(addr).call{value: value}("");
        if(!res) {
            // revert("CannotSendETH");
            emit CannotSendETH(to, value);
        }
        // depositToken.safeTransfer(address(xQoreStorage), value);
        emit StoredETH(from, to, level, value);
    }
    
    function withdrawLostTokens(address tokenAddress) public onlyContractOwner {
        if (tokenAddress == address(0)) {
            address(uint160(multisig)).transfer(address(this).balance);
        } else {
            IERC20(tokenAddress).transfer(multisig, IERC20(tokenAddress).balanceOf(address(this)));
        }
    }

    function withdrawStoredETH(address payable to, uint value) public onlyContractOwner {
        xQoreStorage.transferTo(to, value);
    }
}