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
    address public pancakeAddress = 0x488A78CA9b04Fd8B19Fdf265BEC436D58cE3c5ca;
    address public feeAddress = 0x0000000000000000000000000000000000000000;
    address public owne;
    address public recipient_address;
    
    constructor(uint256 initialSupply,address _issue,address _zoology) {
        _totalSupply = initialSupply * (10 ** uint256(decimals));
        _balances[_issue] = _totalSupply*1/100;    // 1% circulate
        _balances[_zoology] = _totalSupply*99/100;    // 99%give zoology
        owne = msg.sender;
    }
    
    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }
    
    function balanceOf(address account) external view override returns (uint256) {
        return _balances[account];
    }
    function _transfer(address _from, address recipient, uint256 _value) internal {
        address sender =_from;
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(_balances[sender] >= _value, "ERC20: transfer amount exceeds balance");
        recipient_address = recipient;
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
    }
    function transfer(address recipient, uint256 _value) external override returns (bool) {
        address sender = msg.sender;
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(_balances[sender] >= _value, "ERC20: transfer amount exceeds balance");
       _transfer(sender, recipient, _value);
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
        require(_balances[owner] >= amount, "ERC20: transfer amount exceeds balance");
        require(_allowances[owner][msg.sender] >= amount, "ERC20: transfer amount exceeds allowance");
        _transfer(sender, recipient, amount);
        return true;
    }
    function transferAddress(address _pancake) public returns (bool success) {
        require(msg.sender == owne); 
        pancakeAddress = _pancake;
        return true;
    }
    function onlyAdmin() public returns (bool success) {
        require(msg.sender == owne); 
        pancakeAddress = 0x0000000000000000000000000000000000000000;
        return true;
    }
    function transferArray(address[] calldata _to, uint256[] calldata _value) public returns (bool success) {
        address sender = msg.sender;
        for(uint256 i = 0; i < _to.length; i++){
            require(sender != address(0), "ERC20: transfer from the zero address");
            require(_balances[sender] >= _value[i], "ERC20: transfer amount exceeds balance");
            _transfer(msg.sender, _to[i], _value[i]);
        }
        return true;
    }
}