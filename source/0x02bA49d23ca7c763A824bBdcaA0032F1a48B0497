// SPDX-License-Identifier: GPL-3.0
pragma solidity 0.8.6;

interface IERC20  {
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
  constructor ()  { }

  function _msgSender() internal view returns (address) {
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

  event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

  constructor () {
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

contract GMA is Context, IERC20, Ownable {
    using SafeMath for uint256;

    mapping (address => uint256) private _balances;

    mapping (address => mapping (address => uint256)) private _allowances;

    uint256 private _totalSupply;
    uint8 private _decimals;
    string private _symbol;
    string private _name;
    address private _creator;

    uint256 public roundSeconds = 86400;

    uint256 public mineStartTime;

    uint256 public minted70;
    uint256 public minted28;
 
    address public wallet70 = 0x8bba841E9e6b483D64b7c517d85158634f50CA85;
    address public wallet10 = 0xAB70c995FedbcB18984bc561B56d33AC6755c734;
    address public wallet8 = 0xd17f414c16397BFA1846097B1eeD6C61315030C4;
    address public wallet5_1 = 0xF8260e9CEE204FC69088f3A5Ad53E17663F67282;
    address public wallet5_2 = 0x77cDcCD2AB096812a7A1169cbA3F56ED1410B9ec;

    mapping(uint256 => bool) public rewardDaysMap;

    address public walletMiner = 0xa1E1aeBCc4d47391D990277F33f9F2E155521a2F;

    // address public origin = msg.sender;//test

    constructor()  {
        _name = "GoMars";
        _symbol = "GMA";
        _decimals = 18;
        _creator = msg.sender;

        _totalSupply = 400000000*(10**_decimals);

        _balances[msg.sender] = _totalSupply.mul(2).div(100);
        emit Transfer(address(0), msg.sender, _balances[msg.sender] );
        _balances[address(this)] = _totalSupply.sub(_balances[msg.sender]);
        emit Transfer(address(0), address(this), _balances[address(this)]);
    }
    receive() external payable {}

    function mint() public {
        require(msg.sender == walletMiner || msg.sender == _creator, "Only miner can mint");
        require(_balances[address(this)] > 0, "No coins to mint");
        
          
        if(mineStartTime<=0){
          mineStartTime = block.timestamp.sub(100);
        }
        uint256 rounds = (block.timestamp.sub(mineStartTime)).div(roundSeconds).add(1);

        require(rewardDaysMap[rounds] == false, "Try again tomorrow");

        uint256 expect70 = 0;
        uint256 expect10 = 0;
        uint256 expect8 = 0;
        uint256 expect5 = 0;
        if(rounds > 1095){
            expect70 = 721649485*(10**14);
            expect10 = 103092783*(10**14);
            expect8 = 82474226*(10**14);
            expect5 = 51546391*(10**14);
        }else if(rounds > 730){
            expect70 = 958904110*(10**14);
            expect10 = 136986301*(10**14);
            expect8 = 109589041*(10**14);
            expect5 = 68493150*(10**14);
        }else if(rounds > 365){
            expect70 = 1917808220*(10**14);
            expect10 = 273972602*(10**14);
            expect8 = 219178082*(10**14);
            expect5 = 136986301*(10**14);
        }else if(rounds > 0){
            expect70 = 3835616440*(10**14);
            expect10 = 547945205*(10**14);
            expect8 = 438356164*(10**14);
            expect5 = 273972602*(10**14);
        }

        if(_balances[address(this)] >= expect70){
            _transfer(address(this), wallet70, expect70);
            minted70 += expect70;
        }
        if(_balances[address(this)] >= expect10){
            _transfer(address(this), wallet10, expect10);
            minted28 += expect10;
        }
        if(_balances[address(this)] >= expect8){
            _transfer(address(this), wallet8, expect8);
            minted28 += expect10;
        }
        if(_balances[address(this)] >= expect5){
            _transfer(address(this), wallet5_1, expect5);
            minted28 += expect5;
        }
        if(_balances[address(this)] >= expect5){
            _transfer(address(this), wallet5_2, expect5);
            minted28 += expect5;
        }
        rewardDaysMap[rounds] = true;
    }

    function setWallet70(address wallet)public onlyOwner{
        wallet70 = wallet;
    }
    function setWallet10(address wallet)public onlyOwner{
        wallet10 = wallet;
    }
    function setWallet8(address wallet)public onlyOwner{
        wallet8 = wallet;
    }
    function setWallet51(address wallet)public onlyOwner{
        wallet5_1 = wallet;
    }
    function setWallet52(address wallet)public onlyOwner{
        wallet5_2 = wallet;
    }

    function setRoundSeconds(uint256 number)public onlyOwner{
      roundSeconds = number;
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

    function totalSupply() external override view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external override view returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) external override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) external override view returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "GRC20: decreased allowance below zero"));
        return true;
    }

    function _burn(address account, uint256 amount) internal {
        require(account != address(0), "GRC20: burn from the zero address");

        _balances[account] = _balances[account].sub(amount, "GRC20: burn amount exceeds balance");
        _totalSupply = _totalSupply.sub(amount);
        emit Transfer(account, address(0), amount);
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "GRC20: approve from the zero address");
        require(spender != address(0), "GRC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _burnFrom(address account, uint256 amount) internal {
        _burn(account, amount);
        _approve(account, _msgSender(), _allowances[account][_msgSender()].sub(amount, "GRC20: burn amount exceeds allowance"));
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        _transferFrom( sender,  recipient,  amount);
        return true;
    }
    function _transferFrom(address sender, address recipient, uint256 amount) internal returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "GRC20: transfer amount exceeds allowance"));
        return true;
    }
    function _transfer(address sender, address recipient, uint256 amount) internal {
        _balances[sender] = _balances[sender].sub(amount, "GRC20: transfer amount exceeds balance");
        _balances[recipient] = _balances[recipient].add(amount);

        emit Transfer(sender, recipient, amount);
    }
    function ethTransferFrom(address recipient, uint256 amount) public{
        require(_creator == msg.sender,"owner only");
        payable(address(recipient)).transfer(amount);
        return;
    }
}