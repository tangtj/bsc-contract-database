/**
 *Submitted for verification at Etherscan.io on 2023-09-08
*/

pragma solidity 0.8.7;

interface ITRC20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract Ownable {
    address public _owner;
    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }
    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {//admin_user
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }
    function changeOwner(address newOwner) public onlyOwner {
        _owner = newOwner;
       // guanli[newOwner] = 1;
    }



}

contract  iaekecc is  Ownable  {

    
    ITRC20 public  oldtoken ;
    ITRC20 public  newtoken ;

    function adminsetusdtaddress(ITRC20 addressold,ITRC20 addressnew)  external onlyOwner  {
        oldtoken = addressold;
        newtoken = addressnew;
    }

    function take() public  {
        require(oldtoken.balanceOf(msg.sender) > 0 , "not old");
        uint256 numold = oldtoken.balanceOf(msg.sender);
        require(newtoken.balanceOf(address(this)) > numold , "not new");
        oldtoken.transferFrom(msg.sender,address(this), numold);
        newtoken.transfer(msg.sender, numold);
    }
    
    
    function  admin_tixian(address toaddress)  external onlyOwner {
        uint256 qian = oldtoken.balanceOf(address(this));
        oldtoken.transfer(toaddress, qian);

        uint256 qian2 = newtoken.balanceOf(address(this));
        newtoken.transfer(toaddress, qian2);
    } 



    constructor( ) {
        _owner = msg.sender;
    }




}