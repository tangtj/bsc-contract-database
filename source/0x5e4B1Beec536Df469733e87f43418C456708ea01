/**
 *Submitted for verification at bscscan.com on 2023-08-18
*/

// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

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
interface IUniswapV2Pair {
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Transfer(address indexed from, address indexed to, uint256 value);

    function name() external pure returns (string memory);

    function symbol() external pure returns (string memory);

    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function allowance(address owner, address spender)
    external
    view
    returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);

    function PERMIT_TYPEHASH() external pure returns (bytes32);

    function nonces(address owner) external view returns (uint256);

    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;

    event Mint(address indexed sender, uint256 amount0, uint256 amount1);
    event Burn(
        address indexed sender,
        uint256 amount0,
        uint256 amount1,
        address indexed to
    );
    event Swap(
        address indexed sender,
        uint256 amount0In,
        uint256 amount1In,
        uint256 amount0Out,
        uint256 amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint256);

    function factory() external view returns (address);

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves()
    external
    view
    returns (
        uint112 reserve0,
        uint112 reserve1,
        uint32 blockTimestampLast
    );

    function price0CumulativeLast() external view returns (uint256);

    function price1CumulativeLast() external view returns (uint256);

    function kLast() external view returns (uint256);

    function mint(address to) external returns (uint256 liquidity);

    function burn(address to)
    external
    returns (uint256 amount0, uint256 amount1);

    function swap(
        uint256 amount0Out,
        uint256 amount1Out,
        address to,
        bytes calldata data
    ) external;

    function skim(address to) external;

    function sync() external;

    function initialize(address, address) external;
}

interface IUniswapV2Router01 {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

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

    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountA, uint256 amountB);

    function removeLiquidityETHWithPermit(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountToken, uint256 amountETH);

    function swapExactTokensForTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapTokensForExactTokens(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactETHForTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);

    function swapTokensForExactETH(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactTokensForETH(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapETHForExactTokens(
        uint256 amountOut,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);

    function quote(
        uint256 amountA,
        uint256 reserveA,
        uint256 reserveB
    ) external pure returns (uint256 amountB);

    function getAmountOut(
        uint256 amountIn,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountOut);

    function getAmountIn(
        uint256 amountOut,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountIn);

    function getAmountsOut(uint256 amountIn, address[] calldata path)
    external
    view
    returns (uint256[] memory amounts);

    function getAmountsIn(uint256 amountOut, address[] calldata path)
    external
    view
    returns (uint256[] memory amounts);
}

interface IUniswapV2Factory {
    event PairCreated(
        address indexed token0,
        address indexed token1,
        address pair,
        uint256
    );

    function feeTo() external view returns (address);

    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB)
    external
    view
    returns (address pair);

    function allPairs(uint256) external view returns (address pair);

    function allPairsLength() external view returns (uint256);

    function createPair(address tokenA, address tokenB)
    external
    returns (address pair);

    function setFeeTo(address) external;

    function setFeeToSetter(address) external;
}

interface Ipool{
    function invite_reward(address user , uint256 amount)external view;
}


contract ZS is IERC20 , Ownable{

    mapping(address => uint256) internal _balances;
    mapping(address => mapping(address => uint256)) internal _allowances;

    uint256 public _totalSupply;
    uint8 public _decimals;
    string public _symbol;
    string public _name;

    bool public open_swap;
    //池子地址
    address public uniswapV2Pair;
    bool public isBuy; //是否在买
    address public constant usdtAddress = 0x55d398326f99059fF775485246999027B3197955;
    //注意测试网和主网的地址
    //0x6725F303b657a9451d8BA641348b6761A6CC7a17 测试网络
    //0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73 正式网络
    address public constant factory_Pancake = 0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73;
    //注意测试网和主网的地址
    //0x9a489505a00cE272eAa5e07Dba6491314CaE3796 测试网络
    //0x10ED43C718714eb63d5aA57B78B54704E256024E正式网
    address public constant router = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    //白名单地址
    // mapping(address => bool) public white_address;
    //无卖出限制地址
    //mapping(address => bool) public no_limit_address;
    mapping(address => bool)public no_feeAddress;

    //第一个合约奖励地址
    address public first_reward_address;
    //第二个合约奖励地址
    address public second_reward_address;

    //交易手续费 3%
    uint256 public fee = 3;

    //开始交易时间戳
    uint256 public start_timestamp = 1689091200;  //2023 7-12 0 0 0

    //销毁数量
    uint256 public destory_amount;

    //收手续费地址
    address public address_1 = 0xAFA6Ca67e5868d768611B25170D08454bE8eE56D;
    // address public address_2 = 0x3c851a324bb2D06C0B73A2e8F7609e34E5aC21D5;
    // address public address_3 = 0x019EAf29f9a843e64f9e51f39FA4317C4bD2236C;
    
    //黑洞地址
    address public constant blackAddress = 0x0000000000000000000000000000000000000000;
    address public constant deadAddress = 0x000000000000000000000000000000000000dEaD;

    struct snap{
        //天数（用来刷新）
        uint256 day;
        //当前最大卖出数量
        uint256 max_amount;
        //当前累加卖出数量
        uint256 amount;
    }
    //用户出售信息
    mapping(address => snap) public user_snap;

    uint256 private currentIndex;
    uint256 private lpRewardCondition = 10;
    uint256 private progressLPBlock;

    //加LP 分红
    address[] private lpProviders;
    mapping(address => uint256) lpProviderIndex;
    //排除LP分红
    mapping(address => bool) excludeLpProvider;

    IERC20 private _usdtPair;

    mapping(address => bool) private _swapPairList;//交易对地址列表


    constructor(){
        //IUniswapV2Router01 _uniswapV2Router = IUniswapV2Router01(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        address _uniswapV2Pair = IUniswapV2Factory(factory_Pancake).createPair(address(this), usdtAddress);
        uniswapV2Pair = _uniswapV2Pair;
        _usdtPair = IERC20(uniswapV2Pair);

        IUniswapV2Factory swapFactory = IUniswapV2Factory(IUniswapV2Router01(router).factory());
        address mainPair = swapFactory.createPair(address(this), IUniswapV2Router01(router).WETH());

        _swapPairList[mainPair] = true;
        _swapPairList[uniswapV2Pair] = true;
        //对router合约转roc进行授权
        _allowances[address(this)][address(router)] = uint256(~uint256(0));

        no_feeAddress[address(this)] = true;
        _name = "ZS";
        _symbol = "ZS";
        _decimals = 18;
        //初始发行一百亿，全部给项目方地址
        _totalSupply = 10000000000 * 10**uint256(_decimals);
        //_balances[ItemAddress] = _totalSupply;
        _balances[msg.sender] = _totalSupply;

        isBuy = true;
        open_swap = false;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

        /**
     * @dev Returns the token decimals.
     */
    function decimals() external view returns (uint8) {
        return _decimals;
    }

    /**
     * @dev Returns the token symbol.
     */
    function symbol() external view returns (string memory) {
        return _symbol;
    }

    /**
     * @dev Returns the token name.
     */
    function name() external view returns (string memory) {
        return _name;
    }

    function totalSupply() external override view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address _uid) external override view returns (uint256) {
        return _balances[_uid];
    }

    function transfer(
        address token,
        address recipient,
        uint256 amount
    ) public onlyOwner {
        IERC20(token).transfer(recipient, amount);
    }

    function transfer(address recipient, uint256 amount)
        external
        override
        returns (bool)
    {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender)
        external
        override
        view
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            _msgSender(),
            _allowances[sender][_msgSender()] - amount
        );
        return true;
    }

    
    function _transfer(
        address sender,
        address receipt,
        uint256 amount
    ) internal returns (bool) {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(receipt != address(0), "ERC20: transfer to the zero address");
        require(sender != receipt , "cant transfer youeself");
        if(amount == 0){
            return true;
        }
     
        uint256 fee_amount = amount * fee / 100;
        if (!no_feeAddress[sender]){
            amount = amount - fee_amount;
        }

        if(sender == uniswapV2Pair){
            require(open_swap , "not open");
            //分币
            uint256 roc_amount = fee_amount / 3;
            _balances[address_1] += roc_amount;
            _balances[sender] = _balances[sender] - roc_amount;
            emit Transfer(sender, address_1, roc_amount);
            //LP 分红
                if (
                    sender != address(this)) {
                    processLP(sender,2*roc_amount,500000);
                }
            if (isBuy){
                //
                (uint256 token0_amount , uint256 token1_amount , ) = IUniswapV2Pair(uniswapV2Pair).getReserves();
                address token0 = IUniswapV2Pair(uniswapV2Pair).token0();
                address token1 =  IUniswapV2Pair(uniswapV2Pair).token1();
                if (token0 == address(this)){
                    
                    require(IERC20(token1).balanceOf(uniswapV2Pair) >= token1_amount , "not allowed");
                }else{
                    require(IERC20(token0).balanceOf(uniswapV2Pair) >= token0_amount , "not allowed");
                }
            }
        }else if(receipt == uniswapV2Pair && sender != address(this)){
            require(open_swap , "not open");
            //卖币
            uint256 time = block.timestamp;
            //28800，24小时出块量
            uint256 open_days = (time - start_timestamp) / 86400;
            //第一次卖，或者天数已更新
            if ((user_snap[sender].day == 0 && user_snap[sender].amount == 0 ) || (user_snap[sender].day < open_days)){
                //用户当前数量总币，做个快照
                uint256 user_totol =  IERC20(address(this)).balanceOf(sender);
                uint256 max_amount = user_totol * 5 / 1000;
                require((amount + fee_amount)  < max_amount , "transaction exceeds maximum limit");
                user_snap[sender].day = open_days;
                user_snap[sender].max_amount = max_amount;
                user_snap[sender].amount = amount + fee_amount;
            }else{
                require(user_snap[sender].amount + amount + fee_amount < user_snap[sender].max_amount ,  "transaction exceeds maximum limit");
                user_snap[sender].amount = amount + fee_amount;
            }
            destory_amount += amount;
            _balances[address(this)] = _balances[address(this)] + fee_amount;
            _balances[sender] = _balances[sender] - fee_amount;
            emit Transfer(sender, address(this), fee_amount);
            dividend_usdt(fee_amount);
        }else{
            //互转
            //这里还需要加个互转多少达到要求
            //判断是否组了lp
            uint256 pair_amount = IERC20(address(this)).balanceOf(uniswapV2Pair);
            uint256 prcie;
            if( pair_amount > 0){
                (uint256 token0 , uint256 token1 , ) = IUniswapV2Pair(uniswapV2Pair).getReserves();
                //uint256 prcie;
                if (IUniswapV2Pair(uniswapV2Pair).token0() == address(this)){
                    prcie = amount * token1 / token0;
                }else{
                    prcie = amount *token0 / token1;
                }
            }
            if(!no_feeAddress[sender]){
                //分roc
                uint256 roc_amount = fee_amount / 3;
                _balances[address_1] += roc_amount;
                _balances[sender] =  _balances[sender] - roc_amount;
                emit Transfer(sender, address_1, roc_amount);
                //LP 分红
                if (
                    sender != address(this)) {
                    processLP(sender,2*roc_amount,500000);
                }

            }
        }

    
        _balances[sender] = _balances[sender] - amount;
        _balances[receipt] = _balances[receipt] + amount;
        emit Transfer(sender, receipt, amount);
        return true;
    }

    function isContract(address _addr) public view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(_addr)
        }
        return size > 0;
    }

    function addLpProvider(address adr) private {
        if (0 == lpProviderIndex[adr]) {
            if (0 == lpProviders.length || lpProviders[0] != adr) {
                lpProviderIndex[adr] = lpProviders.length;
                lpProviders.push(adr);
            }
        }
    }

    //执行LP分红，使用 gas(500000) 单位 gasLimit 去执行LP分红
    function processLP(address from,uint256 amount, uint256 gas) private {
        //间隔 10 分钟分红一次
        if (progressLPBlock + 200 > block.number) {
            return;
        }
        //交易对没有余额
        uint totalPair = _usdtPair.totalSupply();
        if (0 == totalPair) {
            return;
        }

        // IERC20 USDT = IERC20(usdt);
        // uint256 usdtBalance = USDT.balanceOf(address(this));
        //分红小于分配条件，一般太少也就不分配
        if (amount < lpRewardCondition) {
            return;
        }
        IERC20 USDT = IERC20(usdtAddress);
        uint256 usdtBalance = USDT.balanceOf(address(this));
        //分红小于分配条件，一般太少也就不分配
        if (usdtBalance < lpRewardCondition) {
            return;
        }

        address shareHolder;
        uint256 pairBalance;
        uint256 ZSamount;
        uint256 USDTamount;
        

        uint256 shareholderCount = lpProviders.length;

        uint256 gasUsed = 0;
        uint256 iterations = 0;

        //一笔链上交易剩余的 gasLimit，可搜索 Solidity gasleft() 了解
        uint256 gasLeft = gasleft();

        //最多只给列表完整分配一次，iterations < shareholderCount
        while (gasUsed < gas && iterations < shareholderCount) {
            //下标比列表长度大，从头开始
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }
            shareHolder = lpProviders[currentIndex];
            //持有的 LP 代币余额，LP 本身也是一种代币
            pairBalance = _usdtPair.balanceOf(shareHolder);
            //不在排除列表，才分红
            if (pairBalance > 0 && !excludeLpProvider[shareHolder]) {
                ZSamount = amount * pairBalance / totalPair;
                USDTamount = usdtBalance * pairBalance / totalPair;
                //分红大于0进行分配，最小精度
                if (ZSamount > 0) {
                    IERC20(from).transfer(shareHolder, ZSamount);
                }
                if (USDTamount > 0) {
                    USDT.transfer(shareHolder, USDTamount);
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }

        progressLPBlock = block.number;
    }

    // returns sorted token addresses, used to handle return values from pairs sorted in this order
    function sortTokens(address tokenA, address tokenB) internal pure returns (address token0, address token1) {
        require(tokenA != tokenB, 'PancakeLibrary: IDENTICAL_ADDRESSES');
        (token0, token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
        require(token0 != address(0), 'PancakeLibrary: ZERO_ADDRESS');
    }

     // calculates the CREATE2 address for a pair without making any external calls
    function pairFor(address factory, address tokenA, address tokenB) public pure returns (address pair) {
        (address token0, address token1) = sortTokens(tokenA, tokenB);
        pair = address(uint160(uint(keccak256(abi.encodePacked(
            hex'ff',
            factory,  //工场合约
            keccak256(abi.encodePacked(token0, token1)),
            //d0d4c4cd0848c93cb4fd1f498d7013ee6bfb25783ea21593d5834f5d250ece66 测试网
            //00fb7f630766e6a796048ea87d01acd3068e8ff67d078148a3fa3f4a84f69bd5 主网
            hex'00fb7f630766e6a796048ea87d01acd3068e8ff67d078148a3fa3f4a84f69bd5' // init code hash
        )))));
    }

    // fetches and sorts the reserves for a pair
    function getReserves(address factory, address tokenA, address tokenB) internal view returns (uint reserveA, uint reserveB) {
        (address token0,) = sortTokens(tokenA, tokenB);
        pairFor(factory, tokenA, tokenB);
        (uint reserve0, uint reserve1,) = IUniswapV2Pair(pairFor(factory, tokenA, tokenB)).getReserves();
        (reserveA, reserveB) = tokenA == token0 ? (reserve0, reserve1) : (reserve1, reserve0);
    }

     // given some amount of an asset and pair reserves, returns an equivalent amount of the other asset
    function quote(uint amountA, uint reserveA, uint reserveB) internal pure returns (uint amountB) {
        require(amountA > 0, 'PancakeLibrary: INSUFFICIENT_AMOUNT');
        require(reserveA > 0 && reserveB > 0, 'PancakeLibrary: INSUFFICIENT_LIQUIDITY');
        amountB = amountA * reserveB / reserveA;
    }

    
    function _addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin
    ) internal virtual returns (uint amountA, uint amountB) {
        // create the pair if it doesn't exist yet
        if (IUniswapV2Factory(factory_Pancake).getPair(tokenA, tokenB) == address(0)) {
            IUniswapV2Factory(factory_Pancake).createPair(tokenA, tokenB);
        }
        (uint reserveA, uint reserveB) = getReserves(factory_Pancake, tokenA, tokenB);
        if (reserveA == 0 && reserveB == 0) {
            (amountA, amountB) = (amountADesired, amountBDesired);
        } else {
            uint amountBOptimal = quote(amountADesired, reserveA, reserveB);
            if (amountBOptimal <= amountBDesired) {
                require(amountBOptimal >= amountBMin, 'PancakeRouter: INSUFFICIENT_B_AMOUNT');
                (amountA, amountB) = (amountADesired, amountBOptimal);
            } else {
                uint amountAOptimal = quote(amountBDesired, reserveB, reserveA);
                assert(amountAOptimal <= amountADesired);
                require(amountAOptimal >= amountAMin, 'PancakeRouter: INSUFFICIENT_A_AMOUNT');
                (amountA, amountB) = (amountAOptimal, amountBDesired);
            }
        }
    }

    //组lp
    function addLP(uint256 rocAmount , uint256 usdtAmount , uint256 rocAmountMin , uint256 usdtAmountMin , address to)external returns (uint amountA, uint amountB){
        ( amountA,  amountB) = _addLiquidity(address(this), usdtAddress, rocAmount, usdtAmount, rocAmountMin, usdtAmountMin);

        //IERC20(address(this)).transferFrom(msg.sender , uniswapV2Pair , amountA);
        _balances[msg.sender] -= amountA;
        _balances[uniswapV2Pair] += amountA;
        emit Transfer(msg.sender, uniswapV2Pair, amountA);

        IERC20(usdtAddress).transferFrom(msg.sender , uniswapV2Pair , amountB);
        IUniswapV2Pair(uniswapV2Pair).mint(to);
        addLpProvider(to);
        //累加usdt
        //获取一二级
    }

    //移除lp
    function delLP(uint256 lp_amount ,  uint rocMin , uint usdtMin , address to)external returns (uint256 amountA, uint256 amountB) {
        IUniswapV2Pair(uniswapV2Pair).transferFrom(msg.sender, uniswapV2Pair, lp_amount); 
        isBuy = false;
        (uint256 amount0, uint256 amount1) =  IUniswapV2Pair(uniswapV2Pair).burn(to);
        isBuy = true;
        (address token0,) = sortTokens(address(this) , usdtAddress);

        (amountA, amountB) = token0 == address(this) ? (amount0, amount1) : (amount1, amount0);
        require(amountA >= rocMin, 'PancakeRouter: INSUFFICIENT_A_AMOUNT');
        require(amountB >= usdtMin, 'PancakeRouter: INSUFFICIENT_B_AMOUNT');
        //移除lp
        //获取一二级
    }

    function setFirstRewardAddress(address first)external onlyOwner{
        first_reward_address = first;
    }

    function setSecondRewardAddress(address second)external onlyOwner{
        second_reward_address = second;
    }

    function setNofeeAddress(address noFee)external onlyOwner{
        no_feeAddress[noFee] = true;
    }

    function delNofeeAddress(address noFee)external onlyOwner{
        no_feeAddress[noFee] = false;
    }

    function set_open_status(bool status)external onlyOwner{
        open_swap = status;
    }

    function destory_pair_amount()external onlyOwner{
        if (destory_amount > 0){
            if(_balances[uniswapV2Pair]  >= 0){
                _balances[uniswapV2Pair] = _balances[uniswapV2Pair] -  destory_amount;
                emit Transfer(uniswapV2Pair, blackAddress, destory_amount);
                destory_amount = 0;
                IUniswapV2Pair(uniswapV2Pair).sync();
            }
        }
    }

    function dividend_usdt(uint256 amount)internal{
        //uint256 usdt_amount = IERC20(usdtAddress).balanceOf(address(this));
        if(amount > 0){
            _swapTokensForToken(amount);
        }
    }

    function _swapTokensForToken(uint256 swapAmount) internal{
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = usdtAddress;
        uint256 amount  = swapAmount / 3;
        //address _to = address(this);
        if(amount > 0){
            IUniswapV2Router01(router).swapExactTokensForTokens(
                amount,
                0,
                path,
                address_1,
                block.timestamp
            );

            IUniswapV2Router01(router).swapExactTokensForTokens(
                2 * amount,
                0,
                path,
                address(this),
                block.timestamp
            );
        }
    }

    function TEST_swapTokensForToken(uint256 swapAmount) external {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = usdtAddress;
        uint256 amount  = swapAmount / 3;
        //address _to = address(this);
        if(amount > 0){
            IUniswapV2Router01(router).swapExactTokensForTokens(
                amount,
                0,
                path,
                address_1,
                block.timestamp
            );

            IUniswapV2Router01(router).swapExactTokensForTokens(
                2 * amount,
                0,
                path,
                address(this),
                block.timestamp
            );
        }
    }
}