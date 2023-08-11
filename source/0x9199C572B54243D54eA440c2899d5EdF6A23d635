// SPDX-License-Identifier: Unlicensed 
// This contract is not open source and can not be used/forked without permission
// Contract created at https://TokensByGen.com


pragma solidity 0.8.19;
 
interface IERC20 {

    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint256);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
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
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}




contract REFLECTIONS_FACTORY is Context {


    function CreateToken(string memory Token_Name, 
                         string memory Token_Symbol, 
                         uint256 Total_Supply, 
                         uint256 Number_Of_Decimals) public {


        // Min of two, max of 18 decimals required
        require(Number_Of_Decimals >= 2 && Number_Of_Decimals <= 18, "DEC"); // Decimals limit is 2 to 18

    new REFLECTIONS_TOKEN(Token_Name,
                      Token_Symbol,
                      Total_Supply, 
                      Number_Of_Decimals,
                      payable(msg.sender));
    
    }

}




contract REFLECTIONS_TOKEN is Context, IERC20 { 


    // Contract Wallets
    address public _owner;
    address public Wallet_Liquidity;
    address payable public Wallet_Marketing;
    address public constant Wallet_Burn = 0x000000000000000000000000000000000000dEaD; 
    address payable public constant feeCollector = payable(0xde491C65E507d281B6a3688d11e8fC222eee0975);

    // Token Info
    string private  _name;
    string private  _symbol;
    uint256 private _decimals;
    uint256 private _tTotal;

    // Project links
    string private _website;
    string private _telegram;
    string private _lplock;

    // Wallet and transaction limits
    uint256 private max_Hold;
    uint256 private max_Tran;

    // Fees
    uint8 public _fee__Buy_Contract = 1;
    uint8 public _fee__Buy_Liquidity;
    uint8 public _fee__Buy_Marketing;
    uint8 public _fee__Buy_Reflection;
    uint8 public _fee__Buy_Burn;

    uint8 public _fee__Sell_Contract = 1;
    uint8 public _fee__Sell_Liquidity;
    uint8 public _fee__Sell_Marketing;
    uint8 public _fee__Sell_Reflection;
    uint8 public _fee__Sell_Burn;

    // Total Fee for Swap
    uint8 private _SwapFeeTotal_Buy = 1;
    uint8 private _SwapFeeTotal_Sell = 1;


    // Supply Tracking for RFI
    uint256 private _rTotal;
    uint256 private _tFeeTotal;
    uint256 private constant MAX = ~uint256(0);

    // Set factory
    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapV2Pair;

    constructor (string memory      _TokenName, 
                 string memory      _TokenSymbol,  
                 uint256            _TotalSupply, 
                 uint256            _Decimals, 
                 address payable    _OwnerWallet) {

    emit TokenCreated(address(this));

    // Set owner
    _owner              = _OwnerWallet;

    // Set basic token details
    _name               = _TokenName;
    _symbol             = _TokenSymbol;
    _decimals           = _Decimals;
    _tTotal             = _TotalSupply * 10**_decimals;
    _rTotal             = (MAX - (MAX % _tTotal));
    
    // Wallet limits
    max_Hold            = _tTotal;
    max_Tran            = _tTotal;

    // Project Wallets Set to Owner
    Wallet_Marketing    = payable(_OwnerWallet);
    Wallet_Liquidity    = _OwnerWallet;


    // Transfer token supply to owner wallet
    _rOwned[_owner]     = _rTotal;

    // Set PancakeSwap Router Address
    IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);


    // Create initial liquidity pair with BNB on PancakeSwap factory
    uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), _uniswapV2Router.WETH());
    uniswapV2Router = _uniswapV2Router;

    // Set the initial liquidity pair
    _isPair[uniswapV2Pair] = true;    

    // Wallets excluded from holding limits
    _isLimitExempt[_owner] = true;
    _isLimitExempt[address(this)] = true;
    _isLimitExempt[Wallet_Burn] = true;
    _isLimitExempt[uniswapV2Pair] = true;

    // Wallets excluded from fees
    _isExcludedFromFee[_owner] = true;
    _isExcludedFromFee[address(this)] = true;
    _isExcludedFromFee[Wallet_Burn] = true;


    // Exclude from Rewards
    _isExcludedFromRewards[Wallet_Burn] = true;
    _isExcludedFromRewards[uniswapV2Pair] = true;
    _isExcludedFromRewards[address(this)] = true;

    // Push excluded wallets to array
    _excluded.push(Wallet_Burn);
    _excluded.push(uniswapV2Pair);
    _excluded.push(address(this));

    // Wallets granted access before trade is open
    _isWhiteListed[_owner] = true;

    // Emit Supply Transfer to Owner
    emit Transfer(address(0), _owner, _tTotal);

    // Emit ownership transfer
    emit OwnershipTransferred(address(0), _owner);

    }

    
    // Events
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    event updated_Wallet_Limits(uint256 max_Tran, uint256 max_Hold);
    event updated_Buy_fees(uint8 Marketing, uint8 Liquidity, uint8 Reflection, uint8 Burn, uint8 Contract_Fee);
    event updated_Sell_fees(uint8 Marketing, uint8 Liquidity, uint8 Reflection, uint8 Burn, uint8 Contract_Fee);
    event updated_SwapAndLiquify_Enabled(bool Swap_and_Liquify_Enabled);
    event SwapAndLiquify(uint256 tokensSwapped, uint256 ethReceived, uint256 tokensIntoLiqudity);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event TokenCreated(address indexed Token_CA);


    // Restrict function to contract owner only 
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    // Address mappings
    mapping (address => uint256) private _tOwned;                               // Tokens Owned
    mapping (address => uint256) private _rOwned;                               // Reflected balance
    mapping (address => mapping (address => uint256)) private _allowances;      // Allowance to spend another wallets tokens
    mapping (address => bool) public _isExcludedFromFee;                        // Wallets that do not pay fees
    mapping (address => bool) public _isExcludedFromRewards;                    // Excluded from RFI rewards
    mapping (address => bool) public _isWhiteListed;                            // Wallets that have access before trade is open
    mapping (address => bool) public _isLimitExempt;                            // Wallets that are excluded from HOLD and TRANSFER limits
    mapping (address => bool) public _isPair;                                   // Address is liquidity pair
    mapping (address => bool) public _EarlyBuyer;                               // Early Buyers 
    mapping (address => bool) public _isBlacklisted;                            // Blacklisted wallets
    address[] private _excluded;                                                // Array of wallets excluded from rewards



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



    // Project info
    function Project_Information() external view returns(address Owner_Wallet,
                                                       uint256 Transaction_Limit,
                                                       uint256 Max_Wallet,
                                                       uint256 Fee_When_Buying,
                                                       uint256 Fee_When_Selling,
                                                       string memory Website,
                                                       string memory Telegram,
                                                       string memory Liquidity_Lock,
                                                       string memory Contract_Created_By) {
                                                           
        string memory Creator = "https://tokensbygen.com";

        uint256 Total_buy =  _fee__Buy_Contract     +
                             _fee__Buy_Liquidity    +
                             _fee__Buy_Marketing    +
                             _fee__Buy_Reflection   +
                             _fee__Buy_Burn;

        uint256 Total_sell = _fee__Sell_Contract    +
                             _fee__Sell_Liquidity   +
                             _fee__Sell_Marketing   +
                             _fee__Sell_Reflection  +
                             _fee__Sell_Burn;

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
                _website,
                _telegram,
                _lplock,
                Creator);

    }
    
    // Prepare the contract for pre-sale
    function F_01_Add_PreSale_Address(

        address Presale_Contract_Address

        ) external onlyOwner {

        _isExcludedFromFee[Presale_Contract_Address] = true; 
        _isLimitExempt[Presale_Contract_Address] = true;
        _isWhiteListed[Presale_Contract_Address] = true;
        _isExcludedFromRewards[Presale_Contract_Address] = true;

        // Push excluded wallets to array
        _excluded.push(Presale_Contract_Address);

    }

    // Set Buy and Sell Fees
    function F_02_Set_Fees(

        uint8 Marketing_on_BUY, 
        uint8 Liquidity_on_BUY, 
        uint8 Reflections_on_BUY,
        uint8 Burn_on_BUY,

        uint8 Marketing_on_SELL,
        uint8 Liquidity_on_SELL,
        uint8 Reflections_on_SELL,
        uint8 Burn_on_SELL

        ) external onlyOwner {

        // Buyer Protection: Max Fee 15% 
        require (Marketing_on_BUY + Liquidity_on_BUY + Reflections_on_BUY + Burn_on_BUY + _fee__Buy_Contract <= 15, "SF1");  // Max fee 15%

        // Buyer Protection: Max Fee 15% 
        require (Marketing_on_SELL + Liquidity_on_SELL + Reflections_on_SELL + Burn_on_SELL + _fee__Sell_Contract <= 15, "SF2");  // Max fee 15%

        // Update Fees
        _fee__Buy_Marketing   = Marketing_on_BUY;
        _fee__Buy_Liquidity   = Liquidity_on_BUY;
        _fee__Buy_Reflection  = Reflections_on_SELL;
        _fee__Buy_Burn        = Burn_on_BUY;

        _fee__Sell_Marketing   = Marketing_on_SELL;
        _fee__Sell_Liquidity   = Liquidity_on_SELL;
        _fee__Sell_Reflection  = Reflections_on_SELL;
        _fee__Sell_Burn        = Burn_on_SELL;

        // Fees For Processing
        _SwapFeeTotal_Sell   = _fee__Sell_Marketing + _fee__Sell_Liquidity + _fee__Buy_Contract;
        _SwapFeeTotal_Buy    = _fee__Buy_Marketing + _fee__Buy_Liquidity + _fee__Sell_Contract;

        emit updated_Buy_fees(_fee__Buy_Marketing, _fee__Buy_Liquidity, _fee__Buy_Reflection, _fee__Buy_Burn, _fee__Buy_Contract);
        emit updated_Sell_fees(_fee__Sell_Marketing, _fee__Sell_Liquidity, _fee__Sell_Reflection, _fee__Sell_Burn, _fee__Sell_Contract);
    
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

        // Push excluded wallets to array
        _excluded.push(Wallet_Address);
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
        _isWhiteListed[owner()]     = false;

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
        _isWhiteListed[owner()]     = false;

        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;

        // Set up new owner status 
        _isLimitExempt[owner()]     = true;
        _isExcludedFromFee[owner()] = true;
        _isWhiteListed[owner()]     = true;

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
        _isWhiteListed[Wallet_Address] = true_or_false;
    }


    /*

    -------------------
    REMOVE CONTRACT FEE
    -------------------

    Removal of the 1% Contract Fee Costs 2 BNB 

    */

    function _Remove_Contract_Fee() external payable {

        require(msg.value == 2*10**18, "REQ_2BNB"); // Need to enter 2, and pay 2 BNB to remove the 1% contract fee

        send_BNB(feeCollector, msg.value);

        // Remove Contract Fee
        _fee__Buy_Contract  = 0;
        _fee__Sell_Contract = 0;

        // Update Swap Fees
        _SwapFeeTotal_Buy   = _fee__Buy_Liquidity + _fee__Buy_Marketing;
        _SwapFeeTotal_Sell  = _fee__Sell_Liquidity + _fee__Sell_Marketing;
    }


   



    /*

    ------------------
    REFLECTION REWARDS
    ------------------

    The following functions are used to exclude or include a wallet in the reflection rewards.
    By default, all wallets are included. 

    Wallets that are excluded:

            The Burn address 
            The Liquidity Pair
            The Contract Address

    ----------------------------------------
    *** WARNING - DoS 'OUT OF GAS' Risk! ***
    ----------------------------------------

    A reflections contract needs to loop through all excluded wallets to correctly process several functions. 
    This loop can break the contract if it runs out of gas before completion.

    To prevent this, keep the number of wallets that are excluded from rewards to an absolute minimum. 
    In addition to the default excluded wallets, you may need to exclude the address of any locked tokens.

    */

    // Wallet will not get reflections
    function _Rewards_Exclude_Wallet(address account) public onlyOwner() {
        require(!_isExcludedFromRewards[account], "Account is already excluded");
        if(_rOwned[account] > 0) {
            _tOwned[account] = tokenFromReflection(_rOwned[account]);
        }
        _isExcludedFromRewards[account] = true;
        _excluded.push(account);
    }


    // Wallet will get reflections - DEFAULT
    function _Rewards_Include_Wallet(address account) external onlyOwner() {
        require(_isExcludedFromRewards[account], "Account is already included");
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_excluded[i] == account) {
                _excluded[i] = _excluded[_excluded.length - 1];
                _tOwned[account] = 0;
                _isExcludedFromRewards[account] = false;
                _excluded.pop();
                break;
            }
        }
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
        if (_isExcludedFromRewards[account]) return _tOwned[account];
        return tokenFromReflection(_rOwned[account]);
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
   
    function tokenFromReflection(uint256 _rAmount) internal view returns(uint256) {
        require(_rAmount <= _rTotal, "rAmount can not be greater than rTotal");
        uint256 currentRate =  _getRate();
        return _rAmount / currentRate;
    }

    function _getRate() private view returns(uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply / tSupply;
    }

    function _getCurrentSupply() private view returns(uint256, uint256) {
        uint256 rSupply = _rTotal;
        uint256 tSupply = _tTotal;      
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_rOwned[_excluded[i]] > rSupply || _tOwned[_excluded[i]] > tSupply) return (_rTotal, _tTotal);
            rSupply = rSupply - _rOwned[_excluded[i]];
            tSupply = tSupply - _tOwned[_excluded[i]];
        }
        if (rSupply < _rTotal / _tTotal) return (_rTotal, _tTotal);
        return (rSupply, tSupply);
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


        require(balanceOf(from) >= amount, "TO1"); // Sender does not have enough tokens!


        if (!Trade_Open){

            require(_isWhiteListed[from] || _isWhiteListed[to], "TO2");  // Trade closed, only whitelisted wallets can move tokens


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
                require(!_isBlacklisted[to] && !_isBlacklisted[from],"BL"); // Blacklisted wallets can not buy or sell (only send tokens to owner)
            }

        // Wallet Limit
        if (!_isLimitExempt[to] && from != owner()) {

            uint256 heldTokens = balanceOf(to);
            require((heldTokens + amount) <= max_Hold, "WL"); // Over max wallet limit

        }

        // Transaction limit - To send over the transaction limit the sender AND the recipient must be limit exempt
        if (!_isLimitExempt[to] || !_isLimitExempt[from]){

            require(amount <= max_Tran, "TL"); // Over max transaction limit
            
        }


        // Compliance and safety checks
        require(from != address(0), "FROM0"); // Not a valid BSC wallet address
        require(to != address(0), "TO0"); // Not a valid BSC wallet address
        require(amount > 0, "AMT0"); // Amount must be greater than 0

      
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

    }


    /*
    
    ------------
    PROCESS FEES
    ------------

    */
    function processFees(uint256 Tokens) private {

        // Lock Swap
        processingFees = true;

        // Totals for buy and sell fees
        uint8 _LiquidityTotal   = _fee__Buy_Liquidity + _fee__Sell_Liquidity;
        uint8 _ContractFeeTotal = _fee__Buy_Contract + _fee__Sell_Contract;
        uint8 _FeesTotal        = _SwapFeeTotal_Buy + _SwapFeeTotal_Sell;

        // Calculate tokens for swap
        uint256 LP_Tokens       = Tokens * _LiquidityTotal / _FeesTotal / 2;
        uint256 Swap_Tokens     = Tokens - LP_Tokens;

        // Swap Tokens
        uint256 contract_BNB    = address(this).balance;
        swapTokensForBNB(Swap_Tokens);
        uint256 returned_BNB    = address(this).balance - contract_BNB;

        // Avoid Rounding Errors on LP Fee if Odd Number
        uint256 fee_Split       = _FeesTotal * 2 - _LiquidityTotal;


        // Add auto liquidity 
        if (_LiquidityTotal > 0 ) {

            uint256 BNB_Liquidity = returned_BNB * _LiquidityTotal / fee_Split;
            addLiquidity(LP_Tokens, BNB_Liquidity);
            emit SwapAndLiquify(LP_Tokens, BNB_Liquidity, LP_Tokens);
        
        }


        // Take contract fee
        if (_ContractFeeTotal > 0) {

            uint256 BNB_ContractFee = returned_BNB * _ContractFeeTotal * 2 / fee_Split;
            send_BNB(feeCollector, BNB_ContractFee);
            
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

    // Swap tokens for BNB
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

    // Add liquidity and send Cake LP tokens to liquidity collection wallet
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


   
        uint256 tBurn = 0;
        uint256 tReflect = 0;
        uint256 tSwapFeeTotal = 0;
    

    // Transfer Tokens and Calculate Fees
    function _tokenTransfer(address sender, address recipient, uint256 tAmount, bool Fee) private {

        
        if (Fee){

            if(_isPair[recipient]){

                // Sell fees
                tReflect        = tAmount * _fee__Sell_Reflection / 100;
                tSwapFeeTotal   = tAmount * _SwapFeeTotal_Sell    / 100;
                tBurn           = tAmount * _fee__Sell_Burn       / 100;

            } else {

                // Buy fees
                tReflect        = tAmount * _fee__Buy_Reflection  / 100;
                tSwapFeeTotal   = tAmount * _SwapFeeTotal_Buy     / 100;
                tBurn           = tAmount * _fee__Buy_Burn        / 100;

            }
        } else {

                tBurn = 0;
                tReflect = 0;
                tSwapFeeTotal = 0;
        }


        // Calculate reflected fees for RFI
        uint256 RFI = _getRate(); 

        uint256 rAmount         = tAmount       * RFI;
        uint256 rReflect        = tReflect      * RFI;
        uint256 rSwapFeeTotal   = tSwapFeeTotal * RFI;
        uint256 rBurn           = tBurn         * RFI;

        uint256 tTransferAmount = tAmount - (tReflect + tSwapFeeTotal + tBurn);
        uint256 rTransferAmount = rAmount - (rReflect + rSwapFeeTotal + rBurn);

        // Remove tokens from sender
        _rOwned[sender] -= rAmount;
        if(_isExcludedFromRewards[sender]){
            _tOwned[sender] -= tAmount;
        }

        // Send tokens to recipient or remove from supply
        if (recipient == Wallet_Burn && burnFromSupply) {

            _tTotal -= tTransferAmount;
            _rTotal -= rTransferAmount;

            } else {

            _rOwned[recipient] += rTransferAmount;
            if(_isExcludedFromRewards[recipient]){
                _tOwned[recipient] += tTransferAmount;

            }
        }

        emit Transfer(sender, recipient, tTransferAmount);

        // Take reflections
        if(tReflect > 0){

            _rTotal -= rReflect;
            _tFeeTotal += tReflect;
        }

        // Take fees that require processing during swap and liquify
        if(tSwapFeeTotal > 0){

            _rOwned[address(this)] += rSwapFeeTotal;
            if(_isExcludedFromRewards[address(this)]){_tOwned[address(this)] += tSwapFeeTotal;}
            emit Transfer(sender, address(this), tSwapFeeTotal);

            // Increase the transaction counter
            if(swapCounter < swapTrigger){
                unchecked{swapCounter++;}
            }
                
        }


        // Take burn fee
        if(tBurn > 0){

            if(burnFromSupply){

                _tTotal -= tBurn;
                _rTotal -= rBurn;

                } else {

                _rOwned[Wallet_Burn] += rBurn;
                if(_isExcludedFromRewards[Wallet_Burn]){_tOwned[Wallet_Burn] += tBurn;}
                emit Transfer(sender, Wallet_Burn, tBurn);
                
            }
                
        }

    }


    // This function is required so that the contract can receive BNB during fee processing
    receive() external payable {}

}



/*


    Reflection Contract (Rewards holders the native token)
    Created at tokensbygen.com

    This contract is not open source - Can not be used or forked without permission.
    Fees from the creation of this token help to support GEN - https://gentokens.com


*/