pragma solidity ^0.6.0;
pragma experimental ABIEncoderV2;
// SPDX-License-Identifier: GPL-3.0-or-later

contract Context {
    // Empty internal constructor, to prevent people from mistakenly deploying
    // an instance of this contract, which should be used via inheritance.
    constructor() internal {}

    function _msgSender() internal view returns (address payable) {
        return msg.sender;
    }

    function _msgData() internal view returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
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
contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() internal {
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

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(_owner == _msgSender(), 'Ownable: caller is not the owner');
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     */
    function _transferOwnership(address newOwner) internal {
        require(newOwner != address(0), 'Ownable: new owner is the zero address');
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
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
        require(c >= a, 'SafeMath: addition overflow');

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
        return sub(a, b, 'SafeMath: subtraction overflow');
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
        require(c / a == b, 'SafeMath: multiplication overflow');

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
        return div(a, b, 'SafeMath: division by zero');
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
        return mod(a, b, 'SafeMath: modulo by zero');
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

    function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
        z = x < y ? x : y;
    }

    // babylonian method (https://en.wikipedia.org/wiki/Methods_of_computing_square_roots#Babylonian_method)
    function sqrt(uint256 y) internal pure returns (uint256 z) {
        if (y > 3) {
            z = y;
            uint256 x = y / 2 + 1;
            while (x < z) {
                z = x;
                x = (y / x + x) / 2;
            }
        } else if (y != 0) {
            z = 1;
        }
    }
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
        // According to EIP-1052, 0x0 is the value returned for not-yet created accounts
        // and 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470 is returned
        // for accounts without code, i.e. `keccak256('')`
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly {
            codehash := extcodehash(account)
        }
        return (codehash != accountHash && codehash != 0x0);
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
        require(address(this).balance >= amount, 'Address: insufficient balance');

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{value: amount}('');
        require(success, 'Address: unable to send value, recipient may have reverted');
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
        return functionCall(target, data, 'Address: low-level call failed');
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
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, 'Address: low-level call with value failed');
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
        require(address(this).balance >= value, 'Address: insufficient balance for call');
        return _functionCallWithValue(target, data, value, errorMessage);
    }

    function _functionCallWithValue(
        address target,
        bytes memory data,
        uint256 weiValue,
        string memory errorMessage
    ) private returns (bytes memory) {
        require(isContract(target), 'Address: call to non-contract');

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{value: weiValue}(data);
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
     * @dev Returns the bep token owner.
     */
    function getOwner() external view returns (address);

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
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

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

library SafeBEP20 {
    using SafeMath for uint256;
    using Address for address;

    function safeTransfer(
        IBEP20 token,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(
        IBEP20 token,
        address from,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    /**
     * @dev Deprecated. This function has issues similar to the ones found in
     * {IBEP20-approve}, and its usage is discouraged.
     *
     * Whenever possible, use {safeIncreaseAllowance} and
     * {safeDecreaseAllowance} instead.
     */
    function safeApprove(
        IBEP20 token,
        address spender,
        uint256 value
    ) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        // solhint-disable-next-line max-line-length
        require(
            (value == 0) || (token.allowance(address(this), spender) == 0),
            'SafeBEP20: approve from non-zero to non-zero allowance'
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(
        IBEP20 token,
        address spender,
        uint256 value
    ) internal {
        uint256 newAllowance = token.allowance(address(this), spender).add(value);
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(
        IBEP20 token,
        address spender,
        uint256 value
    ) internal {
        uint256 newAllowance = token.allowance(address(this), spender).sub(
            value,
            'SafeBEP20: decreased allowance below zero'
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     */
    function _callOptionalReturn(IBEP20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We use {Address.functionCall} to perform this call, which verifies that
        // the target address contains contract code and also asserts for success in the low-level call.

        bytes memory returndata = address(token).functionCall(data, 'SafeBEP20: low-level call failed');
        if (returndata.length > 0) {
            // Return data is optional
            // solhint-disable-next-line max-line-length
            require(abi.decode(returndata, (bool)), 'SafeBEP20: BEP20 operation did not succeed');
        }
    }
}

library TransferHelper {
    function safeApprove(
        address token,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('approve(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x095ea7b3, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: APPROVE_FAILED');
    }

    function safeTransfer(
        address token,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FAILED');
    }

    function safeTransferFrom(
        address token,
        address from,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }

    function safeTransferBNB(address to, uint256 value) internal {
        (bool success, ) = to.call{value: value}(new bytes(0));
        require(success, 'TransferHelper: BNB_TRANSFER_FAILED');
    }
}


contract ReentrancyGuard {
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

    constructor () internal {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and make it call a
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

contract BEP20 is Context, IBEP20, Ownable {
    using SafeMath for uint256;
    using Address for address;

    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    /**
     * @dev Sets the values for {name} and {symbol}, initializes {decimals} with
     * a default value of 18.
     *
     * To select a different value for {decimals}, use {_setupDecimals}.
     *
     * All three of these values are immutable: they can only be set once during
     * construction.
     */
    constructor(string memory name, string memory symbol) public {
        _name = name;
        _symbol = symbol;
        _decimals = 18;
    }

    /**
     * @dev Returns the bep token owner.
     */
    function getOwner() external override view returns (address) {
        return owner();
    }

    /**
     * @dev Returns the token name.
     */
    function name() public override view returns (string memory) {
        return _name;
    }

    /**
     * @dev Returns the token decimals.
     */
    function decimals() public override view returns (uint8) {
        return _decimals;
    }

    /**
     * @dev Returns the token symbol.
     */
    function symbol() public override view returns (string memory) {
        return _symbol;
    }

    /**
     * @dev See {BEP20-totalSupply}.
     */
    function totalSupply() public override view returns (uint256) {
        return _totalSupply;
    }

    /**
     * @dev See {BEP20-balanceOf}.
     */
    function balanceOf(address account) public override view returns (uint256) {
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
    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    /**
     * @dev See {BEP20-allowance}.
     */
    function allowance(address owner, address spender) public override view returns (uint256) {
        return _allowances[owner][spender];
    }

    /**
     * @dev See {BEP20-approve}.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 amount) public override returns (bool) {
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
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            _msgSender(),
            _allowances[sender][_msgSender()].sub(amount, 'BEP20: transfer amount exceeds allowance')
        );
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
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender].sub(subtractedValue, 'BEP20: decreased allowance below zero')
        );
        return true;
    }

    /**
     * @dev Creates `amount` tokens and assigns them to `msg.sender`, increasing
     * the total supply.
     *
     * Requirements
     *
     * - `msg.sender` must be the token owner
     */
    
    /*function mint(uint256 amount) public onlyOwner returns (bool) {
        _mint(_msgSender(), amount);
        return true;
    }*/

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
    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal {
        require(sender != address(0), 'BEP20: transfer from the zero address');
        require(recipient != address(0), 'BEP20: transfer to the zero address');

        _beforeTokenTransfer(sender, recipient, amount);
        _balances[sender] = _balances[sender].sub(amount, 'BEP20: transfer amount exceeds balance');
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }

    /** @dev Creates `amount` tokens and assigns them to `account`, increasing
     * the total supply.
     *
     * Emits a {Transfer} event with `from` set to the zero address.
     *
     * Requirements
     *
     * - `to` cannot be the zero address.
     */
    function _mint(address account, uint256 amount) internal {
        require(account != address(0), 'BEP20: mint to the zero address');

        _beforeTokenTransfer(address(0), account, amount);
        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
        emit Transfer(address(0), account, amount);
    }

    /**
     * @dev Destroys `amount` tokens from `account`, reducing the
     * total supply.
     *
     * Emits a {Transfer} event with `to` set to the zero address.
     *
     * Requirements
     *
     * - `account` cannot be the zero address.
     * - `account` must have at least `amount` tokens.
     */
    function _burn(address account, uint256 amount) internal {
        require(account != address(0), 'BEP20: burn from the zero address');

        _beforeTokenTransfer(account, address(0), amount);
        _balances[account] = _balances[account].sub(amount, 'BEP20: burn amount exceeds balance');
        _totalSupply = _totalSupply.sub(amount);
        emit Transfer(account, address(0), amount);
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
    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal {
        require(owner != address(0), 'BEP20: approve from the zero address');
        require(spender != address(0), 'BEP20: approve to the zero address');

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    /**
     * @dev Destroys `amount` tokens from `account`.`amount` is then deducted
     * from the caller's allowance.
     *
     * See {_burn} and {_approve}.
     */
    function _burnFrom(address account, uint256 amount) internal {
        _burn(account, amount);
        _approve(
            account,
            _msgSender(),
            _allowances[account][_msgSender()].sub(amount, 'BEP20: burn amount exceeds allowance')
        );
    }
    
    /**
     * @dev Hook that is called before any transfer of tokens. This includes
     * minting and burning.
     *
     * Calling conditions:
     *
     * - when `from` and `to` are both non-zero, `amount` of ``from``'s tokens
     * will be to transferred to `to`.
     * - when `from` is zero, `amount` tokens will be minted for `to`.
     * - when `to` is zero, `amount` of ``from``'s tokens will be burned.
     * - `from` and `to` are never both zero.
     *
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     */
    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual { }
}

contract xWinDefi  {
    
    struct UserInfo {
        uint256 amount;     
        uint256 blockstart; 
    }
    struct PoolInfo {
        address lpToken;           
        uint256 rewardperblock;       
        uint256 multiplier;       
    }
    function DepositFarm(uint256 _pid, uint256 _amount) public {}
    function pendingXwin(uint256 _pid, address _user) public view returns (uint256) {}
    function WithdrawFarm(uint256 _pid, uint256 _amount) public {}
    
    mapping (uint256 => mapping (address => UserInfo)) public userInfo;
    PoolInfo[] public poolInfo;
}

abstract contract OlaFinance {
    
    function mint(uint mintAmount) external virtual returns (uint);
    function redeem(uint redeemTokens) external virtual returns (uint);
    function balanceOf(address owner) external virtual view returns (uint);
    function exchangeRateStored() public virtual view returns (uint);
}

interface RainMakerForOlaLens {
    function claimComp(address holder) external;
    function compAccrued(address holder) external view returns (uint);
}


contract xWinFixedRateStableCoin is ReentrancyGuard, IBEP20, BEP20 {
    
    using SafeMath for uint256;

    // Info of each pool.
    struct PoolInfo {
        string name;
        uint maxLimit;       
        uint totalLockedSupply;
        uint totalInterestPaid;
        uint totalXWINPaid;
        uint interestPerDay;
        uint lockDays;
        uint xwinMultiplier;
    }
    
    struct UserInfo {
        uint amount;     
        uint lastHarvest; 
        uint farmingStart;       
        uint lockedEnd;
        uint totalHarvest; 
        uint unClaimBalance;
    }
    
    bool public stopDeposit = false; 
    bool public emergency = false; 
    uint public entryFee = 20; 
    uint public burnFee = 250; 
    uint public oneDayBlock = 28750; 
    mapping (uint256 => mapping (address => UserInfo)) public userInfo;
    PoolInfo[] public poolInfo;
    address[] public userAddressArray;
    mapping(address => bool) public userAddress;
    address public stablecoin; 
    address public xwin = address(0xd88ca08d8eec1E9E09562213Ae83A7853ebB5d28);
    address public burnAddress = address(0x000000000000000000000000000000000000dEaD);
    address public platformWallet = address(0x62691eF999C7F07BC1653416df0eC4f3CDDBb0c7);
    
    xWinDefi public _xWinDefi = xWinDefi(address(0x1Bf7fe7568211ecfF68B6bC7CCAd31eCd8fe8092));
    OlaFinance public _OlaFinance = OlaFinance(address(0xdBFd516D42743CA3f1C555311F7846095D85F6Fd));
    RainMakerForOlaLens public _rainMaker = RainMakerForOlaLens(address(0x5CB93C0AdE6B7F2760Ec4389833B0cCcb5e4efDa));

    uint public xwinpid = 0;
    
    event Received(address, uint);
    event _StakeToken(address indexed from, uint256 pid, uint256 depositAmt, uint256 finalDeposit);
    event _UnStakeToken(address indexed from, uint256 pid, uint256 totalReturnToUser);
    
    
    constructor (
            string memory name,
            string memory symbol,
            address _stablecoin
        ) public BEP20(name, symbol) {
            stablecoin = _stablecoin;
        }
        
    
    receive() external payable {
        emit Received(msg.sender, msg.value);
    }
    

    function getMyXWINRewards(uint _pid) public view returns (uint) {
        
        PoolInfo memory pool = poolInfo[_pid];
        UserInfo memory user = userInfo[_pid][msg.sender];
        return _getxWINRewards(user.amount, user.lockedEnd, user.lastHarvest, pool.xwinMultiplier);
    }
    
    function _getxWINRewards(uint _amount, uint _lockedEnd, uint _lastHarvest, uint _xwinMultiplier) internal view returns (uint) {
        
        (,uint rewardperblock, uint multiplier) = _xWinDefi.poolInfo(xwinpid);
        uint maxBlock = block.number >= _lockedEnd? _lockedEnd: block.number;
        uint blockdiff = maxBlock.sub(_lastHarvest);
        uint currentRealizedQty = multiplier.mul(rewardperblock).mul(blockdiff).mul(_amount).div(1e18).div(100);
        currentRealizedQty = currentRealizedQty.mul(_xwinMultiplier).div(10000);
        return currentRealizedQty;
    }
    
    function updateRouter(
        address olaFinance_,
        address xWinDefi_
        ) public onlyOwner {
        _OlaFinance = OlaFinance(olaFinance_);
        _xWinDefi = xWinDefi(xWinDefi_);
    }

    function updateTokenAddresses(
        address _stablecoin,
        address _xwin
        ) public onlyOwner {
        
        stablecoin = _stablecoin;
        xwin = _xwin;
    }

    function updatePlatformWallet(address _platformWallet) public onlyOwner {
        platformWallet = _platformWallet;
    }
    
    function updateOneDayBlock(uint _oneDayBlock) public onlyOwner {
        oneDayBlock = _oneDayBlock;
    }
    
    // update xwin farming pool id
    function updateXWINid(uint _xwinpid) public onlyOwner {
        xwinpid = _xwinpid;
    }
    
    function updateStopDeposit(bool _stopDeposit) public onlyOwner {
        stopDeposit = _stopDeposit;
    }

    function updateEmergency(bool _emergency) public onlyOwner {
        emergency = _emergency;
    }

    //allow admin to move unncessary token inside the contract
    function adminMoveToken(address _tokenAddress) public onlyOwner {
        uint256 tokenBal = IBEP20(_tokenAddress).balanceOf(address(this));
        TransferHelper.safeTransfer(_tokenAddress, msg.sender, tokenBal); 
    }
    
    function olaClaimComp() public onlyOwner {
        _rainMaker.claimComp(address(this));
    }

    function getOlaAccruedComp() public view returns (uint) {
        return _rainMaker.compAccrued(address(this));
    }

    function getNumberOfUsers() public view returns (uint) {
        return userAddressArray.length;
    }
    
    function getNumberOfPools() public view returns (uint) {
        return poolInfo.length;
    }

    function getMyStakedBalance(uint _pid) public view returns (uint){
        UserInfo memory user = userInfo[_pid][msg.sender];
        return user.amount;
    }

    function getMyStakedBalanceWithInterest(uint _pid) public view returns (uint){
        UserInfo memory user = userInfo[_pid][msg.sender];
        (uint pendingInterest, ) = _entitledInterest(_pid, msg.sender);
        return user.amount.add(pendingInterest);
    }
    
    function getTotalStakedBalance() public view returns (uint){

        uint totalOlaAmount = IBEP20(address(_OlaFinance)).balanceOf(address(this));
        return totalOlaAmount;
    }

    function _getOwnershipStakeAmount(uint _userBalance) internal view returns (uint){
        
        uint256 redeemratio = _userBalance.mul(1e18).div(totalSupply());
        uint totalAutoStake = getTotalStakedBalance();
        uint withdrawAmt = redeemratio.mul(totalAutoStake).div(1e18);
        withdrawAmt = totalAutoStake < withdrawAmt ? totalAutoStake: withdrawAmt;
        return withdrawAmt;
    }
    
    function updatePool(
            uint _pid, 
            uint _maxLimit,
            uint _lockDays, 
            uint _xwinMultiplier
            ) public onlyOwner {
        
        PoolInfo storage pool = poolInfo[_pid];
        pool.maxLimit = _maxLimit;
        pool.lockDays = _lockDays;
        pool.xwinMultiplier = _xwinMultiplier;
    }

    function add(
        string memory _name, 
        uint _maxLimit, 
        uint _interestPerDay,
        uint _lockDays,
        uint _xwinMultiplier) public onlyOwner {
        
        poolInfo.push(PoolInfo({
            name : _name,
            maxLimit : _maxLimit,
            totalLockedSupply : 0,
            totalInterestPaid : 0,
            totalXWINPaid : 0,
            interestPerDay : _interestPerDay,
            lockDays : _lockDays,
            xwinMultiplier: _xwinMultiplier
        }));
    }
    
    /// @dev user to stake token 
    function stakeToken(uint _pid, uint _amount) external nonReentrant payable {
        
        require(emergency == false, "emergency is on");
        require(stopDeposit == false, "stop depositing");
        PoolInfo storage pool = poolInfo[_pid];
        require(pool.totalLockedSupply.add(_amount) <= pool.maxLimit, "exceed offering limit");
        require(IBEP20(stablecoin).balanceOf(msg.sender) >= _amount, "Not enough balance");
        
        TransferHelper.safeTransferFrom(stablecoin, msg.sender, address(this), _amount);
        UserInfo storage user = userInfo[_pid][msg.sender];
        require(user.amount == 0, "already deposit");
        
        uint entryAmt = _amount.mul(entryFee).div(10000);
        TransferHelper.safeTransfer(stablecoin, platformWallet, entryAmt); 
        uint finalDeposit = _amount.sub(entryAmt);
        user.amount = finalDeposit;
        user.farmingStart = block.number;
        user.lastHarvest = block.number;
        uint endDay = pool.lockDays.mul(oneDayBlock);
        user.lockedEnd = block.number.add(endDay);
        
        //keep the address for record later
        if(userAddress[msg.sender] == false){
            userAddressArray.push(msg.sender);
            userAddress[msg.sender] = true;
        }
        
        _depositOla(finalDeposit);
        _mint(address(this), finalDeposit);
        pool.totalLockedSupply = pool.totalLockedSupply.add(finalDeposit);
        
        TransferHelper.safeApprove(address(this), address(_xWinDefi), finalDeposit); 
        _xWinDefi.DepositFarm(xwinpid, finalDeposit);

        emit _StakeToken(msg.sender, _pid, _amount, finalDeposit);
    }
    
    /// @dev user to unstake token 
    function unStakeToken(uint _pid) external nonReentrant payable {
        
        require(emergency == false, "emergency is on");
        PoolInfo storage pool = poolInfo[_pid];
        UserInfo storage user = userInfo[_pid][msg.sender];
        require(user.amount > 0, "no stake balance found");
        require(user.lockedEnd <= block.number, "still in locked period");
        
        uint withdrawAmt = _getOwnershipStakeAmount(user.amount);
        
        //remove from Ola
        _removeOla(withdrawAmt);
        
        //withdraw xwin farming 
        _xWinDefi.WithdrawFarm(xwinpid, user.amount);

        uint xwinrewards = _getxWINRewards(user.amount, user.lockedEnd, user.lastHarvest, pool.xwinMultiplier);
        uint256 xwinBalance = IBEP20(xwin).balanceOf(address(this));
        require(xwinBalance >= xwinrewards, "not enough xwin. contact admin");
        pool.totalXWINPaid = pool.totalXWINPaid.add(xwinrewards);
        uint burnFeeTotal = xwinrewards.mul(burnFee).div(10000);
        TransferHelper.safeTransfer(xwin, burnAddress, burnFeeTotal); 
        TransferHelper.safeTransfer(xwin, msg.sender, xwinrewards.sub(burnFeeTotal));

        
        _burn(address(this), user.amount);
        (uint pendingInterest, ) = _entitledInterest(_pid, msg.sender);
        uint256 usdtBalance = IBEP20(stablecoin).balanceOf(address(this));
        uint totalReturnToUser = user.amount.add(pendingInterest);
        require(usdtBalance >= totalReturnToUser, "not enough usdt. contact admin");
        
        pool.totalLockedSupply = pool.totalLockedSupply > user.amount ? pool.totalLockedSupply.sub(user.amount) : 0;
        pool.totalInterestPaid = pool.totalInterestPaid.add(pendingInterest);
        user.lastHarvest = 0; 
        user.amount = 0; 
        
        TransferHelper.safeTransfer(stablecoin, msg.sender, totalReturnToUser); 
        
        emit _UnStakeToken(msg.sender, _pid, totalReturnToUser);

    }
    
    //only trigger by emergency
    function emergencyRemove(uint _pid) public  {
        
        require(emergency == true, "only emergency is allow");
        UserInfo storage user = userInfo[_pid][msg.sender];
        require(user.amount > 0, "no balance");
        _xWinDefi.WithdrawFarm(xwinpid, user.amount);
        uint256 ibBalance = IBEP20(address(_OlaFinance)).balanceOf(address(this));
        uint256 redeemratio = user.amount.mul(1e18).div(totalSupply());
        uint withdrawAmt = redeemratio.mul(ibBalance).div(1e18);
        withdrawAmt = ibBalance < withdrawAmt ? ibBalance: withdrawAmt;
        TransferHelper.safeTransfer(address(_OlaFinance), msg.sender, withdrawAmt); 
        _burn(address(this), user.amount);
        user.lastHarvest = 0; 
        user.amount = 0; 
        user.lockedEnd = 0; 
        user.farmingStart = 0; 
    }
    
    function _depositOla(uint _amount) internal {        
        TransferHelper.safeApprove(stablecoin, address(_OlaFinance), _amount); 
        _OlaFinance.mint(_amount);
    }
    
    function _removeOla(uint _amount) internal {        
        _OlaFinance.redeem(_amount);
    }

    function entitledInterest(uint _pid) public view returns (uint){
          
        (uint entitledInterest_, ) = _entitledInterest(_pid, msg.sender);
        return entitledInterest_;
    }
    
    // one day interest calc
    function _entitledInterest(uint _pid, address _user) internal view returns (uint entitledInterest_, uint lastHarvest){
          
        PoolInfo memory pool = poolInfo[_pid];
        UserInfo memory user = userInfo[_pid][_user];
        uint maxBlock = block.number >= user.lockedEnd? user.lockedEnd: block.number;
        if(maxBlock < user.lastHarvest) return (user.unClaimBalance, maxBlock);
        uint diff = maxBlock.sub(user.lastHarvest);
        uint rounded = diff.div(oneDayBlock);
        if(rounded == 0) return (user.unClaimBalance, user.lastHarvest);
        uint modBlock = diff.mod(oneDayBlock);
        uint entitled = pool.interestPerDay.mul(user.amount).mul(rounded).div(1000000);
        entitledInterest_ = user.unClaimBalance.add(entitled);
        return (entitledInterest_, block.number.sub(modBlock));
    }
}