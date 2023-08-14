// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function approve(address spender, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract MEMEABLE is IBEP20 {
    string public name = "MEMEABLE";
    string public symbol = "MEMBL";
    uint8 public decimals = 18;
    uint256 public override totalSupply = 500_000_000_000 * 10**uint256(decimals);

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public liquidityWallet = 0x7C1E935Ae5f169aA73A73F0C6B74CDc1A5945c27;
    address public cexListingsWallet = 0xf4f5737C889E88Ba883E9b32547e656143b30406;
    address public charityWallet = 0x7a46004B43ef49e3c42EE49640195db3B04FDdB0;
    address public rewardsWallet = 0x9c5e6C89A4893cdaecab036ec9B29D795588fCE3;

    constructor() {
        _balances[liquidityWallet] = totalSupply * 80 / 100;
        _balances[cexListingsWallet] = totalSupply * 10 / 100;
        _balances[charityWallet] = totalSupply * 5 / 100;
        _balances[rewardsWallet] = totalSupply * 5 / 100;
        emit Transfer(address(0), liquidityWallet, _balances[liquidityWallet]);
        emit Transfer(address(0), cexListingsWallet, _balances[cexListingsWallet]);
        emit Transfer(address(0), charityWallet, _balances[charityWallet]);
        emit Transfer(address(0), rewardsWallet, _balances[rewardsWallet]);
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender] - amount);
        return true;
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function _approve(address owner, address spender, uint256 amount) private {
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(address sender, address recipient, uint256 amount) private {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");
        require(amount > 0, "BEP20: transfer amount must be greater than zero");
        require(_balances[sender] >= amount, "BEP20: insufficient balance");

        _balances[sender] -= amount;
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);
    }
}