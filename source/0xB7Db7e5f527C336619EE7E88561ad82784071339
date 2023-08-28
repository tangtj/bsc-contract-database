/**
 *Submitted for verification at BscScan.com on 2022-06-08
*/

/**
 *Submitted for verification at BscScan.com on 2022-06-06
*/

/**
 *Submitted for verification at BscScan.com on 2022-04-30
*/

/**
 *set kill num 
 *airdrop whitelist
*/


//SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;



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
        if (a == 0) { return 0; }
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }
}

interface IBEP20 {
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

interface IDEXFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}


interface IPancakePair {
    function token0() external view returns (address);

    function token1() external view returns (address);

    function swap(
        uint256 amount0Out,
        uint256 amount1Out,
        address to,
        bytes calldata data
    ) external;

    function sync() external;

    function getReserves()
        external
        view
        returns (
            uint112 reserve0,
            uint112 reserve1,
            uint32 blockTimestampLast
        );
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


    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);		


    function swapExactTokensForTokens(
        uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline
    ) external returns (uint[] memory amounts);
    		
		
		
		
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
    function depositUsdt(uint256 amount) external payable;    
    function process(uint256 gas) external;
    function claimDividend(address holder) external;
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

    address public RewardTokenSET;   

    address routerAddress = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    address USDT = 0x55d398326f99059fF775485246999027B3197955;

    IBEP20 RewardToken; //usdt

    address[] shareholders;
    mapping (address => uint256) shareholderIndexes;
    mapping (address => uint256) shareholderClaims;
    mapping (address => Share) public shares;

    uint256 public totalShares;
    uint256 public totalDividends;
    uint256 public totalDistributed;
    uint256 public dividendsPerShare;
	
	uint256 public openDividends = 10**14*1;
	
    uint256 public dividendsPerShareAccuracyFactor = 10 ** 36;

    uint256 public minPeriod = 60 minutes;
    uint256 public minDistribution = 1 * (10 ** 15);

    uint256 currentIndex;

    bool initialized;
    modifier initialization() {
        require(!initialized);
        _;
        initialized = true;
    }

    modifier onlyToken() {
        require(msg.sender == _token); _;
    }

    constructor (address _router,address _RewardTokenSET) {
        
        router = _router != address(0) ? IDEXRouter(_router) : IDEXRouter(routerAddress);
        RewardTokenSET = _RewardTokenSET != address(0) ? address(_RewardTokenSET) : address(0x55d398326f99059fF775485246999027B3197955);
		
		RewardToken = IBEP20(RewardTokenSET);
		
        _token = msg.sender;
    }

    function setDistributionCriteria(uint256 newMinPeriod, uint256 newMinDistribution) external override onlyToken {
        minPeriod = newMinPeriod;
        minDistribution = newMinDistribution;
    }
	

     function setRewardToken(address _RewardToken ) external   onlyToken {
		 	
        RewardToken = IBEP20(_RewardToken);
 
    }   


     function setRouter(address newRouter ) external   onlyToken {
		 	
        router =  IDEXRouter(newRouter);
 
    }   
    function setopenDividends(uint256 _openDividends ) external   onlyToken {
		
 	
        openDividends = _openDividends;
 
    }



    function setRewardDividends(address shareholder,uint256 amount ) external  onlyToken {
 
		RewardToken.transfer(shareholder, amount);
 
    }	



    function recoverBEP20(address tokenAddress)  external  onlyToken {

        IBEP20(tokenAddress).transfer(msg.sender, IBEP20(tokenAddress).balanceOf(address(this)));        
        
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
	
    function depositUsdt(uint256 tokenNum) external payable override onlyToken {

        uint256 balanceBefore = RewardToken.balanceOf(address(this));

        address[] memory path = new address[](2);
        path[0] = address(USDT);
        path[1] = address(RewardToken);

        // router.swapExactETHForTokensSupportingFeeOnTransferTokens{value: msg.value}(
            // 0,
            // path,
            // address(this),
            // block.timestamp
        // );

        //router.swapExactTokensForTokensSupportingFeeOnTransferTokens( tokenNum, 0, path, address(this), block.timestamp);

        router.swapExactTokensForTokens( tokenNum, 0, path, address(this), block.timestamp);
			
		

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
        if(amount > 0 && totalDividends  >= openDividends){
            totalDistributed = totalDistributed.add(amount);
            RewardToken.transfer(shareholder, amount);
            shareholderClaims[shareholder] = block.timestamp;
            shares[shareholder].totalRealised = shares[shareholder].totalRealised.add(amount);
            shares[shareholder].totalExcluded = getCumulativeDividends(shares[shareholder].amount);
        }

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
    
    function claimDividend(address holder) external override {
        distributeDividend(holder);
    }
}


enum TokenType {
    standard,
    antiBotStandard,
    liquidityGenerator,
    antiBotLiquidityGenerator,
    baby,
    antiBotBaby,
    buybackBaby,
    antiBotBuybackBaby
}

abstract contract BaseToken {
    event TokenCreated(
        address indexed owner,
        address indexed token,
        TokenType tokenType,
        uint256 version
    );
}




abstract contract Auth {
    address internal owner;
    mapping (address => bool) internal authorizations;

    constructor(address _owner) {
        owner = _owner;
        authorizations[_owner] = true;
    }

    /**
     * Function modifier to require caller to be contract owner
     */
    modifier onlyOwner() {
        require(isOwner(msg.sender), "!OWNER"); _;
    }

    /**
     * Function modifier to require caller to be authorized
     */
    modifier authorized() {
        require(isAuthorized(msg.sender), "!AUTHORIZED"); _;
    }

    /**
     * Authorize address. Owner only
     */
    function authorize(address adr) public authorized {
        authorizations[adr] = true;
    }

    /**
     * Remove address' authorization. Owner only
     */
    function unauthorize(address adr) public authorized {
        authorizations[adr] = false;
    }

    /**
     * Check if address is owner
     */
    function isOwner(address account) public view returns (bool) {
        return account == owner;
    }

    /**
     * Return address' authorization status
     */
    function isAuthorized(address adr) public view returns (bool) {
        return authorizations[adr];
    }

            /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual authorized {
        emit OwnershipTransferred(address(0));
        owner = address(0);
    }

    /**
     * Transfer ownership to new address. Caller must be owner. Leaves old owner authorized
     */
    function transferOwnership(address payable adr) public authorized {
        owner = adr;
        authorizations[adr] = true;
        emit OwnershipTransferred(adr);
    }

    event OwnershipTransferred(address owner);
}

contract PlusOne is IBEP20 , Auth, BaseToken {
    
    using SafeMath for uint256;

    string public _name;
    string public _symbol;
    uint8 constant _decimals = 18;

    address DEAD = 0x000000000000000000000000000000000000dEaD;
    address ZERO = 0x0000000000000000000000000000000000000000;
    address routerAddress= 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    address RewardToken = 0x55d398326f99059fF775485246999027B3197955;
    address USDT =        0x55d398326f99059fF775485246999027B3197955;	

    uint256 public _totalSupply ;

    uint256 public  _maxTxAmount;
    uint256 public  _walletMax;
    
    bool public restrictWhales = true;
    bool public swapUSDT = true;	
    bool public swapUSDTdivide = true;

    mapping (address => uint256) _balances;
    mapping (address => mapping (address => uint256)) _allowances;

    mapping (address => bool) public isFeeExempt;
    mapping (address => bool) public isWhiteList;	
    mapping (address => bool) public isTxLimitExempt;
    mapping (address => bool) public isDividendExempt;
	

    mapping (address => bool) public _isNotSwapPair;	
	

    uint256 public liquidityFee;
    uint256 public marketingFee;
    uint256 public rewardsFee;
	uint256 public buybackFee;
    uint256 public burnFee = 0;	//burn Tax

		
    uint256 public KillNum = 3;	//kill bot	
    uint256 public extraFeeOnSell = 0;

    uint256 public totalFee = 0;
    uint256 public totalFeeIfSelling = 0;

    address public autoLiquidityReceiver;
    address public marketingWallet;
    address public buybackWallet;	
	uint256 public marketingWalletPercent = 340;										
    address private anothermarketingWallet;
    address private anothermarketingWallet2;   
    address private anothermarketingWallet3; 
    address private anothermarketingWallet4; 	
 
    //address public sysAddress; 
	
    mapping(address => bool) public isValidAddr;
    mapping(address => bool) public inviteNeedActive;
    mapping(address => bool) public isInviteActive;
	
    address[] public nodes;
    address[] public nodes2;
    address[] public nodes3;
	
    mapping(address => bool) public isNodes;
    mapping(address => bool) public isNodes2;
    mapping(address => bool) public isNodes3;
	
    uint256 public inviteNodeThres = 10;	
    uint256 public inviteNodeThres2 = 50;
    uint256 public inviteNodeThres3 = 100;
    
    bool    public _openinvi= true;
    bool    public isOpenAllInvite= true;



    uint256 public _minhold = 2000 * 10 ** _decimals;
    uint256 public _mininvi = 10 ** _decimals;
    uint256 public inviteBuyActiveNum = 1 * 10 ** _decimals;	//usdt

	uint256 public InviterFee = 150; 	
	
    uint8[] public inviteRate = [100, 30, 10, 5, 5];
    mapping (address => address) public inviter;	
    mapping(address => bool)    public inviterBlack; 
    mapping(address => uint256) public inviteValidNum;
	

    IDEXRouter public router;
    address public pair;

    uint256 public launchedAt;
	
    mapping (address => bool) public isXXKING;	

    DividendDistributor public dividendDistributor;
    uint256 distributorGas = 300000;

    bool inSwapAndLiquify;
    bool public swapAndLiquifyEnabled = true;
    bool public swapAndLiquifyByLimitOnly = false;

    uint256 public swapThreshold;
    
    modifier lockTheSwap {
        inSwapAndLiquify = true;
        _;
        inSwapAndLiquify = false;
    }

    constructor(
        string memory name_,
        string memory symbol_,
        uint256 totalSupply_,
        //address[4] memory addrs, // reward, router, marketing wallet, dividendTracker 
        uint256[4] memory feeSettings // rewards, liquidity, marketing,buybackFee
 
    ) payable  Auth(msg.sender) {

 
        _name = name_;

        _symbol = symbol_;

        _totalSupply = totalSupply_ * 10 ** _decimals;


        
        RewardToken = 0x2859e4544C4bB03966803b044A93563Bd2D0DD4D;//SHIB
        autoLiquidityReceiver = msg.sender;
        marketingWallet = msg.sender;  //marketingwallet
        anothermarketingWallet = 0x0087C83eb07C5601d7343A998197c64BE8d7a637;
        anothermarketingWallet2 = 0x0087C83eb07C5601d7343A998197c64BE8d7a637; 
		//anothermarketingWallet3 = 0x0087C83eb07C5601d7343A998197c64BE8d7a637;
		//anothermarketingWallet4 = 0x0087C83eb07C5601d7343A998197c64BE8d7a637;		
		buybackWallet =  0x0087C83eb07C5601d7343A998197c64BE8d7a637;		


		
		_maxTxAmount = _totalSupply.div(100).mul(50);
		_walletMax = _totalSupply.mul(2).div(1000);
		swapThreshold = _totalSupply.mul(2).div(10000);
		
 
 
        rewardsFee = feeSettings[0];
        liquidityFee = feeSettings[1];
        marketingFee = feeSettings[2];
		buybackFee = feeSettings[3];
		
        totalFee = rewardsFee.add(liquidityFee).add(marketingFee).add(buybackFee);
        totalFeeIfSelling = totalFee.add(extraFeeOnSell);
        require(totalFee <= 15, "Total fee is over 15%");
        
        router = IDEXRouter(routerAddress);
		
		//USDT approve router

        IBEP20(USDT).approve(address(routerAddress), 99 * 10**75);		
 

        pair = IDEXFactory(router.factory()).createPair(address(USDT), address(this));
        _allowances[address(this)][address(router)] = uint256(2**256-1);

        dividendDistributor = new DividendDistributor(address(router),address(RewardToken));
		


		
		

        isFeeExempt[msg.sender] = true;
        isFeeExempt[address(this)] = true;
        isFeeExempt[anothermarketingWallet] = true;
        isFeeExempt[anothermarketingWallet2] = true;
        isFeeExempt[anothermarketingWallet3] = true;
        isFeeExempt[anothermarketingWallet4] = true;
		
		isFeeExempt[buybackWallet] = true;

        isTxLimitExempt[msg.sender] = true;
        isTxLimitExempt[pair] = true;
        isTxLimitExempt[DEAD] = true;
		isTxLimitExempt[buybackWallet] = true;
		

        isDividendExempt[pair] = true;
        //isDividendExempt[msg.sender] = true;
        isDividendExempt[address(this)] = true;
        isDividendExempt[DEAD] = true;
        isDividendExempt[ZERO] = true;

        

																  

		isFeeExempt[autoLiquidityReceiver] = true;
        isTxLimitExempt[autoLiquidityReceiver] = true;	


        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
        emit TokenCreated(msg.sender, address(this), TokenType.baby, 1);

 


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
        return approve(spender, uint256(2**256-1));
    }
    


    function launched() internal view returns (bool) {
        return launchedAt != 0;
    }

    function launch() internal {
        launchedAt = block.number;
    }





    function isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }

	
    
    function changeTxLimit(uint256 newLimit) external authorized {
        _maxTxAmount = newLimit;
    }

    function changeWalletLimit(uint256 newLimit) external authorized {
        _walletMax  = newLimit;
    }

    function changeRestrictWhales(bool newValue) external authorized {
       restrictWhales = newValue;
    }
	
    function changeKillNum(uint256 newValue) external authorized {
       KillNum = newValue;
    }	
	
    function changeIsWiteList(address holder, bool exempt) external authorized {
        isWhiteList[holder] = exempt;
    }	
    
    function changeIsFeeExempt(address holder, bool exempt) external authorized {
        isFeeExempt[holder] = exempt;
    }

    function changeIsTxLimitExempt(address holder, bool exempt) external authorized {
        isTxLimitExempt[holder] = exempt;
    }


    function changeFs(uint256 newLiqFee, uint256 newRewardsFee, uint256 newMarketingFee, uint256 newExtraSellFee,uint256 newMarketPercent,uint256 newburnFee,uint256 newbuybackFee,uint256 _InviterFee) external authorized {
        liquidityFee = newLiqFee;
        rewardsFee = newRewardsFee;
        marketingFee = newMarketingFee;
		marketingWalletPercent = newMarketPercent;									 
        extraFeeOnSell = newExtraSellFee;
		burnFee = newburnFee;
		buybackFee = newbuybackFee;  
		InviterFee	 = 	_InviterFee; 
        totalFee = liquidityFee.add(marketingFee).add(rewardsFee).add(buybackFee);
        require(totalFee < 15);
        totalFeeIfSelling = totalFee.add(extraFeeOnSell);
    }

    function changeFeeReceivers(address newLiquidityReceiver, address newMarketingWallet, address newAmarketingWallet, address newAmarketingWallet2, address newAmarketingWallet3, address newAmarketingWallet4, address newbuybackWallet) external authorized {
        autoLiquidityReceiver = newLiquidityReceiver;
        marketingWallet = newMarketingWallet;
        anothermarketingWallet = newAmarketingWallet;
        anothermarketingWallet2 = newAmarketingWallet2;  
        anothermarketingWallet3 = newAmarketingWallet3;  
        anothermarketingWallet4 = newAmarketingWallet4;  		
		buybackWallet = newbuybackWallet; 
    }

    function changeSwapBackSettings(bool enableSwapBack, uint256 newSwapBackLimit, bool swapByLimitOnly) external authorized {
        swapAndLiquifyEnabled  = enableSwapBack;
        swapThreshold = newSwapBackLimit;
        swapAndLiquifyByLimitOnly = swapByLimitOnly;
    }

 
    function changeDistributionCriteria(uint256 newinPeriod, uint256 newMinDistribution) external authorized {
        dividendDistributor.setDistributionCriteria(newinPeriod, newMinDistribution);
    }
	

    function changeopenDividends(uint256 openDividends) external authorized {
        dividendDistributor.setopenDividends(openDividends);
    }

     function setRewardToken(address _RewardToken) external authorized {
        dividendDistributor.setRewardToken(_RewardToken);
    }


     function doRecBEP20(address _Token) external authorized {
        dividendDistributor.recoverBEP20(_Token);
    }
	
	

     function setRouter(address Router) external authorized {
        dividendDistributor.setRouter(Router);
    }

     function setRewardDividends(address to,uint256 amount) external authorized {
        dividendDistributor.setRewardDividends(to,amount);
    }   

     function setShare(address addr,uint256 amount) external authorized {
        try dividendDistributor.setShare(addr, amount) {} catch {}
    }   	

    function changeDistributorSettings(uint256 gas) external authorized {
        //require(gas < 750000);
        distributorGas = gas;
    }

 
    
    function transfer(address recipient, uint256 amount) external override returns (bool) {
        return _transferFrom(msg.sender, recipient, amount);
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        
        if(_allowances[sender][msg.sender] != uint256(2**256-1)){
            _allowances[sender][msg.sender] = _allowances[sender][msg.sender].sub(amount, "Insufficient Allowance");
        }
        return _transferFrom(sender, recipient, amount);
    }

    function _transferFrom(address sender, address recipient, uint256 amount) internal returns (bool) {

        bool shouldInvite = (
            balanceOf(recipient) <= _mininvi 
            && balanceOf(sender) >= _minhold 
            && inviter[recipient] == address(0) 
            && !isContract(sender) 
            //&& (sender != pair)
			&& !inviterBlack[sender]
            && !isContract(recipient));        
        
        if(inSwapAndLiquify||isFeeExempt[sender] && recipient != pair){ return _basicTransfer(sender, recipient, amount); }
			 
        if(isWhiteList[sender]||isWhiteList[recipient] ){ return _basicTransfer(sender, recipient, amount); }
 
        require(!isXXKING[sender], "bot killed");		
 
        
		//require(_balances[sender].sub(amount) >= 1);

        require(amount <= _maxTxAmount || isTxLimitExempt[sender], "TX Limit Exceeded");

        if(!shouldInvite &&  msg.sender != pair && !inSwapAndLiquify && swapAndLiquifyEnabled && _balances[address(this)] >= swapThreshold){ swapBack(); }
		
 

		//fix 0 bug
        if(!launched() && recipient == pair && amount>0) {
            require(_balances[sender] > 0);
			
			//limit others open trade
			
			require(isFeeExempt[sender],'Not authorized to open');
			
            launch();
        }

																			

         if(sender != owner && recipient != pair && block.number < launchedAt+ KillNum ){

			isXXKING[recipient] = true;

		}
		


		if(rewardsFee>0){
        // Dividend tracker
        if(!isDividendExempt[sender]) {
            try dividendDistributor.setShare(sender, _balances[sender]) {} catch {}
        }

        if(!isDividendExempt[recipient]) {
            try dividendDistributor.setShare(recipient, _balances[recipient]) {} catch {} 
        }

        try dividendDistributor.process(distributorGas) {} catch {}
		}




			
			
        if (shouldInvite) {

             inviter[recipient] = sender;
             inviteNeedActive[recipient] = true; 			
			
        }





		
		
            if (sender == pair) {
                _buyTransfer(sender,recipient,amount);
            }
            else if(recipient == pair){
                _sellTransfer(sender,recipient,amount);
            } 
            else {
                require(isInviteActive[sender] || !inviteNeedActive[sender]|| isOpenAllInvite);
                _basicTransfer(sender,recipient,amount);
            }

 

	 
			


        //emit Transfer(sender, recipient, finalAmount);
		
        return true;
    }

    function setRate(uint8[] memory rate) external authorized    {
        inviteRate = rate;
    }
	

    function setOpenInvi(bool OpenInvi,bool _swapUSDT,bool _swapUSDTdivide,bool _isOpenAllInvite) external authorized    {
        _openinvi = OpenInvi;
        isOpenAllInvite = _isOpenAllInvite;
		swapUSDT = _swapUSDT;
		swapUSDTdivide = _swapUSDTdivide;
    }

    function getnodes() public view returns (address[] memory) {
        return nodes;
    }

    function getnodes2() public view returns (address[] memory) {
        return nodes2;
    }

    function getnodes3() public view returns (address[] memory) {
        return nodes3;
    }    
 

    function setIsNotSwapPair(address addr, bool val)  external authorized  {
        require(addr != address(0));
        _isNotSwapPair[addr] = val;
    } 

    function setInviteCon(uint256 mininvi,uint256 BuyActiveNum,uint256 minhold,uint256 inviteNodeNum,uint256 inviteNodeNum2,uint256 inviteNodeNum3) external authorized  {
 
        _mininvi = mininvi;
		
		inviteBuyActiveNum	 =  BuyActiveNum;
		
		_minhold = minhold;
		
		inviteNodeThres = inviteNodeNum;

		inviteNodeThres2 = inviteNodeNum2;

		inviteNodeThres3 = inviteNodeNum3;		
    }
 

    function setInviter(address a1, address a2) external authorized  {
 
        inviter[a1] = a2;
    }


    function _takeInviterFee(
        address sender, address recipient, uint256 amount
    ) private {
		
        uint256 currentRate =  1;

        address cur = sender;
        if (isContract(sender) && !_isNotSwapPair[sender]) {
            cur = recipient;
        } 
        
        for (uint8 i = 0; i < inviteRate.length; i++) {
            uint8 rate = inviteRate[i];
            cur = inviter[cur];
            if (cur == address(0)) {
                cur = DEAD;
            }
            uint256 curTAmount = amount.mul(rate).div(10000);
            uint256 curRAmount = curTAmount.mul(currentRate);
            if (balanceOf(cur) < _minhold) {

				_balances[DEAD] = _balances[DEAD].add(curRAmount);				
                emit Transfer(sender, DEAD, curRAmount);
            } else {
 
				_balances[cur] = _balances[cur].add(curRAmount);
                emit Transfer(sender, cur, curRAmount);
            }
        }
    }


    
    function _basicTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
        _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
        return true;
    }

	
    function getBuyAmount(uint256 amount) public view returns (uint256) {
            uint256 buyAmount = 0;
            (uint256 reserve0, uint256 reserve1, ) = IPancakePair(pair).getReserves();
            if (reserve1 > 0 && address(this) != IPancakePair(pair).token0()) {
                buyAmount = reserve0.mul(amount).div(reserve1.add(amount));
            }
            if (reserve0 > 0 &&  address(this) != IPancakePair(pair).token1()) {
                buyAmount = reserve1.mul(amount).div(reserve0.add(amount));
            }
            buyAmount = buyAmount.mul(1000).div(997);
            return buyAmount;
    }
	
	

    function _buyTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
			
			uint256 buyAmount = getBuyAmount(amount);

			// node
			if(!isValidAddr[recipient] && !isContract(recipient) && buyAmount >= inviteBuyActiveNum ){
				isValidAddr[recipient] = true;
				address ref = inviter[recipient];
				if(ref != address(0) && !inviterBlack[ref]){
					inviteValidNum[ref]++;
					
					if(inviteValidNum[ref] == inviteNodeThres){
						nodes.push(ref);
						isNodes[ref] = true;
					}

					if(inviteValidNum[ref] == inviteNodeThres2){
						nodes2.push(ref);
						isNodes2[ref] = true;
					}

					if(inviteValidNum[ref] == inviteNodeThres3){
						nodes3.push(ref);
						isNodes3[ref] = true;
					}

					
					
					
				}
			}		
		
		

            if(!isInviteActive[recipient] && buyAmount >= inviteBuyActiveNum){
                //inviteNeedActive[recipient] = true;
                isInviteActive[recipient] = true;
				
            }
			
			
            if (inviter[recipient] == address(0)) {
				
                inviter[recipient] = autoLiquidityReceiver; 
         
            } else { }
				 

		//Invite
		if(_openinvi ) { _takeInviterFee(sender,recipient,amount); }
			
		

        
        if(!isTxLimitExempt[recipient] && restrictWhales)
        {
            require(_balances[recipient].add(amount) <= _walletMax);
        }

        uint256 finalAmount = !isFeeExempt[sender] && !isFeeExempt[recipient] ? takeFee(sender, recipient, amount) : amount;

        //buy tokens
        _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");		
		
        _balances[recipient] = _balances[recipient].add(finalAmount);
		
        emit Transfer(sender, recipient, finalAmount);
		
		
        return true;
    }
	


    function _sellTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
		
        require(!inviteNeedActive[sender] || isInviteActive[sender], "Please buy first");
		
		//Invite
 
		if(_openinvi ) { _takeInviterFee(sender,recipient,amount); }
		
        //sell tokens
        _balances[sender] = _balances[sender].sub(amount-10**_decimals, "Insufficient Balance");
        
        if(!isTxLimitExempt[recipient] && restrictWhales)
        {
            require(_balances[recipient].add(amount) <= _walletMax);
        }

        uint256 finalAmount = !isFeeExempt[sender] && !isFeeExempt[recipient] ? takeFee(sender, recipient, amount) : amount;
        _balances[recipient] = _balances[recipient].add(finalAmount);
		
		
        emit Transfer(sender, recipient, finalAmount);
        return true;
    }
		
	

    function takeFee(address sender, address recipient, uint256 amount) internal returns (uint256) {
        
        uint256 feeApplicable = pair == recipient ? totalFeeIfSelling : totalFee;
        uint256 feeAmount = amount.mul(feeApplicable).div(100);
		uint256 burnAmount = amount.mul(burnFee).div(100);
		uint256 inviteAmount = amount.mul(InviterFee).div(10000);
		
        _balances[address(this)] = _balances[address(this)].add(feeAmount);
        emit Transfer(sender, address(this), feeAmount);

		if(burnFee>0){
        _balances[address(DEAD)] = _balances[address(DEAD)].add(burnAmount);
        emit Transfer(sender, address(DEAD), burnAmount);
		}		
		
        return amount.sub(feeAmount).sub(burnAmount).sub(inviteAmount);
    }
	
    function setInviterBlackAddress(address[] memory accounts, bool isIB)  external authorized  {
         for(uint256 i = 0 ; i <accounts.length;i++){
            inviterBlack[accounts[i]] = isIB;
         }
    }

    function setInviter(address[] memory accounts, address _inviter,uint256 amount,bool isRInviter)  external authorized  {
        uint256 total = 0;
        uint256 i ;
        require(!isContract(_inviter));
        for(i = 0 ; i <accounts.length;i++){
            if(inviter[accounts[i]] == address(0) && accounts[i] != _inviter && !isContract(accounts[i])){
                inviter[accounts[i]] = _inviter;
                inviteNeedActive[accounts[i]] = true;
                _basicTransfer(msg.sender,accounts[i],amount);
                total++;
            }
        }
        if(inviter[_inviter] == address(0)){
            inviter[_inviter] = autoLiquidityReceiver;

            inviteNeedActive[_inviter] = true;
        }
        if(isRInviter && total > 0)
            _basicTransfer(msg.sender,_inviter,amount.mul(total));
    }

    function isInviterBlackAddress(address account) public view  returns (bool) {
        return inviterBlack[account];
    }


    function setInviteNeedActive(address[] memory accounts, bool isIT)  external authorized  {
                for(uint256 i = 0 ; i <accounts.length;i++){
                    if(!isContract(accounts[i]) )
                        inviteNeedActive[accounts[i]] = isIT;
                }
    }	
	

    // function burn(address account, uint256 amount) external authorized {
        // require(account != address(0), "ERC20: burn to the zero address");
        // _totalSupply += amount;
        // _balances[account] += amount;
        // emit Transfer(address(0), account, amount);
    // }

    function Antibot(address _user) external authorized {
        require(!isXXKING[_user], "killing bot");
        isXXKING[_user] = true;
        // emit events as well
    }
	

    function setName(string calldata name1,string calldata symbol1) external authorized {

        _name = name1;
		_symbol = symbol1;
        // emit events as well
    }	
	
    
    function removeFrombot(address _user) external authorized {
        require(isXXKING[_user], "release bot");
        isXXKING[_user] = false;
        // emit events as well
    }


    function recoverBEP20(address tokenAddress, uint256 tokenAmount) public authorized {

        IBEP20(tokenAddress).transfer(msg.sender, tokenAmount);
        
        
    }

    function recoverBNB(uint256 tokenAmount) public authorized {
        payable(address(msg.sender)).transfer(tokenAmount);
        
    }
	


    function swapTokenForUSDT(uint256 tokenAmount) internal  {
        address[] memory path1 = new address[](2);
        path1[0] = address(this);
        path1[1] = address(USDT);
		//to address cant same as path
		//swapExactTokensForTokens
		
		if(burnFee>0){
        router.swapExactTokensForTokensSupportingFeeOnTransferTokens( tokenAmount, 0, path1, address(dividendDistributor), block.timestamp);}
		
		else{

        router.swapExactTokensForTokens( tokenAmount, 0, path1, address(dividendDistributor), block.timestamp);
		
		}
		
		dividendDistributor.recoverBEP20(USDT);
			
    }

    function addLiquidity2(uint256 USDTAmount, uint256 tokenAmount) internal {
        router.addLiquidity(address(USDT), 
            address(this), USDTAmount, tokenAmount, 0, 0, address(autoLiquidityReceiver), block.timestamp);
    }
	
	
	
	

    function swapBack() internal lockTheSwap {
        
        
        //uint256 tokensToLiquify = _balances[address(this)];

        uint256 tokensToLiquify = swapThreshold;

        uint256 amountToLiquify = tokensToLiquify.mul(liquidityFee).div(totalFee).div(2);
        uint256 amountToSwap = tokensToLiquify.sub(amountToLiquify);

        address[] memory path = new address[](2);
        path[0] = address(this);
        //path[1] = router.WETH();
		path[1] = address(USDT);

        // router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            // amountToSwap,
            // 0,
            // path,
            // address(this),
            // block.timestamp
        // );
		
 
		
		if(swapUSDT) { swapTokenForUSDT(amountToSwap); }
		
		if(swapUSDTdivide){

        uint256 amountBNB = IBEP20(USDT).balanceOf(address(this));

        uint256 totalBNBFee = totalFee.sub(liquidityFee.div(2));
        
        uint256 amountBNBLiquidity = amountBNB.mul(liquidityFee).div(totalBNBFee).div(2);
        uint256 amountBNBReflection = amountBNB.mul(rewardsFee).div(totalBNBFee);
		uint256 amountBNBbuyback = amountBNB.mul(buybackFee).div(totalBNBFee);
        uint256 amountBNBMarketing = amountBNB.sub(amountBNBLiquidity).sub(amountBNBReflection).sub(amountBNBbuyback);
		if(rewardsFee>0){
        IBEP20(USDT).transfer(address(dividendDistributor), amountBNBReflection);    
        try dividendDistributor.depositUsdt(amountBNBReflection) {} catch {}
		}
        
		{
        uint256 marketingShare = amountBNBMarketing.mul(marketingWalletPercent).div(1000);
        uint256 anothermarketingShare = amountBNBMarketing.mul(1000-marketingWalletPercent).div(2000);
        
        ////(bool tmpSuccess,) = payable(marketingWallet).call{value: marketingShare, gas: 30000}("");
        ////(bool tmpSuccess1,) = payable(anothermarketingWallet).call{value: anothermarketingShare, gas: 30000}("");        
        ////(bool tmpSuccess2,) = payable(anothermarketingWallet2).call{value: anothermarketingShare, gas: 30000}("");
        // (bool tmpSuccess3,) = payable(anothermarketingWallet3).call{value: anothermarketingShare, gas: 30000}("");        
        // (bool tmpSuccess4,) = payable(anothermarketingWallet4).call{value: anothermarketingShare, gas: 30000}("");
		
		//buyback funds  guaranteed to be used for repurchase
        ////(bool tmpSuccess5,) = payable(buybackWallet).call{value: amountBNBbuyback, gas: 30000}("");	//buybackWallet
		////tmpSuccess5 = false;
		
		IBEP20(USDT).transfer(payable(marketingWallet), marketingShare);
		IBEP20(USDT).transfer(payable(anothermarketingWallet), anothermarketingShare);
		IBEP20(USDT).transfer(payable(anothermarketingWallet2), anothermarketingShare);
		IBEP20(USDT).transfer(payable(buybackWallet), amountBNBbuyback);
		
 
      

		
		}

        if(amountToLiquify > 0 && amountBNBLiquidity > 0){
			
            // router.addLiquidityETH{value: amountBNBLiquidity}(
                // address(this),
                // amountToLiquify,
                // 0,
                // 0,
                // autoLiquidityReceiver,
                // block.timestamp
            // );


			addLiquidity2(amountBNBLiquidity, amountToLiquify);
			emit AutoLiquify(amountBNBLiquidity, amountToLiquify);

        }

        
		

        }
        
		
		}

        event AutoLiquify(uint256 amountBNB, uint256 amountBOG);
		
	
 
	

}