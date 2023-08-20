// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract GOAT is IERC20 {
    string public name = "CRYPTO GOAT";
    string public symbol = "GOAT";
    uint8 public decimals = 18;
    uint256 private _totalSupply = 1000000000000 * 10 ** uint256(decimals);
    address private _owner;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    bool public saleEnabled = true;

    constructor() {
        _owner = msg.sender;
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    modifier onlyOwner() {
        require(msg.sender == _owner, "Only the owner can call this function");
        _;
    }

    modifier canBuyTokens() {
        require(saleEnabled, "Token sale is not enabled");
        _;
    }

    modifier canTransferTokens(address sender) {
        require(saleEnabled || sender == _owner, "Token transfer is not allowed");
        _;
    }

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) external override canTransferTokens(msg.sender) returns (bool) {
        require(recipient != address(0), "Invalid recipient address");
        require(_balances[msg.sender] >= amount, "Insufficient balance");
        
        _balances[msg.sender] -= amount;
        _balances[recipient] += amount;
        
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) external view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external override canTransferTokens(msg.sender) returns (bool) {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override canTransferTokens(sender) returns (bool) {
        require(sender != address(0), "Invalid sender address");
        require(recipient != address(0), "Invalid recipient address");
        require(_balances[sender] >= amount, "Insufficient balance");
        require(_allowances[sender][msg.sender] >= amount, "Allowance exceeded");

        _balances[sender] -= amount;
        _balances[recipient] += amount;
        _allowances[sender][msg.sender] -= amount;

        emit Transfer(sender, recipient, amount);
        return true;
    }

    function renounceOwnership() external onlyOwner {
        _owner = address(0);
    }

    function enableTokenSale() external onlyOwner {
        saleEnabled = true;
    }

    function disableTokenSale() external onlyOwner {
        saleEnabled = false;
    }

    function buyTokens(uint256 amount) external canBuyTokens {
        require(amount > 0, "Amount must be greater than 0");
        require(_balances[_owner] >= amount, "Insufficient balance");
        
        _balances[_owner] -= amount;
        _balances[msg.sender] += amount;
        
        emit Transfer(_owner, msg.sender, amount);
    }
}