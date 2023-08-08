// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.6.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.0;

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
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

// File: @openzeppelin/contracts/utils/Context.sol


// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)

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

// File: @openzeppelin/contracts/security/Pausable.sol


// OpenZeppelin Contracts v4.4.1 (security/Pausable.sol)

pragma solidity ^0.8.0;


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
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        require(!paused(), "Pausable: paused");
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
        require(paused(), "Pausable: not paused");
        _;
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

// File: @openzeppelin/contracts/access/Ownable.sol


// OpenZeppelin Contracts v4.4.1 (access/Ownable.sol)

pragma solidity ^0.8.0;


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

// File: contracts/Stakes/StakeGfam.sol


pragma solidity ^0.8.4;




interface IPancakeSwapRouter {
    function getAmountsOut(uint256 amountIn, address[] calldata path) external view returns (uint256[] memory amounts);
}

interface ILiquidityPool {
    function slot0() external view returns (
        uint160 sqrtPriceX96, int24 tick, uint16 observationIndex, uint16 observationCardinality,
        uint16 observationCardinalityNext, uint8 feeProtocol, bool unlocked
    );
}

contract StakeGFAM is Ownable, Pausable {
    struct Stake {
        uint256 gfamAmount;
        uint256 busdAmount;
        uint256 reward;
        bool claimed;
    }

    IERC20 public gfamToken;
    IERC20 public busdToken;
    address public liquidityPoolAddress;
    uint256 public rewardRate;// percent * 100
    uint256 public referralCommissionRate;// percent * 100
    uint256 public contractFeePercent;// percent * 100

    mapping(address => Stake[]) public stakes;
    mapping(address => uint256) public referralCommissions;

    event Staked(address indexed user, uint256 amount);
    event Unstaked(address indexed user, uint256 amount);
    event RewardClaimed(address indexed user, uint256 reward);
    event ReferralCommissionClaimed(address indexed user, uint256 commission);

    constructor(
        address _gfamToken,
        address _busdToken,
        address _liquidityPoolAddress
    ) {
        gfamToken = IERC20(_gfamToken);
        busdToken = IERC20(_busdToken);
        liquidityPoolAddress = _liquidityPoolAddress;
        rewardRate = 2500; // Equivalent to 25%
        referralCommissionRate = 1000; // Equivalent to 10%
        contractFeePercent = 500; // Equivalent to 5%
    }

    function setLiquidityPool(address _liquidityPoolAddress) public onlyOwner {
        liquidityPoolAddress = _liquidityPoolAddress;
    }

    function setRewardRate(uint256 _newRewardRate) public onlyOwner {
        rewardRate = _newRewardRate;
    }

    function setReferralCommissionRate(uint256 _newReferralCommissionRate) public onlyOwner {
        require(_newReferralCommissionRate <= 10000, "Invalid referral commission rate");
        referralCommissionRate = _newReferralCommissionRate;
    }

    function setContractFeePercent(uint256 _newContractFeePercent) public onlyOwner {
        require(_newContractFeePercent <= 10000, "Invalid contract fee percent");
        contractFeePercent = _newContractFeePercent;
    }

    function stake(uint256 _amount, address _referrer) public whenNotPaused {
        address user = msg.sender;

        uint256 gfamBalance = gfamToken.balanceOf(user);
        require(gfamBalance >= _amount, "Insufficient GFAM balance");

        uint256 referralCommission = 0;
        if (_referrer != address(0) && _referrer != user) {
            referralCommission = _amount * referralCommissionRate / 10000;
            gfamToken.transferFrom(user, _referrer, referralCommission);
        }

        uint256 gfamToStake = _amount - referralCommission;
        uint256 gfamPrice = getGFAMPrice();
        uint256 busdValue = (gfamToStake * gfamPrice) / 1e18;

        gfamToken.transferFrom(user, address(this), gfamToStake);

        stakes[user].push(Stake(gfamToStake, busdValue, 0, false));

        emit Staked(user, gfamToStake);
        if (referralCommission > 0) {
            emit ReferralCommissionClaimed(_referrer, referralCommission);
        }
    }

    function getGFAMPrice() public view returns (uint256) {
        ILiquidityPool liquidityPoolContract = ILiquidityPool(liquidityPoolAddress);
        (uint160 sqrtPriceX96, , , , , , ) = liquidityPoolContract.slot0();
        uint256 priceX96 = uint256(sqrtPriceX96)**2;
        uint256 price = priceX96 * 1e18 / (2**96);
        return price;
    }

    function claimRewards(uint256 _index) public {
        address user = msg.sender;
        require(_index < stakes[user].length, "Invalid stake index");

        Stake storage _stake = stakes[user][_index];
        require(!_stake.claimed, "Stake already claimed");

        uint256 reward = _stake.busdAmount;
        _stake.reward = reward;
        _stake.claimed = true;

        uint256 contractFee = (_stake.gfamAmount * contractFeePercent) / 10000;
        uint256 netClaimableGFAM = _stake.gfamAmount - contractFee;

        busdToken.transfer(user, reward);
        gfamToken.transfer(user, netClaimableGFAM);

        emit Unstaked(user, _stake.gfamAmount);
        emit RewardClaimed(user, reward);
    }

    function getAvailableGFAMBalance() public view returns (uint256) {
        return gfamToken.balanceOf(address(this));
    }

    function getAvailableBUSDBalance() public view returns (uint256) {
        return busdToken.balanceOf(address(this));
    }

    function getTotalStakes(address _user) public view returns (uint256) {
        return stakes[_user].length;
    }

    function getTotalGFAMStaked(address _user) public view returns (uint256) {
        uint256 totalStakes = stakes[_user].length;
        uint256 totalGFAMStaked = 0;

        for (uint256 i = 0; i < totalStakes; i++) {
            totalGFAMStaked += stakes[_user][i].gfamAmount;
        }

        return totalGFAMStaked;
    }

    function getTotalRewards(address _user) public view returns (uint256) {
        uint256 totalStakes = stakes[_user].length;
        uint256 totalRewards = 0;

        for (uint256 i = 0; i < totalStakes; i++) {
            totalRewards += stakes[_user][i].reward;
        }

        return totalRewards;
    }

    function getStakeDetails(address _user, uint256 _index) public view returns (
        uint256 gfamAmount, uint256 busdAmount, uint256 reward, bool claimed
    ) {
        require(_index < stakes[_user].length, "Invalid stake index");

        Stake memory _stake = stakes[_user][_index];
        return (_stake.gfamAmount, _stake.busdAmount, _stake.reward, _stake.claimed);
    }

    function withdrawTokens(address _tokenAddress, uint256 _amount) public onlyOwner {
        IERC20 token = IERC20(_tokenAddress);
        require(token.transfer(owner(), _amount), "Token transfer failed");
    }

    function withdrawAvailableTokens() public onlyOwner {
        uint256 gfamBalance = gfamToken.balanceOf(address(this));
        uint256 busdBalance = busdToken.balanceOf(address(this));

        require(gfamBalance > 0, "No GFAM tokens to withdraw");
        require(busdBalance > 0, "No BUSD tokens to withdraw");

        gfamToken.transfer(owner(), gfamBalance);
        busdToken.transfer(owner(), busdBalance);
    }

    function pause() public onlyOwner {
        _pause();
    }

    function unpause() public onlyOwner {
        _unpause();
    }
}