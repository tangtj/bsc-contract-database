// SPDX-License-Identifier: MIT
pragma solidity ^0.8.16;

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}



contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }   
    
    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }


    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }

}


abstract contract ReentrancyGuard {
   
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

   
    modifier nonReentrant() {
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}

interface IERC20 {

    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function decimals() external view returns(uint8);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

}



contract LPP_PreSale_1 is ReentrancyGuard, Context, Ownable {

    mapping (address => uint256) public _contributions;
    mapping (address => uint256) public _contributionsBusd;
    mapping (address => bool) public _whitelisted;
    mapping (address => uint256) public maxPurchase;
    mapping (address => uint256) public claimed;
    mapping (address => uint256) public claimedUsdt;

    mapping (address => uint256) public claimTime;
    mapping (address => uint256) public currentClaimCount;
    mapping (address => bool) public alreadyClaimed;


    
    IERC20 public _token;
    IERC20 public _busdToken;
    uint256 private _tokenDecimals;
    address public _wallet;
    address public _walletHold;
    uint256 public _rateUsdt;
    uint256 public _weiRaised;
    uint256 public endPresale;
    uint256 public minPurchase;
    uint256 public maxPurchasePer;
    uint256 public hardcap;
    uint256 public purchasedTokens;
    bool public whitelistPurchase = false;

    uint256 public claimStartDate; // Timestamp when the claim period starts
    uint256 public claimInterval = 30 days; // Interval between each claim
    uint256 public totalClaimRounds = 9; // Total number of claim rounds (including the initial release)


    
    

    event TokensPurchased(address  purchaser, uint256 value, uint256 amount);
    constructor (uint256 rateUsdt, address wallet,address walletHold, IERC20 token, IERC20 busdToken)  {
        require(rateUsdt > 0, "Pre-Sale: rate is 0");
        require(wallet != address(0), "Pre-Sale: wallet is the zero address");
        require(address(token) != address(0), "Pre-Sale: token is the zero address");
        
        _rateUsdt = rateUsdt;
        _wallet = wallet;
        _token = token;
        _walletHold = walletHold;
        _busdToken = busdToken;
        _tokenDecimals = 18 - _token.decimals();
    }
    
    function setWhitelist(address[] memory recipients,uint256[] memory _maxPurchase) public onlyOwner{
        require(recipients.length == _maxPurchase.length);
        for(uint256 i = 0; i < recipients.length; i++){
            _whitelisted[recipients[i]] = true;
            maxPurchase[recipients[i]] = _maxPurchase[i] * (10**18);
        }
    }

    function setBlacklist(address[] memory recipients) public onlyOwner{
        for(uint256 i = 0; i < recipients.length; i++){
            _whitelisted[recipients[i]] = false;
        }
    }
    
    function whitelistAccount(address account) external onlyOwner{
        _whitelisted[account] = true;
    }

    function blacklistAccount(address account) external onlyOwner{
        _whitelisted[account] = false;
    }
    
    
    //Start Pre-Sale
    function startPresale(uint256 endDate, uint256 _minPurchase,uint256 _maxPurchase,  uint256 _hardcap) external onlyOwner icoNotActive() {
        require(endDate > block.timestamp, 'duration should be > 0');
        endPresale = endDate; 
        minPurchase = _minPurchase;
        maxPurchasePer = _maxPurchase;
        hardcap = _hardcap;
        _weiRaised = 0;
    }
    
    function stopPresale() external onlyOwner icoActive(){
        endPresale = 0;
    }
    

    function buyTokensBusd(uint256 amount) public nonReentrant icoActive{
        uint256 weiAmount = amount;
        require(_busdToken.balanceOf(msg.sender)>=amount,"Balance is Low");
        require(_busdToken.transferFrom(msg.sender,address(this),amount),"Couldnt Transfer Amount");
        if(_whitelisted[msg.sender]){
            require(_busdToken.transfer(_wallet,amount),"Couldnt Transfer Amount");
        }else{
            require(_busdToken.transfer(_walletHold,amount),"Couldnt Transfer Amount");
        }

        uint256 tokens = _getTokenAmountBusd(weiAmount);
        _preValidatePurchase(msg.sender, weiAmount);
        _weiRaised = _weiRaised + weiAmount;
        purchasedTokens += tokens;
        _contributionsBusd[msg.sender] = _contributionsBusd[msg.sender] + weiAmount;
        emit TokensPurchased(msg.sender, weiAmount, tokens);
    }


    function _preValidatePurchase(address beneficiary, uint256 weiAmount) internal view {
        require(beneficiary != address(0), "Presale: beneficiary is the zero address");
        require(weiAmount != 0, "Presale: weiAmount is 0");
        require(weiAmount >= minPurchase, "have to send at least: minPurchase");
        require(_weiRaised + weiAmount <= hardcap, "Exceeding hardcap");
        if(whitelistPurchase){
            require(_whitelisted[beneficiary], "You are not in whitelist");
            if(maxPurchasePer>0){
                require(_contributions[beneficiary] + weiAmount <= maxPurchasePer, "can't buy more than: maxPurchase");
            }else{
                require(_contributions[beneficiary] + weiAmount <= maxPurchase[beneficiary], "can't buy more than: maxPurchase");
            }
        }else{
            require(_contributions[beneficiary] + weiAmount <= maxPurchasePer, "can't buy more than: maxPurchase");
        }
    }


    // Start the claim process
    function startClaim() external onlyOwner {
        require(claimStartDate == 0, "Claim has already started");
        claimStartDate = block.timestamp;
    }

    function claimUsdt() internal{
        require(checkContributionUsdt(msg.sender) > 0, "No tokens to claim");
        require(checkContributionUsdt(msg.sender) <= IERC20(_token).balanceOf(address(this)), "No enough tokens in contract");
        uint256 amount = _contributionsBusd[msg.sender];
        claimedUsdt[msg.sender] = claimedUsdt[msg.sender] + amount;
        uint256 tokenTransfer = _getTokenAmountBusd(amount);
        require(IERC20(_token).transfer(msg.sender, tokenTransfer));
    }

    function calculateClaimableAmount(address account) public view returns (uint256) {

        uint256 userDate;
        if(currentClaimCount[account]==0){
            userDate = claimStartDate;
        }else{
            userDate = claimTime[account];
        }
        if(alreadyClaimed[account]){
            return 0;
        }

        uint256 elapsedTime = block.timestamp - userDate;
        uint256 currentRound = (elapsedTime / claimInterval);

        uint256 firstRelease = (checkContributionUsdt(account) * 125) / 10000; // 12.5%

        if (currentClaimCount[account] == 0) {
            uint256 unclaimedAmount = firstRelease - claimed[account];
            return unclaimedAmount;
        }else{
 
            uint256 claimableAmount = (checkContributionUsdt(account)-firstRelease) / (totalClaimRounds - 1);

            if(claimed[account] + (claimableAmount*currentRound) > checkContributionUsdt(account)){
                return checkContributionUsdt(account) - claimed[account];
            }else{    
                return claimableAmount*currentRound;
            }
        }

        
    }

    function totalClaimed(address account) external view returns (uint256){
        return claimed[account];
    }


    function setAlreadyClaimed(address _account, bool status) external onlyOwner{
        alreadyClaimed[_account] = status;
    }


    // Claim funds based on the claimable amount
    function claim() external {
        require(claimStartDate > 0, "Claim has not started yet");
        uint256 claimableAmount = calculateClaimableAmount(msg.sender);
        require(claimableAmount > 0, "No funds available for claim");

        claimed[msg.sender] += claimableAmount;
        claimTime[msg.sender] = block.timestamp;
        currentClaimCount[msg.sender] = currentClaimCount[msg.sender]+1;
        require(IERC20(_token).transfer(msg.sender, claimableAmount));
    }

    

    function checkWhitelist(address account) external view returns(bool){
        return _whitelisted[account];
    }


    function _getTokenAmountBusd(uint256 weiAmount) internal view returns (uint256) {
        return (weiAmount * _rateUsdt);
    }

    function _forwardFunds(uint256 amount) external onlyOwner {
        payable(_wallet).transfer(amount);
    }

    function checkContributionUsdt(address addr) public view returns(uint256){
        uint256 tokensBought = _getTokenAmountBusd(_contributionsBusd[addr]);
        return (tokensBought);
    }

    function checkContributionExtBusd(address addr) external view returns(uint256){
        uint256 tokensBought = _getTokenAmountBusd(_contributions[addr]);
        return (tokensBought);
    }

    function switchWhitelistPurchase(bool _turn) external onlyOwner {
        whitelistPurchase = _turn;
    }


    function setRateUsdt(uint256 newRate) external onlyOwner{
        _rateUsdt = newRate;
    }
    
    function setWalletReceiver(address newWallet) external onlyOwner(){
        _wallet = newWallet;
    }

    function setWalletHolder(address newWallet) external onlyOwner(){
        _walletHold = newWallet;
    }

    
     function setMinPurchase(uint256 value) external onlyOwner{
        minPurchase = value;
    }

    function setMaxPurchase(uint256 value) external onlyOwner{
        maxPurchasePer = value;
    }
    
    function setHardcap(uint256 value) external onlyOwner{
        hardcap = value;
    }
    
    function foreignTokens(IERC20 tokenAddress) public onlyOwner{
        IERC20 tokenBEP = tokenAddress;
        uint256 tokenAmt = tokenBEP.balanceOf(address(this));
        require(tokenAmt > 0, "BEP-20 balance is 0");
        tokenBEP.transfer(_wallet, tokenAmt);
    }
    
    modifier icoActive() {
        require(endPresale > 0 && block.timestamp < endPresale && _weiRaised < hardcap, "Presale must be active");
        _;
    }
    
    modifier icoNotActive() {
        require(endPresale < block.timestamp, 'Presale should not be active');
        _;
    }
    
}