/*
 _____  _____  _____  _____  __  __ 
|  _  \/  _  \/   __\/   __\/  \/  \
|  |  ||  |  ||  |_ ||   __||  \/  |
|_____/\_____/\_____/\_____/\__ \__/
                                    

   total suppy : 1 Quadrillion

9% Buying Fee
   4% sent to marketing wallet
   1% Supplied to game prize wallet
   1% reflect to all holders
   3% add liquidity

14% Selling fee
   6% sent to marketing wallet
   2% Supplied to game prize wallet
   1% reflect to all holders
   5% add liquidity

12% Dungeon Fee
   5% sent to marketing wallet
   3% Supplied to game prize wallet
   1% reflect to all holders
   3% add liquidity        
*/   

// SPDX-License-Identifier: Unlicensed

pragma solidity ^0.6.12;

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

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

contract Ownable is Context {
    address private _owner;    

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor () internal {
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
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
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
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }    
}

interface IUniswapV2Factory {
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

interface IUniswapV2Pair {
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

interface IUniswapV2Router01 {
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

interface IUniswapV2Router02 is IUniswapV2Router01 {
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

interface IGameContract {
    function payEntrance (address addr, uint256 amount, uint256 level) external;
}

contract DOGEM is Context, IERC20, Ownable {
    using SafeMath for uint256;

    mapping (address => uint256) private _rOwned;
    mapping (address => uint256) private _tOwned;
    mapping (address => mapping (address => uint256)) private _allowances;

    mapping (address => bool) private _isExcludedFromFee;
    mapping (address => bool) private _isExcluded;
    address[] private _excluded;
      
    uint256 private constant MAX = ~uint256(0);
    uint256 private _tTotal = 1 * 10**15 * 10**9;
    uint256 private _rTotal = (MAX - (MAX % _tTotal));

    string private _name = "DOGEM";
    string private _symbol = "DND";
    uint8 private _decimals = 9;
    
    uint256 public marketingFeeOnBuy = 4;
    uint256 public gameFeeOnBuy = 1;    
    uint256 public reflectionFeeOnBuy = 1;
    uint256 public liquidityFeeOnBuy = 3;

    uint256 public marketingFeeOnSell = 6;
    uint256 public gameFeeOnSell = 2;    
    uint256 public reflectionFeeOnSell = 1;
    uint256 public liquidityFeeOnSell = 5;

    uint256 public marketingFeeOnDungeon = 5;
    uint256 public gameFeeOnDungeon = 3;    
    uint256 public reflectionFeeOnDungeon = 1;
    uint256 public liquidityFeeOnDungeon = 3;

    uint256 marketingFee;
    uint256 gameFee;
    uint256 reflectionFee;
    uint256 liquidityFee;

    uint256 public marketingAccumulated = 0;    
    uint256 public liquidityAccumulated = 0;
    
    address public marketingWallet = 0x7a4D4ab88003f326110bD6F51D8d9c0bf9b3EAf8;
    address public gameWallet = 0x0000000000000000000000000000000000000001;    
    IGameContract gameContract = IGameContract(gameWallet);
    address public DEAD = 0x000000000000000000000000000000000000dEaD;
    
    IUniswapV2Router02 public immutable uniswapV2Router;
    address public immutable uniswapV2Pair;
    
    bool inSwapAndLiquify;
    bool public swapAndLiquifyEnabled = true;

    mapping (address=>bool) public blacklist;
    uint256 public tradingOpenBlock;
    uint256 public tradingOpenTimeStamp;
    
    uint256 public maxTxAmount = _tTotal.div(1000).mul(3);
    uint256 public maxWalletAmount = _tTotal.div(100).mul(3);
    uint256 public numTokensSellToAddToLiquidity = _tTotal.div(10000);
    
    event MinTokensBeforeSwapUpdated(uint256 minTokensBeforeSwap);
    event SwapAndLiquifyEnabledUpdated(bool enabled);
    event SwapAndLiquify(uint256 tokensSwapped, uint256 ethReceived, uint256 tokensIntoLiqudity);
    event ExcludeFromReward(address account);
    event IncludeInReward(address account);
    event ExcludeFromFee(address account);
    event IncludeInFee(address account);    
    event MaxTransactionAmountChanged(uint256 amount);
    event MaxHoldAmountChanged(uint256 amount);
    event WalletAddressChanged(address _gameWallet, address _marketingWallet);    
    event RemoveFromBlacklist(address addr);
    
    modifier lockTheSwap {
        inSwapAndLiquify = true;
        _;
        inSwapAndLiquify = false;
    }
    
    constructor () public {
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
         // Create a uniswap pair for this new token
        uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory())
            .createPair(address(this), _uniswapV2Router.WETH());
        // set the rest of the contract variables
        uniswapV2Router = _uniswapV2Router;
        
        //exclude owner and this contract from fee
        _isExcludedFromFee[msg.sender] = true;
        _isExcludedFromFee[address(this)] = true;        
        
        _isExcluded[DEAD] = true;
        _excluded.push(DEAD);
        
        _rOwned[msg.sender] = _rTotal;        
        
        emit Transfer(address(0), msg.sender, _tTotal);       
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
        if (_isExcluded[account]) return _tOwned[account];
        return tokenFromReflection(_rOwned[account]);
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }

    function isExcludedFromReward(address account) public view returns (bool) {
        return _isExcluded[account];
    }

    function reflectionFromToken(uint256 tAmount) public view returns(uint256) {
       uint256 currentRate =  _getRate();
        return tAmount.mul(currentRate);
    }

    function tokenFromReflection(uint256 rAmount) public view returns(uint256) {
        require(rAmount <= _rTotal, "Amount must be less than total reflections");
        uint256 currentRate =  _getRate();
        return rAmount.div(currentRate);
    }

    function excludeFromReward(address account) public onlyOwner() {
        // require(account != 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D, 'We can not exclude Uniswap router.');
        require(!_isExcluded[account], "Account is already excluded");
        if(_rOwned[account] > 0) {
            _tOwned[account] = tokenFromReflection(_rOwned[account]);
        }
        _isExcluded[account] = true;
        _excluded.push(account);
        emit ExcludeFromReward(account);
    }

    function includeInReward(address account) external onlyOwner() {
        require(_isExcluded[account], "Account is already excluded");
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_excluded[i] == account) {
                _excluded[i] = _excluded[_excluded.length - 1];
                _rOwned[account] = reflectionFromToken(_tOwned[account]);
                _isExcluded[account] = false;
                _excluded.pop();
                break;
            }
        }
        emit IncludeInReward(account);
    }
    
    function excludeFromFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = true;
        emit ExcludeFromFee(account);
    }
    
    function includeInFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = false;
        emit IncludeInFee(account);
    }    
   
    function setMaxTxAmount(uint256 amount) external onlyOwner() {
        maxTxAmount = amount;
        require(maxTxAmount >= _tTotal.div(10000));
        emit MaxTransactionAmountChanged(maxTxAmount);
    } 

    function setMaxWalletAmount(uint256 amount) external onlyOwner() {
        maxWalletAmount = amount;
        require(maxWalletAmount >= _tTotal.div(1000));
        emit MaxHoldAmountChanged(maxWalletAmount);
    }     

    function setWalletAddress(address _gameWallet, address _marketingWallet) external onlyOwner() {
        gameWallet = _gameWallet;
        marketingWallet = _marketingWallet;        
        gameContract = IGameContract(gameWallet);
        emit WalletAddressChanged(_gameWallet, _marketingWallet);
    }

    function setSwapAndLiquifyEnabled(bool _enabled) public onlyOwner {
        swapAndLiquifyEnabled = _enabled;
        emit SwapAndLiquifyEnabledUpdated(_enabled);
    }

    function setFeeOnBuy(uint256 marketingFeeInput, uint256 gameFeeInput, uint256 reflectionFeeInput, uint256 liquidityFeeInput) external onlyOwner {
        marketingFeeOnBuy = marketingFeeInput;
        gameFeeOnBuy = gameFeeInput;
        reflectionFeeOnBuy = reflectionFeeInput;
        liquidityFeeOnBuy = liquidityFeeInput;
        uint256 totalFee = marketingFeeOnBuy.add(gameFeeOnBuy).add(reflectionFeeOnBuy).add(liquidityFeeOnBuy);
        require(totalFee <= 25, "Total Fee must be lower than 25.");
    }

    function setFeeOnSell(uint256 marketingFeeInput, uint256 gameFeeInput, uint256 reflectionFeeInput, uint256 liquidityFeeInput) external onlyOwner {
        marketingFeeOnSell = marketingFeeInput;
        gameFeeOnSell = gameFeeInput;
        reflectionFeeOnSell = reflectionFeeInput;
        liquidityFeeOnSell = liquidityFeeInput;
        uint256 totalFee = marketingFeeOnSell.add(gameFeeOnSell).add(reflectionFeeOnSell).add(liquidityFeeOnSell);
        require(totalFee <= 25, "Total Fee must be lower than 25.");
    }

    function setFeeOnDungeon(uint256 marketingFeeInput, uint256 gameFeeInput, uint256 reflectionFeeInput, uint256 liquidityFeeInput) external onlyOwner {
        marketingFeeOnDungeon = marketingFeeInput;
        gameFeeOnDungeon = gameFeeInput;
        reflectionFeeOnDungeon = reflectionFeeInput;
        liquidityFeeOnDungeon = liquidityFeeInput;
        uint256 totalFee = marketingFeeOnDungeon.add(gameFeeOnDungeon).add(reflectionFeeOnDungeon).add(liquidityFeeOnDungeon);
        require(totalFee <= 25, "Total Fee must be lower than 25.");
    }

    function removeFromBlacklist(address addr) external onlyOwner {
        require(blacklist[addr], "Address already removed from blacklist!");
        blacklist[addr] = false;
        emit RemoveFromBlacklist(addr);
    }

    function payEntrance (uint256 amount, uint256 level) external returns (uint256){
        amount = amount.mul(1000000000);
        _basicTransfer(msg.sender, gameWallet, amount);
        gameContract.payEntrance(msg.sender, amount, level);
        return block.number;
    }
    
     //to recieve ETH from uniswapV2Router when swaping
    receive() external payable {}

    function _getRate() private view returns(uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply.div(tSupply);
    }

    function _getCurrentSupply() private view returns(uint256, uint256) {
        uint256 rSupply = _rTotal;
        uint256 tSupply = _tTotal;      
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_rOwned[_excluded[i]] > rSupply || _tOwned[_excluded[i]] > tSupply) return (_rTotal, _tTotal);
            rSupply = rSupply.sub(_rOwned[_excluded[i]]);
            tSupply = tSupply.sub(_tOwned[_excluded[i]]);
        }
        if (rSupply < _rTotal.div(_tTotal)) return (_rTotal, _tTotal);
        return (rSupply, tSupply);
    }
    
    function isExcludedFromFee(address account) public view returns(bool) {
        return _isExcludedFromFee[account];
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        require(!blacklist[from], "Blacklisted account can not sell or transfer token!");        

        if(!_isExcludedFromFee[from] && !_isExcludedFromFee[to])
            require(amount <= maxTxAmount, "Transfer amount exceeds the maxTxAmount.");
        
        bool overMinTokenBalance = balanceOf(address(this)) >= numTokensSellToAddToLiquidity;
        if (
            overMinTokenBalance &&
            !inSwapAndLiquify &&
            from != uniswapV2Pair &&
            swapAndLiquifyEnabled
        ) {
            swapAndLiquify();
        }

        if (tradingOpenBlock == 0 && to == uniswapV2Pair){
            require(_isExcludedFromFee[from], "Whitelisted address only can be the first liquidity provider.");
            tradingOpenBlock = block.number;
            tradingOpenTimeStamp = block.timestamp;
        }

        if (tradingOpenBlock + 10 >= block.number){
            if (from == uniswapV2Pair && !_isExcludedFromFee[to]) blacklist[to] = true;            
        }
        
        if(_isExcludedFromFee[from] || _isExcludedFromFee[to] || (tradingOpenBlock == 0)){
            _basicTransfer(from, to, amount);            
        } else {
            _transferStandard(from, to, amount);
            if (!_isExcludedFromFee[to] && to != uniswapV2Pair && to != gameWallet)  {
                require(balanceOf(to) <= maxWalletAmount, "Wallet amount exceeds maxWalletAmount.");
            }
        }
        
    }

    function swapAndLiquify() private lockTheSwap {
        // split the contract balance into halves
        uint256 swapAmount = liquidityAccumulated.div(2);
        swapTokensForEth(swapAmount);
        uint256 balance = address(this).balance;
        addLiquidity(swapAmount, balance);
        emit SwapAndLiquify(swapAmount, balance, swapAmount);

        swapAmount = balanceOf(address(this));
        swapTokensForEth(swapAmount);
        address(payable(marketingWallet)).call{value: address(this).balance}("");
        liquidityAccumulated = 0;
        marketingAccumulated = 0;
    }

    function swapTokensForEth(uint256 tokenAmount) private {
        // generate the uniswap pair path of token -> weth
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapV2Router.WETH();

        _approve(address(this), address(uniswapV2Router), tokenAmount);

        // make the swap
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of ETH
            path,
            address(this),
            block.timestamp
        );
    }

    function addLiquidity(uint256 tokenAmount, uint256 ethAmount) private {
        // approve token transfer to cover all possible scenarios
        _approve(address(this), address(uniswapV2Router), tokenAmount);

        // add the liquidity
        uniswapV2Router.addLiquidityETH{value: ethAmount}(
            address(this),
            tokenAmount,
            0, // slippage is unavoidable
            0, // slippage is unavoidable
            owner(),
            block.timestamp
        );
    }
    
    function _basicTransfer(address sender, address recipient, uint256 tAmount) private {
        uint256 rate = _getRate();
        uint256 rAmount = tAmount.mul(rate);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        _rOwned[recipient] = _rOwned[recipient].add(rAmount);
        if (_isExcluded[sender]) _tOwned[sender] = _tOwned[sender].sub(tAmount);
        if (_isExcluded[recipient]) _tOwned[recipient] = _tOwned[recipient].add(tAmount);
        emit Transfer(sender, recipient, tAmount);
    }

    function _transferStandard(address sender, address recipient, uint256 tAmount) private {        
        if (sender == uniswapV2Pair){
            marketingFee = marketingFeeOnBuy;
            gameFee = gameFeeOnBuy;
            reflectionFee = reflectionFeeOnBuy;
            liquidityFee = liquidityFeeOnBuy;
        } else if (sender == gameWallet){
            marketingFee = marketingFeeOnDungeon;
            gameFee = gameFeeOnDungeon;
            reflectionFee = reflectionFeeOnDungeon;
            liquidityFee = liquidityFeeOnDungeon;
        } else if (recipient == uniswapV2Pair) {
            marketingFee = marketingFeeOnSell;
            gameFee = gameFeeOnSell;
            reflectionFee = reflectionFeeOnSell;
            liquidityFee = liquidityFeeOnSell;
        } else {
            marketingFee = marketingFeeOnBuy;
            gameFee = gameFeeOnBuy;
            reflectionFee = reflectionFeeOnBuy;
            liquidityFee = liquidityFeeOnBuy;
        }
        uint256 rate = _getRate();
        uint256 tReflectionAmount = tAmount.div(100).mul(reflectionFee);
        uint256 tLiquidityAmount = tAmount.div(100).mul(liquidityFee);
        uint256 tGameAmount = tAmount.div(100).mul(gameFee);
        uint256 tMarketingAmount = tAmount.div(100).mul(marketingFee);        
        uint256 tTransferAmount = tAmount.sub(tReflectionAmount);
        tTransferAmount = tTransferAmount.sub(tLiquidityAmount).sub(tGameAmount).sub(tMarketingAmount);
        
        _rOwned[sender] = _rOwned[sender].sub(tAmount.mul(rate));
        _rOwned[recipient] = _rOwned[recipient].add(tTransferAmount.mul(rate));
        _rOwned[address(this)] = _rOwned[address(this)].add(tLiquidityAmount.mul(rate)).add(tMarketingAmount.mul(rate));
        _rOwned[gameWallet] = _rOwned[gameWallet].add(tGameAmount.mul(rate));
        // _rOwned[marketingWallet] = _rOwned[marketingWallet].add(tMarketingAmount.mul(rate));
        
        if (_isExcluded[sender]) _tOwned[sender] = _tOwned[sender].sub(tAmount);
        if (_isExcluded[recipient]) _tOwned[recipient] = _tOwned[recipient].add(tTransferAmount);
        if (_isExcluded[address(this)]) _tOwned[address(this)] = _tOwned[address(this)].add(tLiquidityAmount).add(tMarketingAmount);
        if (_isExcluded[gameWallet]) _tOwned[gameWallet] = _tOwned[gameWallet].add(tGameAmount);
        // if (_isExcluded[marketingWallet]) _tOwned[marketingWallet] = _tOwned[marketingWallet].add(tMarketingAmount);               
        liquidityAccumulated = liquidityAccumulated.add(tLiquidityAmount);
        marketingAccumulated = marketingAccumulated.add(tMarketingAmount);

        emit Transfer(sender, address(this), tLiquidityAmount.add(tMarketingAmount));
        emit Transfer(sender, gameWallet, tGameAmount);
        // emit Transfer(sender, marketingWallet, tMarketingAmount);        
        emit Transfer(sender, recipient, tTransferAmount);
        
        _rTotal = _rTotal.sub(tReflectionAmount.mul(rate));
    }

}