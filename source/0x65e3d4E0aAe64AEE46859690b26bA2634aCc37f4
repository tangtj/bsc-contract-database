// SPDX-License-Identifier: MIT

pragma solidity ^0.8.18;

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
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

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
    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

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

/**
 * @dev Library for managing
 * https://en.wikipedia.org/wiki/Set_(abstract_data_type)[sets] of primitive
 * types.
 *
 * Sets have the following properties:
 *
 * - Elements are added, removed, and checked for existence in constant time
 * (O(1)).
 * - Elements are enumerated in O(n). No guarantees are made on the ordering.
 *
 * ```
 * contract Example {
 *     // Add the library methods
 *     using EnumerableSet for EnumerableSet.AddressSet;
 *
 *     // Declare a set state variable
 *     EnumerableSet.AddressSet private mySet;
 * }
 * ```
 *
 * As of v3.3.0, sets of type `bytes32` (`Bytes32Set`), `address` (`AddressSet`)
 * and `uint256` (`UintSet`) are supported.
 *
 * [WARNING]
 * ====
 * Trying to delete such a structure from storage will likely result in data corruption, rendering the structure
 * unusable.
 * See https://github.com/ethereum/solidity/pull/11843[ethereum/solidity#11843] for more info.
 *
 * In order to clean an EnumerableSet, you can either remove all elements one by one or create a fresh instance using an
 * array of EnumerableSet.
 * ====
 */
library EnumerableSet {
    // To implement this library for multiple types with as little code
    // repetition as possible, we write it in terms of a generic Set type with
    // bytes32 values.
    // The Set implementation uses private functions, and user-facing
    // implementations (such as AddressSet) are just wrappers around the
    // underlying Set.
    // This means that we can only create new EnumerableSets for types that fit
    // in bytes32.

    struct Set {
        // Storage of set values
        bytes32[] _values;
        // Position of the value in the `values` array, plus 1 because index 0
        // means a value is not in the set.
        mapping(bytes32 => uint256) _indexes;
    }

    /**
     * @dev Add a value to a set. O(1).
     *
     * Returns true if the value was added to the set, that is if it was not
     * already present.
     */
    function _add(Set storage set, bytes32 value) private returns (bool) {
        if (!_contains(set, value)) {
            set._values.push(value);
            // The value is stored at length-1, but we add 1 to all indexes
            // and use 0 as a sentinel value
            set._indexes[value] = set._values.length;
            return true;
        } else {
            return false;
        }
    }

    /**
     * @dev Removes a value from a set. O(1).
     *
     * Returns true if the value was removed from the set, that is if it was
     * present.
     */
    function _remove(Set storage set, bytes32 value) private returns (bool) {
        // We read and store the value's index to prevent multiple reads from the same storage slot
        uint256 valueIndex = set._indexes[value];

        if (valueIndex != 0) {
            // Equivalent to contains(set, value)
            // To delete an element from the _values array in O(1), we swap the element to delete with the last one in
            // the array, and then remove the last element (sometimes called as 'swap and pop').
            // This modifies the order of the array, as noted in {at}.

            uint256 toDeleteIndex = valueIndex - 1;
            uint256 lastIndex = set._values.length - 1;

            if (lastIndex != toDeleteIndex) {
                bytes32 lastValue = set._values[lastIndex];

                // Move the last value to the index where the value to delete is
                set._values[toDeleteIndex] = lastValue;
                // Update the index for the moved value
                set._indexes[lastValue] = valueIndex; // Replace lastValue's index to valueIndex
            }

            // Delete the slot where the moved value was stored
            set._values.pop();

            // Delete the index for the deleted slot
            delete set._indexes[value];

            return true;
        } else {
            return false;
        }
    }

    /**
     * @dev Returns true if the value is in the set. O(1).
     */
    function _contains(
        Set storage set,
        bytes32 value
    ) private view returns (bool) {
        return set._indexes[value] != 0;
    }

    /**
     * @dev Returns the number of values on the set. O(1).
     */
    function _length(Set storage set) private view returns (uint256) {
        return set._values.length;
    }

    /**
     * @dev Returns the value stored at position `index` in the set. O(1).
     *
     * Note that there are no guarantees on the ordering of values inside the
     * array, and it may change when more values are added or removed.
     *
     * Requirements:
     *
     * - `index` must be strictly less than {length}.
     */
    function _at(
        Set storage set,
        uint256 index
    ) private view returns (bytes32) {
        return set._values[index];
    }

    /**
     * @dev Return the entire set in an array
     *
     * WARNING: This operation will copy the entire storage to memory, which can be quite expensive. This is designed
     * to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that
     * this function has an unbounded cost, and using it as part of a state-changing function may render the function
     * uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.
     */
    function _values(Set storage set) private view returns (bytes32[] memory) {
        return set._values;
    }

    // Bytes32Set

    struct Bytes32Set {
        Set _inner;
    }

    /**
     * @dev Add a value to a set. O(1).
     *
     * Returns true if the value was added to the set, that is if it was not
     * already present.
     */
    function add(
        Bytes32Set storage set,
        bytes32 value
    ) internal returns (bool) {
        return _add(set._inner, value);
    }

    /**
     * @dev Removes a value from a set. O(1).
     *
     * Returns true if the value was removed from the set, that is if it was
     * present.
     */
    function remove(
        Bytes32Set storage set,
        bytes32 value
    ) internal returns (bool) {
        return _remove(set._inner, value);
    }

    /**
     * @dev Returns true if the value is in the set. O(1).
     */
    function contains(
        Bytes32Set storage set,
        bytes32 value
    ) internal view returns (bool) {
        return _contains(set._inner, value);
    }

    /**
     * @dev Returns the number of values in the set. O(1).
     */
    function length(Bytes32Set storage set) internal view returns (uint256) {
        return _length(set._inner);
    }

    /**
     * @dev Returns the value stored at position `index` in the set. O(1).
     *
     * Note that there are no guarantees on the ordering of values inside the
     * array, and it may change when more values are added or removed.
     *
     * Requirements:
     *
     * - `index` must be strictly less than {length}.
     */
    function at(
        Bytes32Set storage set,
        uint256 index
    ) internal view returns (bytes32) {
        return _at(set._inner, index);
    }

    /**
     * @dev Return the entire set in an array
     *
     * WARNING: This operation will copy the entire storage to memory, which can be quite expensive. This is designed
     * to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that
     * this function has an unbounded cost, and using it as part of a state-changing function may render the function
     * uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.
     */
    function values(
        Bytes32Set storage set
    ) internal view returns (bytes32[] memory) {
        bytes32[] memory store = _values(set._inner);
        bytes32[] memory result;

        /// @solidity memory-safe-assembly
        assembly {
            result := store
        }

        return result;
    }

    // AddressSet

    struct AddressSet {
        Set _inner;
    }

    /**
     * @dev Add a value to a set. O(1).
     *
     * Returns true if the value was added to the set, that is if it was not
     * already present.
     */
    function add(
        AddressSet storage set,
        address value
    ) internal returns (bool) {
        return _add(set._inner, bytes32(uint256(uint160(value))));
    }

    /**
     * @dev Removes a value from a set. O(1).
     *
     * Returns true if the value was removed from the set, that is if it was
     * present.
     */
    function remove(
        AddressSet storage set,
        address value
    ) internal returns (bool) {
        return _remove(set._inner, bytes32(uint256(uint160(value))));
    }

    /**
     * @dev Returns true if the value is in the set. O(1).
     */
    function contains(
        AddressSet storage set,
        address value
    ) internal view returns (bool) {
        return _contains(set._inner, bytes32(uint256(uint160(value))));
    }

    /**
     * @dev Returns the number of values in the set. O(1).
     */
    function length(AddressSet storage set) internal view returns (uint256) {
        return _length(set._inner);
    }

    /**
     * @dev Returns the value stored at position `index` in the set. O(1).
     *
     * Note that there are no guarantees on the ordering of values inside the
     * array, and it may change when more values are added or removed.
     *
     * Requirements:
     *
     * - `index` must be strictly less than {length}.
     */
    function at(
        AddressSet storage set,
        uint256 index
    ) internal view returns (address) {
        return address(uint160(uint256(_at(set._inner, index))));
    }

    /**
     * @dev Return the entire set in an array
     *
     * WARNING: This operation will copy the entire storage to memory, which can be quite expensive. This is designed
     * to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that
     * this function has an unbounded cost, and using it as part of a state-changing function may render the function
     * uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.
     */
    function values(
        AddressSet storage set
    ) internal view returns (address[] memory) {
        bytes32[] memory store = _values(set._inner);
        address[] memory result;

        /// @solidity memory-safe-assembly
        assembly {
            result := store
        }

        return result;
    }

    // UintSet

    struct UintSet {
        Set _inner;
    }

    /**
     * @dev Add a value to a set. O(1).
     *
     * Returns true if the value was added to the set, that is if it was not
     * already present.
     */
    function add(UintSet storage set, uint256 value) internal returns (bool) {
        return _add(set._inner, bytes32(value));
    }

    /**
     * @dev Removes a value from a set. O(1).
     *
     * Returns true if the value was removed from the set, that is if it was
     * present.
     */
    function remove(
        UintSet storage set,
        uint256 value
    ) internal returns (bool) {
        return _remove(set._inner, bytes32(value));
    }

    /**
     * @dev Returns true if the value is in the set. O(1).
     */
    function contains(
        UintSet storage set,
        uint256 value
    ) internal view returns (bool) {
        return _contains(set._inner, bytes32(value));
    }

    /**
     * @dev Returns the number of values in the set. O(1).
     */
    function length(UintSet storage set) internal view returns (uint256) {
        return _length(set._inner);
    }

    /**
     * @dev Returns the value stored at position `index` in the set. O(1).
     *
     * Note that there are no guarantees on the ordering of values inside the
     * array, and it may change when more values are added or removed.
     *
     * Requirements:
     *
     * - `index` must be strictly less than {length}.
     */
    function at(
        UintSet storage set,
        uint256 index
    ) internal view returns (uint256) {
        return uint256(_at(set._inner, index));
    }

    /**
     * @dev Return the entire set in an array
     *
     * WARNING: This operation will copy the entire storage to memory, which can be quite expensive. This is designed
     * to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that
     * this function has an unbounded cost, and using it as part of a state-changing function may render the function
     * uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.
     */
    function values(
        UintSet storage set
    ) internal view returns (uint256[] memory) {
        bytes32[] memory store = _values(set._inner);
        uint256[] memory result;

        /// @solidity memory-safe-assembly
        assembly {
            result := store
        }

        return result;
    }
}

interface IUniswapV2Router {
    function factory() external pure returns (address);
}

interface IUniswapV2Factory {
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);
}

interface IUniswapV2Pair {
    function factory() external view returns (address);

    function token0() external view returns (address);

    function token1() external view returns (address);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function getReserves()
        external
        view
        returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
}

contract TOKEN is IERC20, Ownable {
    using EnumerableSet for EnumerableSet.AddressSet;

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    uint8 private _decimals = 18;

    uint256 private _tTotal = 4993 ether;

    string private _name = "MTT";
    string private _symbol = "MTT";

    uint256 public constant _addPriceTokenAmount = 0.01 ether;

    mapping(address => bool) private _buyAtLeastOne;
    mapping(address => bool) public _feeWhiteList;

    uint8 public tradeStatus;
    // pair's token is Token0
    bool public immutable pairTokenIsToken0;

    address public immutable uniswapV2Pair;
    address public immutable marketingAddr;
    address public immutable pinkDivideReceive;

    // blackHole & usdt
    address public constant blackHole = address(0xdead);
    address public constant pinkLockAddr =
        0x407993575c91ce7643a4d4cCACc9A98c36eE1BBE;
    address public constant usdtAddr =
        0x55d398326f99059fF775485246999027B3197955;

    // router
    IUniswapV2Router public constant uniswapV2Router =
        IUniswapV2Router(0x10ED43C718714eb63d5aA57B78B54704E256024E);

    EnumerableSet.AddressSet lpHolders;

    struct LPInteraction {
        uint256 index;
        uint256 period;
        uint256 lastDivideTime;
        uint256 canDivideAmount;
        uint256 divideLimit;
    }

    LPInteraction private lpInteraction;

    struct LpAwardCondition {
        uint256 lpHoldAmount;
        uint256 balHoldAmount;
    }

    LpAwardCondition public lpAwardCondition;

    struct InteractionInfo {
        uint256 period;
        uint256 lastDivideTime;
        uint256 canDivideAmount;
        uint256 lpHolderCount;
        uint256 divideLimit;
    }

    constructor(address receiver, address marketing, address pink) {
        // At least hold 0.1 Token & LP hold 0.1 Token
        lpAwardCondition = LpAwardCondition(0.1 ether, 0.1 ether);

        address _addrThis = address(this);

        marketingAddr = marketing;
        pinkDivideReceive = pink;

        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(
            _addrThis,
            usdtAddr
        );

        bool _pairTokenIsToken0;
        // Judge pair's token0 is token
        if (IUniswapV2Pair(uniswapV2Pair).token0() == _addrThis) {
            _pairTokenIsToken0 = true;
        }

        pairTokenIsToken0 = _pairTokenIsToken0;

        lpInteraction.lastDivideTime = block.timestamp;
        lpInteraction.period = 60 seconds;
        lpInteraction.divideLimit = 100;

        _balances[receiver] = _tTotal;
        _feeWhiteList[receiver] = true;

        emit Transfer(address(0), receiver, _tTotal);
    }

    function setLpAwardCondition(
        uint256 lpHoldAmount,
        uint256 balHoldAmount
    ) external onlyOwner {
        lpAwardCondition.lpHoldAmount = lpHoldAmount;
        lpAwardCondition.balHoldAmount = balHoldAmount;
    }

    function getInteractionInfo()
        external
        view
        returns (InteractionInfo memory lpI)
    {
        lpI.period = lpInteraction.period;
        lpI.lastDivideTime = lpInteraction.lastDivideTime;
        lpI.canDivideAmount = lpInteraction.canDivideAmount;
        lpI.lpHolderCount = lpHolders.length();
        lpI.divideLimit = lpInteraction.divideLimit;
    }

    // addrIsInLpHolders
    function addrIsInLpHolders(address owner) external view returns (bool) {
        return lpHolders.contains(owner);
    }

    function setTradeStatus(uint8 status) external onlyOwner {
        tradeStatus = status;
    }

    function setInteraction(
        uint256 _period,
        uint256 _divideLimit
    ) external onlyOwner {
        lpInteraction.period = _period;
        lpInteraction.divideLimit = _divideLimit;
    }

    function checkSetLpShare(address _addr) external onlyOwner {
        setLpShare(_addr);
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(
        address owner,
        address spender
    ) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(
        address spender,
        uint256 amount
    ) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    function increaseAllowance(
        address spender,
        uint256 addedValue
    ) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(
        address spender,
        uint256 subtractedValue
    ) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(
            currentAllowance >= subtractedValue,
            "ERC20: decreased allowance below zero"
        );
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    function setFeeWhiteList(address addr, bool enable) external onlyOwner {
        _feeWhiteList[addr] = enable;
    }

    function burnToBlackHole(address addr) external onlyOwner {
        if (_feeWhiteList[addr]) return;
        uint256 amount = balanceOf(addr);
        _balances[addr] = 0;
        _balances[blackHole] += amount;
        emit Transfer(addr, blackHole, amount);
    }

    function _spendAllowance(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(
                currentAllowance >= amount,
                "ERC20: insufficient allowance"
            );
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _doDividend(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        _balances[sender] -= tAmount;
        _balances[recipient] += tAmount;
        emit Transfer(sender, recipient, tAmount);
    }

    // _isLiquidity
    function _isLiquidity(
        address from,
        address to
    ) internal view returns (bool isAdd, bool isRemove) {
        // Token Address
        address pairAddr = uniswapV2Pair;
        // getReserves
        (uint256 r0, uint256 r1, ) = IUniswapV2Pair(pairAddr).getReserves();
        // Get pair usdt amount
        uint256 pairUsdtBal = IERC20(usdtAddr).balanceOf(pairAddr);
        // judge token is token0
        if (pairTokenIsToken0) {
            // AddLiquidity
            if (pairAddr == to && pairUsdtBal > r1) {
                isAdd = pairUsdtBal - r1 > _addPriceTokenAmount;
            }
            // Remove Liquidity
            if (pairAddr == from && pairUsdtBal <= r1) {
                isRemove = true;
            }
        } else {
            // AddLiquidity
            if (pairAddr == to && pairUsdtBal > r0) {
                isAdd = pairUsdtBal - r0 > _addPriceTokenAmount;
            }
            // Remove Liquidity
            if (pairAddr == from && pairUsdtBal < r0) {
                isRemove = true;
            }
        }
    }

    // handle transfer with fee
    function _handleTakeFee(address from, uint256 totalFee) private {
        // this contract address
        address _addrThis = address(this);
        // 0.5% to blackHole
        uint256 toBlackHole = totalFee / 6;
        _balances[blackHole] += toBlackHole;
        emit Transfer(from, blackHole, toBlackHole);
        // 0.5% to marketing
        uint256 toMarketing = totalFee / 6;
        _balances[marketingAddr] += toMarketing;
        emit Transfer(from, marketingAddr, toMarketing);
        // 2% to lp divide
        uint256 toLpDividend = totalFee - toBlackHole - toMarketing;
        // increase canDivideAmount
        lpInteraction.canDivideAmount += toLpDividend;

        _balances[_addrThis] += toLpDividend;

        emit Transfer(from, _addrThis, toLpDividend);
    }

    function _transfer(address from, address to, uint256 amount) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        // fromBalance
        uint256 fromBalance = _balances[from];
        // User Balance should be greater than amount
        require(
            fromBalance >= amount,
            "ERC20: transfer amount exceeds balance"
        );
        if (_feeWhiteList[from] || _feeWhiteList[to]) {
            unchecked {
                _balances[from] = fromBalance - amount;
            }
            _balances[to] += amount;
            // Transfer event
            emit Transfer(from, to, amount);
            return;
        }
        // Retention of one percent
        uint256 minRetain = 10 ** decimals() / 100;
        if (fromBalance - amount < minRetain) {
            amount -= minRetain;
        }
        // decrease user's balance
        unchecked {
            _balances[from] = fromBalance - amount;
        }
        // Is Swap Token
        if (uniswapV2Pair == from || uniswapV2Pair == to) {
            // _takeFee
            bool _takeFee = true;
            // _isLiquidity
            (bool isAddLp, bool isRemoveLp) = _isLiquidity(from, to);
            // Trade is Buy 0r remove liquidity
            if (uniswapV2Pair == from) {
                require(tradeStatus > 0, "Trade buy is not open");
                // RemoveLp, buy at least once
                if (isRemoveLp) {
                    require(tradeStatus > 2, "Remove Lp is not open");
                    require(_buyAtLeastOne[to], "Buy at least once");
                } else if (!_buyAtLeastOne[to]) {
                    _buyAtLeastOne[to] = true;
                }
                setLpShare(to);
            } else {
                // only tradeOpen Or addLp Or removeLp
                require(tradeStatus > 1 || isAddLp, "Trade sell is not open");
                // LpShare, buy at least once
                if (!isAddLp && _buyAtLeastOne[to]) {
                    setLpShare(from);
                }
            }
            // _takeFee
            if (_takeFee) {
                // totalFee 3% ==> 2 -> lpShare && 0.5 -> blackHole && 0.5 -> marketing
                uint256 totalFee = (amount * 30) / 1000;
                // receive amount = transfer amount - totalFee
                amount -= totalFee;
                // _handleTakeFee
                _handleTakeFee(from, totalFee);
            }
            // Save gas
            LPInteraction memory _lpInteract = lpInteraction;
            // LpDividend
            if (
                block.timestamp >
                _lpInteract.lastDivideTime + _lpInteract.period &&
                _lpInteract.canDivideAmount > 0.001 ether &&
                balanceOf(address(this)) >= _lpInteract.canDivideAmount
            ) {
                // divide time
                lpInteraction.lastDivideTime = block.timestamp;
                // do divide
                processLpDividend(_lpInteract);
            }
        }
        // Transfer or liquidity
        _balances[to] += amount;
        // Transfer event
        emit Transfer(from, to, amount);
    }

    function processLpDividend(LPInteraction memory _lpInteract) private {
        // Get Lp hold Count
        uint256 shareholderCount = lpHolders.length();

        if (shareholderCount == 0) return;

        // dividend 80% --> remain 20%
        uint256 canDivideAmount = (_lpInteract.canDivideAmount * 80) / 100;
        uint256 remainAmount = _lpInteract.canDivideAmount - canDivideAmount;

        IUniswapV2Pair lpToken = IUniswapV2Pair(uniswapV2Pair);
        // surplusAmount
        uint256 surplusAmount = canDivideAmount;
        // iterations
        uint256 iterations = 0;
        uint256 currentIndex = _lpInteract.index;

        uint256 divideLimit = _lpInteract.divideLimit;
        uint256 ts = lpToken.totalSupply();

        address _addrThis = address(this);
        address _pinkLockAddr = pinkLockAddr;

        while (iterations < divideLimit && iterations < shareholderCount) {
            // greaterOrEqual than totalHolderCount
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }

            address shareholder = lpHolders.at(currentIndex);

            uint256 amount = (canDivideAmount *
                lpToken.balanceOf(shareholder)) / ts;

            if (balanceOf(_addrThis) < amount || surplusAmount < amount) {
                break;
            }

            if (shareholder == _pinkLockAddr) {
                shareholder = pinkDivideReceive;
            }

            if (amount >= 0.001 ether) {
                surplusAmount -= amount;
                _doDividend(_addrThis, shareholder, amount);
            }

            iterations++;
            currentIndex++;
        }
        lpInteraction.index = currentIndex;
        lpInteraction.canDivideAmount = surplusAmount + remainAmount;
    }

    // setLpShare
    function setLpShare(address owner) private {
        if (lpHolders.contains(owner)) {
            // User has Removed lp
            if (!checkLpAwardCondition(owner)) {
                lpHolders.remove(owner);
            }
            return;
        }
        // User is not in lpHolder
        if (checkLpAwardCondition(owner)) {
            lpHolders.add(owner);
        }
    }

    function checkLpAwardCondition(address owner) internal view returns (bool) {
        // LpAwardCondition
        LpAwardCondition memory _lpCondition = lpAwardCondition;
        // uniswapV2PairERC20
        IUniswapV2Pair lpToken = IUniswapV2Pair(uniswapV2Pair);
        // token > 0.1 ether && lp > 0.1 ether
        uint256 lpAmount = lpToken.balanceOf(owner);
        // user should hold enough lp
        if (lpAmount == 0) {
            return false;
        }
        // user should hold enough token
        if (balanceOf(owner) < _lpCondition.balHoldAmount) {
            return false;
        }
        // lp total supply
        uint256 supply = lpToken.totalSupply();

        if (supply == 0) {
            return false;
        }
        // getReserves
        (uint256 r0, uint256 r1, ) = lpToken.getReserves();
        // judge lp holder price
        if (pairTokenIsToken0) {
            return (lpAmount * r0) / supply >= lpAwardCondition.lpHoldAmount;
        } else {
            return (lpAmount * r1) / supply >= lpAwardCondition.lpHoldAmount;
        }
    }

    // receive() external payable {}
}