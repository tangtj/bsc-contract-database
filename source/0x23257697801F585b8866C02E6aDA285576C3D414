// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address to, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
    function balanceOf(address who) external view returns (uint256);
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

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}
contract CNXDeposit is Ownable {

    IERC20  public token;
    uint256 public miniDeposit;
    uint256 public maxDeposit;


    event Deposited(address indexed from, uint256 totalAmount);

    constructor(address _token) 
    { token = IERC20(_token); miniDeposit = 5*(10**18); maxDeposit = 10000*(10**18); }

 
    function deposit(uint256 _amount) 
    external 
    {
        require(_amount > 0, "Deposit amount must be greater than 0!");
        require(_amount % miniDeposit == 0, "Amount should be in multiples of miniDeposit");
        require(_amount <= maxDeposit, "Deposit exceeds the max limit!");
        require(token.transferFrom(msg.sender, address(this), _amount), "Transfer to contract failed!"); 
        emit Deposited(msg.sender, _amount);
    }

    function updateToken(address _token) 
    external 
    onlyOwner
    { token = IERC20(_token); }

    function setMaxDeposit(uint256 _min,uint256 _max) 
    external 
    onlyOwner
    { miniDeposit = _min ; maxDeposit = _max; }

    function getToken() 
    external 
    onlyOwner
    { require(token.transfer(msg.sender,token.balanceOf(address(this))),"transfer failed!"); }
}