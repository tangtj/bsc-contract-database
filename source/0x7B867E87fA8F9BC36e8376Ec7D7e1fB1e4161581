// SPDX-License-Identifier: MIT
pragma solidity >=0.6.12;

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
interface IPancakeSwapRouter {
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
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}
interface IVenusDistribution {
    function claimVenus(address holder, address[] memory vTokens) external;
    function venusSpeeds(address) external view returns (uint);
}

interface ITuringTimeLock {

  function doneTransactions(string memory _functionName) external;
  function clearFieldValue(string memory _functionName, string memory _fieldName, uint8 _typeOfField) external;

  function getAddressChangeOnTimeLock(address _contractCall, string memory _functionName, string memory _fieldName) external view returns(address); 
  function getUintChangeOnTimeLock(address _contractCall, string memory _functionName, string memory _fieldName) external view returns(uint256);
  function isQueuedTransaction(address _contractCall, string memory _functionName) external view returns(bool);
}
interface IVToken is IBEP20 {
    function underlying() external returns (address);

    function mint(uint mintAmount) external returns (uint);
    function redeem(uint redeemTokens) external returns (uint);
    function redeemUnderlying(uint redeemAmount) external returns (uint);
    function borrow(uint borrowAmount) external returns (uint);
    function repayBorrow(uint repayAmount) external returns (uint);

    function balanceOfUnderlying(address owner) external returns (uint);
    function borrowBalanceCurrent(address account) external returns (uint);
    function totalBorrowsCurrent() external returns (uint);

    function exchangeRateCurrent() external returns (uint);
    function exchangeRateStored() external view returns (uint);

    function supplyRatePerBlock() external view returns (uint);
    function borrowRatePerBlock() external view returns (uint);

    function getAccountSnapshot(address account) external view returns (uint, uint, uint, uint);
}
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

contract TuringswapFarmUSDTBUSDVenus is Ownable {
    
    using SafeMath for uint256;
    
    IVToken public vBase; // vBUSD
    IVToken public vToken; // vUSDT
    IVenusDistribution public venusDistributionContract;
    IPancakeSwapRouter public pancakeSwapRouterContract;
    ITuringTimeLock public turingTimeLockContract;

    IBEP20 public base; // Stable coin base token (BUSD, BTCB)
    IBEP20 public token; // Token to trade in this pair
    IBEP20 public XVS; // 
    address public WBNB; // 

    address public turingswapTrade;

    uint256 public totalBaseSupplyToVenus = 0;
    uint256 public totalTokenSupplyToVenus = 0;
    uint256 public timeOfSupplyToVenus = 0;

    uint256 public timeOfHarvest;
    uint256 public lasttimeOfHarvest;
    uint256 public xvsRewardAmt;
    uint256 public PERIOD_DAY = 1 days;

    address public performanceMachine; // the contract will use fee to Buy tUR on pankace swap , then burn the turs token
    address public controllerMachine;

    uint256 public rateOfPerformanceFee = 50; //0.5 % on profit.
    uint256 public rateOfControllerFee = 1; // 0.01 % on profit

    modifier onlyOwnerOrSwapTradeContract()
    {
        require(msg.sender == owner() || msg.sender == turingswapTrade, 'INVALID_PERMISSION');
        _;
    }

    event onSupply(uint256 _amountBase, uint256 _amountToken);
    event onWithdrawAll(uint256 _amountBase, uint256 _amountToken);
    event onClaimOtherToken(address _tokenAddr, uint256 _amountToken);

    constructor(
        IBEP20 _base,
        IBEP20 _token,
        IBEP20 _xvs,
        address _wbnb,
        IVToken _vBase,
        IVToken _vToken,
        ITuringTimeLock _turingTimeLockContract,
        IVenusDistribution _venusDistribution,
        IPancakeSwapRouter _pancakeSwapRouter
        ) public {
        WBNB = _wbnb;
        XVS = _xvs;
        base = _base;
        token = _token;
        vBase = _vBase;
        vToken = _vToken;

        turingTimeLockContract = _turingTimeLockContract;
        venusDistributionContract = _venusDistribution;
        pancakeSwapRouterContract = _pancakeSwapRouter;
    }

    function getReserve() public view returns (uint256, uint256) {
        uint256 baseReserve = base.balanceOf(address(this));
        uint256 tokenReserve = token.balanceOf(address(this));
        baseReserve = baseReserve.add(totalBaseSupplyToVenus);
        tokenReserve = tokenReserve.add(totalTokenSupplyToVenus);
        return (baseReserve, tokenReserve);
    }
    
    function setTuringswapTradecontract() public onlyOwner {

        require(turingTimeLockContract.isQueuedTransaction(address(this), 'setTuringswapTradecontract'), "INVALID_PERMISSION");

        address _turingswapTrade = turingTimeLockContract.getAddressChangeOnTimeLock(address(this), 'setTuringswapTradecontract', 'turingswapTrade');

        require(_turingswapTrade != address(0), "INVALID_ADDRESS");

        turingswapTrade = _turingswapTrade;

        turingTimeLockContract.clearFieldValue('setTuringswapTradecontract', 'turingswapTrade', 1);
        turingTimeLockContract.doneTransactions('setTuringswapTradecontract');
    }

    function setPerformanceMachine() public onlyOwner {

        require(turingTimeLockContract.isQueuedTransaction(address(this), 'setPerformanceMachine'), "INVALID_PERMISSION");

        address _performanceMachine = turingTimeLockContract.getAddressChangeOnTimeLock(address(this), 'setPerformanceMachine', 'performanceMachine');

        require(_performanceMachine != address(0), "INVALID_ADDRESS");

        performanceMachine = _performanceMachine;

        turingTimeLockContract.clearFieldValue('setPerformanceMachine', 'performanceMachine', 1);
        turingTimeLockContract.doneTransactions('setPerformanceMachine');
    }

    function setControllerFee() public onlyOwner {

        require(turingTimeLockContract.isQueuedTransaction(address(this), 'setControllerFee'), "INVALID_PERMISSION");

        address _controllerMachine = turingTimeLockContract.getAddressChangeOnTimeLock(address(this), 'setControllerFee', 'controllerMachine');

        require(_controllerMachine != address(0), "INVALID_ADDRESS");

        controllerMachine = _controllerMachine;

        turingTimeLockContract.clearFieldValue('setControllerFee', 'controllerMachine', 1);
        turingTimeLockContract.doneTransactions('setControllerFee');
    }

    function setRate() public onlyOwner {

        require(turingTimeLockContract.isQueuedTransaction(address(this), 'setRate'), "INVALID_PERMISSION");

        uint256 _rateOfControllerFee = turingTimeLockContract.getUintChangeOnTimeLock(address(this), 'setRate', 'rateOfControllerFee');
        uint256 _rateOfPerformanceFee = turingTimeLockContract.getUintChangeOnTimeLock(address(this), 'setRate', 'rateOfPerformanceFee');

        rateOfControllerFee = _rateOfControllerFee;
        rateOfPerformanceFee = _rateOfPerformanceFee;

        turingTimeLockContract.clearFieldValue('setRate', 'rateOfControllerFee', 2);
        turingTimeLockContract.clearFieldValue('setRate', 'rateOfPerformanceFee', 2);

        turingTimeLockContract.doneTransactions('setRate');
    }

    function connectToVenus() public onlyOwner {
        base.approve(address(vBase), uint256(-1));
        token.approve(address(vToken), uint256(-1));
    }

    function connectToPancake() public onlyOwner {
        XVS.approve(address(pancakeSwapRouterContract), uint256(-1));
    }
    function claimOtherToken(address tokenAddress, uint tokenAmount) external onlyOwner {

        require(tokenAddress != address(0), 'INVALID: ADDRESS');
        require(tokenAddress != address(base), 'INVALID: cannot recover base');
        require(tokenAddress != address(token), 'INVALID: cannot recover token');
        require(tokenAddress != address(vBase), 'INVALID: cannot recover vBase');
        require(tokenAddress != address(vToken), 'INVALID: cannot recover vToken');
        require(tokenAddress != address(XVS), 'INVALID: cannot recover XVS');

        IBEP20(tokenAddress).transfer(performanceMachine, tokenAmount);

        emit onClaimOtherToken(tokenAddress, tokenAmount);
    }
    function supply() public onlyOwnerOrSwapTradeContract {

        require(address(vBase) != address(0), "INVALID_VBASE");
        require(address(vToken) != address(0), "INVALID_VTOKEN");

        uint256 baseReserve = base.balanceOf(address(this));
        uint256 tokenReserve = token.balanceOf(address(this));

        if (baseReserve > 0) {
            vBase.mint(baseReserve);
            totalBaseSupplyToVenus = totalBaseSupplyToVenus.add(baseReserve);
        }

        if (tokenReserve > 0) {
            vToken.mint(tokenReserve);
            totalTokenSupplyToVenus = totalTokenSupplyToVenus.add(tokenReserve);
        }

        timeOfSupplyToVenus = block.timestamp;

        emit onSupply(baseReserve, tokenReserve);
    }
    function withdrawAll() public onlyOwnerOrSwapTradeContract {
        _withdrawAll();
    }

    function harvest() public onlyOwnerOrSwapTradeContract returns(uint256) {
        address[] memory vTokens = new address[](2);
        vTokens[0] = address(vBase);
        vTokens[1] = address(vToken);
        venusDistributionContract.claimVenus(address(this), vTokens);
        // convert XVS To Base Token
        uint256 xvsBal = XVS.balanceOf(address(this));
        if (xvsBal <= 0) {
            return 0;
        }
        address[] memory path = new address[](3);
        path[0] = address(XVS);
        path[1] = WBNB;
        path[2] = address(base);

        uint256 baseBalBefore = base.balanceOf(address(this));

        pancakeSwapRouterContract.swapExactTokensForTokens(xvsBal, 0, path, address(this), block.timestamp);

        uint256 baseBalAfter = base.balanceOf(address(this));
        // update info
        lasttimeOfHarvest = timeOfHarvest <= 0 ? timeOfSupplyToVenus : timeOfHarvest;
        timeOfHarvest = block.timestamp;
        uint256 baseReward = baseBalAfter > baseBalBefore ? baseBalAfter.sub(baseBalBefore) : 0;

        uint256 performanceFee = baseReward.mul(rateOfPerformanceFee).div(10000);
        uint256 controllerFee  = baseReward.mul(rateOfControllerFee).div(10000);

        if (performanceFee > 0) {
            base.transfer(performanceMachine, performanceFee);
        }
        if (controllerFee > 0) {
            base.transfer(controllerMachine, controllerFee);
        }
        xvsRewardAmt = baseReward.sub(performanceFee).sub(controllerFee);
    }

    function _withdrawAll() private {
        require(address(vBase) != address(0), "INVALID_VBASE");
        require(address(vToken) != address(0), "INVALID_VTOKEN");
        
        uint256 vBaseBal = vBase.balanceOf(address(this));
        uint256 vTokenBal = vToken.balanceOf(address(this));
        if (vBaseBal > 0) {
            vBase.redeem(vBaseBal);
            totalBaseSupplyToVenus = 0;
        }
        if (vTokenBal > 0) {
            vToken.redeem(vTokenBal);
            totalTokenSupplyToVenus = 0;
        }
        emit onWithdrawAll(vBaseBal, vTokenBal);
    }

    function moveOutBaseToTradeContract(uint256 _amountBase) public onlyOwnerOrSwapTradeContract {
        require(_amountBase > 0, "INVALID_AMOUNT_BASE");
        require(_amountBase <= base.balanceOf(address(this)));
        require(turingswapTrade != address(0), "INVALID_TRADE_ADDRESS");
        base.transfer(turingswapTrade, _amountBase);
    }

    function moveOutTokenToTradeContract(uint256 _amountToken) public onlyOwnerOrSwapTradeContract {
        require(_amountToken > 0, "INVALID_AMOUNT_BASE");
        require(_amountToken <= token.balanceOf(address(this)));
        require(turingswapTrade != address(0), "INVALID_TRADE_ADDRESS");
        token.transfer(turingswapTrade, _amountToken);
    }

    function releaseFundToTradeContract() public onlyOwnerOrSwapTradeContract {
        require(turingswapTrade != address(0), "INVALID_TRADE_ADDRESS");
        _withdrawAll();
        uint256 baseReserve = base.balanceOf(address(this));
        uint256 tokenReserve = token.balanceOf(address(this));

        if (baseReserve > 0) {
            base.transfer(turingswapTrade, baseReserve);
        }
        if (tokenReserve > 0) {
            token.transfer(turingswapTrade, tokenReserve);
        }   
    }

    function getXVSApy() public view returns(uint256) {
        uint256 totalSupply = IBEP20(turingswapTrade).totalSupply();
        if (totalSupply <= 0) {
            return 0;
        }
        if (timeOfHarvest <=0) { 
            return 0;
        }
        if (lasttimeOfHarvest <=0) { 
            return 0;
        }
        if (timeOfHarvest <= lasttimeOfHarvest) {
            return 0;
        }
        // PERIOD_DAY
        // daily = (xvsRewardAmt / (timeOfHarvest - lasttimeOfHarvest))/ totalSupply
        // => APY = daily * 365
        // => APY =  365 * (xvsRewardAmt / (timeOfHarvest - lasttimeOfHarvest))/ totalSupply
        return xvsRewardAmt.mul(PERIOD_DAY).mul(365).mul(1e12).div(timeOfHarvest.sub(lasttimeOfHarvest)).div(totalSupply.mul(2));
    }
    function getSupplyApy() public view returns(uint256) {
        uint256 baseBalOnVenus  = 0;
        uint256 tokenBalOnVenus = 0;
        (, uint256 vBaseBal, ,uint256 exchangeRateMantissaOfVBase) = vBase.getAccountSnapshot(address(this));
        (, uint256 vTokenBal, ,uint256 exchangeRateMantissaOfVToken) = vToken.getAccountSnapshot(address(this));
        baseBalOnVenus = vBaseBal.mul(exchangeRateMantissaOfVBase).div(1e18);
        tokenBalOnVenus = vTokenBal.mul(exchangeRateMantissaOfVToken).div(1e18);
        uint256 totalBalOnVenus = baseBalOnVenus.add(tokenBalOnVenus);
        if (totalBalOnVenus <= totalBaseSupplyToVenus.add(totalTokenSupplyToVenus)) {
            return 0;
        }
        if (block.timestamp <= timeOfSupplyToVenus) {
            return 0;
        }
        uint256 totalSupply = IBEP20(turingswapTrade).totalSupply();
        uint256 supplyRewardAmt = totalBalOnVenus.sub(totalBaseSupplyToVenus).sub(totalTokenSupplyToVenus);
        return supplyRewardAmt.mul(PERIOD_DAY).mul(365).mul(1e12).div(block.timestamp.sub(timeOfSupplyToVenus)).div(totalSupply.mul(2));
    }
}