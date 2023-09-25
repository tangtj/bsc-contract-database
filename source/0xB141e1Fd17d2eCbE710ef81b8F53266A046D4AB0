/**
 *Submitted for verification at BscScan.com on 2023-09-21
*/

// SPDX-License-Identifier: UNLICENSE
pragma solidity ^0.8.17;

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

contract Calcium is IBEP20 {
    string private _name = "Calcium";
    string private _symbol = "CAL";
    uint8 private _decimals = 18;
    uint256 private _totalSupply = 421000000 * 10**uint256(_decimals);
    address private _owner;
    address private _marketingWallet = 0x7302aD995bAD024d4F838780Fa1071eDD4f7233e;
    address private _deadAddress = 0x000000000000000000000000000000000000dEaD;
    
    uint256 private _marketingTaxRate = 2;
    uint256 private _burnTaxRate = 3;
    uint256 private _totalTaxRate = 5;

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    
    constructor() {
        _owner = msg.sender;
        _balances[msg.sender] = _totalSupply;
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

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        require(recipient != address(0), "BEP20: transfer to the zero address");
        require(_balances[msg.sender] >= amount, "BEP20: transfer amount exceeds balance");
        
        _applyTax(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");
        require(_balances[sender] >= amount, "BEP20: transfer amount exceeds balance");
        require(_allowances[sender][msg.sender] >= amount, "BEP20: transfer amount exceeds allowance");
        
        _applyTax(sender, recipient, amount);
        return true;
    }

    function _applyTax(address sender, address recipient, uint256 amount) private {
        uint256 marketingTax = (amount * _marketingTaxRate) / 100;
        uint256 burnTax = (amount * _burnTaxRate) / 100;
        uint256 netAmount = amount - marketingTax - burnTax;
        
        _balances[sender] -= amount;
        _balances[recipient] += netAmount;
        _balances[_marketingWallet] += marketingTax;
        
        // Burn tokens by sending to the dead address
        _balances[_deadAddress] += burnTax;
        
        emit Transfer(sender, recipient, netAmount);
        emit Transfer(sender, _marketingWallet, marketingTax);
        emit Transfer(sender, _deadAddress, burnTax);
    }
}