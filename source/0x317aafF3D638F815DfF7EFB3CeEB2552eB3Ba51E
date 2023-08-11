// SPDX-License-Identifier: Unlicensed 
// Not open source - Custom Contract Created for HYLAL Token by Gen t.me/GenTokens_GEN TokensByGen.com


pragma solidity 0.8.19;

interface IERC20 {

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint256);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IDividendDistributor {

    function setDistributionCriteria(uint256 _minPeriod, uint256 _minDistribution) external;
    function setShare(address shareholder, uint256 amount) external;
    function deposit() external payable;
    function process(uint256 gas) external;
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IUniswapV2Pair {
    function factory() external view returns (address);
}

interface IUniswapV2Router02 {

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

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
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

}

contract DividendDistributor is IDividendDistributor {

    address _token;

    struct Share {
        uint256 amount;
        uint256 totalExcluded;
        uint256 totalRealised;
    }

    // Reward Token BUSD
    IERC20 RWDTOKEN = IERC20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56);
    
    // WBNB (BSC)
    address WBNB = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;

    IUniswapV2Router02 public DivRouter;

    address[] shareholders;
    mapping (address => uint256) shareholderIndexes;
    mapping (address => uint256) shareholderClaims;

    mapping (address => Share) public shares;

    uint256 public totalShares;
    uint256 public totalDividends;
    uint256 public totalDistributed;
    uint256 public dividendsPerShare;
    uint256 public dividendsPerShareAccuracyFactor = 10 ** 36;

    uint256 public minPeriod = 900; // 15 minutes (900 Seconds)
    uint256 public minDistribution = 1 * (10 ** 17); // $0.10 required for payout

    uint256 currentIndex;

    modifier onlyToken() {
        
        require(msg.sender == _token);
        _;
    }

    constructor () {

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E); 
        DivRouter = _uniswapV2Router;
        _token = msg.sender;
    }



    function Claim_Rewards() external {
        distributeDividend(msg.sender);
    }


    function setDistributionCriteria(uint256 _minPeriod, uint256 _minDistribution) external override onlyToken {
        minPeriod = _minPeriod;
        minDistribution = _minDistribution;
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

        totalShares = totalShares + amount - shares[shareholder].amount;
        shares[shareholder].amount = amount;
        shares[shareholder].totalExcluded = getCumulativeDividends(shares[shareholder].amount);
    }

    function deposit() external payable override onlyToken {

        uint256 balanceBefore = RWDTOKEN.balanceOf(address(this));

        address[] memory path = new address[](2);
        path[0] = WBNB;
        path[1] = address(RWDTOKEN);

        DivRouter.swapExactETHForTokensSupportingFeeOnTransferTokens{value: msg.value}(
            0,
            path,
            address(this),
            block.timestamp
        );

        uint256 amount = RWDTOKEN.balanceOf(address(this)) - balanceBefore;
        totalDividends += amount;
        dividendsPerShare = dividendsPerShare + (dividendsPerShareAccuracyFactor * amount / totalShares);

    }

    function process(uint256 gas) external override onlyToken {

        if (RWDTOKEN.balanceOf(address(this)) == 0) { return; }
       
        uint256 shareholderCount = shareholders.length;

        if(shareholderCount == 0) { return; }

        uint256 gasUsed = 0;
        uint256 gasLeft = gasleft();

        uint256 iterations = 0;

        while(gasUsed < gas && iterations < shareholderCount) {
            if(currentIndex >= shareholderCount){
                currentIndex = 0;
            }

            if(shouldDistribute(shareholders[currentIndex])){
                distributeDividend(shareholders[currentIndex]);
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
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

        if(shares[shareholder].amount == 0){

            return;
        }

        uint256 amount = getUnpaidEarnings(shareholder);

        if(amount > 0){

            totalDistributed += amount;
            shareholderClaims[shareholder] = block.timestamp;
            shares[shareholder].totalRealised += amount;
            shares[shareholder].totalExcluded = getCumulativeDividends(shares[shareholder].amount);
            RWDTOKEN.transfer(shareholder, amount);
        }
    }

    function getUnpaidEarnings(address shareholder) public view returns (uint256) {


        if(shares[shareholder].amount == 0){ return 0; }

        uint256 shareholderTotalDividends = getCumulativeDividends(shares[shareholder].amount);
        uint256 shareholderTotalExcluded = shares[shareholder].totalExcluded;

        if(shareholderTotalDividends <= shareholderTotalExcluded){ return 0; }

        return shareholderTotalDividends - shareholderTotalExcluded;
    }

    function getCumulativeDividends(uint256 share) internal view returns (uint256) {
        return share * dividendsPerShare / dividendsPerShareAccuracyFactor;
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






contract HYLAL is Context, IERC20 { 

    // Contract Wallets
    address private _owner = 0x90F8C0f25Cbd826B3a80d945fe58BD00c55e4c72;  
    address public Wallet_Liquidity = 0x90F8C0f25Cbd826B3a80d945fe58BD00c55e4c72; 
    address public Wallet_Tokens = 0x90F8C0f25Cbd826B3a80d945fe58BD00c55e4c72;
    address payable public Wallet_Marketing = payable(0x90F8C0f25Cbd826B3a80d945fe58BD00c55e4c72); 
    address payable public Wallet_Charity = payable(0x90F8C0f25Cbd826B3a80d945fe58BD00c55e4c72); 

    // Burn Wallet
    address private constant DEAD = 0x000000000000000000000000000000000000dEaD;

    // Token Info
    string private  constant _name = "HYLAL";
    string private  constant _symbol = "HYLAL";
    uint256 private constant _decimals = 18;
    uint256 private constant _tTotal = 200_000_000_000 * 10 ** _decimals;

    // Project Links
    string private _Website;
    string private _Telegram;
    string private _LP_Lock;

    // Limits
    uint256 private max_Hold = _tTotal;
    uint256 private max_Tran = _tTotal;

    // Fees
    uint8 public _Fee__Buy_Charity;
    uint8 public _Fee__Buy_Liquidity;
    uint8 public _Fee__Buy_Marketing;
    uint8 public _Fee__Buy_Rewards;
    uint8 public _Fee__Buy_Tokens;

    uint8 public _Fee__Sell_Charity;
    uint8 public _Fee__Sell_Liquidity;
    uint8 public _Fee__Sell_Marketing;
    uint8 public _Fee__Sell_Rewards;
    uint8 public _Fee__Sell_Tokens;

    // Total Fee for Swap
    uint8 private _SwapFeeTotal_Buy;
    uint8 private _SwapFeeTotal_Sell;

    // Gas Amount
    uint256 distributorGas = 500000;

    // Factory
    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapV2Pair;




    constructor () {

        // Set PancakeSwap Router Address

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);

        // Create Initial Pair With BNB
        uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), _uniswapV2Router.WETH());
        uniswapV2Router = _uniswapV2Router;

        // Create Reward Tracker Contract
        distributor = new DividendDistributor();

        // Set Initial LP Pair
        _isPair[uniswapV2Pair] = true;   

        // Wallets Excluded From Limits
        _isLimitExempt[address(this)] = true;
        _isLimitExempt[DEAD] = true;
        _isLimitExempt[uniswapV2Pair] = true;
        _isLimitExempt[_owner] = true;

        // Wallets With Pre-Launch Access
        _isWhitelisted[_owner] = true;

        // Wallets Excluded From Fees
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[DEAD] = true;
        _isExcludedFromFee[_owner] = true;

        // Wallets Excluded From Rewards
        _isExcludedFromRewards[uniswapV2Pair] = true;
        _isExcludedFromRewards[address(this)] = true;
        _isExcludedFromRewards[_owner] = true;
        _isExcludedFromRewards[DEAD] = true;

        // Transfer Supply To Owner
        _tOwned[_owner] = _tTotal;

        // Emit token transfer to owner
        emit Transfer(address(0), _owner, _tTotal);

        // Emit Ownership Transfer
        emit OwnershipTransferred(address(0), _owner);

    }

    
    // Events
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    event updated_Wallet_Limits(uint256 max_Tran, uint256 max_Hold);
    event updated_Buy_fees(uint8 Marketing, uint8 Liquidity, uint8 Rewards, uint8 Charity, uint8 Tokens);
    event updated_Sell_fees(uint8 Marketing, uint8 Liquidity, uint8 Rewards, uint8 Charity, uint8 Tokens);
    event updated_SwapAndLiquify_Enabled(bool Swap_and_Liquify_Enabled);
    event SwapAndLiquify(uint256 tokensSwapped, uint256 ethReceived, uint256 tokensIntoLiqudity);


    // Restrict Function to Current Owner
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    // Mappings
    mapping (address => uint256) private _tOwned;                               // Tokens Owned
    mapping (address => mapping (address => uint256)) private _allowances;      // Allowance to spend another wallets tokens
    mapping (address => bool) public _isExcludedFromFee;                        // Wallets that do not pay fees
    mapping (address => bool) public _isLimitExempt;                            // Wallets that are excluded from HOLD and TRANSFER limits
    mapping (address => bool) public _isPair;                                   // Address is liquidity pair
    mapping (address => bool) public _isWhitelisted;                            // Pre-Launch Access
    mapping (address => bool) public _isExcludedFromRewards;                    // Excluded from Rewards
    mapping (address => bool) public _isExcludedFromProcess;                    // Excluded from triggering reward processing (Used during airdrops!)

    // Set Distributor
    DividendDistributor public distributor;


    // Public Token Info
    function Token_Information() external view returns(address Owner_Wallet,
                                                       uint256 Transaction_Limit,
                                                       uint256 Max_Wallet,
                                                       uint256 Fee_When_Buying,
                                                       uint256 Fee_When_Selling,
                                                       string memory Website,
                                                       string memory Telegram,
                                                       string memory Liquidity_Lock,
                                                       string memory Custom_Contract_Created_By) {

                                                           
        string memory Creator = "https://tokensbygen.com";

        uint256 Total_buy =  _Fee__Buy_Charity      +
                             _Fee__Buy_Liquidity    +
                             _Fee__Buy_Marketing    +
                             _Fee__Buy_Rewards      +
                             _Fee__Buy_Tokens       ;

        uint256 Total_sell = _Fee__Sell_Charity     +   
                             _Fee__Sell_Liquidity   +
                             _Fee__Sell_Marketing   +
                             _Fee__Sell_Rewards     +
                             _Fee__Sell_Tokens      ;


        uint256 _max_Hold = max_Hold / 10 ** _decimals;
        uint256 _max_Tran = max_Tran / 10 ** _decimals;

        if (_max_Tran > _max_Hold) {

            _max_Tran = _max_Hold;
        }


        // Return Token Data
        return (_owner,
                _max_Tran,
                _max_Hold,
                Total_buy,
                Total_sell,
                _Website,
                _Telegram,
                _LP_Lock,
                Creator);

    }
    

    // Fee Processing Triggers
    uint8 private swapTrigger = 11;
    uint8 private swapCounter = 1;    
    
    // SwapAndLiquify Switch                  
    bool public inSwapAndLiquify;
    bool public swapAndLiquifyEnabled; 

    // Launch Settings
    bool private Trade_Open;
    bool private No_Fee_Transfers = true;

    // Fee Tracker
    bool private takeFee;

    /*
    
    -----------------
    BUY AND SELL FEES
    -----------------

    */


    // Buy Fees
    function Set_Fees_on_Buy(

        uint8 Marketing_on_BUY,
        uint8 Charity_on_BUY,
        uint8 Liquidity_on_BUY, 
        uint8 Rewards_on_BUY, 
        uint8 Tokens_on_BUY

        ) external onlyOwner {

        require (Marketing_on_BUY    +
                 Charity_on_BUY      +
                 Liquidity_on_BUY    + 
                 Rewards_on_BUY      +
                 Tokens_on_BUY       <= 9, "F1"); // Buy fees are limited to 9%

        // Update Fees
        _Fee__Buy_Marketing  = Marketing_on_BUY;
        _Fee__Buy_Charity    = Charity_on_BUY;
        _Fee__Buy_Liquidity  = Liquidity_on_BUY;
        _Fee__Buy_Rewards    = Rewards_on_BUY;
        _Fee__Buy_Tokens     = Tokens_on_BUY;

        // Fees For Processing
        _SwapFeeTotal_Buy    = _Fee__Buy_Marketing + _Fee__Buy_Liquidity + _Fee__Buy_Rewards + _Fee__Buy_Charity;
        emit updated_Buy_fees(_Fee__Buy_Marketing, _Fee__Buy_Liquidity, _Fee__Buy_Rewards, _Fee__Buy_Charity, _Fee__Buy_Tokens);
    }

    // Sell Fees
    function Set_Fees_on_Sell(

        uint8 Marketing_on_SELL,
        uint8 Charity_on_SELL,
        uint8 Liquidity_on_SELL, 
        uint8 Rewards_on_SELL, 
        uint8 Tokens_on_SELL

        ) external onlyOwner {

        require (Marketing_on_SELL  +
                 Charity_on_SELL    +
                 Liquidity_on_SELL  + 
                 Rewards_on_SELL    +
                 Tokens_on_SELL     <= 9, "F2");  // Sell fees are limited to 9%

        // Update Fees
        _Fee__Sell_Marketing  = Marketing_on_SELL;
        _Fee__Sell_Charity    = Charity_on_SELL;
        _Fee__Sell_Liquidity  = Liquidity_on_SELL;
        _Fee__Sell_Rewards    = Rewards_on_SELL;
        _Fee__Sell_Tokens     = Tokens_on_SELL;


        // Fees For Processing
        _SwapFeeTotal_Sell    = _Fee__Sell_Marketing + _Fee__Sell_Liquidity + _Fee__Sell_Rewards + _Fee__Sell_Charity;
        emit updated_Sell_fees(_Fee__Sell_Marketing, _Fee__Sell_Liquidity, _Fee__Sell_Rewards,  _Fee__Sell_Charity, _Fee__Sell_Tokens);
    }


    /*
    
    ------------------------------------------
    SET MAX TRANSACTION AND MAX HOLDING LIMITS
    ------------------------------------------

    To protect buyers, these values must be set to a minimum of 0.5% of the total supply

    Total Supply = 300_000_000_000_000

    Common Percentages 

    0.5% = 1_000_000_000
    1.0% = 2_000_000_000
    1.5% = 3_000_000_000
    2.0% = 4_000_000_000
    2.5% = 5_000_000_000
    3.0% = 6_000_000_000
    100% = 200_000_000_000
    
    */

    function Set_Wallet_Limits(

        uint256 Max_Tokens_Each_Transaction,
        uint256 Max_Total_Tokens_Per_Wallet 

        ) external onlyOwner {

            require(Max_Tokens_Each_Transaction >= 1_000_000_000, "W1"); // Max Transaction must be greater than 0.5% of supply
            require(Max_Total_Tokens_Per_Wallet >= 1_000_000_000, "W2"); // Max Wallet must be greater than 0.5% of supply

            max_Tran = Max_Tokens_Each_Transaction * 10**_decimals;
            max_Hold = Max_Total_Tokens_Per_Wallet * 10**_decimals;

            emit updated_Wallet_Limits(max_Tran, max_Hold);

    }


    // Open Trade
    function Open_Trade() external onlyOwner {

        // Can Only Use Once!
        require(!Trade_Open);

        swapAndLiquifyEnabled = true;
        Trade_Open = true;

    }




    /*
    
    ---------------------------------
    No FEE WALLET TO WALLET TRANSFERS
    ---------------------------------

    Default = true

    Having no fee on wallet-to-wallet transfers means that people can move tokens between wallets, 
    or send them to friends etc without incurring a fee. 

    If false, the 'Buy' fee will apply to all wallet to wallet transfers.

    */

    function No_Fee_Wallet_Transfers(bool true_or_false) public onlyOwner {

        No_Fee_Transfers = true_or_false;

    }




    /*

    -------------
    REWARD TOKENS
    -------------

    */

    function Rewards__Exclude_From_Rewards(address Wallet_Address, bool true_or_false) external onlyOwner {

        require(Wallet_Address != address(this) && Wallet_Address != uniswapV2Pair);
        _isExcludedFromRewards[Wallet_Address] = true_or_false;

        if(true_or_false){

            distributor.setShare(Wallet_Address, 0);

            } else {

            distributor.setShare(Wallet_Address, _tOwned[Wallet_Address]);
        }
    }

    /*


        IMPORTANT: Remember the Decimals - BUSD has 18 decimals!
        100000000000000000 (17 zeroes) = $0.10

    */

    function Rewards__Distribution_Triggers(uint256 Minutes_Between_Payments, uint256 Required_Reward_Balance) external onlyOwner {

        // Max Wait is 2 Days
        require(Minutes_Between_Payments <= 2880,"R1"); // Can not set wait time to longer than 2 days!

        // Convert minutes to seconds
        uint256 _minPeriod = Minutes_Between_Payments * 60;

        distributor.setDistributionCriteria(_minPeriod, Required_Reward_Balance);

    }


    function Rewards__Set_Gas(uint256 Gas_Amount) external onlyOwner {

        require(Gas_Amount < 750000);
        distributorGas = Gas_Amount;

    }
    

    /*

    ----------------------
    UPDATE PROJECT WALLETS
    ----------------------

    */


    function Update_Wallet_Charity(

        address payable Charity_Wallet

        ) external onlyOwner {

        // Update Charity Fee Wallet
        require(Charity_Wallet != address(0), "W1"); // Enter a valid BSC Address
        Wallet_Charity = payable(Charity_Wallet);
        
    }

    function Update_Wallet_Liquidity(

        address Liquidity_Collection_Wallet

        ) external onlyOwner {

        // Update LP Collection Wallet
        require(Liquidity_Collection_Wallet != address(0), "W2"); // Enter a valid BSC Address
        Wallet_Liquidity = Liquidity_Collection_Wallet;

    }

    function Update_Wallet_Marketing(

        address payable Marketing_Wallet

        ) external onlyOwner {

        // Update Marketing Fee Wallet
        require(Marketing_Wallet != address(0), "W3"); // Enter a valid BSC Address
        Wallet_Marketing = payable(Marketing_Wallet);

    }

    function Update_Wallet_Tokens(

        address Token_Wallet

        ) external onlyOwner {

        // Update Token Fee Wallet
        require(Token_Wallet != address(0), "W4"); // Enter a valid BSC Address
        Wallet_Tokens = Token_Wallet;

    }

    /*

    --------------------
    UPDATE PROJECT LINKS
    --------------------

    */

    function Update_Links_Website(

        string memory Website_URL

        ) external onlyOwner{

        _Website = Website_URL;

    }


    function Update_Links_Telegram(

        string memory Telegram_URL

        ) external onlyOwner{

        _Telegram = Telegram_URL;

    }


    function Update_Links_Liquidity_Lock(

        string memory LP_Lock_URL

        ) external onlyOwner{

        _LP_Lock = LP_Lock_URL;

    }


 
    /*
    
    ----------------------
    ADD NEW LIQUIDITY PAIR
    ----------------------

    */

    // Add Liquidity Pair
    function Maintenance__Add_Liquidity_Pair(

        address Wallet_Address,
        bool true_or_false)

         external onlyOwner {
        _isPair[Wallet_Address] = true_or_false;
        _isLimitExempt[Wallet_Address] = true_or_false;
    } 



    /* 

    ----------------------------
    CONTRACT OWNERSHIP FUNCTIONS
    ----------------------------

    */


    // Transfer to New Owner
    function Ownership_TRANSFER(address payable newOwner) public onlyOwner {
        require(newOwner != address(0), "O1"); // Enter a valid BSC Address

        emit OwnershipTransferred(_owner, newOwner);

        // Remove old owner status 
        _isLimitExempt[owner()]     = false;
        _isExcludedFromFee[owner()] = false;
        _isWhitelisted[owner()]     = false;

        _owner = newOwner;

        // Add new owner status
        _isLimitExempt[owner()]     = true;
        _isExcludedFromFee[owner()] = true;
        _isWhitelisted[owner()]     = true;

    }

  
    // Renounce Ownership
    function Ownership_RENOUNCE() public virtual onlyOwner {

        emit OwnershipTransferred(_owner, address(0));

        // Remove old owner status 
        _isLimitExempt[owner()]     = false;
        _isExcludedFromFee[owner()] = false;
        _isWhitelisted[owner()]     = false;

        _owner = address(0);
    }




    /*

    --------------
    FEE PROCESSING
    --------------

    */


    // Auto Fee Processing Switch
    function Process__Auto(bool true_or_false) external onlyOwner {
        swapAndLiquifyEnabled = true_or_false;
        emit updated_SwapAndLiquify_Enabled(true_or_false);
    }

    // Manually Process Fees
    function Process__Manual(uint256 Percent_of_Tokens_to_Process) external onlyOwner {
        require(!inSwapAndLiquify, "P1"); // Already processing fees!
        if (Percent_of_Tokens_to_Process > 100){Percent_of_Tokens_to_Process == 100;}
        uint256 tokensOnContract = balanceOf(address(this));
        uint256 sendTokens = tokensOnContract * Percent_of_Tokens_to_Process / 100;
        swapAndLiquify(sendTokens);

    }

    // Remove Random Tokens - number_of_Tokens must include the decimals too!
    function Process__Rescue_Tokens(

        address random_Token_Address,
        uint256 number_of_Tokens

        ) external onlyOwner {

            // Can Not Remove Native Token
            require (random_Token_Address != address(this), "P2"); // Can not purge the native token - Must be processed as fees!
            IERC20(random_Token_Address).transfer(msg.sender, number_of_Tokens);
            
    }

    // Update Swap Count Trigger
    function Process__Trigger_Count(uint8 Transaction_Count) external onlyOwner {

        // To Save Gas, Start At 1 Not 0
        swapTrigger = Transaction_Count + 1;
    }




    /*

    ---------------
    WALLET SETTINGS
    ---------------

    */


    // Exclude From Fees
    function Wallet__Exclude_From_Fees(

        address Wallet_Address,
        bool true_or_false

        ) external onlyOwner {
        _isExcludedFromFee[Wallet_Address] = true_or_false;

    }

    // When the 'from' wallet is excluded from processing token transfers will not trigger reward contract processing
    function Wallet__Exclude_From_Processing(

        address Wallet_Address,
        bool true_or_false

        ) external onlyOwner {
        _isExcludedFromProcess[Wallet_Address] = true_or_false;

    }

    // Exclude From Transaction and Holding Limits
    function Wallet__Exempt_From_Limits(

        address Wallet_Address,
        bool true_or_false

        ) external onlyOwner {  
        _isLimitExempt[Wallet_Address] = true_or_false;
    }

    // Grant Pre-Launch Access (Whitelist)
    function Wallet__Pre_Launch_Access(

        address Wallet_Address,
        bool true_or_false

        ) external onlyOwner {    
        _isWhitelisted[Wallet_Address] = true_or_false;
    }


    

  




    /*

    -----------------------------
    BEP20 STANDARD AND COMPLIANCE
    -----------------------------

    */

    function owner() public view returns (address) {
        return _owner;
    }

    function name() public pure returns (string memory) {
        return _name;
    }

    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint256) {
        return _decimals;
    }

    function totalSupply() public pure override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _tOwned[account];
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        _approve(_msgSender(), spender, currentAllowance - subtractedValue);

        return true;
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }
    
    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
        _approve(sender, _msgSender(), currentAllowance - amount);

        return true;
    }

    function send_BNB(address _to, uint256 _amount) internal returns (bool SendSuccess) {
        (SendSuccess,) = payable(_to).call{value: _amount}("");
    }

    function getCirculatingSupply() public view returns (uint256) {
        return (_tTotal - balanceOf(address(DEAD)));
    }






   


    /*

    ---------------
    TOKEN TRANSFERS
    ---------------

    */

    function _transfer(
        address from,
        address to,
        uint256 amount
      ) private {



        require(balanceOf(from) >= amount, "T1"); // Sender does not have enough tokens!

        if (!Trade_Open){
        require(_isWhitelisted[from] || _isWhitelisted[to], "T2"); // Trade is not open - Only whitelisted wallets can interact with tokens
        }

        // Wallet Limit
        if (!_isLimitExempt[to]) {

            uint256 heldTokens = balanceOf(to);
            require((heldTokens + amount) <= max_Hold, "T3"); // Purchase would take balance of max permitted
            
        }

        // Transaction limit - To send over the transaction limit the sender AND the recipient must be limit exempt
        if (!_isLimitExempt[to] || !_isLimitExempt[from]) {

            require(amount <= max_Tran, "T4"); // Exceeds max permitted transaction amount
        
        }


        // Compliance and Safety Checks
        require(from != address(0), "T5"); // Enter a valid BSC Address
        require(to != address(0), "T6"); // Enter a valid BSC Address
        require(amount > 0, "T7"); // Amount of tokens can not be 0


        // Check Fee Status
        if(!takeFee){
            
            takeFee = true;
        }


        // Trigger Fee Processing
        if (_isPair[to] && !inSwapAndLiquify && swapAndLiquifyEnabled) {

            // Check Transaction Count
            if(swapCounter >= swapTrigger){

                // Check Contract Tokens
                uint256 contractTokens = balanceOf(address(this));

                if (contractTokens > 0) {

                    // Remove the fee for wallet that triggers processing to avoid inflated sell gas on charts
                    takeFee = false;

                    // Limit Swap to Max Transaction
                    if (contractTokens <= max_Tran) {

                        swapAndLiquify (contractTokens);

                        } else {

                        swapAndLiquify (max_Tran);

                    }
                }
            }  
        }



        if(takeFee){
            
            if(_isExcludedFromFee[from] || _isExcludedFromFee[to] || (No_Fee_Transfers && !_isPair[to] && !_isPair[from])){
                takeFee = false;
            }
        }

        _tokenTransfer(from, to, amount, takeFee);


            // Distribute Rewards
            if(!_isExcludedFromRewards[from]) {
                try distributor.setShare(from, _tOwned[from]) {} catch {}
            }

            if(!_isExcludedFromRewards[to]) {
                try distributor.setShare(to, _tOwned[to]) {} catch {} 
            }

            if(!_isExcludedFromProcess[from]) {
                try distributor.process(distributorGas) {} catch {}
            }

    }


    /*
    
    ------------
    PROCESS FEES
    ------------

    */

    function swapAndLiquify(uint256 Tokens) private {

        // Lock Swap
        inSwapAndLiquify        = true;  

        // Calculate Tokens for Swap
        uint256 _FeesTotal      = _SwapFeeTotal_Buy + _SwapFeeTotal_Sell;
        uint256 LP_Tokens       = Tokens * (_Fee__Buy_Liquidity + _Fee__Sell_Liquidity) / _FeesTotal / 2;
        uint256 Swap_Tokens     = Tokens - LP_Tokens;

        // Swap Tokens
        uint256 contract_BNB    = address(this).balance;
        swapTokensForBNB(Swap_Tokens);
        uint256 returned_BNB    = address(this).balance - contract_BNB;

        // Avoid Rounding Errors on LP Fee if Odd Number
        uint256 fee_Split       = _FeesTotal * 2 - (_Fee__Buy_Liquidity + _Fee__Sell_Liquidity);

        // Calculate BNB Values
        uint256 BNB_Liquidity   = returned_BNB * (_Fee__Buy_Liquidity   + _Fee__Sell_Liquidity)       / fee_Split;
        uint256 BNB_Rewards     = returned_BNB * (_Fee__Buy_Rewards     + _Fee__Sell_Rewards)     * 2 / fee_Split; 
        uint256 BNB_Charity     = returned_BNB * (_Fee__Buy_Charity     + _Fee__Sell_Charity)     * 2 / fee_Split; 

        // Add Liquidity 
        if (BNB_Liquidity > 0){
            addLiquidity(LP_Tokens, BNB_Liquidity);
            emit SwapAndLiquify(LP_Tokens, BNB_Liquidity, LP_Tokens);
        }

        // Deposit Rewards
        if(BNB_Rewards > 0){

            try distributor.deposit{value: BNB_Rewards}() {} catch {}

        }

        // Deposit Charity 
        if (BNB_Charity > 0){

            send_BNB(Wallet_Charity, BNB_Charity);
        }
        

        
        // Deposit Marketing
        contract_BNB = address(this).balance;

        if (contract_BNB > 0){

            send_BNB(Wallet_Marketing, contract_BNB);
        }


        // Reset Counter
        swapCounter = 1;

        // Unlock Swap
        inSwapAndLiquify = false;


    }

    // Swap Tokens
    function swapTokensForBNB(uint256 tokenAmount) private {

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapV2Router.WETH();
        _approve(address(this), address(uniswapV2Router), tokenAmount);
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0, 
            path,
            address(this),
            block.timestamp
        );
    }

    // Add Liquidity
    function addLiquidity(uint256 tokenAmount, uint256 BNBAmount) private {

        _approve(address(this), address(uniswapV2Router), tokenAmount);
        uniswapV2Router.addLiquidityETH{value: BNBAmount}(
            address(this),
            tokenAmount,
            0, 
            0,
            Wallet_Liquidity, 
            block.timestamp
        );
    } 



    /*
    
    ----------------------------------
    TRANSFER TOKENS AND CALCULATE FEES
    ----------------------------------

    */

    uint256 private tSwapFeeTotal;
    uint256 private tTransferAmount;
    uint256 private tTokenFeeAmount;

    // Transfer Tokens and Calculate Fees
    function _tokenTransfer(address sender, address recipient, uint256 tAmount, bool Fee) private {

        
        if (Fee){

            if(_isPair[recipient]){

                // Sell Fees
                tSwapFeeTotal   = tAmount * _SwapFeeTotal_Sell / 100;
                tTokenFeeAmount = tAmount * _Fee__Sell_Tokens / 100;


            } else {

                // Buy Fees
                tSwapFeeTotal   = tAmount * _SwapFeeTotal_Buy / 100;
                tTokenFeeAmount = tAmount * _Fee__Buy_Tokens / 100;

            }

        } else {

                // No Fees
                tSwapFeeTotal   = 0;
                tTokenFeeAmount = 0;

        }

        tTransferAmount = tAmount - (tSwapFeeTotal + tTokenFeeAmount);

        // Remove Tokens from Sender
        _tOwned[sender] -= tAmount;

        // Send tokens to recipient
        _tOwned[recipient] += tTransferAmount;

        emit Transfer(sender, recipient, tTransferAmount);

        // Take the token fee and send to token fee collection wallet
        if(tTokenFeeAmount > 0){

            _tOwned[Wallet_Tokens] += tTokenFeeAmount;
            emit Transfer(sender, Wallet_Tokens, tTokenFeeAmount);
        }

        // Take Fees for BNB Processing
        if(tSwapFeeTotal > 0){

            _tOwned[address(this)] += tSwapFeeTotal;
            emit Transfer(sender, address(this), tSwapFeeTotal);

            // Increase Transaction Counter
            if (swapCounter < swapTrigger){
                unchecked{swapCounter++;}
            }
                
        }

    }

    // This function is required so that the contract can receive BNB during fee processing
    receive() external payable {}

}

// Custom contract by GEN https://TokensByGEN.com TG: https://t.me/GenTokens_GEN
// Not open source - Can not be used or forked without permission.