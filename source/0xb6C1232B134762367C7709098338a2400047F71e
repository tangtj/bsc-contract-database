// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

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
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
contract Ownable is Context {
    address internal _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), _owner);
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
        require(isOwner(), "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Returns true if the caller is the current owner.
     */
    function isOwner() public view returns (bool) {
        return _msgSender() == _owner;
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
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
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
        // This method relies on extcodesize, which returns 0 for contracts in
        // construction, since the code is only stored at the end of the
        // constructor execution.

        uint256 size;
        // solhint-disable-next-line no-inline-assembly
        assembly { size := extcodesize(account) }
        return size > 0;
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
/// @title Optimized overflow and underflow safe math operations
/// @notice Contains methods for doing math operations that revert on overflow or underflow for minimal gas cost

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
    function add(uint256 x, uint256 y) internal pure returns (uint256 z) {
        require((z = x + y) >= x, "SafeMath: addition overflow");
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
    function sub(uint256 x, uint256 y) internal pure returns (uint256 z) {
        require((z = x - y) <= x, "SafeMath: subtraction overflow");
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
     function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {
        require(x == 0 || (z = x * y) / x == y, "SafeMath: multiplication overflow");
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
    function div(uint256 x, uint256 y) internal pure returns (uint256) {
        require(y > 0, "SafeMath: division by zero");
        return x / y;
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
    function mod(uint256 x, uint256 y) internal pure returns (uint256) {
        require(y != 0, "SafeMath: modulo by zero");
        return x % y;
    }
}

library Math {
    function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
        z = x < y ? x : y;
    }
    function max(uint256 x, uint256 y) internal pure returns (uint256 z) {
        z = x > y ? x : y;
    }
}

interface IPancakeRouter {
    function factory() external pure returns (address);
}

interface IPancakeFactory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IPancakePair {
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
}

interface IMintRouter {
    function factory() external view returns (address);
}

interface IMintFactory {
    function config(uint x) external view returns (bool begin, address token, address from, uint period, uint m, uint n);
    function set0(uint256 x, uint256 y) external;
    function mint(address h, uint256 a, uint256 b, uint256 c) external view returns (uint256 v);
}

contract WjToken is Context, IERC20, Ownable{
    using Address for address;
    using SafeMath for uint256;
    
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    string private _name = 'ZITY0000 Token';
    string private _symbol = 'ZITY0000';
    uint256 private _totalSupply = 210000 * 1e18;
    uint256 private constant BURN_MAX = 189000 * 1e18;
    uint256 private constant OPEN_TIME = 1690848000;

    address private immutable pair;
    address private immutable token0;
    address private immutable mrouter;
    //uint8 private _decimals = 18;

    //uses single storage slot
    struct Volume{
        uint8 s;            //status
        uint40 t;           //time
        uint208 v;          //value
    }

    struct Tp{
        address a;
        address b; 
        address c;
        uint32 d;
        uint256 e;
        uint256 f;
        uint256 g;
        uint256 h;
        uint256 i;
        uint256 j;
        uint256 k;
        uint256 l;  
        uint256 m;      
        uint256 v;     
    }

    struct Tmc{
        uint64 lock;
        uint64 pers;
        uint64 next;    
        uint64 time;  
    }

    struct Tlp{
        uint32 nums;
        uint32 pers;
        uint32 next;    
        uint32 time;  
        uint128 avgs;
    }

    Tmc private _mc;
    Tlp private _lp;
    uint256 private _lp_mint;
    address[] private _lp_holders;    
    mapping(address => uint256) private _lp_indexs;   
    mapping(address => Volume) private _lp_limits;   
    mapping(address => Volume) private _lp_directs;
    mapping (address => Volume[]) private _lp_volumes;
    mapping(address => address) private _invites;
    mapping(address => address[]) private _directs;
    

    /**
     * @dev Sets the values for {name} and {symbol}.
     *
     * All two of these values are immutable: they can only be set once during
     * construction.
     */
    constructor(address r, address t, address m) {
	    pair = IPancakeFactory(IPancakeRouter(r).factory()).createPair(t, address(this));
        token0 = t;
        mrouter = m;

        _balances[msg.sender] = 110000 * 1e18;
        emit Transfer(address(0), msg.sender, 110000 * 1e18);

        _balances[address(this)] = 100000 * 1e18;
        emit Transfer(address(0), address(this), 100000 * 1e18);
        _lp_mint = 100000 * 1e18;

        //_balances[msg.sender] = _totalSupply;
        //emit Transfer(address(0), msg.sender, _totalSupply);
    }

    /**
     * @dev Returns the name of the token.
     */
    function name() public view virtual returns (string memory) {
        return _name;
    }

    /**
     * @dev Returns the symbol of the token, usually a shorter version of the
     * name.
     */
    function symbol() public view virtual returns (string memory) {
        return _symbol;
    }

    /**
     * @dev Returns the number of decimals used to get its user representation.
     * For example, if `decimals` equals `2`, a balance of `505` tokens should
     * be displayed to a user as `5.05` (`505 / 10 ** 2`).
     *
     * Tokens usually opt for a value of 18, imitating the relationship between
     * Ether and Wei. This is the default value returned by this function, unless
     * it's overridden.
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * no way affects any of the arithmetic of the contract, including
     * {IERC20-balanceOf} and {IERC20-transfer}.
     */
    function decimals() public view virtual returns (uint8) {
        return 18;
    }

    /**
     * @dev See {IERC20-totalSupply}.
     */
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    /**
     * @dev See {IERC20-balanceOf}.
     */
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }

    /**
     * @dev See {IERC20-transfer}.
     *
     * Requirements:
     *
     * - `to` cannot be the zero address.
     * - the caller must have a balance of at least `amount`.
     */
    function transfer(address to, uint256 amount) public virtual override returns (bool) {
        _transfer(_msgSender(), to, amount);
        return true;
    }

    /**
     * @dev See {IERC20-allowance}.
     */
    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    /**
     * @dev See {IERC20-approve}.
     *
     * NOTE: If `amount` is the maximum `uint256`, the allowance is not updated on
     * `transferFrom`. This is semantically equivalent to an infinite approval.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    /**
     * @dev See {IERC20-transferFrom}.
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {ERC20}.
     *
     * NOTE: Does not update the allowance if the current allowance
     * is the maximum `uint256`.
     *
     * Requirements:
     *
     * - `from` and `to` cannot be the zero address.
     * - `from` must have a balance of at least `amount`.
     * - the caller must have allowance for ``from``'s tokens of at least
     * `amount`.
     */
    function transferFrom(address from, address to, uint256 amount) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    /**
     * @dev Atomically increases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IERC20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    /**
     * @dev Atomically decreases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IERC20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     * - `spender` must have allowance for the caller of at least
     * `subtractedValue`.
     */
    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "decreased allowance below zero");
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }
        return true;
    }

    /**
     * @dev Moves `amount` of tokens from `from` to `to`.
     *
     * This internal function is equivalent to {transfer}, and can be used to
     * e.g. implement automatic token fees, slashing mechanisms, etc.
     *
     * Emits a {Transfer} event.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `from` must have a balance of at least `amount`.
     */
    function _transfer(address from, address to, uint256 amount) internal virtual {
        require(from != address(0), "ERC20: transfer from 0 address");
        require(to != address(0), "ERC20: transfer to 0 address");
        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "Insufficient funds");
        uint256 received = amount;
        if(amount == 0){
           if(from != to){
                if(_invites[from] == address(0)){
                    require(_invites[to] != from, "Reject recursion invite");
                    if(!from.isContract() && !to.isContract()){
                        _invites[from] = to;
                        if(_directs[to].length < 256){
                            _directs[to].push(from);
                        }
                        _setHolderLp(from, 1);
                    }
                }
            }
            _setHolderLp(to, 0);
        }else{
            (uint8 action, address holder) = _isLiquidity(from, to, amount);
            if(action > 0){
                if(action != 3){
                    require(block.timestamp > OPEN_TIME, "Not Open");
                }
                uint256 fee;
                uint256 left;
                if(action == 1){
                    fee = amount / 100;
                }else if(action == 2){
                    if(amount == fromBalance){
                        //Retain holding address
                        amount -= Math.min(amount / 10000, 1e14);
                        received = amount;
                    }
                    fee = amount / 100;
                }else if(action == 4){
                    uint256 burn = amount / 50;
                    if(burn > 0){
                        //Limit burn Max
                        uint256 v = _balances[address(0xdEaD)];
                        if(BURN_MAX > v){
                            unchecked{
                                burn = Math.min(BURN_MAX - v , burn);
                                _balances[address(0xdEaD)] += burn;
                            }
                            emit Transfer(holder, address(0xdEaD), burn);
                        }else{
                            //Into Lp
                            left = burn;
                        }
                        unchecked{
                            received -= burn;
                        }
                    }
                }

                if(fee > 0){
                    unchecked{
                        received -= fee;

                        //Level dividends and left info Lp
                        left += _dividendLevels(holder, amount, fee);
                     }
                }

                if(left > 0){
                    //Lp dividends
                    unchecked{
                        _balances[address(this)] += left;
                    }
                    emit Transfer(holder, address(this), left);
                }
                if(action == 1){

                }else if(action == 2){
                    //Trigger dividends
                    _dividendLp();
                    _dividendMint();
                }else{
                    //Add or Remove Liquidity
                    _setHolderLp(holder, action);
                }
            }
        }
        // Overflow not possible: the sum of all balances is capped by totalSupply, and the sum is preserved by
        // decrementing then incrementing.
        unchecked{
            _balances[from] -= amount;
            _balances[to] += received;
        }
        emit Transfer(from, to, received);
    }
    
    

    function _isLiquidity(address from, address to, uint256 amount) private view returns(uint8 action, address holder){
        //Avoid stack too deep
        //address token0 = IPancakePair(pair).token0();
        //1,2,3,4 
        if(pair == from){
            //Buy
            action = 1;
            uint256 balance0 = IERC20(token0).balanceOf(from);
            (uint256 reserve0,,) = IPancakePair(from).getReserves();
            if(balance0 < reserve0){                
                //RemoveLiquidity;
                action = 4;
            }
            holder = to;
        }else if(pair == to){
            //Sell
            action = 2;
            uint256 balance0 = IERC20(token0).balanceOf(to);
            (uint256 reserve0, uint256 reserve1,) = IPancakePair(to).getReserves();
            if(reserve0 == 0){
                require(from == _owner, "First addition must be owner");
                action = 3;
            }else if(balance0 > reserve0){
                if(_isLiquidity0(balance0 - reserve0, amount, reserve0, reserve1)){
                    //AddLiquidity;
                    action = 3;
                }
            }
            holder = from;
        } 
    }

    function _isLiquidity0(uint256 amount0, uint256 amount1, uint256 reserve0, uint256 reserve1) private pure returns(bool){
        //Avoid stack too deep
        unchecked{
            //103 gas
            return amount0 * reserve1 * 10000 / reserve0 / 9949 > amount1;
        }
    }
    

    function _setHolderLp(address holder, uint8 action) private{
        Volume[] storage a = _lp_volumes[holder];
        uint j = a.length;
        if(j == 0 && action != 3){
            return ;
        }
        //uses single storage slot
        uint40 t = uint40(block.timestamp);
        uint208 v = uint208(IERC20(pair).balanceOf(holder));
        //Not Add Liquidity
        if(v < 1e9 && action != 3){
            if(j > 0){
                --_lp.nums;
                delete _lp_limits[holder];
                delete _lp_volumes[holder];
                delete _lp_holders[_lp_indexs[holder]];
                address f = _invites[holder];
                if(f != address(0)){
                    _lp_directs[f].s = 0;
                }
            }
            return;
        }
        if(j == 0){
            j = _lp_holders.length;
            uint i = _lp_indexs[holder];
            if(j > i && _lp_holders[i] == address(0)){
                _lp_holders[i] = holder;
            }else{
                //First Add Liquidity Place
                _lp_indexs[holder] = j;
                _lp_holders.push(holder);
            }
            ++_lp.nums;

            //Active
            _lp_limits[holder].s = 1;

            //Reduce dividends gas
            if(_directs[holder].length == 0){
                _lp_directs[holder].s = 1;
            }
        }else if(j == 1){
            Volume memory e = a[0];
            if(e.v > v || e.s == 0){
                e.s = 1;
                e.v = v;
                a[0] = e;
            }else if(e.v == v && action == 0){
                return;
            }
        }else{
            //Last
            Volume memory e = a[j - 1];
            if(e.v == v){
                if(e.t < t){
                    e.s = 1;
                    a[0] = e;
                    //a.length = 1;
                    _deleteX(a, 1);
                }else if(e.s == 0){
                    a[j - 1].s = 1;
                }
                if(action == 0){
                    return;
                }
            }else if(e.v < v){
                uint i = j;
                while(i > 0){
                    unchecked{--i;}
                    e = a[i];
                    if(e.v < v && e.s == 0){
                        e.s = 1;
                        e.v = v;
                        a[i] = e;
                        break;
                    }
                }
            }else if(e.v > v){
                uint i = j;
                while(i > 0){
                    unchecked{--i;}
                    e = a[i];
                    if(e.t < t){
                        if(e.v > v){
                            a[i].v = v;
                            if(++i < j){
                                //a.length = i;
                                _deleteX(a, i);
                                j = i;
                            }
                        }else{
                            unchecked{++i;}
                        }
                        break;
                    }
                }
                while(i < j){
                    e = a[i];
                    if(e.v > v){
                        e.s = 1;
                        e.v = v;
                        a[i] = e;
                        if(++i < j){
                            //a.length = i;
                            _deleteX(a, i);
                        }
                        break;
                    }
                    unchecked{++i;}
                }   
            }
        }
        //Append Add Liquidity
        if(action == 3){
            a.push(Volume(0, t + 86400, v));
        }
        address p = _invites[holder];
        if(p != address(0)){
            _lp_directs[p].s = 0;
        }
    }

    function _deleteX(Volume[] storage a, uint i) private{
        uint j = a.length;
        while(j > i){
            a.pop();
            unchecked{--j;}
        }
    }
    
    function _dividendLevels(address from, uint256 amount, uint256 fee) private returns(uint256) {
        //If nesting to reduce code, and save gas
        //And avoid stack too deep, few variables as possible
        address f = _invites[from];
        if(f == address(0)){
            return fee;
        }
        uint i;
        uint j;
        Tp memory p = _createP(from, pair, amount, fee);
        address[] memory a = new address[](8);
        do{
            i = 0;
            while(i < j){
                if(a[i] == f){
                    return fee;
                }
                unchecked{++i;}
            }
            
            p.b = f;
            _limitLevelLp(p);
            if(p.i > 0){
                _profitLevelLp(p);
                if(p.l > 0){
                    unchecked{
                        fee -= p.l;
                        //Avoid non divide exactly
                        if(fee < 1000){
                            p.l += fee;
                            fee = 0;
                        }
                        _sendLevelLp(p);
                        if(fee == 0 || ++p.d == 2){
                            break;
                        }
                    }
                }
                p.i = 0;
            }

            if(a.length == j){
                //Array Expansion Maximum 24, Limited EVM 1024 bits, and to save gas
                if(j == 24){
                    break;
                }
                i = 0;
                address[] memory b = new address[](j + 8);
                while(i < j){
                    b[i] = a[i];
                    unchecked{++i;}
                }
                a = b;
            }
            a[j] = f;
            unchecked{++j;}
            f = _invites[f];
        } while(f != address(0) && f != from);

        return fee; 
    }


    function _createP(address a, address c, uint256 e, uint256 f) private pure returns(Tp memory){
       return Tp(a, address(0), c, 0, e, f, 0, 0 , 0, 0, 0 ,0 , 0, 0); 
    }


    function _profitLevelLp(Tp memory p) private pure {
        unchecked{
            if(p.d == 0){
                p.j = p.e;
            }else{
                p.j = p.g;
                p.j += (p.e - p.g) >> 2;
            }
            p.k = p.i < p.j ? p.i : p.j;
            p.l = p.f * p.k * 4 / p.e / 5;
            p.g = p.j - p.k;
            p.v += p.k;
            if(p.v >= p.h){
                p.v = 1e27;
            }
        }
    }


    function _limitLevelLp(Tp memory p) private{
        Volume memory e = _lp_limits[p.b];
        if(e.s == 0){
            return;
        }
        uint256 v = e.v;
        if(v > 0){
            //Reset transaction limit, At Time 00:00:00
            if(e.t < block.timestamp){
                v = 0;
            }
        }
        if(v < 1e27){
            unchecked{
                uint256 h = _effectLevelLp(p.b);
                if(h > 0){
                    if(p.m == 0){
                        p.m = 2e23 * _balances[p.c] / IERC20(p.c).totalSupply();
                    }
                    h *= p.m;
                    h /= 1e23;
                    if(v < h){
                        p.h = h;
                        p.i = h - v;
                        p.v = v;
                    }
                }
            }
        }
    }


    function _effectLevelLp(address holder) private returns (uint256 v){
        //Avoid level inter transitions Lp
        Volume[] memory a = _lp_volumes[holder];
        if(a.length > 0){
            //00:00:00 effective
            uint i = a.length;
            uint t = block.timestamp;
            unchecked{
                Volume memory e = a[i - 1];
                if(e.s == 0){
                    _setHolderLp(holder, 0);
                    a = _lp_volumes[holder];
                    i = a.length;
                }
                while(i > 0){
                    e = a[--i];
                    if(e.t < t || (e.t + 28800) / 86400 * 86400 - 28800 < t){
                        v = e.v;
                        break;
                    }
                }
            }
            if(v > 0){
                uint256 r = IERC20(pair).balanceOf(holder);
                if(v > r){
                    v = r;
                    _setHolderLp(holder, 0);
                }
            }
        }
    }


    function _sendLevelLp(Tp memory p) private{
        //Avoid transfer to LP, After exceeding the limit, wait for reset at time 00:00:00
        unchecked{
            Volume storage e = _lp_limits[p.b];
            e.t = uint40((block.timestamp + 28800) / 86400 * 86400 + 57600);  
            e.v = uint208(p.v); 
            _balances[p.b] += p.l;
        }
        emit Transfer(p.a, p.b, p.l);
    }


    function _dividendLp() private {
        Tlp memory lp = _lp;
        uint32 t = uint32(block.timestamp % 2**32);
        if(lp.time == 0){
            if(t < OPEN_TIME + 86400){
                return;
            }
        }else{
            if(lp.time / 86400 * 86400 + 86400 < t){
                lp.pers = 0;
            }
        }
        if(lp.pers > lp.nums*3>>3){
            return;
        }
        uint256 r = _release2Lp();
        if(r == 0){
            return;
        }
        uint256 s;
        uint n;
        uint iter;
        uint length = _lp_holders.length;
        uint32 pers = lp.pers;
        uint32 next = lp.next;
        //1. After joining LP for 24 hours, effected
        //2. Avoid transfer LP and take the minimum value of 24 hours
        while (iter < length) {
            if (next >= length) {
                next = 0;
            }
            //Reset 
            if(next == 0){
                lp.avgs = uint128(1e24 * r / IERC20(pair).totalSupply() / 11);
            }
            address h = _lp_holders[next];
            unchecked{
                ++iter;
                ++next;
            }
            if(h == address(0)){
                continue;
            }
            unchecked{
                uint256 v = _effectHolderLp(h) + _directsLevelLp(h) / 10;
                if(v > 0){
                    v = v * lp.avgs / 1e23;
                    s += v;
                    if(r > s){
                        ++pers;
                        _balances[h] += v;
                        emit Transfer(address(this), h, v);
                        if(++n == 10){
                            break;
                        }
                    }else{
                        s -= v;
                        pers = ~uint32(0);
                        if(next > 0){
                            --next;   
                        }
                        break;
                    }
                }
            }
        }
        if(s > 0){
            unchecked{
                _balances[address(this)] -= s;
            }
        }
        lp.pers = pers;
        lp.next = next;
        lp.time = t;
        _lp = lp;
    }
    

    function _release2Lp() private view returns (uint256 s){
        s = _balances[address(this)];
        unchecked{
            uint t = block.timestamp;
            uint f = OPEN_TIME + 94608000;
            if(t < f){
                uint256 r;
                uint256 v = _lp_mint / 10;
                //Release in 3 years, 2:3:5 
                f -= 63072000;
                if(t < f){
                    r = v << 3;
                    v <<= 1;
                }else {
                    f += 31536000;
                    if(t < f){
                        r = v * 5;
                        v *= 3;
                    }else {
                        f += 31536000;
                        v *= 5;
                    }
                }
                r += (f - t) * v / 31536000;
                if(s > r){
                    s -= r;
                }else{
                    s = 0;
                }  
            }
        }
    }


    function _effectHolderLp(address holder) private returns (uint256 v){
        Volume[] memory a = _lp_volumes[holder];
        if(a.length > 0){
            unchecked{
                uint i = a.length;
                uint t = block.timestamp;
                Volume memory e = a[i - 1];
                if(e.s == 0){
                    _setHolderLp(holder, 0);
                    a = _lp_volumes[holder];
                    i = a.length;
                }
                while(i > 0){
                    e = a[--i];
                    if(e.t < t){
                        v = e.v;
                        break;
                    }
                }
            }
            if(v > 0){
                uint256 r = IERC20(pair).balanceOf(holder);
                if(v > r){
                    v = r;
                    _setHolderLp(holder, 0);
                }
            }
        }
    }


    function _directsLevelLp(address holder) private returns (uint256 v){
        Volume memory e = _lp_directs[holder];
        if(e.s == 0){
            address[] memory a = _directs[holder];
            uint i = a.length;
            while(i > 0){
                unchecked{
                    v += _directsLevelLp0(a[--i]);
                }
            }
            e.s = 1;
            e.t = uint40(block.timestamp);
            e.v = uint208(v);
            _lp_directs[holder] = e;
        }else{
            v = e.v;
        }
    }


    function _directsLevelLp0(address holder) private returns (uint256 v){
        Volume[] memory a = _lp_volumes[holder];
        if(a.length > 0){
            unchecked{
                Volume memory e = a[a.length - 1];
                if(e.s == 0){
                    _setHolderLp(holder, 0);
                    a = _lp_volumes[holder];
                    if(a.length > 0){
                        v = a[a.length - 1].v;
                    }
                }else{
                    v = e.v;
                }
            }
        }
    }


    function _dividendMint() private {
        address factory = IMintRouter(mrouter).factory();
        if(factory == address(0)){
            return;
        }
        Tmc memory mc = _mc;
        if(mc.lock == 1){
            return;
        }
        (bool begin, address token, address from, uint period, uint m, uint n) = IMintFactory(factory).config(_lp.nums);
        if(!begin){
            //Reset
            if(mc.time > 0){
                delete _mc;
            }
            return;
        }
        if(mc.time > 0){
            if(period > 0){
                if(block.timestamp < mc.time + period){
                    return;
                }
            }
            if(m > 0){
                if(mc.time / 86400 * 86400 + 86400 < block.timestamp){
                    mc.pers = 0;
                }
                if(mc.pers > m){
                    return;
                }
            }
        }
        uint256 s = IERC20(token).balanceOf(from);
        if(s == 0){
            return;
        }
        _mc.lock = 1;
        uint iter;
        uint next = mc.next;
        uint length = _lp_holders.length;
        uint256 t = IERC20(pair).totalSupply();
        while (iter < length) {
            if (next >= length) {
                next = 0;
            }
            //Reset 
            if(next == 0){
                IMintFactory(factory).set0(_lp.nums, t);
            }
            address h = _lp_holders[next];
            unchecked{
                ++iter;
                ++next;
            }
            if(h == address(0)){
                continue;
            }
            uint256 v = IMintFactory(factory).mint(h, t, _effectHolderLp(h), _directsLevelLp(h));
            if(v == 0){
                continue;
            }
            if(v > s){
                v = s;
            }
            if(from == address(this)){
                if(!_safeTransfer(token, h , v)){
                    if(next > 0){
                        unchecked{--next;}
                    }
                    break;
                }
            }else{
               if(!_safeTransferFrom(token, from, h , v)){
                    if(next > 0){
                        unchecked{--next;}
                    }
                    break;
               }
            }
            unchecked{
                s -= v;
                ++mc.pers;
                if(--n == 0 || s == 0){
                    break;
                }
            }
        }
        mc.lock = 0;
        mc.next = uint64(next);
        mc.time = uint64(block.timestamp);
        _mc = mc;
    }
    

    function _safeTransfer(address token, address to, uint value) private returns (bool){
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        return success && (data.length == 0 || abi.decode(data, (bool)));
        //require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FAILED');
    }
    

    function _safeTransferFrom(address token, address from, address to, uint value) private returns (bool){
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        return success && (data.length == 0 || abi.decode(data, (bool)));
        //require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }


    /**
     * @dev Sets `amount` as the allowance of `spender` over the `owner` s tokens.
     *
     * This internal function is equivalent to `approve`, and can be used to
     * e.g. set automatic allowances for certain subsystems, etc.
     *
     * Emits an {Approval} event.
     *
     * Requirements:
     *
     * - `owner` cannot be the zero address.
     * - `spender` cannot be the zero address.
     */
    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "ERC20: approve from 0 address");
        require(spender != address(0), "ERC20: approve to 0 address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    /**
     * @dev Updates `owner` s allowance for `spender` based on spent `amount`.
     *
     * Does not update the allowance amount in case of infinite allowance.
     * Revert if not enough allowance is available.
     *
     * Might emit an {Approval} event.
     */
    function _spendAllowance(address owner, address spender, uint256 amount) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }

    function revertLpMint(uint256 v) external onlyOwner{
       _lp_mint = v;
    }    
    
    function revertLpHolders(address[] calldata a) external onlyOwner{
        uint l = a.length;
        uint t = OPEN_TIME - 86400;
        address p = pair;
        while(l > 0){
            unchecked{--l;}
            address h = a[l];
            uint256 v = IERC20(p).balanceOf(h);
            if(v > 0){
                Volume[] storage e = _lp_volumes[h];
                if(e.length == 0){
                    uint i = _lp_indexs[h];
                    uint j = _lp_holders.length;
                    if(j > i && _lp_holders[i] == address(0)){
                        _lp_holders[i] = h;
                    }else{
                        _lp_indexs[h] = j;
                        _lp_holders.push(h);
                    }
                    ++_lp.nums;
                    //Active
                    _lp_limits[h].s = 1;
                    e.push(Volume(1, uint40(t), uint208(v)));
                }
            }
        }
    }    
    

    function revertInvites(address[] calldata a, address[] calldata b) external onlyOwner{
        require(a.length > 0 && a.length == b.length, "Length mismatched");
        uint i = a.length;
        while(i > 0){
            unchecked{--i;}
            address f = a[i];
            address t = b[i];
            if(f != t && _invites[f] == address(0) && _invites[t] != f){
                _invites[f] = t;
                _directs[t].push(f);
            }
        }
    }

    function getDirects(address from) external view returns (address[] memory){
        return _directs[from];
    }

    function fromInviters(address from) external view returns (address[] memory){
        address p = _invites[from];
        if(p == address(0)){
            return new address[](0);
        }
        uint i;
        uint j;
        bool f;
        address[] memory a = new address[](16);
        while(p != address(0) && p != from){
            i = 0;
            while(i < j){
                if(a[i] == p){
                    f = true;
                    break;
                }
                ++i;
            }
            if(f){
                break;
            }else{
                if(a.length == j){
                    i = 0;
                    address[] memory b = new address[](j + 16);
                    while(i < j){
                       b[i] = a[i];
                       ++i;
                    }
                    a = b;
                }
                a[j ++] = p;
            }
            p = _invites[p];
        }
        if(j < a.length){
            i = 0;
            address[] memory b = new address[](j);
            while(i < j){
                b[i] = a[i];
                ++i;
            }
            a = b;
        }
        return a; 
    }
}