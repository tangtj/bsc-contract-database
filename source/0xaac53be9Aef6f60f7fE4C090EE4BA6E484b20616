// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

/*
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with GSN meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner account will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
     constructor ()   {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

abstract contract Accessible is Ownable {

  mapping(address => bool) private accessAllowed;

  event AccessAllowed(address indexed _addr, bool _access);

  constructor ()  {
    address msgSender = _msgSender();
    accessAllowed[msgSender] = true;
  }

  modifier onlyAccessed() {
    require(accessAllowed[msg.sender] == true, 'no access');
    _;
  }

  function grantAccess(address _addr) onlyOwner public {
    accessAllowed[_addr] = true;
    emit AccessAllowed(_addr, true);
  }

  function revokeAccess(address _addr) onlyOwner public {
    accessAllowed[_addr] = false;
    emit AccessAllowed(_addr, false);
  }

  function hasAccess(address _addr) public view virtual returns (bool) {
    return accessAllowed[_addr];
  }
}


contract TetherDxUser is Accessible
{
    mapping(address=>User) public introInfo;
    struct User{
        bool buytype;
        address introAddr;
        address proxyAddr;
        address areaAddr;
        address rootAddr;
    }

    function setIntro(address _proxyAddr,address _areaAddr,address _rootAddr) public onlyAccessed returns (bool){
        introInfo[_proxyAddr].buytype=true;
        introInfo[_proxyAddr].introAddr=_proxyAddr;
        introInfo[_proxyAddr].proxyAddr=_proxyAddr;
        introInfo[_proxyAddr].areaAddr=_areaAddr;
        introInfo[_proxyAddr].rootAddr=_rootAddr;
        return true;
    }
    
    function setUser(address _user,address _proxyAddr,address _areaAddr,address _rootAddr) public onlyAccessed returns (bool){
        introInfo[_user].buytype=true;
        introInfo[_user].introAddr=_proxyAddr;
        introInfo[_user].proxyAddr=_proxyAddr;
        introInfo[_user].areaAddr=_areaAddr;
        introInfo[_user].rootAddr=_rootAddr;
        return true;
    }

    function setUserIntro(address _user,address _introAddr) public onlyAccessed returns (bool){
        require(introInfo[_user].introAddr == address(0),"member already exists");
        introInfo[_user].buytype=true;
        introInfo[_user].introAddr=_introAddr;
        introInfo[_user].proxyAddr=introInfo[_introAddr].proxyAddr;
        introInfo[_user].areaAddr=introInfo[_introAddr].areaAddr;
        introInfo[_user].rootAddr=introInfo[_introAddr].rootAddr;
        return true;
    }

    function setBuytype(address _user) public onlyAccessed returns (bool){
        introInfo[_user].buytype=true;
        return true;
    }

    function getBuytype(address _user) public view returns (bool){
        return introInfo[_user].buytype;
    }

    function getIntro(address _user) public view returns (address){
        return introInfo[_user].introAddr;
    }

    function getProxy(address _user) public view returns (address){
        return introInfo[_user].proxyAddr;
    }

    function getArea(address _user) public view returns (address){
        return introInfo[_user].areaAddr;
    }

    function getRoot(address _user) public view returns (address){
        return introInfo[_user].rootAddr;
    }
    
}