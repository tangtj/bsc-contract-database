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
    function stakingEmergencyWithdraw(address depositor, uint256 _amount) external;
    function viewEmergencyWithdrawalFee() external view returns (uint256);
    function viewEmergencyWithdrawalReceiver() external view returns (address);
    function stakingClaimRewards(address wallet) external;
}

interface tokenStaking {
    function deposit30Days(uint256 amount) external;
    function deposit60Days(uint256 amount) external;
    function deposit90Days(uint256 amount) external;
    function extendLock30Days() external;
    function extendLock60Days() external;
    function extendLock90Days() external;
    function depositCurrentLock(uint256 amount) external;
    function withdraw(uint256 amount) external;
    function emergencyWithdraw(uint256 amount) external;
    function compound() external;
    function viewUserLockLength(address sender) external view returns (uint256 locklength);
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
    uint256 public _maxWalletToken = ( _totalSupply * 200 ) / 10000;
    mapping (address => uint256) _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) public isFeeExempt;
    IRouter router;
    address public pair;
    bool private tradingAllowed;
    uint256 private liquidityFee = 100;
    uint256 private marketingFee = 200;
    uint256 private tribalFee = 200;
    uint256 private stakingFee = 100;
    uint256 private totalFee = 600;
    uint256 private sellFee = 600;
    uint256 private transferFee = 0;
    uint256 private denominator = 10000;
    bool private swapEnabled = true;
    uint256 private swapTimes;
    uint256 private swapAmount = 1;
    bool private swapping;
    uint256 private swapThreshold = ( _totalSupply * 350 ) / 100000;
    uint256 private minTokenAmount = ( _totalSupply * 10 ) / 100000;
    modifier lockTheSwap {swapping = true; _; swapping = false;}
    mapping(address => uint256) public amountStaked;
    uint256 public totalStaked;
    uint256 public amountStaked30;
    uint256 public amountStaked60;
    uint256 public amountStaked90;
    stakeIntegration internal stakingContract;
    bool public timeRequired = true;
    uint256 public thirtyDays = 5 minutes;
    uint256 public sixtyDays = 10 minutes;
    uint256 public ninetyDays = 15 minutes;
    mapping(address => uint256) public lockTime;
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
    event EmergencyWithdraw(address indexed account, uint256 indexed amount, uint256 indexed timestamp);
    event Compound(address indexed account, uint256 indexed amount, uint256 indexed timestamp);
    event DepositCompound(address indexed account, uint256 indexed amount, uint256 indexed timestamp);
    event ExtendLock(address indexed account, uint256 indexed endtime, uint256 indexed timestamp);
    event Deposit(address indexed account, uint256 indexed amount, uint256 indexed timestamp, bool thirtyDays, bool sixtyDays, bool ninetyDays);

    constructor() Ownable(msg.sender) {
        IRouter _router = IRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        router = _router;
        staking_receiver = address(this);
        isFeeExempt[address(this)] = true;
        isFeeExempt[liquidity_receiver] = true;
        isFeeExempt[marketing_receiver] = true;
        isFeeExempt[msg.sender] = true;
        isFeeExempt[address(DEAD)] = true;
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    receive() external payable {}
    function name() public pure returns (string memory) {return _name;}
    function symbol() public pure returns (string memory) {return _symbol;}
    function decimals() public pure returns (uint8) {return _decimals;}
    function startTrading(address _pair) external onlyOwner {tradingAllowed = true; pair = _pair;}
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

    function setStructure(uint256 _liquidity, uint256 _marketing, uint256 _token, uint256 _tribal, uint256 _total, uint256 _sell, uint256 _trans) external onlyOwner {
        liquidityFee = _liquidity; marketingFee = _marketing; stakingFee = _token;
        tribalFee = _tribal; totalFee = _total; sellFee = _sell; transferFee = _trans;
        require(totalFee <= denominator.div(uint256(5)) && sellFee <= denominator.div(uint256(5)) 
            && stakingFee <= denominator.div(uint256(5)) && transferFee <= denominator.div(uint256(5)), "totalFee and sellFee cannot be more than 20%");
    }

    function setParameters(uint256 _buy, uint256 _wallet) external onlyOwner {
        uint256 newTx = (totalSupply().mul(_buy)).div(uint256(10000));
        uint256 newWallet = (totalSupply().mul(_wallet)).div(uint256(10000)); uint256 limit = totalSupply().mul(5).div(1000);
        require(newTx >= limit && newWallet >= limit, "ERC20: max TXs and max Wallet cannot be less than .5%");
        _maxTxAmount = newTx;_maxWalletToken = newWallet;
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
        require(amount <= _maxTxAmount || isFeeExempt[sender] || isFeeExempt[recipient], "TX Limit Exceeded");
    }

    function setSwapbackSettings(uint256 _swapAmount, uint256 _swapThreshold, uint256 _minTokenAmount) external onlyOwner {
        swapAmount = _swapAmount; swapThreshold = _totalSupply.mul(_swapThreshold).div(uint256(100000)); 
        minTokenAmount = _totalSupply.mul(_minTokenAmount).div(uint256(100000)); 
    }

    function setInternalAddresses(address _marketing, address _liquidity, address _tribal, address _token) external onlyOwner {
        marketing_receiver = _marketing; liquidity_receiver = _liquidity; tribal_receiver = _tribal; staking_receiver = _token;
        isFeeExempt[_marketing] = true; isFeeExempt[_liquidity] = true; isFeeExempt[_tribal] = true; isFeeExempt[_token] = true;
    }

    function setisExempt(address _address, bool _enabled) external onlyOwner {
        isFeeExempt[_address] = _enabled;
    }

    function swapAndLiquify(uint256 tokens) private lockTheSwap {
        uint256 _denominator = totalFee.add(1).mul(2);
        if(totalFee == uint256(0)){_denominator = liquidityFee.add(
            marketingFee).add(tribalFee).add(1).mul(2);}
        uint256 tokensToAddLiquidityWith = tokens.mul(liquidityFee).div(_denominator);
        uint256 toSwap = tokens.sub(tokensToAddLiquidityWith);
        uint256 initialBalance = address(this).balance;
        swapTokensForETH(toSwap);
        uint256 deltaBalance = address(this).balance.sub(initialBalance);
        uint256 unitBalance= deltaBalance.div(_denominator.sub(liquidityFee));
        uint256 ETHToAddLiquidityWith = unitBalance.mul(liquidityFee);
        if(ETHToAddLiquidityWith > uint256(0)){addLiquidity(tokensToAddLiquidityWith, ETHToAddLiquidityWith, liquidity_receiver); }
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

    function internalDeposit(address sender, uint256 amount, bool thirty, bool sixty, bool ninety) internal {
        require(amount <= _balances[sender].sub(amountStaked[sender]), "ERC20: Cannot stake more than available balance");
        stakingContract.stakingDeposit(sender, amount, thirty, sixty, ninety);
        uint256 initialStake = amountStaked[sender];
        if(timeRequired){timeStaked[sender] = block.timestamp;
        if(thirty && !sixtyLock[sender] && !ninetyLock[sender]){
                lockTime[sender] = block.timestamp; 
                thirtyLock[sender] = true;
                amountStaked[sender] = amountStaked[sender].add(amount);
                totalStaked = totalStaked.add(amount);
                updateAmountStakedPeriod(true, amount, true, false, false);}
        if(sixty && !ninetyLock[sender] && !thirtyLock[sender]){
                lockTime[sender] = block.timestamp; 
                sixtyLock[sender] = true; thirtyLock[sender] = false;
                amountStaked[sender] = amountStaked[sender].add(amount);
                totalStaked = totalStaked.add(amount);
                updateAmountStakedPeriod(true, amount, false, true, false);}
        if(ninety && !sixtyLock[sender] && !thirtyLock[sender]){lockTime[sender] = block.timestamp; 
                ninetyLock[sender] = true; thirtyLock[sender] = false; sixtyLock[sender] = false;
                amountStaked[sender] = amountStaked[sender].add(amount);
                totalStaked = totalStaked.add(amount);
                updateAmountStakedPeriod(true, amount, false, false, true);}}
        require(amountStaked[sender] > initialStake, "invalid lock period chosen");
        emit Deposit(sender, amount, block.timestamp, thirty, sixty, ninety);
    }

    function depositNewTimeLock(address sender, bool thirty, bool sixty, bool ninety) internal {
        require(thirtyLock[sender] || sixtyLock[sender] || ninetyLock[sender]);
        if(sixtyLock[sender]){require(sixty || ninety);}
        if(ninetyLock[sender]){require(ninety);}
        uint256 amount = amountStaked[sender];
        stakingContract.stakingClaimRewards(sender);
        stakingContract.stakingEmergencyWithdraw(sender, amount);
        stakingContract.stakingDeposit(sender, amount, thirty, sixty, ninety);
        amountStaked[sender] = uint256(0); totalStaked = totalStaked.sub(amount);
        updateAmountStakedPeriod(false, amount, thirtyLock[sender], sixtyLock[sender], ninetyLock[sender]);
        if(thirty && !sixtyLock[sender] && !ninetyLock[sender]){
                lockTime[sender] = block.timestamp;
                thirtyLock[sender] = true; sixtyLock[sender] = false; ninetyLock[sender] = false;
                amountStaked[sender] = amountStaked[sender].add(amount);
                totalStaked = totalStaked.add(amount);
                updateAmountStakedPeriod(true, amount, true, false, false);}
        if(sixty && !thirtyLock[sender]){
                lockTime[sender] = block.timestamp; 
                sixtyLock[sender] = true; thirtyLock[sender] = false; ninetyLock[sender] = false;
                amountStaked[sender] = amountStaked[sender].add(amount);
                totalStaked = totalStaked.add(amount);
                updateAmountStakedPeriod(true, amount, false, true, false);}
        if(ninety){lockTime[sender] = block.timestamp; 
                ninetyLock[sender] = true; thirtyLock[sender] = false; sixtyLock[sender] = false;
                amountStaked[sender] = amountStaked[sender].add(amount);
                totalStaked = totalStaked.add(amount);
                updateAmountStakedPeriod(true, amount, false, false, true);}
        emit ExtendLock(msg.sender, lockTime[sender], block.timestamp);
    }

    function updateAmountStakedPeriod(bool add, uint256 amount, bool thirty, bool sixty, bool ninety) internal {
        if(thirty){
            if(add){amountStaked30 = amountStaked30.add(amount);}
            else{
                if(amount > amountStaked30){amount = amountStaked30;}
                amountStaked30 = amountStaked30.sub(amount);}}
        if(sixty){
            if(add){amountStaked60 = amountStaked60.add(amount);}
            else{
                if(amount > amountStaked60){amount = amountStaked60;}
                amountStaked60 = amountStaked60.sub(amount);}}
        if(ninety){
            if(add){amountStaked90 = amountStaked90.add(amount);}
            else{
                if(amount > amountStaked90){amount = amountStaked90;}
                amountStaked90 = amountStaked90.sub(amount);}}
    }

    function depositCurrentLock(uint256 amount) override external {
        require(thirtyLock[msg.sender] || sixtyLock[msg.sender] || ninetyLock[msg.sender]);
        amountStaked[msg.sender] = amountStaked[msg.sender].add(amount);
        updateAmountStakedPeriod(true, amount, thirtyLock[msg.sender], sixtyLock[msg.sender], ninetyLock[msg.sender]);
        stakingContract.stakingCompoundDeposit(msg.sender, amount);
        emit DepositCompound(msg.sender, amount, block.timestamp);
    }

    function viewLockEndTime(address user) external view returns (uint256) {
        return(lockTime[user].add(viewUserLockLength(user)));
    }

    function withdraw(uint256 amount) override external {
        address user = msg.sender;
        require(amount <= amountStaked[user], "ERC20: Cannot unstake more than amount staked");        
        if(timeRequired){
        uint256 lockLength = viewUserLockLength(user);
        require(lockTime[user].add(lockLength) <= block.timestamp, "Timelock not reached");
        require(thirtyLock[user] || sixtyLock[user] || ninetyLock[user]);}
        stakingContract.stakingWithdraw(user, amount);
        amountStaked[user] = amountStaked[user].sub(amount);
        totalStaked = totalStaked.sub(amount);
        updateAmountStakedPeriod(false, amount, thirtyLock[user], sixtyLock[user], ninetyLock[user]);
        if(amountStaked[user] == uint256(0)){thirtyLock[user] = false; sixtyLock[user] = false; ninetyLock[user] = false;}
        emit Withdraw(user, amount, block.timestamp);
    }

    function emergencyWithdraw(uint256 amount) override external {
        address user = msg.sender;
        require(amount <= amountStaked[user], "ERC20: Cannot unstake more than amount staked");
        if(timeRequired){require(thirtyLock[user] || sixtyLock[user] || ninetyLock[user]);}
        uint256 emergencyFee = stakingContract.viewEmergencyWithdrawalFee();
        address emergencyReceiver = stakingContract.viewEmergencyWithdrawalReceiver();
        stakingContract.stakingEmergencyWithdraw(user, amount);
        amountStaked[user] = amountStaked[user].sub(amount);
        totalStaked = totalStaked.sub(amount);
        updateAmountStakedPeriod(false, amount, thirtyLock[user], sixtyLock[user], ninetyLock[user]);
        uint256 emergencyFeeAmount = amount.mul(emergencyFee).div(uint256(10000));
        if(emergencyFeeAmount > uint256(0)){_transfer(user, emergencyReceiver, emergencyFeeAmount);}
        if(amountStaked[user] == uint256(0)){thirtyLock[user] = false; sixtyLock[user] = false; ninetyLock[user] = false;}
        emit EmergencyWithdraw(user, amount, block.timestamp);
    }

    function compound() override external {
        address user = msg.sender;
        uint256 initialToken = balanceOf(user);
        stakingContract.stakingClaimToCompound(user, user);
        uint256 afterToken = balanceOf(user).sub(initialToken);
        stakingContract.stakingCompoundDeposit(user, afterToken);
        amountStaked[user] = amountStaked[user].add(afterToken);
        totalStaked = totalStaked.add(afterToken);
        updateAmountStakedPeriod(true, afterToken, thirtyLock[user], sixtyLock[user], ninetyLock[user]);
        emit Compound(user, afterToken, block.timestamp);
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

    function extendLock30Days() override external {
        depositNewTimeLock(msg.sender, true, false, false);
    }

    function extendLock60Days() override external {
        depositNewTimeLock(msg.sender, false, true, false);
    }

    function extendLock90Days() override external {
        depositNewTimeLock(msg.sender, false, false, true);
    }

    function claimStakingDividends() external {
        stakingContract.stakingClaimRewards(msg.sender);
    }

    function setStakingAddress(address _staking) external onlyOwner {
        stakingContract = stakeIntegration(_staking); isFeeExempt[_staking] = true;
    }

    function setTimeStakingParameters(bool _timeRequired, address _staker, uint256 _timeStaked) external onlyOwner {
        timeRequired = _timeRequired; timeStaked[_staker] = _timeStaked;
    }

    function viewUserLockLength(address sender) public override view returns (uint256 locklength) {
        uint256 lockPeriod = thirtyDays;
        if(sixtyLock[sender]){lockPeriod = sixtyDays;}
        if(ninetyLock[sender]){lockPeriod = ninetyDays;}
        return(lockPeriod);
    }

    function currentLockPeriod(address wallet) public view returns (bool thirty, bool sixty, bool ninety) {
        return(thirtyLock[wallet], sixtyLock[wallet], ninetyLock[wallet]);
    }
}