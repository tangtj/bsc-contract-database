// SPDX-License-Identifier: MIT
pragma solidity =0.8.6;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

abstract contract Ownable {
    address private _owner;
    address private _previousOwner;
    uint256 private _lockTime;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor ()  {
        address msgSender =  msg.sender;
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

library SafeMath {

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

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

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

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
}

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB)
    external
    returns (address pair);
}



interface IUniswapV2Router01 {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    )
    external
    returns (
        uint256 amountA,
        uint256 amountB,
        uint256 liquidity
    );
}

interface IUniswapV2Pair {
    function getReserves()
    external
    view
    returns (
        uint112 reserve0,
        uint112 reserve1,
        uint32 blockTimestampLast
    );
    function sync() external;
}

interface IUniswapV2Router02 is IUniswapV2Router01 {

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;


}

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
                bytes32 lastvalue = set._values[lastIndex];

                // Move the last value to the index where the value to delete is
                set._values[toDeleteIndex] = lastvalue;
                // Update the index for the moved value
                set._indexes[lastvalue] = valueIndex; // Replace lastvalue's index to valueIndex
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
    function _contains(Set storage set, bytes32 value) private view returns (bool) {
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
    function _at(Set storage set, uint256 index) private view returns (bytes32) {
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
    function add(Bytes32Set storage set, bytes32 value) internal returns (bool) {
        return _add(set._inner, value);
    }

    /**
     * @dev Removes a value from a set. O(1).
     *
     * Returns true if the value was removed from the set, that is if it was
     * present.
     */
    function remove(Bytes32Set storage set, bytes32 value) internal returns (bool) {
        return _remove(set._inner, value);
    }

    /**
     * @dev Returns true if the value is in the set. O(1).
     */
    function contains(Bytes32Set storage set, bytes32 value) internal view returns (bool) {
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
    function at(Bytes32Set storage set, uint256 index) internal view returns (bytes32) {
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
    function values(Bytes32Set storage set) internal view returns (bytes32[] memory) {
        return _values(set._inner);
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
    function add(AddressSet storage set, address value) internal returns (bool) {
        return _add(set._inner, bytes32(uint256(uint160(value))));
    }

    /**
     * @dev Removes a value from a set. O(1).
     *
     * Returns true if the value was removed from the set, that is if it was
     * present.
     */
    function remove(AddressSet storage set, address value) internal returns (bool) {
        return _remove(set._inner, bytes32(uint256(uint160(value))));
    }

    /**
     * @dev Returns true if the value is in the set. O(1).
     */
    function contains(AddressSet storage set, address value) internal view returns (bool) {
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
    function at(AddressSet storage set, uint256 index) internal view returns (address) {
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
    function values(AddressSet storage set) internal view returns (address[] memory) {
        bytes32[] memory store = _values(set._inner);
        address[] memory result;

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
    function remove(UintSet storage set, uint256 value) internal returns (bool) {
        return _remove(set._inner, bytes32(value));
    }

    /**
     * @dev Returns true if the value is in the set. O(1).
     */
    function contains(UintSet storage set, uint256 value) internal view returns (bool) {
        return _contains(set._inner, bytes32(value));
    }

    /**
     * @dev Returns the number of values on the set. O(1).
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
    function at(UintSet storage set, uint256 index) internal view returns (uint256) {
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
    function values(UintSet storage set) internal view returns (uint256[] memory) {
        bytes32[] memory store = _values(set._inner);
        uint256[] memory result;

        assembly {
            result := store
        }

        return result;
    }
}


contract  JDGTOKEN is IERC20, Ownable {
    using SafeMath for uint256;
    using EnumerableSet for EnumerableSet.AddressSet;

    mapping(address => uint256) private _tOwned;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcludedFromFee;
    mapping(address => bool) public _updated;
    string public _name ;
    string public _symbol ;
    uint8 public _decimals ;
    uint256 private _tTotal ;
    address public _uniswapV2Pair;
    address public _marketAddr ;
    address public _token ;
    uint256 public _startTimeForSwap;
    uint256 public _intervalSecondsForSwap ;
    uint8 public _enabOwnerAddLiq;  //0 start ， 1 closed

    IUniswapV2Router02 public  _uniswapV2Router;
    address private _fromAddress;
    address private _toAddress;
    uint256 public _currentIndex;
    mapping(address => bool) public _isDividendExempt;
    address public _lpRouter;
    uint256 public _minPeriod ;
    uint256 public _lpDiv;
    mapping (address=>uint) public addLPTime;
    uint256 public _feeMarket = 1;

    mapping(address => bool) public _blackList;
    address public _projectorAddress;

    constructor() {
        _projectorAddress = 0x8df084F9c4227AC6d8E6a28B333c198dA416243A;
        _marketAddr = 0x7903960DAC72Be4dC46f558Ee5f1851adcd20C20;
        address router;
//        if(block.chainid == 56) {
            _token = 0x55d398326f99059fF775485246999027B3197955;
            router = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
        /*
        } else {
            _token = 0x89614e3d77C00710C8D87aD5cdace32fEd6177Bd;
            router = 0xD99D1c33F9fC3444f8101754aBC46c52416550D1;
        }
*/

        _enabOwnerAddLiq = 1;
        _name = "JDGAME";
        _symbol = "JDG";
        _decimals= uint8(18);
        _tTotal = 21000000 * (10**uint256(_decimals));
        _intervalSecondsForSwap =  4*30*86400;
        _minPeriod = 86400;

        // _tOwned[admin] =  210000000* (10**uint256(_decimals));
        _tOwned[_projectorAddress] =  12000000* (10**uint256(_decimals));
        _tOwned[address(this)] = 9000000* (10**uint256(_decimals));
        _uniswapV2Router = IUniswapV2Router02(router);

        //exclude owner and this contract from fee
        _isDividendExempt[0x407993575c91ce7643a4d4cCACc9A98c36eE1BBE] = true;
        _isDividendExempt[address(this)] = true;
        _isDividendExempt[address(0)] = true;
        _isDividendExempt[address(0xdead)] = true;
        _isDividendExempt[_projectorAddress] = true;
        _isDividendExempt[msg.sender] = true;

        emit Transfer(address(0), _projectorAddress,  _tTotal);
        _token.call(abi.encodeWithSelector(0x095ea7b3, _uniswapV2Router, ~uint256(0)));

    }


    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint256) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _tOwned[account];
    }

    function transfer(address recipient, uint256 amount)
    public
    override
    returns (bool)
    {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender)
    public
    view
    override
    returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount)
    public
    override
    returns (bool)
    {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        if(_startTimeForSwap == 0 && msg.sender == address(_uniswapV2Router) ) {
            if(_enabOwnerAddLiq == 1) { require(sender== owner() || sender == _projectorAddress, "not owner"); }
            _startTimeForSwap = block.timestamp;
            _uniswapV2Pair = recipient;
        }
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            msg.sender,
            _allowances[sender][msg.sender].sub(
                amount,
                "ERC20: transfer amount exceeds allowance"
            )
        );
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(
            msg.sender,
            spender,
            _allowances[msg.sender][spender].add(addedValue)
        );
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(
            msg.sender,
            spender,
            _allowances[msg.sender][spender].sub(
                subtractedValue,
                "ERC20: decreased allowance below zero"
            )
        );
        return true;
    }

    //to recieve ETH from uniswapV2Router when swaping
    receive() external payable {}

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(address from, address to, uint256 amount) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        require(!_blackList[from], "blackList");

        uint256 fee = amount * _feeMarket / 100;
        amount = amount - fee;
        if(fee > 0) {
            _basicTransfer(from, _marketAddr, fee);
        }
        _basicTransfer(from, to, amount);

        if(_enMint) {
            if (to==_uniswapV2Pair && _isAddLiquidity()) {
                addLPTime[from] = block.timestamp;
            }
            if (from==_uniswapV2Pair && _isRemoveLiquidity()) {
                addLPTime[to] = 0;
            }
            if(_fromAddress == address(0)) _fromAddress = from;
            if(_toAddress == address(0)) _toAddress = to;
            if(!_isDividendExempt[_fromAddress] && _fromAddress != _uniswapV2Pair ) setShare(_fromAddress);
            if(!_isDividendExempt[_toAddress] && _toAddress != _uniswapV2Pair ) setShare(_toAddress);
            _fromAddress = from;
            _toAddress = to;
            uint lpBal =  getMintNum();

            if(lpBal > 0&& from !=address(this)) {
                process() ;
                _lpDiv = block.timestamp;
            }
        }
    }

    uint public lastClaimTime;
    uint public indexDay;
    mapping(uint =>uint  ) public theDayMint;
    bool public _enMint = true;

    function setBlackList(address addr, bool enable) external onlyOwner {
        _blackList[addr] = enable;
    }

    function setFeeMarket(uint256 val) external onlyOwner {
        require( val > 0 && val <= 100, "invalid value");
        _feeMarket = val;
    }

    function setEnMint(bool val) external onlyOwner {
        _enMint = val;
    }

    function setMinPeriod(uint val) external onlyOwner {
        _minPeriod = val;
    }

    function setEnableOwnerAddLiq(uint8 val) external onlyOwner {
        _enabOwnerAddLiq = val; // 0 is start
    }

    function getC() public view returns (uint){
        return (block.timestamp-_startTimeForSwap)/_minPeriod;
    }

    uint public everyDivi = 30;
    function setEveryDivi(uint val) external onlyOwner {
        everyDivi = val;
    }

    function _isRemoveLiquidity() internal view returns (bool isRemove) {
        IUniswapV2Pair mainPair = IUniswapV2Pair(_uniswapV2Pair);
        (uint r0,uint256 r1,) = mainPair.getReserves();

        address tokenOther = _token;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isRemove = r >= bal;
    }

    function process() private {
        if( (block.timestamp-_startTimeForSwap)/_minPeriod< lastClaimTime||theDayMint[getC()]== getMintNum()||(block.timestamp-_startTimeForSwap)<=13*3600 ){
            return;
        }

        uint256 shareholderCount = _shareholders.length();

        if(shareholderCount == 0)return;

        uint256 tokenBal =  getMintNum();
        uint ss = everyDivi>shareholderCount?shareholderCount:everyDivi;

        for(uint i;i<ss;i++){
            if(getC()<lastClaimTime){
                break;
            }
            if(_currentIndex >= shareholderCount){
                _currentIndex = 0;
                lastClaimTime += 1;
            }

            uint256 amount = tokenBal.mul(IERC20(_uniswapV2Pair).balanceOf(_shareholders.at(_currentIndex))).div(getLpTotal());

            if( amount < 1e13 ||_isDividendExempt[_shareholders.at(_currentIndex)]||addLPTime[_shareholders.at(_currentIndex)]+(12*3600)>block.timestamp||addLPTime[_shareholders.at(_currentIndex)]==0  ) {
                _currentIndex++;
                continue;
            }

            if(theDayMint[getC()]+amount>=tokenBal){
                amount =tokenBal>theDayMint[getC()]? (tokenBal - theDayMint[getC()]):0 ;
                //lastClaimTime += 1;
            }
            _basicTransfer(address(this),_shareholders.at(_currentIndex),amount);
            theDayMint[getC()]+=amount;
            _currentIndex++;
        }
    }


    function setShare(address shareholder) private {
        if(_shareholders.contains(shareholder) ){
            if(IERC20(_uniswapV2Pair).balanceOf(shareholder) == 0) _shareholders.remove(shareholder);
            return;
        }
        _shareholders.add(shareholder);
    }

    function setShareholder(address addr) external onlyOwner {
        _shareholders.add(addr);
    }
    function romveShareholder(address addr) external onlyOwner {
        _shareholders.remove(addr);
    }

    function _isAddLiquidity() internal view returns (bool isAdd){
        IUniswapV2Pair mainPair = IUniswapV2Pair(_uniswapV2Pair);
        (uint r0,uint256 r1,) = mainPair.getReserves();

        address tokenOther = _token;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isAdd = bal > r;
    }

    EnumerableSet.AddressSet _shareholders; //获取分红地址

    function getHolder() public view returns (address [] memory) {
        return _shareholders.values();
    }

    function getHolder(uint i) public view returns (address) {
        return _shareholders.at(i);
    }


    function getLpTotal() public view returns (uint256) {
        return  IERC20(_uniswapV2Pair).totalSupply() - IERC20(_uniswapV2Pair).balanceOf(0x407993575c91ce7643a4d4cCACc9A98c36eE1BBE)
        - IERC20(_uniswapV2Pair).balanceOf(address(0xdead));
    }

    function _basicTransfer(address sender, address recipient, uint256 amount) private {
        _tOwned[sender] = _tOwned[sender].sub(amount, "Insufficient Balance");
        _tOwned[recipient] = _tOwned[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }


    function setIsDividendExempt(address addr,bool value) external onlyOwner {
        _isDividendExempt[addr] = value;
    }

    uint public ddd = 150000e18;


    function setDDD(uint amount) onlyOwner external {
        ddd = amount;
    }

    function getMintNum() public view returns(uint num){
        if(_startTimeForSwap == 0||balanceOf(address(this))==0 ) return 0 ;
        if((block.timestamp - _startTimeForSwap) /_intervalSecondsForSwap==0){
            num = ddd ;
        }else{
            num =  ddd/( ((block.timestamp - _startTimeForSwap) /_intervalSecondsForSwap )*2 );
        }
    }

    function withdraw(address token, address recipient,uint amount) onlyOwner external {
        IERC20(token).transfer(recipient, amount);
    }

    function withdrawBNB() onlyOwner external {
        payable(owner()).transfer(address(this).balance);
    }


}