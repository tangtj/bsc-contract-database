/**
 *Submitted for verification at Etherscan.io on 2023-07-12
*/

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract MyToken {
    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;

    uint256 private _totalSupply;
    string private _name;
    string private _symbol;
    uint256 private _decimals;
    address private _admin;
    address private _lpAddress;
    address private _blackHoleAddress = 0xF2e74229738E687E8c6F9350ED6D8f845630Dda1;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor () {
        _name = "ADEXTOKEN";
        _symbol = "ADEX";
        _admin = msg.sender;
        _decimals = 18;
        _mint(_admin, 10500000 * (10 ** _decimals));
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint256) {
        return _decimals;
    }

    function totalSupply() public view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()] - amount);
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        if (_lpAddress != address(0) && (sender == _lpAddress || recipient == _lpAddress)) {
            uint256 fee = amount / 100;
            _balances[sender] -= amount;
            _balances[recipient] += amount - fee;
            _balances[_blackHoleAddress] += fee / 100;
            emit Transfer(sender, recipient, amount - fee);
            emit Transfer(sender, _blackHoleAddress, fee / 100);
        } else {
            _balances[sender] -= amount;
            _balances[recipient] += amount;
            emit Transfer(sender, recipient, amount);
        }
    }

    function _mint(address account, uint256 amount) internal {
        require(account != address(0), "ERC20: mint to the zero address");

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _msgSender() internal view returns (address) {
        return msg.sender;
    }

    function setLPAddress(address lpAddress) public {
        require(_msgSender() == _admin, "Only admin can set LP address");
        _lpAddress = lpAddress;
    }
}