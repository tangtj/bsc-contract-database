// SPDX-License-Identifier: Unlicense

pragma solidity ^0.8.17;

 

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

 

library SafeMath {

function add(uint256 a, uint256 b) internal pure returns (uint256) {

uint256 c = a + b;

require(c >= a, "SafeMath: addition overflow");

return c;

}

 

function sub(uint256 a, uint256 b) internal pure returns (uint256) {

require(b <= a, "SafeMath: subtraction overflow");

uint256 c = a - b;

return c;

}

 

function mul(uint256 a, uint256 b) internal pure returns (uint256) {

if (a == 0) {

return 0;

}

uint256 c = a * b;

require(c / a == b, "SafeMath: multiplication overflow");

return c;

}

 

function div(uint256 a, uint256 b) internal pure returns (uint256) {

require(b > 0, "SafeMath: division by zero");

uint256 c = a / b;

return c;

}

}

 

contract Catfitt is IERC20 {

using SafeMath for uint256;

 

string private _name;

string private _symbol;

uint256 private _totalSupply;

uint8 private _decimals;

 

mapping(address => uint256) private _balances;

mapping(address => mapping(address => uint256)) private _allowances;

 

address private _owner;

 

constructor() {

_name = "catfitt";

_symbol = "CAT";

_totalSupply = 36600000000000000000 * 10**18; // 36.6 billion tokens with 18 decimal places

_decimals = 18;

_balances[msg.sender] = _totalSupply;

_owner = msg.sender; // Set initial owner

emit Transfer(address(0), msg.sender, _totalSupply);

}

 

modifier onlyOwner() {

require(msg.sender == _owner, "Only the owner can perform this action");

_;

}

 

function name() external view returns (string memory) {

return _name;

}

 

function symbol() external view returns (string memory) {

return _symbol;

}

 

function decimals() external view returns (uint8) {

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

uint256 currentAllowance = _allowances[sender][msg.sender];

require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");

_approve(sender, msg.sender, currentAllowance - amount);

return true;

}

 

function _transfer(address sender, address recipient, uint256 amount) internal {

require(sender != address(0), "ERC20: transfer from the zero address");

require(recipient != address(0), "ERC20: transfer to the zero address");

_balances[sender] = _balances[sender].sub(amount);

_balances[recipient] = _balances[recipient].add(amount);

emit Transfer(sender, recipient, amount);

}

 

function _approve(address owner, address spender, uint256 amount) internal {

require(owner != address(0), "ERC20: approve from the zero address");

require(spender != address(0), "ERC20: approve to the zero address");

_allowances[owner][spender] = amount;

emit Approval(owner, spender, amount);

}

 

function transferOwnership(address newOwner) external onlyOwner {

require(newOwner != address(0), "Invalid new owner address");

_owner = newOwner;

}

 

function relinquishOwnership() external onlyOwner {

_owner = address(0);

}

}