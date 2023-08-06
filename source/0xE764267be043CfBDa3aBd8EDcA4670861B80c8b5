// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
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

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
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
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

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
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

contract Airdrop is Ownable {
    IERC20 public token;
    uint256 public totalAirdropAmount = 1000000e18;
    uint256 public individualAirdropAmount = 200e18;
    uint256 public totalTokensDistributed = 0;
    uint256 public claimFee = 0.002 ether;
    uint256 public referRewardAmount = 20e18;
    bool public isWhitelistOn;

    mapping(address => bool) public whitelist;
    mapping(address => address) public referrals;
    mapping(address => bool) public claimed;

    event AirdropClaimed(address indexed user, address indexed referrer);

    constructor(address _tokenAddress) {
        token = IERC20(_tokenAddress);
        isWhitelistOn = true;
    }

    function addToWhitelist(address[] calldata addresses) external onlyOwner {
        for (uint256 i = 0; i < addresses.length; i++) {
            whitelist[addresses[i]] = true;
        }
    }

    function removeFromWhitelist(address[] calldata addresses) external onlyOwner {
        for (uint256 i = 0; i < addresses.length; i++) {
            whitelist[addresses[i]] = false;
        }
    }

    function toggleWhitelist(bool _isWhitelistOn) external onlyOwner {
        isWhitelistOn = _isWhitelistOn;
    }

    function claimAirdrop(address referrer) external payable {
    // If whitelist is on, check if the user is eligible for the airdrop
    if (isWhitelistOn) {
        require(whitelist[msg.sender], "You are not eligible for the airdrop");
    }

    // Check if the airdrop has ended
    require(totalTokensDistributed + individualAirdropAmount <= totalAirdropAmount, "Airdrop has ended");
    require(!claimed[msg.sender], "You have already claimed the airdrop");

    // Check if the referrer is in the whitelist (only if whitelist is on)
    require(!isWhitelistOn || whitelist[referrer], "Invalid referrer");

    // Check if the user has paid the claim fee
    require(msg.value >= claimFee, "Insufficient claim fee");

    referrals[msg.sender] = referrer;
    claimed[msg.sender] = true;

    // Transfer tokens to the user
    require(token.transfer(msg.sender, individualAirdropAmount), "Token transfer failed");
    totalTokensDistributed += individualAirdropAmount;

    // Send the claim fee to the contract owner
    if (msg.value > 0) {
        payable(owner()).transfer(msg.value);
    }

    // Reward the referrer
    if (referrer != address(0)) {
        require(token.transfer(referrer, referRewardAmount), "Referrer reward transfer failed");
        totalTokensDistributed += referRewardAmount;
    }
    
    emit AirdropClaimed(msg.sender, referrer);
}


    function withdrawTokens(uint256 amount) external onlyOwner {
        require(amount <= token.balanceOf(address(this)), "Insufficient contract balance");
        require(token.transfer(owner(), amount), "Token transfer failed");
    }

    function setClaimFee(uint256 _claimFeeInBNB) external onlyOwner {
        claimFee = _claimFeeInBNB;
    }

    function setReferRewardAmount(uint256 _referRewardAmount) external onlyOwner {
        referRewardAmount = _referRewardAmount;
    }

    function setIndividualAirdropAmount(uint256 _individualAirdropAmount) external onlyOwner {
        individualAirdropAmount = _individualAirdropAmount;
    }

    function setToken(address _tokenAddress) public onlyOwner {
        token = IERC20(_tokenAddress);
    }

    function withdrawBNBFee() external onlyOwner {
        require(address(this).balance > 0, "No BNB to withdraw");
        payable(owner()).transfer(address(this).balance);
    }
}