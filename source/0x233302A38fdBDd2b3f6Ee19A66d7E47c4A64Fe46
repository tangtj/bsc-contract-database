// SPDX-License-Identifier: MIT

pragma solidity 0.8.3;

//--------------------------------------
//  EVAI contract
//
// Symbol      : EVAI
// Name        : EVAI.IO
// Total supply: 1000000000
// Decimals    : 8
//--------------------------------------

/// @title An ERC20 Interface
/// @author Meer Ali
/// @author Chay

abstract contract ERC20Interface {
    function totalSupply() virtual external view returns (uint256);
    function balanceOf(address tokenOwner) virtual external view returns (uint);
    function allowance(address tokenOwner, address spender) virtual external view returns (uint);
    function transfer(address to, uint tokens) virtual external returns (bool);
    function approve(address spender, uint tokens) virtual external returns (bool);
    function transferFrom(address from, address to, uint tokens) virtual external returns (bool);
    function buy(address to, uint tokens) virtual external returns (bool);
    function operationProfit(uint _profit) virtual external returns(bool);
    
 
    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
    event Burn(address from, address, uint256 value);
    event Mint(address from, address, uint256 value);
    event Profit(address from, uint profit, uint totalProfit);

    }

/// @title Safe Math Contract
/// @notice This is used for safe add and safe subtract. This overcomes the overflow errors.

contract SafeMath {
    function safeAdd(uint a, uint b) public pure returns (uint c) {
        c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function safeSub(uint a, uint b) public pure returns (uint c) {
        require(b <= a, "SafeMath: subtraction overflow"); 
        c = a - b; 
        return c;
    }

}

/// @title BEP20 TOKEN Contract
/// @notice This contract is the EVAI.IO Token on Binance Smart Chain.

contract BEP20 is ERC20Interface, SafeMath{
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public _totalSupply;
    address public owner;
    uint public totalProfit;
    uint public profit;
   
    mapping(address => uint) internal balances;
    mapping(address => mapping(address => uint)) internal allowed;
    mapping(uint256 => uint256) internal token;
    mapping(address => bool) internal admins;

    modifier onlyAdmin() {
        require(admins[msg.sender]);
        _;
    }

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }
    
    constructor() {
        name = "EVAI.IO";
        symbol = "EVAI";
        decimals = 8;
        _totalSupply = 0;
	    balances[msg.sender] = _totalSupply;
        owner = msg.sender;
    }
    
    function addAdmin(address account) public onlyOwner {
        admins[account] = true;
    }

    function removeAdmin(address account) public onlyOwner {
        admins[account] = false;
    }

    function isAdmin(address account) public view onlyOwner returns (bool) {
        return admins[account];
    }

    function totalSupply() external view override returns (uint256) {
        return safeSub(_totalSupply, balances[address(0)]);
    }

    function balanceOf(address tokenOwner) external view override returns (uint getBalance) {
        return balances[tokenOwner];
    }
 
    function allowance(address tokenOwner, address spender) external view override returns (uint remaining) {
        return allowed[tokenOwner][spender];
    }
 
    function approve(address spender, uint tokens) external override returns (bool success) {
        allowed[msg.sender][spender] = tokens;
        emit Approval(msg.sender, spender, tokens);
        return true;
    }
    
    function transfer(address to, uint tokens) external override returns (bool success) {
        require(to != address(0),"BEP20: Address should not be a zero");
        balances[msg.sender] = safeSub(balances[msg.sender], tokens);
        balances[to] = safeAdd(balances[to], tokens);
        emit Transfer(msg.sender, to, tokens);
        return true;
    }
    
   function transferFrom(address from, address to, uint tokens) external override returns (bool success) {
        require(to != address(0),"BEP20: Address should not be a zero");
        balances[from] = safeSub(balances[from], tokens);
        allowed[from][msg.sender] = safeSub(allowed[from][msg.sender], tokens);
        balances[to] = safeAdd(balances[to], tokens);
        emit Transfer(from, to, tokens);
        return true;
   }
   
   function buy(address to, uint tokens) external override returns (bool success) {
        require(to != address(0),"BEP20: Address should not be a zero");
        balances[msg.sender] = safeSub(balances[msg.sender], tokens);
        balances[to] = safeAdd(balances[to], tokens);
        emit Transfer(msg.sender, to, tokens);
        return true;
    }
    
    function operationProfit(uint _profit) external override onlyOwner returns(bool success){
         profit = _profit;
         totalProfit = safeAdd(totalProfit, profit);
         emit Profit(msg.sender, profit, totalProfit);
         return true;   
    }
    
    function burn(address account,uint tokens) public onlyAdmin returns(bool success) {
        require(account != address(0),"BEP20: Burn from a zero address");
        uint256 accountBalance = balances[account];
        require(accountBalance >= tokens , "BEP20: Burn amount exceeds Balance");
        balances[account] = safeSub(accountBalance,tokens);
        _totalSupply = safeSub(_totalSupply,tokens);
        emit Burn(msg.sender,address(0), tokens);
        return true;
    }
    
    function mint(address account,uint tokens) public onlyAdmin returns(bool success) {
        require(account != address(0),"BEP20: Mint from a zero address");
        balances[account] = safeAdd(balances[owner],tokens);
        _totalSupply = safeAdd(_totalSupply,tokens);
        emit Mint(msg.sender,address(0),tokens);
        return true;   
    }
    
 }