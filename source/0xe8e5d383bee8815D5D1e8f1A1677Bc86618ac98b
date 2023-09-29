// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

interface IERC20_EXT {
    function name() external view returns (string memory);
    function decimals() external view returns (uint256);
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

    /**
     * @dev The caller account is not authorized to perform an operation.
     */
    error OwnableUnauthorizedAccount(address account);

    /**
     * @dev The owner is not a valid owner account. (eg. `address(0)`)
     */
    error OwnableInvalidOwner(address owner);

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the address provided by the deployer as the initial owner.
     */
    constructor(address initialOwner) {
        _transferOwnership(initialOwner);
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
        if (owner() != _msgSender()) {
            revert OwnableUnauthorizedAccount(_msgSender());
        }
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
        if (newOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
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

library SafeMath {

  /**
  * @dev Multiplies two numbers, throws on overflow.
  */
  function mul(uint256 a, uint256 b) internal pure returns (uint256 c) {
    if (a == 0) {
      return 0;
    }
    c = a * b;
    assert(c / a == b);
    return c;
  }

  /**
  * @dev Integer division of two numbers, truncating the quotient.
  */
  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    // assert(b > 0); // Solidity automatically throws when dividing by 0
    // uint256 c = a / b;
    // assert(a == b * c + a % b); // There is no case in which this doesn't hold
    return a / b;
  }

  /**
  * @dev Subtracts two numbers, throws on overflow (i.e. if subtrahend is greater than minuend).
  */
  function sub(uint256 a, uint256 b) internal pure returns (uint256) {
    assert(b <= a);
    return a - b;
  }

  /**
  * @dev Adds two numbers, throws on overflow.
  */
  function add(uint256 a, uint256 b) internal pure returns (uint256 c) {
    c = a + b;
    assert(c >= a);
    return c;
  }
}


interface IERC20 {
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

    /**
     * @dev Returns the value of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the value of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves a `value` amount of tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 value) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets a `value` amount of tokens as the allowance of `spender` over the
     * caller's tokens.
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
    function approve(address spender, uint256 value) external returns (bool);

    /**
     * @dev Moves a `value` amount of tokens from `from` to `to` using the
     * allowance mechanism. `value` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}


contract BIPresale is Ownable{
    
    using SafeMath for uint;
    address public tokenAddress; 
    address public USDTAddress;
    uint public price;
    uint public tokenSold;
    bool public presaleStatus;
    uint public minimumDollar;
    uint public tokenPerUsdt;
    uint sellerPercentage;
    uint referrerPercentage;
    
    address payable public seller;

    event tokenPurchased(address buyer, uint price, uint tokenValue);

    constructor(
        address _tokenAddress,
        address _USDTAddress,
        uint _minimumDollar,
        uint _tokenPerUsdt,
        uint _sellerPercentage,
        uint _referrerPercentage
        
        ) Ownable(_msgSender()) {
        tokenAddress = _tokenAddress;
        seller = payable(_msgSender());
        USDTAddress = _USDTAddress;
        presaleStatus = true;
        minimumDollar = _minimumDollar * (10**18);
        tokenPerUsdt = _tokenPerUsdt * (10**18);
        sellerPercentage = _sellerPercentage;
        referrerPercentage = _referrerPercentage;
    }


    function tokenForSale() public view returns (uint){
        return IERC20(tokenAddress).allowance(seller, address(this));
    }


    function usdtToToken(uint _amount) public view returns (uint) {
        return _amount.mul(tokenPerUsdt).div(1e18);
    }

    function buy(uint USDTamount, address _referrerAddress, uint _x) public returns (bool){
        uint256 amountInWei = _toTokenDecimals(USDTamount, IERC20_EXT(USDTAddress).decimals(), 18);
        require(presaleStatus == true, "The presale has ended.");
        require(_msgSender() != address(0), "Invalid Address");
        require(_referrerAddress != address(0), "Invalid Address");
        require(amountInWei >= minimumDollar,"The minimum amount is $25."); 
        require(tokenSold <= _toTokenDecimals(tokenForSale(), IERC20_EXT(tokenAddress).decimals(), 18), "All tokens sold.");
        require(_toTokenDecimals(IERC20(USDTAddress).balanceOf(msg.sender), IERC20_EXT(USDTAddress).decimals(), 18) >= amountInWei, "USDT balance insufficient.");

        // Calculate Percentages
        uint sellerPercent = (amountInWei * sellerPercentage) / 100;
        uint referrerPercent = (amountInWei * referrerPercentage) / 100;

        // transfer USDT from Buyer to seller
        IERC20(USDTAddress).transferFrom(msg.sender, seller, sellerPercent);

        // transfer USDT from Buyer to _referrer
        IERC20(USDTAddress).transferFrom(msg.sender, _referrerAddress, referrerPercent);

        // calculate number of tokens
        uint256 numberOfTokens = usdtToToken(amountInWei);

        // require(numberOfTokens <= tokenForSale(), "Remaining tokens are less than the buying value: ");
        require(numberOfTokens <= _toTokenDecimals(tokenForSale(), IERC20_EXT(tokenAddress).decimals(), 18), "Remaining tokens worth less than purchase value.");

        // Transfer tokens to the buyer's address
        IERC20(tokenAddress).transferFrom(seller, _msgSender(), _toTokenDecimals(numberOfTokens, IERC20_EXT(USDTAddress).decimals(), 9) * _x);


        // Update tokenSold Variable
        tokenSold = tokenSold.add(numberOfTokens);

        // Fire tokenPurchased event
        emit tokenPurchased(_msgSender(), amountInWei, numberOfTokens);

        return true;

    }

    function updateTokenPerUsdt(uint _newTokenPerUsdt) public onlyOwner {
        tokenPerUsdt = _newTokenPerUsdt * (10**18);
    }

    function updateSeller(address payable newSeller) public onlyOwner{
        seller = newSeller;
    }

    function updateTokenAddress(address newTokenAddress) public onlyOwner{
        tokenAddress = newTokenAddress;
    }

    function withDrawToken(IERC20 _tokenAddress) public onlyOwner returns(bool){
        uint tokenBalance = _tokenAddress.balanceOf(address(this));
        _tokenAddress.transfer(seller, tokenBalance);
        return true;
    }

    function withDrawFunds() public onlyOwner returns(bool) {
        seller.transfer(address(this).balance);
        return true;
    }

    function _toTokenDecimals(
        uint256 _value,
        uint256 _from,
        uint256 _to
    ) private pure returns (uint256) {
        return (_value * 10 ** _to) / 10 ** _from;
    }


    function updateReferrerAndSellerPercentage(uint _newSellerPercentage, uint _newReferrerPercentage) public onlyOwner returns (bool){
        sellerPercentage = _newSellerPercentage;
        referrerPercentage = _newReferrerPercentage;
        return true;
    }
}