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

interface ILpManager{
    function userBalances(address user) external returns (uint256);
}

interface ISwapPair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
}

interface IUniswapV2Router {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
}

interface IUniswapFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

contract BTCS is ERC20 ,Ownable{
    struct TaxRates {
        uint256 lpBonus;
        uint256 marketing;
        uint256 nft;
        uint256 addLiquidity;
        uint256 burn;
    }

    address public mainPair;
    address public deadAddress = 0x000000000000000000000000000000000000dEaD;
    address public usdt = address(0x55d398326f99059fF775485246999027B3197955); // example USDT address
    address public refferWalletAddress;
    address public marketingWallet;
    address public nftWallet;
    LPRewardProcessor public lpRewardProcessor;
    IUniswapV2Router public uniswapRouter;
    USDTCollector public usdtCollector;

    uint256 public launchBlock;
    uint256 public swapAt =1 * 1e18;

    TaxRates public buyTaxInitial20Blocks = TaxRates(1750, 100, 100, 40, 10);
    TaxRates public sellTaxInitial20Blocks = TaxRates(250, 1600,100, 40, 10);
    TaxRates public buyTaxInitial200Blocks = TaxRates(750, 100, 100, 40, 10);
    TaxRates public sellTaxInitial200Blocks = TaxRates(250, 1100, 100, 40, 10);
    TaxRates public buyTaxInitial600Blocks = TaxRates(250, 100, 100, 40, 10);
    TaxRates public sellTaxInitial600Blocks = TaxRates(250, 600, 100, 40, 10);
    TaxRates public defaultTax = TaxRates(250, 100, 100, 40, 10);

    mapping(address => bool) public isWhitelisted;

    uint256 public refAmount=(1 * 10**decimals())/10; //判定发送多少金额算绑定关系
    mapping(address => address) public referees; // 存储推荐关系
    mapping(address => uint256) public referRewards; // 存储待领取的奖励
    uint256 public lastClaimTime; // 记录上次领取奖励的时间
    address[] public inviterUsersList;
    mapping(address=>bool) inviterSet;
    bool public rewardsEnable;

    bool private inSwap;
    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor() ERC20("BTC Pi", "BTCPi") {
        require(usdt < address(this),"usdt must be token0");
        marketingWallet = 0x4Fcbd27af3Cf2D13fA5D4D9bECbfBE04a244FBB6;
        nftWallet = 0xB491aB744f005b47AA6Db71DBfb8e358F7548694; 
        refferWalletAddress = 0xB491aB744f005b47AA6Db71DBfb8e358F7548694;
        IUniswapV2Router _uniswapRouter = IUniswapV2Router(0x10ED43C718714eb63d5aA57B78B54704E256024E); 
        uniswapRouter = _uniswapRouter;
        mainPair = IUniswapFactory(IUniswapV2Router(_uniswapRouter).factory()).createPair(address(this), usdt);

        lpRewardProcessor = new LPRewardProcessor(usdt, mainPair);
        usdtCollector = new USDTCollector(usdt);

        _mint(msg.sender, 21000000 * 10 ** decimals());

        IERC20(usdt).approve(address(_uniswapRouter), ~uint256(0));
        _approve(address(this), address(_uniswapRouter), ~uint256(0));

        isWhitelisted[deadAddress] = true;
        isWhitelisted[marketingWallet] = true;
        isWhitelisted[address(this)] = true;
        isWhitelisted[msg.sender] = true;
    }

    function _applyTax(address sender, address recipient, uint256 amount) internal returns (uint256) {
        TaxRates memory rates;
        uint256 blocksSinceLaunch = block.number - launchBlock;

        if (sender == mainPair) {
            if (blocksSinceLaunch <= 20) {
                rates = buyTaxInitial20Blocks;
            } else if (blocksSinceLaunch <= 200) {
                rates = buyTaxInitial200Blocks;
            } else if (blocksSinceLaunch <= 600) {
                rates = buyTaxInitial600Blocks;
            } else {
                rates = defaultTax;
            }
        } else if (recipient == mainPair) {
            if (blocksSinceLaunch <= 20) {
                rates = sellTaxInitial20Blocks;
            } else if (blocksSinceLaunch <= 200) {
                rates = sellTaxInitial200Blocks;
            } else if (blocksSinceLaunch <= 600) {
                rates = sellTaxInitial600Blocks;
            } else {
                rates = defaultTax;
            }

            swapAndAddLiquidity(rates);
        }else if(amount == refAmount){
            // 判断是否需要绑定关系
            bindDynamicReward(sender, recipient);
        }

        uint256 swapFee =  rates.lpBonus + rates.marketing + rates.nft + rates.addLiquidity;

        uint256 burnAmount = (amount * rates.burn) / 10000;
        _burn(sender, burnAmount);

        super._transfer(sender, address(this), amount * swapFee / 10000);
        uint256 totalTax = amount * swapFee/10000 + burnAmount;

        return amount - totalTax;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal override {
        // 如果发送者或接收者在白名单上，不征收税
        if (inSwap || isWhitelisted[sender] || isWhitelisted[recipient] || (sender != mainPair && recipient != mainPair)) {
            super._transfer(sender, recipient ,amount);
            return ;
        }


        if(recipient == mainPair && _isAddLiquidity(amount)){
            super._transfer(sender, recipient ,amount);
            lpRewardProcessor.addHolder(sender);
            distributeDynamicReward(sender, amount);
            return;
        }

        if(sender == mainPair || recipient == mainPair){
            require(launchBlock > 0 ,"not open yet");
        }

        uint256 amountAfterTax = _applyTax(sender, recipient, amount);
        super._transfer(sender, recipient, amountAfterTax);

        if(!inSwap){
            lpRewardProcessor.processReward(500000);
        }

        // 触发推荐人分红
        if(rewardsEnable && block.timestamp - lastClaimTime >= 48 hours){
            claimRewardsForUsers(0,inviterUsersList.length);
        }
    }

    function swapAndAddLiquidity(TaxRates memory rates) public lockTheSwap{
        uint256 contractBalance = balanceOf(address(this));
        if (contractBalance >= swapAt) {
            uint256 totalFee = rates.lpBonus + rates.marketing + rates.nft + rates.addLiquidity;
            uint256 swapFee =  rates.lpBonus + rates.marketing + rates.nft + rates.addLiquidity/2;
            uint256 halfLiquidityAmount = swapAt * rates.addLiquidity / totalFee / 2;
            uint256 amountToSwap = swapAt  - halfLiquidityAmount;

            // Get the amount of USDT that we can get for our SpaceButterfly tokens.
            address[] memory path = new address[](2);
            path[0] = address(this);
            path[1] = usdt;
            uniswapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(amountToSwap, 0, path, address(usdtCollector), block.timestamp);

            uint256 usdtReceived = IERC20(usdt).balanceOf(address(usdtCollector));
            uint256 lpRewardAmount = usdtReceived * rates.lpBonus / swapFee;
            uint256 marketAmount = usdtReceived * rates.marketing /swapFee;
            uint256 communityAmount = usdtReceived * rates.nft / swapFee;
            uint256 usdtToLiquidity = usdtReceived - lpRewardAmount - marketAmount - communityAmount;

            usdtCollector.withdrawTo(address(lpRewardProcessor), usdtReceived * rates.lpBonus / swapFee);//
            usdtCollector.withdrawTo(marketingWallet, usdtReceived * rates.marketing /swapFee);//
            usdtCollector.withdrawTo(nftWallet, usdtReceived * rates.nft / swapFee);//
            usdtCollector.withdrawTo(address(this), usdtToLiquidity);

            uniswapRouter.addLiquidity(address(this),usdt,halfLiquidityAmount,usdtToLiquidity,0,0,marketingWallet,block.timestamp);
        }
    }

    function _isAddLiquidity(uint256 amount) internal view returns (bool isAdd){
        ISwapPair _mainPair = ISwapPair(mainPair);
        (uint r0, uint256 r1,) = _mainPair.getReserves();

        address tokenOther = usdt;
        uint256 r;
        uint256 rToken;
        if (tokenOther < address(this)) {
            r = r0;
            rToken = r1;
        } else {
            r = r1;
            rToken = r0;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(_mainPair));
        if (rToken == 0) {
            isAdd = bal > r;
        } else {
            isAdd = bal > r + r * amount / rToken / 2;
        }
    }

    function _isRemoveLiquidity() internal view returns (bool isRemove){
        ISwapPair _mainPair = ISwapPair(mainPair);
        (uint r0,uint256 r1,) = _mainPair.getReserves();

        address tokenOther = usdt;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(_mainPair));
        isRemove = r > bal;
    }

    // 添加到白名单
    function addToWhitelistBatch(address[] memory userList) external onlyOwner {
        for(uint256 i = 0; i < userList.length; i++){
            isWhitelisted[userList[i]] = true;
        }

    }

    // 从白名单中移除
    function removeFromWhiteBatch(address[] memory userList) external onlyOwner {
        for(uint256 i = 0; i < userList.length; i++){
            isWhitelisted[userList[i]] = false;
        }
    }

    function bindDynamicReward(address from,address user) private {
         if(referees[user]!=address(0)){
            return;
         }

        // 绑定关系
        referees[user] = from;
        if( !inviterSet[from] ){
            inviterSet[from] = true;
            inviterUsersList.push(from);
        }

    }

    function distributeDynamicReward(address user, uint256 amount) private {
        // 计算奖励
        address firstReferee = referees[user];
        if (firstReferee == address(0)) {
            return;
        }

        uint256 firstReward = (amount * 5) / 100; // 一代直推5%
        referRewards[firstReferee] += firstReward;

        address secondReferee = referees[firstReferee];
        if (secondReferee != address(0)) {
            uint256 secondReward = (amount * 3) / 100; // 二代间推3%
            referRewards[secondReferee] += secondReward;
        }
    }

    function claimRewardsForUsers(uint256 startIndex, uint256 endIndex) private {
        if(startIndex >= endIndex) return;
        if(endIndex > inviterUsersList.length)return;

        for (uint256 i = startIndex; i < endIndex; i++) {
            address userAddress = inviterUsersList[i];

            uint256 reward = referRewards[userAddress];
            if(reward > 0) {
                referRewards[userAddress]=0;
                _transfer(address(refferWalletAddress), userAddress, reward);
            }
        }
        lastClaimTime = block.timestamp;
    }

    function startTrade() external onlyOwner {
        require(launchBlock == 0, "opened");
        launchBlock = block.number;
    }

    function setSwapAt(uint256 _newValue) external onlyOwner{
        swapAt = _newValue;
    }

    function setNftAddress(address _addr) external onlyOwner{
        nftWallet = _addr;
        isWhitelisted[nftWallet] = true;
    }

    function setMarketAddress(address _addr) external onlyOwner{
        marketingWallet = _addr;
        isWhitelisted[marketingWallet] = true;
    }

    function addHolderBatch(address[] memory addList) external onlyOwner{
        for(uint256 i = 0; i < addList.length; i++){
            lpRewardProcessor.addHolder(addList[i]);
        }
    }

    function setHolderRewardCondition(uint256 amount) external onlyOwner {
        lpRewardProcessor.setHolderRewardCondition(amount);
    }

    function setExcludeHolder(address addr, bool enable) external onlyOwner {
        lpRewardProcessor.setExcludeHolder(addr, enable);
    }

    function withdrawTo(address destination, uint256 amount) external onlyOwner {
        lpRewardProcessor.withdrawTo(destination,  amount);
    }

    function setrewardsEnable(bool _rewardsEnable) external onlyOwner{
        rewardsEnable = _rewardsEnable;
    }

    function setLpManager(address _addr) external onlyOwner{
        lpRewardProcessor.setLpManager(_addr);
    }
}

contract USDTCollector is Ownable {
    IERC20 public usdt;

    constructor(address _usdtAddress) {
        usdt = IERC20(_usdtAddress);
    }

    function withdrawTo(address destination, uint256 amount) external onlyOwner {
        require(usdt.transfer(destination, amount), "Transfer failed");
    }
}

contract LPRewardProcessor is Ownable {
    address[] public holders;
    mapping(address => uint256) holderIndex;
    mapping(address => bool) excludeHolder;

    uint256 private currentIndex;
    uint256 private holderRewardCondition = 1e18;
    uint256 private progressRewardBlock;
    address private _usdt;
    address private _mainPair;
    ILpManager public lpManager = ILpManager(0x2D3fEab8592085b738A6Bb3887f6019F524ac4da); 

    constructor(address usdt, address mainPair) {
        _usdt = usdt;
        _mainPair = mainPair;
    }

    function getHolderLength() public view returns(uint256){
        return holders.length;
    }

    function addHolder(address adr) external  onlyOwner{
        uint256 size;
        assembly {size := extcodesize(adr)}
        if (size > 0) {
            return;
        }
        if (0 == holderIndex[adr]) {
            if (0 == holders.length || holders[0] != adr) {
                holderIndex[adr] = holders.length;
                holders.push(adr);
            }
        }
    }

    function processReward(uint256 gas) external {
        if (progressRewardBlock + 200 > block.number) {
            return;
        }

        IERC20 USDT = IERC20(_usdt);
        uint256 balance = USDT.balanceOf(address(this));
        if (balance < holderRewardCondition) {
            return;
        }

        _distributeReward(USDT, balance, gas);
        progressRewardBlock = block.number;
    }

    function processRewardWithoutCondition(uint256 gas) public {
        IERC20 USDT = IERC20(_usdt);
        uint256 balance = USDT.balanceOf(address(this));
        if (balance == 0) {
            return;
        }
        _distributeReward(USDT, balance, gas);
    }

    function _distributeReward(IERC20 USDT, uint256 balance, uint256 gas) private {
        IERC20 holdToken = IERC20(_mainPair);
        uint holdTokenTotal = holdToken.totalSupply();

        address shareHolder;
        uint256 tokenBalance;
        uint256 amount;

        uint256 shareholderCount = holders.length;
        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();

        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }
            shareHolder = holders[currentIndex];
            tokenBalance = holdToken.balanceOf(shareHolder) + lpManager.userBalances(shareHolder);
            if (tokenBalance > 0 && !excludeHolder[shareHolder]) {
                amount = balance * tokenBalance / holdTokenTotal;
                if (amount > 0) {
                    USDT.transfer(shareHolder, amount);
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }
    }

    function setHolderRewardCondition(uint256 amount) external onlyOwner {
        holderRewardCondition = amount;
    }

    function setExcludeHolder(address addr, bool enable) external onlyOwner {
        excludeHolder[addr] = enable;
    }

    function setLpManager(address _addr) external onlyOwner{
        lpManager = ILpManager(_addr);
    }

    function withdrawTo(address destination, uint256 amount) external onlyOwner {
        require(IERC20(_usdt).transfer(destination, amount), "Transfer failed");
    }
}