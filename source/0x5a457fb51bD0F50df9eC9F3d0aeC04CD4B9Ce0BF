// SPDX-License-Identifier: Unlicensed
// Not Open Source - Do Not Copy

/*
    
    GEN [GEN]

        Factory Website:    https://tokensbygen.com
        Token Website:      https://gentokens.com/
        Community Group:    https://t.me/GenTokens
        Token Group:        https://t.me/gen_gentokens
        Twitter:            https://twitter.com/GEN_GenTokens
        YouTube:            https://youtube.com/c/gentokens

    Why Buy GEN? 

        GEN is the flagship token on the GenTokens Community. 
        Free Reward Tokens created at www.tokensbygen.com include an immutable 5% GEN Reward
        Every new reward token created pumps GEN! 

    GEN Tokenomics

        2% GEN Reflection rewards to Holders on every sell

        Total Buy Fee: 9%
        Total Sell Fee: 9%

    Buyer Protection

        Fees are capped at 9% on buys and sells
        Immutable Wallet limits are hard-coded to 1.5% (3,000,000 Tokens)
        Trade can not be paused
        Developer Doxed 
        Liquidity Locked
        No team tokens
        See "Project_Information" on BSCScan Contract Read Page for links to the Liquidity Lock, Security Audit, and KYC Certificate.

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

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}


interface IUniswapV2Factory {
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
    function factory() external view returns (address);
}

interface IUniswapV2Router02 {

    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

contract GEN is Context, IERC20 { 

    // Contract Wallets
    address public _owner = 0x888220cf888894A717EAFFd9c29480C52C5cBBb7;
    address payable public FeeCollector = payable(0x091207Db05D3Ca5afeCDb01c739F5665317854f0);
    address public constant Wallet_Burn = 0x000000000000000000000000000000000000dEaD; 


    // Token Info
    string private constant _name = "GEN";
    string private constant _symbol = "GEN";
    uint8 private constant  _decimals = 9;

    // Buy fees (Project fee includes marketing, liquidity, team etc.)
    uint8 public FeeBuy_Reflect = 0;
    uint8 public FeeBuy_Project = 9;

    // Sell Fees
    uint8 public FeeSell_Reflect = 2;
    uint8 public FeeSell_Project = 7;

    // Fee Processing Triggers
    uint8 private swapTrigger = 12;
    uint8 private swapCounter = 1;    
    

    uint256 private constant _tTotal   = 200_000_000 * (10 ** _decimals); 
    uint256 private constant max_Hold  =   3_000_000 * (10 ** _decimals); // Max wallet and max transaction are 1.5% of total supply

    // Project links
    string private _Website = "https://gentokens.com/";
    string private _Telegram = "https://t.me/gen_gentokens";
    string private _audit;
    string private _dox;
    string private _LP_Lock;

    // Supply Tracking for RFI
    uint256 private _tFeeTotal;
    uint256 private constant MAX = ~uint256(0);
    uint256 private _rTotal = (MAX - (MAX % _tTotal));

    // Set factory
    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapV2Pair;

        constructor () {

        // Transfer token supply to owner wallet
        _rOwned[_owner] = _rTotal;

        // Set PancakeSwap Router Address
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);

        // Create initial liquidity pair with BNB on PancakeSwap factory
        uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), _uniswapV2Router.WETH());
        uniswapV2Router = _uniswapV2Router;

        // Wallets excluded from holding limits
        _isLimitExempt[_owner] = true;
        _isLimitExempt[address(this)] = true;
        _isLimitExempt[Wallet_Burn] = true;
        _isLimitExempt[uniswapV2Pair] = true;

        // Wallets excluded from fees
        _isExcludedFromFee[_owner] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[Wallet_Burn] = true;

        // Set the initial liquidity pair
        _isPair[uniswapV2Pair] = true;    

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
    event updated_Buy_fees(uint256 TotalBuyFee);
    event updated_Sell_fees(uint256 TotalSellFee);
    event updated_AutoFeeProcesing(bool AutoFeeProcessing);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);


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
    address[] private _excluded;                                                // Array of wallets excluded from rewards


    // Fee Processing Switch                  
    bool public processingFees;
    bool public autoFeeProcessing; 

    // Launch Settings
    bool public tradeOpen;
    bool public noFeeWhenProcessing;
    bool public freeWalletTransfers = true;

    // Fee Tracker
    bool private takeFee;

    // Project info
    function Project_Information() external view returns(address Owner_Wallet,
                                                         uint256 Max_Wallet,
                                                         uint256 Fee_When_Buying,
                                                         uint256 Fee_When_Selling,
                                                         string memory Website,
                                                         string memory Telegram,
                                                         string memory Security_Audit,
                                                         string memory Dox_Certificate,
                                                         string memory Liquidity_Lock) {
                                                           
        uint256 Total_buy = FeeBuy_Project + FeeBuy_Reflect;
        uint256 Total_sell = FeeSell_Project + FeeSell_Reflect;
        uint256 _max_Hold = max_Hold / (10 ** _decimals);

        // Return Token Data
        return (_owner,
                _max_Hold,
                Total_buy,
                Total_sell,
                _Website,
                _Telegram,
                _audit,
                _dox,
                _LP_Lock);

    }

    // Set Fees
    function set_Fees(

        uint8 ProjectFee_BUY, 
        uint8 Reflection_BUY,  

        uint8 ProjectFee_SELL, 
        uint8 Reflection_SELL

        ) external onlyOwner {

        require (ProjectFee_BUY + Reflection_BUY <= 9, "F1"); // Max buy fee is 9%
        require (ProjectFee_SELL + Reflection_SELL <= 9, "F2"); // Max sell fee is 9%

        // Update Buy Fees
        FeeBuy_Project = ProjectFee_BUY;
        FeeBuy_Reflect = Reflection_BUY;

        // Update Sell Fees
        FeeSell_Project = ProjectFee_SELL;
        FeeSell_Reflect = Reflection_SELL;

        emit updated_Buy_fees(FeeBuy_Project + FeeBuy_Reflect);
        emit updated_Sell_fees(FeeSell_Project + FeeSell_Reflect);
    }


    // Open Trade
    function open_Trade() external onlyOwner {

        autoFeeProcessing = true;
        tradeOpen = true;

    }




    /*

    ----------------------
    UPDATE PROJECT WALLETS
    ----------------------

    */



    function update_FeeCollector( 

        address payable Fee_Collector

        ) external onlyOwner {

        require(Fee_Collector != address(0), "W1"); // Enter a valid Address
        FeeCollector = payable(Fee_Collector);

    }


    /*

    --------------------
    UPDATE PROJECT LINKS
    --------------------

    */

    function update_Link_Website(

        string memory Website_URL

        ) external onlyOwner{

        _Website = Website_URL;

    }


    function update_Link_Telegram(

        string memory Telegram_URL

        ) external onlyOwner{

        _Telegram = Telegram_URL;

    }


    function update_Link_Audit(

        string memory Audit_URL

        ) external onlyOwner{

        _audit = Audit_URL;

    }


    function update_Link_Dox(

        string memory Dox_URL

        ) external onlyOwner{

        _dox = Dox_URL;

    }


    function update_Link_Liquidity_Lock(

        string memory LP_Lock_URL

        ) external onlyOwner{

        _LP_Lock = LP_Lock_URL;

    }


    // Add Liquidity Pair - required for correct fee calculations 
    function add_Liquidity_Pair(

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
    function ownership_RENOUNCE(uint256 Confirmation_Code) public virtual onlyOwner {

        require(Confirmation_Code == 1234, "O1"); // Renounce confirmation not correct

        // Remove old owner status 
        _isLimitExempt[owner()]     = false;
        _isExcludedFromFee[owner()] = false;
        _isWhiteListed[owner()]     = false;

        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    // Transfer to New Owner - To prevent accidental renounce, you must enter the Confirmation_Code: 1234
    function ownership_TRANSFER(address payable newOwner, uint256 Confirmation_Code) public onlyOwner {

        require(Confirmation_Code == 1234, "O2"); // Renounce confirmation not correct
        require(newOwner != address(0), "O3"); // Enter a valid BSC wallet

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

    function free_Wallet_Transfers(bool true_or_false) public onlyOwner {

        freeWalletTransfers = true_or_false;

    }



    /*

    --------------
    FEE PROCESSING
    --------------

    */

    // Auto Fee Processing Switch 
    function process__Auto_Process(bool true_or_false) external onlyOwner {
        autoFeeProcessing = true_or_false;
        emit updated_AutoFeeProcesing(true_or_false);
    }

    // Manually Process Fees
    function process__Process_Now (uint256 percent) external onlyOwner {

        require(!processingFees, "P1"); // Already in swap, try later
        require(percent <= 100, "P2"); // Max percent is 100 
        uint256 tokensOnContract = balanceOf(address(this));
        uint256 sendTokens = tokensOnContract * percent / 100;
        processFees(sendTokens);

    }

    // Update Swap Count Trigger
    function process__Swap_Trigger_Count(uint8 Transaction_Count) external onlyOwner {

        swapTrigger = Transaction_Count + 1; // Reset to 1 (not 0) to save gas
    }

    // Rescue Tokens Accidentally Sent to Contract Address
    function process__Rescue_Tokens(

        address random_Token_Address,
        uint256 number_of_Tokens

        ) external onlyOwner {

            require (random_Token_Address != address(this), "P3"); // Can not remove the native token
            IERC20(random_Token_Address).transfer(msg.sender, number_of_Tokens);
            
    }

    // Remove fee for the sell that triggers fee processing
    function process__RemoveFeeWhenProcessing(bool true_or_false) external onlyOwner {
        noFeeWhenProcessing = true_or_false;
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

    ------------------------------------
    *** WARNING - 'OUT OF GAS' Risk! ***
    ------------------------------------

    A reflections contract needs to loop through all excluded wallets to correctly process several functions. 
    This loop can break the contract if it runs out of gas before completion.

    To prevent this, keep the number of wallets that are excluded from rewards to an absolute minimum. 
    In addition to the default excluded wallets, you may need to exclude the address of any locked tokens.

    */


    // Wallet will not get reflections
    function rewards_Exclude_Wallet(address account) public onlyOwner() {
        require(!_isExcludedFromRewards[account], "R1"); // Account is already excluded
        if(_rOwned[account] > 0) {
            _tOwned[account] = tokenFromReflection(_rOwned[account]);
        }
        _isExcludedFromRewards[account] = true;
        _excluded.push(account);
    }


    // Wallet will get reflections - DEFAULT
    function rewards_Include_Wallet(address account) external onlyOwner() {
        require(_isExcludedFromRewards[account], "R2"); // Account is already included
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

    ---------------
    WALLET SETTINGS
    ---------------

    */

    // Exclude From Fees
    function wallet_Exclude_From_Fees(

        address Wallet_Address,
        bool true_or_false

        ) external onlyOwner {
        _isExcludedFromFee[Wallet_Address] = true_or_false;

    }

    // Exclude From Transaction and Holding Limits
    function wallet_Exempt_From_Limits(

        address Wallet_Address,
        bool true_or_false

        ) external onlyOwner {  
        _isLimitExempt[Wallet_Address] = true_or_false;
    }

    // Grant Pre-Launch Access (Required for Airdrop contract during migration) 
    function wallet_Pre_Launch_Access( 

        address Wallet_Address,
        bool true_or_false

        ) external onlyOwner {    
        _isWhiteListed[Wallet_Address] = true_or_false;
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

    function decimals() public pure returns (uint8) {
        return _decimals;
    }

    function totalSupply() public pure override returns (uint256) { 
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


        require(balanceOf(from) >= amount, "T1"); // Sender does not have enough tokens!

        if (!tradeOpen){
            require(_isWhiteListed[from] || _isWhiteListed[to], "T2");
        }

        // Wallet Limit
        if (!_isLimitExempt[to] && from != owner()) {

            uint256 heldTokens = balanceOf(to);
            require((heldTokens + amount) <= max_Hold, "T3"); // Over max wallet limit

        }

        // Transaction limit - To send over the transaction limit the sender AND the recipient must be limit exempt 
        if (!_isLimitExempt[to] || !_isLimitExempt[from]){
            require(amount <= max_Hold, "T4"); // Over max transaction limit
            
        }

        // Compliance and safety checks
        require(from != address(0), "T5"); // Not a valid BSC wallet address
        require(to != address(0), "T6"); // Not a valid BSC wallet address
        require(amount > 0, "T7"); // Amount must be greater than 0


        // Remove fee is fee exempt, or during wallet to wallet transfers
        if(_isExcludedFromFee[from] || _isExcludedFromFee[to] || (freeWalletTransfers && !_isPair[to] && !_isPair[from])){
            takeFee = false;
        } else {
            takeFee = true;
        }

        // Check if fee processing is possible
        if( _isPair[to] && !processingFees && autoFeeProcessing) {

            // Check that enough transactions have passed since last swap
            if(swapCounter >= swapTrigger){

                // Check number of tokens on contract
                uint256 contractTokens = balanceOf(address(this));

                // Only trigger fee processing if there are tokens to swap!
                if (contractTokens > 0){

                    // Remove fee on transaction that triggers fee processing
                    if(noFeeWhenProcessing && takeFee) {takeFee = false;}

                    // Limit number of tokens that can be swapped 
                    if (contractTokens <= max_Hold){
                        processFees (contractTokens);   
                        } else {
                        processFees (max_Hold);
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

        // Lock 
        processingFees = true;  

        swapTokensForBNB(Tokens);
        uint256 contract_BNB = address(this).balance;

        if(contract_BNB > 0){
            send_BNB(FeeCollector, contract_BNB);
        }

        // Reset transaction counter (reset to 1 not 0 to save gas)
        swapCounter = 1;

        // Unlock
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

    /*
    
    ----------------------------------
    TRANSFER TOKENS AND CALCULATE FEES
    ----------------------------------

    */

    uint256 private rAmount;

    uint256 private tReflect;
    uint256 private rReflect;

    uint256 private tSwapFeeTotal;
    uint256 private rSwapFeeTotal;

    uint256 private tTransferAmount;
    uint256 private rTransferAmount;

    

    // Transfer Tokens and Calculate Fees
    function _tokenTransfer(address sender, address recipient, uint256 tAmount, bool Fee) private {

        
        if (Fee){

            if(_isPair[recipient]){

                // Sell fees
                tReflect        = tAmount * FeeSell_Reflect / 100;
                tSwapFeeTotal   = tAmount * FeeSell_Project / 100;

            } else {

                // Buy fees
                tReflect        = tAmount * FeeBuy_Reflect / 100;
                tSwapFeeTotal   = tAmount * FeeBuy_Project / 100;

            }

        } else {

                // No fee
                tReflect        = 0;
                tSwapFeeTotal   = 0;

        }

        // Calculate reflected fees for RFI
        uint256 RFI     = _getRate(); 

        rAmount         = tAmount       * RFI;
        rReflect        = tReflect      * RFI;
        rSwapFeeTotal   = tSwapFeeTotal * RFI;

        tTransferAmount = tAmount - (tReflect + tSwapFeeTotal);
        rTransferAmount = rAmount - (rReflect + rSwapFeeTotal);

        // Transfer tokens
        _rOwned[sender] -= rAmount;
        _rOwned[recipient] += rTransferAmount;


        if(_isExcludedFromRewards[sender]){
            _tOwned[sender] -= tAmount;
        }

        if(_isExcludedFromRewards[recipient]){
            _tOwned[recipient] += tTransferAmount;
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
            unchecked{swapCounter++;}
                
        }

    }   

    // This function is required so that the contract can receive BNB during fee processing
    receive() external payable {}

}