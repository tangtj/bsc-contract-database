// File: interfaces/IInsurance.sol


pragma solidity >=0.8.0;
 
interface IInsurance {
     function addAmountToPool(uint256[] memory amounts, address[] memory tos) external;
}

// File: TOKEN_NEW.sol

/**
 *Submitted for verification at BscScan.com on 2022-02-16
 */


pragma solidity ^0.8.6;


interface IERC20 {
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
    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

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
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

abstract contract Ownable {
    address private _owner;
    address private _previousOwner;
    uint256 private _lockTime;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        address msgSender = msg.sender;
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
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
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
    event PairCreated(
        address indexed token0,
        address indexed token1,
        address pair,
        uint256
    );

    function feeTo() external view returns (address);

    function feeToSetter() external view returns (address);

    function getPair(
        address tokenA,
        address tokenB
    ) external view returns (address pair);

    function allPairs(uint256) external view returns (address pair);

    function allPairsLength() external view returns (uint256);

    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);

    function setFeeTo(address) external;

    function setFeeToSetter(address) external;
}

interface IUniswapV2Pair {
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Transfer(address indexed from, address indexed to, uint256 value);

    function name() external pure returns (string memory);

    function symbol() external pure returns (string memory);

    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);

    function PERMIT_TYPEHASH() external pure returns (bytes32);

    function nonces(address owner) external view returns (uint256);

    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;

    event Mint(address indexed sender, uint256 amount0, uint256 amount1);
    event Burn(
        address indexed sender,
        uint256 amount0,
        uint256 amount1,
        address indexed to
    );
    event Swap(
        address indexed sender,
        uint256 amount0In,
        uint256 amount1In,
        uint256 amount0Out,
        uint256 amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint256);

    function factory() external view returns (address);

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves()
        external
        view
        returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function price0CumulativeLast() external view returns (uint256);

    function price1CumulativeLast() external view returns (uint256);

    function kLast() external view returns (uint256);

    function mint(address to) external returns (uint256 liquidity);

    function burn(
        address to
    ) external returns (uint256 amount0, uint256 amount1);

    function swap(
        uint256 amount0Out,
        uint256 amount1Out,
        address to,
        bytes calldata data
    ) external;

    function skim(address to) external;

    function sync() external;

    function initialize(address, address) external;
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
    ) external returns (uint256 amountA, uint256 amountB, uint256 liquidity);

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);

    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB);

    function removeLiquidityETH(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountToken, uint256 amountETH);

    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountA, uint256 amountB);

    function removeLiquidityETHWithPermit(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountToken, uint256 amountETH);

    function swapExactTokensForTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapTokensForExactTokens(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactETHForTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);

    function swapTokensForExactETH(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactTokensForETH(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapETHForExactTokens(
        uint256 amountOut,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);

    function quote(
        uint256 amountA,
        uint256 reserveA,
        uint256 reserveB
    ) external pure returns (uint256 amountB);

    function getAmountOut(
        uint256 amountIn,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountOut);

    function getAmountIn(
        uint256 amountOut,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountIn);

    function getAmountsOut(
        uint256 amountIn,
        address[] calldata path
    ) external view returns (uint256[] memory amounts);

    function getAmountsIn(
        uint256 amountOut,
        address[] calldata path
    ) external view returns (uint256[] memory amounts);
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountETH);

    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

contract Token is IERC20, Ownable {
    using SafeMath for uint256;

    mapping(address => uint256) private _tOwned;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) isDividendExempt;
    mapping(address => bool) private _isExcludedFromFee;
    mapping(address => bool) private _isExcludedToFee;
    mapping(address => bool) private _updated;
    mapping(address => bool) public isMarketPair;

    uint256 private _allPercentage = 10000;
    uint256 private _tFeeTotal;
    string private _name = "King of Prophecy";
    string private _symbol = "KOP";
    uint8 private _decimals = 18;

    //销毁
    uint256 public _burnFee = 450;
    uint256 private _previousburnFee;

    //LP分红
    uint256 public _lpFee = 0;
    uint256 private _previousLpFee;

    //流动性矿池
    uint256 public _lpFeePool = 600;
    uint256 private _previousLpFeePool;

    uint256 public _cakeFee = 0;
    uint256 private _previousCakeFee;

    //销毁矿池
    uint256 public _destoryPoolFee = 100;
    uint256 private _previousDestoryPoolFee;

    //创世矿池
    uint256 public _genesisPoolFee = 100;
    uint256 private _previousGenesisPoolFee;

    //合伙人奖励
    uint256 public _partnerPoolFee = 25;
    uint256 private _previousPartnerPoolFee;

    //工会
    uint256 public _industrialPoolFee = 25;
    uint256 private _previousIndustrialPoolFee;

    //买方滑点

    //流动性矿池
    uint256 public _buyLpFeePool = 1523;
    uint256 private _buypreviousLpFeePool;

    //买方销毁矿池
    uint256 public _buyburnFee = 1142;
    uint256 private _buypreviousburnFee;

    //LP分红
    uint256 public _buylpFee = 0;
    uint256 private _buypreviousLpFee;

    uint256 public _buycakeFee = 0;
    uint256 private _buypreviousCakeFee;

    //销毁矿池
    uint256 public _buydestoryPoolFee = 253;
    uint256 private _buypreviousDestoryPoolFee;

    //创世矿池
    uint256 public _buygenesisPoolFee = 253;
    uint256 private _buypreviousGenesisPoolFee;

    //合伙人奖励
    uint256 public _buypartnerPoolFee = 63;
    uint256 private _buypreviousPartnerPoolFee;

    //工会
    uint256 public _buyindustrialPoolFee = 63;
    uint256 private _buypreviousIndustrialPoolFee;

    uint256 currentIndex;
    //总发现2.1亿
    uint256 private _tTotal = 21 * 10 ** 7 * 10 ** 18;
    uint256 distributorGas = 500000;
    uint256 public minPeriod = 1 hours;
    uint256 public lpFeeShareTime;

    IUniswapV2Router02 public uniswapV2Router;
    address public  uniswapV2Pair;
    address private fromAddress;
    address private toAddress;

    mapping(address => address) public inviter;
    address[] shareholders;
    mapping(address => uint256) shareholderIndexes;
    //销毁池合约
    address public destoryNFTPool;
    //创世合约
    address public genesisNFTPool;
    //工会合约
    address public industrialPool;
    //合伙人合约
    address public partnerPool;
    //流动性矿池合约
    address public lpPool;

    bool inSwapAndLiquify;
    IInsurance public insurance;
    modifier lockTheSwap() {
        inSwapAndLiquify = true;
        _;
        inSwapAndLiquify = false;
    }

    constructor() {
        _tOwned[msg.sender] = _tTotal;

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(
            0x10ED43C718714eb63d5aA57B78B54704E256024E
        );

        // Create a uniswap pair for this new token
        uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory())
            .createPair(address(this), address(0x55d398326f99059fF775485246999027B3197955));

        // set the rest of the contract variables
        uniswapV2Router = _uniswapV2Router;

        //exclude owner and this contract from fee
        _isExcludedFromFee[msg.sender] = true;
        _isExcludedFromFee[address(this)] = true;
        isDividendExempt[address(this)] = true;
        isDividendExempt[address(0)] = true;
        isMarketPair[uniswapV2Pair] = true;
        emit Transfer(address(0), msg.sender, _tTotal);
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

    function transfer(
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        address from = msg.sender;
        address to = recipient;
        if (!_isExcludedFromFee[from] && !_isExcludedToFee[to]) {
            _transfer(from, recipient, amount);
        } else {
            _tOwned[from] = _tOwned[from].sub(amount);
            _tOwned[to] = _tOwned[to].add(amount);
            emit Transfer(from, to, amount);
        }
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
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        if (!_isExcludedFromFee[sender] && !_isExcludedToFee[recipient]) {
            _transfer(sender, recipient, amount);
        } else {
            _tOwned[sender] = _tOwned[sender].sub(amount);
            _tOwned[recipient] = _tOwned[recipient].add(amount);
            emit Transfer(sender, recipient, amount);
        }
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

    function increaseAllowance(
        address spender,
        uint256 addedValue
    ) public virtual returns (bool) {
        _approve(
            msg.sender,
            spender,
            _allowances[msg.sender][spender].add(addedValue)
        );
        return true;
    }

    function decreaseAllowance(
        address spender,
        uint256 subtractedValue
    ) public virtual returns (bool) {
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

    function totalFees() public view returns (uint256) {
        return _tFeeTotal;
    }

    function isExcludedFromFee(address account) public view returns (bool) {
        return _isExcludedFromFee[account];
    }

    function updateExcludeFromFee(
        address account,
        bool status
    ) public onlyOwner {
        _isExcludedFromFee[account] = status;
    }

    function isExcludedToFee(address account) public view returns (bool) {
        return _isExcludedToFee[account];
    }

    function isDividendExemptF(address account) public view returns (bool) {
        return isDividendExempt[account];
    }

    function updateIsDividendExempt(
        address account,
        bool status
    ) public onlyOwner {
        isDividendExempt[account] = status;
    }

    function updateExcludeToFee(address account, bool status) public onlyOwner {
        _isExcludedToFee[account] = status;
    }

    function updateBurnFee(uint256 _updateBurnFee) public onlyOwner {
        _burnFee = _updateBurnFee;
    }

    function updatePartnerPoolFee(
        uint256 _updatePartnerPoolFee
    ) public onlyOwner {
        _partnerPoolFee = _updatePartnerPoolFee;
    }

    function updateLpFee(uint256 _updateLpFee) public onlyOwner {
        _lpFee = _updateLpFee;
    }

    function updateLpFeePool(uint256 _updateLpFeePool) public onlyOwner {
        _lpFeePool = _updateLpFeePool;
    }

    function updateDestoryPoolFee(
        uint256 _updateDestoryPoolFee
    ) public onlyOwner {
        _destoryPoolFee = _updateDestoryPoolFee;
    }

    function updateGenesisPoolFee(
        uint256 _updateGenesisPoolFee
    ) public onlyOwner {
        _genesisPoolFee = _updateGenesisPoolFee;
    }

    function updateIndustrialPoolFee(
        uint256 _updateIndustrialPoolFee
    ) public onlyOwner {
        _industrialPoolFee = _updateIndustrialPoolFee;
    }

    function buyupdateBurnFee(uint256 _updateBurnFee) public onlyOwner {
        _buyburnFee = _updateBurnFee;
    }

    function buyupdateLpFee(uint256 _updateLpFee) public onlyOwner {
        _buylpFee = _updateLpFee;
    }

    function buyupdateDestoryPoolFee(
        uint256 _updateDestoryPoolFee
    ) public onlyOwner {
        _buydestoryPoolFee = _updateDestoryPoolFee;
    }

    function buyupdateGenesisPoolFee(
        uint256 _updateGenesisPoolFee
    ) public onlyOwner {
        _buygenesisPoolFee = _updateGenesisPoolFee;
    }

    function buyupdateIndustrialPoolFee(
        uint256 _updateIndustrialPoolFee
    ) public onlyOwner {
        _buyindustrialPoolFee = _updateIndustrialPoolFee;
    }

    function buyupdatePartnerPoolFee(
        uint256 _updatePartnerPoolFee
    ) public onlyOwner {
        _buypartnerPoolFee = _updatePartnerPoolFee;
    }

    function buyupdateLpFeePool(uint256 _updateLpFeePool) public onlyOwner {
        _buyLpFeePool = _updateLpFeePool;
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

        //indicates if fee should be deducted from transfer
        bool takeFee = false;
        //默认0未买入
        uint8 transferType = 0;
        //if any account belongs to _isExcludedFromFee account then remove the fee
        if (from == uniswapV2Pair || to == address(uniswapV2Pair)) {
            takeFee = true;
            if (to == address(uniswapV2Pair)) {
                transferType = 1;
            }
        }

        if (takeFee) {
            //买入手续费
            if (transferType == 0) {
                _buyTokenTransfer(from, to, amount, takeFee);
            } else {
                _tokenTransfer(from, to, amount, takeFee);
            }
        } else {
            _tOwned[from] = _tOwned[from].sub(amount);
            _tOwned[to] = _tOwned[to].add(amount);
            emit Transfer(from, to, amount);
        }

        if(_lpFee!=0  && _buylpFee!=0){
            if (fromAddress == address(0)) fromAddress = from;
            if (toAddress == address(0)) toAddress = to;
            if (!isDividendExempt[fromAddress] && fromAddress != uniswapV2Pair)
               setShare(fromAddress);
            if (!isDividendExempt[toAddress] && toAddress != uniswapV2Pair)
               setShare(toAddress);

               fromAddress = from;
               toAddress = to;
            if (
               _tOwned[address(this)] >= 1 * 10 ** 4 * 10 ** 18 &&
               from != address(this) &&
               lpFeeShareTime.add(minPeriod) <= block.timestamp
             ) {
               process(distributorGas);
               lpFeeShareTime = block.timestamp;
             }
        }
    }

    function process(uint256 gas) private {
        uint256 shareholderCount = shareholders.length;

        if (shareholderCount == 0) return;
        uint256 nowbanance = _tOwned[address(this)];
        uint256 gasUsed = 0;
        uint256 gasLeft = gasleft();

        uint256 iterations = 0;

        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }

            uint256 amount = nowbanance
                .mul(
                    IERC20(uniswapV2Pair).balanceOf(shareholders[currentIndex])
                )
                .div(IERC20(uniswapV2Pair).totalSupply());
            if (amount < 1 * 10 ** 18) {
                currentIndex++;
                iterations++;
                return;
            }
            if (_tOwned[address(this)] < amount) return;
            distributeDividend(shareholders[currentIndex], amount);

            gasUsed = gasUsed.add(gasLeft.sub(gasleft()));
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }
    }

    function distributeDividend(address shareholder, uint256 amount) internal {
        _tOwned[address(this)] = _tOwned[address(this)].sub(amount);
        _tOwned[shareholder] = _tOwned[shareholder].add(amount);
        emit Transfer(address(this), shareholder, amount);
    }

    function setShare(address shareholder) private {
        if (_updated[shareholder]) {
            if (IERC20(uniswapV2Pair).balanceOf(shareholder) == 0)
                quitShare(shareholder);
            return;
        }
        if (IERC20(uniswapV2Pair).balanceOf(shareholder) == 0) return;
        addShareholder(shareholder);
        _updated[shareholder] = true;
    }

    function addShareholder(address shareholder) internal {
        shareholderIndexes[shareholder] = shareholders.length;
        shareholders.push(shareholder);
    }

    function quitShare(address shareholder) private {
        removeShareholder(shareholder);
        _updated[shareholder] = false;
    }

    function removeShareholder(address shareholder) internal {
        shareholders[shareholderIndexes[shareholder]] = shareholders[
            shareholders.length - 1
        ];
        shareholderIndexes[
            shareholders[shareholders.length - 1]
        ] = shareholderIndexes[shareholder];
        shareholders.pop();
    }

    //this method is responsible for taking all fee, if takeFee is true
    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 amount,
        bool takeFee
    ) private {
        _transferStandard(sender, recipient, amount);
    }

    function _buyTokenTransfer(
        address sender,
        address recipient,
        uint256 amount,
        bool takeFee
    ) private {
        _buyTransferStandard(sender, recipient, amount);
    }

    //
    function _takeburnFee(address sender, uint256 tAmount) private {
        if (_burnFee == 0) return;
        if (_tFeeTotal >= 999 * 10 ** 7 * 10 ** 9) _burnFee = 0;
        _tOwned[address(0)] = _tOwned[address(0)].add(tAmount);
        _tFeeTotal = _tFeeTotal.add(tAmount);
        emit Transfer(sender, address(0), tAmount);
    }

    function _takeLPFee(address sender, uint256 tAmount) private {
        if (_lpFee == 0 && _cakeFee == 0) return;
        _tOwned[address(this)] = _tOwned[address(this)].add(tAmount);
        emit Transfer(sender, address(this), tAmount);
    }

    function _buyTakeLPFee(address sender, uint256 tAmount) private {
        if (_buylpFee == 0 && _buycakeFee == 0) return;
        _tOwned[address(this)] = _tOwned[address(this)].add(tAmount);
        emit Transfer(sender, address(this), tAmount);
    }

    function _transferStandard(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        _tOwned[sender] = _tOwned[sender].sub(tAmount);
        _takeLPFee(
            sender,
            tAmount.div(_allPercentage).mul(_lpFee.add(_cakeFee))
        );

        //销毁
        uint256 _burnAmount = tAmount.div(_allPercentage).mul(_burnFee);
        _tOwned[address(0x000000000000000000000000000000000000dEaD)] = _tOwned[
            address(0x000000000000000000000000000000000000dEaD)
        ].add(_burnAmount);

        emit Transfer(
            sender,
            address(0x000000000000000000000000000000000000dEaD),
            _burnAmount
        );
        
        //销毁矿池添加余额
        uint256 _destoryNFTPoolAmount = tAmount.div(_allPercentage).mul(
            _destoryPoolFee
        );
        
        _tOwned[address(destoryNFTPool)] = _tOwned[address(destoryNFTPool)].add(
            _destoryNFTPoolAmount
        );

        emit Transfer(sender, address(destoryNFTPool), _destoryNFTPoolAmount);

       //创世矿池添加余额
        uint256 _genesisPoolAmount = tAmount.div(_allPercentage).mul(
            _genesisPoolFee
        );
        
        _tOwned[address(genesisNFTPool)] = _tOwned[address(genesisNFTPool)].add(
            _genesisPoolAmount
        );

        emit Transfer(sender, address(genesisNFTPool), _genesisPoolAmount);

        //流动性矿池添加
        uint256 _lpFeePoolAmount = tAmount.div(_allPercentage).mul(
            _lpFeePool
        );
        
        _tOwned[address(lpPool)] = _tOwned[address(lpPool)].add(
            _lpFeePoolAmount
        );


        emit Transfer(sender, address(lpPool), _lpFeePoolAmount);

        //工会矿池添加
        uint256 _industrialPoolAmount = tAmount.div(_allPercentage).mul(
            _industrialPoolFee
        );
        
        _tOwned[address(industrialPool)] = _tOwned[address(industrialPool)].add(
            _industrialPoolAmount
        );


        emit Transfer(sender, address(industrialPool), _industrialPoolAmount);

        //合伙人矿池添加
        uint256 _partnerPoolAmount = tAmount.div(_allPercentage).mul(
            _partnerPoolFee
        );
        
        _tOwned[address(partnerPool)] = _tOwned[address(partnerPool)].add(
            _partnerPoolAmount
        );

        emit Transfer(sender, address(partnerPool), _partnerPoolAmount);

        uint256 recipientRate = _allPercentage -
            _lpFee -
            _cakeFee -
            _burnFee -
            _genesisPoolFee -
            _destoryPoolFee -
            _lpFeePool -
            _partnerPoolFee -
            _industrialPoolFee;
        _tOwned[recipient] = _tOwned[recipient].add(
            tAmount.div(_allPercentage).mul(recipientRate)
        );
        emit Transfer(
            sender,
            recipient,
            tAmount.div(_allPercentage).mul(recipientRate)
        );
    }

    function _buyTransferStandard(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        _tOwned[sender] = _tOwned[sender].sub(tAmount);
        _buyTakeLPFee(
            sender,
            tAmount.div(_allPercentage).mul(_buylpFee.add(_buycakeFee))
        );

        uint256 _burnAmount = tAmount.div(_allPercentage).mul(_buyburnFee);
        _tOwned[address(0x000000000000000000000000000000000000dEaD)] = _tOwned[
            address(0x000000000000000000000000000000000000dEaD)
        ].add(_burnAmount);
        emit Transfer(
            sender,
            address(0x000000000000000000000000000000000000dEaD),
            _burnAmount
        );

        
        //销毁矿池添加余额
        uint256 _destoryNFTPoolAmount = tAmount.div(_allPercentage).mul(
            _buydestoryPoolFee
        );
        
        _tOwned[address(destoryNFTPool)] = _tOwned[address(destoryNFTPool)].add(
            _destoryNFTPoolAmount
        );

        emit Transfer(sender, address(destoryNFTPool), _destoryNFTPoolAmount);

       //创世矿池添加余额
        uint256 _genesisPoolAmount = tAmount.div(_allPercentage).mul(
            _buygenesisPoolFee
        );
        
        _tOwned[address(genesisNFTPool)] = _tOwned[address(genesisNFTPool)].add(
            _genesisPoolAmount
        );

        emit Transfer(sender, address(genesisNFTPool), _genesisPoolAmount);

        //流动性矿池添加
        uint256 _lpFeePoolAmount = tAmount.div(_allPercentage).mul(
            _buyLpFeePool
        );
        
        _tOwned[address(lpPool)] = _tOwned[address(lpPool)].add(
            _lpFeePoolAmount
        );


        emit Transfer(sender, address(lpPool), _lpFeePoolAmount);

        //工会矿池添加
        uint256 _industrialPoolAmount = tAmount.div(_allPercentage).mul(
            _buyindustrialPoolFee
        );
        
        _tOwned[address(industrialPool)] = _tOwned[address(industrialPool)].add(
            _industrialPoolAmount
        );


        emit Transfer(sender, address(industrialPool), _industrialPoolAmount);

        //合伙人矿池添加
        uint256 _partnerPoolAmount = tAmount.div(_allPercentage).mul(
            _buypartnerPoolFee
        );
        
        _tOwned[address(partnerPool)] = _tOwned[address(partnerPool)].add(
            _partnerPoolAmount
        );

        emit Transfer(sender, address(partnerPool), _partnerPoolAmount);

        uint256 recipientRate = _allPercentage -
            _buylpFee -
            _buycakeFee -
            _buyburnFee -
            _buygenesisPoolFee -
            _buydestoryPoolFee -
            _buyLpFeePool -
            _buypartnerPoolFee -
            _buyindustrialPoolFee;
        _tOwned[recipient] = _tOwned[recipient].add(
            tAmount.div(_allPercentage).mul(recipientRate)
        );

        emit Transfer(
            sender,
            recipient,
            tAmount.div(_allPercentage).mul(recipientRate)
        );
    }

    function setMarketPair(address pair, bool hasPair) external onlyOwner {
        isMarketPair[pair] = hasPair;
    }

    function setDestoryNFTPool(address _destoryNFTPool) external onlyOwner {
        destoryNFTPool = _destoryNFTPool;
    }

    function setGenesisNFTPool(address _genesisNFTPool) external onlyOwner {
        genesisNFTPool = _genesisNFTPool;
    }

    function setIndustrialPool(address _industrialPool) external onlyOwner {
        industrialPool = _industrialPool;
    }

    function setLpPool(address _lpPool) external onlyOwner {
        lpPool = _lpPool;
    }

    function setPartnerPool(address _partnerPool) external onlyOwner {
        partnerPool = _partnerPool;
    }
}