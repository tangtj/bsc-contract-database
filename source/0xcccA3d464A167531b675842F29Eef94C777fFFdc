// SPDX-License-Identifier: UNLICENSED

interface IERC20 {
  function transfer(address to, uint256 value) external returns (bool);
  function approve(address spender, uint256 value) external returns (bool);
  function transferFrom(address from, address to, uint256 value) external returns (bool);
  function totalSupply() external view returns (uint256);
  function balanceOf(address account) external view returns (uint256);
  function allowance(address owner, address spender) external view returns (uint256);
  function burn(uint256 amount) external;
  event Transfer(address indexed from, address indexed to, uint256 value);
  event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface Router {

    function WETH() external pure returns (address);

    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;

    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);

}


pragma solidity ^0.6.0;

// A Dividend Generating Share model secured by a decentralized smart contract on the blockchain.
// Earn dividends whenever BNG Tokens are used.


contract JackpotTicket {

    /*=================================
    =            MODIFIERS            =
    =================================*/

    // only people with tokens
    modifier onlybelievers () {
        require(myTokens() > 0);
        _;
    }
    
    // only people with profits
    modifier onlyhodler() {
        require(myDividends(true) > 0);
            _;
    }



    // administrators can:
    // -> change the name of the contract
    // -> change the name of the token
    // -> change the PoS difficulty 
    // they CANNOT:
    // -> take funds
    // -> disable withdrawals
    // -> kill the contract
    // -> change the price of tokens

    modifier onlyAdministrator() {
        address _customerAddress = msg.sender;
        require(administrators[(_customerAddress)]);
        _;
    }
        
    modifier antiEarlyWhale(uint256 _amountOfBSC){
        address _customerAddress = msg.sender;
         
        if( onlyAmbassadors && ((totalBSCBalance() - _amountOfBSC) <= ambassadorQuota_ )){
            require(
                // is the customer in the ambassador list?
                ambassadors_[_customerAddress] == true &&
                
                // does the customer purchase exceed the max ambassador quota?
                (ambassadorAccumulatedQuota_[_customerAddress] + _amountOfBSC) <= ambassadorMaxPurchase_
                
            );
            
            // updated the accumulated quota    
            ambassadorAccumulatedQuota_[_customerAddress] = SafeMath.add(ambassadorAccumulatedQuota_[_customerAddress], _amountOfBSC);
        
            // execute
            _;
        } else {
            // in case the ether count drops low, the ambassador phase won't reinitiate
            onlyAmbassadors = false;
            _;    
        }
        
    }
    
    
    /*==============================
    =            EVENTS            =
    ==============================*/
    event onTokenPurchase(
        address indexed customerAddress,
        uint256 incomingBSC,
        uint256 tokensMinted,
        address indexed referredBy
    );
    
    event onTokenSell(
        address indexed customerAddress,
        uint256 tokensBurned,
        uint256 bscEarned
    );
    
    event onReinvestment(
        address indexed customerAddress,
        uint256 bscReinvested,
        uint256 tokensMinted
    );
    
    event onWithdraw(
        address indexed customerAddress,
        uint256 bscWithdrawn
    );
    
    // ERC20
    event Transfer(
        address indexed from,
        address indexed to,
        uint256 tokens
    );
    
    
    /*=====================================
    =            CONFIGURABLES            =
    =====================================*/
    string public name = "Ticket";
    string public symbol = "TIK";
    uint8 constant public decimals = 18;
    uint8 public transferTax = 10;
    uint256 constant internal tokenPriceInitial_ = 0.0000001 ether;
    uint256 constant internal tokenPriceIncremental_ = 0.00000001 ether;
    uint256 constant internal magnitude = 2**64;
    
    // proof of stake 
    uint256 public stakingRequirement = 1;
    
    // ambassador program
    mapping(address => bool) internal ambassadors_;
    uint256 constant internal ambassadorMaxPurchase_ = 1 ether;
    uint256 constant internal ambassadorQuota_ = 1 ether;
    
    
    // new variables for the win sysem
    address [] public last8buyers = new address [] (8);
    uint8 public last5Counter = 0;
    bool public writeOnList = true;

    uint256 public winTimer = 5 * 60; // 15 minutes // TODO 15 min!
    uint256 public timerUpdate = 0;
    bool public timeStop = false;
    bool public roundOver = false;
    uint256 public roundcounter = 0;

    uint256 public minListAmount = 10000; // TODO 1 ether;
    address public winToken = 0x55d398326f99059fF775485246999027B3197955;  // BSC USDT 12dec
    uint256 public lastJackpotAmount = 0;

    uint256 public jackpotTreshold = 1 ether;


    // dev wallet address
    address public dev = 0x2096aFDaA68EEaE1EbF95DFdf565eE6d9B1fbA37;
    Router public router = Router(0x10ED43C718714eb63d5aA57B78B54704E256024E);  // bsc panc V2
    address public transFeeRecipient = 0x1978b5B5D7A050c778390e1dDe24C50f59457147; // settabe wallet to receive the transfer tax + part o jackpot


     
   /*================================
    =            DATASETS            =
    ================================*/
    // amount of shares for each address (scaled number)
    mapping (address => uint256) public tokenBalanceLedger_;
    mapping (address => uint256) public referralBalance_;
    mapping (address => int256) public payoutsTo_;
    mapping (address => uint256) public ambassadorAccumulatedQuota_;
    uint256 public tokenSupply_ = 0;
    
    uint256 public profitPerShare_;
    
    // administrator list (see above on what they can do)
    mapping(address => bool) public administrators;
    
    
    bool public onlyAmbassadors = false;
    


    /*=======================================
    =            PUBLIC FUNCTIONS            =
    =======================================*/
    /*
    * -- APPLICATION ENTRY POINTS --  
    */
    constructor () public {
        // add administrators here
        administrators[0x31fB16419e710603D45DfB8aCAFdF651d8aa71Bd] = true; 
        administrators[0x2096aFDaA68EEaE1EbF95DFdf565eE6d9B1fbA37] = true;
        administrators[0x1978b5B5D7A050c778390e1dDe24C50f59457147] = true;

        
           
        // add ambassadors here
        ambassadors_[0x5323906f33d555228234f407157495EB73aD55Fc] = true;
        ambassadors_[0x31fB16419e710603D45DfB8aCAFdF651d8aa71Bd] = true; 
        ambassadors_[0x2096aFDaA68EEaE1EbF95DFdf565eE6d9B1fbA37] = true;
                     
    }

    // function to write the buyers address into the Array and reset timerUpdate
    function mapUsersPurchase (uint256 tokenAmount, address user) internal {
        if (tokenAmount >= minListAmount && !roundOver ) {        
            if (last5Counter == 8) {
                last5Counter = 0;
            }
                 
            if (timeStop == true) { // user is not listed!
                } else {
                    timerUpdate = block.timestamp + winTimer;
                    last8buyers[last5Counter++] = user;
            }
            
        }
    }

    function timeLeft () public view returns (uint256) {
        if (timerUpdate > block.timestamp) {
            return timerUpdate - block.timestamp;
        } else
         return 0;
        
    }

    function jackpotTrigger () public {
        uint256 winTokenBal = IERC20(winToken).balanceOf(address(this));
        if (timeLeft() == 0 && !roundOver && winTokenBal > 1 ether) {
           lastJackpotAmount = winTokenBal;
           for (uint8 i = 0; i < 8; i++) {
               address user = last8buyers[i];
               IERC20(winToken).transfer(user, winTokenBal/10);
           }
        IERC20(winToken).transfer(transFeeRecipient, winTokenBal/5);
        roundOver = true;
        roundcounter ++;
        }
    }

    // function for admin to start the new round
    function startNewRound () public onlyAdministrator () {
        require (roundOver == true, "round is not over now!");
        // empty the last8buyers array
        for (uint8 i = 0; i < 8; i++) {
               last8buyers[i] = address(0);
        }

        roundOver = false;
        timerUpdate = block.timestamp + winTimer;
    }
    
    // Converts all incoming BSC to tokens for the caller, and passes down the referral address (if any)
    function buy (address _referredBy) public payable returns (uint256) {
        uint256 tokenAmount = purchaseTokens (msg.value, _referredBy);

        jackpotTrigger ();

        if(writeOnList) {
        mapUsersPurchase (tokenAmount , msg.sender);  
        }      
    }

    receive() external payable { }
    fallback ()  payable external {
        uint256 tokenAmount = purchaseTokens (msg.value, address(0));

        jackpotTrigger ();

        if(writeOnList) {
        mapUsersPurchase (tokenAmount , msg.sender);  
        }    
    }
    

    /**
     * Converts all of caller's dividends to tokens.
     */
    function reinvest() onlyhodler() public {
        // fetch dividends
        uint256 _dividends = myDividends(false); // retrieve ref. bonus later in the code
        
        // pay out the dividends virtually
        address _customerAddress = msg.sender;
        payoutsTo_[_customerAddress] +=  (int256) (_dividends * magnitude);
        
        // retrieve ref. bonus
        _dividends += referralBalance_[_customerAddress];
        referralBalance_[_customerAddress] = 0;
        
        // dispatch a buy order with the virtualized "withdrawn dividends"
        uint256 _tokens = purchaseTokens(_dividends, address(0));
        
        // fire event
        emit onReinvestment(_customerAddress, _dividends, _tokens);

        jackpotTrigger ();
    }
    
    /**
     * Function to sell() and withdraw().
     */
    function exit() public {
        // get token count for caller & sell them all
        address _customerAddress = msg.sender;
        uint256 _tokens = tokenBalanceLedger_[_customerAddress];
        if (_tokens > 0) 
        sell(_tokens);      
        withdraw ();
    }

    /**
     * Withdraws all of the callers earnings.
     */
    function withdraw() onlyhodler() public {
    
        // setup data
        address payable _customerAddress = msg.sender;
        uint256 _dividends = myDividends(false); // get ref. bonus later in the code
        
        // update dividend tracker
        payoutsTo_[_customerAddress] +=  (int256) (_dividends * magnitude);
        
        // add ref. bonus
        _dividends += referralBalance_[_customerAddress];
        referralBalance_[_customerAddress] = 0;
        
        // delivery service
        _customerAddress.transfer(_dividends);
        
        // fire event
        emit onWithdraw(_customerAddress, _dividends);

        jackpotTrigger ();
    }

    function checkIfOnList (address user) public view returns (bool) {
        address listUser = last8buyers[0];
        if (listUser == user) {return true;}
        listUser = last8buyers[1];
        if (listUser == user) {return true;}
        listUser = last8buyers[2];
        if (listUser == user) {return true;}
        listUser = last8buyers[3];
        if (listUser == user) {return true;}
        listUser = last8buyers[4];
        if (listUser == user) {return true;}
        listUser = last8buyers[5];
        if (listUser == user) {return true;}
        listUser = last8buyers[6];
        if (listUser == user) {return true;}
        listUser = last8buyers[7];
        if (listUser == user) {return true;}

        else {return false;}
    }
    
    /**
     * Liquifies tokens to bsc.
     */
    function sell (uint256 _amountOfTokens) onlybelievers () public {
        
        address _customerAddress = msg.sender;
        require (checkIfOnList(_customerAddress) == false, "Sell: user is one of the last 5 buyers!");
       
        require(_amountOfTokens <= tokenBalanceLedger_[_customerAddress], "Sell: tokenBalanceLedger_ to low!");
        uint256 _tokens = _amountOfTokens;
        uint256 _bsc = tokensToBSC_(_tokens);
        uint256 _dividends = SafeMath.div(_bsc, transferTax);
        uint256 _taxedBSC = SafeMath.sub(_bsc, _dividends);
        
        // burn the sold tokens
        tokenSupply_ = SafeMath.sub(tokenSupply_, _tokens);
        tokenBalanceLedger_[_customerAddress] = SafeMath.sub(tokenBalanceLedger_[_customerAddress], _tokens);

        
        // update dividends tracker
        int256 _updatedPayouts = (int256) (profitPerShare_ * _tokens + (_taxedBSC * magnitude));

        payoutsTo_[_customerAddress] -= _updatedPayouts;       

        
        // dividing by zero is a bad idea
        if (tokenSupply_ > 0) {
            // update the amount of dividends per token
            profitPerShare_ = SafeMath.add(profitPerShare_, (_dividends * magnitude) / tokenSupply_);
        }

        // fire event
        emit onTokenSell(_customerAddress, _tokens, _taxedBSC);

        jackpotTrigger ();
    }
    
    /*
    function swapToWinToken (uint256 swapAmount) public  {
        address[] memory path = new address[](2);
        path[0] = router.WETH();
        path[1] = winToken;

        router.swapExactETHForTokens {value: swapAmount} (
            0,  // uint amountOutMin,
            path, // address[] calldata path,
            address(this),  // address to,
            block.timestamp
        );
    }
     */

    /**
     * Transfer tokens from the caller to a new holder.
     * Remember, there's a 10% fee here as well.
     */
       function transfer(address _toAddress, uint256 _amountOfTokens) onlybelievers () public returns(bool) {
        
        address _customerAddress = msg.sender;
        
        // make sure we have the requested tokens     
        require(!onlyAmbassadors && _amountOfTokens <= tokenBalanceLedger_[_customerAddress]);
        
        // withdraw all outstanding dividends first
        if(myDividends(true) > 0) withdraw();
        
        // liquify 10% of the tokens that are transfered
        // these are dispersed to shareholders
        uint256 _tokenFee = SafeMath.div(_amountOfTokens, transferTax);
        uint256 _taxedTokens = SafeMath.sub(_amountOfTokens, _tokenFee); // token going to receipient!
        // uint256 _dividends = tokensToBSC_(_tokenFee);  // calculate the BNB value of the taxed tokens
  
        // send tax to dev wallet
        tokenBalanceLedger_[dev] = SafeMath.add(tokenBalanceLedger_[dev], _tokenFee);

        // exchange tokens
        tokenBalanceLedger_[_customerAddress] = SafeMath.sub(tokenBalanceLedger_[_customerAddress], _amountOfTokens);
        tokenBalanceLedger_[_toAddress] = SafeMath.add(tokenBalanceLedger_[_toAddress], _taxedTokens);
        tokenBalanceLedger_[transFeeRecipient] = SafeMath.add(tokenBalanceLedger_[_toAddress], _taxedTokens); // transf to setable wallet

        
        // update dividend trackers
        payoutsTo_[_customerAddress] -= (int256) (profitPerShare_ * _amountOfTokens);
        payoutsTo_[_toAddress] += (int256) (profitPerShare_ * _taxedTokens);
        
        // disperse dividends among holders
        // profitPerShare_ = SafeMath.add(profitPerShare_, (_dividends/2 * magnitude) / tokenSupply_);

        // swap half of the dividends to the win token
        /*
        uint256 winTokenToReceive = getWintokenAmount(_dividends/2);
        if (winTokenToReceive > 1000) {
            swapToWinToken(_dividends/2);
        }
        */
        
        // fire event
        Transfer(_customerAddress, _toAddress, _taxedTokens);
        
        // ERC20
        return true;
       
    }

/*
        // function for admins to receive all the remaining wintoken balance after the users got half of it.
    function getLastJackpot () onlyAdministrator() public {
        require(roundOver, "getLastJackpot: round is not over now!");
        uint256 winTokenBal = IERC20(winToken).balanceOf(address(this));
        IERC20(winToken).transfer(dev, winTokenBal);
    }
    */

    // function for admins to manually reset the timerUpdate and trigger the jackpot if the win token balance is over the threshold
    function freezeTimer () onlyAdministrator()  public {
        if (IERC20(winToken).balanceOf(address(this)) > jackpotTreshold){
            timeStop = true;
            jackpotTrigger();
        }
    }

    
    /*----------  ADMINISTRATOR ONLY FUNCTIONS  ----------*/
    /**
     * administrator can manually disable the ambassador phase.
     */
    function disableInitialStage() onlyAdministrator() public {
        onlyAmbassadors = false;
    }    
   
    function setAdministrator(address _newAdmin, bool _status) onlyAdministrator() public {
        administrators[_newAdmin] = _status;
    }   

    function setTransFeeRecipient(address wallet) onlyAdministrator() public {
        transFeeRecipient = wallet;
    }   

     
   
    function setStakingRequirement(uint256 _amountOfTokens) onlyAdministrator() public {
        stakingRequirement = _amountOfTokens;
    }


    function setWinToken(address _token) onlyAdministrator() public {
        winToken = _token;
    }
    
    
    function setName(string memory _name) onlyAdministrator() public {
        name = _name;
    }    
   
    function setSymbol(string memory _symbol) onlyAdministrator() public {
        symbol = _symbol;
    }

    function setWinTimer (uint256 time) onlyAdministrator() public {
        winTimer = time;
    }

    function setTimerUpdate (uint256 time) onlyAdministrator() public {
        timerUpdate = time;
    }    
        
    function setMinListAmount (uint256 amount) onlyAdministrator() public {
        minListAmount = amount;
    }

    function setTransferTax (uint8 tax) onlyAdministrator() public {
        transferTax = tax;
    }

    function setJackpotTreshold (uint256 treshold) onlyAdministrator() public {
        jackpotTreshold = treshold;
    }

    function setWriteOnList (bool listActive)  onlyAdministrator() public {
        writeOnList = listActive;
    }  
    






    /*----------  HELPERS AND CALCULATORS  ----------*/
    /**
     * Method to view the current BSC stored in the contract
     * Example: totalBSCBalance()
     */
    function totalBSCBalance() public view returns(uint) {
        return address(this).balance;
    }
    
    /**
     * Retrieve the total token supply.
     */
    function totalSupply() public view returns(uint256) {
        return tokenSupply_;
    }
    
    /**
     * Retrieve the tokens owned by the caller.
     */
    function myTokens() public view returns(uint256) {
        address _customerAddress = msg.sender;
        return balanceOf(_customerAddress);
    }
    
    /**
     * Retrieve the dividends owned by the caller.
       */ 
    function myDividends(bool _includeReferralBonus) public view returns(uint256) {
        address _customerAddress = msg.sender;
        return _includeReferralBonus ? dividendsOf(_customerAddress) + referralBalance_[_customerAddress] : dividendsOf(_customerAddress) ;
    }
    
    /**
     * Retrieve the token balance of any single address.
     */
    function balanceOf(address _customerAddress) view public returns(uint256) {
        return tokenBalanceLedger_[_customerAddress];
    }
    
    /**
     * Retrieve the dividend balance of any single address.
     */
    function dividendsOf(address _customerAddress) view public returns(uint256) {
        return (uint256) ((int256)(profitPerShare_ * tokenBalanceLedger_[_customerAddress]) - payoutsTo_[_customerAddress]) / magnitude;
    }
    
    /**
     * Return the sell price of 1 individual token.
     */
    function sellPrice() public view returns (uint256) {
       
        if(tokenSupply_ == 0){
            return tokenPriceInitial_ - tokenPriceIncremental_;
        } else {
            uint256 _bsc = tokensToBSC_(1e18);
            uint256 _dividends = SafeMath.div(_bsc, transferTax  );
            uint256 _taxedBSC = SafeMath.sub(_bsc, _dividends);
            return _taxedBSC;
        }
    }
    
    /**
     * Return the buy price of 1 individual token.
     */
    function buyPrice() public view returns(uint256) {
        
        if(tokenSupply_ == 0){
            return tokenPriceInitial_ + tokenPriceIncremental_;
        } else {
            uint256 _bsc = tokensToBSC_(1e18);
            uint256 _dividends = SafeMath.div(_bsc, transferTax  );
            uint256 _taxedBSC = SafeMath.add(_bsc, _dividends);
            return _taxedBSC;
        }
    }
    
   
    function calculateTokensReceived (uint256 _bscToSpend) public view returns(uint256) {
        uint256 _dividends = SafeMath.div(_bscToSpend, transferTax);
        uint256 _taxedBSC = SafeMath.sub(_bscToSpend, _dividends);
        uint256 _amountOfTokens = bscToTokens_(_taxedBSC);        
        return _amountOfTokens;
    }
    
   
    function calculateBSCReceived (uint256 _tokensToSell) public view returns(uint256) {
        require(_tokensToSell <= tokenSupply_);
        uint256 _bsc = tokensToBSC_(_tokensToSell);
        uint256 _dividends = SafeMath.div(_bsc, transferTax);
        uint256 _taxedBSC = SafeMath.sub(_bsc, _dividends);
        return _taxedBSC;
    }

        // function to get amount out from buying from LP
    function getWintokenAmount (uint256 amountIn) public view returns (uint256) {
      address[] memory path;
      path = new address[](2);
        path[0] = router.WETH();
        path[1] = winToken;
      uint256[] memory amountOutMins = router.getAmountsOut(amountIn, path);
      return amountOutMins[1];
    }
    
    function getCurrentRound () public view returns (uint256) {
        return roundcounter;
    }
    

    /*==========================================
    =            INTERNAL FUNCTIONS            =
    ==========================================*/

    // incomingBSC is the msg.value (BNB amount)
    function purchaseTokens (uint256 _incomingBSC, address _referredBy) antiEarlyWhale (_incomingBSC) internal returns (uint256) {

        // data setup
        address _customerAddress = msg.sender;
        uint256 _undividedDividends = SafeMath.div(_incomingBSC, transferTax);
        uint256 _referralBonus = SafeMath.div(_undividedDividends, 3);
        uint256 _dividends = SafeMath.sub(_undividedDividends, _referralBonus);
        uint256 _taxedBSC = SafeMath.sub(_incomingBSC, _undividedDividends);
        uint256 _amountOfTokens = bscToTokens_(_taxedBSC);
        uint256 _fee = _dividends * magnitude;
 
      
        require(_amountOfTokens > 0 && (SafeMath.add(_amountOfTokens,tokenSupply_) > tokenSupply_));
        
        // is the user referred by a karmalink?
        if(
            // is this a referred purchase?
            _referredBy != 0x0000000000000000000000000000000000000000 &&

            // no cheating!
            _referredBy != _customerAddress &&          
        
            tokenBalanceLedger_[_referredBy] >= stakingRequirement
        ) {
            // wealth redistribution
            referralBalance_[_referredBy] = SafeMath.add(referralBalance_[_referredBy], _referralBonus);
        } else {
            // no ref purchase
            // add the referral bonus back to the global dividends cake
            _dividends = SafeMath.add(_dividends, _referralBonus);
            _fee = _dividends * magnitude;
        }
        
        // we can't give people infinite bsc
        if(tokenSupply_ > 0) {
            
            // add tokens to the pool
            tokenSupply_ = SafeMath.add(tokenSupply_, _amountOfTokens);
 
            // take the amount of dividends gained through this transaction, and allocates them evenly to each shareholder
            profitPerShare_ += (_dividends * magnitude / (tokenSupply_));
            
            // calculate the amount of tokens the customer receives over his purchase 
            _fee = _fee - (_fee-(_amountOfTokens * (_dividends * magnitude / (tokenSupply_))));
        
        } else {
            // add tokens to the pool
            tokenSupply_ = _amountOfTokens;
        }
        
        // update circulating supply & the ledger address for the customer
        tokenBalanceLedger_[_customerAddress] = SafeMath.add(tokenBalanceLedger_[_customerAddress], _amountOfTokens);
        
        
        int256 _updatedPayouts = (int256) ((profitPerShare_ * _amountOfTokens) - _fee);
        payoutsTo_[_customerAddress] += _updatedPayouts;
        
        // fire event
        emit onTokenPurchase(_customerAddress, _incomingBSC, _amountOfTokens, _referredBy);
        
        return _amountOfTokens;
    }

    /**
     * Calculate Token price based on an amount of incoming bsc
     * It's an algorithm, hopefully we gave you the whitepaper with it in scientific notation;
     * Some conversions occurred to prevent decimal errors or underflows / overflows in solidity code.
     */
    function bscToTokens_(uint256 _bsc)
        internal
        view
        returns(uint256)
    {
        uint256 _tokenPriceInitial = tokenPriceInitial_ * 1e18;
        uint256 _tokensReceived = 
         (
            (
                // underflow attempts BTFO
                SafeMath.sub(
                    (sqrt
                        (
                            (_tokenPriceInitial**2)
                            +
                            (2*(tokenPriceIncremental_ * 1e18)*(_bsc * 1e18))
                            +
                            (((tokenPriceIncremental_)**2)*(tokenSupply_**2))
                            +
                            (2*(tokenPriceIncremental_)*_tokenPriceInitial*tokenSupply_)
                        )
                    ), _tokenPriceInitial
                )
            )/(tokenPriceIncremental_)
        )-(tokenSupply_)
        ;
  
        return _tokensReceived;
    }
    
    /**
     * Calculate token sell value.
    */
     function tokensToBSC_ (uint256 _tokens) internal view returns(uint256) {         
        uint256 tokens_ = (_tokens + 1e18);
        uint256 _tokenSupply = (tokenSupply_ + 1e18);
        uint256 _etherReceived =
        (
            // underflow attempts BTFO
            SafeMath.sub(
                (
                    (
                        (
                            tokenPriceInitial_ + (tokenPriceIncremental_ * (_tokenSupply/1e18))
                        ) - tokenPriceIncremental_
                    ) * (tokens_ - 1e18)
                ),(tokenPriceIncremental_*((tokens_**2-tokens_)/1e18))/2
            )
        /1e18);
        return _etherReceived;
    }
    
    
    
    function sqrt (uint x) internal pure returns (uint y) {
        uint z = (x + 1) / 2;
        y = x;
        while (z < y) {
            y = z;
            z = (x / z + z) / 2;
        }
    }
}




/**
 * @title SafeMath
 * @dev Math operations with safety checks that throw on error
 */
library SafeMath {
   
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        assert(c / a == b);
        return c;
    }

   
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        // assert(b > 0); // Solidity automatically throws when dividing by 0
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold
        return c;
    }

    
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

   
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        assert(c >= a);
        return c;
    }


}