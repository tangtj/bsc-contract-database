// DELTO
// Website: https://delto.org

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;
 
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
// -----------------------------------------------------------------DELTO-----------------------------------------------------------------
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

contract DELTO is Context, IERC20 { 

    // Contract Wallets
    address public _owner = 0x624B9507Fd3117b260c119C95D2eAB3BFa313378;
    address payable public FeeCollector = payable(0xdB12C568a03eC289dEC14229177cfB647d3471d5);
    address public constant Wallet_Burn = 0x000000000000000000000000000000000000dEaD; 
    // DELTO
    string private constant _name = "DELTO";
    string private constant _symbol = "DLT";
    uint8 private constant  _decimals = 9;
    // Buy Fees
    uint8 public FeeBuy_Reflect = 0;
    uint8 public FeeBuy_Project = 0;

    // Sell Fees
    uint8 public FeeSell_Reflect = 0;
    uint8 public FeeSell_Project = 0;

    uint8 private swapTrigger = 12;
    uint8 private swapCounter = 1;    
    

    uint256 private constant _tTotal   = 1_000_000_000 * (10 ** _decimals); 
    uint256 private constant max_Hold  =   30_000_000 * (10 ** _decimals); // Max wallet and max transaction are 3% of total supply
    // DELTO links
    string private _Website = "https://delto.org/";
    string private _Telegram = "https://t.me/delto_org";
    string private _audit;
    string private _dox;
    string private _LP_Lock;

    uint256 private _tFeeTotal;
    uint256 private constant MAX = ~uint256(0);
    uint256 private _rTotal = (MAX - (MAX % _tTotal));

    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapV2Pair;

        constructor () {

        _rOwned[_owner] = _rTotal;

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);

        uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), _uniswapV2Router.WETH());
        uniswapV2Router = _uniswapV2Router;

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
    mapping (address => uint256) private _tOwned;
    mapping (address => uint256) private _rOwned;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) public _isExcludedFromFee;
    mapping (address => bool) public _isExcludedFromRewards;
    mapping (address => bool) public _isWhiteListed;
    mapping (address => bool) public _isLimitExempt;
    mapping (address => bool) public _isPair;
    address[] private _excluded;


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

        require (ProjectFee_BUY + Reflection_BUY <= 10, "F1"); // Max buy fee is 10%
        require (ProjectFee_SELL + Reflection_SELL <= 10, "F2"); // Max sell fee is 10%

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


    function update_FeeCollector( 

        address payable Fee_Collector

        ) external onlyOwner {

        require(Fee_Collector != address(0), "W1");
        FeeCollector = payable(Fee_Collector);

    }

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
// -----------------------------------------------------------------DELTO-----------------------------------------------------------------

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


    function add_Liquidity_Pair(

        address Wallet_Address,
        bool true_or_false)

        external onlyOwner {

        _isPair[Wallet_Address] = true_or_false;
        _isLimitExempt[Wallet_Address] = true_or_false;
        _isExcludedFromRewards[Wallet_Address] = true;

        _excluded.push(Wallet_Address);
    } 

    function ownership_RENOUNCE(uint256 Confirmation_Code) public virtual onlyOwner {

        require(Confirmation_Code == 9693, "O1"); 

        _isLimitExempt[owner()]     = false;
        _isExcludedFromFee[owner()] = false;
        _isWhiteListed[owner()]     = false;

        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }


    function ownership_TRANSFER(address payable newOwner, uint256 Confirmation_Code) public onlyOwner {

        require(Confirmation_Code == 9693, "O2");
        require(newOwner != address(0), "O3");

        _isLimitExempt[owner()]     = false;
        _isExcludedFromFee[owner()] = false;
        _isWhiteListed[owner()]     = false;

        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;

        _isLimitExempt[owner()]     = true;
        _isExcludedFromFee[owner()] = true;
        _isWhiteListed[owner()]     = true;

    }

    function process__Auto_Process(bool true_or_false) external onlyOwner {
        autoFeeProcessing = true_or_false;
        emit updated_AutoFeeProcesing(true_or_false);
    }

    function free_Wallet_Transfers(bool true_or_false) public onlyOwner {

        freeWalletTransfers = true_or_false;

    }

    function process__Process_Now (uint256 percent) external onlyOwner {

        require(!processingFees, "P1");
        require(percent <= 100, "P2");
        uint256 tokensOnContract = balanceOf(address(this));
        uint256 sendTokens = tokensOnContract * percent / 100;
        processFees(sendTokens);

    }

    function process__RemoveFeeWhenProcessing(bool true_or_false) external onlyOwner {
        noFeeWhenProcessing = true_or_false;
    }

    function process__Swap_Trigger_Count(uint8 Transaction_Count) external onlyOwner {

        swapTrigger = Transaction_Count + 1;
    }

    function process__Rescue_Tokens(

        address random_Token_Address,
        uint256 number_of_Tokens

        ) external onlyOwner {

            require (random_Token_Address != address(this), "P3");
            IERC20(random_Token_Address).transfer(msg.sender, number_of_Tokens);
            
    }

    function rewards_Exclude_Wallet(address account) public onlyOwner() {
        require(!_isExcludedFromRewards[account], "R1");
        if(_rOwned[account] > 0) {
            _tOwned[account] = tokenFromReflection(_rOwned[account]);
        }
        _isExcludedFromRewards[account] = true;
        _excluded.push(account);
    }

    function rewards_Include_Wallet(address account) external onlyOwner() {
        require(_isExcludedFromRewards[account], "R2");
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

    function wallet_Exclude_From_Fees(

        address Wallet_Address,
        bool true_or_false

        ) external onlyOwner {
        _isExcludedFromFee[Wallet_Address] = true_or_false;

    }
// -----------------------------------------------------------------DELTO-----------------------------------------------------------------
    function wallet_Exempt_From_Limits(

        address Wallet_Address,
        bool true_or_false

        ) external onlyOwner {  
        _isLimitExempt[Wallet_Address] = true_or_false;
    }

    function wallet_Pre_Launch_Access( 

        address Wallet_Address,
        bool true_or_false

        ) external onlyOwner {    
        _isWhiteListed[Wallet_Address] = true_or_false;
    }

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
// -----------------------------------------------------------------DELTO-----------------------------------------------------------------
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

    function _transfer(
        address from,
        address to,
        uint256 amount
      ) private {


        require(balanceOf(from) >= amount, "T1");

        if (!tradeOpen){
            require(_isWhiteListed[from] || _isWhiteListed[to], "T2");
        }

        if (!_isLimitExempt[to] && from != owner()) {

            uint256 heldTokens = balanceOf(to);
            require((heldTokens + amount) <= max_Hold, "T3");

        }

        if (!_isLimitExempt[to] || !_isLimitExempt[from]){
            require(amount <= max_Hold, "T4");
            
        }

        require(from != address(0), "T5");
        require(to != address(0), "T6");
        require(amount > 0, "T7");


        if(_isExcludedFromFee[from] || _isExcludedFromFee[to] || (freeWalletTransfers && !_isPair[to] && !_isPair[from])){
            takeFee = false;
        } else {
            takeFee = true;
        }

        if( _isPair[to] && !processingFees && autoFeeProcessing) {

            if(swapCounter >= swapTrigger){

                uint256 contractTokens = balanceOf(address(this));

                if (contractTokens > 0){

                    if(noFeeWhenProcessing && takeFee) {takeFee = false;}

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

    function processFees(uint256 Tokens) private {

        processingFees = true;  

        swapTokensForBNB(Tokens);
        uint256 contract_BNB = address(this).balance;

        if(contract_BNB > 0){
            send_BNB(FeeCollector, contract_BNB);
        }

        swapCounter = 1;

        processingFees = false;
    }
// -----------------------------------------------------------------DELTO-----------------------------------------------------------------
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

    uint256 private rAmount;

    uint256 private tReflect;
    uint256 private rReflect;

    uint256 private tSwapFeeTotal;
    uint256 private rSwapFeeTotal;

    uint256 private tTransferAmount;
    uint256 private rTransferAmount;


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

                tReflect        = 0;
                tSwapFeeTotal   = 0;

        }

        uint256 RFI     = _getRate(); 

        rAmount         = tAmount       * RFI;
        rReflect        = tReflect      * RFI;
        rSwapFeeTotal   = tSwapFeeTotal * RFI;

        tTransferAmount = tAmount - (tReflect + tSwapFeeTotal);
        rTransferAmount = rAmount - (rReflect + rSwapFeeTotal);

        _rOwned[sender] -= rAmount;
        _rOwned[recipient] += rTransferAmount;


        if(_isExcludedFromRewards[sender]){
            _tOwned[sender] -= tAmount;
        }

        if(_isExcludedFromRewards[recipient]){
            _tOwned[recipient] += tTransferAmount;
        }

        emit Transfer(sender, recipient, tTransferAmount);

        if(tReflect > 0){

            _rTotal -= rReflect;
            _tFeeTotal += tReflect;
        }

        if(tSwapFeeTotal > 0){

            _rOwned[address(this)] += rSwapFeeTotal;
            if(_isExcludedFromRewards[address(this)]){_tOwned[address(this)] += tSwapFeeTotal;}
            emit Transfer(sender, address(this), tSwapFeeTotal);

            unchecked{swapCounter++;}
                
        }

    }   
 // -----------------------------------------------------------------DELTO-----------------------------------------------------------------
    receive() external payable {}

}