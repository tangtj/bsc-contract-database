// SPDX-License-Identifier: MIT
// Contract Author: UPD :)

pragma solidity 0.5.16;

interface IERC20 {
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
    * @dev Returns the ERC token owner.
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

contract Ownable is Context {
    address private _developers;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
    * @dev Initializes the contract setting the deployer as the initial owner.
    */
    constructor () internal {
        address msgSender = _msgSender();
        _developers = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
    * @dev Returns the address of the current owner.
    */
    function developers() public view returns (address) {
        return _developers;
    }

    /**
    * @dev Throws if called by any account other than the owner.
    */
    modifier onlyOwner() {
        require(_developers == _msgSender(), "Ownable: caller is not the owner");
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
        emit OwnershipTransferred(_developers, address(0));
        _developers = address(0);
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
        emit OwnershipTransferred(_developers, newOwner);
        _developers = newOwner;
    }
}

contract UPD is Context, IERC20, Ownable {
    using SafeMath for uint256;

    mapping (address => uint256) private _balances;
    mapping (address => uint256) private _tetherBalances;
    mapping (address => mapping (address => uint256)) private _allowances;

    string private _symbol;
    string private _name;
    uint8 private _decimals;
    uint256 private _tetherLiquidity;
    uint256 private _totalSupply;
    uint256 private _price;
    IERC20 public token;

    constructor() public {
        _name = "UPD Token: Without Price Reduction";
        _symbol = "UPD";
        _decimals = 18;
        _tetherLiquidity = 1000000000000000000; // 1 Tether
        _totalSupply = 1000000000000000000000; // 1000 UPD
        _price = _tetherLiquidity / _totalSupply; // 0.001 Tether
        _balances[msg.sender] = _totalSupply;
        token = IERC20(0x55d398326f99059fF775485246999027B3197955);

        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    function getOwner() external view returns (address) {
        return developers();
    }

    function name() external view returns (string memory) {
        return _name;
    }

    function symbol() external view returns (string memory) {
        return _symbol;
    }

    function decimals() external view returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view returns (uint256) {
        return _totalSupply;
    }

    function tetherLiquidity() public view returns (uint256) {
        return _tetherLiquidity;
    }

    function price() public view returns (uint256) {
        return SafeMath.div( SafeMath.mul(_tetherLiquidity, 10 ** uint256(_decimals)), _totalSupply);
    }

    function balanceOf(address account) public view returns (uint256) {
        return _balances[account];
    }

    function balanceOfTether(address account) public view returns (uint256) {
        return _tetherBalances[account];
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "Approve from the zero address");
        require(spender != address(0), "Approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function allowance(address owner, address spender) external view returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "Decreased allowance below zero"));
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "Transfer from the zero address");
        require(recipient != address(0), "Transfer to the zero address");

        _balances[sender] = _balances[sender].sub(amount, "Transfer amount exceeds balance");
        _balances[recipient] = _balances[recipient].add(amount);
        
        emit Transfer(sender, recipient, amount);
    }

    function transfer(address recipient, uint256 _amount) external returns (bool) {
        require(_balances[_msgSender()] >= SafeMath.div( SafeMath.mul(_amount, 101), 100), "");

        uint256 recivedAmount = SafeMath.div( SafeMath.mul(_amount, 101), 100);
        uint256 sentAmount = SafeMath.div( SafeMath.mul(_amount, 98), 100);
        uint256 developersAmount = SafeMath.div( _amount, 100);
        uint256 burnAmount = SafeMath.div( SafeMath.mul(_amount, 2), 100);

        _transfer(_msgSender(), address(this), recivedAmount);
        _transfer(address(this), recipient, sentAmount);
        _transfer(address(this), developers(), developersAmount);

        emit Transfer(address(this), address(0), burnAmount);
        _balances[address(this)] = _balances[address(this)].sub(burnAmount);
        _totalSupply = _totalSupply.sub(burnAmount);

        return true;
    }

    function transferFrom(address sender, address recipient, uint256 _amount) external returns (bool) {
        require(_balances[sender] >= SafeMath.div( SafeMath.mul(_amount, 101), 100), "");

        uint256 recivedAmount = SafeMath.div( SafeMath.mul(_amount, 101), 100);
        uint256 sentAmount = SafeMath.div( SafeMath.mul(_amount, 98), 100);
        uint256 developersAmount = SafeMath.div( _amount, 100);
        uint256 burnAmount = SafeMath.div( SafeMath.mul(_amount, 2), 100);

        _transfer(sender, address(this), recivedAmount);
        _transfer(address(this), recipient, sentAmount);
        _transfer(address(this), developers(), developersAmount);

        emit Transfer(address(this), address(0), burnAmount);     
        _balances[address(this)] = _balances[address(this)].sub(burnAmount);   
        _totalSupply = _totalSupply.sub(burnAmount);

        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(_amount, "Transfer amount exceeds allowance"));
        return true;
    }

    // Buy token = Mint new token
    function buy(uint256 _amount) external payable {
        // _amount is Tether
        require(_msgSender() != address(0), "Mint to the zero address");
        require(token.allowance(_msgSender(), address(this)) >= _amount, "Token allowance not enough");
        require(token.balanceOf(_msgSender()) >= _amount, "Insufficient token balance");
        token.transferFrom(_msgSender(), address(this), _amount);
        mintToken(_amount);
        fee(_amount);
    }

    function mintToken(uint256 _amount) private {
        // _amount is Tether
        uint256 mintAmount = SafeMath.div(SafeMath.mul( SafeMath.div(SafeMath.mul(_amount, 98), 100), _totalSupply) , _tetherLiquidity);
        _totalSupply = _totalSupply.add(mintAmount);
        _tetherLiquidity = _tetherLiquidity.add( SafeMath.div( SafeMath.mul(_amount, 995), 1000) );
        _balances[_msgSender()] = _balances[_msgSender()].add(mintAmount);
        emit Transfer(address(0), _msgSender(), mintAmount);
    }

    function fee(uint256 _amount) private {
        // _amount is Tether
        _tetherBalances[developers()] = _tetherBalances[developers()].add( SafeMath.div( SafeMath.mul(_amount, 5), 1000) );
    }

    // Sell token = Burn token
    function sell(uint256 _amount) public {
        // _amount is UPD
        require(_msgSender() != address(0), "Burn from the zero address!");
        require(_amount > 0, "Amount should be greater than zero.");
        require(_balances[_msgSender()] >= _amount, "Insufficient balance.");
        emit Transfer(_msgSender(), address(this), _amount);
        transferTether(_amount);
        burnToken(_amount);
    }

    // Transfer tether to seller
    function transferTether(uint256 _amount) private {
        // _amount is UPD
        uint256 tetherAmount = SafeMath.div(SafeMath.mul( _amount, _tetherLiquidity), _totalSupply);
        require(token.transfer(_msgSender(), SafeMath.div( SafeMath.mul(tetherAmount, 99), 100)), "Token transfer failed");
        _tetherLiquidity = _tetherLiquidity.sub( SafeMath.div( SafeMath.mul(tetherAmount, 995), 1000) );
        fee(tetherAmount);
    }

    // Burn seller's tokens
    function burnToken(uint256 _amount) private {
        // _amount is UPD
        emit Transfer(address(this), address(0), _amount);
        _balances[_msgSender()] = _balances[_msgSender()].sub(_amount);
        _totalSupply = _totalSupply.sub(_amount);
    }

    // Developers: Withdraw fee
    function withdraw(uint256 _amount) public {
        // _amount is Tether
        require(_amount > 0, "Amount should be greater than zero.");
        require(_tetherBalances[_msgSender()] >= _amount, "Insufficient balance.");
        _tetherBalances[_msgSender()] = _tetherBalances[_msgSender()].sub(_amount);
        require(token.transfer(_msgSender(), _amount), "Token transfer failed");
    }

    // Add projects Profit to liquidity
    function projectsProfit(uint256 _amount) external payable {
        // _amount is Tether
        require(token.allowance(_msgSender(), address(this)) >= _amount, "Token allowance is not enough.");
        require(token.balanceOf(_msgSender()) >= _amount, "Insufficient token balance.");
        require(_amount > 0, "Amount should be greater than zero.");
        token.transferFrom(_msgSender(), address(this), _amount);
        _tetherLiquidity = _tetherLiquidity.add(_amount);
    }
}