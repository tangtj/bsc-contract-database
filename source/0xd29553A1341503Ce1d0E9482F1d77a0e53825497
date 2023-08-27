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

contract AGUADELSOL is IBEP20 {
    string public name = "AGUADELSOL";
    string public symbol = "AGUADELSOL";
    uint8 public decimals = 18;
    uint256 private _totalSupply = 100000000 * 10**uint256(decimals);
    uint256 private _maxTokensPerAddress = 100000 * 10**uint256(decimals);
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    bool private _paused;
    mapping(address => bool) private _blacklist;
    address private _owner;

    constructor() {
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
        _owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == _owner, "BEP20: caller is not the owner");
        _;
    }

    modifier notPaused() {
        require(!_paused, "BEP20: token transfers are paused");
        _;
    }

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) external override notPaused returns (bool) {
        require(recipient != address(0), "BEP20: transfer to the zero address");
        require(_balances[msg.sender] >= amount, "BEP20: insufficient balance");
        require(_balances[recipient] + amount <= _maxTokensPerAddress, "BEP20: recipient would exceed maximum token holding");

        _balances[msg.sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) external view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external override notPaused returns (bool) {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override notPaused returns (bool) {
        require(recipient != address(0), "BEP20: transfer to the zero address");
        require(_balances[sender] >= amount, "BEP20: insufficient balance");
        require(_allowances[sender][msg.sender] >= amount, "BEP20: insufficient allowance");
        require(_balances[recipient] + amount <= _maxTokensPerAddress, "BEP20: recipient would exceed maximum token holding");

        _balances[sender] -= amount;
        _balances[recipient] += amount;
        _allowances[sender][msg.sender] -= amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function pause() external onlyOwner {
        _paused = true;
    }

    function unpause() external onlyOwner {
        _paused = false;
    }

    function isPaused() external view returns (bool) {
        return _paused;
    }

    function blacklist(address account) external onlyOwner {
        _blacklist[account] = true;
    }

    function unblacklist(address account) external onlyOwner {
        _blacklist[account] = false;
    }

    function isBlacklisted(address account) external view returns (bool) {
        return _blacklist[account];
    }
}