// SPDX-License-Identifier: MIT
pragma solidity 0.8.17;


// CAUTION
// This version of SafeMath should only be used with Solidity 0.8 or later,
// because it relies on the compiler's built in overflow checks.
library SafeMath {
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
}


// Interface of the ERC20 standard as defined in the EIP.
interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address from, address to) external view returns (uint256);
    function approve(address to, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}


interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}


interface IUniswapV2Pair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);

    function mint(address to) external returns (uint liquidity);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;

    function initialize(address, address) external;
}


// owner
abstract contract Ownable {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, 'owner error');
        _;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        if (newOwner != address(0)) {
            owner = newOwner;
        }
    }
}


// VYHDAO
contract VYHDAO is IERC20, Ownable {
    string private _name = "VYHDAO Token";
    string private _symbol = "VYHDAO";
    uint8 private _decimals = 18;
    uint256 private _totalSupply = 1000000 * (10**_decimals);
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    mapping(address => bool) public blackList;  // black list
    mapping(address => bool) public whiteList;  // white list


    constructor(address _router) {
        _balances[msg.sender] = _totalSupply;
        whiteList[_router] = true;
    }

    function name() public view override returns (string memory) {
        return _name;
    }

    function symbol() public view override returns (string memory) {
        return _symbol;
    }

    function decimals() public view override returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function _transfer(address from, address to, uint256 amount) private {
        _balances[from] = SafeMath.sub(_balances[from], amount);
        _balances[to] = SafeMath.add(_balances[to], amount);
        emit Transfer(from, to, amount);
    }

    function _transferFull(address from, address to, uint256 amount) private {
        require(!blackList[from], "from is black list");
        require(!blackList[to], "to is black list");
        if(whiteList[from] || whiteList[to]) {
            _transfer(from, to, amount);
            return;
        }

        require(!isLP(from), "from is lp address");
        _transfer(from, to, amount);
    }

    function transfer(address to, uint256 amount) public override returns (bool) {
        require(_balances[msg.sender] >= amount, 'balance error');
        _transferFull(msg.sender, to, amount);
        return true;
    }

    function allowance(address from, address to) public override view returns (uint256) {
        return _allowances[from][to];
    }

    function approve(address to, uint256 amount) public override returns (bool) {
        _allowances[msg.sender][to] = amount;
        emit Approval(msg.sender, to, amount);
        return true;
    }

    function transferFrom(address from, address to, uint256 amount) public override returns (bool) {
        require(_balances[from] >= amount, 'balance error');
        require(_allowances[from][msg.sender] >= amount, 'approve error');
        _allowances[from][msg.sender] = SafeMath.sub(_allowances[from][msg.sender], amount);
        _transferFull(from, to, amount);
        return true;
    }

    // set blackList
    function setBlackList(address account, bool isBlack) external onlyOwner {
        require(account != address(0), "VYHDAO-setBlackList: address error");
        blackList[account] = isBlack;
    }

    // set whiteList
    function setWhiteList(address account, bool isWhite) external onlyOwner {
        require(account != address(0), "VYHDAO-setWhiteList: address error");
        whiteList[account] = isWhite;
    }

    // is lp
    function isLP(address account) public view returns(bool) {
        if(isContract(account)) {
            try IUniswapV2Pair(account).token0() {} catch { return false; }
            return true;
        }else {
            return false;
        }
    }

    function isContract(address account) internal view returns (bool) {
        return account.code.length > 0;
    }

}