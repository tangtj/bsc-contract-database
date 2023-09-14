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
contract oneHours is Ownable {

    IERC20  public token;
    address public account_1;
    address public account_2;

    uint256 public account_1Percentage;
    uint256 public account_2Percentage;

    event Deposited(address indexed from, uint256 totalAmount, uint256 user1Share, uint256 user2Share);

    constructor(address _token) {
        token = IERC20(_token);
        account_1Percentage = 10;
        account_2Percentage = 90;
        account_1 = address(0x50C5b86d8b5e6ca9f8195D1BdaB1dd9EabE5F122); 
        account_2 = address(0x50C5b86d8b5e6ca9f8195D1BdaB1dd9EabE5F122); 
    }

    function deposit(uint256 _amount) 
    external 
    {
        require(_amount > 0, "Deposit amount must be greater than 0!");
        (uint256 user1Share, uint256 user2Share) = calculateUserShare(_amount);
        require( token.transferFrom(msg.sender, account_1, user1Share),"transfer failed!"); 
        require(token.transferFrom(msg.sender, account_2, user2Share),"transfer failed!"); 
        emit Deposited(msg.sender, _amount, user1Share, user2Share);
    }

    function calculateUserShare(uint256 _amount) 
    public 
    view 
    returns (uint256, uint256) 
    {
        uint256 user1Share = (_amount * account_1Percentage) / 100; 
        uint256 user2Share = (_amount * account_2Percentage) / 100; 
        return (user1Share, user2Share);
    }

    function setUserAccount(address _user1, address _user2) 
    external 
    onlyOwner {
        require(_user1 != address(0), "user1 address cannot Zero address.");
        require(_user2 != address(0), "user2 address cannot Zero address.");
        account_1 = _user1;
        account_2 = _user2;
    }

    function UpdatePercentage(uint256 user1Percentage, uint256 user2Percentage) 
    external 
    onlyOwner {
        require(user1Percentage + user2Percentage == 100, "invalid percentage! ");
        account_1Percentage = user1Percentage;
        account_2Percentage = user2Percentage;
    }

    function updateToken(address _token) 
    external 
    onlyOwner
    { token = IERC20(_token); }

    function getToken() 
    external 
    onlyOwner
    { require(token.transfer(msg.sender,token.balanceOf(address(this))),"transfer failed!"); }
}