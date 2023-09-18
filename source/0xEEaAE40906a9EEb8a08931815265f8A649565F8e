// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract SHUTOUYAcoin {
    string public constant name = "SHUTOUYA coin";
    string public constant symbol = "STY";
    uint8 public constant decimals = 18;
    uint256 public constant totalSupply = 99999999999999999999999999999999999999999999999999999;

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _excludedFromDividends;

    uint256 private _totalDividends;
    mapping(address => uint256) private _dividendBalances;
    mapping(address => uint256) private _lastDividendPoints;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event DividendsDistributed(address indexed from, uint256 amount);
    event DividendsCollected(address indexed by, uint256 amount);

    constructor() {
        _balances[msg.sender] = totalSupply;
        _excludedFromDividends[msg.sender] = true;
        emit Transfer(address(0), msg.sender, totalSupply);
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
        _approve(sender, msg.sender, _allowances[sender][msg.sender] - amount);
        return true;
    }

    function excludeFromDividends(address account) public {
        require(msg.sender == address(this), "Only the contract itself can exclude an address from dividends");
        require(!_excludedFromDividends[account], "Address is already excluded from dividends");
        _excludedFromDividends[account] = true;
    }

    function includeInDividends(address account) public {
        require(msg.sender == address(this), "Only the contract itself can include an address in dividends");
        require(_excludedFromDividends[account], "Address is not excluded from dividends");
        _excludedFromDividends[account] = false;
    }

    function distributeDividends() public payable {
        require(_totalDividends == 0, "Dividends can only be distributed when there are no pending dividends");
        require(msg.value > 0, "Invalid dividend amount");

        _totalDividends = msg.value;

        emit DividendsDistributed(msg.sender, msg.value);
    }

    function collectDividends() public {
        require(_dividendBalances[msg.sender] > 0, "No pending dividends for the caller");

        uint256 amount = _dividendBalances[msg.sender];
        _dividendBalances[msg.sender] = 0;

        payable(msg.sender).transfer(amount);

        emit DividendsCollected(msg.sender, amount);
    }

    function _transfer(address sender, address recipient, uint256 amount) private {
        require(sender != address(0), "Transfer from the zero address is not allowed");
        require(recipient != address(0), "Transfer to the zero address is not allowed");
        require(amount > 0, "Transfer amount must be greater than zero");
        require(_balances[sender] >= amount, "Insufficient balance");

        _balances[sender] -= amount;
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "Approve from the zero address is not allowed");
        require(spender != address(0), "Approve to the zero address is not allowed");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
}