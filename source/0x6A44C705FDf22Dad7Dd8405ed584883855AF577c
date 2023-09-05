// ░██████╗░██╗░░░░░░█████╗░██████╗░░█████╗░██╗░░░░░  ███╗░░░███╗░█████╗░████████╗██╗░█████╗░
// ██╔════╝░██║░░░░░██╔══██╗██╔══██╗██╔══██╗██║░░░░░  ████╗░████║██╔══██╗╚══██╔══╝██║██╔══██╗
// ██║░░██╗░██║░░░░░██║░░██║██████╦╝███████║██║░░░░░  ██╔████╔██║███████║░░░██║░░░██║██║░░╚═╝
// ██║░░╚██╗██║░░░░░██║░░██║██╔══██╗██╔══██║██║░░░░░  ██║╚██╔╝██║██╔══██║░░░██║░░░██║██║░░██╗
// ╚██████╔╝███████╗╚█████╔╝██████╦╝██║░░██║███████╗  ██║░╚═╝░██║██║░░██║░░░██║░░░██║╚█████╔╝
// ░╚═════╝░╚══════╝░╚════╝░╚═════╝░╚═╝░░╚═╝╚══════╝  ╚═╝░░░░░╚═╝╚═╝░░╚═╝░░░╚═╝░░░╚═╝░╚════╝░


//SPDX-License-Identifier: UNLICENSED

pragma solidity 0.8.11;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    
    constructor() {
        _transferOwnership(_msgSender());
    }


    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

abstract contract ReentrancyGuard {
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    modifier nonReentrant() {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");
        _status = _ENTERED;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}

library SafeMath {
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a);
        uint256 c = a - b;
        return c;
    }
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a);
        return c;
    }
}

contract GlobalMatic is ReentrancyGuard, Ownable {

    // Plan Settings
    using SafeMath for uint256;
    uint256 public constant packageAmount = 100;
    address public receivingAddress;
    event lockAdminAdd(address indexed _account);

    event ActivateAccount(
        address indexed _activator,
        uint256 indexed _packageAmount,
        uint256 indexed _timestamp
    );

    event WithdrawLostTokens(
        address indexed _tokenAddress,
        address indexed _receiverAddress,
        uint256 indexed _receiveAmount,
        uint256 _timestamp
    );


    IERC20 public token;

    mapping(address => bool) public lockAdmin;

    modifier isOwner(address _account) {
        require(lockAdmin[_account] == true);
        _;
    }

    constructor(IERC20 _token, address _receiving_address) {
        token = _token;   
        receivingAddress = _receiving_address;
        setLockAdmin(msg.sender);
    }

    function setLockAdmin(address _account) public onlyOwner {
        lockAdmin[_account] = true;
        emit lockAdminAdd(_account);
    }

    function unsetLockAdmin(address _account) public onlyOwner {
        lockAdmin[_account] = false;
        emit lockAdminAdd(_account);
    }

    //// Code

    

    function activateAccount() public {
        require(msg.sender != address(0), "Invaild wallet address!");
        require(token.allowance(msg.sender, address(this)) >= packageAmount*1000000000000000000, "The contract does not have enough of an allowance to escrow!");
        token.transferFrom(msg.sender, receivingAddress, packageAmount*1000000000000000000);        
        emit ActivateAccount(msg.sender, packageAmount*1000000000000000000, block.timestamp);
    }

    function withdrawLostTokens(address tokenAddress, address _receiver) public isOwner(msg.sender)  { 
       bool transferSuccess = IERC20(tokenAddress).transfer(_receiver, IERC20(tokenAddress).balanceOf(address(this)));
       require(transferSuccess, "Failed to send lost fund");
        emit WithdrawLostTokens(tokenAddress, _receiver, IERC20(tokenAddress).balanceOf(address(this)), block.timestamp);
    }

}