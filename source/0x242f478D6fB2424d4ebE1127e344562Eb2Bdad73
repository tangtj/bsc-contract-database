// SPDX-License-Identifier: Unlicensed
 
pragma solidity 0.6.12;

interface IBEP20 {
  function totalSupply() external view returns (uint256);
  function balanceOf(address account) external view returns (uint256);
  function transfer(address recipient, uint256 amount) external returns (bool);
  function allowance(address owner, address spender) external view returns (uint256);
  function approve(address spender, uint256 amount) external returns (bool);
  function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
  event Transfer(address indexed from, address indexed to, uint256 value);
  event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract Context {
  constructor () internal { }

  function _msgSender() internal view returns (address payable) {
    return msg.sender;
  }

  function _msgData() internal view returns (bytes memory) {
    this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
    return msg.data;
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
    // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
    // benefit is lost if 'b' is also tested.
    // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
    if (a == 0) {
      return 0;
    }

    uint256 c = a * b;
    require(c / a == b, "SafeMath: multiplication overflow");

    return c;
  }

  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    return div(a, b, "SafeMath: division by zero");
  }

  function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
    // Solidity only automatically asserts when dividing by 0
    require(b > 0, errorMessage);
    uint256 c = a / b;
    // assert(a == b * c + a % b); // There is no case in which this doesn't hold

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

contract Ownable is Context {
  address private _owner;
  address public _newOwner;

  event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

  constructor () internal {
    _owner = _msgSender();
    emit OwnershipTransferred(address(0), _msgSender());
  }

  function owner() public view returns (address) {
    return _owner;
  }

  modifier onlyOwner() {
    require(_owner == _msgSender(), "Ownable: caller is not the owner");
    _;
  }
  
  modifier onlyMidWayOwner() {
    require(_newOwner == _msgSender(), "Ownable: caller is not the Mid Way Owner");
    _;
  }

  function renounceOwnership() public onlyOwner {
    emit OwnershipTransferred(_owner, address(0));
    _owner = address(0);
  }

  function transferOwnership(address newOwner) public onlyOwner {
    require(newOwner != address(0), "Ownable: new owner is the zero address");
    _newOwner = newOwner;
  }

  function recieveOwnership() public onlyMidWayOwner {
    emit OwnershipTransferred(_owner, _newOwner);
    _owner = _newOwner;
  }
}

contract HVGOToken is Context, IBEP20, Ownable {
  using SafeMath for uint256;
    
  mapping (address => uint256) private _balances;
  mapping (address => mapping (address => uint256)) private _allowances;
    
  mapping (address => bool) private _isExcludedFromFee;
  
  address private _marketingWalletAddress = 0xf575a9d16d1b662EbaB29706A6D10138F0b06ba1;
  address private _developmentWalletAddress = 0x51B092172bCB2154c8e1B0B2e69cFf5a5Bc2a286;
  
  string private _name = "HarvestGO";
  string private _symbol = "HVGO";
  uint8 private _decimals = 18;
  
  uint256 private _totalSupply = 1000000 * 10**18;
  uint256 private _tFeeTotal;

  uint256 public _marketingFee = 2;
  uint256 private _previousMarketingFee = _marketingFee;

  uint256 public _developmentFee = 3;
  uint256 private _previousDevelopmentFee = _developmentFee;
  
  constructor() public {
    //exclude owner and this contract from fee
    _isExcludedFromFee[owner()] = true;
    _isExcludedFromFee[address(this)] = true;
    
    _balances[msg.sender] = _totalSupply;

    emit Transfer(address(0), _msgSender(), _totalSupply);
  }
  
  function getOwner() external view returns (address) {
    return owner();
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
  
  function totalSupply() public view override returns (uint256) {
    return _totalSupply;
  }
  
  function balanceOf(address account) public view override returns (uint256) {
    return _balances[account];
  }
  
  function transfer(address recipient, uint256 amount) public override returns (bool) {
    _transfer(_msgSender(), recipient, amount);
    return true;
  }
  
  function allowance(address owner, address spender) public view override returns (uint256) {
    return _allowances[owner][spender];
  }
  
  function approve(address spender, uint256 amount) public override returns (bool) {
    _approve(_msgSender(), spender, amount);
    return true;
  }
  
  function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
    _transfer(sender, recipient, amount);
    _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "BEP20: transfer amount exceeds allowance"));
    return true;
  }

  function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
    _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
    return true;
  }

  function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
    _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "BEP20: decreased allowance below zero"));
    return true;
  }
  
  function mint(uint256 amount) public onlyOwner returns (bool) {
    _mint(_msgSender(), amount);
    return true;
  }

  function totalFees() public view returns (uint256) {
    return _tFeeTotal;
  }
    
  function excludeFromFee(address account) public onlyOwner {
    _isExcludedFromFee[account] = true;
  }
    
  function includeInFee(address account) public onlyOwner {
    _isExcludedFromFee[account] = false;
  }
  
  function setMarketingFeePercent(uint256 marketingFee) external onlyOwner() {
    _marketingFee = marketingFee;
  }

  function setDevelopmentFeePercent(uint256 developmentFee) external onlyOwner() {
    _developmentFee = developmentFee;
  }
  
  function setMarketingWallet(address account) external onlyOwner() {
    _marketingWalletAddress = account;
  }

  function setDevelopmentWallet(address account) external onlyOwner() {
    _developmentWalletAddress = account;
  }
  
  function _takeFeeMarketing(uint256 tFee) private {
    _balances[_marketingWalletAddress] = _balances[_marketingWalletAddress].add(tFee);
    _tFeeTotal = _tFeeTotal.add(tFee);
  }

  function _takeFeeDevelopment(uint256 tFee) private {
    _balances[_developmentWalletAddress] = _balances[_developmentWalletAddress].add(tFee);
    _tFeeTotal = _tFeeTotal.add(tFee);
  }
  
  function _getValues(uint256 amount) private view returns (uint256, uint256, uint256) {
    uint256 tMarketingFee = calculateMarketingFee(amount);
    uint256 tDevelopmentFee = calculateDevelopmentFee(amount);
    uint256 tTransferAmount = amount.sub(tMarketingFee).sub(tDevelopmentFee);
    return (tTransferAmount, tMarketingFee, tDevelopmentFee);
  }
  
  function calculateMarketingFee(uint256 _amount) private view returns (uint256) {
    return _amount.mul(_marketingFee).div(
      10**2
    );
  }

  function calculateDevelopmentFee(uint256 _amount) private view returns (uint256) {
    return _amount.mul(_developmentFee).div(
      10**2
    );
  }
  
  function removeAllFee() private {
    if(_marketingFee == 0 && _developmentFee == 0) return;

    _previousMarketingFee = _marketingFee;
    _previousDevelopmentFee = _developmentFee;

    _marketingFee = 0;
    _developmentFee = 0;
  }
    
  function restoreAllFee() private {
    _marketingFee = _previousMarketingFee;
    _developmentFee = _previousDevelopmentFee;
  }
  
  function isExcludedFromFee(address account) public view returns(bool) {
    return _isExcludedFromFee[account];
  }
  
  function _transfer(address sender, address recipient, uint256 amount) internal {
    require(sender != address(0), "BEP20: transfer from the zero address");
    require(recipient != address(0), "BEP20: transfer to the zero address");
    require(amount > 0, "Transfer amount must be greater than zero");

    //indicates if fee should be deducted from transfer
    bool takeFee = true;
    
    //if any account belongs to _isExcludedFromFee account then remove the fee
    if(_isExcludedFromFee[sender] || _isExcludedFromFee[recipient]){
      takeFee = false;
    }
        
    //transfer amount, it will take tax fee
    _tokenTransfer(sender,recipient,amount,takeFee);
  }
  
  //this method is responsible for taking all fee, if takeFee is true
  function _tokenTransfer(address sender, address recipient, uint256 amount, bool takeFee) private {
    if(!takeFee) removeAllFee();
    _transferStandard(sender, recipient, amount);
    if(!takeFee) restoreAllFee();
  }
  
  function _transferStandard(address sender, address recipient, uint256 amount) private {
    (uint256 tTransferAmount, uint256 tMarketingFee, uint256 tDevelopmentFee) = _getValues(amount);
    _takeFeeMarketing(tMarketingFee);
    _takeFeeDevelopment(tDevelopmentFee);
    
    _balances[sender] = _balances[sender].sub(amount, "BEP20: transfer amount exceeds balance");
    _balances[recipient] = _balances[recipient].add(tTransferAmount);
    emit Transfer(sender, recipient, tTransferAmount);
  }
  
  function _approve(address owner, address spender, uint256 amount) private {
    require(owner != address(0), "BEP20: approve from the zero address");
    require(spender != address(0), "BEP20: approve to the zero address");

    _allowances[owner][spender] = amount;
    emit Approval(owner, spender, amount);
  }
  
  function _mint(address account, uint256 amount) internal {
    require(account != address(0), "BEP20: mint to the zero address");

    _totalSupply = _totalSupply.add(amount);
    _balances[account] = _balances[account].add(amount);
    emit Transfer(address(0), account, amount);
  }
}