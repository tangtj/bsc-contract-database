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
    string public constant name = "testone";
    string public constant symbol = "ttone";
    uint8 public constant decimals = 18;

    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;

    uint256 private _totalSupply = 1000000 * 10**18; // مثال: 1 مليون رمز
    uint256 public buyTaxRate = 10; // 10%
    uint256 public sellTaxRate = 10; // 10%
    address public taxWallet;
    address private _owner;

    constructor(address _taxWallet) {
        _owner = msg.sender;
        taxWallet = _taxWallet;
        _balances[_owner] = _totalSupply;
    }

    modifier onlyOwner() {
        require(_owner == msg.sender, "Not the contract owner");
        _;
    }

    function setBuyTaxRate(uint256 rate) external onlyOwner {
        buyTaxRate = rate;
    }

    function setSellTaxRate(uint256 rate) external onlyOwner {
        sellTaxRate = rate;
    }

    function setTaxWallet(address newTaxWallet) external onlyOwner {
        taxWallet = newTaxWallet;
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
        require(amount > 0, "Transfer amount must be greater than zero");

        uint256 taxAmount = 0;
        if (recipient == address(this)) {
            taxAmount = amount * buyTaxRate / 100;
        } else if (sender == address(this)) {
            taxAmount = amount * sellTaxRate / 100;
        }

        _balances[sender] -= amount;
        _balances[recipient] += amount - taxAmount;
        _balances[taxWallet] += taxAmount;

        emit Transfer(sender, recipient, amount - taxAmount);
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