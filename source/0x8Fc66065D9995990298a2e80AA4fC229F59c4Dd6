// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract DoofyToken is IBEP20 {
    string public constant name = "Doofy";
    string public constant symbol = "DOF";
    uint8 public constant decimals = 10;
    uint256 private _totalSupply = 200_000_000_000_000 * 10**uint256(decimals);
    
    address private _owner;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => uint256) private _lockedUntil;
    
    constructor() {
        _owner = msg.sender;
        _balances[_owner] = _totalSupply;
        emit Transfer(address(0), _owner, _totalSupply);
    }
    
    modifier onlyOwner() {
        require(msg.sender == _owner, "Caller is not the owner");
        _;
    }
    
    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }
    
    function balanceOf(address account) external view override returns (uint256) {
        return _balances[account];
    }
    
    function transfer(address recipient, uint256 amount) external override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }
    
    function allowance(address owner, address spender) external view override returns (uint256) {
        return _allowances[owner][spender];
    }
    
    function approve(address spender, uint256 amount) external override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }
    
    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender] - amount);
        return true;
    }
    
    function mint(address account, uint256 amount) external onlyOwner returns (bool) {
        _mint(account, amount);
        return true;
    }
    
    function burn(uint256 amount) external onlyOwner returns (bool) {
        _burn(msg.sender, amount);
        return true;
    }
    
    function lockTokens(address account, uint256 lockDuration) external onlyOwner {
        require(lockDuration > 0, "Lock duration must be greater than zero");
        require(account != address(0), "Cannot lock tokens for the zero address");
        require(_balances[account] > 0, "Account balance must be greater than zero");
        
        _lockedUntil[account] = block.timestamp + lockDuration;
    }
    
    function unlockTokens() external {
        require(_lockedUntil[msg.sender] > 0, "Tokens are not locked for this account");
        require(block.timestamp >= _lockedUntil[msg.sender], "Tokens are still locked");
        
        _lockedUntil[msg.sender] = 0;
    }
    
    function isTokenLocked(address account) external view returns (bool) {
        return _lockedUntil[account] > block.timestamp;
    }
    
    function _transfer(address sender, address recipient, uint256 amount) private {
        require(sender != address(0), "Transfer from the zero address");
        require(recipient != address(0), "Transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        require(_balances[sender] >= amount, "Insufficient balance");
        require(_lockedUntil[sender] == 0 || block.timestamp >= _lockedUntil[sender], "Tokens are locked");
        
        _balances[sender] -= amount;
        _balances[recipient] += amount;
        
        emit Transfer(sender, recipient, amount);
    }
    
    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "Approve from the zero address");
        require(spender != address(0), "Approve to the zero address");
        
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    
    function _mint(address account, uint256 amount) private {
        require(account != address(0), "Mint to the zero address");
        require(amount > 0, "Mint amount must be greater than zero");
        
        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
    }
    
    function _burn(address account, uint256 amount) private {
        require(account != address(0), "Burn from the zero address");
        require(amount > 0, "Burn amount must be greater than zero");
        require(_balances[account] >= amount, "Burn amount exceeds balance");
        require(_lockedUntil[account] == 0 || block.timestamp >= _lockedUntil[account], "Tokens are locked");
        
        _balances[account] -= amount;
        _totalSupply -= amount;
        emit Transfer(account, address(0), amount);
    }
}