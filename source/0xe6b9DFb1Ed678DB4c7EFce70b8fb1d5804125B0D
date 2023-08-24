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
}


// new..
contract MOSCardTokenV2 is IERC20, Ownable {
    string private _name = "BMOS Token";
    string private _symbol = "BMOS";
    uint8 private _decimals = 18;
    uint256 private _totalSupply = 100000000 * (10**_decimals);
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    mapping(address => bool) public whitelistAddress;
    mapping(address => bool) public blacklistAddress;


    constructor() {
        _balances[msg.sender] = _totalSupply;
        whitelistAddress[msg.sender] = true;
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
        require(whitelistAddress[_from] || whitelistAddress[_to], "not white list");
        _transfer(_from, _to, _value);
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

    // set white list
    function setWhitelist(address account, bool status) public onlyOwner {
        whitelistAddress[account] = status;
    }

    // set block list
    function setBlacklist(address account, bool status) public onlyOwner {
        blacklistAddress[account] = status;
    }

    function isContract(address account) internal view returns (bool) {
        return account.code.length > 0;
    }

    // verify blacklist
    function _verifyBlacklist(address _from, address _to) private view {
        if(blacklistAddress[_from] || blacklistAddress[_to]) {
            revert('blacklist error');
        }
    }

}