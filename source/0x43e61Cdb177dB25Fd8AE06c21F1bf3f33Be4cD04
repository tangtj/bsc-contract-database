// File: @openzeppelin/contracts/utils/Context.sol
pragma solidity ^0.8.0;
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}



// File: @openzeppelin/contracts/access/Ownable.sol
pragma solidity ^0.8.0;
abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _setOwner(_msgSender());
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
        _setOwner(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}



// File: @openzeppelin/contracts/security/ReentrancyGuard.sol
pragma solidity ^0.8.0;
abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and make it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol
pragma solidity ^0.8.0;
interface IERC20 {
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
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

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



// File: @openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol
pragma solidity ^0.8.0;
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
pragma solidity ^0.8.0;
contract ERC20 is Context, IERC20, IERC20Metadata {
    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;

    /**
     * @dev Sets the values for {name} and {symbol}.
     *
     * The default value of {decimals} is 18. To select a different value for
     * {decimals} you should overload it.
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
     * Ether and Wei. This is the value {ERC20} uses, unless this function is
     * overridden;
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
     * - `recipient` cannot be the zero address.
     * - the caller must have a balance of at least `amount`.
     */
    function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
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
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    /**
     * @dev See {IERC20-transferFrom}.
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {ERC20}.
     *
     * Requirements:
     *
     * - `sender` and `recipient` cannot be the zero address.
     * - `sender` must have a balance of at least `amount`.
     * - the caller must have allowance for ``sender``'s tokens of at least
     * `amount`.
     */
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
        unchecked {
            _approve(sender, _msgSender(), currentAllowance - amount);
        }

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
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
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
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(_msgSender(), spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    /**
     * @dev Moves `amount` of tokens from `sender` to `recipient`.
     *
     * This internal function is equivalent to {transfer}, and can be used to
     * e.g. implement automatic token fees, slashing mechanisms, etc.
     *
     * Emits a {Transfer} event.
     *
     * Requirements:
     *
     * - `sender` cannot be the zero address.
     * - `recipient` cannot be the zero address.
     * - `sender` must have a balance of at least `amount`.
     */
    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(sender, recipient, amount);

        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[sender] = senderBalance - amount;
        }
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);

        _afterTokenTransfer(sender, recipient, amount);
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
        _balances[account] += amount;
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
        }
        _totalSupply -= amount;

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
    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
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
    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}

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
    function _afterTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
}


// File: contracts/GenieStakingPool.sol
// "SPDX-License-Identifier: UNLICENSED"
pragma solidity ^0.8.7;
contract GenieStakingPool is ReentrancyGuard, Ownable {


    /* ========== STATE VARIABLES ========== */

    //after creation, the pool is finalized in the "InCreation" status and then aytomatically changes in "Active", which remains until set to "Closed" after all users exited
    enum Status {InCreation, Active, Closed}
    
    IERC20 public stakingToken;
    uint256 public startStaking;
    uint256 public endStaking;
    uint256 public poolWeightedAverage;
    
    uint256 public rewardTokensAmount; 
    uint256 public stakedTokensTotal;
    uint256 public nrOfStakingUsers;
    bool paused;
    Status status;
    
    mapping(address => uint256) public tokensStakedPerUser;
    mapping(address => uint256) public userWeightedAverage;
    
    
    /* ========== EVENTS ========== */

    event RewardAdded(uint256 reward);
    event Staked(address indexed user, uint256 amount);
    event Withdrawn(address indexed user, uint256 amount);
    event RewardPaid(address indexed user, uint256 reward);
    
    
    /* ========== CONSTRUCTOR ========== */

    constructor(
        IERC20 _stakingToken
    ) {
        stakingToken = _stakingToken;
        status = Status.InCreation;
    }


    /* ========== FUNCTIONS ========== */
    
    function finalizePoolCreation (uint256 _startStaking, uint256 periodInDays, uint256 amountOfRewardsTokensToSend) public onlyOwner nonReentrant {
        require (status == Status.InCreation, "Error: Pool status is not InCreation");
        require (amountOfRewardsTokensToSend > 0, "Error: The creator must send some reward tokens to the pool in order to create it");
        require (stakingToken.transferFrom(msg.sender, address(this), amountOfRewardsTokensToSend), "Error: Reward tokens trasnfer error, cannot create pool");
        startStaking = _startStaking;
        
        endStaking = _startStaking + 1 days * periodInDays;
        
        rewardTokensAmount = amountOfRewardsTokensToSend;
        status = Status.Active;
        emit RewardAdded(amountOfRewardsTokensToSend);
    }

    function stake(uint256 amount) public nonReentrant checkPoolOpen checkStakingUnpaused{
        require(status == Status.Active, "Error: Pool status is not Active");
        require(amount > 0, "Error: Cannot stake 0");
        
        stakingToken.transferFrom(msg.sender, address(this), amount);
        
        if(tokensStakedPerUser[msg.sender] == 0){
           nrOfStakingUsers += 1;
        }
        
        tokensStakedPerUser[msg.sender] += amount;
        
        uint256 weightedAverage = calcWeightedAverage(amount);
        userWeightedAverage[msg.sender] += weightedAverage;
        
        poolWeightedAverage += weightedAverage;
        stakedTokensTotal += amount;
        
        emit Staked(msg.sender, amount);
    }

    function withdraw() internal checkStakingFinished checkStakingUnpaused{
        require(status == Status.Active, "Error: Pool status is not Active");
        uint256 toWithdraw = tokensStakedPerUser[msg.sender];
        tokensStakedPerUser[msg.sender] = 0;
        require(toWithdraw > 0, "Error: Cannot withdraw = 0");
        require(stakingToken.transfer(msg.sender, toWithdraw), "Error during the withdrawal of user invested sum");
        emit Withdrawn(msg.sender, toWithdraw);
    }

    function getReward() internal checkStakingFinished checkStakingUnpaused{
        require(status == Status.Active, "Error: Pool status is not Active");
        uint256 toWithdraw = calcReward(msg.sender);
        userWeightedAverage[msg.sender] = 0;
        require(toWithdraw > 0, "Error: Cannot get reward = 0");
        require(stakingToken.transfer(msg.sender, toWithdraw), "Error during the withdrawal of user reward");
        emit RewardPaid(msg.sender, toWithdraw);
    }

    function exit() public nonReentrant checkStakingFinished checkStakingUnpaused{
        withdraw();
        getReward();
    }

    function addRewardTokensAmount(uint256 amountToAdd) external onlyOwner nonReentrant {
        require(stakingToken.transferFrom(msg.sender, address(this), amountToAdd), "Error during the token reward increase transaction!");
        rewardTokensAmount += amountToAdd;
        emit RewardAdded(amountToAdd);
    }

    function setStatus(uint8 choice) public onlyOwner {
        if(choice == 0){
            status = Status.InCreation;
        }
        else if(choice == 1){
            status = Status.Active;
        }
        else if(choice == 2){
            status = Status.Closed;
        }
    }
    
    function setTime(uint256 initTimestamp, uint256 endTimestamp) public onlyOwner {
        if(endTimestamp > initTimestamp){
            startStaking = initTimestamp;
            endStaking = endTimestamp;
        }
    }
    
    function closePool() public onlyOwner nonReentrant{                        
        require (status == Status.Closed, "Error: Pool status is not Closed");
        require (stakingToken.transfer(msg.sender, stakingToken.balanceOf(address(this))), "Error during token transfer");
    }
    
    function pauseStaking () public onlyOwner {
        paused = true;
    }
    
    function unpauseStaking () public onlyOwner {
        paused = false;
    }
    
    
    /* ========== VIEWS ========== */

    function calcWeightedAverage(uint256 amount) public view returns (uint256 weightedAverage) {
        if(endStaking > startStaking){
            return amount * (endStaking - block.timestamp ) / (endStaking - startStaking );
        }
        return 0;
    }
    
    function calcReward(address user) public view returns (uint256 reward) {
        if(poolWeightedAverage>0){
            return userWeightedAverage[user] * rewardTokensAmount / poolWeightedAverage;
        }
        return 0;
    }
    
    function canIStakeNow() public view returns (bool poolReady) {                  
        if((status == Status.Active) && (block.timestamp >= startStaking) && (block.timestamp <= endStaking)){
            return true;
        }
        return false;
    }
    
    function canIExitPoolNow() public view returns (bool poolReady) {                  
        if((status == Status.Active) && (block.timestamp >= endStaking)){
            return true;
        }
        return false;
    }
    
    function getStartStaking() public view returns (uint256){
        return startStaking;
    }
    
    function getEndStaking() public view returns (uint256){
        return endStaking;
    }
    
    function getPoolWeightedAverage() public view returns (uint256){
        return poolWeightedAverage;
    }
    
    function getRewardTokensAmount() public view returns (uint256){
        return rewardTokensAmount;
    }
    
    function getStakedTokensTotal() public view returns (uint256){
        return stakedTokensTotal;
    }
    
    function getPoolStatus() public view returns (Status){
        return status;
    }
    
    function getTokensStakedPerUser(address user) public view returns (uint256){
        return tokensStakedPerUser[user];
    }
    
    function getUserWeightedAverage(address user) public view returns (uint256){
        return userWeightedAverage[user];
    }
    
    function getNrOfStakingUsers() public view returns (uint256){
        return nrOfStakingUsers;
    }
    

    /* ========== MODIFIERS ========== */
    
    modifier checkPoolOpen() {
        require(block.timestamp <= endStaking, "Error: Staking is finished");
        require(block.timestamp >= startStaking, "Error: Staking has not yet begun");
        _;
    }
    
    modifier checkStakingFinished() {
        require(block.timestamp >= endStaking, "Error: Staking period is not finished");
        _;
    }
    
    modifier checkStakingUnpaused() {
        require(paused == false, "Error: Swap is paused, try again later");
        _;
    }
    
}