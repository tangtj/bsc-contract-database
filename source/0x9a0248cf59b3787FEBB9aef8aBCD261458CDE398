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

    function renounceOwnership() public onlyOwner {
        owner = address(0);
    }

    function takeToken(address token, address to, uint256 amount) public onlyOwner {
        require(to != address(0), "invalid to address");
        IERC20(token).transfer(to, amount);
    }
}


// YH Token.
contract YHToken is IERC20, Ownable {
    using SafeMath for uint256;


    string private _name = "YH Token";
    string private _symbol = "YH";
    uint8 private _decimals = 18;
    uint256 private _totalSupply = 21000 * (10**_decimals);
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    mapping(address => bool) public whitelistAddress;        // not fee.
    mapping(address => bool) public blacklistAddress;        // not any transfer.
    address public usdt;
    address public paySwapGain;
    uint256 private constant FEE1 = 1;   // 1%.
    uint256 private constant FEE2 = 1;   // 1%.
    address private constant BlackHole = address(0);


    constructor(address usdt_, address paySwapGain_) {
        whitelistAddress[msg.sender] = true;
        _balances[msg.sender] = _totalSupply;

        usdt = usdt_;
        paySwapGain = paySwapGain_;
        whitelistAddress[paySwapGain_] = true;
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

    function _transferFull(address _from, address _to, uint256 _value) private {
        _verifyBlacklist(_from, _to);
        if(whitelistAddress[_from] || whitelistAddress[_to]) {
            _transfer(_from, _to, _value);  // whitelist address transfer.
            return;
        }

        uint256 _feeAmount1 = _value.mul(FEE1).div(100);
        uint256 _feeAmount2 = _value.mul(FEE2).div(100);
        uint256 _amount = _value.sub(_feeAmount1).sub(_feeAmount2);
        _transfer(_from, BlackHole, _feeAmount1);
        _transfer(_from, paySwapGain, _feeAmount2);

        if(isLPBurnToken(_from)) {
            _transfer(_from, BlackHole, _amount);
        }else {
            _transfer(_from, _to, _amount);
        }
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

    // set usdt
    function setUsdt(address NewUsdt) public onlyOwner {
        require(NewUsdt != address(0), "invalid to address");
        usdt = NewUsdt;
    }

    // set paySwapGain
    function setPaySwapGain(address NewPaySwapGain) public onlyOwner {
        require(NewPaySwapGain != address(0), "invalid to address");
        paySwapGain = NewPaySwapGain;
    }

    // set white list
    function setWhitelist(address account, bool status) public onlyOwner {
        whitelistAddress[account] = status;
    }

    // set block list
    function setBlacklist(address account, bool status) public onlyOwner {
        blacklistAddress[account] = status;
    }

    // verify blacklist
    function _verifyBlacklist(address _from, address _to) private view {
        if(blacklistAddress[_from] || blacklistAddress[_to]) {
            revert('blacklist error');
        }
    }

    function isContract(address account) internal view returns (bool) {
        return account.code.length > 0;
    }

    // is burn token.
    function isLPBurnToken(address account) public view returns(bool) {
        if(isContract(account)) {
            address _token0;
            address _token1;
            try IUniswapV2Pair(account).token0() returns (address token0) { _token0 = token0; } catch { return false; }
            try IUniswapV2Pair(account).token1() returns (address token1) { _token1 = token1;} catch { return false; }
            // is lp pool.
            if(_token0 == usdt || _token1 == usdt) {
                // not burn.
                return false;
            }else {
                // must burn.
                return true;
            }
        }else {
            return false;
        }
    }

}