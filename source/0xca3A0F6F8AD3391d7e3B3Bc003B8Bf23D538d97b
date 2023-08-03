// SPDX-License-Identifier: UNLICENSE
pragma solidity ^0.8.17;

interface IBEP20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _setOwner(_msgSender());
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface IFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IRouter {
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
    
    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

library Address {
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{ value: amount }("");
        require(success, "Address: unable to send value, recipient may have reverted");
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
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
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
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    /**
     * @dev Returns the integer division of two unsigned integers. Reverts on
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
        return div(a, b, "SafeMath: division by zero");
    }

    /**
     * @dev Returns the integer division of two unsigned integers. Reverts with custom message on
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
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts with custom message when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}



interface IDividendDistributor {
    function setDistributionCriteria(uint256 _minPeriod, uint256 _minDistribution) external;
    function setShare(address shareholder, uint256 amount) external;
    function deposit() external payable;
    function process(uint256 gas) external;
}

interface IpresaleAirdrop {
    function airdropPresale(address recipient, uint256 amount) external;
}

contract ManualDividendDistributor is IDividendDistributor {
    using SafeMath for uint256;

    address _token;
    mapping(address => bool) adminAccounts;

    struct Share {
        uint256 amount;
        uint256 totalExcluded;
        uint256 totalRealised;
    }
   
    address WBNB = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
    
    IBEP20 public rewardtoken = IBEP20(WBNB);
    uint public rewardTokenDiv = 10;
    IBEP20 public nativetoken = IBEP20(WBNB);
    uint public nativeTokenDiv = 2;
    uint public totalDiv = 12;


    mapping (address => uint256) totaldividendsOfToken;
    IRouter router;

    address[] public shareholders;
    mapping (address => uint256) shareholderIndexes;
    mapping (address => uint256) shareholderClaims;

    mapping (address => Share) public shares;
    mapping (address => mapping (address => Share)) public rewardshares;

    uint256 public totalShares;
    //uint256 public totalDividends;
    uint256 public totalRewardDistributed;
    uint256 public totalNativeDistributed;
    //uint256 public dividendsPerShare;
    mapping (address => uint256) public dividendsPerShareRewardToken;
    mapping (address => uint256) public totaldividendsrewardtoken;
    uint256 public dividendsPerShareAccuracyFactor = 10**36;

    uint256 public minPeriod = 1 hours;
    uint256 public minDistribution = 1 * (10 ** 18);

    uint256 public currentIndex;
    

    bool initialized = false; // unneccesary as all booleans are initialiased to false;

    modifier initialization() {
        require(!initialized);
        _;
        initialized = true;
    }

    modifier onlyAdmin() {
        require(adminAccounts[msg.sender]); _;
    }

    constructor (address _router) {
        router = _router != address(0)
            ? IRouter(_router)
           : IRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E); //Mainnet
           //  : IRouter(0xD99D1c33F9fC3444f8101754aBC46c52416550D1); //Testnet
        adminAccounts[msg.sender] = true;
        rewardtoken = IBEP20(_token);
    }

    function distributeToken(address[] calldata holders) external onlyAdmin {
        for(uint i = 0; i < holders.length; i++){
            if(shares[holders[i]].amount > 10000){ 
                distributeDividendInToken(holders[i]);
            }
        }
    }
    

    function setDistributionCriteria(uint256 _minPeriod, uint256 _minDistribution) external override onlyAdmin {
        minPeriod = _minPeriod;
        minDistribution = _minDistribution;
    }
    
    function setRewardToken(IBEP20 newrewardToken) external onlyAdmin{
        rewardtoken = newrewardToken;
    }

    function setNativeToken(IBEP20 newnativeToken) external onlyAdmin{
         nativetoken = newnativeToken;
    }
    
    function addAdmin(address adminAddress) public onlyAdmin{
        adminAccounts[adminAddress] = true;
    }
    
    
    function removeAdmin(address adminAddress) public onlyAdmin{
        adminAccounts[adminAddress] = false;
    }
    
    
    function setInitialShare(address shareholder, uint256 amount) external onlyAdmin {
        addShareholder(shareholder);
        totalShares += amount;
        shares[shareholder].amount = amount;
        shares[shareholder].totalExcluded = getCumulativeDividends(shares[shareholder].amount);
    }
    
    function setShareMultiple(address[] calldata addresses, uint256[] calldata amounts) external onlyAdmin
    {
        require(addresses.length == amounts.length, "must have the same length");
        for (uint i = 0; i < addresses.length; i++){
            setShareInternal(addresses[i], amounts[i]*(10**18));
        }
    }

    function getEstimatedNativeTokenForBNB(uint bnbAmount) internal view returns (uint[] memory) {
        address[] memory path = new address[](2);
        path[0] = router.WETH();
        path[1] = address(nativetoken);
        return router.getAmountsOut(bnbAmount, path);
    }

    function getEstimatedRewardTokenForBNB(uint bnbAmount) internal view returns (uint[] memory) {
        address[] memory path = new address[](2);
        path[0] = router.WETH();
        path[1] = address(rewardtoken);
        return router.getAmountsOut(bnbAmount, path);
    }
    
    function setShareInternal(address shareholder, uint256 amount) internal {
        
        if(amount > 0 && shares[shareholder].amount == 0){
            addShareholder(shareholder);
        }else if(amount == 0 && shares[shareholder].amount > 0){
            removeShareholder(shareholder);
        }
        totalShares += (shares[shareholder].amount) + (amount);
        shares[shareholder].amount = amount;
        rewardshares[WBNB][shareholder].totalExcluded = getCumulativeDividends(shares[shareholder].amount);
    }

    function setShare(address shareholder, uint256 amount) external override onlyAdmin {
        
        if(amount > 0 && shares[shareholder].amount == 0){
            addShareholder(shareholder);
        }else if(amount == 0 && shares[shareholder].amount > 0){
            removeShareholder(shareholder);
        }

        totalShares -= (shares[shareholder].amount);
        shares[shareholder].amount = amount;
        totalShares += (amount);
        rewardshares[WBNB][shareholder].totalExcluded = getCumulativeDividends(shares[shareholder].amount);
    }

    function deposit() external payable override {

        totaldividendsOfToken[WBNB] = totaldividendsOfToken[WBNB] + msg.value;
        dividendsPerShareRewardToken[WBNB] = dividendsPerShareRewardToken[WBNB] + (dividendsPerShareAccuracyFactor * (msg.value) / (totalShares));
        
    }

    function process(uint256 gas) external override {
        // this shouldnt be called from outside
    }
    
    function shouldDistribute(address shareholder) internal view returns (bool) {
        return shareholderClaims[shareholder] + minPeriod < block.timestamp
                && getUnpaidEarnings(shareholder) > minDistribution;
    }
    
    function distributeDividendInToken(address shareholder) internal {
        if(shares[shareholder].amount == 0){ return; }

        uint256 amount = getUnpaidEarnings(shareholder);
        if(amount > 0){


            shareholderClaims[shareholder] = block.timestamp;
            
            rewardshares[WBNB][shareholder].totalRealised  += (amount);
            rewardshares[WBNB][shareholder].totalExcluded = getCumulativeDividends(shares[shareholder].amount);

            uint share = amount/totalDiv;
            totalRewardDistributed += getEstimatedRewardTokenForBNB(share * rewardTokenDiv)[1];
            totalNativeDistributed += getEstimatedNativeTokenForBNB(share * nativeTokenDiv)[1];

            uint256 beforeRewardBalance = IBEP20(rewardtoken).balanceOf(shareholder);
            address[] memory pathReward = new address[](2);
            pathReward[0] = address(WBNB);
            pathReward[1] = address(rewardtoken);

            router.swapExactETHForTokensSupportingFeeOnTransferTokens{value: share * rewardTokenDiv}(
                0,
                pathReward,
                shareholder,
                block.timestamp
            );

            uint256 afterRewardBalance = IBEP20(rewardtoken).balanceOf(shareholder).sub(beforeRewardBalance);
            rewardshares[address(rewardtoken)][shareholder].totalRealised  += afterRewardBalance;

            uint256 beforeNativeBalance = IBEP20(nativetoken).balanceOf(shareholder);
            address[] memory pathNative = new address[](2);
            pathNative[0] = address(WBNB);
            pathNative[1] = address(nativetoken);

            router.swapExactETHForTokensSupportingFeeOnTransferTokens{value: share * nativeTokenDiv}(
                0,
                pathNative,
                shareholder,
                block.timestamp
            );

            uint256 afterNativeBalance = IBEP20(nativetoken).balanceOf(shareholder).sub(beforeNativeBalance);
            rewardshares[address(nativetoken)][shareholder].totalRealised  += afterNativeBalance;

        }
    }
    
    function claimDividend() external {
        distributeDividendInToken(msg.sender);
    }

    function setRewardTokensAndPercentages(IBEP20 rewardToken, uint rewardPercent, IBEP20 nativeToken, uint nativePercent)external onlyAdmin{
        rewardtoken = rewardToken;
        rewardTokenDiv = rewardPercent;
        nativetoken = nativeToken;
        nativeTokenDiv = nativePercent;
        totalDiv = nativeTokenDiv + rewardTokenDiv;
    }

    function getUnpaidEarnings(address shareholder) public view returns (uint256) {
        if(shares[shareholder].amount == 0){ return 0; }

        uint256 shareholderTotalDividends = getCumulativeDividends(shares[shareholder].amount);
        uint256 shareholderTotalExcluded = rewardshares[WBNB][shareholder].totalExcluded;

        if(shareholderTotalDividends <= shareholderTotalExcluded){ return 0; }

        return shareholderTotalDividends - (shareholderTotalExcluded);
    }

    function getUnpaidEarningsInTokens(address shareholder) public view returns (uint256[2] memory tokenAmounts) {
        uint256[2] memory retVal;
        retVal[0] = 0;
        retVal[1] = 0;
        if(shares[shareholder].amount == 0){ return retVal; }

        uint256 shareholderTotalDividends = getCumulativeDividends(shares[shareholder].amount);
        uint256 shareholderTotalExcluded = rewardshares[WBNB][shareholder].totalExcluded;

        if(shareholderTotalDividends <= shareholderTotalExcluded){ return retVal; }

        uint amount = shareholderTotalDividends - (shareholderTotalExcluded);
        uint256 share = amount/totalDiv;
        retVal[0] = getEstimatedNativeTokenForBNB(share * nativeTokenDiv)[1];
        retVal[1] = getEstimatedRewardTokenForBNB(share * rewardTokenDiv)[1];
        return retVal;
    }

    function getCumulativeDividends(uint256 share) internal view returns (uint256) {
        return share * dividendsPerShareRewardToken[WBNB] / dividendsPerShareAccuracyFactor;
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

contract BTCBullDog is Context, IBEP20, Ownable {
    using Address for address payable;
    using SafeMath for uint256;

    uint256 public maxBuyTransactionAmount =  1000000  * (10**18);
    uint256 public maxSellTransactionAmount = 1000000 * (10**18);

    mapping(address => bool) public _isBlacklisted;
    mapping(address => bool) public teamWallets;

    mapping(address => uint256) private _rOwned;
    mapping(address => uint256) private _tOwned;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcludedFromFee;
    mapping(address => bool) private _isExcluded;
    mapping (address => bool) isDividendExempt;

    mapping (address => bool) public pairs;

    address[] private _excluded;

    bool private swapping;
    bool public tradingIsEnabled = false;
    bool public rewardDividendEnabled = false;
    bool teamCanTrade = false;

    IRouter public router;
    address public pair;

    ManualDividendDistributor public distributor;

    uint8 private constant _decimals = 18;
    uint256 private constant MAX = ~uint256(0);

    uint256 private _tTotal = 1000000000 * 10**_decimals;
    uint256 private _rTotal = (MAX - (MAX % _tTotal));

    uint256 public swapTokensAtAmount = 20000 * 10**_decimals;
    uint256 public previousDividendRewardsFee = 2;

    event SwapAndLiquify(
        uint256 tokensSwapped,
        uint256 ethReceived,
        uint256 tokensIntoLiqudity,
        bool success
    );

    address BTCB = 0x7130d2A12B9BCbFAe4f2634d864A1Ee1Ce3Ead9c;

    address public deadWallet = 0x000000000000000000000000000000000000dEaD;
    address public marketingWallet = 0x4F97EB360eA1aFC5A0a5e5C1b340400d88d1eceF;
    address public liquidityWallet = 0x4F97EB360eA1aFC5A0a5e5C1b340400d88d1eceF;
    address public nftWallet = 0x34A466A757dCE5ed13aF7F7a09496C7E96de3E01;

    string private constant _name = "BTCBullDog Finance";
    string private constant _symbol = "BitDog Ltd";

    uint256 countMarketingTokens;
    uint256 countLPTokens;
    uint256 countNFTRewardsTokens;
    uint256 countRewardTokens;

    struct BuyTaxes {
        uint256 rfi;
        uint256 marketing;
        uint256 lp;
        uint256 nftRewards;
        uint256 rewardToken;
    }

    struct SellTaxes {
        uint256 rfi;
        uint256 marketing;
        uint256 lp;
        uint256 nftRewards;
        uint256 rewardToken;
    }

    // tax reflection, mkt, lp, nftRewards
    BuyTaxes public taxes = BuyTaxes(0, 0, 0, 0, 0);
    SellTaxes public selltaxes = SellTaxes(2, 2, 2, 2, 0);

    struct TotFeesPaidStruct {
        uint256 rfi;
        uint256 marketing;
        uint256 lp;
        uint256 nftRewards;
        uint256 rewardToken;
    }

    TotFeesPaidStruct public totFeesPaid;

    struct valuesFromGetValues {
        uint256 rAmount;
        uint256 rTransferAmount;
        uint256 rRfi;
        uint256 rMarketing;
        uint256 rNFTRewards;
        uint256 rRewardToken;
        uint256 rLP;
        uint256 tTransferAmount;
        uint256 tRfi;
        uint256 tMarketing;
        uint256 tNFTRewards;
        uint256 tRewardToken;
        uint256 tLP;
    }

    modifier lockTheSwap() {
        swapping = true;
        _;
        swapping = false;
    }

    constructor() {
       IRouter _router = IRouter(address(0x10ED43C718714eb63d5aA57B78B54704E256024E));
        address _pair = IFactory(_router.factory()).createPair(address(this), _router.WETH());

        router = _router;
        pair = _pair;
        pairs[pair] = true;

        distributor = new ManualDividendDistributor(address(router));
        distributor.addAdmin(address(msg.sender));
        distributor.setRewardToken(IBEP20(BTCB));
        distributor.setNativeToken(IBEP20(address(this)));

        excludeFromReward(pair);
        excludeFromReward(deadWallet);

        isDividendExempt[pair] = true;
        isDividendExempt[address(this)] = true;
        isDividendExempt[deadWallet] = true;

        _rOwned[owner()] = _rTotal;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[marketingWallet] = true;
        _isExcludedFromFee[deadWallet] = true;
        emit Transfer(address(0), owner(), _tTotal);
    }

    //std BEP20:
    function name() public pure returns (string memory) {
        return _name;
    }

    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint8) {
        return _decimals;
    }

    //override BEP20:
    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

    function rewardtoken()external view returns(address) {return address(distributor.rewardtoken());}
    function getrewardDistributionTime()external view returns(uint256){return distributor.minPeriod();}
    function getRewardDistributionMinAmount() external view returns(uint256){return distributor.minDistribution();}

    function balanceOf(address account) public view override returns (uint256) {
        if (_isExcluded[account]) return _tOwned[account];
        return tokenFromReflection(_rOwned[account]);
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(currentAllowance >= amount, "BEP20: transfer amount exceeds allowance");
        _approve(sender, _msgSender(), currentAllowance - amount);

        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        returns (bool)
    {
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(currentAllowance >= subtractedValue, "BEP20: decreased allowance below zero");
        _approve(_msgSender(), spender, currentAllowance - subtractedValue);

        return true;
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function isExcludedFromReward(address account) public view returns (bool) {
        return _isExcluded[account];
    }


    function reflectionFromToken(uint256 tAmount, bool deductTransferRfi)
        public
        view
        returns (uint256)
    {
        require(tAmount <= _tTotal, "Amount must be less than supply");
        if (!deductTransferRfi) {
            valuesFromGetValues memory s = _getValues(tAmount, true);
            return s.rAmount;
        } else {
            valuesFromGetValues memory s = _getValues(tAmount, true);
            return s.rTransferAmount;
        }
    }

    function updateMaxSellTransactionAmount(uint256 amount) external onlyOwner {
        maxSellTransactionAmount = amount * 10**_decimals;
    }

    function updateMaxBuyTransactionAmount(uint256 amount) external onlyOwner {
        maxBuyTransactionAmount = amount * 10**_decimals;
    }

    function setMarketMakerPair(address _pair, bool value) public onlyOwner {
      require(pairs[_pair] != value, " Automated market maker pair is already set to that value");
         pairs[_pair] = value;
         excludeFromReward(_pair);
         isDividendExempt[_pair] = true;

    }

    function tokenFromReflection(uint256 rAmount) public view returns (uint256) {
        require(rAmount <= _rTotal, "Amount must be less than total reflections");
        uint256 currentRate = _getRate();
        return rAmount / currentRate;
    }

    //@dev kept original RFI naming -> "reward" as in reflection
    function excludeFromReward(address account) public onlyOwner {
        require(!_isExcluded[account], "Account is already excluded");
        if (_rOwned[account] > 0) {
            _tOwned[account] = tokenFromReflection(_rOwned[account]);
        }
        _isExcluded[account] = true;
        _excluded.push(account);
    }

    function setRewardDividendEnabled(bool _enabled) external onlyOwner {
        if (_enabled == false) {
            previousDividendRewardsFee = taxes.rewardToken;
            taxes.rewardToken = 0;
            selltaxes.rewardToken = 0;
            rewardDividendEnabled = _enabled;
        } else {
            taxes.rewardToken = previousDividendRewardsFee;
            selltaxes.rewardToken = previousDividendRewardsFee;
            rewardDividendEnabled = _enabled;
        }


    }

    function blacklistAddress(address account, bool value) external onlyOwner{
        _isBlacklisted[account] = value;
    }

    function setDistributor(ManualDividendDistributor dist)external onlyOwner {
        distributor = dist;
    }

     function setRewardToken(IBEP20 newrewardToken) external onlyOwner {
        distributor.setRewardToken(newrewardToken);
    }
    
    function revertRewardToken() external onlyOwner {
        distributor.setRewardToken(IBEP20(address(this)));
    }

    function setTradingIsEnabled() external onlyOwner {
        tradingIsEnabled = true;
    }

    function allowTeamtrading() external onlyOwner {
        teamCanTrade = true;
    } 
    
    function addTeamWallet(address wallet) external onlyOwner {
        teamWallets[wallet] = true;
    }

    function includeInReward(address account) external onlyOwner {
        require(_isExcluded[account], "Account is not excluded");
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_excluded[i] == account) {
                _excluded[i] = _excluded[_excluded.length - 1];
                _tOwned[account] = 0;
                _isExcluded[account] = false;
                _excluded.pop();
                break;
            }
        }
    }

    function setIsDividendExempt(address holder, bool exempt) external onlyOwner {
        require(holder != address(this) && !pairs[holder]);
        isDividendExempt[holder] = exempt;
        if(exempt){
            distributor.setShare(holder, 0);
        }else{

            distributor.setShare(holder, balanceOf(holder));

            
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

    function _reflectRfi(uint256 rRfi, uint256 tRfi) private {
        _rTotal -= rRfi;
        totFeesPaid.rfi += tRfi;
    }


    function _takeMarketing(uint256 rMarketing, uint256 tMarketing) private {
        totFeesPaid.marketing += tMarketing;
        countMarketingTokens += tMarketing;

        if (_isExcluded[address(this)]) {
            _tOwned[address(this)] += tMarketing;
        }
        _rOwned[address(this)] += rMarketing;
    }

    function _takeNFTRewards(uint256 rNFTRewards, uint256 tNFTRewards) private {
        totFeesPaid.nftRewards += tNFTRewards;
        countNFTRewardsTokens  += tNFTRewards;

        if (_isExcluded[address(this)]) {
            _tOwned[address(this)] += tNFTRewards;
        }
        _rOwned[address(this)] += rNFTRewards;
    }

    function _takeRewardToken(uint256 rRewardToken, uint256 tRewardToken) private {
        totFeesPaid.rewardToken += tRewardToken;
        countRewardTokens  += tRewardToken;

        if (_isExcluded[address(this)]) {
            _tOwned[address(this)] += tRewardToken;
        }
        _rOwned[address(this)] += rRewardToken;
    }

    function _takeLP(uint256 rLP, uint256 tLP) private {
        totFeesPaid.lp += tLP;
        countLPTokens  += tLP;

        if (_isExcluded[address(this)]) {
            _tOwned[address(this)] += tLP;
        }
        _rOwned[address(this)] += rLP;
    }

    function _getValues(
        uint256 tAmount,
        bool takeFee
    ) private view returns (valuesFromGetValues memory to_return) {
        to_return = _getTValues(tAmount, takeFee);
        (
            to_return.rAmount,
            to_return.rTransferAmount,
            to_return.rRfi,
            to_return.rMarketing,
            to_return.rNFTRewards,
            to_return.rRewardToken,
            to_return.rLP
        ) = _getRValues(to_return, tAmount, takeFee, _getRate());

        return to_return;
    }

    function _getTValues(
        uint256 tAmount,
        bool takeFee
    ) private view returns (valuesFromGetValues memory s) {
        if (!takeFee) {
            s.tTransferAmount = tAmount;
            return s;
        }

        s.tRfi = (tAmount * taxes.rfi) / 100;
        s.tMarketing = (tAmount * taxes.marketing) / 100;
        s.tNFTRewards = (tAmount * taxes.nftRewards) / 100;
        s.tRewardToken = (tAmount * taxes.rewardToken) / 100;
        s.tLP = (tAmount * taxes.lp) / 100;
        s.tTransferAmount =
            tAmount -
            s.tRfi -
            s.tMarketing -
            s.tNFTRewards -
            s.tRewardToken -
            s.tLP;
        return s;
    }

    function _getRValues(
        valuesFromGetValues memory s,
        uint256 tAmount,
        bool takeFee,
        uint256 currentRate
    )
        private
        pure
        returns (
            uint256 rAmount,
            uint256 rTransferAmount,
            uint256 rRfi,
            uint256 rMarketing,
            uint256 rNFTRewards,
            uint256 rRewardToken,
            uint256 rLP
        )
    {
        rAmount = tAmount * currentRate;

        if (!takeFee) {
            return (rAmount, rAmount, 0, 0, 0, 0, 0);
        }

        rRfi = s.tRfi * currentRate;
        rMarketing = s.tMarketing * currentRate;
        rNFTRewards = s.tNFTRewards * currentRate;
        rRewardToken = s.tRewardToken * currentRate;
        rLP = s.tLP * currentRate;
        rTransferAmount =
            rAmount -
            rRfi -
            rMarketing -
            rNFTRewards -
            rRewardToken - 
            rLP;
        return (rAmount, rTransferAmount, rRfi, rMarketing, rNFTRewards, rRewardToken, rLP);
    }

    function _getRate() private view returns (uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply / tSupply;
    }

    function _getCurrentSupply() private view returns (uint256, uint256) {
        uint256 rSupply = _rTotal;
        uint256 tSupply = _tTotal;
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_rOwned[_excluded[i]] > rSupply || _tOwned[_excluded[i]] > tSupply)
                return (_rTotal, _tTotal);
            rSupply = rSupply - _rOwned[_excluded[i]];
            tSupply = tSupply - _tOwned[_excluded[i]];
        }
        if (rSupply < _rTotal / _tTotal) return (_rTotal, _tTotal);
        return (rSupply, tSupply);
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) private {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "BEP20: transfer from the zero address");
        require(to != address(0), "BEP20: transfer to the zero address");
        require(!_isBlacklisted[from] && !_isBlacklisted[to], "Blacklisted address");
        require(tradingIsEnabled || (_isExcludedFromFee[from] || _isExcludedFromFee[to]), "Trading not started");
        require(amount > 0, "Transfer amount must be greater than zero");
        require(
            amount <= balanceOf(from),
            "You are trying to transfer more than your balance"
        );

        if(!teamCanTrade){
            require(!teamWallets[from], "Team Cannot trade!");
        }

        bool isSelling = pairs[to];
        bool isBuying =  pairs[from];

        bool canSwap = balanceOf(address(this)) >= swapTokensAtAmount;

            uint256 oldBuyRFI = taxes.rfi;
            uint256 oldBuyMarketing = taxes.marketing;
            uint256 oldBuyNFTRewards = taxes.nftRewards;
            uint256 oldBuyRewardToken = taxes.rewardToken;
            uint256 oldBuyLP = taxes.lp;

        if(isSelling){
        
            taxes.rfi = selltaxes.rfi;
            taxes.marketing = selltaxes.marketing;
            taxes.nftRewards = selltaxes.nftRewards;
            taxes.rewardToken = selltaxes.rewardToken;
            taxes.lp = selltaxes.lp;
        }

        if (
            isBuying &&
             !_isExcludedFromFee[from] &&
             !_isExcludedFromFee[to]
        ) {
            
            require(amount <= maxBuyTransactionAmount, "maximum buy amount reached.");
        }

        if ( 
            isSelling &&
             !_isExcludedFromFee[from] &&
             !_isExcludedFromFee[to]
        ) {

           require(amount <= maxSellTransactionAmount, "maximum sell amount reached.");
        }

        if (
            !swapping &&
            canSwap &&
            isSelling &&
            !_isExcludedFromFee[from] &&
            !_isExcludedFromFee[to]
        ) {

            swapAndSendMarketingBNB(countMarketingTokens);
            swapAndSendNFTRewardBNB(countNFTRewardsTokens);
            if (rewardDividendEnabled) {
                 swapAndSendRewardTokenBNB(countRewardTokens);
            }
            swapAndLiquify(countLPTokens);
        }
        bool takeFee = true;
        if (swapping || _isExcludedFromFee[from] || _isExcludedFromFee[to]) takeFee = false;

        if (!isSelling && !isBuying) takeFee = false;

        _tokenTransfer(from, to, amount, takeFee);
        
        if(!isDividendExempt[from]){ try distributor.setShare(from, balanceOf(from)) {} catch {} }        
        if(!isDividendExempt[to]){ try distributor.setShare(to, balanceOf(to)) {} catch {} }

        if(isSelling){
           
            taxes.rfi = oldBuyRFI;
            taxes.marketing = oldBuyMarketing;
            taxes.nftRewards = oldBuyNFTRewards;
            taxes.rewardToken = oldBuyRewardToken;
            taxes.lp = oldBuyLP;

        }
    }

    //this method is responsible for taking all fee, if takeFee is true
    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee
    ) private {

        valuesFromGetValues memory s = _getValues(tAmount, takeFee);

        if (_isExcluded[sender]) {
            //from excluded
            _tOwned[sender] = _tOwned[sender] - tAmount;
        }
        if (_isExcluded[recipient]) {
            //to excluded
            _tOwned[recipient] = _tOwned[recipient] + s.tTransferAmount;
        }

        _rOwned[sender] = _rOwned[sender] - s.rAmount;
        _rOwned[recipient] = _rOwned[recipient] + s.rTransferAmount;

        if (s.rRfi > 0 || s.tRfi > 0) _reflectRfi(s.rRfi, s.tRfi);
        if (s.rMarketing > 0 || s.tMarketing > 0) _takeMarketing(s.rMarketing, s.tMarketing);
        if (s.rNFTRewards > 0 || s.tNFTRewards > 0) _takeNFTRewards(s.rNFTRewards, s.tNFTRewards);
        if (s.rRewardToken > 0 || s.tRewardToken > 0) _takeRewardToken(s.rRewardToken, s.tRewardToken);
        if (s.rLP > 0 || s.tLP > 0) _takeLP(s.rLP, s.tLP);
        emit Transfer(sender, recipient, s.tTransferAmount);
    }

    function swapAndSendMarketingBNB(uint256 _tokenAmount) private lockTheSwap {
       
        uint256 contractBalance = balanceOf(address(this));
        
        if(_tokenAmount <= 0){
            return;
        }

        if(_tokenAmount > contractBalance){
            return;
        }

        swapTokensForBNB(_tokenAmount, marketingWallet);
        countMarketingTokens -= _tokenAmount;

    }

    function swapAndSendNFTRewardBNB(uint256 _tokenAmount) private lockTheSwap {

        uint256 contractBalance = balanceOf(address(this));

       if(_tokenAmount <= 0){
            return;
        }

        if(_tokenAmount > contractBalance){
            return;
        }

        swapTokensForBNB(_tokenAmount, nftWallet);
        countNFTRewardsTokens -= _tokenAmount;

    }

    function swapAndSendRewardTokenBNB(uint256 _tokenAmount) private lockTheSwap {

        uint256 contractBalance = balanceOf(address(this));
        uint256 balanceBefore = address(this).balance;

       if(_tokenAmount <= 0){
            return;
        }

        if(_tokenAmount > contractBalance){
            return;
        }

        swapTokensForBNB(_tokenAmount, address(this));
        uint256 amountBNB = address(this).balance.sub(balanceBefore);
         if(amountBNB > 0){
            try distributor.deposit{value: amountBNB}() {} catch {}
        }
        countRewardTokens -= _tokenAmount;

    }

    function swapAndLiquify(uint256 tokens) private {


       if(tokens <= 0){
           emit SwapAndLiquify(0, 0, 0, false);
            return;
        }

        if(tokens > balanceOf(address(this))){
            emit SwapAndLiquify(0, 0, 0, false);
            return;
        }
        
        
        // split the contract balance into halves
        uint256 half = tokens.div(2);
        uint256 otherHalf = tokens.sub(half);
        
        if(half <= 0 || otherHalf <= 0){
            return;
        }

        // capture the contract's current BNB balance.
        // this is so that we can capture exactly the amount of BNB that the
        // swap creates, and not make the liquidity event include any BNB that
        // has been manually sent to the contract
        uint256 initialBalance = address(this).balance;

        // swap tokens for ETH
        swapTokensForBNB(half, payable(address(this)));
        
        countLPTokens -= half;

        // how much ETH did we just swap into?
        uint256 newBalance = address(this).balance.sub(initialBalance);

        // add liquidity to uniswap
        addLiquidity(otherHalf, newBalance);
        
        countLPTokens -= otherHalf;
        
        emit SwapAndLiquify(half, newBalance, otherHalf, true);
    }


      
    function addLiquidity(uint256 tokenAmount, uint256 ethAmount) private {
        // approve token transfer to cover all possible scenarios
        _approve(address(this), address(router), tokenAmount);

        // add the liquidity
        router.addLiquidityETH{value: ethAmount}(
            address(this),
            tokenAmount,
            0, // slippage is unavoidable
            0, // slippage is unavoidable
            liquidityWallet,
            block.timestamp
        );
    }



    function swapTokensForBNB(uint256 tokenAmount, address wallet) private {
        // generate the pair path of token -> weth
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();

        _approve(address(this), address(router), tokenAmount);

        // make the swap
        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of ETH
            path,
            payable(wallet),
            block.timestamp
        );
    }

    function bulkExcludeFee(address[] memory accounts, bool state) external onlyOwner {
        for (uint256 i = 0; i < accounts.length; i++) {
            _isExcludedFromFee[accounts[i]] = state;
        }
    }

    function updateMarketingWallet(address newWallet) external onlyOwner {
        require(newWallet != address(0),"Fee Address cannot be zero address");
        marketingWallet = newWallet;
    }

    function updateNFTWallet(address newWallet) external onlyOwner {
        require(newWallet != address(0),"Fee Address cannot be zero address");
        nftWallet = newWallet;
    }

    function updateSwapTokensAtAmount(uint256 amount) external onlyOwner {
        require(amount <= 10000000, "Cannot set swap threshold amount higher than 1% of tokens");
        swapTokensAtAmount = amount * 10**_decimals;
    }

    function updateBuyTax(uint256 _rfl, uint256 _marketing, uint256 _nftRewards, uint256 _rewardToken, uint256 _lp) external onlyOwner {
        uint256 _totalBuyFees = _rfl.add(_marketing).add(_nftRewards).add(_rewardToken).add(_lp);

        require(_totalBuyFees <= 16, "Cannot be total fees higher than 16%");
        taxes.rfi = _rfl;
        taxes.marketing = _marketing;
        taxes.nftRewards = _nftRewards;
        taxes.rewardToken = _rewardToken;
        taxes.lp = _lp;
   
    }

    function updateSellTax(uint256 _rfl, uint256 _marketing, uint256 _nftRewards, uint256 _rewardToken, uint256 _lp) external onlyOwner {
        uint256 _totalSellFees = _rfl.add(_marketing).add(_nftRewards).add(_rewardToken).add(_lp);

        require(_totalSellFees <= 16, "Cannot be total fees higher than 16%");
        selltaxes.rfi = _rfl;
        selltaxes.marketing = _marketing;
        selltaxes.nftRewards = _nftRewards;
        selltaxes.rewardToken = _rewardToken;
        selltaxes.lp = _lp;

        
    }

    receive() external payable {}
}