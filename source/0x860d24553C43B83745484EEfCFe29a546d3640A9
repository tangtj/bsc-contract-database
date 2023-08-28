// SPDX-License-Identifier: MIT
pragma solidity ^0.6.2;

contract CustomToken {
    string public name;
    string public symbol;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint256 private _totalSupply;
    bool private _paused;

    address public owner;

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    modifier whenNotPaused() {
        require(!_paused, "Token transfers are paused");
        _;
    }

    constructor(string memory _name, string memory _symbol) public {
        name = _name;
        symbol = _symbol;
        owner = msg.sender;
        _paused = false;
    }

    function totalSupply() external view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view returns (uint256) {
        return _balances[account];
    }

    function allowance(address owner, address spender) external view returns (uint256) {
        return _allowances[owner][spender];
    }

    function mint(address account, uint256 amount) external onlyOwner whenNotPaused {
        _balances[account] += amount;
        _totalSupply += amount;
    }

    function burn(address account, uint256 amount) external onlyOwner whenNotPaused {
        require(_balances[account] >= amount, "Insufficient balance");
        _balances[account] -= amount;
        _totalSupply -= amount;
    }

    function approve(address spender, uint256 amount) external whenNotPaused returns (bool) {
        _allowances[msg.sender][spender] = amount;
        return true;
    }

    function transfer(address recipient, uint256 amount) external whenNotPaused returns (bool) {
        require(recipient != address(0), "Transfer to zero address");
        require(_balances[msg.sender] >= amount, "Insufficient balance");
        _balances[msg.sender] -= amount;
        _balances[recipient] += amount;
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external whenNotPaused returns (bool) {
        require(recipient != address(0), "Transfer to zero address");
        require(_balances[sender] >= amount, "Insufficient balance");
        require(_allowances[sender][msg.sender] >= amount, "Allowance exceeded");
        _balances[sender] -= amount;
        _balances[recipient] += amount;
        _allowances[sender][msg.sender] -= amount;
        return true;
    }

    function pause() external onlyOwner {
        _paused = true;
    }

    function unpause() external onlyOwner {
        _paused = false;
    }
}