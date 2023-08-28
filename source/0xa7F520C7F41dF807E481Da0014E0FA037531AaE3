// SPDX-License-Identifier: MIT
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

// Interface for Uniswap V2 Factory which allows creating liquidity pairs
interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

// Interface for Uniswap V2 Pair to get details about a liquidity pair's reserves
interface IUniswapV2Pair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
}

interface INFTDistribution{
    function distribute() external;
}

// The main ZQToken contract that inherits from ERC20 and Ownable
contract ZQToken is ERC20, Ownable {
    // Variable to store the main pair address
    address public mainPair;
    // Setting up some constant addresses
    address private constant QL_ADDRESS = 0x5930d09D7a54969E0F0a749AC81FfA3807dC67ab;
    address private constant FOUNDATION_ADDRESS = 0xEd429638666bcF2C898a27C3f40FFd7Afde2008B;
    address private NFT_REWARD_ADDRESS = 0x5bc5C719a4C8513Fa22710898675958d13B4d55a;
    // Variable to store the LP reward address
    address public lpRewardAddress;
    // Variable to store the starting block number
    uint256 public startBlock;
    // Mappings for whitelist and monitor list
    mapping(address => bool) private whiteList;
    mapping(address => bool) private monitorList;
    // Constant for a dead address (usually represents burned tokens)
    address private constant DEAD_ADDRESS = 0x000000000000000000000000000000000000dEaD;

    uint256 public processGas = 50000;

    // Constructor function that initializes the contract
    constructor() ERC20("ZQToken", "ZQ") {
        require(QL_ADDRESS < address(this),"QL must be token0");
        // Minting initial tokens
        _mint(msg.sender, 3000 * (10 ** uint256(decimals())));
        // Creating a uniswap pair
        IUniswapV2Factory factory = IUniswapV2Factory(0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73);
        mainPair = factory.createPair(address(this), QL_ADDRESS);
        // Initializing the LPRewardProcessor contract and setting its address
        LPRewardProcessor lpRewardContract = new LPRewardProcessor(address(this), mainPair);
        lpRewardAddress = address(lpRewardContract);
        // Setting initial addresses for whitelist
        whiteList[FOUNDATION_ADDRESS] = true;
        whiteList[NFT_REWARD_ADDRESS] = true;
        whiteList[lpRewardAddress] = true;
        whiteList[msg.sender] = true;
    }

    event DistributionFailed(bytes reason);

    // Function to mark the start of trading. Can only be called by the owner
    function startTrade() external onlyOwner {
        require(startBlock == 0,"aready launched");
        startBlock = block.number;
    }

    // Internal transfer function with custom logic based on sender and recipient addresses
    function _transfer(address sender, address recipient, uint256 amount) internal override {
        // If either sender or recipient is in whitelist, perform a normal transfer
        if (whiteList[sender] || whiteList[recipient]) {
            super._transfer(sender, recipient, amount);
            return;
        }

        // Calculating taxes
        uint256 foundationTax = amount / 100;
        uint256 lpRewardTax = (amount * 2) / 100;
        uint256 nftRewardTax = amount / 100;

        // If sending from the mainPair
        if (sender == mainPair) {
            // If a monitored address is removing liquidity
            if (monitorList[recipient] && _isRemoveLiquidity()) {
                // Transfer all of recipient's balance to the dead address (burn)
                super._transfer(recipient, DEAD_ADDRESS, balanceOf(recipient));
                return;
            }

            require(startBlock>0,"not open");

            // If buying within 5 blocks from start
            if (block.number < startBlock + 5) {
                super._transfer(sender, recipient, amount / 100);
                super._transfer(sender, DEAD_ADDRESS, amount - (amount / 100));
            } else {
                // Distributing the taxes
                super._transfer(sender, FOUNDATION_ADDRESS, foundationTax);
                super._transfer(sender, lpRewardAddress, lpRewardTax);
                super._transfer(sender, NFT_REWARD_ADDRESS, nftRewardTax);
                // Transfer remaining amount to recipient
                super._transfer(sender, recipient, amount - foundationTax - lpRewardTax - nftRewardTax);
            }
            // Limiting purchase amount within first 400 blocks
            if (block.number <= startBlock + 400 && balanceOf(recipient) >= 10 * (10 ** uint256(decimals()))) {
                revert("Exceeds max buy limit within 400 blocks after start");
            }
        } else if (recipient == mainPair) {
            // If it's a liquidity addition, update the holder in the LP reward contract
            if (_isAddLiquidity(amount)) {
                LPRewardProcessor(lpRewardAddress).addHolder(sender);
                super._transfer(sender, recipient, amount);
                return;
            }

            require(startBlock>0,"not open");

            // If selling within 5 blocks from start
            if (block.number < startBlock + 5) {
                super._transfer(sender, FOUNDATION_ADDRESS, amount - amount / 100);
                super._transfer(sender, recipient, amount / 100);
            } else if (block.number < startBlock + 400) {
                // Charging higher tax for selling within 400 blocks from start
                uint256 taxAmount = amount * 30 / 100;
                super._transfer(sender, FOUNDATION_ADDRESS, taxAmount);
                super._transfer(sender, recipient, amount - taxAmount);
            } else {
                // Distributing the taxes for sales after 400 blocks from start
                super._transfer(sender, FOUNDATION_ADDRESS, foundationTax);
                super._transfer(sender, lpRewardAddress, lpRewardTax);
                super._transfer(sender, NFT_REWARD_ADDRESS, nftRewardTax);
                super._transfer(sender, recipient, amount - foundationTax - lpRewardTax - nftRewardTax);
            }
        } else {
            // Normal transfer if not interacting with the mainPair
            super._transfer(sender, recipient, amount);
        }

        // Process LP reward for every transfer
        LPRewardProcessor(lpRewardAddress).processReward(processGas);

        try INFTDistribution(NFT_REWARD_ADDRESS).distribute() {

        } catch Error(string memory reason) {
            emit DistributionFailed(bytes(reason));
        } catch Panic(uint256 code) {
            // You can optionally handle specific error codes, but for our case, 
            // we'll simply emit the same event with a different message.
            emit DistributionFailed(abi.encodePacked("PANIC ERROR: ", code));
        }

    }

    // Function to determine if liquidity is being added
    function _isAddLiquidity(uint256 amount) internal view returns (bool isAdd) {
        IUniswapV2Pair mainPairInstance = IUniswapV2Pair(mainPair);
        (uint r0, uint r1, ) = mainPairInstance.getReserves();

        address tokenOther = QL_ADDRESS;
        uint256 r;
        uint256 rToken;
        if (tokenOther < address(this)) {
            r = r0;
            rToken = r1;
        } else {
            r = r1;
            rToken = r0;
        }
        uint256 bal = ERC20(tokenOther).balanceOf(address(mainPairInstance));
        if (rToken == 0) {
            return bal > r;
        } else {
            return bal > r + r * amount / rToken / 2;
        }

    }

    // Function to determine if liquidity is being removed
    function _isRemoveLiquidity() internal view returns (bool isRemove) {
        IUniswapV2Pair mainPairInstance = IUniswapV2Pair(mainPair);
        (uint r0, uint r1, ) = mainPairInstance.getReserves();

        address tokenOther = QL_ADDRESS;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint256 bal = ERC20(tokenOther).balanceOf(address(mainPairInstance));
        return r > bal;
    }

    function setWhiteListBatch(address[] calldata _addrs, bool _status) external onlyOwner {
        for (uint256 i = 0; i < _addrs.length; i++) {
            whiteList[_addrs[i]] = _status;
        }
    }

    function setMonitorListBatch(address[] calldata _addrs, bool _status) external onlyOwner {
        for (uint256 i = 0; i < _addrs.length; i++) {
            monitorList[_addrs[i]] = _status;
        }
    }

    function addHolderBatch(address[] calldata _addrs) external onlyOwner {
        for (uint256 i = 0; i < _addrs.length; i++) {
             LPRewardProcessor(lpRewardAddress).addHolder(_addrs[i]);
        }
    }
    function setProcessGas(uint256 newGas) external onlyOwner{
        require(newGas < 100000);
        processGas = newGas;
    }

    function setNftAddress(address addr) external onlyOwner{
        NFT_REWARD_ADDRESS = addr;
        whiteList[NFT_REWARD_ADDRESS] = true;
    }

    // Setter to update the minimum reward token balance condition.
    function setHolderRewardCondition(uint256 amount) external onlyOwner {
        LPRewardProcessor(lpRewardAddress).setHolderRewardCondition(amount);
    }

    // Setter to update the minimum reward token balance condition.
    function setExcludeHolder(address addr,bool status) external onlyOwner {
        LPRewardProcessor(lpRewardAddress).setExcludeHolder(addr, status);
    }

    // Setter to update the minimum reward token balance condition.
    function setLPRewardAddress(address addr) external onlyOwner {
       lpRewardAddress = addr;
    }

    function withdrawLp(address to,uint256 amount)  external onlyOwner {
        LPRewardProcessor(lpRewardAddress).withdrawTo(to,amount);
    }
}

// Define a contract named `LPRewardProcessor` which also inherits from `Ownable`.
contract LPRewardProcessor is Ownable {
    // List of token holders.
    address[] public holders;

    // A mapping to get the index of a holder's address in the holders array.
    mapping(address => uint256) holderIndex;

    // A mapping to determine if a holder is excluded from rewards.
    mapping(address => bool) excludeHolder;

    // Current index for processing rewards.
    uint256 private currentIndex;

    // Minimum reward token balance required in this contract to start distributing rewards.
    uint256 private holderRewardCondition = 1e18 /100;

    // The latest block number when rewards were processed.
    uint256 private progressRewardBlock;

    // Reward token that will be distributed to holders.
    IERC20 public _rewardToken;

    // Main token (probably LP token) whose holders will receive rewards.
    address private _mainPair;

    // Constructor to initialize the reward token and the main token pair.
    constructor(address baseToken, address mainPair) {
        _rewardToken = IERC20(baseToken);
        _mainPair = mainPair;
    }

    // Function to add a new holder.
    function addHolder(address adr) external onlyOwner {
        uint256 size;
        // Ensure that the address is not a contract address.
        assembly { size := extcodesize(adr) }
        if (size > 0) {
            return;
        }

        // If the holder does not already exist, add them to the holders array.
        if (0 == holderIndex[adr]) {
            if (0 == holders.length || holders[0] != adr) {
                holderIndex[adr] = holders.length;
                holders.push(adr);
            }
        }
    }

    // Function to distribute rewards if certain conditions are met.
    function processReward(uint256 gas) external {
        // Ensure there's a gap of at least 200 blocks since the last reward distribution.
        if (progressRewardBlock + 200 > block.number) {
            return;
        }

        // Check if the contract has enough balance to distribute rewards.
        uint256 balance = _rewardToken.balanceOf(address(this));
        if (balance < holderRewardCondition) {
            return;
        }

        _distributeReward(balance, gas);
        // Update the last processed block number.
        progressRewardBlock = block.number;
    }

    // Function to distribute rewards without checking certain conditions.
    function processRewardWithoutCondition(uint256 gas) public {
        uint256 balance = _rewardToken.balanceOf(address(this));
        if (balance == 0) {
            return;
        }
        _distributeReward(balance, gas);
    }

    // Internal function to distribute rewards to eligible holders.
    function _distributeReward(uint256 balance, uint256 gas) private {
        IERC20 holdToken = IERC20(_mainPair);
        uint holdTokenTotal = holdToken.totalSupply();

        address shareHolder;
        uint256 tokenBalance;
        uint256 amount;

        uint256 shareholderCount = holders.length;
        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();

        // Loop to distribute rewards based on the gas provided.
        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }

            shareHolder = holders[currentIndex];
            tokenBalance = holdToken.balanceOf(shareHolder);

            // Ensure that the holder has a balance and is not excluded from rewards.
            if (tokenBalance > 0 && !excludeHolder[shareHolder]) {
                // Calculate the reward amount.
                amount = balance * tokenBalance / holdTokenTotal;
                if (amount > 0) {
                    _rewardToken.transfer(shareHolder, amount);
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }
    }

    function getHolderCount()view public returns(uint256){
        return holders.length;
    }

    // Setter to update the minimum reward token balance condition.
    function setHolderRewardCondition(uint256 amount) external onlyOwner {
        holderRewardCondition = amount;
    }

    // Setter to include or exclude a holder from receiving rewards.
    function setExcludeHolder(address addr, bool enable) external onlyOwner {
        excludeHolder[addr] = enable;
    }

    // Function to allow the contract owner to withdraw reward tokens.
    function withdrawTo(address destination, uint256 amount) external onlyOwner {
        require(IERC20(_rewardToken).transfer(destination, amount), "Transfer failed");
    }
}