pragma solidity ^0.8.16;

//SPDX-License-Identifier: MIT

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
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
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

abstract contract Auth {
    address internal owner;
    mapping (address => bool) internal authorizations;

    constructor(address _owner) {
        owner = _owner;
        authorizations[_owner] = true;
    }

    modifier onlyOwner() {
        require(isOwner(msg.sender), "!OWNER"); _;
    }

    modifier authorized() {
        require(isAuthorized(msg.sender), "!AUTHORIZED"); _;
    }

    function authorize(address adr) public onlyOwner {
        authorizations[adr] = true;
    }

    function unauthorize(address adr) public onlyOwner {
        authorizations[adr] = false;
    }

    function isOwner(address account) public view returns (bool) {
        return account == owner;
    }

    function isAuthorized(address adr) public view returns (bool) {
        return authorizations[adr];
    }

    function transferOwnership(address payable adr) public onlyOwner {
        owner = adr;
        authorizations[adr] = true;
        emit OwnershipTransferred(adr);
    }

    event OwnershipTransferred(address owner);
}

interface IDEXFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IDEXRouter {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

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

    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}
interface IDividendDistributor {
    function setDistributionCriteria(uint256 _minPeriod, uint256 _minDistribution) external;
    function setShare(address shareholder, uint256 amount) external;
    function deposit() external payable;
    function process(uint256 gas) external;
}

contract DividendDistributor is IDividendDistributor {

    using SafeMath for uint256;
    address _token;

    struct Share {
        uint256 amount;
        uint256 totalExcluded;  
        uint256 totalRealised;
    }

    IDEXRouter router;
    address routerAddress = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    IERC20 RewardToken;

    address[] shareholders;
    mapping (address => uint256) shareholderIndexes;
    mapping (address => uint256) shareholderClaims;
    mapping (address => Share) public shares;

    uint256 public totalShares;
    uint256 public totalDividends;
    uint256 public totalDistributed;
    uint256 public dividendsPerShare;
    uint256 public dividendsPerShareAccuracyFactor = 10 ** 36;

    uint256 public minPeriod = 60 minutes;
    uint256 public minDistribution = 1 * (10 ** 24);

    uint256 currentIndex;

    bool initialized;
    modifier initialization() {
        require(!initialized);
        _;
        initialized = true;
    }

    modifier onlyToken() {
        require(msg.sender == _token || Auth(_token).isAuthorized(msg.sender)); _;
    }

    constructor (address _router, address _rewardToken) {
        router = _router != address(0) ? IDEXRouter(_router) : IDEXRouter(routerAddress);
        _token = msg.sender;
        RewardToken = IERC20(_rewardToken);
    }

    function setDistributionCriteria(uint256 newMinPeriod, uint256 newMinDistribution) external override onlyToken {
        minPeriod = newMinPeriod;
        minDistribution = newMinDistribution;
    }

    function setShare(address shareholder, uint256 amount) external override onlyToken {

        if(shares[shareholder].amount > 0){
            distributeDividend(shareholder);
        }

        if(amount > 0 && shares[shareholder].amount == 0){
            addShareholder(shareholder);
        }else if(amount == 0 && shares[shareholder].amount > 0){
            removeShareholder(shareholder);
        }

        totalShares = totalShares.sub(shares[shareholder].amount).add(amount);
        shares[shareholder].amount = amount;
        shares[shareholder].totalExcluded = getCumulativeDividends(shares[shareholder].amount);
    }

    function deposit() external payable override onlyToken {

        uint256 balanceBefore = RewardToken.balanceOf(address(this));

        address[] memory path = new address[](2);
        path[0] = router.WETH();
        path[1] = address(RewardToken);

        router.swapExactETHForTokensSupportingFeeOnTransferTokens{value: msg.value}(
            0,
            path,
            address(this),
            block.timestamp
        );

        uint256 amount = RewardToken.balanceOf(address(this)).sub(balanceBefore);
        totalDividends = totalDividends.add(amount);
        dividendsPerShare = dividendsPerShare.add(dividendsPerShareAccuracyFactor.mul(amount).div(totalShares));
    }

    function process(uint256 gas) external override onlyToken {
        uint256 shareholderCount = shareholders.length;

        if(shareholderCount == 0) { return; }

        uint256 iterations = 0;
        uint256 gasUsed = 0;
        uint256 gasLeft = gasleft();

        while(gasUsed < gas && iterations < shareholderCount) {

            if(currentIndex >= shareholderCount){ currentIndex = 0; }

            if(shouldDistribute(shareholders[currentIndex])){
                distributeDividend(shareholders[currentIndex]);
            }

            gasUsed = gasUsed.add(gasLeft.sub(gasleft()));
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }
    }
    
    function shouldDistribute(address shareholder) internal view returns (bool) {
        return shareholderClaims[shareholder] + minPeriod < block.timestamp
                && getUnpaidEarnings(shareholder) > minDistribution;
    }

    function distributeDividend(address shareholder) internal {
        if(shares[shareholder].amount == 0){ return; }

        uint256 amount = getUnpaidEarnings(shareholder);
        if(amount > 0){
            totalDistributed = totalDistributed.add(amount);
            RewardToken.transfer(shareholder, amount);
            shareholderClaims[shareholder] = block.timestamp;
            shares[shareholder].totalRealised = shares[shareholder].totalRealised.add(amount);
            shares[shareholder].totalExcluded = getCumulativeDividends(shares[shareholder].amount);
        }

    }
    
    function claimDividend(address shareholder) external onlyToken{
        distributeDividend(shareholder);
    }
    
    function rescueDividends(address to) external onlyToken {
        RewardToken.transfer(to, RewardToken.balanceOf(address(this)));
    }
    
    function setRewardToken(address _rewardToken) external onlyToken{
        RewardToken = IERC20(_rewardToken);
    }

    function getUnpaidEarnings(address shareholder) public view returns (uint256) {
        if(shares[shareholder].amount == 0){ return 0; }

        uint256 shareholderTotalDividends = getCumulativeDividends(shares[shareholder].amount);
        uint256 shareholderTotalExcluded = shares[shareholder].totalExcluded;

        if(shareholderTotalDividends <= shareholderTotalExcluded){ return 0; }

        return shareholderTotalDividends.sub(shareholderTotalExcluded);
    }

    function getCumulativeDividends(uint256 share) internal view returns (uint256) {
        return share.mul(dividendsPerShare).div(dividendsPerShareAccuracyFactor);
    }

    function addShareholder(address shareholder) internal {
        shareholderIndexes[shareholder] = shareholders.length;
        shareholders.push(shareholder);
    }

    function removeShareholder(address shareholder) internal {
        shareholders[shareholderIndexes[shareholder]] = shareholders[shareholders.length-1];
        shareholderIndexes[shareholders[shareholders.length-1]] = shareholderIndexes[shareholder];
        shareholders.pop();
    }
    
}

contract TemplateRewards is IERC20, Auth {
    using SafeMath for uint256;

    string _name;
    string _symbol;
    uint8 constant _decimals = 9;

    address private constant DEAD = 0x000000000000000000000000000000000000dEaD;
    address private constant ZERO = 0x0000000000000000000000000000000000000000;
    address WETH;
    address adminFeeWallet = 0x769bFF707502941c5540cED416Dc884D0383f2c3;
    address public RewardToken;

    uint256 _totalSupply; 
    
    uint256 public _maxTxAmount;
    uint256 public _maxWalletToken;
    
    bool public restrictWhales = true;

    mapping (address => uint256) public _balances;
    mapping (address => mapping (address => uint256)) public _allowances;

    mapping (address => bool) public isFeeExempt;
    mapping (address => bool) public isTxLimitExempt;
    mapping (address => bool) public isDividendExempt;
    mapping (address => bool) private _isBlacklisted;

    uint256 marketingBuyFee;
    uint256 liquidityBuyFee;
    uint256 devBuyFee;
    uint256 rewardsBuyFee;
    uint256 public totalBuyFee;

    uint256 marketingSellFee;
    uint256 liquiditySellFee;
    uint256 devSellFee;
    uint256 rewardsSellFee;
    uint256 public totalSellFee;

    uint256 adminFee;
    uint256 totalAdminFee;

    address public liquidityWallet;
    address public marketingWallet;
    address public devWallet;
    address private referralWallet;

    uint256 marketingFee;
    uint256 liquidityFee;
    uint256 devFee;
    uint256 rewardsFee;
    uint256 totalFee;

    IDEXRouter public router;
    address public pair;

    bool public tradingOpen = false;

    DividendDistributor public dividendDistributor;
    uint256 distributorGas = 750000;

    bool private inSwapAndLiquify;
    bool public swapAndLiquifyEnabled = true;
    bool public swapAndLiquifyByLimitOnly = false;

    uint256 public swapThreshold;

    bool public takeBuyFee = true;
    bool public takeSellFee = true;
    bool public takeTransferFee = true;

    event maxWalletChanged(uint256 amountBase1000);
    event maxTxChanged(uint256 amountBase1000);
    event tradingOpened();
    event feeExemptStatusChanged(address target, bool status);
    event limitExemptStatusChanged(address target, bool status);
    event dividendExemptStatusChanged(address target, bool status);
    event buyFeesChanged(uint256 newBuy, uint256 newSell, uint256 newLP, uint256 newMkt, uint256 newDev, uint256 newReward);
    event sellFeesChanged(uint256 newBuy, uint256 newSell, uint256 newLP, uint256 newMkt, uint256 newDev, uint256 newReward);
    event feeReceiversChanged(address lpReceiver, address marketingReceiver, address devReceiver);
    event swapBackSettingChanged(bool enabled, uint256 limit, bool byLimitOnly);
    event distributionCriteriaChanged(uint256 newPeriod, uint256 newAmount);
    event rewardTokenAddressChanged(address newRewardToken);
    event AutoLiquify(uint256 amountSHIB, uint256 amountTok);
    
    modifier lockTheSwap {
        inSwapAndLiquify = true;
        _;
        inSwapAndLiquify = false;
    }

    constructor (uint[] memory numbers, address[] memory addresses, string[] memory names) Auth(msg.sender) {
        
        router = router = IDEXRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        WETH = router.WETH();
        pair = IDEXFactory(router.factory()).createPair(WETH, address(this));
        _allowances[address(this)][address(router)] = type(uint256).max;
        RewardToken = addresses[5];
        dividendDistributor = new DividendDistributor(address(router), RewardToken);

        transferOwnership(payable(addresses[0]));

        _name = names[0];
        _symbol = names[1];
        _totalSupply = numbers[0] * (10 ** _decimals);
        
        isFeeExempt[addresses[0]] = true;
        isFeeExempt[address(this)] = true;
        isFeeExempt[DEAD] = true;

        isTxLimitExempt[addresses[0]] = true;
        isTxLimitExempt[pair] = true;

        isDividendExempt[pair] = true;
        isDividendExempt[addresses[0]] = true;
        isDividendExempt[address(this)] = true;
        isDividendExempt[DEAD] = true;
        isDividendExempt[ZERO] = true;

        marketingWallet = addresses[1];
        devWallet = addresses[2];
        liquidityWallet = addresses[3];
        referralWallet = addresses[4];

        isFeeExempt[marketingWallet] = true;
        isTxLimitExempt[marketingWallet] = true;

        swapThreshold = _totalSupply.mul(10).div(10000);
        
        marketingBuyFee = numbers[1];
        liquidityBuyFee = numbers[3];
        devBuyFee = numbers[5];
        rewardsBuyFee = numbers[7];
        adminFee = 25;

        totalBuyFee = totalBuyFee = marketingBuyFee.add(liquidityBuyFee).add(rewardsBuyFee).add(devBuyFee).add(adminFee);
        require(totalBuyFee <= 3000, "Buy tax too high!"); //30% buy tax

        marketingSellFee = numbers[2];
        liquiditySellFee = numbers[4];
        devSellFee = numbers[6];
        rewardsSellFee = numbers[8];
        
        totalSellFee = totalSellFee = marketingSellFee.add(liquiditySellFee).add(rewardsSellFee).add(devSellFee).add(adminFee);
        require(totalSellFee <= 3000, "Sell tax too high!"); //30% sell tax

        marketingFee = marketingBuyFee.add(marketingSellFee);
        liquidityFee = liquidityBuyFee.add(liquiditySellFee);
        devFee = devBuyFee.add(devSellFee);
        rewardsFee = rewardsBuyFee.add(rewardsSellFee);
        totalAdminFee = adminFee * 2;

        totalFee = liquidityFee.add(marketingFee).add(devFee).add(rewardsFee).add(totalAdminFee);

        _maxTxAmount = ( _totalSupply * numbers[9] ) / 1000;
        require(numbers[9] >= 5,"Max txn too low!"); //0.5% max txn
        _maxWalletToken = ( _totalSupply * numbers[10] ) / 1000;
        require(numbers[10] >= 5,"Max wallet too low!"); //0.5% max wallet

        _balances[addresses[0]] = _totalSupply;
        emit Transfer(address(0), addresses[0], _totalSupply);
    }

    receive() external payable { }

    function name() external view override returns (string memory) { return _name; }
    function symbol() external view override returns (string memory) { return _symbol; }
    function decimals() external pure override returns (uint8) { return _decimals; }
    function totalSupply() external view override returns (uint256) { return _totalSupply; }
    function getOwner() external view override returns (address) { return owner; }

    function getCirculatingSupply() public view returns (uint256) {
        return _totalSupply.sub(balanceOf(DEAD)).sub(balanceOf(ZERO));
    }

    function balanceOf(address account) public view override returns (uint256) { return _balances[account]; }
    function allowance(address holder, address spender) external view override returns (uint256) { return _allowances[holder][spender]; }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function approveMax(address spender) external returns (bool) {
        return approve(spender, type(uint256).max);
    }
    
    function claimDividend() external {
        dividendDistributor.claimDividend(msg.sender);
    }

    function checkTxLimit(address sender, uint256 amount) internal view {
        require(amount <= _maxTxAmount || isTxLimitExempt[sender], "TX Limit Exceeded");
    }

    function transfer(address recipient, uint256 amount) external override returns (bool) {
        return _transferFrom(msg.sender, recipient, amount);
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        
        if(_allowances[sender][msg.sender] != type(uint256).max){
            _allowances[sender][msg.sender] = _allowances[sender][msg.sender].sub(amount, "Insufficient Allowance");
        }
        return _transferFrom(sender, recipient, amount);
    }

    function _transferFrom(address sender, address recipient, uint256 amount) internal returns (bool) {
        
        if(inSwapAndLiquify){ return _basicTransfer(sender, recipient, amount); }

        if(!authorizations[sender] && !authorizations[recipient]){
            require(tradingOpen, "Trading not open yet");
        }

        require(amount <= _maxTxAmount || isTxLimitExempt[sender], "TX Limit Exceeded");

        if(msg.sender != pair && !inSwapAndLiquify && swapAndLiquifyEnabled && _balances[address(this)] >= swapThreshold){
             swapBack(); 
        }
        
        //Exchange tokens
        _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");
        
        if(!isTxLimitExempt[recipient] && restrictWhales)
        {
            require(_balances[recipient].add(amount) <= _maxWalletToken);
        }

        uint256 finalAmount = !isFeeExempt[sender] && !isFeeExempt[recipient] ? takeFee(sender, recipient, amount) : amount;
        _balances[recipient] = _balances[recipient].add(finalAmount);

        // Dividend tracker
        if(!isDividendExempt[sender]) {
            try dividendDistributor.setShare(sender, _balances[sender]) {} catch {}
        }

        if(!isDividendExempt[recipient]) {
            try dividendDistributor.setShare(recipient, _balances[recipient]) {} catch {} 
        }

        try dividendDistributor.process(distributorGas) {} catch {}

        emit Transfer(sender, recipient, finalAmount);
        return true;
    }
    
    function _basicTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
        _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function takeFee(address sender, address recipient, uint256 amount) internal returns (uint256) {
        uint feeApplicable = 0;
        if (recipient == pair && takeSellFee) {
            feeApplicable = totalSellFee;        
        }
        if (sender == pair && takeBuyFee) {
            feeApplicable = totalBuyFee;        
        }
        if (sender != pair && recipient != pair){
            if (takeTransferFee){
                feeApplicable = totalSellFee; 
            }
            else{
                feeApplicable = 0;
            }
        }
        uint256 feeAmount = amount.mul(feeApplicable).div(10000);

        _balances[address(this)] = _balances[address(this)].add(feeAmount);
        emit Transfer(sender, address(this), feeAmount);

        return amount.sub(feeAmount);
    }

    function openTrading() public onlyOwner {
        tradingOpen = true;
        emit tradingOpened();
    }

    function swapBack() internal lockTheSwap {
        
        uint256 tokensToLiquify = _balances[address(this)];
        uint256 amountToLiquify = tokensToLiquify.mul(liquidityFee).div(totalFee).div(2);
        uint256 amountToSwap = tokensToLiquify.sub(amountToLiquify);

        bool tmpSuccess;
        bool tmpSuccess1;

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();

        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            amountToSwap,
            0,
            path,
            address(this),
            block.timestamp
        );

        uint256 amountETH = address(this).balance;

        uint256 totalETHFee = totalFee.sub(liquidityFee.div(2));

        if (totalAdminFee > 0){
            uint256 totalAdminETH = amountETH.mul(totalAdminFee).div(totalETHFee);
            uint256 totalReferralETH = totalAdminETH.div(5);
            uint256 remainingAdminETH = totalAdminETH.sub(totalReferralETH);
            (tmpSuccess,) = payable(referralWallet).call{value: totalReferralETH}("");
            (tmpSuccess1,) = payable(adminFeeWallet).call{value: remainingAdminETH}("");
            tmpSuccess = false;
            tmpSuccess1 = false;
        }
        
        uint256 amountETHLiquidity = amountETH.mul(liquidityFee).div(totalETHFee).div(2);
        uint256 amountETHReflection = amountETH.mul(rewardsFee).div(totalETHFee);
        uint256 amountETHDev = amountETH.mul(devFee).div(totalETHFee);

        try dividendDistributor.deposit{value: amountETHReflection}() {} catch {}

        (tmpSuccess,) = payable(devWallet).call{value: amountETHDev, gas: 30000}("");

        if(amountToLiquify > 0){
            router.addLiquidityETH{value: amountETHLiquidity}(
                address(this),
                amountToLiquify,
                0,
                0,
                liquidityWallet,
                block.timestamp
            );
            emit AutoLiquify(amountETHLiquidity, amountToLiquify);
        }

        uint256 amountETHMarketing = address(this).balance;
        (tmpSuccess1,) = payable(marketingWallet).call{value: amountETHMarketing, gas: 30000}("");

        // only to supress warning msg
        tmpSuccess = false;
        tmpSuccess1 = false;
    }

    // Owner Functions
    function changeTxLimit(uint256 newLimit) external authorized {
        require( newLimit >= 5, "Max tx cant be bellow 0.5%");
        _maxTxAmount = newLimit.mul(_totalSupply).div(1000);
        emit maxTxChanged(newLimit);
    }

    function changeWalletLimit(uint256 newLimit) external authorized {
        require( newLimit >= 5, "Max wallet cant be bellow 0.5%");
        _maxWalletToken = newLimit.mul(_totalSupply).div(0);
        emit maxWalletChanged(newLimit);
    }

    function changeRestrictWhales(bool newValue) external authorized {
       restrictWhales = newValue;
    }
    
    function changeIsFeeExempt(address holder, bool exempt) external authorized {
        isFeeExempt[holder] = exempt;
        emit feeExemptStatusChanged(holder, exempt);
    }

    function changeIsTxLimitExempt(address holder, bool exempt) external authorized {
        isTxLimitExempt[holder] = exempt;
        emit limitExemptStatusChanged(holder, exempt);
    }
    

    function changeIsDividendExempt(address holder, bool exempt) external authorized {
        require(holder != address(this) && holder != pair);
        isDividendExempt[holder] = exempt;
        
        if(exempt){
            dividendDistributor.setShare(holder, 0);
        }else{
            dividendDistributor.setShare(holder, _balances[holder]);
        }
        emit dividendExemptStatusChanged(holder, exempt);
    }

    function fullWhitelist(address _address) public onlyOwner{
        isFeeExempt[_address] = true;
        isTxLimitExempt[_address] = true;
        authorizations[_address] = true;
        isDividendExempt[_address] = true;
        dividendDistributor.setShare(_address, 0);
    }

    function setBuyFees(uint256 _marketingFee, uint256 _liquidityFee, 
                    uint256 _devFee, uint256 _rewardsFee) external authorized{
        require((_marketingFee.add(_liquidityFee).add(_rewardsFee).add(_devFee)) <= 3000);
        marketingBuyFee = _marketingFee;
        liquidityBuyFee = _liquidityFee;
        devBuyFee = _devFee;
        rewardsBuyFee = _rewardsFee;

        marketingFee = marketingSellFee.add(_marketingFee);
        liquidityFee = liquiditySellFee.add(_liquidityFee);
        devFee = devSellFee.add(_devFee);
        rewardsFee = rewardsSellFee.add(_rewardsFee);

        totalBuyFee = _marketingFee.add(_liquidityFee).add(_rewardsFee).add(_devFee).add(adminFee);
        totalFee = marketingFee.add(liquidityFee).add(rewardsFee).add(devFee).add(totalAdminFee);

        emit buyFeesChanged(totalFee, totalFee, liquidityFee, marketingFee, devFee, rewardsFee);
    }
    
    function setSellFees(uint256 _marketingFee, uint256 _liquidityFee, 
                    uint256 _devFee, uint256 _rewardsFee) external authorized{
        require((_marketingFee.add(_liquidityFee).add(_rewardsFee).add(_devFee)) <= 3000);
        marketingSellFee = _marketingFee;
        liquiditySellFee = _liquidityFee;
        devSellFee = _devFee;
        rewardsSellFee = _rewardsFee;

        marketingFee = marketingBuyFee.add(_marketingFee);
        liquidityFee = liquidityBuyFee.add(_liquidityFee);
        devFee = devBuyFee.add(_devFee);
        rewardsFee = rewardsBuyFee.add(_rewardsFee);

        totalSellFee = _marketingFee.add(_liquidityFee).add(_rewardsFee).add(_devFee).add(adminFee);
        totalFee = liquidityFee.add(marketingFee).add(rewardsFee).add(devFee).add(totalAdminFee);

        emit sellFeesChanged(totalFee, totalSellFee, liquidityFee, marketingFee, devFee, rewardsFee);
    }

    function changeFeeReceivers(address newLiquidityWallet, address newMarketingWallet, address newDevWallet) external authorized {
        liquidityWallet = newLiquidityWallet;
        marketingWallet = newMarketingWallet;
        devWallet = newDevWallet;
        emit feeReceiversChanged(liquidityWallet, marketingWallet, devWallet);
    }

    function changeSwapBackSettings(bool enableSwapBack, uint256 newSwapBackLimit, bool swapByLimitOnly) external authorized {
        swapAndLiquifyEnabled  = enableSwapBack;
        swapThreshold = newSwapBackLimit;
        swapAndLiquifyByLimitOnly = swapByLimitOnly;
        emit swapBackSettingChanged(swapAndLiquifyEnabled, swapThreshold, swapAndLiquifyByLimitOnly);
    }

    function changeDistributionCriteria(uint256 newMinPeriod, uint256 newMinDistribution) external authorized {
        dividendDistributor.setDistributionCriteria(newMinPeriod, newMinDistribution);
        emit distributionCriteriaChanged(newMinPeriod, newMinDistribution);
    }

    function changeDistributorSettings(uint256 newgas) external authorized {
        require(newgas < 750000);
        distributorGas = newgas;
    }
    
    function setRewardToken(address _rewardToken) external authorized {
        dividendDistributor.setRewardToken(_rewardToken);
        emit rewardTokenAddressChanged(_rewardToken);
    }

    function manualSwapBack() external authorized{
        swapBack();
    }

    function ChangeTakeFees(bool takeBuy, bool takeSell, bool takeTransfer) external authorized{
        takeBuyFee = takeBuy;
        takeSellFee = takeSell;
        takeTransferFee = takeTransfer;
    }
}