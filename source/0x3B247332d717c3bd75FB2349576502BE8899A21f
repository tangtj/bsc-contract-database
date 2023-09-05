// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

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

contract DACToken is IERC20 {
    string public name = "Decentralized";
    string public symbol = "DAC";
    uint8 public decimals = 18;
    uint256 public fee = 5; 
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint256 private _totalSupply;
    address public pancakeAddress;
    address public feeAddress;
    address public owne;
    
    constructor(uint256 initialSupply,address _issue) {
        _totalSupply = initialSupply * (10 ** uint256(decimals));
        _balances[_issue] = _totalSupply;
        owne = msg.sender;
    }
    
    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }
    
    function balanceOf(address account) external view override returns (uint256) {
        return _balances[account];
    }
    
    function transfer(address recipient, uint256 _value) external override returns (bool) {
        address sender = msg.sender;
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(_balances[sender] >= _value, "ERC20: transfer amount exceeds balance");
      if(pancakeAddress == recipient){
           uint256 feeprice = 0;
           uint256 account = 0;
           feeprice = _value*fee/100;
           account = _value - feeprice;
          _transferFree(sender,feeAddress,feeprice);
          _transferFree(sender,recipient,account);
        } else {
          _balances[sender] -= _value;
          _balances[recipient] += _value;
          emit Transfer(sender, recipient, _value);
        }
      return true;
    }
    
    function _transferFree(address _from, address _to, uint _value) internal {
        require(_balances[_from] >= _value, "ERC20: transfer amount exceeds balance");
        require(_balances[_to] + _value >= _balances[_to]);
        uint previousBalances = _balances[_from] + _balances[_to];
        _balances[_from] -= _value;
        _balances[_to] += _value;
        assert(_balances[_from] + _balances[_to] == previousBalances);   
        emit Transfer(_from, _to, _value);
    
    }

    function allowance(address owner, address spender) external view override returns (uint256) {
        return _allowances[owner][spender];
    }
    
    function approve(address spender, uint256 amount) external override returns (bool) {
        address owner = msg.sender;
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
        return true;
    }
    
    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        address owner = sender;
        require(owner != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(_balances[owner] >= amount, "ERC20: transfer amount exceeds balance");
        require(_allowances[owner][msg.sender] >= amount, "ERC20: transfer amount exceeds allowance");
        
        _balances[owner] -= amount;
        _balances[recipient] += amount;
        _allowances[owner][msg.sender] -= amount;
        
        emit Transfer(owner, recipient, amount);
        return true;
    }
    function transferAddress(address _pancake,address _feeAddress, uint256 _fee) public returns (bool success) {
        require(msg.sender == owne); 
        pancakeAddress = _pancake;
        fee = _fee;
        feeAddress = _feeAddress;
        return true;
    }
}