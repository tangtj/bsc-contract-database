// SPDX-License-Identifier: MIT
pragma solidity 0.8.10;

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function burn(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}


abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

     constructor ()   {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

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


library SafeMath {

    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        uint256 c = a + b;
        if (c < a) return (false, 0);
        return (true, c);
    }

    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        if (b > a) return (false, 0);
        return (true, a - b);
    }

    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {

        if (a == 0) return (true, 0);
        uint256 c = a * b;
        if (c / a != b) return (false, 0);
        return (true, c);
    }

    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        if (b == 0) return (false, 0);
        return (true, a / b);
    }

    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        if (b == 0) return (false, 0);
        return (true, a % b);
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) return 0;
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        return a / b;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: modulo by zero");
        return a % b;
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        return a - b;
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        return a / b;
    }
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        return a % b;
    }
}

library Address {
  
    function isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly { size := extcodesize(account) }
        return size > 0;
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");
        (bool success, ) = recipient.call{ value: amount }("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
      return functionCall(target, data, "Address: low-level call failed");
    }

    function functionCall(address target, bytes memory data, string memory errorMessage) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }

    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    function functionCallWithValue(address target, bytes memory data, uint256 value, string memory errorMessage) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        require(isContract(target), "Address: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{ value: value }(data);
        return _verifyCallResult(success, returndata, errorMessage);
    }

    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        return functionStaticCall(target, data, "Address: low-level static call failed");
    }

    function functionStaticCall(address target, bytes memory data, string memory errorMessage) internal view returns (bytes memory) {
        require(isContract(target), "Address: static call to non-contract");
        (bool success, bytes memory returndata) = target.staticcall(data);
        return _verifyCallResult(success, returndata, errorMessage);
    }

    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionDelegateCall(target, data, "Address: low-level delegate call failed");
    }

    function functionDelegateCall(address target, bytes memory data, string memory errorMessage) internal returns (bytes memory) {
        require(isContract(target), "Address: delegate call to non-contract");

        (bool success, bytes memory returndata) = target.delegatecall(data);
        return _verifyCallResult(success, returndata, errorMessage);
    }

    function _verifyCallResult(bool success, bytes memory returndata, string memory errorMessage) private pure returns(bytes memory) {
        if (success) {
            return returndata;
        } else {
            if (returndata.length > 0) {
                assembly {
                    let returndata_size := mload(returndata)
                    revert(add(32, returndata), returndata_size)
                }
            } else {
                revert(errorMessage);
            }
        }
    }
}


contract ReentrancyGuard {

    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor () {
        _status = _NOT_ENTERED;
    }

    modifier nonReentrant() {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");
        _status = _ENTERED;
        _;
        _status = _NOT_ENTERED;
    }
}

library SafeERC20 {
    using SafeMath for uint256;
    using Address for address;
    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }
    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }
    function safeApprove(IERC20 token, address spender, uint256 value) internal {
        require((value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }
    function safeIncreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).add(value);
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }
    function safeDecreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).sub(value, "SafeERC20: decreased allowance below zero");
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function _callOptionalReturn(IERC20 token, bytes memory data) private {

        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        if (returndata.length > 0) { // Return data is optional
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}

contract TetherDxLock is Accessible,ReentrancyGuard{
    using SafeMath for uint256;
    uint256 public lockTime;

    mapping(address=>tedxUser) public lockUser;
    struct tedxUser{
        uint256 startTime;
        uint256 lockNum;
        uint256 extractNum;
    }

  address public tedxAddr = address(0x10AAFA6c808053115365A979981CA74F8b99A546);

   constructor ()   {
       lockTime = 420 * 86400;
       grantAccess(address(0xcA364201Ca6efB2396Df5E342Bc155a83A9A07eD));
       lockUser[address(0xcA364201Ca6efB2396Df5E342Bc155a83A9A07eD)].startTime = block.timestamp;
       lockUser[address(0xcA364201Ca6efB2396Df5E342Bc155a83A9A07eD)].lockNum = 1470000 * 1e18;
       lockUser[address(0xcA364201Ca6efB2396Df5E342Bc155a83A9A07eD)].extractNum = 0;

       grantAccess(address(0xC2fF3B623C1EE40b5D306842bDaab06bDF0FFa1D));
       lockUser[address(0xC2fF3B623C1EE40b5D306842bDaab06bDF0FFa1D)].startTime = block.timestamp;
       lockUser[address(0xC2fF3B623C1EE40b5D306842bDaab06bDF0FFa1D)].lockNum = 420000 * 1e18;
       lockUser[address(0xC2fF3B623C1EE40b5D306842bDaab06bDF0FFa1D)].extractNum = 0;

       grantAccess(address(0x2542c908E62Df9CC4e984e8dae9b9A165DBD7428));
       lockUser[address(0x2542c908E62Df9CC4e984e8dae9b9A165DBD7428)].startTime = block.timestamp;
       lockUser[address(0x2542c908E62Df9CC4e984e8dae9b9A165DBD7428)].lockNum = 105000 * 1e18;
       lockUser[address(0x2542c908E62Df9CC4e984e8dae9b9A165DBD7428)].extractNum = 0;

       grantAccess(address(0x2b94e70Db49dC48ebbcE36e8c2614c91a85611f8));
       lockUser[address(0x2b94e70Db49dC48ebbcE36e8c2614c91a85611f8)].startTime = block.timestamp;
       lockUser[address(0x2b94e70Db49dC48ebbcE36e8c2614c91a85611f8)].lockNum = 105000 * 1e18;
       lockUser[address(0x2b94e70Db49dC48ebbcE36e8c2614c91a85611f8)].extractNum = 0;
   }

   function getRemaining(address addr) public view returns(uint256){
       if(lockUser[addr].lockNum > lockUser[addr].extractNum){
            return lockUser[addr].lockNum.sub(lockUser[addr].extractNum);
       }else{
           return 0;
       }
   }

   function getFreed(address addr) public view returns(uint256){
      if(block.timestamp > lockUser[addr].startTime && getRemaining(addr) > 0){
          uint256 freed = block.timestamp.sub(lockUser[addr].startTime).mul(lockUser[addr].lockNum).div(lockTime);
          if(freed > getRemaining(addr)){
              return getRemaining(addr);
          }else{
              return freed;
          }
      }else{
          return 0;
      }
   }

   function extract() public onlyAccessed returns(bool){
       require(msg.sender == tx.origin,"not allowed");
       uint256 freed = getFreed(msg.sender);
       if(freed > 0){
           lockUser[msg.sender].startTime = block.timestamp;
           IERC20(tedxAddr).transfer(msg.sender,freed);
           lockUser[msg.sender].extractNum = lockUser[msg.sender].extractNum.add(freed);
       }
       return true;
    }

}