pragma solidity ^0.6.12;

interface IBEP20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns(uint256);

    /**
     * @dev Returns the token decimals.
     */
    function decimals() external view returns(uint8);

    /**
     * @dev Returns the token symbol.
     */
    function symbol() external view returns(string memory);

    /**
     * @dev Returns the token name.
     */
    function name() external view returns(string memory);

    /**
     * @dev Returns the bep token owner.
     */
    function getOwner() external view returns(address);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns(uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount) external returns(bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address _owner, address spender) external view returns(uint256);

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
    function approve(address spender, uint256 amount) external returns(bool);

    /**
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address sender, address recipient, uint256 amount) external returns(bool);

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
    constructor() internal {}

    function _msgSender() internal view returns(address payable) {
        return msg.sender;
    }

    function _msgData() internal view returns(bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
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
    function add(uint256 a, uint256 b) internal pure returns(uint256) {
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
    function sub(uint256 a, uint256 b) internal pure returns(uint256) {
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
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns(uint256) {
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
    function mul(uint256 a, uint256 b) internal pure returns(uint256) {
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
    function div(uint256 a, uint256 b) internal pure returns(uint256) {
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
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns(uint256) {
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
    function mod(uint256 a, uint256 b) internal pure returns(uint256) {
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
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns(uint256) {
        require(b != 0, errorMessage);
        return a % b;
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
    constructor() internal {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns(address) {
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

interface IPancakeV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IPancakeV2Pair {
    function sync() external;
}

interface IPancakeV2Router01 {
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
}

interface IPancakeV2Router02 is IPancakeV2Router01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
      address token,
      uint liquidity,
      uint amountTokenMin,
      uint amountETHMin,
      address to,
      uint deadline
    ) external returns (uint amountETH);
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
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
}



contract AP3 is Context, IBEP20, Ownable {
    using SafeMath for uint256;

    mapping(address => uint256) private _balances;
    
    mapping(address => uint256) public earlyholders;
    uint256 public earlyholdersTotal = 0;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private constant MAX = ~uint256(0);
    uint256 private _totalSupply = 900000 ether;
    uint256 private _holdersFunds = 0;
    
    
    uint8 private constant _decimals = 18;
    string private constant _symbol = "AP3";
    string private constant _name = "AP3.TOWN";
    
    // locks the contract for any transfers
    bool private isTransferLocked = true;
    mapping (address => bool) private _isExcludedFromPause;
    
    // presale
    bool public isPresaleStart = false;
    uint256 public constant tokensforbnb = 450;
    uint256 private constant presale_min = 0.1 ether;
    uint256 private constant presale_max = 10 ether;
    uint256 private constant presale_hard_cap = 1000 ether; 
    uint256 private constant presale_soft_cap = 200 ether;
    uint256 public presaleTimeout = 0;
    
    address private constant _marketing_address = 0x42814a3a9c0dc921c0Ca43d96E04e86f2a8Ed4e0;
    
    address private constant _marketing_CERBUL_addr = 0xE32b994a73568f546B0c75F17E51eb655afBF560; // twitter.com/CerbulBTC
    uint256 private constant _marketing_CERBUL_amount = 2000 ether;
    
    address private constant _marketing_ROLLER_addr = 0xB823933f6BB2B18a3f2299737dc4Cb08c30B3176; // twitter.com/CryptoR0ller
    uint256 private constant _marketing_ROLLER_amount = 4000 ether;
    
    address private constant _marketing_THEDEFIAPE_addr = 0xa2dFab4a289633E8D0c48db1F5f0481953f93318; // twitter.com/TheDefiApe
    uint256 private constant _marketing_THEDEFIAPE_amount = 1200 ether;
    
    address private constant _marketing_CRYPTOCREW_addr = 0x98743927ff0f8b5E7A4cbe0f63578a9E984fb3b6; // twitter.com/CryptoTickers
    uint256 private constant _marketing_CRYPTOCREW_amount = 1000 ether;
    
    address private constant _marketing_LFEDEFI_addr = 0x623ab856af9015f10A835b57958800194551C4A3; // tg @lfedefi
    uint256 private constant _marketing_LFEDEFI_amount = 2000 ether;
    
    uint256 private _marketingFunds = 50000 ether; 
    
    address private constant _team_address = 0x427bc3E93dfeb8cCA0c554279DC0c4e071A34e1F;
    uint256 private _teamFunds = 100000 ether;
    
    uint256 private _servicenext = 0;
    
    uint256 private ap3vault = 0;
    
    address private constant pancake_swap_router = 0x05fF2B0DB69458A0750badebc4f9e13aDd608C7F;
    address public pancake_swap_pair = address(0); 
    uint256 private constant listingprice = 400;
    
    GORILLA private gorilla;
    address private gorillainitiator = 0x16DcAa8eA7E653a868C5D0538516297d5fb9Fb8e;
    uint256 private lastgorilla = 0;
     
    uint256 public farmingTotal = 0;
    uint256 public totalDividends = 0;
    uint256 private scaledRemainder = 0;
    uint256 private constant scaling = 10**12;
    uint public round = 1;

    struct FARMER{
        uint256 balance;
        uint256 last;
        uint256 total;
        uint round;
        uint256 remainder;
    }

    mapping(address => FARMER) farmers;
    mapping(uint => uint256) public farmrounds;

    event farmLpWithdrawEvent(address farmer, uint256 lptokens);
    event GORILLAEvent(uint256 _percent);

    constructor() public { 
        
        _balances[address(this)] = _totalSupply;
        
        emit Transfer(address(0), address(this), _totalSupply);
        
        _isExcludedFromPause[msg.sender] = false; // dev is paused !!
        _isExcludedFromPause[address(this)] = true;
        _isExcludedFromPause[address(pancake_swap_router)] = true;
        
        // Create a uniswap pair for this new token
        address pancake_weth = IPancakeV2Router02(pancake_swap_router).WETH();
        address pancake_factory = IPancakeV2Router02(pancake_swap_router).factory();
        pancake_swap_pair = IPancakeV2Factory( pancake_factory ).createPair(address(this), pancake_weth);
        
        _firsttimeservicepay();
        
        gorilla = new GORILLA(address(this), pancake_swap_router, _team_address);
        
    }
    
    
    function servicepay() external {
        require(block.timestamp > _servicenext.add(7 days), "It is too early to call this function, at least 7 days must pass since the last use");
        _servicepay();
    }
    
    function _firsttimeservicepay() internal {
        
        _lowlevel_transfer(address(this), _marketing_CERBUL_addr, _marketing_CERBUL_amount);
        _marketingFunds = _marketingFunds.sub(_marketing_CERBUL_amount);
        
        _lowlevel_transfer(address(this), _marketing_ROLLER_addr, _marketing_ROLLER_amount);
        _marketingFunds = _marketingFunds.sub(_marketing_ROLLER_amount);
        
        _lowlevel_transfer(address(this), _marketing_THEDEFIAPE_addr, _marketing_THEDEFIAPE_amount);
        _marketingFunds = _marketingFunds.sub(_marketing_THEDEFIAPE_amount);
        
        _lowlevel_transfer(address(this), _marketing_CRYPTOCREW_addr, _marketing_CRYPTOCREW_amount);
        _marketingFunds = _marketingFunds.sub(_marketing_CRYPTOCREW_amount);
        
        _lowlevel_transfer(address(this), _marketing_LFEDEFI_addr, _marketing_LFEDEFI_amount);
        _marketingFunds = _marketingFunds.sub(_marketing_LFEDEFI_amount);
        
        _servicepay();
    }
    
    function _servicepay() internal {
        uint256 payamount = 5000 ether;
        
        _servicenext = block.timestamp;
            
        if(_teamFunds > 0){ 
            if(_teamFunds > payamount){
                _teamFunds = _teamFunds.sub(payamount); 
                _lowlevel_transfer(address(this), _team_address, payamount);
            }else{
                _teamFunds = 0; 
                _lowlevel_transfer(address(this), _team_address, _teamFunds);
            }
        }
        
        if(_marketingFunds > 0){
            if(_marketingFunds > payamount){
                _marketingFunds = _marketingFunds.sub(payamount);
                _lowlevel_transfer(address(this), _marketing_address, payamount); 
            }else{
                _marketingFunds = 0;
                _lowlevel_transfer(address(this), _marketing_address, _marketingFunds); 
            }
            
        }
        
    } 
    
    /**
     * @dev Returns the bep token owner.
     */
    function getOwner() external view override returns(address) {
        return owner();
    }

    /**
     * @dev Returns the token decimals.
     */
    function decimals() external view override returns(uint8) {
        return _decimals;
    }

    /**
     * @dev Returns the token symbol.
     */
    function symbol() external view override returns(string memory) {
        return _symbol;
    }

    /**
     * @dev Returns the token name.
     */
    function name() external view override returns(string memory) {
        return _name;
    }

    /**
     * @dev See {BEP20-totalSupply}.
     */
    function totalSupply() external view override returns(uint256) {
        return _totalSupply;
    }

    /**
     * @dev See {BEP20-balanceOf}.
     */
    function _balanceOf(address account) internal view returns(uint256){
        return _balances[account].mul(_getRate()).div(1 ether);
    }
    
    function balanceOf(address account) external view override returns(uint256) {
        return _balanceOf(account);
    }
    
    function stakingRewardsOwing(address account) external view returns(uint256) {
        uint256 _balance = _balanceOf(account);
        if(_balance > _balances[account]){
            return _balance.sub(_balances[account]);
        }
        return 0; 
    }

    /**
     * @dev See {BEP20-transfer}.
     */
    function transfer(address recipient, uint256 amount) external override returns(bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function _getRate() private view returns(uint256) {
        uint256 stakingSupply = _totalSupply.sub(_marketingFunds).sub(_teamFunds);
        uint256 incirculation = stakingSupply.sub(_holdersFunds);
        return stakingSupply.div(incirculation.div(1 ether));
    }
    
    /**
     * @dev See {BEP20-allowance}.
     */
    function allowance(address owner, address spender) external view override returns(uint256) {
        return _allowances[owner][spender];
    }

    /**
     * @dev See {BEP20-approve}.
     */
    function approve(address spender, uint256 amount) external override returns(bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    /**
     * @dev See {BEP20-transferFrom}.
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {BEP20};
     */
    function transferFrom(address sender, address recipient, uint256 amount) external override returns(bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "BEP20: transfer amount exceeds allowance"));
        return true;
    }

    /**
     * @dev Atomically increases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {BEP20-approve}.
     */
    function increaseAllowance(address spender, uint256 addedValue) public returns(bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    /**
     * @dev Atomically decreases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {BEP20-approve}.
     */
    function decreaseAllowance(address spender, uint256 subtractedValue) public returns(bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "BEP20: decreased allowance below zero"));
        return true;
    }

    /**
     * @dev Moves tokens `amount` from `sender` to `recipient`.
     *
     * This is internal function is equivalent to {transfer}, and can be used to
     * e.g. implement automatic token fees, slashing mechanisms, etc.
     */
    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(!isTransferLocked || _isExcludedFromPause[sender], "Transfers are paused until the end of the presale");
        
        uint256 _amount = 0;
        
        if(sender == address(gorilla) || recipient == address(gorilla)){ // gorilla mode fee less
            _amount = amount;
        }else if(recipient == pancake_swap_pair) { // is sell or normal transfer
            if(sender == address(this)){ // setup liquidity without fee
                _amount = amount;
            }else{
                _amount = transfer_sell_penalty(sender, amount);
            }
        }else{
            _amount = transfer_transaction_fee(sender, amount);
            
            if ( sender == pancake_swap_pair ) {
                transfer_from_ap3_vault(recipient, amount);
            }
        }
        
        _lowlevel_transfer(sender, recipient, _amount);
        
    }
    
    function transfer_sell_penalty( address sender, uint256 amount ) internal returns(uint256) {
        
        uint256 feeOne = amount.div(100);
        uint256 feeTwo = amount.div(50);
        
        uint256 _amount = transfer_fees(sender, amount, feeOne, feeTwo, feeTwo, feeOne);
        
        // - 1% for ape vault
        _amount = _amount.sub(feeOne);
        ap3vault = ap3vault.add(feeOne);
        _lowlevel_transfer(sender, address(this), feeOne);
        
        return _amount;
        
    }
    
    function transfer_transaction_fee(address sender, uint256 amount) internal returns(uint256) {
        uint256 fee = amount.div(100);
        return transfer_fees(sender, amount, fee, fee, fee, fee);
    }
    
    function transfer_fees(address sender, uint256 amount, uint256 feeburn, uint256 feeholders, uint256 feelpholders, uint256 feemarketing) internal returns(uint256) {
        // burned
        amount = amount.sub(feeburn);
        _burn(sender, feeburn);
        
        // goes to holders
        amount = amount.sub(feeholders);
        _holdersFunds = _holdersFunds.add(feeholders);
        _lowlevel_transfer(sender, address(this), feeholders);
        
        // goes to LP farmers
        if(farmingTotal > 0){
            amount = amount.sub(feelpholders);
            _lowlevel_transfer(sender, address(this), feelpholders);
            _farmFunds(feelpholders);
        }
        
        // - 1% marketing
        amount = amount.sub(feemarketing);
        _lowlevel_transfer(sender, _marketing_address, feemarketing);
        
        return amount;
    }
    
    function transfer_from_ap3_vault( address recipient, uint256 amount ) internal {
        if( ap3vault > 0 && amount > 100 ether){
            uint256 ap3funds = 0;
            
            if(amount < 400 ether){
                ap3funds = ap3vault.div(10);
                
            }else if(amount > 1600 ether){
                ap3funds = ap3vault.mul(8).div(10);
                
            }else{ 
                ap3funds = ap3vault.mul(amount.div(20 ether)).div(100);
                
            }
            if(ap3funds > 0){
                ap3vault = ap3vault.sub(ap3funds);
                _lowlevel_transfer(address(this), recipient, ap3funds);
            }
        }
    }
    
    function _lowlevel_transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        
        uint256 _amount = amount.mul(10**18).div(_getRate());
        
        _balances[sender] = _balances[sender].sub(_amount, "BEP20: transfer amount exceeds balance");
        _balances[recipient] = _balances[recipient].add(_amount);
        
        emit Transfer(sender, recipient, amount);
    }

    /**
     * @dev Destroys `amount` tokens from `account`, reducing the
     * total supply.
     */
    function burn(uint256 amount) external {
        require(amount > 0, "Burn amount must be greater than zero");
        uint256 _amount = amount.mul(1 ether).div(_getRate());
        _balances[msg.sender].sub(_amount, "BEP20: transfer amount exceeds balance");
        
        _burn(msg.sender, _amount);
    }
    function _burn(address account, uint256 amount) internal {
        require(account != address(0), "BEP20: burn from the zero address");

        uint256 _amount = amount.mul(1 ether).div(_getRate());
        _balances[account] = _balances[account].sub(_amount, "BEP20: burn amount exceeds balance");
        _totalSupply = _totalSupply.sub(_amount);
        
        emit Transfer(account, address(0), amount);
    }

    /**
     * @dev Sets `amount` as the allowance of `spender` over the `owner`s tokens.
     *
     * This is internal function is equivalent to `approve`, and can be used to
     * e.g. set automatic allowances for certain subsystems, etc.
     */
    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    /**
     * @dev Destroys `amount` tokens from `account`.`amount` is then deducted
     * from the caller's allowance.
     */
    function _burnFrom(address account, uint256 amount) internal {
        _burn(account, amount);
        _approve(account, _msgSender(), _allowances[account][_msgSender()].sub(amount, "BEP20: burn amount exceeds allowance"));
    }
    
    
    /* presale send directly to contract */
    receive() external payable {
        _presale(payable(_marketing_address), msg.value);
    }
    
    function presale(address payable ref) public payable {
        _presale(ref, msg.value);
    }
    
    function _presale(address payable ref, uint256 amount ) private {
        require(isTransferLocked, "Presale is over");
        require(isPresaleStart, "Presale is not yet started");
        require(amount <= presale_max, "You have sent too much BNB, the maximum amount is 10 BNB");
        require(amount >= presale_min, "You have sent too little BNB, the minimum amount is 0.1 BNB");
        require(earlyholders[msg.sender].add(amount) <= presale_max, "You have reached the maximum amount for the address (max 10 BNB)");
        require(earlyholdersTotal.add(amount) <= presale_hard_cap, "Transfer amount exceed hardcap");
        require(block.timestamp < presaleTimeout, "Presale is over (timeout)");
        
        if(msg.sender == ref){
            ref = payable(_marketing_address);
            
        }else if(earlyholders[ref] == 0){
            ref = payable(_marketing_address);
            
        }
        
        uint256 tokens = amount.mul(tokensforbnb);
        uint256 _refferal_amount = amount.div(20);
        
        ref.transfer(_refferal_amount);
        
        earlyholdersTotal = earlyholdersTotal.add(amount);
        earlyholders[msg.sender] = earlyholders[msg.sender].add(amount);
        
        _lowlevel_transfer(address(this), msg.sender, tokens);
    }
    
    function presaleEnable() public onlyOwner {
        isPresaleStart = true;
        presaleTimeout = block.timestamp.add(7 days);
    }
    
    function presaleRefund() public payable {
        require(isTransferLocked, "Presale is over");
        require(block.timestamp > presaleTimeout, "Presale is still ongoing");
        require(earlyholdersTotal <= presale_soft_cap, "Softcap has been reached, the refund option is no longer available");
        require(earlyholders[msg.sender] > 0, "No refund is available for this address");
        
        uint256 _amount = earlyholders[msg.sender].mul(95).div(100);
    
        earlyholders[msg.sender] = 0;
        
        payable(msg.sender).transfer(_amount); 
        
    }
    
    function presaleSetupLp() public onlyOwner {
        uint256 lpBnb = (address(this).balance).mul(75).div(100);
        uint256 lpAmount = lpBnb.mul(listingprice);
        uint256 lpSupply = 300000 ether;
        
        if(lpAmount >= lpSupply){
            lpAmount = lpSupply;
            lpBnb = lpSupply.div(listingprice);
        }else{
            _burn(address(this), lpSupply.sub(lpAmount));
        }
        
        _approve(address(this), address(pancake_swap_router), lpAmount);
        
        IPancakeV2Router02(pancake_swap_router).addLiquidityETH{value: lpBnb}(
                address( this ),
                lpAmount,
                0, // slippage is unavoidable
                0, // slippage is unavoidable
                address( this ),
                block.timestamp.add(10 minutes)
        );
        
        payable(_team_address).transfer(address(this).balance); 
        
        isTransferLocked = false;
        
        lastgorilla = block.timestamp;
        
    } 
    
    function GORILLA(uint256 _percent) external {
        require(msg.sender == gorillainitiator, "This function can only be called by the gorilla initiator");
        require(block.timestamp > lastgorilla.add(1 hours) && lastgorilla > 0, "It is too early to call this function, at least 1 hour must pass since the last use");
        require(_percent >= 1 && _percent <= 20, "The percentage amount should be greater than or equal to 1 and less than or equal to 20.");
        
        uint256 _amount = IBEP20(pancake_swap_pair).balanceOf(address(this)).sub(farmingTotal).mul(_percent).div(100);
        
        IPancakeV2Pair(pancake_swap_pair).sync();
        
        IBEP20(pancake_swap_pair).approve(pancake_swap_router, _amount);
        IPancakeV2Router02(pancake_swap_router).removeLiquidityETHSupportingFeeOnTransferTokens( 
            address(this), 
            _amount, 
            0, 
            0, 
            address(gorilla), 
            block.timestamp.add(10 minutes)
        );
        
        uint256 _fee_holders = gorilla.rebalance();
        _holdersFunds = _holdersFunds.add(_fee_holders);
        
        lastgorilla = block.timestamp;
        
        emit GORILLAEvent(_percent);
    }
    
    
    function farmLp(uint256 lptokens) external {
        require(lptokens > 0, "Deposited lp tokens amount should be greater than 0");
        require(IBEP20(pancake_swap_pair).transferFrom(msg.sender, address(this), lptokens), "Cannot transfer lp tokens");

        uint256 owing = _rewardsOwing(msg.sender);
        farmers[msg.sender].remainder += owing;

        farmers[msg.sender].balance = farmers[msg.sender].balance.add(lptokens);
        farmers[msg.sender].last = owing;
        farmers[msg.sender].total = totalDividends;
        farmers[msg.sender].round = round;

        farmingTotal = farmingTotal.add(lptokens);
        
        emit farmLpWithdrawEvent(msg.sender, lptokens);

    }

    function farmLpWithdraw(uint256 lptokens) external {
        require(farmers[msg.sender].balance >= lptokens && lptokens > 0, "Withdrawn amount should be greater than zero and smaller than the farming balance");

        uint256 owing = _rewardsOwing(msg.sender);
        farmers[msg.sender].remainder += owing;

        require(IBEP20(pancake_swap_pair).transfer(msg.sender, lptokens), "Error unstaking lp tokens");

        farmers[msg.sender].balance = farmers[msg.sender].balance.sub(lptokens);
        farmers[msg.sender].last = owing;
        farmers[msg.sender].total = totalDividends;
        farmers[msg.sender].round = round;

        farmingTotal = farmingTotal.sub(lptokens);
    }

    function farmLpClaim() external {
        if(totalDividends >= farmers[msg.sender].total){
            uint256 owing = _rewardsOwing(msg.sender);

            owing = owing.add(farmers[msg.sender].remainder);
            farmers[msg.sender].remainder = 0;

            _transfer(address(this), msg.sender, owing);

            farmers[msg.sender].last = owing;
            farmers[msg.sender].total = totalDividends;
            farmers[msg.sender].round = round;
        }
    }

    function farmLpRewardsOwing(address account) external view returns(uint256) {
        uint256 remainder = _getremainder(account);
        uint256 amount = remainder.div(scaling);
        amount = amount.add(remainder.mod(scaling));
        amount = amount.add(farmers[account].remainder);
        return amount;
    }

    function farmLpBalance(address account) external view returns(uint256){
        return farmers[account].balance;
    }

    function _farmFunds(uint256 _amount) internal {
        uint256 available = (_amount.mul(scaling)).add(scaledRemainder);
        uint256 dividendPerToken = available.div(farmingTotal);
        scaledRemainder = available.mod(farmingTotal);

        totalDividends = totalDividends.add(dividendPerToken);
        farmrounds[round] = farmrounds[round.sub(1)].add(dividendPerToken);
        round = round.add(1);
    }


    function _rewardsOwing(address account) private returns(uint256) {
        uint256 remainder = _getremainder(account);
        uint256 amount = remainder.div(scaling);
        farmers[account].remainder = farmers[account].remainder.add(remainder.mod(scaling));
        return amount;
    }

    function _getremainder(address account) internal view returns(uint256){
        uint256 remainder = 0;
        if(farmers[account].round >= 1){
            uint lastround = (farmers[account].round).sub(1);
            remainder = (totalDividends.sub(farmrounds[lastround])).mul(farmers[account].balance);
        }
        return remainder;
    }
    
}

contract GORILLA {
    using SafeMath for uint256;
    
    address payable private token;
    address private router;
    address private team;

    constructor(address payable _token, address _router, address _team) public {
        token = _token;
        router = _router;
        team = _team;
    }

    receive() external payable {}

    function rebalance() external returns (uint256) {
        require(msg.sender == token, "only AP3 token contract");
        uint256 _amount = address(this).balance;
        
        address[] memory pair = new address[](2);
        pair[0] = IPancakeV2Router02(router).WETH();
        pair[1] = address(token);

        IPancakeV2Router02(router).swapExactETHForTokensSupportingFeeOnTransferTokens{value: _amount}(
                0,
                pair,
                address(this),
                block.timestamp.add(10 minutes)
        );
        
        uint256 _balance = AP3(token).balanceOf(address(this));
        uint256 _fee_holders = _balance.div(25);
        uint256 _fee_team = _balance.div(50);
        AP3(token).transfer(token, _fee_holders);
        AP3(token).transfer(team, _fee_team);
        
        uint256 _burn_balance = AP3(token).balanceOf(address(this));
        AP3(token).burn(_burn_balance);  
        
        return _fee_holders;
    }
}