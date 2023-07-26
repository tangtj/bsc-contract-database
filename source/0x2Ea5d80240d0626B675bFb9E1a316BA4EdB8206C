// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

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

contract DDeVitoToken is IERC20 {
    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;

    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;

    address private _owner;
    address private _devWallet;
    uint256 private _buyTax;
    uint256 private _sellTax;

    constructor() {
        _name = "Danny DeVito";
        _symbol = "D-DeVito";
        _decimals = 18;
        _totalSupply = 800000000 * (10**uint256(_decimals));
        _balances[msg.sender] = _totalSupply;
        _owner = msg.sender;
        _devWallet = 0x406AFd63f83D04522a26711C69bf47d7D5fF4d4c;
        _buyTax = 5;
        _sellTax = 5;

        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint8) {
        return _decimals;
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

    function buyTax() public view returns (uint256) {
        return _buyTax;
    }

    function sellTax() public view returns (uint256) {
        return _sellTax;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "Transfer from the zero address");
        require(recipient != address(0), "Transfer to the zero address");

        uint256 taxAmount = 0;
        if (sender == _owner) {
            // Transfer by the owner, no tax applied.
            taxAmount = 0;
        } else if (sender == _devWallet) {
            // Transfer from the dev wallet, applying the sell tax.
            taxAmount = amount * _sellTax / 100;
        } else {
            // Regular transfer, applying the buy tax.
            taxAmount = amount * _buyTax / 100;
        }

        uint256 transferAmount = amount - taxAmount;

        _balances[sender] -= amount;
        _balances[recipient] += transferAmount;

        emit Transfer(sender, recipient, transferAmount);

        if (taxAmount > 0) {
            _burn(sender, taxAmount);
            emit Transfer(sender, address(0), taxAmount);
        }
    }

    function _burn(address account, uint256 amount) internal {
        require(account != address(0), "Burn from the zero address");
        require(amount > 0, "Burn amount must be greater than zero");

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "Burn amount exceeds balance");

        _balances[account] = accountBalance - amount;
        _totalSupply -= amount;
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "Approve from the zero address");
        require(spender != address(0), "Approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
}