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

contract TaxableBEP20 is IBEP20 {
    string public constant name = "gooooooo";
    string public constant symbol = "go";
    uint8 public constant decimals = 18;

    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;

    uint256 private _totalSupply = 1000000 * 10**18; // 1 million tokens
    address public taxWallet = 0x39577830fb7219d7e860BffEFD8f9c81d444920A; // Replace with your tax wallet address
    uint256 public taxRate = 5; // 5% for general transfers
    uint256 public buyTaxRate = 7; // 7% for buys
    uint256 public sellTaxRate = 3; // 3% for sells

    constructor() {
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
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

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");
        require(_balances[sender] >= amount, "BEP20: transfer amount exceeds balance");

        uint256 taxAmount = 0;

        if (recipient == address(this)) {
            taxAmount = amount * buyTaxRate / 100;
        } else if (sender == address(this)) {
            taxAmount = amount * sellTaxRate / 100;
        } else {
            taxAmount = amount * taxRate / 100;
        }

        uint256 sendAmount = amount - taxAmount;

        _balances[sender] -= amount;
        _balances[recipient] += sendAmount;
        _balances[taxWallet] += taxAmount;

        emit Transfer(sender, recipient, sendAmount);
        if (taxAmount > 0) {
            emit Transfer(sender, taxWallet, taxAmount);
        }
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
}