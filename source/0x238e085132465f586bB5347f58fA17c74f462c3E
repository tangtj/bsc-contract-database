// SPDX-License-Identifier: MIT
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

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

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
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

// File: @openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol


// OpenZeppelin Contracts v4.4.1 (token/ERC20/extensions/IERC20Metadata.sol)

pragma solidity ^0.8.0;


/**
 * @dev Interface for the optional metadata functions from the ERC20 standard.
 *
 * _Available since v4.1._
 */
interface IERC20Metadata is IERC20 {
    /**
     * @dev Returns the name of the token.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the symbol of the token.
     */
    function symbol() external view returns (string memory);

    /**
     * @dev Returns the decimals places of the token.
     */
    function decimals() external view returns (uint8);
}

// File: @openzeppelin/contracts/token/ERC20/ERC20.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/ERC20.sol)

pragma solidity ^0.8.0;




/**
 * @dev Implementation of the {IERC20} interface.
 *
 * This implementation is agnostic to the way tokens are created. This means
 * that a supply mechanism has to be added in a derived contract using {_mint}.
 * For a generic mechanism see {ERC20PresetMinterPauser}.
 *
 * TIP: For a detailed writeup see our guide
 * https://forum.openzeppelin.com/t/how-to-implement-erc20-supply-mechanisms/226[How
 * to implement supply mechanisms].
 *
 * The default value of {decimals} is 18. To change this, you should override
 * this function so it returns a different value.
 *
 * We have followed general OpenZeppelin Contracts guidelines: functions revert
 * instead returning `false` on failure. This behavior is nonetheless
 * conventional and does not conflict with the expectations of ERC20
 * applications.
 *
 * Additionally, an {Approval} event is emitted on calls to {transferFrom}.
 * This allows applications to reconstruct the allowance for all accounts just
 * by listening to said events. Other implementations of the EIP may not emit
 * these events, as it isn't required by the specification.
 *
 * Finally, the non-standard {decreaseAllowance} and {increaseAllowance}
 * functions have been added to mitigate the well-known issues around setting
 * allowances. See {IERC20-approve}.
 */
contract ERC20 is Context, IERC20, IERC20Metadata {
    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;

    /**
     * @dev Sets the values for {name} and {symbol}.
     *
     * All two of these values are immutable: they can only be set once during
     * construction.
     */
    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    /**
     * @dev Returns the name of the token.
     */
    function name() public view virtual override returns (string memory) {
        return _name;
    }

    /**
     * @dev Returns the symbol of the token, usually a shorter version of the
     * name.
     */
    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    /**
     * @dev Returns the number of decimals used to get its user representation.
     * For example, if `decimals` equals `2`, a balance of `505` tokens should
     * be displayed to a user as `5.05` (`505 / 10 ** 2`).
     *
     * Tokens usually opt for a value of 18, imitating the relationship between
     * Ether and Wei. This is the default value returned by this function, unless
     * it's overridden.
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * no way affects any of the arithmetic of the contract, including
     * {IERC20-balanceOf} and {IERC20-transfer}.
     */
    function decimals() public view virtual override returns (uint8) {
        return 18;
    }

    /**
     * @dev See {IERC20-totalSupply}.
     */
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    /**
     * @dev See {IERC20-balanceOf}.
     */
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }

    /**
     * @dev See {IERC20-transfer}.
     *
     * Requirements:
     *
     * - `to` cannot be the zero address.
     * - the caller must have a balance of at least `amount`.
     */
    function transfer(address to, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }

    /**
     * @dev See {IERC20-allowance}.
     */
    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    /**
     * @dev See {IERC20-approve}.
     *
     * NOTE: If `amount` is the maximum `uint256`, the allowance is not updated on
     * `transferFrom`. This is semantically equivalent to an infinite approval.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, amount);
        return true;
    }

    /**
     * @dev See {IERC20-transferFrom}.
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {ERC20}.
     *
     * NOTE: Does not update the allowance if the current allowance
     * is the maximum `uint256`.
     *
     * Requirements:
     *
     * - `from` and `to` cannot be the zero address.
     * - `from` must have a balance of at least `amount`.
     * - the caller must have allowance for ``from``'s tokens of at least
     * `amount`.
     */
    function transferFrom(address from, address to, uint256 amount) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    /**
     * @dev Atomically increases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IERC20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    /**
     * @dev Atomically decreases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IERC20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     * - `spender` must have allowance for the caller of at least
     * `subtractedValue`.
     */
    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    /**
     * @dev Moves `amount` of tokens from `from` to `to`.
     *
     * This internal function is equivalent to {transfer}, and can be used to
     * e.g. implement automatic token fees, slashing mechanisms, etc.
     *
     * Emits a {Transfer} event.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `from` must have a balance of at least `amount`.
     */
    function _transfer(address from, address to, uint256 amount) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(from, to, amount);

        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[from] = fromBalance - amount;
            // Overflow not possible: the sum of all balances is capped by totalSupply, and the sum is preserved by
            // decrementing then incrementing.
            _balances[to] += amount;
        }

        emit Transfer(from, to, amount);

        _afterTokenTransfer(from, to, amount);
    }

    /** @dev Creates `amount` tokens and assigns them to `account`, increasing
     * the total supply.
     *
     * Emits a {Transfer} event with `from` set to the zero address.
     *
     * Requirements:
     *
     * - `account` cannot be the zero address.
     */
    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        unchecked {
            // Overflow not possible: balance + amount is at most totalSupply + amount, which is checked above.
            _balances[account] += amount;
        }
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(address(0), account, amount);
    }

    /**
     * @dev Destroys `amount` tokens from `account`, reducing the
     * total supply.
     *
     * Emits a {Transfer} event with `to` set to the zero address.
     *
     * Requirements:
     *
     * - `account` cannot be the zero address.
     * - `account` must have at least `amount` tokens.
     */
    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
            // Overflow not possible: amount <= accountBalance <= totalSupply.
            _totalSupply -= amount;
        }

        emit Transfer(account, address(0), amount);

        _afterTokenTransfer(account, address(0), amount);
    }

    /**
     * @dev Sets `amount` as the allowance of `spender` over the `owner` s tokens.
     *
     * This internal function is equivalent to `approve`, and can be used to
     * e.g. set automatic allowances for certain subsystems, etc.
     *
     * Emits an {Approval} event.
     *
     * Requirements:
     *
     * - `owner` cannot be the zero address.
     * - `spender` cannot be the zero address.
     */
    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    /**
     * @dev Updates `owner` s allowance for `spender` based on spent `amount`.
     *
     * Does not update the allowance amount in case of infinite allowance.
     * Revert if not enough allowance is available.
     *
     * Might emit an {Approval} event.
     */
    function _spendAllowance(address owner, address spender, uint256 amount) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }

    /**
     * @dev Hook that is called before any transfer of tokens. This includes
     * minting and burning.
     *
     * Calling conditions:
     *
     * - when `from` and `to` are both non-zero, `amount` of ``from``'s tokens
     * will be transferred to `to`.
     * - when `from` is zero, `amount` tokens will be minted for `to`.
     * - when `to` is zero, `amount` of ``from``'s tokens will be burned.
     * - `from` and `to` are never both zero.
     *
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     */
    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual {}

    /**
     * @dev Hook that is called after any transfer of tokens. This includes
     * minting and burning.
     *
     * Calling conditions:
     *
     * - when `from` and `to` are both non-zero, `amount` of ``from``'s tokens
     * has been transferred to `to`.
     * - when `from` is zero, `amount` tokens have been minted for `to`.
     * - when `to` is zero, `amount` of ``from``'s tokens have been burned.
     * - `from` and `to` are never both zero.
     *
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     */
    function _afterTokenTransfer(address from, address to, uint256 amount) internal virtual {}
}

// File: stakeNew.sol


pragma solidity ^0.8.0;


/**
 * @title StakingContract
 * @dev This contract represents a staking system with different plans.
 */
contract Staking {
    address immutable owner; // The address of the contract owner

    ERC20 immutable token; // Address of the ERC20 token being staked

    // Constants defining referral limits and tier values
    uint256 public constant maxRefferalLimit = 10;
    uint256 public constant ZeroUSD = 0;
    uint256 public constant FiftyUSD = 50;
    uint256 public constant HundreadUSD = 100;
    uint256 public constant TwoHundreadUSD = 200;
    uint256 public constant FiveHundreadUSD = 500;
    uint256 public constant ThousandUSD = 1000;
    address public fees_address;

    uint256 public totalStaked;
    uint256[] RewardPercentage = [50, 20, 10, 5, 5, 4, 3, 2, 1];

    // Struct to store user staking information
    struct User {
        uint256 stakedAmount; // Amount of tokens staked
        uint256 stakingEndTime; // Time when staking ends
        uint256 StartDate;
        uint256 teamSize; // Size of the staking team
    }

    // Struct to store subscription details
    struct Subscription {
        uint256 tokenAmount;
        address parent;
        uint256 tier;
    }

    // Struct to store Stake_subscription details
    struct StakeSubscription {
        uint256 tokenAmount;
        address parent;
    }

    //Struct to Store Rewards
    struct Rewards {
        uint256 totalrewards;
    }
    struct User_children {
        address[] child;
    }

    // Mapping to store user data using their address
    mapping(address => User[]) public userStaking;

    mapping(address => mapping(uint256 => User)) public users;

    mapping(address => uint256) public userCount; // Count of stakes per user

    // Mapping to store user subscription data using their address
    mapping(address => Subscription) public userSubscription;

    // Mapping to store user subscription data using their address
    mapping(address => StakeSubscription) public stakeSubscription;

    mapping(address => Rewards) public userRewards;

    // Mapping to track whether an address has been referred by the owner
    mapping(address => bool) public ownerReferred;

    // Mapping to store children for each referrer
    mapping(address => User_children) private referrerToChildren;

    // Mapping to track the number of referrals per tier for each referrer
    mapping(address => uint256) public maxTierReferralCounts;

    mapping(address => uint256) public rewardAmount;

    // Event to log staking action
    event TokensStaked(
        address indexed user,
        uint256 amount,
        uint256 stakingEndTime,
        uint256 id
    );

    // Event to log unstaking action
    event TokensUnstaked(address indexed user, uint256 amount);

    // Event to log referred from referrer
    event UserReferred(address indexed user, address indexed referrer);

    // Event to log buy token with tier
    event TokenBought(address indexed buyer, uint256 amount, uint256 tier);

    event DirectEntry(address indexed buyer, uint256 amount);

    /**
     * @dev Modifier to restrict function access to the contract owner only.
     */
    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can perform this action");
        _;
    }

    // Constructor to initialize the contract with the ERC20 token address
    constructor(address _tokenAddress) {
        token = ERC20(_tokenAddress);
        owner = msg.sender;
        fees_address = 0x7597C6c5e04E159f669B85aFDf67979Da5047ef1;
    }

    /**
     *@dev Parent Address for a given User
     */
    function getParent(address user) public view returns (address) {
        Subscription memory parent = userSubscription[user];

        return parent.parent;
    }

    /**
     * @dev Stake tokens for a given plan, staking duration, and team size.
     * @param tokenAmount_ amount of tokens to stake.
     * @param stakingDuration_ The duration of staking in days.
     * @param teamSize_ The size of the staking team.
     */
    function stakeTokens(
        uint256 tokenAmount_,
        uint256 stakingDuration_,
        uint256 teamSize_,
        uint256 id
    ) public {
        uint256 requiredAmount = tokenAmount_ * 1e8;
        // Calculate the required staking amount based on the chosen plan

        require(
            stakingDuration_ == 90 ||
                stakingDuration_ == 180 ||
                stakingDuration_ == 365,
            "Invalid staking duration"
        );

        // Calculate the end time of staking
        uint256 stakingEndTime = block.timestamp + stakingDuration_ * 1 days;
        uint256 StartDate = block.timestamp;

        // Store user staking data
        users[msg.sender][id] = User({
            stakedAmount: requiredAmount,
            stakingEndTime: stakingEndTime,
            StartDate: StartDate,
            teamSize: teamSize_ // Corrected field name to match the struct
        });
        userCount[msg.sender]++;

        // Update the total staked amount
        totalStaked += requiredAmount * 1e8;

        // userStaking[msg.sender].push(staking);

        // Transfer tokens to this contract
        token.transferFrom(msg.sender, address(this), requiredAmount);

        // Emit staking event
        emit TokensStaked(msg.sender, requiredAmount, stakingEndTime, id);
    }

    /**
     * @dev Unstake tokens and return them to the user.
     */
    function unstakeTokens(uint256 stakeId_) public {
        // Get the user's staking data
        User storage user = userStaking[msg.sender][stakeId_];
        uint256 stakedAmount = user.stakedAmount;

        // Ensure the user has staked tokens
        require(stakedAmount > 0, "No staked tokens");
        require(
            block.timestamp >= user.stakingEndTime,
            "Staking period not ended yet"
        );

        // Reset user data
        delete userStaking[msg.sender];

        // Transfer tokens back to the user
        token.transfer(msg.sender, stakedAmount);

        // Emit unstaking event
        emit TokensUnstaked(msg.sender, stakedAmount);
    }

    function TotalTokenStaked(
        address userAddress
    ) public view returns (uint256) {
        uint256 totalStakedByUser = 0;

        for (uint256 id = 101; id <= 100 + userCount[userAddress]; id++) {
            User memory user = users[userAddress][id];
            totalStakedByUser += user.stakedAmount * 1e8;
        }

        return totalStakedByUser;
    }

    /**
     * @dev Check if a user is referred by a given referrer.
     * @param _referrer The address of the referrer.
     * @return Whether the user is referred and the tier of the referrer.
     */
    function isReferred(address _referrer) public view returns (uint256) {
        if (_referrer == owner) {
            return (ThousandUSD);
        }

        Subscription memory referrerSubscription = userSubscription[_referrer];

        if (referrerSubscription.tokenAmount == 0) {
            return (ZeroUSD);
        }

        return (referrerSubscription.tier);
    }

    /**
     * @dev Direct stake tokens and associate them with a referrer.
     * @param _referreladdress The address of the referrel.
     * @param _tokenAmount The amount of token.

     */
    function DirectStakeJoining(
        address _referreladdress,
        uint256 _tokenAmount
    ) external {
        StakeSubscription memory subscription = stakeSubscription[msg.sender];
        require(
            subscription.tokenAmount == 0,
            "User already has a subscription"
        );
        uint256 amount = _tokenAmount * 1e8;
        subscription.tokenAmount = amount;
        subscription.parent = _referreladdress;

        // uint256 self_stakeamount = amount;
        // uint256 remaining_tokens = amount - self_stakeamount;

        //stakeTokens(self_stakeamount, 180 , 1);

        // Assuming you have a "token" contract with a "transfer" function
        token.approve(address(this), amount);
        token.transferFrom(msg.sender, address(this), amount);
        address new_referrel;
        new_referrel = _referreladdress;

        for (uint256 i = 0; i < 9; i++) {
            if (new_referrel == owner) {
                break;
            }

            address parent_addr = getParent(new_referrel);
            uint256 reward_amount = (RewardPercentage[i] * amount) / 100;
            userRewards[parent_addr] = Rewards({totalrewards: reward_amount});
            token.transferFrom(address(this), parent_addr, reward_amount);

            new_referrel = parent_addr;
        }

        emit DirectEntry(msg.sender, _tokenAmount);
    }

    /**
     * @dev Buy tokens and associate them with a referrer.
     * @param _referrer The address of the referrer.
     * @param _tokenAmount The amount of token.
     * @param _tier The chosen referral tier.
     */
    function buyTokens(
        address _referrer,
        uint256 _tokenAmount,
        uint256 _tier,
        uint256 _fees
    ) external {
        require(
            _tier == ZeroUSD ||
                _tier == FiftyUSD ||
                _tier == HundreadUSD ||
                _tier == TwoHundreadUSD ||
                _tier == FiveHundreadUSD ||
                _tier == ThousandUSD,
            "Invalid tier value"
        );
        uint256 amount = _tokenAmount * 1e8;
        uint256 fee = _fees * 1e8;

        // Check for zero address
        //require(_referrer != address(0), "Invalid referrer address");

        if (_referrer == owner) {
            Subscription memory subscription = Subscription(
                amount,
                _referrer,
                _tier
            );

            userSubscription[msg.sender] = subscription;
        }

        uint256 userTier = isReferred(_referrer);

        if (_tier == userTier) {
            require(
                maxTierReferralCounts[_referrer] <= maxRefferalLimit,
                "Already referred to maximum users."
            );
            maxTierReferralCounts[_referrer]++;
            Subscription memory subscription = Subscription(
                amount,
                _referrer,
                _tier
            );
            userSubscription[msg.sender] = subscription;
            referrerToChildren[_referrer].child.push(msg.sender);
        } else {
            // Add the child to the array of children for the referrer
            Subscription memory subscription = Subscription(
                amount,
                _referrer,
                _tier
            );
            userSubscription[msg.sender] = subscription;
        }

        // Calculate fees
        //uint256 self_stakeamount = amount * 15 / 100;
        //uint256 remaing_tokens = amount - self_stakeamount;
        token.approve(address(this), amount);

        // Check if the contract has enough tokens to transfer
        require(
            token.balanceOf(address(this)) >= amount,
            "Not enough tokens in the contract"
        );

        // Check allowance
        require(
            token.allowance(msg.sender, address(this)) >= amount,
            "Not enough allowance"
        );

        // Perform the transfer
        require(
            token.transferFrom(msg.sender, address(this), amount),
            "Token transfer failed"
        );

        // Transfer fees to fees_address
        require(token.transfer(fees_address, fee), "Fee transfer failed");

        address new_referrel;
        new_referrel = _referrer;
        for (uint256 i = 0; i < 9; i++) {
            if (new_referrel == owner) {
                break;
            }
            address parent_addr = getParent(new_referrel);
            uint256 reward_amount = (RewardPercentage[i] * amount) / 100;
            userRewards[parent_addr] = Rewards({totalrewards: reward_amount});
            // Check if the contract has enough tokens to transfer
            require(
                token.balanceOf(address(this)) >= reward_amount,
                "Not enough tokens in the contract"
            );

            // Perform the transfer to the parent
            require(
                token.transfer(parent_addr, reward_amount),
                "Reward transfer failed"
            );

            new_referrel = parent_addr;
        }

        emit TokenBought(msg.sender, amount, _tier);
    }

    function showAllParent(
        address user
    ) external view returns (address[] memory) {
        address[] memory parent = new address[](9); // Initialize an array with a fixed size of 9
        address new_referrel = user;

        for (uint256 i = 0; i < 9; i++) {
            address parent_addr = getParent(new_referrel);
            parent[i] = parent_addr;

            if (new_referrel == owner) {
                break;
            } else {
                new_referrel = parent_addr;
            }
        }

        return parent;
    }

    function showAllChild(
        address user
    ) external view returns (address[] memory) {
        address[] memory children = referrerToChildren[user].child;

        return children;
    }

    /**
     * @dev Calculate the total rewards received by a user.
     * @param userAddress The address of the user.
     * @return The total rewards received by the user.
     */
    function totalRewardsReceived(
        address userAddress
    ) public view returns (uint256) {
        uint256 totalRewards = 0;

        // Calculate rewards received during DirectStakeJoining
        StakeSubscription memory directStake = stakeSubscription[userAddress];
        totalRewards += userRewards[directStake.parent].totalrewards;

        // Calculate rewards received during buyTokens
        Subscription memory referral = userSubscription[userAddress];
        totalRewards += userRewards[referral.parent].totalrewards;

        return totalRewards;
    }
}