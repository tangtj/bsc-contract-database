/**
 *Submitted for verification at BscScan.com on 2022-06-17
*/

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
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
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

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
    function allowance(address owner, address spender)
    external
    view
    returns (uint256);

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
    function balanceOf(address account) public view virtual override returns (uint256)    {
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
    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool)    {
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
     * @dev Moves `amount` of tokens from `sender` to `recipient`.
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
        }
        _balances[to] += amount;

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
            require(
                currentAllowance >= amount,
                "ERC20: insufficient allowance"
            );
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

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

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

abstract contract TokenVesting is ERC20, Ownable {
    struct Vesting {
        bool exists;
        uint256 version;
        uint256 cliff;
        uint256 releaseCount;
        uint256 released;
        uint256 totalLockAmount;
        uint256 firstRatio;
        uint256[] customizeRatio;
    }

    // vesting data
    mapping(address => Vesting) private _vestingOf;
    mapping(uint256 => uint256) private _startTimeOf;

    modifier availableBalance(address _account, uint256 _amount) {
        if (_vestingOf[_account].exists) {
            releaseDue(_account);
        }
        require(_amount <= balanceOf(_account) - unReleaseAmount(_account), "Insufficient available balance");
        _;
    }

    modifier haveNoVesting(address _beneficiary) {
        require(
            _vestingOf[_beneficiary].totalLockAmount == 0 &&
            _vestingOf[_beneficiary].cliff == 0 &&
            _vestingOf[_beneficiary].releaseCount == 0,
            "Vesting already exists"
        );
        _;
    }

    modifier startTimingCheck(address _beneficiary) {
        require(_vestingOf[_beneficiary].totalLockAmount > 0, "Vesting not exists");
        require(_getStartTimestamp(_beneficiary) > 0, "Vesting timing no start");
        _;
    }

    event TokensReleased(address beneficiary, uint256 amount);
    event CreateVesting(
        address beneficiary,
        uint256 version,
        uint256 cliff,
        uint256 releaseCount,
        uint256 totalLockAmount,
        uint256 firstRatio,
        uint256[] customizeRatio
    );
    event SetVersionTime(uint256 version, uint256 timestamp);

    /**
     * @dev batch create vesting
     */
    function createBatchVesting(
        address[] memory _beneficiary,
        uint256[] memory _totalLockAmount,
        uint256[] memory _version,
        uint256[] memory _firstRatio,
        uint256[] memory _cliff,
        uint256[] memory _releaseCount,
        uint256[][] memory _customizeRatio
    ) external onlyOwner {
        for (uint256 i = 0; i < _beneficiary.length; i++) {
            createVesting(
                _beneficiary[i],
                _totalLockAmount[i],
                _version[i],
                _firstRatio[i],
                _cliff[i],
                _releaseCount[i],
                _customizeRatio[i]
            );
        }
    }

    /**
     * @dev batch set version time
     */
    function setBatchVersionTime(uint256[] memory _version, uint256[] memory _timestamp) external onlyOwner {
        for (uint256 i = 0; i < _version.length; i++) {
            setVersionTime(_version[i], _timestamp[i]);
        }
    }

    /**
     * @dev Create Vesting
     *
     * @param _beneficiary beneficiary address
     * @param _totalLockAmount totalLock amount
     * @param _firstRatio first ratio (100.00%)
     * @param _cliff cliff
     * @param _releaseCount release count
     * @param _customizeRatio Customize every unlock rate (100.00%)
     *
     */
    function createVesting(
        address _beneficiary,
        uint256 _totalLockAmount,
        uint256 _version,
        uint256 _firstRatio,
        uint256 _cliff,
        uint256 _releaseCount,
        uint256[] memory _customizeRatio
    ) public onlyOwner haveNoVesting(_beneficiary) {
        require(_beneficiary != address(0), "Beneficiary is the zero address");
        require(_totalLockAmount > 0, "Amount count is 0");
        require(_cliff > 0, "Cliff is 0");
        require(_releaseCount > 0, "Release count is 0");
        require(_firstRatio <= 10000, "No more than one hundred");
        if (_customizeRatio.length > 0) {
            require(_releaseCount == _customizeRatio.length, "The number of unlocks must correspond to the customize ratio array");
            uint256 __total;
            for (uint256 i = 0; i < _customizeRatio.length; i++) {
                __total += _customizeRatio[i];
            }
            require(__total + _firstRatio == 10000, "The Ratio total is not 100.00");
        }
        _vestingOf[_beneficiary] = Vesting(
            true,
            _version,
            _cliff,
            _releaseCount,
            0,
            _totalLockAmount,
            _firstRatio,
            _customizeRatio
        );
        transfer(_beneficiary, _totalLockAmount);
        emit CreateVesting(
            _beneficiary,
            _version,
            _cliff,
            _releaseCount,
            _totalLockAmount,
            _firstRatio,
            _customizeRatio
        );
    }

    /**
     * @dev start vesting timing
     * @param _version version
     * @param _timestamp timestamp
     */
    function setVersionTime(uint256 _version, uint256 _timestamp) public onlyOwner {
        require(_timestamp >= block.timestamp, "Timestamp cannot be less than current time");
        require(_startTimeOf[_version] == 0, "Time has begun");
        _startTimeOf[_version] = _timestamp;
        emit SetVersionTime(_version, _timestamp);
    }

    /**
     * @dev Unlock this amount for the beneficiary
     * @param _beneficiary beneficiary address
     */
    function releaseDue(address _beneficiary) internal startTimingCheck(_beneficiary) {
        if (unReleaseAmount(_beneficiary) > 0) {
            uint256 _nowReleased = nowReleaseAllAmount(_beneficiary);
            if (_nowReleased > 0) {
                Vesting storage vestingOf_ = _vestingOf[_beneficiary];
                vestingOf_.released += _nowReleased;
                emit TokensReleased(_beneficiary, _nowReleased);
            }
        }
    }

    function release(address _beneficiary) external startTimingCheck(_beneficiary) {
        require(unReleaseAmount(_beneficiary) > 0, "Vesting is done");
        uint256 _nowReleased = nowReleaseAllAmount(_beneficiary);
        require(_nowReleased > 0, "No tokens are due");
        Vesting storage vestingOf_ = _vestingOf[_beneficiary];
        vestingOf_.released += _nowReleased;
        emit TokensReleased(_beneficiary, _nowReleased);
    }

    /**
     * @dev Get the unlocked amount
     * @param _beneficiary beneficiary address
     * @return uint256 token.balance
     */
    function unReleaseAmount(address _beneficiary) public view returns (uint256) {
        return _vestingOf[_beneficiary].totalLockAmount - _vestingOf[_beneficiary].released;
    }

    /**
     * @dev Get all amounts currently unLockable
     * @param _beneficiary beneficiary address
     * @return uint256 token.balance
     */
    function nowReleaseAllAmount(address _beneficiary) public view startTimingCheck(_beneficiary) returns (uint256) {
        uint256 _nowAmount = _firstAmount(_beneficiary);
        if (block.timestamp < (_getStartTimestamp(_beneficiary) + _vestingOf[_beneficiary].cliff)) {
            return _nowAmount - _vestingOf[_beneficiary].released;
        } else if (block.timestamp >= endReleaseTime(_beneficiary)) {
            return unReleaseAmount(_beneficiary);
        } else {
            if (_vestingOf[_beneficiary].customizeRatio.length > 0) {
                for (uint256 i = 0; i < _nowReleaseCount(_beneficiary); i++) {
                    _nowAmount += _vestedCustomizeRatioAmount(_beneficiary, i);
                }
            } else {
                _nowAmount += _singleReleaseAmount(_beneficiary) * _nowReleaseCount(_beneficiary);
            }
            return _nowAmount - _vestingOf[_beneficiary].released;
        }
    }

    /**
     * @dev Get the next unlock time
     * @param _beneficiary beneficiary address
     * @return uint256 block.timestamp
     */
    function nextReleaseTime(address _beneficiary) public view startTimingCheck(_beneficiary) returns (uint256) {
        uint256 _firstGetTime = _getStartTimestamp(_beneficiary) + _vestingOf[_beneficiary].cliff;
        uint256 _nextTime = ((_nowReleaseCount(_beneficiary)) * _vestingOf[_beneficiary].cliff) + _firstGetTime;
        if (_nextTime >= endReleaseTime(_beneficiary) || block.timestamp >= endReleaseTime(_beneficiary)) {
            return endReleaseTime(_beneficiary);
        } else {
            return _nextTime;
        }
    }

    /**
     * @dev Get the lock-up end time
     * @param _beneficiary beneficiary address
     * @return uint256 block.timestamp
     */
    function endReleaseTime(address _beneficiary) public view startTimingCheck(_beneficiary) returns (uint256) {
        return _getStartTimestamp(_beneficiary) + (_vestingOf[_beneficiary].cliff * _vestingOf[_beneficiary].releaseCount);
    }

    /**
     * @dev Get the available amount of the current account
     * @param _account beneficiary address
     * @return uint256 token.balance
     */
    function availableBalanceOf(address _account) public view returns (uint256) {
        return balanceOf(_account) - unReleaseAmount(_account);
    }

    /**
     * @dev Get Custom Unlock Ratio Amount that should be unlocked
     * @param _beneficiary beneficiary address
     * @return uint256 token.balance
     */
    function _vestedCustomizeRatioAmount(address _beneficiary, uint256 _count) private view returns (uint256) {
        return (_vestingOf[_beneficiary].totalLockAmount * _vestingOf[_beneficiary].customizeRatio[_count]) / 10000;
    }

    /**
     * @dev Get a single unlock amount
     * @param _beneficiary beneficiary address
     * @return uint256 token.balance
     */
    function _singleReleaseAmount(address _beneficiary) private view returns (uint256) {
        return (_vestingOf[_beneficiary].totalLockAmount - _firstAmount(_beneficiary)) / _vestingOf[_beneficiary].releaseCount;
    }

    /**
     * @dev Get the first unlock amount
     * @param _beneficiary beneficiary address
     * @return uint256 token.balance
     */
    function _firstAmount(address _beneficiary) private view returns (uint256) {
        return (_vestingOf[_beneficiary].totalLockAmount * _vestingOf[_beneficiary].firstRatio) / 10000;
    }

    /**
     * @dev Get the current unlock stage
     * @param _beneficiary beneficiary address
     * @return uint256 number
     */
    function _nowReleaseCount(address _beneficiary) private view returns (uint256) {
        return (block.timestamp - _getStartTimestamp(_beneficiary)) / _vestingOf[_beneficiary].cliff;
    }

    /**
     * @dev Get the start timestamp
     * @param _beneficiary beneficiary address
     * @return uint256 number
     */
    function _getStartTimestamp(address _beneficiary) private view returns (uint256) {
        return _startTimeOf[_vestingOf[_beneficiary].version];
    }
}

contract AGGToken is TokenVesting {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 public transferFee = 0;
    uint256 public buyFee = 300;
    uint256 public sellFee = 300;
    uint256 public burnFee = 0;
    uint256 public liquidityFee = 0;

    address public marketingAddress;

    mapping(address => bool) public pairAddress;
    mapping(address => bool) private _isExcludedFromFee;
    mapping(address => bool) private _isExcludedFromTransferFee;

    constructor() ERC20("Aggregator", "AGG") {
        uint256 initMintAmount = 10 ** 6;
        _mint(msg.sender, initMintAmount * 10 ** decimals());

        marketingAddress = _msgSender();
        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[marketingAddress] = true;
        _isExcludedFromTransferFee[owner()] = true;
        //bsc router
        _isExcludedFromFee[address(0x10ED43C718714eb63d5aA57B78B54704E256024E)] = true;
        _isExcludedFromTransferFee[address(0x10ED43C718714eb63d5aA57B78B54704E256024E)] = true;

    }

    function transfer(address to, uint256 amount) public virtual override availableBalance(_msgSender(), amount) returns (bool) {
        address owner = _msgSender();
        amount = estimateFee(owner, to, amount);
        _transfer(owner, to, amount);
        return true;
    }

    function transferFrom(address from, address to, uint256 amount) public virtual override availableBalance(from, amount) returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        amount = estimateFee(from, to, amount);
        _transfer(from, to, amount);
        return true;
    }

    function estimateFee(address sender, address recipient, uint256 amount) internal returns (uint256){
        // not trading
        if (!pairAddress[sender] && !pairAddress[recipient]) {
            return shouldTakeTransferFee(sender, recipient) ? takeTransferFee(sender, amount) : amount;
        }
        bool takeFee = true;
        uint256 fee = 0;
        if (_isExcludedFromFee[sender] || _isExcludedFromFee[recipient]) {
            takeFee = false;
        }
        if (takeFee) {
            //buy
            if (pairAddress[sender]) {
                fee = buyFee;
            }
            // Sell
            else if (pairAddress[recipient]) {
                fee = sellFee;
            }
            //take market fee
            uint256 feeAmount = amount * fee / 10000;
            _transfer(sender, marketingAddress, feeAmount);
            amount = amount - feeAmount;
        }
        return amount;
    }

    function burn(uint256 amount) public virtual availableBalance(_msgSender(), amount) onlyOwner {
        _burn(_msgSender(), amount);
    }

    function burnFrom(address account, uint256 amount) public virtual availableBalance(account, amount) onlyOwner {
        _spendAllowance(account, _msgSender(), amount);
        _burn(account, amount);
    }

    //trade fee
    function excludeMultipleAccountsFromFees(address[] calldata accounts, bool excluded) public onlyOwner {
        for (uint256 i = 0; i < accounts.length; i++) {
            _isExcludedFromFee[accounts[i]] = excluded;
        }
    }

    function excludeFromFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = true;
    }

    function includeInFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = false;
    }

    function isExcludedFromFee(address account) public view returns (bool) {
        return _isExcludedFromFee[account];
    }

    //transfer fee
    function excludeMultipleAccountsFromTransferFees(address[] calldata accounts, bool excluded) public onlyOwner {
        for (uint256 i = 0; i < accounts.length; i++) {
            _isExcludedFromTransferFee[accounts[i]] = excluded;
        }
    }

    function excludeFromTransferFee(address account) public onlyOwner {
        _isExcludedFromTransferFee[account] = true;
    }

    function includeInTransferFee(address account) public onlyOwner {
        _isExcludedFromTransferFee[account] = false;
    }

    function isExcludedFromTransferFee(address account) public view returns (bool) {
        return _isExcludedFromTransferFee[account];
    }

    function setMarketingAddress(address _mAddress) public onlyOwner {
        marketingAddress = _mAddress;
    }

    function setPairAddress(address _Address, bool _value) public onlyOwner {
        pairAddress[_Address] = _value;
    }

    function setFees(uint256 _transferFee, uint256 _buyFee, uint256 _sellFee) public onlyOwner {
        transferFee = _transferFee;
        buyFee = _buyFee;
        sellFee = _sellFee;
    }

    function shouldTakeTransferFee(address sender, address recipient) internal view returns (bool) {
        return !_isExcludedFromTransferFee[sender] && !_isExcludedFromTransferFee[recipient];
    }

    function takeTransferFee(address sender, uint256 amount) internal returns (uint256) {
        uint256 feeAmount = amount * transferFee / 10000;
        if (feeAmount > 0) {
            _transfer(sender, marketingAddress, feeAmount);
        }
        return amount - feeAmount;
    }
}