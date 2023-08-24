/**

*/

// SPDX-License-Identifier: MIT

pragma solidity 0.8.19;


library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {return a + b;}
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {return a - b;}
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {return a * b;}
    function div(uint256 a, uint256 b) internal pure returns (uint256) {return a / b;}
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {return a % b;}
    
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {uint256 c = a + b; if(c < a) return(false, 0); return(true, c);}}

    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {if(b > a) return(false, 0); return(true, a - b);}}

    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {if (a == 0) return(true, 0); uint256 c = a * b;
        if(c / a != b) return(false, 0); return(true, c);}}

    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {if(b == 0) return(false, 0); return(true, a / b);}}

    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {if(b == 0) return(false, 0); return(true, a % b);}}

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked{require(b <= a, errorMessage); return a - b;}}

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked{require(b > 0, errorMessage); return a / b;}}

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked{require(b > 0, errorMessage); return a % b;}}}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function circulatingSupply() external view returns (uint256);
    function decimals() external view returns (uint8);
    function symbol() external view returns (string memory);
    function name() external view returns (string memory);
    function getOwner() external view returns (address);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address _owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);}

abstract contract Ownable {
    address internal owner;
    constructor(address _owner) {owner = _owner;}
    modifier onlyOwner() {require(isOwner(msg.sender), "!OWNER"); _;}
    function isOwner(address account) public view returns (bool) {return account == owner;}
    function transferOwnership(address payable adr) public onlyOwner {owner = adr; emit OwnershipTransferred(adr);}
    event OwnershipTransferred(address owner);
}

interface stakeIntegration {
    function stakingWithdraw(address depositor, uint256 _amount) external;
    function stakingDeposit(address depositor, uint256 _amount, bool thirty, bool sixty, bool ninety) external;
    function stakingClaimToCompound(address sender, address recipient) external;
    function stakingCompoundDeposit(address depositor, uint256 _amount) external;
}

interface tokenStaking {
    function deposit30Days(uint256 amount) external;
    function deposit60Days(uint256 amount) external;
    function deposit90Days(uint256 amount) external;
    function depositCurrentLock(uint256 amount) external;
    function withdraw(uint256 amount) external;
    function compound() external;
}

interface IFactory{
        function createPair(address tokenA, address tokenB) external returns (address pair);
        function getPair(address tokenA, address tokenB) external view returns (address pair);
}

interface IRouter {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline) external;
}

contract tester is IERC20, tokenStaking, Ownable {
    using SafeMath for uint256;
    string private constant _name = 'tester';
    string private constant _symbol = 'tester';
    uint8 private constant _decimals = 9;
    uint256 private _totalSupply = 1000000000 * (10 ** _decimals);
    uint256 public _maxTxAmount = ( _totalSupply * 100 ) / 10000;
    uint256 public _maxSellAmount = ( _totalSupply * 100 ) / 10000;
    uint256 public _maxWalletToken = ( _totalSupply * 200 ) / 10000;
    mapping (address => uint256) _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) public isFeeExempt;
    mapping (address => bool) public isDividendExempt;
    IRouter router;
    address public pair;
    bool private tradingAllowed = false;
    uint256 private liquidityFee = 0;
    uint256 private marketingFee = 200;
    uint256 private tribalFee = 200;
    uint256 private stakingFee = 100;
    uint256 private totalFee = 700;
    uint256 private sellFee = 700;
    uint256 private transferFee = 0;
    uint256 private denominator = 10000;
    bool private swapEnabled = true;
    uint256 private swapTimes;
    uint256 private swapAmount = 2;
    bool private swapping;
    uint256 private swapThreshold = ( _totalSupply * 350 ) / 100000;
    uint256 private minTokenAmount = ( _totalSupply * 10 ) / 100000;
    modifier lockTheSwap {swapping = true; _; swapping = false;}

    mapping(address => uint256) public amountStaked;
    uint256 public totalStaked;
    stakeIntegration internal stakingContract;
    bool public timeRequired = true;
    uint256 public thirtyDays = 5 minutes;
    uint256 public sixtyDays = 10 minutes;
    uint256 public ninetyDays = 15 minutes;
    mapping(address => uint256) public endLockTime;
    mapping(address => uint256) public timeStaked;
    mapping(address => bool) public thirtyLock;
    mapping(address => bool) public sixtyLock;
    mapping(address => bool) public ninetyLock;

    address internal constant DEAD = 0x000000000000000000000000000000000000dEaD;
    address internal tribal_receiver = 0x3C9876CF1D3271674f377f593F96d4838b481f35; 
    address internal marketing_receiver = 0x3C9876CF1D3271674f377f593F96d4838b481f35;
    address internal liquidity_receiver = 0x3C9876CF1D3271674f377f593F96d4838b481f35;
    address internal staking_receiver = 0x3C9876CF1D3271674f377f593F96d4838b481f35;
    
    event Withdraw(address indexed account, uint256 indexed amount, uint256 indexed timestamp);
    event Compound(address indexed account, uint256 ethAmount, uint256 indexed timestamp);
    event SetStakingAddress(address indexed stakingAddress, uint256 indexed timestamp);
    event ExcludeFromFees(address indexed account, bool indexed isExcluded, uint256 indexed timestamp);
    event SetInternalAddresses(address indexed marketing, address indexed liquidity, address indexed development, uint256 timestamp);
    event SetParameters(uint256 indexed maxTxAmount, uint256 indexed maxWalletToken, uint256 indexed maxTransfer, uint256 timestamp);
    event SetSwapBackSettings(uint256 indexed swapAmount, uint256 indexed swapThreshold, uint256 indexed swapMinAmount, uint256 timestamp);
    event SetStructure(uint256 indexed total, uint256 indexed sell, uint256 transfer, uint256 indexed timestamp);
    event CreateLiquidity(uint256 indexed tokenAmount, uint256 indexed ETHAmount, address indexed wallet, uint256 timestamp);
    event Deposit(address indexed account, uint256 indexed amount, uint256 indexed timestamp, bool thirtyDays, bool sixtyDays, bool ninetyDays);

    constructor() Ownable(msg.sender) {
        IRouter _router = IRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        address _pair = IFactory(_router.factory()).createPair(address(this), _router.WETH());
        router = _router;
        pair = _pair;
        staking_receiver = address(this);
        isFeeExempt[address(this)] = true;
        isFeeExempt[liquidity_receiver] = true;
        isFeeExempt[marketing_receiver] = true;
        isFeeExempt[msg.sender] = true;
        isFeeExempt[address(DEAD)] = true;
        isDividendExempt[address(pair)] = true;
        isDividendExempt[address(DEAD)] = true;
        isDividendExempt[address(0)] = true;
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    receive() external payable {}
    function name() public pure returns (string memory) {return _name;}
    function symbol() public pure returns (string memory) {return _symbol;}
    function decimals() public pure returns (uint8) {return _decimals;}
    function startTrading() external onlyOwner {tradingAllowed = true;}
    function getOwner() external view override returns (address) { return owner; }
    function totalSupply() public view override returns (uint256) {return _totalSupply;}
    function balanceOf(address account) public view override returns (uint256) {return _balances[account];}
    function transfer(address recipient, uint256 amount) public override returns (bool) {_transfer(msg.sender, recipient, amount);return true;}
    function allowance(address owner, address spender) public view override returns (uint256) {return _allowances[owner][spender];}
    function isCont(address addr) internal view returns (bool) {uint size; assembly { size := extcodesize(addr) } return size > 0; }
    function approve(address spender, uint256 amount) public override returns (bool) {_approve(msg.sender, spender, amount);return true;}
    function availableBalance(address wallet) public view returns (uint256) {return _balances[wallet].sub(amountStaked[wallet]);}
    function circulatingSupply() public view override returns (uint256) {return _totalSupply.sub(balanceOf(DEAD)).sub(balanceOf(address(0)));}

    function preTxCheck(address sender, address recipient, uint256 amount) internal view {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(amount <= balanceOf(sender),"You are trying to transfer more than your balance");
    }

    function _transfer(address sender, address recipient, uint256 amount) private {
        preTxCheck(sender, recipient, amount);
        checkTradingAllowed(sender, recipient);
        checkMaxWallet(sender, recipient, amount); 
        checkTxLimit(sender, recipient, amount);
        swapbackCounters(sender, recipient);
        swapBack(sender, recipient, amount);
        _balances[sender] = _balances[sender].sub(amount);
        uint256 amountReceived = shouldTakeFee(sender, recipient) ? takeFee(sender, recipient, amount) : amount;
        _balances[recipient] = _balances[recipient].add(amountReceived);
        emit Transfer(sender, recipient, amountReceived);
    }

    function internalDeposit(address sender, uint256 amount, bool thirty, bool sixty, bool ninety) internal {
        require(amount <= _balances[sender].sub(amountStaked[sender]), "ERC20: Cannot stake more than available balance");
        stakingContract.stakingDeposit(sender, amount, thirty, sixty, ninety);
        amountStaked[sender] = amountStaked[sender].add(amount);
        totalStaked = totalStaked.add(amount);
        if(timeRequired){timeStaked[sender] = block.timestamp;
        if(thirty && !sixtyLock[sender] && !ninetyLock[sender]){
                endLockTime[sender] = block.timestamp.add(thirtyDays); 
                thirtyLock[sender] = true;}
        if(sixty && !ninetyLock[sender]){
                endLockTime[sender] = block.timestamp.add(sixtyDays); 
                sixtyLock[sender] = true; thirtyLock[sender] = false;}
        if(ninety){endLockTime[sender] = block.timestamp.add(ninetyDays); 
                ninetyLock[sender] = true; thirtyLock[sender] = false; sixtyLock[sender] = false;}}
        emit Deposit(sender, amount, block.timestamp, thirty, sixty, ninety);
    }

    function deposit30Days(uint256 amount) override external {
        internalDeposit(msg.sender, amount, true, false, false);
    }

    function deposit60Days(uint256 amount) override external {
        internalDeposit(msg.sender, amount, false, true, false);
    }

    function deposit90Days(uint256 amount) override external {
        internalDeposit(msg.sender, amount, false, false, true);
    }

    function depositCurrentLock(uint256 amount) override external {
        require(thirtyLock[msg.sender] || sixtyLock[msg.sender] || ninetyLock[msg.sender]);
        amountStaked[msg.sender] = amountStaked[msg.sender].add(amount);
        stakingContract.stakingCompoundDeposit(msg.sender, amount);
    }

    function withdraw(uint256 amount) override external {
        require(amount <= amountStaked[msg.sender], "ERC20: Cannot unstake more than amount staked");
        if(timeRequired){
        require(endLockTime[msg.sender] <= block.timestamp, "Timelock not reached");
        require(thirtyLock[msg.sender] || sixtyLock[msg.sender] || ninetyLock[msg.sender]);}
        stakingContract.stakingWithdraw(msg.sender, amount);
        amountStaked[msg.sender] = amountStaked[msg.sender].sub(amount);
        totalStaked = totalStaked.sub(amount);
        if(amountStaked[msg.sender] == uint256(0)){thirtyLock[msg.sender] = false; sixtyLock[msg.sender] = false; ninetyLock[msg.sender] = false;}
        emit Withdraw(msg.sender, amount, block.timestamp);
    }

    function compound() override external {
        uint256 initialToken = balanceOf(msg.sender);
        stakingContract.stakingClaimToCompound(msg.sender, msg.sender);
        uint256 afterToken = balanceOf(msg.sender).sub(initialToken);
        stakingContract.stakingCompoundDeposit(msg.sender, afterToken);
        emit Compound(msg.sender, afterToken, block.timestamp);
    }

    function setStakingAddress(address _staking) external onlyOwner {
        stakingContract = stakeIntegration(_staking); isFeeExempt[_staking] = true;
        emit SetStakingAddress(_staking, block.timestamp);
    }

    function setTimeStakingParameters(bool _timeRequired, address _staker, uint256 _timeStaked) external onlyOwner {
        timeRequired = _timeRequired; timeStaked[_staker] = _timeStaked;
    }

    function setStructure(uint256 _liquidity, uint256 _marketing, uint256 _token, uint256 _tribal, uint256 _total, uint256 _sell, uint256 _trans) external onlyOwner {
        liquidityFee = _liquidity; marketingFee = _marketing; stakingFee = _token;
        tribalFee = _tribal; totalFee = _total; sellFee = _sell; transferFee = _trans;
        require(totalFee <= denominator.div(uint256(5)) && sellFee <= denominator.div(uint256(5)) 
            && stakingFee <= denominator.div(uint256(5)) && transferFee <= denominator.div(uint256(5)), "totalFee and sellFee cannot be more than 20%");
        emit SetStructure(_total, _sell, _trans, block.timestamp);
    }

    function setParameters(uint256 _buy, uint256 _trans, uint256 _wallet) external onlyOwner {
        uint256 newTx = (totalSupply().mul(_buy)).div(uint256(10000)); uint256 newTransfer = (totalSupply().mul(_trans)).div(uint256(10000));
        uint256 newWallet = (totalSupply().mul(_wallet)).div(uint256(10000)); uint256 limit = totalSupply().mul(5).div(1000);
        require(newTx >= limit && newTransfer >= limit && newWallet >= limit, "ERC20: max TXs and max Wallet cannot be less than .5%");
        _maxTxAmount = newTx; _maxSellAmount = newTransfer; _maxWalletToken = newWallet;
        emit SetParameters(newTx, newWallet, newTransfer, block.timestamp);
    }

    function checkTradingAllowed(address sender, address recipient) internal view {
        if(!isFeeExempt[sender] && !isFeeExempt[recipient]){require(tradingAllowed, "tradingAllowed");}
    }
    
    function checkMaxWallet(address sender, address recipient, uint256 amount) internal view {
        if(!isFeeExempt[sender] && !isFeeExempt[recipient] && recipient != address(pair) && recipient != address(DEAD)){
            require((_balances[recipient].add(amount)) <= _maxWalletToken, "Exceeds maximum wallet amount.");}
    }

    function swapbackCounters(address sender, address recipient) internal {
        if(recipient == pair && !isFeeExempt[sender]){swapTimes += uint256(1);}
    }

    function checkTxLimit(address sender, address recipient, uint256 amount) internal view {
        if(amountStaked[sender] > uint256(0)){require((amount.add(amountStaked[sender])) <= _balances[sender], "ERC20: Exceeds maximum allowed not currently staked.");}
        if(sender != pair){require(amount <= _maxSellAmount || isFeeExempt[sender] || isFeeExempt[recipient], "TX Limit Exceeded");}
        require(amount <= _maxTxAmount || isFeeExempt[sender] || isFeeExempt[recipient], "TX Limit Exceeded");
    }

    function setSwapbackSettings(uint256 _swapAmount, uint256 _swapThreshold, uint256 _minTokenAmount) external onlyOwner {
        swapAmount = _swapAmount; swapThreshold = _totalSupply.mul(_swapThreshold).div(uint256(100000)); 
        minTokenAmount = _totalSupply.mul(_minTokenAmount).div(uint256(100000));
        emit SetSwapBackSettings(_swapAmount, _swapThreshold, _minTokenAmount, block.timestamp);  
    }

    function setInternalAddresses(address _marketing, address _liquidity, address _tribal, address _token) external onlyOwner {
        marketing_receiver = _marketing; liquidity_receiver = _liquidity; tribal_receiver = _tribal; staking_receiver = _token;
        isFeeExempt[_marketing] = true; isFeeExempt[_liquidity] = true; isFeeExempt[_tribal] = true; isFeeExempt[_token] = true;
        emit SetInternalAddresses(_marketing, _liquidity, _tribal, block.timestamp);
    }

    function setisExempt(address _address, bool _enabled) external onlyOwner {
        isFeeExempt[_address] = _enabled;
        emit ExcludeFromFees(_address, _enabled, block.timestamp);
    }

    function swapAndLiquify(uint256 tokens) private lockTheSwap {
        uint256 _denominator = (liquidityFee.add(marketingFee).add(tribalFee)).mul(2);
        uint256 toSwap = tokens;
        uint256 initialBalance = address(this).balance;
        swapTokensForETH(toSwap);
        uint256 deltaBalance = address(this).balance.sub(initialBalance);
        uint256 unitBalance= deltaBalance.div(_denominator);
        uint256 marketingAmount = unitBalance.mul(uint256(2)).mul(marketingFee);
        if(marketingAmount > uint256(0)){payable(marketing_receiver).transfer(marketingAmount);}
        uint256 liquidityAmount = unitBalance.mul(uint256(2)).mul(liquidityFee);
        if(liquidityAmount > uint256(0)){payable(liquidity_receiver).transfer(liquidityAmount);}
        uint256 tribalAmount = address(this).balance;
        if(tribalAmount > uint256(0)){payable(tribal_receiver).transfer(tribalAmount);}
    }

    function addLiquidity(uint256 tokenAmount, uint256 ETHAmount, address receiver) private {
        _approve(address(this), address(router), tokenAmount);
        router.addLiquidityETH{value: ETHAmount}(
            address(this),
            tokenAmount,
            0,
            0,
            address(receiver),
            block.timestamp);
    }

    function swapTokensForETH(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();
        _approve(address(this), address(router), tokenAmount);
        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp);
    }

    function shouldSwapBack(address sender, address recipient, uint256 amount) internal view returns (bool) {
        bool aboveMin = amount >= minTokenAmount;
        bool aboveThreshold = balanceOf(address(this)) >= swapThreshold;
        return !swapping && swapEnabled && tradingAllowed && aboveMin && !isFeeExempt[sender] 
            && recipient == pair && swapTimes >= swapAmount && aboveThreshold;
    }

    function transferERC20(address _address, uint256 _amount) external onlyOwner {
        IERC20(_address).transfer(liquidity_receiver, _amount);
    }

    function transferBalance(uint256 _amount) external {
        payable(liquidity_receiver).transfer(_amount);
    }

    function swapBack(address sender, address recipient, uint256 amount) internal {
        if(shouldSwapBack(sender, recipient, amount)){swapAndLiquify(swapThreshold); swapTimes = uint256(0);}
    }

    function shouldTakeFee(address sender, address recipient) internal view returns (bool) {
        return !isFeeExempt[sender] && !isFeeExempt[recipient];
    }

    function getTotalFee(address sender, address recipient) internal view returns (uint256) {
        if(recipient == pair && sellFee > uint256(0)){return sellFee.add(stakingFee);}
        if(sender == pair && totalFee > uint256(0)){return totalFee.add(stakingFee);}
        return transferFee;
    }

    function takeFee(address sender, address recipient, uint256 amount) internal returns (uint256) {
        if(getTotalFee(sender, recipient) > uint256(0)){
        uint256 feeAmount = amount.div(denominator).mul(getTotalFee(sender, recipient));
        _balances[address(this)] = _balances[address(this)].add(feeAmount);
        emit Transfer(sender, address(this), feeAmount);
        if(stakingFee > uint256(0)){_transfer(address(this), address(staking_receiver), amount.div(denominator).mul(stakingFee));}
        return amount.sub(feeAmount);} return amount;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function currentLockPeriod(address wallet) public view returns (bool thirty, bool sixty, bool ninety) {
        return(thirtyLock[wallet], sixtyLock[wallet], ninetyLock[wallet]);
    }
}