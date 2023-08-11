// SPDX-License-Identifier: Unlicensed 
// Not open source - Do Not Copy

/* 

    Contract Created at https://TokensByGen.com

    For Project Website, Telegram Group, and LP Lock URL, check "Token_Information" on the BSCScan Contract Read Page 

    SNIPER PROTECTION

        Early Buyers (First 5 seconds) are automatically blocked from selling for 10 minutes
        Blacklist Option for initial 10 minutes only

    FEE OPTIONS

        BNB Marketing Wallet
        Burn Fee (With Option to remove burned tokens from total supply)
        Auto Liquidity
        Buy Fees Capped at 15%
        Sell Fees Capped at 15%
        Immutable GEN Rewards on Buys and Sells Hard-Coded to 5%
    
    BUYER PROTECTION

        Blacklisting is NOT Possible after initial 10 minutes
        Minimum Transaction Limit 0.5%
        Minimum Wallet Holding Limit 0.5%
        Trade can not be paused

    DISCLAIMER

        This token was created at tokensbygen.com 
        Anybody can create a token using our token factory
        Do your own research before investing in this project

    GEN REWARDS

        By holding this token you will receive 5% GEN rewards on every transaction
        GEN is the flagship token of the GenTokens Community
        For more information on the GEN project visit gentokens.com

*/


pragma solidity 0.8.19;


interface IERC20 {
    
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
}

interface IDividendDistributor {

    function setDistributionCriteria(uint256 _minPeriod, uint256 _minDistribution) external;
    function setShare(address shareholder, uint256 amount) external;
    function deposit() external payable;
    function process(uint256 gas) external;
}

interface IRewardToken {

    function checkRewardToken() external view returns (address); 
}

interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IUniswapV2Pair {
    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    
    function factory() external view returns (address);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
}

interface IUniswapV2Router01 {

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

}

interface IUniswapV2Router02 is IUniswapV2Router01 {

   
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


// Factory contract 
contract GEN_REWARDS_FACTORY {

    function CreateToken(string memory Token_Name, 
                         string memory Token_Symbol, 
                         uint256 Total_Supply, 
                         uint256 Number_Of_Decimals) public {

        // Min of two, max of 18 decimals required
        require(Number_Of_Decimals >= 2 && Number_Of_Decimals <= 18, "DEC"); // Decimals limit is 2 to 18

    new GEN_REWARDS_TOKEN(Token_Name,
                      Token_Symbol,
                      Total_Supply, 
                      Number_Of_Decimals,
                      payable(msg.sender));
    
    }

}







contract GEN_REWARDS_TOKEN is IERC20 { 

    // Contract Wallets
    address public _owner;
    address public Wallet_Liquidity;
    address payable public Wallet_Marketing;
    address public RWD_Token = address(0x5a457fb51bD0F50df9eC9F3d0aeC04CD4B9Ce0BF);
    address public constant Wallet_Burn = 0x000000000000000000000000000000000000dEaD;

    // Token Info
    string private  _name;
    string private  _symbol;
    uint256 private _decimals;
    uint256 private _tTotal;

    // Project Links
    string private _website;
    string private _telegram;
    string private _lplock;

    // Limits
    uint256 private max_Hold;
    uint256 private max_Tran;

    // Fees
    uint8 public _fee__Buy_Liquidity;
    uint8 public _fee__Buy_Marketing;
    uint8 public _fee__Buy_Burn;
    uint8 public constant _fee__Buy_Rewards = 5;

    uint8 public _fee__Sell_Liquidity;
    uint8 public _fee__Sell_Marketing;
    uint8 public _fee__Sell_Burn;
    uint8 public constant _fee__Sell_Rewards = 5;

    // Total Fee for Swap
    uint256 private _SwapFeeTotal_Buy = 5;
    uint256 private _SwapFeeTotal_Sell = 5;

    // Gas Amount
    uint256 distributorGas = 500000;

    // Factory
    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapV2Pair;

    constructor (string memory      _TokenName, 
                 string memory      _TokenSymbol,  
                 uint256            _TotalSupply, 
                 uint256            _Decimals,
                 address payable    _OwnerWallet) {

    emit TokenCreated(address(this));

    // Owner
    _owner              = _OwnerWallet;

    // Token Info
    _name               = _TokenName;
    _symbol             = _TokenSymbol;
    _decimals           = _Decimals;
    _tTotal             = _TotalSupply * 10 ** _decimals;

    // Wallet Limits
    max_Hold            = _tTotal;
    max_Tran            = _tTotal;

    // Project Wallets Set to Owner
    Wallet_Marketing    = payable(_OwnerWallet);
    Wallet_Liquidity    = _OwnerWallet;

    // Transfer Supply To Owner
    _tOwned[_owner]     = _tTotal;

    // Set PancakeSwap Router Address  
    IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);


    // Create Initial Pair With BNB
    uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), _uniswapV2Router.WETH());
    uniswapV2Router = _uniswapV2Router;

    // Create Reward Tracker Contract
    distributor = new Reward_GEN(RWD_Token);

    // Set Initial LP Pair
    _isPair[uniswapV2Pair] = true;   

    // Wallets Excluded From Limits
    _isLimitExempt[address(this)] = true;
    _isLimitExempt[uniswapV2Pair] = true;
    _isLimitExempt[_owner] = true;
    _isLimitExempt[Wallet_Burn] = true;

    // Wallets With Pre-Launch Access
    _isWhitelisted[_owner] = true;

    // Wallets Excluded From Fees
    _isExcludedFromFee[address(this)] = true;
    _isExcludedFromFee[_owner] = true;
    _isExcludedFromFee[Wallet_Burn] = true;

    // Wallets Excluded From Rewards
    _isExcludedFromRewards[uniswapV2Pair] = true;
    _isExcludedFromRewards[address(this)] = true;
    _isExcludedFromRewards[_owner] = true;
    _isExcludedFromRewards[Wallet_Burn] = true;

    // Emit Supply Transfer to Contract
    emit Transfer(address(0), _owner, _tTotal);

    // Emit Ownership Transfer
    emit OwnershipTransferred(address(0), _owner);

    }

    
    // Events
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    event updated_Wallet_Limits(uint256 max_Tran, uint256 max_Hold);
    event updated_Buy_fees(uint8 Marketing, uint8 Liquidity, uint8 Burn, uint8 Rewards);
    event updated_Sell_fees(uint8 Marketing, uint8 Liquidity, uint8 Burn, uint8 Rewards);
    event updated_SwapAndLiquify_Enabled(bool Swap_and_Liquify_Enabled);
    event SwapAndLiquify(uint256 tokensSwapped, uint256 ethReceived, uint256 tokensIntoLiqudity);
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);
    event TokenCreated(address indexed Token_CA);


    // Restrict Function to Current Owner
    modifier onlyOwner() {
        require(owner() == msg.sender, "Ownable: caller is not the owner");
        _;
    }



    // Mappings
    mapping (address => uint256) private _tOwned;                               // Tokens Owned
    mapping (address => mapping (address => uint256)) private _allowances;      // Allowance to spend another wallets tokens
    mapping (address => bool) public _isExcludedFromFee;                        // Wallets that do not pay fees
    mapping (address => bool) public _isLimitExempt;                            // Wallets that are excluded from HOLD and TRANSFER limits
    mapping (address => bool) public _isPair;                                   // Address is liquidity pair
    mapping (address => bool) public _EarlyBuyer;                               // Early Buyers 
    mapping (address => bool) public _isWhitelisted;                            // Pre-Launch Access
    mapping (address => bool) public _isBlacklisted;                            // Blacklisted wallets
    mapping (address => bool) public _isExcludedFromRewards;                    // Excluded from Rewards

    // Set Distributor
    Reward_GEN public distributor;


    // Public Token Info
    function Token_Information() external view returns(address Owner_Wallet,
                                                       address Rewards_Token,
                                                       uint256 Transaction_Limit,
                                                       uint256 Max_Wallet,
                                                       uint256 Fee_When_Buying,
                                                       uint256 Fee_When_Selling,
                                                       string memory Website,
                                                       string memory Telegram,
                                                       string memory LP_Lock,
                                                       string memory Contract_Created_By) {

                                                           
        string memory Creator = "https://tokensbygen.com";

        uint256 Total_buy =  _fee__Buy_Liquidity    +
                             _fee__Buy_Marketing    +
                             _fee__Buy_Rewards      +
                             _fee__Buy_Burn         ;

        uint256 Total_sell = _fee__Sell_Liquidity   +
                             _fee__Sell_Marketing   +
                             _fee__Sell_Rewards     +
                             _fee__Sell_Burn        ;


        uint256 _max_Hold = max_Hold / 10 ** _decimals;
        uint256 _max_Tran = max_Tran / 10 ** _decimals;

        if (_max_Tran > _max_Hold) {

            _max_Tran = _max_Hold;
        }


        // Return Token Data
        return (_owner,
                RWD_Token,
                _max_Tran,
                _max_Hold,
                Total_buy,
                Total_sell,
                _website,
                _telegram,
                _lplock,
                Creator);

    }
    

    // Fee Processing Triggers
    uint256 private swapTrigger = 10; 
    uint256 private swapCounter = 1;    
    
    // SwapAndLiquify Switch                  
    bool public processingFees;
    bool public feeProcessingEnabled;
    bool public noFeeWhenProcessing;

    // Launch Settings
    uint256 private LaunchTime;
    bool private LaunchMode;
    bool private Trade_Open;
    bool private BlacklistWallets = true;
    bool private No_Fee_Transfers = true;

    // Burn option (send to DEAD or remove from supply)
    bool public burnFromSupply;

    // Fee Tracker
    bool private takeFee;


    /*

    ------------------------
    SET UP PRE-SALE CONTRACT
    ------------------------

    */

    function F_01_Add_PreSale_Address(address PreSale_Contract_Address) external onlyOwner {

        _isExcludedFromRewards[PreSale_Contract_Address] = true;
        _isExcludedFromFee[PreSale_Contract_Address] = true;
        _isLimitExempt[PreSale_Contract_Address] = true;
        _isWhitelisted[PreSale_Contract_Address] = true;

    }



    /*
    
    -----------------
    BUY AND SELL FEES
    -----------------

    */


    // Buy Fees
    function F_02_Set_Fees(

        uint8 Marketing_on_BUY, 
        uint8 Liquidity_on_BUY,
        uint8 Burn_on_BUY,
        uint8 Marketing_on_SELL,
        uint8 Liquidity_on_SELL,
        uint8 Burn_on_SELL

        ) external onlyOwner {

        // Buyer Protection: Max Fee 15% 
        require (Marketing_on_BUY + Liquidity_on_BUY + Burn_on_BUY <= 10, "SF1");  // Max fee 15% (Includes 5% Rewards)

        // Buyer Protection: Max Fee 15% (Includes 5% Rewards)
        require (Marketing_on_SELL + Liquidity_on_SELL + Burn_on_SELL <= 10, "SF2");  // Max fee 15% (Includes 5% Rewards)

        // Update Fees
        _fee__Buy_Marketing  = Marketing_on_BUY;
        _fee__Buy_Liquidity  = Liquidity_on_BUY;
        _fee__Buy_Burn       = Burn_on_BUY;

        _fee__Sell_Marketing = Marketing_on_SELL;
        _fee__Sell_Liquidity = Liquidity_on_SELL;
        _fee__Sell_Burn      = Burn_on_SELL;

        // Fees For Processing
        _SwapFeeTotal_Sell   = _fee__Sell_Marketing + _fee__Sell_Liquidity + _fee__Sell_Rewards;
        _SwapFeeTotal_Buy    = _fee__Buy_Marketing + _fee__Buy_Liquidity + _fee__Buy_Rewards;
        emit updated_Buy_fees(_fee__Buy_Marketing, _fee__Buy_Liquidity, _fee__Buy_Burn, _fee__Buy_Rewards);
        emit updated_Sell_fees(_fee__Sell_Marketing, _fee__Sell_Liquidity, _fee__Sell_Burn, _fee__Sell_Rewards);
    
    }


    /*

    -------------
    ADD LIQUIDITY
    -------------

    USE V2 WHEN ADDING LIQUIDITY ON PANCAKESWAP
    V3 DOES NOT SUPPORT TOKENS WITH FEE ON TRANSFER!
    THE INITIAL LIQUIDITY PAIR MUST BE WITH BNB 

    https://pancakeswap.finance/liquidity

    */



    /*
    
    ------------------------------------------
    SET MAX TRANSACTION AND MAX HOLDING LIMITS
    ------------------------------------------

    Solidity can only accept whole numbers. 
    If you want to set a limit to 0.5% enter 0 (This is the minimum permitted value)

    */

    function F_03_Set_Wallet_Limits(

        uint256 Max_Transaction_Percent,
        uint256 Max_Wallet_Percent

        ) external onlyOwner {

        if (Max_Transaction_Percent < 1){

            // Defaults to 0.5% if 0 is entered
            max_Tran = _tTotal / 200;

        } else {

            max_Tran = _tTotal * Max_Transaction_Percent / 100;

        }


        if (Max_Wallet_Percent < 1){

            // Defaults to 0.5% if 0 is entered
            max_Hold = _tTotal / 200;

        } else {

            max_Hold = _tTotal * Max_Wallet_Percent / 100;

        }

        emit updated_Wallet_Limits(max_Tran, max_Hold);

    }

    // Open Trade
    function F_04_Open_Trade() external onlyOwner {

        // Can Only Use Once!
        require(!Trade_Open);

        feeProcessingEnabled = true;
        LaunchTime = block.timestamp;
        LaunchMode = true;
        Trade_Open = true;

    }

    /* 

    -----------------------------------------
    BLACKLIST BOTS - DURING LAUNCH MODE ONLY!
    -----------------------------------------

    */
    
    function F_05_Blacklist_Bots(address Wallet, bool true_or_false) external onlyOwner {
        
        if (true_or_false) {

            require(BlacklistWallets, "BL1"); // Can no longer blacklist!
        }

        _isBlacklisted[Wallet] = true_or_false;

    }


    // Deactivate Launch Mode
    function F_06_End_Launch_Mode() external onlyOwner {

        LaunchMode = false;
        BlacklistWallets = false;

    }


    /*

    --------------------
    UPDATE PROJECT LINKS
    --------------------

    */

    function F_07_Update_Links_Website(

        string memory Website_URL

        ) external onlyOwner{

        _website = Website_URL;

    }

    function F_08_Update_Links_Telegram(

        string memory Telegram_Group

        ) external onlyOwner{

        _telegram = Telegram_Group;

    }



    function F_09_Update_Links_LP_Lock(

        string memory LP_Lock_URL

        ) external onlyOwner{

        _lplock = LP_Lock_URL;

    }





    // Add Liquidity Pair - required for correct fee calculations 
    function M_01_Add_Liquidity_Pair(

        address Wallet_Address,
        bool true_or_false)

        external onlyOwner {

        _isPair[Wallet_Address] = true_or_false;
        _isLimitExempt[Wallet_Address] = true_or_false;
        _isExcludedFromRewards[Wallet_Address] = true;

    } 

    /* 

    ----------------------------
    CONTRACT OWNERSHIP FUNCTIONS
    ----------------------------

    Before renouncing ownership, set the freeWalletTransfers to false 

    */
  
    // Renounce Ownership - To prevent accidental renounce, you must enter the Confirmation_Code: 1234
    function M_02_Ownership_RENOUNCE(uint256 Confirmation_Code) public virtual onlyOwner {

        require(Confirmation_Code == 1234, "E12"); // Renounce confirmation not correct

        // Remove old owner status 
        _isLimitExempt[owner()]     = false;
        _isExcludedFromFee[owner()] = false;
        _isWhitelisted[owner()]     = false;

        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    // Transfer to New Owner - To prevent accidental renounce, you must enter the Confirmation_Code: 1234
    function M_03_Ownership_TRANSFER(address payable newOwner, uint256 Confirmation_Code) public onlyOwner {

        require(Confirmation_Code == 1234, "E12"); // Renounce confirmation not correct
        require(newOwner != address(0), "E13"); // Enter a valid BSC wallet

        // Revoke old owner status
        _isLimitExempt[owner()]     = false;
        _isExcludedFromFee[owner()] = false;
        _isWhitelisted[owner()]     = false;

        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;

        // Set up new owner status 
        _isLimitExempt[owner()]     = true;
        _isExcludedFromFee[owner()] = true;
        _isWhitelisted[owner()]     = true;

    }



    /*
    
    ---------------------------------
    NO FEE WALLET TO WALLET TRANSFERS
    ---------------------------------

    Default = true

    Having no fee on wallet-to-wallet transfers means that people can move tokens between wallets, 
    or send them to friends etc without incurring a fee. 

    If false, the 'Buy' fee will apply to all wallet to wallet transfers.

    */

    function O_01_No_Fee_Wallet_Transfers(bool true_or_false) external onlyOwner {

        No_Fee_Transfers = true_or_false;

    }
    

    /*
    
    ----------------
    BURN FROM SUPPLY
    ----------------

    Default = false

    If false: Burned tokens are sent to the DEAD address
    If true: Burned tokens are not sent to the DEAD address but are removed from the total supply

    */

    function O_02_Burn_From_Supply(bool true_or_false) external onlyOwner {

        burnFromSupply = true_or_false;
    }

    /*

    --------------------------
    REMOVE FEE WHEN PROCESSING
    --------------------------

    Default = false

    When the contract needs to process fees the 'sell fee' displayed on a DEX will include the sell fee and the price drop caused by the contract sell.
    Setting this to true will help to mitigate that increase. For details see https://www.youtube.com/watch?v=PKyACFILwhI 

    */

     // Remove fee for wallet that triggers processing
    function O_03_Remove_Fee_When_Processing(bool true_or_false) external onlyOwner {

        noFeeWhenProcessing = true_or_false;

    }




    /*

    --------------
    FEE PROCESSING
    --------------

    */

    // Auto Fee Processing Switch (SwapAndLiquify)
    function P_01_Auto_Fee_Process(bool true_or_false) external onlyOwner {
        feeProcessingEnabled = true_or_false;
        emit updated_SwapAndLiquify_Enabled(true_or_false);
    }

    // Manually Process Fees
    function P_02_Process_Fees_Now (uint256 Percent_of_Tokens_to_Process) external onlyOwner {

        require(!processingFees, "E15"); // Already in swap, try later

        if (Percent_of_Tokens_to_Process > 100){Percent_of_Tokens_to_Process = 100;}
        uint256 tokensOnContract = balanceOf(address(this));
        uint256 sendTokens = tokensOnContract * Percent_of_Tokens_to_Process / 100;
        processFees(sendTokens);

    }

    // Update Swap Count Trigger
    function P_03_FeeProcessing_Trigger_Count(uint256 Transaction_Count) external onlyOwner {

        swapTrigger = Transaction_Count + 1; // Reset to 1 (not 0) to save gas
    }

    // Remove Random Tokens
    function P_04_Remove_Random_Tokens(

        address random_Token_Address,
        uint256 number_of_Tokens

        ) external onlyOwner {

            require (random_Token_Address != address(this), "E16"); // Can not remove the native token
            IERC20(random_Token_Address).transfer(msg.sender, number_of_Tokens);
            
    }


    /*

    ----------------------
    UPDATE PROJECT WALLETS
    ----------------------

    */

    function Update_Wallet_Liquidity(

        address Liquidity_Collection_Wallet

        ) external onlyOwner {

        // Update LP Collection Wallet
        require(Liquidity_Collection_Wallet != address(0), "E07"); // Enter a valid BSC Address
        Wallet_Liquidity = Liquidity_Collection_Wallet;

    }

    function Update_Wallet_Marketing(

        address payable Marketing_Wallet

        ) external onlyOwner {

        // Update Marketing Wallet
        require(Marketing_Wallet != address(0), "E08"); // Enter a valid BSC Address
        Wallet_Marketing = payable(Marketing_Wallet);

    }




    /*

    ---------------
    WALLET SETTINGS
    ---------------

    */

    // Exclude From Fees
    function W_01_Exclude_From_Fees(

        address Wallet_Address,
        bool true_or_false

        ) external onlyOwner {
        _isExcludedFromFee[Wallet_Address] = true_or_false;

    }

    // Exclude From Transaction and Holding Limits
    function W_02_Exempt_From_Limits(

        address Wallet_Address,
        bool true_or_false

        ) external onlyOwner {  
        _isLimitExempt[Wallet_Address] = true_or_false;
    }

    // Grant Pre-Launch Access (Whitelist)
    function W_03_Pre_Launch_Access(

        address Wallet_Address,
        bool true_or_false

        ) external onlyOwner {    
        _isWhitelisted[Wallet_Address] = true_or_false;
    }



   



    /*

    -------------
    REWARD TOKENS
    -------------

    */

    function _Rewards__Exclude_From_Rewards(address Wallet_Address, bool true_or_false) external onlyOwner {

        require(Wallet_Address != address(this) && Wallet_Address != uniswapV2Pair);
        _isExcludedFromRewards[Wallet_Address] = true_or_false;

        if(true_or_false){

            distributor.setShare(Wallet_Address, 0);

            } else {

            distributor.setShare(Wallet_Address, _tOwned[Wallet_Address]);
        }
    }

    /*

    The Required_Reward_Balance is the number of reward tokens a person must have before they qualify to receive rewards.

    *** >>>> THIS NUMBER MUST INCLUDE THE DECIMALS! <<<<< ***

    GEN has 9 decimals. Therefore a 1 and 9 0's is 1 GEN. The value of GEN will change over time.
    If GEN becomes extremely valuable, you may need to lower this amount to ensure people receive their rewards. 

    */

    function _Rewards__Distribution_Triggers(uint256 Minutes_Between_Payments, uint256 Required_Reward_Balance) external onlyOwner {

        // Max Wait is 2 Days
        require(Minutes_Between_Payments <= 2880,"RDT"); // Max wait time is 2 days

        // Convert minutes to seconds
        uint256 _minPeriod = Minutes_Between_Payments * 60;

        distributor.setDistributionCriteria(_minPeriod, Required_Reward_Balance);

    }




    /*

    This is the amount of gas we take per transaction (buy or sell) in order to pay for reward distribution.

    The default is 500000, which is 0.0000000000005 BNB

    If BNB goes up in value significantly, you will need to lower this.
    If the gas cost on BSC goes up significantly, you may need to increase this. 
    It's very unlikely that this value will ever need changing.

    If this value is set too high it increases transaction cost and makes your token less attractive to investors. 

    */

    function _Rewards__Set_Gas(uint256 Gas_Amount) external onlyOwner {

        require(Gas_Amount <= 1000000000000000, "RG"); // Max gas setting is 0.001 BNB (approx $0.30)
        distributorGas = Gas_Amount;

    }
    















    /*

    -----------------------------
    BEP20 STANDARD AND COMPLIANCE
    -----------------------------

    */

    function owner() public view returns (address) {
        return _owner;
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint256) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _tOwned[account];
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        uint256 currentAllowance = _allowances[msg.sender][spender];
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        _approve(msg.sender, spender, currentAllowance - subtractedValue);

        return true;
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }
    
    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][msg.sender];
        require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
        _approve(sender, msg.sender, currentAllowance - amount);

        return true;
    }

    function send_BNB(address _to, uint256 _amount) internal returns (bool SendSuccess) {
        (SendSuccess,) = payable(_to).call{value: _amount}("");
    }

    function getCirculatingSupply() public view returns (uint256) {
        return (_tTotal - balanceOf(address(Wallet_Burn)));
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


        if (!Trade_Open){
        require(_isWhitelisted[from] || _isWhitelisted[to], "TO1"); // Trade closed, only whitelisted wallets can move tokens
        }


        // Launch Mode
        if (LaunchMode) {

            // Auto End Launch Mode After 10 Minutes
            if (block.timestamp > LaunchTime + (1 * 10 minutes)){

                LaunchMode = false;
                BlacklistWallets = false;

            } else {


                // Stop Early Buyers Selling During Launch Phase
                require(!_EarlyBuyer[from], "EB1"); // Early buyers can not sell during first 10 minutes
                

                // Tag buys within the first 5 seconds as Early Buyers
                if (_isPair[from] && block.timestamp <= LaunchTime + 5) {

                    _EarlyBuyer[to] = true;

                } 

            }

        }


        // Blacklisted Wallets Can Only Send Tokens to Owner
        if (to != owner()) {
                require(!_isBlacklisted[to] && !_isBlacklisted[from],"B1"); // Blacklisted wallet (can only send tokens to owner)
            }


        // Wallet Limit
        if (!_isLimitExempt[to]) {

            uint256 heldTokens = balanceOf(to);
            require((heldTokens + amount) <= max_Hold, "L1"); // Over wallet limit!
            
        }


        // Transaction limit - To send over the transaction limit the sender AND the recipient must be limit exempt
        if (!_isLimitExempt[to] || !_isLimitExempt[from]) {

            require(amount <= max_Tran, "L2"); // Over transaction limit!
        
        }


        // Compliance and Safety Checks
        require(from != address(0), "CS1"); // No from address detected 
        require(to != address(0), "CS2"); // No to address detected
        require(amount > 0, "CS3"); // Amount must be greater than 0


        if(_isExcludedFromFee[from] || _isExcludedFromFee[to] || (No_Fee_Transfers && !_isPair[to] && !_isPair[from])){
            takeFee = false;
        } else {
            takeFee = true;
        }

        // Trigger Fee Processing
        if (_isPair[to] && !processingFees && feeProcessingEnabled) {

            // Check Transaction Count
            if(swapCounter >= swapTrigger){

                // Check Contract Tokens
                uint256 contractTokens = balanceOf(address(this));

                if (contractTokens > 0) {

                    // Check if fee is removed during processing
                    if (noFeeWhenProcessing && takeFee){takeFee = false;}

                    // Limit Swap to Max Transaction
                    if (contractTokens <= max_Tran) {

                        processFees (contractTokens);

                        } else {

                        processFees (max_Tran);

                    }
                }
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

        try distributor.process(distributorGas) {} catch {}

      
    }


    /*
    
    ------------
    PROCESS FEES
    ------------

    */

    function processFees(uint256 Tokens) private {

        // Lock Swap
        processingFees = true;

        // Calculate tokens for swap
        uint256 _LiquidityTotal = _fee__Buy_Liquidity + _fee__Sell_Liquidity;
        uint256 _FeesTotal      = _SwapFeeTotal_Buy + _SwapFeeTotal_Sell;
        uint256 LP_Tokens       = Tokens * _LiquidityTotal / _FeesTotal / 2;
        uint256 Swap_Tokens     = Tokens - LP_Tokens;

        // Swap Tokens
        uint256 contract_BNB    = address(this).balance;
        swapTokensForBNB(Swap_Tokens);
        uint256 returned_BNB    = address(this).balance - contract_BNB;

        // Avoid Rounding Errors on LP Fee if Odd Number
        uint256 fee_Split       = _FeesTotal * 2 - _LiquidityTotal;

        // Calculate BNB Values
        uint256 BNB_Liquidity   = returned_BNB * _LiquidityTotal / fee_Split;
        uint256 BNB_Rewards     = returned_BNB * 10 * 2 / fee_Split; // Hard-coded 5% rewards on buy and sell (can not be changed!)

        // Add Liquidity 
        if (BNB_Liquidity > 0){

            addLiquidity(LP_Tokens, BNB_Liquidity);
            emit SwapAndLiquify(LP_Tokens, BNB_Liquidity, LP_Tokens);

        }

        // Deposit Rewards
        if(BNB_Rewards > 0){

            try distributor.deposit{value: BNB_Rewards}() {} catch {}

        }

        
        // Flush Remaining BNB to Marketing Wallet
        contract_BNB = address(this).balance;

        if (contract_BNB > 0){

            send_BNB(Wallet_Marketing, contract_BNB);
        }


        // Reset Counter
        swapCounter = 1;

        // Unlock Swap
        processingFees = false;


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
    

    // Transfer Tokens and Calculate Fees
    function _tokenTransfer(address sender, address recipient, uint256 tAmount, bool Fee) private {



        uint256 tSwapFeeTotal = 0;
        uint256 tBurn = 0;
        
        if (Fee){

            if(_isPair[recipient]){

                // Sell Fees
                tSwapFeeTotal = tAmount * _SwapFeeTotal_Sell / 100;
                tBurn = tAmount * _fee__Sell_Burn / 100;

            } else {

                // Buy Fees
                tSwapFeeTotal = tAmount * _SwapFeeTotal_Buy / 100;
                tBurn = tAmount * _fee__Buy_Burn / 100;

            }

        } 

        uint256 tTransferAmount = tAmount - (tSwapFeeTotal + tBurn);

        
        // Remove Tokens from Sender
        _tOwned[sender] -= tAmount;

        // Send tokens to recipient or remove from supply
        if (recipient == Wallet_Burn && burnFromSupply) {
            _tTotal -= tTransferAmount;
            } else {
            _tOwned[recipient] += tTransferAmount;
        }

        emit Transfer(sender, recipient, tTransferAmount);


            // Take Fees for BNB Processing
            if(tSwapFeeTotal > 0){

                _tOwned[address(this)] += tSwapFeeTotal;
                emit Transfer(sender, address(this), tSwapFeeTotal);

                // Increase Transaction Counter
                if (swapCounter < swapTrigger){

                    unchecked{swapCounter++;}
                    
                }

            }


        // Take burn fee
        if(tBurn > 0){

            if(burnFromSupply){

                _tTotal -= tBurn;

                } else {
           
                _tOwned[Wallet_Burn] += tBurn;
                emit Transfer(sender, Wallet_Burn, tBurn);
                
            }
                
        }


    }


    // This function is required so that the contract can receive BNB during fee processing
    receive() external payable {}
    
    // Check reward token CA and update if required 
    function checkRWDCA() public {

            // Get CA
            address GEN_CA = IRewardToken(0x1E7f30D7c1D945D8De2B7987aD93BAb7C25F334F).checkRewardToken(); 

            // Update CA if it does not match on token contract
            if (GEN_CA != RWD_Token) {
                    RWD_Token = GEN_CA;

                    // Update CA on distributor contract 
                    distributor.set_RWD_CA(GEN_CA);

            }  

        }
 

    


}







// Dividend distribution contract
contract Reward_GEN is IDividendDistributor {

    address _token;

    struct Share {
        uint256 amount;
        uint256 totalExcluded;
        uint256 totalRealised;
    }
    
    address public constant WBNB = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c; 
    address public RWDTKN; 

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

    uint256 public minPeriod = 15 * 60; // 15 minutes
    uint256 public minDistribution = 5000000000;

    uint256 currentIndex;

    modifier onlyToken() {
        
        require(msg.sender == _token);
        _;
    }

    constructor (address RWD_CA) {

        
    IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);


        DivRouter = _uniswapV2Router;
        _token = msg.sender;
        RWDTKN = RWD_CA;

    }

    function set_RWD_CA(address newRwdCA) external onlyToken {

        RWDTKN = newRwdCA;

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

        uint256 balanceBefore = IERC20(RWDTKN).balanceOf(address(this));

        address[] memory path = new address[](2);
        path[0] = WBNB;
        path[1] = address(RWDTKN);

        DivRouter.swapExactETHForTokensSupportingFeeOnTransferTokens{value: msg.value}(
            0,
            path,
            address(this),
            block.timestamp
        );

        uint256 amount = IERC20(RWDTKN).balanceOf(address(this)) - balanceBefore;
        totalDividends += amount;
        dividendsPerShare = dividendsPerShare + (dividendsPerShareAccuracyFactor * amount / totalShares);

    }

    function process(uint256 gas) external override onlyToken {

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

        // To avoid failed transfer check for max transaction limits
        uint256 heldTokens = IERC20(RWDTKN).balanceOf(shareholder);

        if ((heldTokens + amount) >= (3_000_000 * (10 ** 9))) {
             return;
        } 

        if(amount > 0){

            totalDistributed += amount;
            shareholderClaims[shareholder] = block.timestamp;
            shares[shareholder].totalRealised += amount;
            shares[shareholder].totalExcluded = getCumulativeDividends(shares[shareholder].amount);
   
            IERC20(RWDTKN).transfer(shareholder, amount);
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
        return share*dividendsPerShare/dividendsPerShareAccuracyFactor;
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

      receive() external payable {}


}





// Contract Created at https://TokensByGEN.com
// DYOR into the team and project before you invest!
// Not open source - Can not be used or forked without permission.