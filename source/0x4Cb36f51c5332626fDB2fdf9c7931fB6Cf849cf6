// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

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

// File: @openzeppelin\contracts\utils\Context.sol

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

// File: @openzeppelin\contracts\access\Ownable.sol

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

// File: @openzeppelin\contracts\security\Pausable.sol

/**
 * @dev Contract module which allows children to implement an emergency stop
 * mechanism that can be triggered by an authorized account.
 *
 * This module is used through inheritance. It will make available the
 * modifiers `whenNotPaused` and `whenPaused`, which can be applied to
 * the functions of your contract. Note that they will not be pausable by
 * simply including this module, only once the modifiers are put in place.
 */
abstract contract Pausable is Context {
    /**
     * @dev Emitted when the pause is triggered by `account`.
     */
    event Paused(address account);

    /**
     * @dev Emitted when the pause is lifted by `account`.
     */
    event Unpaused(address account);

    bool private _paused;

    /**
     * @dev Initializes the contract in unpaused state.
     */
    constructor() {
        _paused = false;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        _requireNotPaused();
        _;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is paused.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    modifier whenPaused() {
        _requirePaused();
        _;
    }

    /**
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Throws if the contract is paused.
     */
    function _requireNotPaused() internal view virtual {
        require(!paused(), "Pausable: paused");
    }

    /**
     * @dev Throws if the contract is not paused.
     */
    function _requirePaused() internal view virtual {
        require(paused(), "Pausable: not paused");
    }

    /**
     * @dev Triggers stopped state.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }

    /**
     * @dev Returns to normal state.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
    }
}

// File: ..\contracts\TGate.sol

interface ITOrbs {
    function mint(address to, uint256 amount) external;
}

interface IRedeem {
    function redeem(uint256 amount) external;
}

contract TOrbTokenSale is Ownable, Pausable {
    ITOrbs public tOrb;
    IERC20 public tKey;
    IRedeem public redeemContract;
    address public gamesAccount;
    address public operationsAccount;

    uint256 public constant BNB_RATE = 10**15; // 0.001 BNB = 1 TOrb
    uint256 public constant TKey_RATE = 25; // 25 TKey = 1 TOrb
    uint256 public constant GAMES_SHARE_BNB = 8 * 10**14; // 0.0008 BNB per TOrb minted
    uint256 public constant GAMES_SHARE_TKEY = 20; // 20 TKey per TOrb minted

    struct BonusRange {
        uint256 lowerBound;
        uint256 upperBound;
        uint256 bonusAmount;
    }

    BonusRange[] public bonusRanges;

    constructor(
        address _tOrbAddress,
        address _tKeyAddress,
        address _gamesAccount,
        address _operationsAccount,
        address _redeemContractAddress
    ) {
        require(_tOrbAddress != address(0), "Invalid TOrb address");
        require(_tKeyAddress != address(0), "Invalid TKey address");
        require(_gamesAccount != address(0), "Invalid Games Account address");
        require(_operationsAccount != address(0), "Invalid Operations Account address");

        tOrb = ITOrbs(_tOrbAddress);
        tKey = IERC20(_tKeyAddress);
        gamesAccount = _gamesAccount;
        operationsAccount = _operationsAccount;
        redeemContract = IRedeem(_redeemContractAddress);
    }

    function buyWithBNB() public payable whenNotPaused {
        require(msg.value > 0 && msg.value % BNB_RATE == 0, "Invalid BNB amount");

        uint256 amountToMint = (msg.value * 10**18) / BNB_RATE;
        uint256 bonus = calculateBonus(amountToMint);
        uint256 totalMint = amountToMint + bonus;

        tOrb.mint(msg.sender, totalMint);

        uint256 gamesShare = totalMint * GAMES_SHARE_BNB;
        uint256 operationsShare = msg.value - gamesShare;

        payable(gamesAccount).transfer(gamesShare);
        payable(operationsAccount).transfer(operationsShare);
    }

    function buyWithTKey(uint256 amountInTKey) external whenNotPaused {
    uint256 amount = amountInTKey * 10**18;
    require(amount > 0 && amount % TKey_RATE == 0, "Invalid TKey amount");
    require(tKey.transferFrom(msg.sender, address(this), amount), "TKey transfer failed");

    uint256 amountToMint = (amountInTKey * 10**18) / TKey_RATE;
    uint256 bonus = calculateBonus(amountToMint);
    uint256 totalMint = amountToMint + bonus;

    tOrb.mint(msg.sender, totalMint);

    // Redeem the TKey for BNB
    redeemContract.redeem(amount);


    uint256 gamesShare = (totalMint * GAMES_SHARE_BNB) / 10**18;
    uint256 operationsShare = address(this).balance - gamesShare;

    payable(gamesAccount).transfer(gamesShare);
    payable(operationsAccount).transfer(operationsShare);
}

    function calculateBonus(uint256 amountToMint) internal view returns (uint256) {
        for (uint256 i = 0; i < bonusRanges.length; i++) {
            if (amountToMint >= bonusRanges[i].lowerBound && amountToMint < bonusRanges[i].upperBound) {
                return bonusRanges[i].bonusAmount;
            }
        }
        return 0;
    }

    function setGamesAccount(address _gamesAccount) external onlyOwner {
        require(_gamesAccount != address(0), "Invalid Games Account address");
        gamesAccount = _gamesAccount;
    }

    function setOperationsAccount(address _operationsAccount) external onlyOwner {
        require(_operationsAccount != address(0), "Invalid Operations Account address");
        operationsAccount = _operationsAccount;
    }

    function addBonusRange(uint256 lowerBound, uint256 upperBound, uint256 bonusAmount) external onlyOwner {
        bonusRanges.push(BonusRange(lowerBound, upperBound, bonusAmount));
    }

    function updateBonusRange(uint256 index, uint256 lowerBound, uint256 upperBound, uint256 bonusAmount) external onlyOwner {
        require(index < bonusRanges.length, "Index out of bounds");
        bonusRanges[index] = BonusRange(lowerBound, upperBound, bonusAmount);
    }

    function resetBonusRanges() external onlyOwner {
    delete bonusRanges;
}

    function getBonusRange(uint256 index) external view returns (uint256 lowerBound, uint256 upperBound, uint256 bonusAmount) {
        require(index < bonusRanges.length, "Index out of bounds");
        BonusRange memory range = bonusRanges[index];
        return (range.lowerBound, range.upperBound, range.bonusAmount);
    }

    function getAllBonusRanges() external view returns (BonusRange[] memory) {
    return bonusRanges;
}

    function pauseSale() external onlyOwner {
        _pause();
    }

    function unpauseSale() external onlyOwner {
        _unpause();
    }

    function withdrawTKey() external onlyOwner {
    uint256 amount = tKey.balanceOf(address(this));
    require(tKey.transfer(msg.sender, amount), "TKey withdrawal failed");
}

    receive() external payable {
        buyWithBNB();
    }
}