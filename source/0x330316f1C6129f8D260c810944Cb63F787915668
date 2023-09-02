//gnomelabs.dev

pragma solidity 0.5.16;

interface IBEP20 {
  function totalSupply() external view returns (uint256);
  function decimals() external view returns (uint8);
  function symbol() external view returns (string memory);
  function name() external view returns (string memory);
  function getOwner() external view returns (address);
  function balanceOf(address account) external view returns (uint256);
  function transfer(address recipient, uint256 amount) external returns (bool);
  function allowance(address _owner, address spender) external view returns (uint256);
  function approve(address spender, uint256 amount) external returns (bool);
  function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
  event Transfer(address indexed from, address indexed to, uint256 value);
  event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface Ixg {
  function getCurrentPrice() external view returns (uint256);        
  function mint(address account, uint256 amount) external;  
  function isTokenRedeemable(address addr) external view returns (bool);
}

interface ILBTC {
  function getMA() external view returns (uint256);
  function getMA2() external view returns (uint256);
  function getCurrentPrice() external view returns (uint256);
  function aboveMinBalance(address account) external view returns (bool);
}

contract Context {
  constructor () internal { }

  function _msgSender() internal view returns (address payable) {
    return msg.sender;
  }

  function _msgData() internal view returns (bytes memory) {
    this; 
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

contract Ownable is Context {
  address private _owner;

  event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

  constructor () internal {
    address msgSender = _msgSender();
    _owner = msgSender;
    emit OwnershipTransferred(address(0), msgSender);
  }

  function owner() public view returns (address) {
    return _owner;
  }

  modifier onlyOwner() {
    require(_owner == _msgSender(), "Ownable: caller is not the owner");
    _;
  }

  function renounceOwnership() public onlyOwner {
    emit OwnershipTransferred(_owner, address(0));
    _owner = address(0);
  }

  function transferOwnership(address newOwner) public onlyOwner {
    _transferOwnership(newOwner);
  }

  function _transferOwnership(address newOwner) internal {
    require(newOwner != address(0), "Ownable: new owner is the zero address");
    emit OwnershipTransferred(_owner, newOwner);
    _owner = newOwner;
  }
}

contract BEP20Token is Context, IBEP20, Ownable {
  using SafeMath for uint256;

  mapping (address => uint256) private _balances;
  mapping (address => mapping (address => uint256)) private _allowances;

  uint256 private _totalSupply;
  uint8 public _decimals;
  string public _symbol;
  string public _name;

  mapping (address => uint256) private userGain;
  mapping (address => bool) public excluded;  
  mapping (address => bool) public convertible;  
  mapping (address => bool) public manager;  
  address[] public convertibleTokens;

  uint256 private lastUpdate;   
  uint256 private gL;
  uint256 private gG;
  uint256 private profit;
  uint256 private loss;
  uint256 private minPrice;
  uint256 private l;
  uint256 private feeB;
  uint256 private mintLimit;
  uint256 private burnLimit;
  uint256 private supply;
  uint256 private profitLimit;
  uint8 private feeC;    
  uint8 private feeA;  
  bool public paused = false;

  event Update(uint256 timeStamp, uint256 price, uint256 gain, uint256 xgPrice);
  event Win(address account, uint256 amount);
  event Loss(address account, uint256 amount);
  event PaidFee(address sender, address recipient, uint256 amount);

  ILBTC private lbtc;
  Ixg private xgCoin;

  address public xgCoinAddr;

  constructor() public {
    _name = "SXAU";
    _symbol = "SXAU";
    _decimals = 18;
  }

  function init() public onlyOwner {
    require(now < 1693562224);
    l = 1000 * 1e10;
    profitLimit = 30;
    minPrice = 1e18;
    feeA = 1;
    feeB = 60;
    feeC = 3;
    mintLimit = 100 * 100 * 1e18;
    burnLimit = 100 * 100 * 1e18;  
    initContracts(0x7702dF5199d8c3B4E78C0760bD0FC23c8c0b0C18, 0x4f34E19eA0582262165a265D64e5a9670757c8B3);
    lastUpdate = now;
    excluded[address(0x0)] = true;    
    manager[msg.sender] = true;
    convertible[xgCoinAddr] = true;    
  }

  function getOwner() external view returns (address) {
    return owner();
  }

  function decimals() external view returns (uint8) {
    return _decimals;
  }

  function symbol() external view returns (string memory) {
    return _symbol;
  }

  function name() external view returns (string memory) {
    return _name;
  }

  function totalSupply() external view returns (uint256) {
    return _totalSupply;
  }

  function balanceOf(address account) external view returns (uint256) {
    return excluded[account] ? _balances[account] : getNewBalance(account).mul(99999) / 100000;
  }

  function transfer(address recipient, uint256 amount) external returns (bool) {
    _transfer(_msgSender(), recipient, amount);
    return true;
  }

  function allowance(address owner, address spender) external view returns (uint256) {
    return _allowances[owner][spender];
  }

  function approve(address spender, uint256 amount) external returns (bool) {
    _approve(_msgSender(), spender, amount);
    return true;
  }

  function transferFrom(address sender, address recipient, uint256 amount) external returns (bool) {
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

  function burn(uint256 amount) public returns (bool) {
    _burn(_msgSender(), amount);
    return true;
  }

  function _transfer(address sender, address recipient, uint256 amount) internal {
    require(sender != address(0), "BEP20: transfer from the zero address");
    require(recipient != address(0), "BEP20: transfer to the zero address");
    require(amount > 0);
    update();
    calcBalance(sender);
    calcBalance(recipient);
    uint256 _fee;
    if(convertible[recipient]) {
      _fee = amount.mul(getTransferFee(sender, recipient) + getMintFee()) / 1000;
      require(amount > _fee);
      uint256 mintAmount = amount - _fee;
      _burn(sender, amount);
      Ixg(recipient).mint(sender, mintAmount);          
    } else {
      _balances[sender] = _balances[sender].sub(amount, "BEP20: transfer amount exceeds balance");    
      _balances[recipient] = _balances[recipient].add(amount);
      _fee = amount.mul(getTransferFee(sender, recipient)) / 1000;      
      require(_balances[recipient] > _fee);
      emit Transfer(sender, recipient, amount);      
      _burn(recipient, _fee);
    }
    if(_fee > 0) {
      emit PaidFee(sender, recipient, _fee);
    }
  }

  function _mint(address account, uint256 amount) internal {
    require(account != address(0), "BEP20: mint to the zero address");
    _totalSupply = _totalSupply.add(amount);
    _balances[account] = _balances[account].add(amount);
    emit Transfer(address(0), account, amount);
  }

  function _burn(address account, uint256 amount) internal {
    require(account != address(0), "BEP20: burn from the zero address");
    if(amount > 0) {
      _balances[account] = _balances[account].sub(amount, "BEP20: burn amount exceeds balance");
      _totalSupply = _totalSupply.sub(amount);
      emit Transfer(account, address(0), amount);
    }
  }

  function _approve(address owner, address spender, uint256 amount) internal {
    require(owner != address(0), "BEP20: approve from the zero address");
    require(spender != address(0), "BEP20: approve to the zero address");

    _allowances[owner][spender] = amount;
    emit Approval(owner, spender, amount);
  }

  function excludeAddr(address account, bool _b) external onlyOwner {
    update();
    calcBalance(account);
    excluded[account] = _b;
    userGain[account] = getGain();
  }

  function emergencyStop(bool _p) external onlyOwner {
    paused = _p;
  }

  function aboveProfitLimit() internal view returns (bool) {
    return ((profit + 1e23) > (loss + 1e23) * profitLimit / 10);
  }

  function setFees(uint8 _feeA,  uint256 _feeB, uint8 _feeC) external onlyOwner {
    require(_feeA <= 200);
    require(_feeB <= 2000);
    require(_feeC <= 200);
    feeA = _feeA;
    feeB = _feeB;
    feeC = _feeC;
  }

  function addManager(address addr, bool _b) external onlyOwner {
    manager[addr] = _b;
  }

  function settings(uint256 _l, uint256 _profitLimit, uint256 _minPrice) external onlyOwner {
    require(_l > 0 && _l < 1000000000);
    require(_profitLimit >= 10);      
    l = _l * 1e10;
    profitLimit = _profitLimit;
    minPrice = _minPrice;
  }

  function getDailyMintLimit() external view returns (uint256) {
    return mintLimit;
  }

  function getDailyBurnLimit() external view returns (uint256) {
    uint256 max = supply / 20;
    if(burnLimit > max && supply > 0) return max;
    return burnLimit;
  }

  function getMA() internal view returns (uint256) {
    return lbtc.getMA();
  }

  function getMA2() internal view returns (uint256) {
    return lbtc.getMA2();
  }

  function getTimePassed(uint256 _time) internal view returns (uint256) {
    uint256 _now = now;
    if(_time > _now) return 0;
    return _now - _time;
  }

  function initContracts(address a1, address a2) public onlyOwner {
    lbtc = ILBTC(a1);
    xgCoinAddr = a2;
    xgCoin = Ixg(xgCoinAddr);
  }

  function aboveMinBalance(address account) public view returns (bool) {
    return lbtc.aboveMinBalance(account);
  }

  function getCurrentPrice() public view returns (uint256) {
    return lbtc.getCurrentPrice();
  }

  function getNewBalance(address account) internal view returns (uint256) {
    return getNewBalance2(account, getGain());
  }

  function getNewBalance2(address account, uint256 _gain) internal view returns (uint256) {
    if(userGain[account] == 0 || paused || _gain == 0) return _balances[account];
    uint256 b = _balances[account] * (_gain / 1e30);
    if(b < _balances[account] / 1e30) return _balances[account];
    b = b / (userGain[account] / 1e30);
    return b;
  }

  function getGain() internal view returns (uint256) {
    uint256 _gL = gL + nextDecrease();
    uint256 _gG = gG + nextIncrease();
    uint256 c = getCurrentPrice();
    uint256 p = ((c / 1e7) ** 3) / ((getMA() / 1e7) ** 2) * 1e7;
    if(p > c * 5) p = c * 5;
    if(p < c / 5) p = c / 5;
    uint256 g = getMA2().add(2 * p).add(minPrice) / 1e10;
    uint256 MAX_INT = 0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff;
    g = g * ((1e20 + ((_gG > _gL) ? _gG - _gL : 0)) / 1e10) / ((1e20 + ((_gL > _gG) ? _gL - _gG : 0)) / 1e10);
    return (MAX_INT / g) / 1e10;
  }

  function nextDecrease() internal view returns (uint256) {
    uint256 c = getCurrentPrice();
    uint256 _ma = getMA();
    if(c > _ma) return 0;
    uint256 temp = _ma - c;
    if(temp > c / 4) temp = c / 4;
    uint256 r = temp * getTimePassed(lastUpdate) * l / c;
    return r * feeB / 100; 
  }

  function nextIncrease() internal view returns (uint256) {
    uint256 c = getCurrentPrice();
    uint256 _ma = getMA();
    if(_ma > c) return 0;
    return (c - _ma) * getTimePassed(lastUpdate) * l / c;
  }

  function update() public {
    uint256 c = getCurrentPrice();
    if(now > (lastUpdate + 86400)) {
      if(c > getMA()) {
        gG = gG.add(nextIncrease());
      } else {
        gL = gL.add(nextDecrease());
      }
      supply = (supply * 2 + _totalSupply) / 3;
      lastUpdate = now;
    }
    emit Update(now, c, getGain(), xgCoin.getCurrentPrice());
  }

  function canBeConverted(address addr) public view returns(bool) {
    return (addr == xgCoinAddr || (convertible[addr] && xgCoin.isTokenRedeemable(addr)));
  }

  function mint(address account, uint256 amount) public returns(bool) {
    require(amount > 0);
    require(xgCoin.isTokenRedeemable(address(this)));
    require(canBeConverted(msg.sender));
    update();
    calcBalance(account);
    uint256 mintAmount = amount;
    if(msg.sender == xgCoinAddr) {
      uint256 _fee = mintAmount.mul(getTransferFee(address(0x0), account) + getMintFee()) / 1000;
      require(mintAmount > _fee);
      mintAmount = amount - _fee;
      emit PaidFee(msg.sender, account, _fee);
    }
    _mint(account, mintAmount);
  }
  
  function makeConvertible(address addr, bool _b) external onlyOwner {
    require(!convertible[address(0x1)]);
    if(!_b) {
      convertibleTokens = new address[](0);
      convertibleTokens.push(xgCoinAddr);
    } else {
      convertibleTokens.push(addr);
    }
    convertible[addr] = _b;
  }
  
  function updateAccounts(address[] calldata accounts) external {
    update();
    for(uint256 i = 0; i < accounts.length; i++) calcBalance(accounts[i]);
  }

  function updateAccount(address account) external {
    update();
    calcBalance(account);
  }

  function calcBalance(address account) internal {
    if(excluded[account]) return;
    uint256 _gain = getGain();
    uint256 newBalance = getNewBalance2(account, _gain);
    uint256 diff;
    if(newBalance > _balances[account]) {
      diff = newBalance - _balances[account];
      _totalSupply = _totalSupply.add(diff);
      profit = profit.add(diff);
      emit Win(account, diff);          
    } else if(newBalance < _balances[account] ) {
      diff = _balances[account] - newBalance;
      _totalSupply = _totalSupply.sub(diff);
      loss = loss.add(diff);
      emit Loss(account, diff);
    }
    if(userGain[account] != _gain) {
      userGain[account] = _gain;
    }
    if(diff > 0) {
      if(!paused && aboveProfitLimit()) {
        paused = true;        
        mintLimit = 1000 * 10e18;        
      } else {
        _balances[account] = newBalance;
      }
    }
  }

  function setLimits(uint256 _mintLimit, uint256 _burnLimit) public {
    require(manager[msg.sender]);
    mintLimit = _mintLimit;
    burnLimit = _burnLimit;
  }

  function getTransferFee(address sender, address recipient) public view returns (uint256) {
    if((excluded[sender] || aboveMinBalance(sender)) && (excluded[recipient] || aboveMinBalance(recipient))) { 
      return feeA;
    }
    return feeA + 1;
  }

  function getMintFee() public view returns (uint256) {
    return feeC;
  }

  function getUser(address _a) external view returns (uint256[4] memory) {
    return [getNewBalance(_a), _balances[_a], userGain[_a], excluded[_a] ? 1 : 0];
  }

  function getGlobals() external view returns (uint256[10] memory) {
    return [getCurrentPrice(), getGain(), gL, gG, nextDecrease(), nextIncrease(), profit, loss, xgCoin.getCurrentPrice(), supply];
  }

}