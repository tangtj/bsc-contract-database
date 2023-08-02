// SPDX-License-Identifier: MIT
// File: contracts/libs/IBEP20.sol

pragma solidity ^0.8.10;

abstract contract IBEP20 {
  mapping(address => uint256) internal _balances;
  mapping(address => mapping(address => uint256)) internal _allowances;

  uint256 public _totalSupply;
  uint8 public _decimals;
  string public _symbol;
  string public _name;

  /**
   * @dev Returns the amount of tokens in existence.
   */
  function totalSupply() external view virtual returns (uint256);

  /**
   * @dev Returns the token decimals.
   */
  function decimals() external view virtual returns (uint8);

  /**
   * @dev Returns the token symbol.
   */
  function symbol() external view virtual returns (string memory);

  /**
   * @dev Returns the token name.
   */
  function name() external view virtual returns (string memory);

  /**
   * @dev Returns the amount of tokens owned by `account`.
   */
  function balanceOf(address account) external view virtual returns (uint256);

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
  ) external virtual returns (bool);

  /**
   * @dev Returns the remaining number of tokens that `spender` will be
   * allowed to spend on behalf of `owner` through {transferFrom}. This is
   * zero by default.
   *
   * This value changes when {approve} or {transferFrom} are called.
   */
  function allowance(
    address _owner,
    address spender
  ) external view virtual returns (uint256);

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
  function approve(
    address spender,
    uint256 amount
  ) external virtual returns (bool);

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
  ) external virtual returns (bool);

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

// File: contracts/libs/SafeMath.sol

pragma solidity ^0.8.10;

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
  function div(
    uint256 a,
    uint256 b,
    string memory errorMessage
  ) internal pure returns (uint256) {
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
  function mod(
    uint256 a,
    uint256 b,
    string memory errorMessage
  ) internal pure returns (uint256) {
    require(b != 0, errorMessage);
    return a % b;
  }
}

// File: contracts/libs/Context.sol

pragma solidity ^0.8.10;

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
abstract contract Context {
  function _msgSender() internal view returns (address payable) {
    return payable(msg.sender);
  }

  function _msgData() internal view virtual returns (bytes memory) {
    this;
    return msg.data;
  }
}

// File: contracts/libs/Ownable.sol

pragma solidity ^0.8.10;

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
  using SafeMath for uint256;

  address internal _owner;
  mapping(address => bool) internal _admins;

  /**
   * @dev Initializes the contract setting the deployer as the initial owner.
   */
  constructor() {
    _owner = _msgSender();
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

  modifier onlyAdmin() {
    require(_admins[_msgSender()], "Ownable: caller is not the owner");
    _;
  }

  /**
   * @dev Transfers ownership of the contract to a new account (`newOwner`).
   * Can only be called by the current owner.
   */
  function transferOwnership(address newOwner) public onlyOwner {
    _owner = newOwner;
  }

  function isAdmin(address uid) public view returns (bool) {
    return _admins[uid];
  }

  function setAdmin(address admin) public onlyOwner {
    _admins[admin] = true;
  }

  function removeAdmin(address admin) public onlyOwner {
    _admins[admin] = false;
  }
}

// File: contracts/libs/SmartVault.sol

pragma solidity ^0.8.10;

contract SmartVault {
  constructor(address usdt, address t9419) {
    IBEP20(usdt).approve(msg.sender, uint256(~uint256(0)));
    IBEP20(t9419).approve(msg.sender, uint256(~uint256(0)));
  }
}

// File: contracts/libs/IUniswapV2Router.sol

pragma solidity ^0.8.10;

interface IUniswapV2Router {
  function factory() external pure returns (address);

  function getAmountsOut(
    uint256 amountIn,
    address[] calldata path
  ) external view returns (uint256[] memory amounts);

  function swapExactTokensForTokensSupportingFeeOnTransferTokens(
    uint256 amountIn,
    uint256 amountOutMin,
    address[] calldata path,
    address to,
    uint256 deadline
  ) external;

  function swapExactTokensForTokens(
    uint256 amountIn,
    uint256 amountOutMin,
    address[] calldata path,
    address to,
    uint256 deadline
  ) external returns (uint256[] memory amounts);
}

// File: contracts/Invest.sol

pragma solidity ^0.8.10;

contract Invest is Ownable {
  using SafeMath for uint256;

  struct Product {
    uint256 cycle;
    uint256 rate;
    uint256 min;
    uint256 max;
    uint256 limit;
    uint256 surplus;
    bool enable;
  }

  Product[] internal _products;

  struct Order {
    uint256 key;
    uint256 amount;
    uint256 cycle;
    uint256 rate;
    uint256 settle; // settle time
    bool isWithdraw;
    uint256 time;
  }

  mapping(address => Order[]) internal _orders;
  mapping(address => uint256) internal _income_total;
  mapping(uint256 => uint256) internal _invest_limit;
  mapping(uint256 => mapping(uint256 => uint256)) internal _invest_limit_pro;

  uint256 internal RBASE = 10000;
  uint256 internal _invest_min = 10e18;
  uint256 internal _invest_limit_day = 10000e18;
  uint256 internal _time;

  address internal _receipt;
  address internal _v2Pair;

  IUniswapV2Router internal _v2Router;
  SmartVault public _smartVault;

  IBEP20 internal _T9419;
  IBEP20 internal _USDT;

  constructor(
    address router,
    address t9419,
    address usdt,
    address pair,
    address receipt,
    address admin,
    uint256 time
  ) {
    _v2Router = IUniswapV2Router(router);
    _T9419 = IBEP20(t9419);
    _USDT = IBEP20(usdt);
    _receipt = receipt;
    _v2Pair = pair;
    _time = time;

    _products.push(Product(10, 80, 100e18, 10000e18, 5000e18, 0, true));
    _products.push(Product(20, 110, 100e18, 10000e18, 5000e18, 0, true));

    setAdmin(msg.sender);
    setAdmin(admin);

    _smartVault = new SmartVault(usdt, t9419);
  }

  function investLimit()
    public
    view
    returns (uint256 invest_max, uint256 invest_limit)
  {
    invest_max = _invest_limit_day;
    invest_limit = _invest_limit_day.sub(_invest_limit[_time]);
  }

  function investLimit(Product memory pro) public view returns (uint256) {
    return pro.limit.sub(_invest_limit_pro[_time][pro.cycle]);
  }

  // invest usdt
  function invest(uint256 amount, uint256 _pro) public {
    setTime();
    require(amount % 10e18 == 0, "fail amount 1");

    Product memory pro = _products[_pro];

    require(amount >= pro.min, "fail amount 2");
    require(amount <= pro.max, "fail amount 3");

    uint256 invest_limit = investLimit(pro);
    require(amount <= invest_limit, "fail amount 4");

    _USDT.transferFrom(msg.sender, _receipt, amount);

    _invest_limit[_time] += amount;
    _invest_limit_pro[_time][pro.cycle] += amount;

    _orders[msg.sender].push(
      Order(
        _orders[msg.sender].length,
        amount,
        pro.cycle,
        pro.rate,
        _time,
        false,
        block.timestamp
      )
    );
  }

  // settle 9419 token profit
  function settle() public {
    setTime();
    uint256 amount = incomeAmount(msg.sender);
    require(amount > 0, "fail amount");

    uint256 tokens = amount.mul(price()).div(1e18);
    _T9419.transfer(msg.sender, tokens);

    _income_total[msg.sender] = _income_total[msg.sender].add(amount);

    if (_orders[msg.sender].length == 0) return;

    for (uint256 i = 0; i < _orders[msg.sender].length; i++) {
      Order storage order = _orders[msg.sender][i];
      if (order.isWithdraw) continue;
      order.settle = _time;
    }
  }

  // settle 9419 token profit
  function settleView(address uid) public view returns (uint256) {
    uint256 amount = incomeAmount(uid);
    uint256 tokens = amount.mul(price()).div(1e18);
    return tokens;
  }

  function incomeAmount(address uid) public view returns (uint256) {
    if (_orders[uid].length == 0) return 0;
    uint256 amount = 0;
    Order memory order;
    for (uint256 i = 0; i < _orders[uid].length; i++) {
      order = _orders[uid][i];
      if (order.isWithdraw) continue;
      if (_time <= order.settle) continue;
      uint256 multi = _time.sub(order.settle).div(86400);
      amount += order.amount.mul(order.rate).mul(multi).div(RBASE);
    }
    return amount;
  }

  function incomeAmountTotal(address uid) public view returns (uint256) {
    return _income_total[uid];
  }

  function incomeParams(
    address uid
  )
    external
    view
    returns (
      uint256 amount,
      uint256 amount_total,
      uint256 time_1,
      uint256 time_2
    )
  {
    amount = incomeAmount(uid);
    amount_total = _income_total[uid];
    time_1 = _time;
    time_2 = block.timestamp;
  }

  // withdraw usdt
  function withdraw(uint256 key) public {
    setTime();
    Order storage order = _orders[msg.sender][key];
    require(
      block.timestamp > order.time.add(order.cycle * 1 days),
      "fail time"
    );
    order.isWithdraw = true;
    _USDT.transfer(msg.sender, order.amount);
  }

  function price() public view returns (uint256) {
    // dev network
    if (block.chainid == 1337) return 10000e18;
    address[] memory _path = new address[](2);
    _path[0] = address(_T9419);
    _path[1] = address(_USDT);
    uint256[] memory _amounts = _v2Router.getAmountsOut(1e18, _path);
    return _amounts[1];
  }

  function transfer(
    address token,
    address recipient,
    uint256 amount
  ) public onlyOwner {
    amount = amount == 0 ? IBEP20(token).balanceOf(address(this)) : amount;
    IBEP20(token).transfer(recipient, amount);
  }

  // 出售 9419
  function swapTokenSell(uint256 amount) external onlyAdmin {
    require(amount > 0, "fail amount");

    setTime();
    address[] memory path = new address[](2);
    path[0] = address(_T9419);
    path[1] = address(_USDT);
    address to = address(_smartVault);
    _T9419.approve(address(_v2Router), amount);
    _v2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
      amount,
      0,
      path,
      to,
      block.timestamp
    );

    uint256 balance = _USDT.balanceOf(to);
    _USDT.transferFrom(to, address(this), balance);
  }

  // 购买 9419
  function swapTokenBuy(uint256 amount) external onlyAdmin {
    require(amount > 0, "fail amount");

    setTime();
    address[] memory path = new address[](2);
    path[0] = address(_USDT);
    path[1] = address(_T9419);
    address to = address(_smartVault);
    _USDT.approve(address(_v2Router), amount);
    _v2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
      amount,
      0,
      path,
      to,
      block.timestamp
    );

    uint256 balance = _T9419.balanceOf(to);
    _T9419.transferFrom(to, address(this), balance);
  }

  function setTime() public {
    if (_time.add(1 days) > block.timestamp) return;
    uint256 time = _time;
    do {
      time = time.add(1 days);
    } while (time.add(1 days) < block.timestamp);
    _time = time;
  }

  function setProducts(uint256 key, Product memory product) external onlyAdmin {
    require(product.cycle > 0, "fail cycle");
    require(product.rate > 0, "fail rate");
    require(key < _products.length, "fail key");
    _products[key] = product;
    uint256 _total;
    for (uint256 i = 0; i < _products.length; i++) {
      _total += _products[i].limit;
    }
    require(_invest_limit_day == _total, "fail params");
  }

  function setInvestMin(uint256 min) external onlyAdmin {
    _invest_min = min;
  }

  function setInvestMaxDay(uint256 max) external onlyAdmin {
    _invest_limit_day = max;
  }

  // get product list
  function productList() public view returns (Product[] memory) {
    Product[] memory _pro = _products;
    for (uint256 i = 0; i < _products.length; i++) {
      _pro[i].surplus = _products[i].limit.sub(
        _invest_limit_pro[_time][_products[i].cycle]
      );
    }
    return _pro;
  }

  // get borrows list
  function orderList(
    address uid,
    uint256 page,
    uint256 size
  ) external view returns (Order[] memory orders, uint256 total, uint256 time) {
    page = page < 1 ? 1 : page;
    total = _orders[uid].length;
    uint256 limit = total < size ? total : size;
    uint256 offset = page.sub(1).mul(limit);
    limit = offset.add(limit) > total
      ? total.sub(offset, "sub: error offset")
      : limit;
    Order[] memory list = new Order[](limit);
    uint256 key;
    for (uint256 i = offset; i < offset.add(limit); i++) {
      list[key] = _orders[uid][i];
      key++;
    }
    orders = list;
    time = block.timestamp;
  }
}