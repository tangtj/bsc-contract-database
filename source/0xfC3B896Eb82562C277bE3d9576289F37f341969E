// SPDX-License-Identifier: MIT
pragma solidity 0.8.12;

// OpenZeppelin Contracts v4.4.1 (security/ReentrancyGuard.sol)

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
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}

interface IStargateRouter {
    function addLiquidity(
        uint256 _poolId,
        uint256 _amountLD,
        address _to
    ) external;

    function instantRedeemLocal(
        uint16 _srcPoolId,
        uint256 _amountLP,
        address _to
    ) external returns (uint256 amountSD);
}

// OpenZeppelin Contracts (last updated v4.5.0) (token/ERC20/IERC20.sol)

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

interface IStargateChef {
    function deposit(uint256 _pid, uint256 _amount) external;

    function withdraw(uint256 _pid, uint256 _amount) external;

    function emergencyWithdraw(uint256 _pid) external;

    function pendingStargate(uint256 _pid, address _user)
        external
        view
        returns (uint256);

    struct UserInfo {
        uint256 amount; // How many LP tokens the user has provided.
        uint256 rewardDebt; // Reward debt. See explanation below.
        //
        // We do some fancy math here. Basically, any point in time, the amount of STGs
        // entitled to a user but is pending to be distributed is:
        //
        //   pending reward = (user.amount * pool.accStargatePerShare) - user.rewardDebt
        //
        // Whenever a user deposits or withdraws LP tokens to a pool. Here's what happens:
        //   1. The pool's `accStargatePerShare` (and `lastRewardBlock`) gets updated.
        //   2. User receives the pending reward sent to his/her address.
        //   3. User's `amount` gets updated.
        //   4. User's `rewardDebt` gets updated.
    }

    function userInfo(uint256 _pid, address _userAddress)
        external
        view
        returns (UserInfo memory _userInfo);

    struct PoolInfo {
        IERC20 lpToken; // Address of LP token contract.
        uint256 allocPoint; // How many allocation points assigned to this pool. STGs to distribute per block.
        uint256 lastRewardBlock; // Last block number that STGs distribution occurs.
        uint256 accStargatePerShare; // Accumulated STGs per share, times 1e12. See below.
    }

    function poolInfo(uint256 _index)
        external
        view
        returns (PoolInfo memory _poolInfo);
}

interface IStargatePool {
  function totalLiquidity() external view returns (uint256);

  function totalSupply() external view returns (uint256);
}

// OpenZeppelin Contracts v4.4.1 (access/Ownable.sol)

// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)

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

// OpenZeppelin Contracts v4.4.1 (security/Pausable.sol)

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
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        require(!paused(), "Pausable: paused");
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
        require(paused(), "Pausable: not paused");
        _;
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

// OpenZeppelin Contracts v4.4.1 (token/ERC20/utils/SafeERC20.sol)

// OpenZeppelin Contracts (last updated v4.5.0) (utils/Address.sol)

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
     * https://diligence.consensys.net/posts/2019/09/stop-using-soliditys-transfer-now/[Learn more].
     *
     * IMPORTANT: because control is transferred to `recipient`, care must be
     * taken to not create reentrancy vulnerabilities. Consider using
     * {ReentrancyGuard} or the
     * https://solidity.readthedocs.io/en/v0.5.11/security-considerations.html#use-the-checks-effects-interactions-pattern[checks-effects-interactions pattern].
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
        return functionCall(target, data, "Address: low-level call failed");
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
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value
    ) internal returns (bytes memory) {
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
        require(isContract(target), "Address: call to non-contract");

        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResult(success, returndata, errorMessage);
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
        require(isContract(target), "Address: static call to non-contract");

        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResult(success, returndata, errorMessage);
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
        require(isContract(target), "Address: delegate call to non-contract");

        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    /**
     * @dev Tool to verifies that a low level call was successful, and revert if it wasn't, either by bubbling the
     * revert reason using the provided one.
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
            // Look for revert reason and bubble it up if present
            if (returndata.length > 0) {
                // The easiest way to bubble the revert reason is using memory via assembly

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

    /**
     * @dev Deprecated. This function has issues similar to the ones found in
     * {IERC20-approve}, and its usage is discouraged.
     *
     * Whenever possible, use {safeIncreaseAllowance} and
     * {safeDecreaseAllowance} instead.
     */
    function safeApprove(
        IERC20 token,
        address spender,
        uint256 value
    ) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
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
        if (returndata.length > 0) {
            // Return data is optional
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}

interface IVaultRegistry {
    function isVault(address _vault) external view returns (bool);
}

interface IStrategy {
  function balanceOfPool() external view returns (uint256);

  function balanceOfWant() external view returns (uint256);

  function totalAssets() external view returns (uint256);

  function deposit() external;

  function withdraw(uint256 _amount) external;

  function want() external returns (address want);

  function retire() external;

  function unirouter() external view returns (address); // ONLY FOR DUALSIDEDS!

  function publicHarvest() external view returns (bool);
}

interface IVault is IERC20 {
  function deposit(uint256 _amount) external returns (uint256 sharesMinted);

  function withdraw(uint256 _shares) external returns (uint256 wantWithdrawn);

  function want() external view returns (IERC20 _want);

  function strategy() external view returns (IStrategy strategy);

  function approve(address _spender, uint256 _amount) external returns (bool);

  function balanceOf(address _user) external view returns (uint256);

  function balance() external view returns (uint256);

  function setNewStrategy(address _newStrategy) external;

  function available() external returns (uint256);

  function getPricePerFullShare() external returns (uint256);

  function initializeVault(
    IStrategy _strategy,
    string memory _name,
    string memory _symbol,
    address _zapper,
    IERC20 _want,
    address owner_
  ) external;
}

interface ILocker {
  function getTier(address user) external view returns (uint256);
}

library Checks {
  function validateAmount(uint256 amount) internal pure {
    require(amount != 0, "Zero amount");
  }

  function validateAddress(address _address) internal pure {
    require(_address != address(0), "Zero address");
  }

  function validateWantOrNot(address want, address isWant) internal pure {
    require(want == isWant, "Given address is not want");
  }
}

abstract contract BaseZapper is Ownable, Pausable {
  using Checks for *;
  using SafeERC20 for IERC20;

  // Constants and immutables //
  uint256 internal constant DEPOSIT_FEE_CAP = 100000; // 10%

  uint256 internal constant FEE_DENOMINATOR = 1_000_000;

  ILocker public immutable locker;

  // Storage variables //

  // Vault registry for validating vaults
  IVaultRegistry public vaultRegistry;

  // Default deposit fee, it can be set by governance later
  uint256 public defaultDepositFee = 100; // denominated by 1_000_000, 0.01%

  // Smart wallet contract that will take the fee on user deposits
  address public smartWallet;

  // Tier ==> deposit fee (1 silver, 2 gold)
  mapping(uint256 => uint256) public tierDepositFees;

  event DepositFeesUpdated(
    uint256 newDefaultDepositFee,
    uint256 newSilverDepositFee,
    uint256 newGoldDepositFee
  );
  event SmartWalletUpdated(address newSmartWallet);
  event FeeTaken(address from, address token, uint256 amount);

  constructor(
    address _vaultRegistry,
    address _smartWallet,
    address _locker
  ) {
    _locker.validateAddress();
    _vaultRegistry.validateAddress();
    _smartWallet.validateAddress();

    locker = ILocker(_locker);
    vaultRegistry = IVaultRegistry(_vaultRegistry);
    smartWallet = _smartWallet;
  }

  // ================== Fee and smart wallet functions ==================
  function setFees(
    uint256 _defaultDepositFee,
    uint256 _silverTierDepositFee,
    uint256 _goldTierDepositFee
  ) external onlyOwner {
    require(
      _defaultDepositFee <= DEPOSIT_FEE_CAP &&
        _silverTierDepositFee <= DEPOSIT_FEE_CAP &&
        _goldTierDepositFee <= DEPOSIT_FEE_CAP,
      "DEPOSIT_FEE_CAP"
    );

    defaultDepositFee = _defaultDepositFee;

    tierDepositFees[1] = _silverTierDepositFee;
    tierDepositFees[2] = _goldTierDepositFee;

    emit DepositFeesUpdated(
      _defaultDepositFee,
      _silverTierDepositFee,
      _goldTierDepositFee
    );
  }

  function setNewSmartWallet(address _smartWallet) external onlyOwner {
    _smartWallet.validateAddress();

    smartWallet = _smartWallet;
    emit SmartWalletUpdated(_smartWallet);
  }

  // ================== Internal check for a vault legicimacy ==================
  function validateVault(address vault) internal view {
    require(vaultRegistry.isVault(vault), "Invalid vault");
  }

  // ================== Emergency pause functions to use in zappers ==================
  function pause() external onlyOwner {
    _pause();
  }

  function unpause() external onlyOwner {
    _unpause();
  }

  // ================== Popular functions to use in zappers ==================
  function _safeTransfer(address token, uint256 amount) internal {
    amount.validateAmount();
    IERC20(token).safeTransfer(msg.sender, amount);
  }

  function _approveToken(
    address token,
    address spender,
    uint256 amount
  ) internal {
    IERC20(token).safeApprove(spender, 0);
    IERC20(token).safeApprove(spender, amount);
  }

  // If the fee taking process is different than implemented here, override this function
  function _takeFee(address token, uint256 fullAmount)
    internal
    virtual
    returns (uint256)
  {
    // get the tier of the user from locker
    // 1 silver, 2 gold, rest is bronze
    uint256 userTier = locker.getTier(msg.sender);

    uint256 discountedFee;
    if (userTier != 1 && userTier != 2) {
      // user is either bronze or does not have a lock
      discountedFee = defaultDepositFee;
    } else {
      discountedFee = tierDepositFees[userTier];
    }

    uint256 feeTaken = (fullAmount * discountedFee) / FEE_DENOMINATOR;

    // due to math roundings on small amounts fee can be 0
    if (feeTaken == 0) {
      emit FeeTaken(msg.sender, token, 0);
      return 0;
    }

    // funds are already taken from user to address(this) let's send the fee to smart wallet
    IERC20(token).safeTransfer(smartWallet, feeTaken);
    emit FeeTaken(msg.sender, token, feeTaken);

    return feeTaken;
  }
}

contract ZapperStargate is BaseZapper, ReentrancyGuard {
  using SafeERC20 for IERC20;
  using Checks for *;

  IStargateRouter private constant stargateRouter =
    IStargateRouter(0x4a364f8c717cAAD9A442737Eb7b8A55cc6cf18D8);
  IStargateChef private constant stargateChef =
    IStargateChef(0x3052A0F6ab15b4AE1df39962d5DdEFacA86DaB47);
  address private constant sBUSD = 0x98a5737749490856b401DB5Dc27F522fC314A4e1; // pool => 5   chef => 1
  address private constant sUSDT = 0x9aA83081AA06AF7208Dcc7A4cB72C94d057D2cda; // pool => 2    chef => 0
  address private constant BUSD = 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56;
  address private constant USDT = 0x55d398326f99059fF775485246999027B3197955;

  struct AssetInfo {
    address lpToken;
    uint16 poolId;
    uint256 chefId;
  }
  // stargate might add new assets to their protocol and we give important allowances to
  // this contract. So ideally, we should not deploy new one. We should be making this
  // as flexible as we can. This mapping will save us hopefully!
  mapping(address => AssetInfo) public underlyingAssets;

  event ZappedIn(address sender, address vault, uint256 sharesZappedIn);
  event ZappedOut(address sender, address vault, uint256 sharesZappedOut);
  event PoolAdded(address _pool);
  event PoolDeleted(address _pool);

  constructor(
    address _vaultRegistry,
    address _smartWallet,
    address _locker
  ) BaseZapper(_vaultRegistry, _smartWallet, _locker) {
    underlyingAssets[BUSD] = AssetInfo(sBUSD, 5, 1);
    underlyingAssets[USDT] = AssetInfo(sUSDT, 2, 0);

    // default values
    tierDepositFees[1] = 50; // 0.0050%
    tierDepositFees[2] = 25; // 0.0025%
  }

  function _addLiquidityToStargate(
    address _fromToken,
    uint256 _amount,
    AssetInfo memory _assetInfo
  ) internal returns (uint256) {
    _approveToken(_fromToken, address(stargateRouter), _amount);

    uint256 _balance = IERC20(_assetInfo.lpToken).balanceOf(address(this));
    stargateRouter.addLiquidity(
      uint256(_assetInfo.poolId),
      _amount,
      address(this)
    );
    _balance = IERC20(_assetInfo.lpToken).balanceOf(address(this)) - _balance;
    _balance.validateAmount();
    return _balance;
  }

  function _removeLiquidityFromStargate(
    address _toToken,
    uint256 _amount,
    AssetInfo memory _assetInfo
  ) internal returns (uint256) {
    _approveToken(_assetInfo.lpToken, address(stargateRouter), _amount);
    uint256 _redeemed = IERC20(_toToken).balanceOf(address(this));
    // this returns something different so we use old school way
    // check before and after
    stargateRouter.instantRedeemLocal(
      _assetInfo.poolId,
      _amount,
      address(this)
    );
    _redeemed = IERC20(_toToken).balanceOf(address(this)) - _redeemed;
    _redeemed.validateAmount();
    return _redeemed;
  }

  function zapIn(
    address _fromToken,
    address _vault,
    uint256 _fromTokenAmount
  ) external whenNotPaused nonReentrant returns (uint256 _sharesZapped) {
    validateVault(_vault);
    _fromTokenAmount.validateAmount();
    require(
      underlyingAssets[_fromToken].lpToken != address(0),
      "WRONG_FROM_TOKEN"
    );

    IERC20(_fromToken).safeTransferFrom(
      msg.sender,
      address(this),
      _fromTokenAmount
    );

    uint256 fee = _takeFee(_fromToken, _fromTokenAmount);
    _fromTokenAmount = _fromTokenAmount - fee;
    _fromTokenAmount.validateAmount();

    AssetInfo memory _assetInfo = underlyingAssets[_fromToken];

    uint256 _lpReceived;
    if (_fromToken == _assetInfo.lpToken) {
      _lpReceived = _fromTokenAmount;
    } else {
      _lpReceived = _addLiquidityToStargate(
        _fromToken,
        _fromTokenAmount,
        _assetInfo
      );
    }

    _approveToken(_assetInfo.lpToken, _vault, _lpReceived);
    _sharesZapped = IVault(_vault).deposit(_lpReceived);
    _safeTransfer(_vault, _sharesZapped);

    emit ZappedIn(msg.sender, _vault, _sharesZapped);
  }

  function zapOut(
    address _toToken,
    address _vault,
    uint256 _sharesAmount
  ) external nonReentrant returns (uint256 _toTokenSent) {
    validateVault(_vault);
    _sharesAmount.validateAmount();
    require(underlyingAssets[_toToken].lpToken != address(0), "WRONG_TO_TOKEN");

    IERC20(_vault).safeTransferFrom(msg.sender, address(this), _sharesAmount);

    uint256 _lpReceived = IVault(_vault).withdraw(_sharesAmount);
    _lpReceived.validateAmount();

    AssetInfo memory _assetInfo = underlyingAssets[_toToken];

    if (_toToken == _assetInfo.lpToken) {
      _toTokenSent = _lpReceived;
    } else {
      uint256 should = calculateWithdrawal(
        _lpReceived,
        underlyingAssets[_toToken].lpToken
      );
      _toTokenSent = _removeLiquidityFromStargate(
        _toToken,
        _lpReceived,
        _assetInfo
      );
      // if user gets lesser than the normal withdrawal then revert
      // User can always zap out to LP token
      require(should <= _toTokenSent, "Stargate is not liquid");
    }

    _safeTransfer(_toToken, _toTokenSent);

    emit ZappedOut(msg.sender, _vault, _sharesAmount);
  }

  // @dev This is the amount the user should receive considering pool is liquid
  function calculateWithdrawal(uint256 amount, address lpToken)
    public
    view
    returns (uint256)
  {
    IStargatePool _lpToken = IStargatePool(lpToken);

    return ((amount * _lpToken.totalLiquidity()) / _lpToken.totalSupply()) - 1;
  }

  function deletePool(address _token) external onlyOwner {
    delete underlyingAssets[_token];
    emit PoolDeleted(_token);
  }

  function addNewPool(address _token, AssetInfo memory _assetInfo)
    external
    onlyOwner
  {
    underlyingAssets[_token] = _assetInfo;
    emit PoolAdded(_token);
  }
}