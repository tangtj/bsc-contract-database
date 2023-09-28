// SPDX-License-Identifier: NOLICENSE
/*
    Site: https://dexhub.app
    Email: dexhub@dexhub.app
*/

pragma solidity 0.8.19;

interface IERC20 {

    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

}

library SafeMath {
    function sqrt(uint y) internal pure returns (uint z) {
        if (y > 3) {
            z = y;
            uint x = y / 2 + 1;
            while (x < z) {
                z = x;
                x = (y / x + x) / 2;
            }
        } else if (y != 0) {
            z = 1;
        }
        // else z = 0 (default value)
    }

    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    unchecked {
        uint256 c = a + b;
        if (c < a) return (false, 0);
        return (true, c);
    }
    }

    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    unchecked {
        if (b > a) return (false, 0);
        return (true, a - b);
    }
    }

    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    unchecked {
        if (a == 0) return (true, 0);
        uint256 c = a * b;
        if (c / a != b) return (false, 0);
        return (true, c);
    }
    }

    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    unchecked {
        if (b == 0) return (false, 0);
        return (true, a / b);
    }
    }

    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
    unchecked {
        if (b == 0) return (false, 0);
        return (true, a % b);
    }
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
    unchecked {
        require(b <= a, errorMessage);
        return a - b;
    }
    }

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
    unchecked {
        require(b > 0, errorMessage);
        return a / b;
    }
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
    unchecked {
        require(b > 0, errorMessage);
        return a % b;
    }
    }
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this;
        return msg.data;
    }
}

library Address {

    function isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCall(target, data, "Address: low-level call failed");
    }

    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }

    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        require(isContract(target), "Address: call to non-contract");

        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return _verifyCallResult(success, returndata, errorMessage);
    }

    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        return functionStaticCall(target, data, "Address: low-level static call failed");
    }

    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        require(isContract(target), "Address: static call to non-contract");

        (bool success, bytes memory returndata) = target.staticcall(data);
        return _verifyCallResult(success, returndata, errorMessage);
    }

    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionDelegateCall(target, data, "Address: low-level delegate call failed");
    }

    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(isContract(target), "Address: delegate call to non-contract");

        (bool success, bytes memory returndata) = target.delegatecall(data);
        return _verifyCallResult(success, returndata, errorMessage);
    }

    function _verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) private pure returns (bytes memory) {
        if (success) {
            return returndata;
        } else {

            if (returndata.length > 0) {

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

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
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

interface IUniSwapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IUniSwapV2Router02 is IUniSwapV2Router01 {
   function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

interface IUniSwapV2Pair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);

    function mint(address to) external returns (uint liquidity);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;

    function initialize(address, address) external;
}

interface IUniSwapV2Factory {
  event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
    
}

library EnumerableSet {

    struct Set {
        bytes32[] _values;
        mapping(bytes32 => uint256) _indexes;
    }

    function _add(Set storage set, bytes32 value) private returns (bool) {
        if (!_contains(set, value)) {
            set._values.push(value);
            set._indexes[value] = set._values.length;
            return true;
        } else {
            return false;
        }
    }

    function _remove(Set storage set, bytes32 value) private returns (bool) {
        uint256 valueIndex = set._indexes[value];

        if (valueIndex != 0) {
            uint256 toDeleteIndex = valueIndex - 1;
            uint256 lastIndex = set._values.length - 1;

            if (lastIndex != toDeleteIndex) {
                bytes32 lastvalue = set._values[lastIndex];

                set._values[toDeleteIndex] = lastvalue;
                set._indexes[lastvalue] = valueIndex;
            }

            set._values.pop();

            delete set._indexes[value];

            return true;
        } else {
            return false;
        }
    }

    function _contains(Set storage set, bytes32 value) private view returns (bool) {
        return set._indexes[value] != 0;
    }

    function _length(Set storage set) private view returns (uint256) {
        return set._values.length;
    }

    function _at(Set storage set, uint256 index) private view returns (bytes32) {
        return set._values[index];
    }

    function _values(Set storage set) private view returns (bytes32[] memory) {
        return set._values;
    }

    struct AddressSet {
        Set _inner;
    }

    function addic(AddressSet storage set, address value) internal returns (bool) {
        return _add(set._inner, bytes32(uint256(uint160(value))));
    }

    function remove(AddressSet storage set, address value) internal returns (bool) {
        return _remove(set._inner, bytes32(uint256(uint160(value))));
    }

    function contains(AddressSet storage set, address value) internal view returns (bool) {
        return _contains(set._inner, bytes32(uint256(uint160(value))));
    }

    function length(AddressSet storage set) internal view returns (uint256) {
        return _length(set._inner);
    }

    function at(AddressSet storage set, uint256 index) internal view returns (address) {
        return address(uint160(uint256(_at(set._inner, index))));
    }

    function values(AddressSet storage set) internal view returns (address[] memory) {
        bytes32[] memory store = _values(set._inner);
        address[] memory result;
        assembly {
            result := store
        }
        return result;
    }
}

contract XXHXX2 is Context, IERC20, Ownable {
    using SafeMath for uint256;
    using Address for address;
    using EnumerableSet for EnumerableSet.AddressSet;
    mapping(address => bool) private _admin;
    event AdminAdded(address indexed newAdmin);
    event AdminRemoved(address indexed oldAdmin);


    EnumerableSet.AddressSet private tokenHoldersEnumSet;

    mapping (address => uint256) private _rOwned;
    mapping (address => uint256) private _tOwned;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) private _isFreeFromFee;
    mapping (address => bool) private _isExcluded;
    mapping (address => uint) public walletToPurchaseTime;
    mapping (address => uint) public walletToSellTime;
    uint8 private constant _decimals = 18;
    uint256 private constant MAX = ~uint256(0);
    address[] private _excluded;
    uint256 private _tTotal = 10_000_000 * 10 **_decimals;    
    uint256 private _rTotal = (MAX - (MAX % _tTotal));
    uint256 private constant TENTH_PERCENT_DENOMINATOR = 1_000;
    uint256 private ONE_TENTH_SUPPLY = (_rTotal / 100) * 1;


    uint256 public  buybackfee = 10;
    address public buybackToken;
    uint256 public _maxInAmount = 200_000 * 10 **_decimals; 
    uint256 public _maxOutAmount = 200_000 * 10 **_decimals; 
    uint256 public numTokensToSwapMarketing;
    uint256 public numTokensToSwapLiquidity; 
    
    uint public sellTime = 0; // 0s per transaciton
    uint public buyTime = 0; // 0s per transaciton

    TotFeesPaidStruct public totFeesPaid;
    string private constant _name = "XXHXX2 Token";
    string private constant _symbol = "XXHXX2"; 

    constructor (
            address _address_mkt,
            // address _address_buyback,
            // address _address_d1,
            // address _address_d2,
            // address _address_d3,
            address _address_dx, 
            address _address_cx, 
            address _address_pv,
            address _router, 
            uint256 _nSwapM,
            uint256 _nSwapL
            ) 
             {
            numTokensToSwapMarketing = _nSwapM * 10**_decimals;  
            numTokensToSwapLiquidity = _nSwapL * 10**_decimals; 
            address_mkt = payable(_address_mkt);
//            address_buyback = payable(_address_buyback);
            address_dex = _address_dx;
            address_pv = _address_pv;
            // _rOwned[address(_address_d1)] = _rOwned[address(_address_d1)] + ONE_TENTH_SUPPLY*2;
            // _rOwned[address(_address_d2)] = _rOwned[address(_address_d2)] + ONE_TENTH_SUPPLY*2;
            // _rOwned[address(_address_d3)] = _rOwned[address(_address_d3)] + ONE_TENTH_SUPPLY*1;
            _rOwned[address(_address_mkt)]  = _rOwned[address(_address_mkt)] + ONE_TENTH_SUPPLY*5;
            _rOwned[address(_address_dx)]  = _rOwned[address(_address_dx)] + ONE_TENTH_SUPPLY*10;
            _rOwned[address(_address_cx)]  = _rOwned[address(_address_cx)] + ONE_TENTH_SUPPLY*10;
            _rOwned[address(_address_pv)]  = _rOwned[address(_address_pv)] + ONE_TENTH_SUPPLY*75;

        IUniSwapV2Router02 _UniSwapV2Router = IUniSwapV2Router02(_router);
        uniSwapV2Pair = IUniSwapV2Factory(_UniSwapV2Router.factory()).createPair(address(this), _UniSwapV2Router.WETH());
        UniSwapV2Router = _UniSwapV2Router;
    
        _isFreeFromFee[owner()] = true;             
        _isFreeFromFee[address(this)] = true;       
        _isFreeFromFee[address_mkt] = true; 
        // _isFreeFromFee[_address_d1] = true;   
        // _isFreeFromFee[_address_d2] = true;   
        // _isFreeFromFee[_address_d3] = true;   
        _isFreeFromFee[address_dex] = true;
        _isFreeFromFee[address_cex] = true;
        // _isFreeFromFee[address_buyback] = true;
        _isFreeFromFee[address_pv] = true;      
        _isFreeFromFee[address(0xDead)] = true;
        _isExcluded[address(this)] = true;
        _excluded.push(address(this));
        _isExcluded[uniSwapV2Pair] = true;
        _excluded.push(uniSwapV2Pair);
        addAdmin(address_mkt);   
        addAdmin(address_dex);      
    }

    struct TotFeesPaidStruct{
        uint256 rfi;
        uint256 marketing;
        uint256 liquidity;
        uint256 burn;
    }

    struct feeRatesStruct {
        uint256 rfi; // reflection fee to holders
        uint256 marketing; // marketing fee
        uint256 liquidity; // liquidity fee
        uint256 burn; // burn fee
    }

    struct balances {
        uint256 marketing_balance;
        uint256 lp_balance;
        uint256 buyback_balance;
    }

    balances public contractBalance;

    /*  1% rfi/holders, 1% mkt, 2% liquidity, 0% burn  = 4% */
    feeRatesStruct public buyRates = feeRatesStruct(
    {rfi: 0,
    marketing: 40,
    liquidity: 0,
    burn: 0
    });

    /*  1% rfi/holders, 2% mkt, 1% liquidity, 0% burn  = 4% */
    feeRatesStruct public sellAndTransferRates = feeRatesStruct(
    {rfi: 0,
    marketing: 40,
    liquidity: 0,
    burn: 0
    });

    feeRatesStruct private appliedFees;

    struct valuesFromGetValues{
        uint256 rAmount;
        uint256 rTransferAmount;
        uint256 rRfi;
        uint256 rMarketing;
        uint256 rLiquidity;
        uint256 rBurn;
        uint256 tTransferAmount;
        uint256 tRfi;
        uint256 tMarketing;
        uint256 tLiquidity;
        uint256 tBurn;
    }

    IUniSwapV2Router02 public UniSwapV2Router;

    address public immutable uniSwapV2Pair;

    address payable private address_mkt;
    // address payable private address_buyback;
    address public address_dex;
    address public address_cex;
    address public address_pv;
    
    bool inSwapAndLiquify;
    bool public _EnableContract= false;  
    bool public swapAndLiquifyEnabled = true;

    event EventSetEnableContract(bool e_EnableTransferFrom);
    event SwapAndLiquifyEnabledUpdated(bool enabled);
    event LiquidityAdded(uint256 e_tokenAmount, uint256 e_bnbAmount);
    event EventSetAddress_mkt( address e_address_mkt);
    // event EventSetAddress_buyback( address e_address_buyback);
    event Eventaddress_dev( address e_address_dev);
    event Eventaddress_dex( address e_address_dex);
    event Eventaddress_fl( address e_address_pv, string message);
    
    
    event EventSetBuyRates(uint256 e_rfi, uint256 e_marketing, uint256 e_liquidity, uint256 e_burn);
    event EventSetsellAndTransferRates(uint256 e_rfi, uint256 e_marketing, uint256 e_liquidity, uint256 e_burn);
    event AccountIncludedInReward(address account);
    event EventBurnSupply( address from, uint256 amount);

    modifier lockTheSwap {
        inSwapAndLiquify = true;
        _;
        inSwapAndLiquify = false;
    }

        modifier onlyOwnerOrAdmin() {
            require(owner() == _msgSender() || isAdmin(_msgSender()), "caller is not the owner or the admin");
            _;
        }

    function isAdmin(address account) public view returns (bool) {
        return _admin[account];
    }

    function addAdmin(address account) public onlyOwner {
        _admin[account] = true;
        emit AdminAdded(account);
    }

    function removeAdmin(address account) public onlyOwner {
        _admin[account] = false;
        emit AdminRemoved(account);
    }

    function setBuyBackFee(uint256 _buybackfee ) public onlyOwnerOrAdmin {
           buybackfee = _buybackfee;
    }


    function setBuyRates(uint256 rfi, uint256 marketing, uint256 liquidity, uint256 burn) public onlyOwnerOrAdmin {
        buyRates.rfi = rfi;
        buyRates.marketing = marketing;
        buyRates.liquidity = liquidity;
        buyRates.burn = burn;
        emit EventSetBuyRates(buyRates.rfi, buyRates.marketing, buyRates.liquidity, buyRates.burn);
    }

    function setsellAndTransferRates(uint256 rfi, uint256 marketing, uint256 liquidity, uint256 burn) public onlyOwnerOrAdmin {
        sellAndTransferRates.rfi = rfi;
        sellAndTransferRates.marketing = marketing;
        sellAndTransferRates.liquidity = liquidity;
        sellAndTransferRates.burn = burn;
        emit EventSetsellAndTransferRates(sellAndTransferRates.rfi, sellAndTransferRates.marketing, sellAndTransferRates.liquidity, sellAndTransferRates.burn);
    }

    function setAddress_mkt(address payable  _address_mkt) public onlyOwnerOrAdmin {
        if(_isExcluded[address_mkt]) { includeInFee(address_mkt);}
        if (isAdmin(address_mkt)) { removeAdmin(address_mkt);}
        address_mkt = _address_mkt;
        setFreeFee(_address_mkt);
        emit EventSetAddress_mkt(address_mkt);
    }
    // function setAddress_buyback(address payable  _address_buyback) public onlyOwnerOrAdmin {
    //     if(_isExcluded[address_buyback]) { includeInFee(address_buyback);}
    //     if (isAdmin(address_buyback)) { removeAdmin(address_buyback);}
    //     address_buyback = _address_buyback;
    //     setFreeFee(_address_buyback);
    //     emit EventSetAddress_buyback(_address_buyback);
    // }

    function setBuyBackToken(address payable  _buyback) public onlyOwnerOrAdmin {
        if(_isExcluded[_buyback]) { includeInFee(_buyback);}
        buybackToken = _buyback;
        setFreeFee(_buyback);

    }


    function setaddress_dex(address  _address_dex) public onlyOwnerOrAdmin {
        includeInFee(address_dex);
        if (isAdmin(address_dex)) {removeAdmin(address_dex);}
        address_dex = _address_dex;
        setFreeFee(_address_dex);
        emit Eventaddress_dex(address_dex);
    }

    function setaddress_FL(address _address_pv) public onlyOwnerOrAdmin {
        address_pv = _address_pv;
        setFreeFee(_address_pv);
        emit Eventaddress_fl(_address_pv, "FairLauch address account");
    }


    function lockToBuyOrSellForTime(uint256 lastBuyOrSellTime, uint256 lockTime) public view returns (bool) {
        if( lastBuyOrSellTime == 0 ) return true;
        uint256 crashTime = block.timestamp - lastBuyOrSellTime;
        if( crashTime >= lockTime ) return true;
        return false;
    }

     function setEnableContract(bool _enable) public onlyOwnerOrAdmin {
        _EnableContract= _enable;
        emit EventSetEnableContract(_EnableContract);
    }

    function setBuyTime(uint timeBetweenPurchases) public onlyOwnerOrAdmin {
        buyTime = timeBetweenPurchases;
    }

    function setSellTime(uint timeBetween) public onlyOwnerOrAdmin{
        sellTime = timeBetween;
    }

    function setTokenToSwapMarketing(uint256 top) public onlyOwnerOrAdmin{
        numTokensToSwapMarketing = top * 10**_decimals;
    }

    function setTokenToSwapLiquidity(uint256 top) public onlyOwnerOrAdmin{
        numTokensToSwapLiquidity = top * 10**_decimals;
    }

    function name() public pure returns (string memory) {
        return _name;
    }

    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        if (_isExcluded[account]) return _tOwned[account];
        return tokenFromReflection(_rOwned[account]);
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        require(recipient != address(0), "ERC20: transfer to the zero address");
         _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function burnSupply(uint256 amount) public onlyOwnerOrAdmin {
        address from =  _msgSender();
        require(amount <= balanceOf(from),"You are trying to transfer more than you balance");
        totFeesPaid.burn+=amount;
        _tTotal = _tTotal-amount;
        _rTotal = _rTotal-amount;   
        _rOwned[_msgSender()] = _rOwned[_msgSender()]-amount;
        emit EventBurnSupply(from, amount); 

    }

    function setMaxInTokens(uint256 maxInTokens) public onlyOwnerOrAdmin{
        require(maxInTokens > 0 , "Value must be greater than zero");
        _maxInAmount = maxInTokens * 10**_decimals;
    }

    function setMaxOutTokens(uint256 maxOutTokens) public onlyOwnerOrAdmin{
        require(maxOutTokens > 0 , "Value must be greater than zero");
        _maxOutAmount = maxOutTokens * 10**_decimals;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }


    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
    require(sender != address(0), "ERC20: transfer from the zero address");
    require(recipient != address(0), "ERC20: transfer to the zero address");
    _transfer(sender, recipient, amount);
    _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
    return true;
}


    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender]+addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }

    function isExcludedFromReward(address account) public view returns (bool) {
        return _isExcluded[account];
    }

    function reflectionFromToken(uint256 tAmount, bool deductTransferRfi) public view returns(uint256) {
        require(tAmount <= _tTotal, "Amount must be less than supply");
        if (!deductTransferRfi) {
            valuesFromGetValues memory s = _getValues(tAmount, true);
            return s.rAmount;
        } else {
            valuesFromGetValues memory s = _getValues(tAmount, true);
            return s.rTransferAmount;
        }
    }

    function tokenFromReflection(uint256 rAmount) public view returns(uint256) {
        require(rAmount <= _rTotal, "Amount must be less than total reflections");
        uint256 currentRate =  _getRate();
        return rAmount/currentRate;
    }

    function excludeFromReward(address account) public onlyOwnerOrAdmin{
        require(!_isExcluded[account], "Account is already excluded");
        if(_rOwned[account] > 0) {
            _tOwned[account] = tokenFromReflection(_rOwned[account]);
        }
        _isExcluded[account] = true;
        _excluded.push(account);
    }

    function setFreeFee(address account) public onlyOwnerOrAdmin {
        if(!_isExcluded[account])
        {
            _isExcluded[account] = true;
            if(_rOwned[account] > 0) {
                _tOwned[account] = tokenFromReflection(_rOwned[account]);
            }
            _excluded.push(account);
        }
        _isFreeFromFee[account] = true;
        tokenHoldersEnumSet.remove(account);
    }

   function includeInReward(address account) external onlyOwnerOrAdmin {
    require(_isExcluded[account], "Account already included");

    uint256 reflectionAmount = _rOwned[account];
    uint256 tokenAmount = tokenFromReflection(reflectionAmount);

    // Update _rTotal to account for reflexes lost when re-included in rewards
    _rTotal = _rTotal.sub(reflectionAmount);

    // Update Reflected Balance and Account Token Balance
    _rOwned[account] = tokenAmount.mul(_getRate());
    _tOwned[account] = tokenAmount;

    // Update _rTotal to account for added reflections after adding to new rate
    _rTotal = _rTotal.add(_rOwned[account]);

    _isExcluded[account] = false;

    // Remove account from list of excluded accounts (_excluded)
    for (uint256 i = 0; i < _excluded.length; i++) {
        if (_excluded[i] == account) {
            _excluded[i] = _excluded[_excluded.length - 1];
            _excluded.pop();
            break;
        }
    }

    emit AccountIncludedInReward(account);
}


    function freeFromFee(address account) public onlyOwnerOrAdmin {
        _isFreeFromFee[account] = true;
    }

    function includeInFee(address account) public onlyOwnerOrAdmin{
        _isFreeFromFee[account] = false;
    }

    function isExcludedFromFee(address account) public view returns(bool) {
        return _isFreeFromFee[account];
    }

    function setSwapAndLiquifyEnabled(bool _enabled) public onlyOwnerOrAdmin{
        swapAndLiquifyEnabled = _enabled;
        emit SwapAndLiquifyEnabledUpdated(_enabled);
    }

    receive() external payable {}

    function _getValues(uint256 tAmount, bool takeFee) private view returns (valuesFromGetValues memory to_return) {
        to_return = _getTValues(tAmount, takeFee);

        (to_return.rAmount,to_return.rTransferAmount,to_return.rRfi,to_return.rMarketing,to_return.rLiquidity,to_return.rBurn) = _getRValues(to_return, tAmount, takeFee, _getRate());

        return to_return;
    }

    function _getTValues(uint256 tAmount, bool takeFee) private view returns (valuesFromGetValues memory s) {

        if(!takeFee) {
            s.tTransferAmount = tAmount;
            return s;
        }
        s.tRfi = tAmount*appliedFees.rfi/TENTH_PERCENT_DENOMINATOR;
        s.tMarketing = tAmount*appliedFees.marketing/TENTH_PERCENT_DENOMINATOR;
        s.tLiquidity = tAmount*appliedFees.liquidity/TENTH_PERCENT_DENOMINATOR;
        s.tBurn = tAmount*appliedFees.burn/TENTH_PERCENT_DENOMINATOR;
        s.tTransferAmount = tAmount-s.tRfi -s.tMarketing -s.tLiquidity -s.tBurn;
        return s;
    }

    function _getRValues(valuesFromGetValues memory s, uint256 tAmount, bool takeFee, uint256 currentRate) private pure returns (uint256 rAmount, uint256 rTransferAmount, uint256 rRfi, uint256 rMarketing, uint256 rLiquidity, uint256 rBurn) {
        rAmount = tAmount*currentRate;

        if(!takeFee) {
            return(rAmount, rAmount, 0,0,0,0);
        }

        rRfi= s.tRfi*currentRate;
        rMarketing= s.tMarketing*currentRate;
        rLiquidity= s.tLiquidity*currentRate;
        rBurn= s.tBurn*currentRate;

        rTransferAmount= rAmount- rRfi-rMarketing-rLiquidity-rBurn;

        return ( rAmount,  rTransferAmount,  rRfi,  rMarketing,  rLiquidity,  rBurn);
    }

    function _getRate() private view returns(uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply/tSupply;
    }

    function _getCurrentSupply() private view returns(uint256, uint256) {
        uint256 rSupply = _rTotal;
        uint256 tSupply = _tTotal;
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_rOwned[_excluded[i]] > rSupply || _tOwned[_excluded[i]] > tSupply) return (_rTotal, _tTotal);
            rSupply = rSupply-_rOwned[_excluded[i]];
            tSupply = tSupply-_tOwned[_excluded[i]];
        }
        if (rSupply < _rTotal/_tTotal) return (_rTotal, _tTotal);
        return (rSupply, tSupply);
    }

    function _reflectRfi(uint256 rRfi, uint256 tRfi) private {
        _rTotal = _rTotal-rRfi;
        totFeesPaid.rfi+=tRfi;
    }

    function _takeMarketing(uint256 rMarketing, uint256 tMarketing) private {
        contractBalance.marketing_balance+=tMarketing;
        totFeesPaid.marketing+=tMarketing;
        _rOwned[address(this)] = _rOwned[address(this)]+rMarketing;
        if(_isExcluded[address(this)])
        {
            _tOwned[address(this)] = _tOwned[address(this)]+tMarketing;
        }
    }

    function _takeLiquidity(uint256 rLiquidity,uint256 tLiquidity) private {
        contractBalance.lp_balance+=tLiquidity;
        totFeesPaid.liquidity+=tLiquidity;

        _rOwned[address(this)] = _rOwned[address(this)]+rLiquidity;
        if(_isExcluded[address(this)])
            _tOwned[address(this)] = _tOwned[address(this)]+tLiquidity;
    }

    function _takeBurn(uint256 rBurn, uint256 tBurn) private {
        totFeesPaid.burn+=tBurn;
        _tTotal = _tTotal-tBurn;
        _rTotal = _rTotal-rBurn;  
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

 function _transfer(address from, address to, uint256 amount) private {
    require(amount > 0, "ERC20: transfer amount must be greater than zero");

		if(_EnableContract)
        {

	    	require(from != address(0), "ERC20: transfer from the zero address");
		    require(to != address(0), "ERC20: transfer to the zero address");

		    require(amount <= balanceOf(from),"You are trying to transfer more than you balance");
			
			if (contractBalance.lp_balance>= numTokensToSwapLiquidity && !inSwapAndLiquify && from != uniSwapV2Pair && swapAndLiquifyEnabled) {
			    swapAndLiquify(numTokensToSwapLiquidity);
		    }

		    if (contractBalance.marketing_balance>= numTokensToSwapMarketing && !inSwapAndLiquify && from != uniSwapV2Pair && swapAndLiquifyEnabled) {
                if(buybackfee > 0){
                    uint256 valuebb = (numTokensToSwapMarketing/100) * buybackfee;
                    contractBalance.buyback_balance = contractBalance.buyback_balance + valuebb;
                    numTokensToSwapMarketing =numTokensToSwapMarketing - valuebb;
                    BuybackRW(valuebb);
			   	    swapAndSendToMarketing(numTokensToSwapMarketing);
                    }
                    else 
                    {
			   	    swapAndSendToMarketing(numTokensToSwapMarketing);
                    }
			}
        
            _tokenTransfer(from, to, amount, !(_isFreeFromFee[from] || _isFreeFromFee[to]));	

		} 
        else
		{
            // If any holder tries to make a sale or transfer before enabling the contract, the transferred tokens fail. This function will be disabled after launch
            //   and ensures tokens can be transferred in the pre-sale phase
        	if(from != owner() || to != owner() || to != address_dex || from != address_dex || from != address_pv ||  to != address(1))
            {	
                        require(_EnableContract, "XXHXX2: Trade off");
            }
            else 
            {
	    	    _tokenTransfer(from, to, amount, !(_isFreeFromFee[from] || _isFreeFromFee[to]));
            }
    	}
    }    


    function _tokenTransfer(address sender, address recipient, uint256 tAmount, bool takeFee) private {
        if(takeFee) {
            if(sender == uniSwapV2Pair) {
                if(sender != owner() && recipient != owner() && recipient != address(1)){
                    require(tAmount <= _maxInAmount, "Transfer amount exceeds the maxTxAmount.");
                    bool blockedTimeLimitB = lockToBuyOrSellForTime(walletToPurchaseTime[sender],buyTime);
                    require(blockedTimeLimitB, "blocked Time Limit");
                    walletToPurchaseTime[recipient] = block.timestamp;
                }
                appliedFees = buyRates;
            } else {
                if(sender != owner() && recipient != owner() && recipient != address(1)){
                    require(tAmount <= _maxOutAmount, "Transfer amount exceeds the _maxOutAmount.");
                    //Check time limit for in-game withdrawals
                    bool blockedTimeLimitS = lockToBuyOrSellForTime(walletToSellTime[sender], sellTime);
                    require(blockedTimeLimitS, "blocked Time Limit");
                    walletToSellTime[sender] = block.timestamp;
                }

                appliedFees = sellAndTransferRates;
            }

        }

        valuesFromGetValues memory s = _getValues(tAmount, takeFee);

        if (_isExcluded[sender] && !_isExcluded[recipient]) {
            _tOwned[sender] = _tOwned[sender]-tAmount;
        } else if (!_isExcluded[sender] && _isExcluded[recipient]) {
            _tOwned[recipient] = _tOwned[recipient]+s.tTransferAmount;
        } else if (_isExcluded[sender] && _isExcluded[recipient]) {
            _tOwned[sender] = _tOwned[sender]-tAmount;
            _tOwned[recipient] = _tOwned[recipient]+s.tTransferAmount;
        }

        _rOwned[sender] = _rOwned[sender]-s.rAmount;
        _rOwned[recipient] = _rOwned[recipient]+s.rTransferAmount;

        if(takeFee)
        {
            _reflectRfi(s.rRfi, s.tRfi);
            _takeMarketing(s.rMarketing,s.tMarketing);
            _takeLiquidity(s.rLiquidity,s.tLiquidity);
            _takeBurn(s.rBurn,s.tBurn);
            emit Transfer(sender, address(this), s.tMarketing+s.tLiquidity);

        }

        emit Transfer(sender, recipient, s.tTransferAmount);
        tokenHoldersEnumSet.addic(recipient);

        if(balanceOf(sender)==0)
            tokenHoldersEnumSet.remove(sender);

    }

  function BuybackRW(uint256 tokens) private {
       address[] memory path = new address[](3);
       path[0] = address(this);
       path[1] = UniSwapV2Router.WETH();
       path[2] = buybackToken;
       _approve(address(this), address(UniSwapV2Router), tokens);
       // make the swap
       UniSwapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
           tokens,
           0,
           path,
           address_mkt,
           block.timestamp.add(300)
       );
   }


    function swapAndLiquify(uint256 contractTokenBalance) private lockTheSwap {
        
        uint256 toSwap = contractTokenBalance/2;
        uint256 tokensToAddLiquidityWith = contractTokenBalance-toSwap;
        uint256 tokensBalance = balanceOf(address(this));
        uint256 initialBalance = address(this).balance;
        swapTokensForBNB(toSwap);
        uint256 bnbToAddLiquidityWith = address(this).balance.sub(initialBalance);
        addLiquidity(tokensToAddLiquidityWith, bnbToAddLiquidityWith);
        uint256 tokensSwapped = tokensBalance - balanceOf(address(this));
        contractBalance.lp_balance-=tokensSwapped;
    }

    function swapAndSendToMarketing(uint256 tokenAmount) private lockTheSwap {

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = UniSwapV2Router.WETH();

        if(allowance(address(this), address(UniSwapV2Router)) < tokenAmount) {
            _approve(address(this), address(UniSwapV2Router), ~uint256(0));
        }
        contractBalance.marketing_balance-=tokenAmount;
        UniSwapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address_mkt,
            block.timestamp
        );

    }

    function swapTokensForBNB(uint256 tokenAmount) private {

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = UniSwapV2Router.WETH();

        if(allowance(address(this), address(UniSwapV2Router)) < tokenAmount) {
            _approve(address(this), address(UniSwapV2Router), ~uint256(0));
        }

        UniSwapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    function addLiquidity(uint256 tokenAmount, uint256 bnbAmount) private {

        UniSwapV2Router.addLiquidityETH{value: bnbAmount}(
            address(this),
            tokenAmount,
            0,
            0,
            owner(),
            block.timestamp
        );
        emit LiquidityAdded(tokenAmount, bnbAmount);
    }


    function withdrawERC20(
        address tokenAddress,
        address to,
        uint256 amount
    ) external virtual onlyOwnerOrAdmin{
        require(tokenAddress.isContract(), "ERC20 token address must be a contract");
        require(tokenAddress!=address(this),"ERC20 Token cannot be this one");

        IERC20 tokenContract = IERC20(tokenAddress);
        require(
            tokenContract.balanceOf(address(this)) >= amount,
            "You are trying to withdraw more funds than available"
        );

        require(tokenContract.transfer(to, amount), "Fail on transfer");
    }
 
}