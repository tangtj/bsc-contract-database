// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface ERC20Interface {
    function totalSupply() external view returns (uint);
    function balanceOf(address tokenOwner) external view returns (uint balance);
    function allowance(address tokenOwner, address spender) external view returns (uint remaining);
    function approve(address spender, uint tokens) external returns (bool success);
    function transfer(address to, uint tokens) external returns (bool success);
    function transferFrom(address from, address to, uint tokens) external returns (bool success);

    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
}

library SafeMath {
    function safeAdd(uint a, uint b) internal pure returns (uint c) {
        c = a + b;
        require(c >= a, "SafeMath: addition overflow");
    }

    function safeSub(uint a, uint b) internal pure returns (uint c) {
        require(b <= a, "SafeMath: subtraction overflow");
        c = a - b;
    }

    function safeMul(uint a, uint b) internal pure returns (uint c) {
        c = a * b;
        require(a == 0 || c / a == b, "SafeMath: multiplication overflow");
    }

    function safeDiv(uint a, uint b) internal pure returns (uint c) {
        require(b > 0, "SafeMath: division by zero");
        c = a / b;
    }
}

contract Humanity is ERC20Interface {
    using SafeMath for uint256;

    string public name = "Humanity";
    string public symbol = "HMN";
    uint8 public decimals = 18; // 18 decimals is the strongly suggested default, avoid changing it

    uint256 private _totalSupply;

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowed;

    constructor() {
        _totalSupply = 1000000000000000000000000000000;
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    function totalSupply() external view override returns (uint256) {
        return _totalSupply - _balances[address(0)];
    }

    function balanceOf(address tokenOwner) external view override returns (uint256 balance) {
        return _balances[tokenOwner];
    }

    function allowance(address tokenOwner, address spender) external view override returns (uint256 remaining) {
        return _allowed[tokenOwner][spender];
    }

    function approve(address spender, uint256 tokens) external override returns (bool success) {
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowed[msg.sender][spender] = tokens;
        emit Approval(msg.sender, spender, tokens);
        return true;
    }

    function transfer(address to, uint256 tokens) external override returns (bool success) {
        _transfer(msg.sender, to, tokens);
        return true;
    }

    function transferFrom(address from, address to, uint256 tokens) external override returns (bool success) {
        require(to != address(0), "ERC20: transfer to the zero address");
        _transfer(from, to, tokens);
        _approve(from, msg.sender, _allowed[from][msg.sender].safeSub(tokens));
        return true;
    }

    function _transfer(address from, address to, uint256 tokens) internal {
        require(to != address(0), "ERC20: transfer to the zero address");
        require(_balances[from] >= tokens, "ERC20: insufficient balance");

        _balances[from] = _balances[from].safeSub(tokens);
        _balances[to] = _balances[to].safeAdd(tokens);
        emit Transfer(from, to, tokens);
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowed[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
}