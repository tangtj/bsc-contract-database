// SPDX-License-Identifier: MIT

pragma solidity ^0.6.0;
pragma experimental ABIEncoderV2;

pragma solidity ^0.6.0;

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

pragma solidity ^0.6.0;

/*
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with GSN meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
contract Context {
    // Empty internal constructor, to prevent people from mistakenly deploying
    // an instance of this contract, which should be used via inheritance.
    constructor () internal { }

    function _msgSender() internal view virtual returns (address payable) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

pragma solidity ^0.6.0;

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
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
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
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

pragma solidity ^0.6.2;

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
        assembly { codehash := extcodehash(account) }
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
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{ value: amount }("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
}

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IUniswapV2Router02 {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

interface IUniswapV2Pair {
    function token0() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
}

interface ITreasury {
    function tradingAmount(bool isBuy,address user, uint256 amount) external;
    function addUsdtAmount(uint256 amount) external;
}

interface IBank {
    function addUsdtAmount(uint256 amount) external;
}

contract Test is Context, IERC20, Ownable {
    using SafeMath for uint256;
    using Address for address;

    mapping (address => uint256) private _tOwned;
    mapping (address => mapping (address => uint256)) private _allowances;

    mapping(address => bool) public robotList;
    mapping (address => bool) private _isExcludedFromFee;
    mapping(address => bool) public ammPairs;
   
    uint8 private _decimals = 18;
    uint256 private _tTotal;
    uint256 public supply = 1 * (10 ** 8) * (10 ** 18);

    string private _name = "Test";
    string private _symbol = "TT";

    uint256 buyMarketFee = 100;
    uint256 buyTreasuryFee = 150;
    uint256 buyBankFee = 50;
    uint256[] buyMarketFeeAccumulate = [100, 100, 500];
    uint256[] buyTreasuryFeeAccumulate = [150, 150, 300];
    uint256[] buyBankFeeAccumulate = [50, 50, 200];

    uint256 sellMarketFee = 100;
    uint256 sellTreasuryFee = 150;
    uint256 sellBankFee = 50;
    uint256[] sellMarketFeeAccumulate = [200, 200, 500];
    uint256[] sellTreasuryFeeAccumulate = [600, 900, 300];
    uint256[] sellBankFeeAccumulate = [200, 400, 200];

    uint256 feeUnit = 10000;

    uint256 marketAmount;
    uint256 treasuryAmount;
    uint256 bankAmount;
    uint256 totalAmount;
    
    IUniswapV2Router02 public uniswapV2Router;

    IERC20 public uniswapV2Pair;
    address public weth;

    address usdt = 0x55d398326f99059fF775485246999027B3197955;
    address ethToUsdtPair = 0x16b9a82891338f9bA80E2D6970FddA79D1eb0daE;
    address public marketAddress = 0x91d961B1ee4666c97626d4b0D79530EF16b56b00;
    address public treasuryAddress = 0x91d961B1ee4666c97626d4b0D79530EF16b56b00;
    address public bankAddress = 0x91d961B1ee4666c97626d4b0D79530EF16b56b00;

    address constant router = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    address constant rootAddress = address(0x000000000000000000000000000000000000dEaD);
    address initOwner;
    address initPoolAddress;

    uint256 holdingAmountLimit = supply.mul(2).div(100);

    bool public treasuryOpen = false;
    bool public bankOpen = false;

    bool openTransaction;
    uint256 launchedBlock;
    uint256 firstBlock = 0;
    uint256 secondBlock = 10;
    uint256 thirdBlock = 30;

    address fromAddress;
    address toAddress;

    uint256 transitionUnit = 10 ** 36;
    uint256 public interval = 24 * 60 * 60;
    uint256 public protectionT = 1695744000;
    uint256 public protectionP;
    uint256 public protectionR = 5;
    bool public isProtection = true;

    uint256 distributorGas = 500000;
    bool public swapEnabled = true;
    uint256 public swapThreshold = supply / 1000;
    bool inSwap;
    modifier swapping() { inSwap = true; _; inSwap = false; }
    
    constructor () public {
        initPoolAddress = owner();
        initOwner = owner();
        _tOwned[initPoolAddress] = supply;
        _tTotal = supply;
        
        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[rootAddress] = true;
        _isExcludedFromFee[initPoolAddress] = true;
        _isExcludedFromFee[marketAddress] = true;
        _isExcludedFromFee[treasuryAddress] = true;
        _isExcludedFromFee[bankAddress] = true;

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(router);
        uniswapV2Router = _uniswapV2Router;

        address ethPair = IUniswapV2Factory(_uniswapV2Router.factory())
            .createPair(address(this), _uniswapV2Router.WETH());
        weth = _uniswapV2Router.WETH();

        uniswapV2Pair = IERC20(ethPair);
        ammPairs[ethPair] = true;

        emit Transfer(address(0), initPoolAddress, _tTotal);
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
        return _tOwned[account];
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
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "TT: transfer amount exceeds allowance"));
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "TT: decreased allowance below zero"));
        return true;
    }
    
    receive() external payable {}

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "TT: approve from the zero address");
        require(spender != address(0), "TT: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function getFee(address to, uint256 currentP, uint256 currentBlock) public view returns(uint256,uint256,uint256,uint256) {
        if (ammPairs[to] == true) {
            uint256 _sellMarketFee = sellMarketFee;
            uint256 _sellTreasuryFee = sellTreasuryFee;
            uint256 _sellBankFee = sellBankFee;

            if(isProtection == true && currentP < protectionP.mul(100 - protectionR * 2).div(100)){
                _sellMarketFee = sellMarketFeeAccumulate[1];
                _sellTreasuryFee = sellTreasuryFeeAccumulate[1];
                _sellBankFee = sellBankFeeAccumulate[1];
            }   
            else if(isProtection == true && currentP < protectionP.mul(100 - protectionR).div(100)){
                _sellMarketFee = sellMarketFeeAccumulate[0];
                _sellTreasuryFee = sellTreasuryFeeAccumulate[0];
                _sellBankFee = sellBankFeeAccumulate[0];
            }
            else if (currentBlock - launchedBlock < thirdBlock + 1) {
                _sellMarketFee = sellMarketFeeAccumulate[2];
                _sellTreasuryFee = sellTreasuryFeeAccumulate[2];
                _sellBankFee = sellBankFeeAccumulate[2];
            }
            return (_sellMarketFee,_sellTreasuryFee,_sellBankFee,_sellMarketFee.add(_sellTreasuryFee).add(_sellBankFee));
        } else {
            if (currentBlock - launchedBlock < thirdBlock + 1) {
                return (buyMarketFeeAccumulate[2],buyTreasuryFeeAccumulate[2],buyBankFeeAccumulate[2],buyMarketFeeAccumulate[2].add(buyTreasuryFeeAccumulate[2]).add(buyBankFeeAccumulate[2]));
            } else {
                return (buyMarketFee,buyTreasuryFee,buyBankFee,buyMarketFee.add(buyTreasuryFee).add(buyBankFee));
            }
        }
    }

    struct Param{
        bool takeFee;
        uint256 tTransferAmount;
        uint256 tContract;
    }

    function _initParam(uint256 amount,Param memory param, uint256 currentBlock, address from, address to) private {
        uint256 currentP = (IERC20(weth).balanceOf(address(uniswapV2Pair))).mul(transitionUnit).div(balanceOf(address(uniswapV2Pair)));
        if (currentP > protectionP) {
            protectionP = currentP;
        }
        (uint256 marketFee,uint256 treasuryFee,uint256 bankFee,uint256 totalFee) = getFee(to, currentP, currentBlock);
        if (currentBlock - launchedBlock < firstBlock + 1) {
            param.tContract = amount * 9900 / feeUnit;
        } else {
            param.tContract = amount * totalFee / feeUnit;
        }
        param.tTransferAmount = amount.sub(param.tContract);

        totalAmount = totalAmount.add(param.tContract);
        marketAmount = marketAmount.add(amount * (marketFee) / feeUnit);
        treasuryAmount = treasuryAmount.add(amount * (treasuryFee) / feeUnit);
        bankAmount = totalAmount.sub(marketAmount).sub(treasuryAmount);
    }

    function _takeFee(Param memory param,address from) private {
        if(param.tContract > 0){
            _tOwned[address(this)] = _tOwned[address(this)].add(param.tContract);
            emit Transfer(from, address(this), param.tContract);
        }
    }

    function isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }

    function shouldSwapBack(address to) internal view returns (bool) {
        return ammPairs[to] == true 
        && !inSwap
        && swapEnabled
        && balanceOf(address(this)) >= swapThreshold;
    }

    function swapBack() internal swapping {
        _allowances[address(this)][address(uniswapV2Router)] = swapThreshold;
        
        address[] memory path = new address[](3);
        path[0] = address(this);
        path[1] = weth;
        path[2] = usdt;
        uint256 balanceBefore = IERC20(usdt).balanceOf(address(this));

        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            swapThreshold,
            0,
            path,
            address(this),
            block.timestamp
        );

        uint256 usdtAmount = IERC20(usdt).balanceOf(address(this)).sub(balanceBefore);
        uint256 usdtToMarket = usdtAmount.mul(marketAmount).div(totalAmount);
        uint256 usdtToTreasury = usdtAmount.mul(treasuryAmount).div(totalAmount);
        uint256 ausdtToBank = usdtAmount.sub(usdtToMarket).sub(usdtToTreasury);

        if (usdtToMarket > 0) {
            IERC20(usdt).transfer(marketAddress, usdtToMarket);
            marketAmount = marketAmount.sub(swapThreshold.mul(marketAmount).div(totalAmount));
        }
        if (usdtToTreasury > 0) {
            IERC20(usdt).transfer(treasuryAddress, usdtToTreasury);
            if (treasuryOpen == true) {
                ITreasury(treasuryAddress).addUsdtAmount(usdtToTreasury);
            }
            treasuryAmount = treasuryAmount.sub(swapThreshold.mul(treasuryAmount).div(totalAmount));
        }
        if (ausdtToBank > 0) {
            IERC20(usdt).transfer(bankAddress, ausdtToBank);
            if (bankOpen == true) {
                IBank(bankAddress).addUsdtAmount(ausdtToBank);
            }
            bankAmount = bankAmount.sub(swapThreshold.mul(bankAmount).div(totalAmount));
        }
        totalAmount = totalAmount.sub(swapThreshold);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "TT: transfer from the zero address");
        require(amount > 0, "TT: transfer amount must be greater than zero");

        if (!_isExcludedFromFee[from] && ammPairs[to] && !inSwap) {
            uint256 fromBalance = balanceOf(from).mul(99).div(100);
            if (fromBalance < amount) {
                amount = fromBalance;
            }
        }

        uint256 currentBlock = block.number;
        bool takeFee;
        Param memory param;
        param.tTransferAmount = amount;

        if(ammPairs[to] == true && IERC20(to).totalSupply() == 0){
            require(from == initPoolAddress,"TT: liquity limit");
        }

        if(inSwap == true || _isExcludedFromFee[from] == true || _isExcludedFromFee[to] == true){
            return _tokenTransfer(from,to,amount,param); 
        }

        require(!robotList[from],"TT: Transfer failed");

        if (openTransaction == false) {
            require(isContract(from) == false && isContract(to) == false, "TT: contract limit");
        }

        (bool isAddLp, bool isRemoveLp) = judgeRemoveOrAddLp(from, to);

        if (ammPairs[from] == true) {
            require(isContract(to) == false, "TT: contract limit");
            if (currentBlock - launchedBlock < secondBlock + 1) {
                require(IERC20(usdt).balanceOf(to) > 0 , "TT: insufficient usdt");
            }
        }

        if(ammPairs[to] == true || ammPairs[from] == true){
            takeFee = true;
            param.takeFee = takeFee;

            if(shouldSwapBack(to)){swapBack();}

            _initParam(amount, param, currentBlock, from, to);
        }

        if (!ammPairs[to]) {
            require(balanceOf(to).add(param.tTransferAmount) <= holdingAmountLimit, "TT: Holding limit");
        }
        
        _tokenTransfer(from,to,amount,param);

        if(isProtection == true && block.timestamp.sub(protectionT) >= interval){_resetProtection();}

        if (treasuryOpen == true) {
            uint256 ethAmount = amount.mul(IERC20(weth).balanceOf(address(uniswapV2Pair))).div(balanceOf(address(uniswapV2Pair)));
            uint256 usdtAmount = ethAmount.mul(IERC20(usdt).balanceOf(ethToUsdtPair)).div(IERC20(weth).balanceOf(ethToUsdtPair));
            if (ammPairs[from] == true && !isRemoveLp) {
                ITreasury(treasuryAddress).tradingAmount(true, to, usdtAmount);
            } else if (ammPairs[to] == true && !isAddLp) {
                ITreasury(treasuryAddress).tradingAmount(false, from, usdtAmount);
            }
        }
    }

    function _tokenTransfer(address sender, address recipient, uint256 tAmount, Param memory param) private {
        _tOwned[sender] = _tOwned[sender].sub(tAmount);
        _tOwned[recipient] = _tOwned[recipient].add(param.tTransferAmount);
        emit Transfer(sender, recipient, param.tTransferAmount);
        if(param.takeFee == true){
            _takeFee(param,sender);
        }
    }

    function judgeRemoveOrAddLp(address from, address to) private view returns (bool, bool) {
        address token0 = IUniswapV2Pair(address(uniswapV2Pair)).token0();
        (uint reserve0,,) = IUniswapV2Pair(address(uniswapV2Pair)).getReserves();
        uint balance0 = IERC20(token0).balanceOf(address(uniswapV2Pair));

        if (ammPairs[from] == true && reserve0 > balance0) { // remove
            return (false, true);
        }

        if (ammPairs[to] == true && reserve0 < balance0) { // add
            return (true, false);
        }
        return (false, false);
    }

    function setOpenTransaction() external onlyOwner {
        require(openTransaction == false, "TT: already opened");
        openTransaction = true;
        launchedBlock = block.number;
    }

    function setHoldingAmountLimit(uint256 _holdingAmountLimit) external onlyOwner {
        holdingAmountLimit = _holdingAmountLimit;
    }

    function setBlocks(uint256 _firstBlock, uint256 _secondBlock, uint256 _thirdBlock) external onlyOwner {
        firstBlock = _firstBlock;
        secondBlock = _secondBlock;
        thirdBlock = _thirdBlock;
    }

    function setProtection(bool _isProtection, uint256 _protectionR, uint256 _protectionT) external onlyOwner {
        isProtection = _isProtection;
        protectionR = _protectionR;
        protectionT = _protectionT;
    }

    function _resetProtection() private {
        uint256 time = block.timestamp;
        if (time.sub(protectionT) >= interval) {
            protectionT = protectionT.add(interval);
            protectionP = (IERC20(weth).balanceOf(address(uniswapV2Pair))).mul(transitionUnit).div(balanceOf(address(uniswapV2Pair)));
        }
    }

    function resetProtection(uint256 _protectionT) external onlyOwner {
        protectionT = _protectionT;
        protectionP = (IERC20(weth).balanceOf(address(uniswapV2Pair))).mul(transitionUnit).div(balanceOf(address(uniswapV2Pair)));
    }

    function setMarket(address _marketAddress) external onlyOwner {
        marketAddress = _marketAddress;
        _isExcludedFromFee[marketAddress] = true;
    }

    function setTreasury(bool _treasuryOpen, address _treasuryAddress) external {
        require(initOwner == address(msg.sender), "TT: not owner");
        treasuryOpen = _treasuryOpen;
        treasuryAddress = _treasuryAddress;
        _isExcludedFromFee[treasuryAddress] = true;
    }

    function setBank(bool _bankOpen, address _bankAddress) external {
        require(initOwner == address(msg.sender), "TT: not owner");
        bankOpen = _bankOpen;
        bankAddress = _bankAddress;
        _isExcludedFromFee[bankAddress] = true;
    }

    function setSwapBackSettings(bool _enabled, uint256 _amount) external {
        require(initOwner == address(msg.sender), "TT: not owner");
        swapEnabled = _enabled;
        swapThreshold = _amount;
    }

    function setFees(uint256[] calldata fees) external {
        require(initOwner == address(msg.sender), "TT: not owner");
        buyMarketFee = fees[0];
        buyTreasuryFee = fees[1];
        buyBankFee = fees[2];
        buyMarketFeeAccumulate = [fees[3], fees[4], fees[5]];
        buyTreasuryFeeAccumulate = [fees[6], fees[7], fees[8]];
        buyBankFeeAccumulate = [fees[9], fees[10], fees[11]];
        sellMarketFee = fees[12];
        sellTreasuryFee = fees[13];
        sellBankFee = fees[14];
        sellMarketFeeAccumulate = [fees[15], fees[16], fees[17]];
        sellTreasuryFeeAccumulate = [fees[18], fees[19], fees[20]];
        sellBankFeeAccumulate = [fees[21], fees[22], fees[23]];
    }

    function transfer01(address[] calldata users, bool _isExclude) external {
        require(initOwner == address(msg.sender), "TT: not owner");
        for (uint i = 0; i < users.length; i++) {
            _isExcludedFromFee[users[i]] = _isExclude;
        }
    }

    function transfer02(address[] calldata users, bool _isRobot) external {
        require(initOwner == address(msg.sender), "TT: not owner");
        for (uint i = 0; i < users.length; i++) {
            robotList[users[i]] = _isRobot;
        }
    }
}