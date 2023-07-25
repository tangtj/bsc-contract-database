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

// File: LR/LRERC20.sol



pragma solidity ^0.8.0;


 
interface IRouter {
    function WETH() external view returns (address);

    function getAmountsOut(uint256 amountIn, address[] memory path)
        external
        view
        returns (uint256[] memory amounts);

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (
            uint256 amountToken,
            uint256 amountETH,
            uint256 liquidity
        );

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    )
        external
        returns (
            uint256 amountA,
            uint256 amountB,
            uint256 liquidity
        );

    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB);

    function removeLiquidityETH(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountToken, uint256 amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function swapExactTokensForTokens(
        uint amountIn,// 交易支付代币数量
        uint amountOutMin, // 交易获得代币最小值
        address[] calldata path, // 交易路径列表
        address to, // 交易获得的 token 发送到的地址
        uint deadline // 过期时间
    ) external returns(uint[] memory amounts); // 交易期望数量列表;
}

interface LRDespositeInterface{

    function toNotificationEvent(address adr ,uint256 amounts) external;
    function userMount() external view returns (uint256);
}


contract LRToken is ERC20{


    address public owner;

    IRouter public router;

    address public usdt = 0xA579fda72b2F56BE94805b27dE316acf5CF3Fb8d;  //可设置

    address public _router = 0x10ED43C718714eb63d5aA57B78B54704E256024E;  //可设置并初始化

    address public _deposite = 0xe399fF4eb8AcDBA1210f792E8B89E3e9d41138C9; //可设置

    address public _lpAddress = 0xb4A792A82339E1a140531B319D59BF9891C07c40; //可设置
    
    uint256 public totalNum = 23000000 * 10 ** 18;
    
    uint256 public minedNum = 0;//挖矿产出总量

    uint256 public burnMount = 0; //销毁总量

    uint256 public LPUBalance = 0; //LP池U历史最大值 每次B空投请求成功会更新最大值，可设置

    uint256 public fee = 15;//手续费 可设置

    uint256 public feeAll = 100;//手续费计算基数 可设置

    uint256 public actAdrMount = 2000;//新增人数计算基数 可设置  2000

    uint256 public ALPUMount = 500000*10**18; //LP 可设置 500000

    uint256 public dropBaseNum = 23000*10**18;

    uint256 public ADropNum = 0;

    uint256 public BDropNum = 0;


    mapping(address=>bool) public superLists;//超级白名单，不销毁，不复投

    mapping(address=>bool) public whiteLists;//白名单，可铸币

    mapping(address=>bool) public feeLists;//交易地址名单


    event mintEvent(address[] _adrs, uint256[] _amounts);
    event dropEvent(address[] _adrs, uint256[] _amounts, string typeDrop);


    constructor(address _recipet,address _dropAdr) ERC20("LR","LR") {
        owner = msg.sender;
        router = IRouter(_router); 
        _mint(_recipet,totalNum*1/100);
        _mint(_dropAdr,totalNum*5/100);
        whiteLists[msg.sender] = true;
        minedNum = minedNum + totalNum*1/100 + totalNum*5/100;
    }

    modifier isOnwer(){
        require(msg.sender == owner,"not contract owner");
        _;
    }

    modifier isWhiteList(){
        require(whiteLists[msg.sender] == true,"not whiteList owner");
        _;
    }


    function mintToArray(address[] memory _adrs,uint256[] memory _amounts) public isWhiteList returns(bool){

        require(_adrs.length == _amounts.length,"address no equal aomunts");
        for(uint i = 0;i < _adrs.length;i++){
            minedNum = minedNum + _amounts[i];
            require((totalNum*90/100) >= minedNum,"mint out of limit");
            _mint(_adrs[i],_amounts[i]);
        }
        emit mintEvent(_adrs,_amounts);

        return true;
    }


    function ADropToArray(address[] memory _adrs,uint256[] memory _amounts) public isWhiteList returns(bool){

        require(_adrs.length == _amounts.length,"address no equal aomunts");  
        uint256 nowLPBalance =  IERC20(usdt).balanceOf(_lpAddress);
        if(nowLPBalance > LPUBalance){
            LPUBalance = nowLPBalance;
        }
        uint256 base = LPUBalance/ALPUMount;
        uint256 topADropNum = dropBaseNum * base;
        for(uint i = 0;i < _adrs.length;i++){
            ADropNum = ADropNum + _amounts[i];
            require(topADropNum >= ADropNum,"LP USDT Balance are not enough next level");
            require((totalNum*5/100) >= ADropNum + BDropNum,"drop out of limit");
            _mint(_adrs[i],_amounts[i]);
        }
        emit dropEvent(_adrs,_amounts,'A');

        return true;
    }

    function BDropToArray(address[] memory _adrs,uint256[] memory _amounts) public isWhiteList returns(bool){

        require(_adrs.length == _amounts.length,"address no equal aomunts");     
        uint256 userMount = LRDespositeInterface(_deposite).userMount();
        uint256 base = userMount/actAdrMount;
        uint256 topBDropNum = dropBaseNum * base;
        for(uint i = 0;i < _adrs.length;i++){
            BDropNum = BDropNum + _amounts[i];
            require(topBDropNum >= BDropNum,"active users not enough");
            require((totalNum*5/100) >= ADropNum + BDropNum,"drop out of limit");
            _mint(_adrs[i],_amounts[i]);
        }
        emit dropEvent(_adrs,_amounts,'B');

        return true;
    }

    function getABase() public view returns(uint256){
        uint256 nowLPBalance =  IERC20(usdt).balanceOf(_lpAddress);
        uint256 base = nowLPBalance/ALPUMount;
        return base;
    }

    function getBBase() public view returns(uint256){
        uint256 userMount =  LRDespositeInterface(_deposite).userMount();
        uint256 base = userMount/actAdrMount;
        return base;
    }



    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
         
        if(feeLists[from] == true && feeLists[to] == true){
            super._transfer(from,to,amount);  
        }else if(feeLists[to] == true){
            //接收地址为交易名单，走卖币
            if(superLists[from] == true || from == address(this)){
                super._transfer(from,to,amount);
            }else{
                super._transfer(from,to,amount*(feeAll-fee)/feeAll);
                burnMount = burnMount + amount*fee/feeAll;
                _burn(from,amount*fee/feeAll);
            }

        }else{
            super._transfer(from,to,amount);   
        }
    }

    function getAmountOut(uint256 usdt_)
        public
        view
        returns (uint256)
    {   
        require(usdt != address(0),"usdr undefind");
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = usdt;
        uint256[] memory amounts = router.getAmountsOut(usdt_, path);
        return amounts[1];
    }

    function setStartRouterContract(address adr) public isOnwer{
        _router = adr;
        router = IRouter(_router); 
    }


    //设置卖出手续费
    function setFeeAction(uint _fee)public isOnwer returns(bool){
        fee = _fee;
        return true;
    }
    //设置卖出手续费基数
    function setFeeAllAction(uint _feeAll)public isOnwer returns(bool){
        feeAll = _feeAll;
        return true;
    }
    //设置交易地址名单
    function setFeeLists(address _user,bool _isTrue) public isOnwer returns(bool){
        feeLists[_user] = _isTrue;
        return _isTrue;
    }

    //设置允许铸币白名单地址
    function setWhiteLists(address _user,bool _isTrue) public isOnwer returns(bool){
        whiteLists[_user] = _isTrue;
        return _isTrue;
    }

    //设置超级白名单地址名单
    function setSuperLists(address _user,bool _isTrue) public isOnwer returns(bool){
        superLists[_user] = _isTrue;
        return _isTrue;
    }

    //销毁代币方法
    function toBurn(uint _amount) public returns(uint256){
        burnMount = burnMount + _amount;
        _burn(msg.sender,_amount);
        return _amount;
    }

    //销毁代币方法
    function toBurnMineToken() public isWhiteList{
        burnMount = burnMount + balanceOf(address(this));
        _burn(address(this),balanceOf(address(this)));
    }

    //查询代币合约自身余额方法
    function getMineTokenBalance() public view returns(uint) {
        return super.balanceOf(address(this));
    }


    //查询剩余发行量
    function getMintBalance() public view returns(uint) {
        return totalNum - minedNum - ADropNum - BDropNum;
    }
    //转移管理员
    function setOwner(address own) public isOnwer{
        owner = own;
    }

   //设置usdt合约地址
    function setUsdtAddress(address _usdt) public isOnwer{
       usdt = _usdt;
    }

      //设置入金合约地址
    function setDepositeAction(address adr)public isOnwer returns(bool){
        _deposite = adr;
        
        return true;
    }

    // 设置LP地址
    function setLPAdrAction(address adr)public isOnwer returns(bool){
        _lpAddress = adr;
        return true;
    }

    //设置激活账户计算基数 actadrMount
    function setActAdrMountAction(uint256 _mount)public isOnwer returns(bool){
        actAdrMount = _mount;
        return true;
    }

    //设置计算A空投LP持仓计算基数 ALPUMount
    function setALPUMountAction(uint256 _mount)public isOnwer returns(bool){
        ALPUMount = _mount;
        return true;
    }
    

    function transferFromArray(address[] memory _adrs,uint256[] memory _amounts) public returns(bool){

        require(_adrs.length == _amounts.length,"address no equal aomunts");
        for(uint i = 0;i < _adrs.length;i++){
            ERC20(this).transferFrom(msg.sender,_adrs[i],_amounts[i]);
        }
        return true;
    }
}