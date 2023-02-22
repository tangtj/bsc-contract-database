pragma solidity 0.5.17;

/*
       ______                            __             
      / ____/____ _ ____   ____ _ _____ / /_ ___   _____
     / / __ / __ `// __ \ / __ `// ___// __// _ \ / ___/
    / /_/ // /_/ // / / // /_/ /(__  )/ /_ /  __// /    
    \____/ \__,_//_/ /_/ \__, //____/ \__/ \___//_/     
        ______ _        /____/                          
       / ____/(_)____   ____ _ ____   _____ ___         
      / /_   / // __ \ / __ `// __ \ / ___// _ \        
     / __/  / // / / // /_/ // / / // /__ /  __/        
    /_/    /_//_/ /_/ \__,_//_/ /_/ \___/ \___/         
    ==================================================
    | V1.0.0 | Gangster Finance Vault | MIT License |
    =================================================
    
    This contract is for the "OG Vaults" component of Gangster Finance.
    
=======================================================================================

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, 
INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A 
PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT 
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF 
CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE 
OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
    
=======================================================================================
*/

/////////////////////////////////////////////////////////////////////////
// SafeMath - prevents a whole class of underflow/overflow math issues //
/////////////////////////////////////////////////////////////////////////

library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256 c) {
        if (a == 0) {return 0;}
        c = a * b;
        assert(c / a == b);
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

    function safeSub(uint a, uint b) internal pure returns (uint) {
        if (b > a) {return 0;} else {return a - b;}
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256 c) {
        c = a + b;
        assert(c >= a);
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {return a / b;}
    function max(uint256 a, uint256 b) internal pure returns (uint256) {return a >= b ? a : b;}
    function min(uint256 a, uint256 b) internal pure returns (uint256) {return a < b ? a : b;}
}

/////////////////////////////////////////////////////////////////////////
// Token - A standard BEP20 adapter/interface, enabling token movement //
/////////////////////////////////////////////////////////////////////////

contract Token {
    function transferFrom(address from, address to, uint256 value) public returns (bool);
    function transfer(address to, uint256 value) public returns (bool);
    function balanceOf(address who) public view returns (uint256);
    function approve(address spender, uint256 value) public returns (bool);
}

/////////////////////////////////////////////////////////////////////////
// Reward - A customised adapter/interface to facilitate token minting //
/////////////////////////////////////////////////////////////////////////

contract Reward {
    function mintTokens(address _receiver, uint256 _amount) external;
}

/////////////////////////////////////////////////////////////////////////
// TokenVault - The smart contract where tokens are stored and handled //
/////////////////////////////////////////////////////////////////////////

contract TokenVault {
    using SafeMath for uint;

    // Import the OGX and BEP20 token interfaces
    Token token;
    Reward rewardtokens;
    
    /////////////////////////////////
    // CONFIGURABLES AND VARIABLES //
    /////////////////////////////////
    
    // Store the developer's wallet address and the token address
    address public developer;
    address public tokenAddress;
    
    // Store the number of unique users and total Tx's 
    uint public users;
    uint public totalTxs;
    
    // Store the starting time & block number, the last payout time, and the reward rate.
    uint public startTime; // What time did the vault open?
    uint public startBlock; // What block are all addresses able to interact?
    uint public lastPayout; // What time was the last payout (timestamp)?
    uint public rewardRate; // What is the multiplier of OGX minting for this vault?
    
    // Store the details of total deposits & claims
    uint public totalClaims;
    uint public totalDeposits;

    // Store the total drip pool balance and rate
    uint  public dripPoolBalance;
    uint8 public dripRate;

    // 10% fee on deposit and withdrawal
    uint8 constant internal divsFee = 10;
    uint256 constant internal magnitude = 2 ** 64;

    // Rebase and payout frequency
    uint256 constant public rebaseFrequency = 6 hours;
    uint256 constant public payoutFrequency = 2 seconds;
    
    // Timestamp of last rebase
    uint256 public lastRebaseTime;
    
    // Current total tokens staked, and profit per share
    uint256 private currentTotalStaked;
    uint256 private profitPerShare_;
    
    
    ////////////////////////////////////
    // MODIFIERS                      //
    ////////////////////////////////////

    // Only holders - Caller must have funds in the vault
    modifier onlyHolders {
        require(myTokens() > 0);
        _;
    }
    
    // Only earners - Caller must have some earnings
    modifier onlyEarners {
        require(myEarnings() > 0);
        _;
    }
    
    // Only Don - The boss is the boss, y'know?
    modifier onlyDon {
        require(msg.sender == developer, "YOU_ARE_NOT_DON");
        _;
    }
    
    // If the block number is before a certain block number, require the caller to be whitelisted.
    // Allows team buy-in prior to countdowns, because The OGs have worked hard to put all this together.
    modifier checkBlock(uint256 _blockNumber) {
        if (block.number < _blockNumber) {
            require(isWhitelisted(msg.sender), 'NOT_READY_YET');
        }
        _;
    }

    ////////////////////////////////////
    // ACCOUNT STRUCT                 //
    ////////////////////////////////////

    struct Account {
        uint deposited;
        uint withdrawn;
        uint compounded;
        uint rewarded;
        uint contributed;
        uint transferredShares;
        uint receivedShares;
        
        uint xInvested;
        uint xRolled;
        uint xRewarded;
        uint xContributed;
        uint xWithdrawn;
        uint xTransferredShares;
        uint xReceivedShares;
    }

    ////////////////////////////////////
    // MAPPINGS                       //
    ////////////////////////////////////

    mapping(address =>  int256) payoutsTo_;
    mapping(address => uint256) balanceOf_;
    mapping(address => Account) accountOf_;
    
    mapping(address => bool) _isOriginalGangster;
    
    ////////////////////////////////////
    // EVENTS                         //
    ////////////////////////////////////
    
    event onDeposit( address indexed _user, uint256 _deposited,  uint256 tokensMinted, uint256 tokensRewarded, uint timestamp);
    event onResolve( address indexed _user, uint256 _liquidated, uint256 tokensEarned, uint timestamp);
    event onCompound(address indexed _user, uint256 _compounded, uint256 tokensMinted, uint timestamp);
    event onWithdraw(address indexed _user, uint256 _withdrawn,                        uint timestamp);
    event onTransfer(address indexed from,  address indexed to,  uint256 tokens,       uint timestamp);
    
    event onRebase(uint256 balance, uint256 timestamp);
    event onDonate(address indexed from, uint256 amount, uint timestamp);
    
    event onMakeOriginalGangster(address indexed caller, address indexed made, uint256 timestamp);
    
    ////////////////////////////////////
    // CONSTRUCTOR                    //
    ////////////////////////////////////

    constructor(address _tokenAddress, address _rewardAddress, uint256 _rewardRate, uint8 _dripRate, uint256 _blockToCountdownTo) public {
        tokenAddress = _tokenAddress;
        token = Token(_tokenAddress);
        
        dripRate = _dripRate;
        
        rewardRate = _rewardRate;
        rewardtokens = Reward(_rewardAddress);
        
        startTime = (block.timestamp);
        startBlock = _blockToCountdownTo;
        
        lastPayout = (block.timestamp);
        
        developer = msg.sender;
        _isOriginalGangster[msg.sender] = true;
    }
    
    ////////////////////////////////////
    // FALLBACK                       //
    ////////////////////////////////////
    
    function() payable external {
        require(false);
    }
    
    ////////////////////////////////////
    // WRITE FUNCTIONS                //
    ////////////////////////////////////

    // Dividend Sauce, for everyone!
    // This is how you drop tokens directly into the Drip Pool balance
    function donate(uint _amount) checkBlock(startBlock) public returns (uint256) {
        
        // Move the tokens from the caller's wallet to this contract.
        require(token.transferFrom(msg.sender, address(this), _amount));
        
        // Add the tokens to the drip pool balance
        dripPoolBalance += _amount;
        
        // Tell the network, successful function - how much in the pool now?
        emit onDonate(msg.sender, _amount, block.timestamp);
        return dripPoolBalance;
    }

    // Deposit: Put tokens into the vault, to save.
    // The deposited amount incurs a 10% fee, which is split 80:20 to the Daily Drip, and instant divs.
    function deposit(uint _amount) checkBlock(startBlock) public returns (uint256)  {
        
        // Approve the token to be transferred by this contract
        token.approve(address(this), _amount);
        
        // Return a call to depositTo...
        return depositTo(msg.sender, _amount);
    }

    // DepositTo: Put tokens into the vault for another address, to save.
    // The deposited amount incurs a 10% fee, which is split 80:20 to the Daily Drip, and instant divs.
    function depositTo(address _user, uint _amount) checkBlock(startBlock) public returns (uint256)  {
        
        // Move the tokens from the caller's wallet to this contract.
        require(token.transferFrom(msg.sender, address(this), _amount));
        
        // Add the deposit to the totalDeposits...
        totalDeposits += _amount;
        
        // Then actually call the deposit method...
        uint amount = _depositTokens(msg.sender, _user, _amount);
        
        // Trigger a distribution for everyone, kind soul!
        distribute();
        
        // Successful function - how many 'shares' (tokens) are the result?
        return amount;
    }

    // Compound: Do this, whenever you have earnings! It's great!
    // This helps you to earn more, faster! It saves gas for withdraw-then-stake'ers.
    //
    // NOTE: The compound function calls the _depositTokens function, so your
    // earnings will also incur a 10% fee.
    function compound() checkBlock(startBlock) onlyEarners public {
         _compoundTokens();
    }
    
    // Harvest: All in a hard day's work!
    // This is how you take the bread home - how you bring the bacon to the house.
    // Do this to collect your earnings or withdraw unstaked tokens to your wallet.
    function harvest() checkBlock(startBlock) onlyEarners public {
        address _user = msg.sender;
        uint256 _dividends = myEarnings();
        
        // Calculate the payout, add it to the user's total paid out accounting...
        payoutsTo_[_user] += (int256) (_dividends * magnitude);
        
        // Pay the user their tokens to their wallet
        token.transfer(_user,_dividends);

        // Update accounting for user/total withdrawal stats...
        accountOf_[_user].withdrawn = SafeMath.add(accountOf_[_user].withdrawn, _dividends);
        accountOf_[_user].xWithdrawn += 1;
        
        // Update total Tx's and claims stats
        totalTxs += 1;
        totalClaims += _dividends;

        // Tell the network...
        emit onWithdraw(_user, _dividends, block.timestamp);

        // Trigger a distribution for everyone, kind soul!
        distribute();
    }

    // Resolve: The fancy name I gave the 'unstake' function.
    // You call this, then 'withdraw' to get your tokens back into your wallet...
    //
    // "BuT wHy wOuLd aNyOnE wAnt tO Do tHat???"
    function resolve(uint256 _amount) checkBlock(startBlock) onlyHolders public {
        address _user = msg.sender;
        require(_amount <= balanceOf_[_user]);
        
        // Calculate dividends and 'shares' (tokens)
        uint256 _undividedDividends = SafeMath.mul(_amount, divsFee) / 100;
        uint256 _taxedTokens = SafeMath.sub(_amount, _undividedDividends);

        // Subtract amounts from user and totals...
        currentTotalStaked = SafeMath.sub(currentTotalStaked, _amount);
        balanceOf_[_user] = SafeMath.sub(balanceOf_[_user], _amount);

        // Update the payment ratios for the user and everyone else...
        int256 _updatedPayouts = (int256) (profitPerShare_ * _amount + (_taxedTokens * magnitude));
        payoutsTo_[_user] -= _updatedPayouts;

        // Serve dividends between the drip and instant divs (4:1)...
        allocateFees(_undividedDividends);
        
        // Tell the network, and trigger a distribution
        emit onResolve( _user, _amount, _taxedTokens, block.timestamp);
        distribute();
    }

    // Transfer staked tokens to another user
    // Means you don't have to unstake your tokens to pay your friend
    // And your friend keeps earning 'till they pay the gas to pull their dough!
    function transfer(address _to, uint256 _amount) checkBlock(startBlock) onlyHolders external returns (bool) {
        return _transferTokens(_to, _amount);
    }
    
    ///////////////////////////////////////
    // OG-ONLY FUNCTIONS                 //
    ///////////////////////////////////////
    
    
    // This function allows The Don to add users to a whitelist, so they can buy in BEFORE the timer finishes.
    // This is purely for preparation of the platform, with the original ten gangsters of Gangster Finance.
    
    function addOriginalGangster(address _user) public onlyDon() returns (bool _success) {
        
        // The _user can't already be an OG - that makes no sense...
        require(_isOriginalGangster[_user] == false, "IS_ALREADY_OG");
        
        // Make them an OG...
        _isOriginalGangster[_user] = true;
        
        // Tell the network, successful function!
        emit onMakeOriginalGangster(msg.sender, _user, block.timestamp);
        return true;
    }
    
    ////////////////////////////////////
    // VIEW FUNCTIONS                 //
    ////////////////////////////////////

    function myTokens() public view returns (uint256) {return balanceOf(msg.sender);}
    function myEarnings() public view returns (uint256) {return dividendsOf(msg.sender);}

    function balanceOf(address _user) public view returns (uint256) {return balanceOf_[_user];}
    function tokenBalance(address _user) public view returns (uint256) {return _user.balance;}
    function totalBalance() public view returns (uint256) {return token.balanceOf(address(this));}
    function totalSupply() public view returns (uint256) {return currentTotalStaked;}
    
    function dividendsOf(address _user) public view returns (uint256) {
        return (uint256) ((int256) (profitPerShare_ * balanceOf_[_user]) - payoutsTo_[_user]) / magnitude;
    }

    function sellPrice() public pure returns (uint256) {
        uint256 _tokens = 1e18;
        uint256 _dividends = SafeMath.div(SafeMath.mul(_tokens, divsFee), 100);
        uint256 _taxedTokens = SafeMath.sub(_tokens, _dividends);
        return _taxedTokens;
    }

    function buyPrice() public pure returns (uint256) {
        uint256 _tokens = 1e18;
        uint256 _dividends = SafeMath.div(SafeMath.mul(_tokens, divsFee), 100);
        uint256 _taxedTokens = SafeMath.add(_tokens, _dividends);
        return _taxedTokens;
    }

    function calculateSharesReceived(uint256 _amount) public pure returns (uint256) {
        uint256 _divies = SafeMath.div(SafeMath.mul(_amount, divsFee), 100);
        uint256 _remains = SafeMath.sub(_amount, _divies);
        uint256 _result = _remains;
        return  _result;
    }

    function calculateTokensReceived(uint256 _amount) public view returns (uint256) {
        require(_amount <= currentTotalStaked);
        uint256 _tokens  = _amount;
        uint256 _divies  = SafeMath.div(SafeMath.mul(_tokens, divsFee), 100);
        uint256 _remains = SafeMath.sub(_tokens, _divies);
        return _remains;
    }

    function accountOf(address _user) public view returns (uint256[14] memory){
        Account memory a = accountOf_[_user];
        uint256[14] memory accountArray = [
            a.deposited, 
            a.withdrawn, 
            a.rewarded, 
            a.compounded,
            a.contributed, 
            a.transferredShares, 
            a.receivedShares, 
            a.xInvested, 
            a.xRewarded, 
            a.xContributed, 
            a.xWithdrawn, 
            a.xTransferredShares, 
            a.xReceivedShares, 
            a.xRolled
        ];
        return accountArray;
    }

    // dailyEstimate: Indeed, an estimate of how many tokens you'll get from the daily drip
    function dailyEstimate(address _user) public view returns (uint256) {
        uint256 share = dripPoolBalance.mul(dripRate).div(100);
        return (currentTotalStaked > 0) ? share.mul(balanceOf_[_user]).div(currentTotalStaked) : 0;
    }

    ////////////////////////////////////
    // PRIVATE / INTERNAL FUNCTIONS   //
    ////////////////////////////////////

    // Allocate fees - Calculates amounts to put into Drip Pool balance and instant divs!
    function allocateFees(uint fee) private {

        // instant = one of five pieces.
        uint256 instant = fee.div(5); 

        // If there's more than 0 tokens staked in the vault...
        if (currentTotalStaked > 0) {
            
            // Distribute those instant divs...
            profitPerShare_ = SafeMath.add(profitPerShare_, (instant * magnitude) / currentTotalStaked);
        }

        // For Drip = (fee - instant)
        dripPoolBalance += fee.safeSub(instant);
    }

    // Distribute - makes the drip, "drip"!
    function distribute() private {
        
        uint _currentTimestamp = (block.timestamp);
        
        // Log a rebase, if it's time to do so...
        if (_currentTimestamp.safeSub(lastRebaseTime) > rebaseFrequency) {
            
            // Tell the network...
            emit onRebase(totalBalance(), _currentTimestamp);
            
            // Update the time this was last updated...
            lastRebaseTime = _currentTimestamp;
        }

        // If there's any time difference...
        if (SafeMath.safeSub(_currentTimestamp, lastPayout) > payoutFrequency && currentTotalStaked > 0) {
            
            // Calculate shares and profits...
            uint256 share = dripPoolBalance.mul(dripRate).div(100).div(24 hours);
            uint256 profit = share * _currentTimestamp.safeSub(lastPayout);
            
            // Subtract from drip pool balance and add to all user earnings
            dripPoolBalance = dripPoolBalance.safeSub(profit);
            profitPerShare_ = SafeMath.add(profitPerShare_, (profit * magnitude) / currentTotalStaked);
            
            // Update the last payout timestamp
            lastPayout = _currentTimestamp;
        }
    }
    
    // _depositTokens - the method which actually does all the accounting on deposit...
    function _depositTokens(address _depositor, address _recipient, uint256 _amount) internal returns (uint256) {
        
        // If the recipient has zero activity, they're new - COUNT THEM!!!
        if (accountOf_[_recipient].deposited == 0 && accountOf_[_recipient].receivedShares == 0) {
            users += 1;
        }

        // Count this tx...
        totalTxs += 1;

        // Calculate dividends and 'shares' (tokens)
        uint256 _undividedDividends = SafeMath.mul(_amount, divsFee) / 100;
        uint256 _tokens = SafeMath.sub(_amount, _undividedDividends);
        
        // Find the initial reward, which is 10% of the quantity deposited
        uint256 _reward = (_amount * rewardRate) / 10;
        
        // Mint OGX tokens, to whomever deposited the base vault token.
        _mintRewardTokens(_depositor, _reward);
        
        // Tell the network...
        emit onDeposit(_recipient, _amount, _tokens, _reward, block.timestamp);

        // There needs to be something being added in this call...
        require(_tokens > 0 && SafeMath.add(_tokens, currentTotalStaked) > currentTotalStaked);
        if (currentTotalStaked > 0) {
            currentTotalStaked += _tokens;
        } else {
            currentTotalStaked = _tokens;
        }
        
        // Allocate fees, and balance to the recipient
        allocateFees(_undividedDividends);
        balanceOf_[_recipient] = SafeMath.add(balanceOf_[_recipient], _tokens);
        
        // Updated payouts...
        int256 _updatedPayouts = (int256) (profitPerShare_ * _tokens);
        
        // Update stats...
        payoutsTo_[_recipient] += _updatedPayouts;
        accountOf_[_recipient].deposited += _amount;
        accountOf_[_recipient].xInvested += 1;

        // Successful function - how many "shares" generated?
        return _tokens;
    }
    
    // The internal function which compound() calls
    function _compoundTokens() internal returns (uint256) {
        address _account = msg.sender;
        
        // Quickly roll the caller's earnings into their payouts
        uint256 _dividends = dividendsOf(_account);
        payoutsTo_[_account] += (int256) (_dividends * magnitude);
        
        // Then actually trigger the deposit method
        // (NOTE: No tokens required here, earnings are tokens already within the contract)
        uint256 _tokens = _depositTokens(msg.sender, msg.sender, _dividends);
        
        // Tell the network...
        emit onCompound(_account, _dividends, _tokens, block.timestamp);

        // Then update the stats...
        accountOf_[_account].compounded = SafeMath.add(accountOf_[_account].compounded, _dividends);
        accountOf_[_account].xRolled += 1;
        
        // Call a distribution for everyone, you kind soul!
        distribute();
        
        // Successful function!
        return _tokens;
    }
    
    // Transfer, but don't actually transfer, tokens between users.
    // This means the tokens stay in the contract and keep earning, till the recipient wants to actually withdraw.
    function _transferTokens(address _recipient, uint256 _amount) internal returns (bool _success) {
        address _sender = msg.sender;
        require(_amount <= balanceOf_[_sender]);
        
        // Harvest any earnings before transferring, to help with cleaner accounting
        if (myEarnings() > 0) {
            harvest();
        }
        
        // "Move" the tokens...
        balanceOf_[_sender] = SafeMath.sub(balanceOf_[_sender], _amount);
        balanceOf_[_recipient] = SafeMath.add(balanceOf_[_recipient], _amount);

        // Adjust payout ratios to match the new balances...
        payoutsTo_[_sender] -= (int256) (profitPerShare_ * _amount);
        payoutsTo_[_recipient] += (int256) (profitPerShare_ * _amount);

        // If the recipient has zero activity, they're new - COUNT THEM!!!
        if (accountOf_[_recipient].deposited == 0 && accountOf_[_recipient].receivedShares == 0) {
            users += 1;
        }
        
        // Update stats...
        accountOf_[_sender].xTransferredShares += 1;
        accountOf_[_sender].transferredShares += _amount;
        accountOf_[_recipient].receivedShares += _amount;
        accountOf_[_recipient].xReceivedShares += 1;
        
        // Add this to the Tx counter...
        totalTxs += 1;

        // Tell the network, successful function!
        emit onTransfer(_sender, _recipient, _amount, block.timestamp);
        return true;
    }
    
    // Mint Token Rewards
    // Pass an address to receive the tokens, and how many should be minted.
    // NOTE: The quantity will be multiplied by rewardRate
    function _mintRewardTokens(address _recipient, uint256 _amount) internal {
        rewardtokens.mintTokens(_recipient, _amount.mul(rewardRate));
    }
    
    // isWhitelisted
    // This function checks if a user has been added to a list of approval to bypass wait timers.
    // After launch, nobody is whitelisted anymore. 
    function isWhitelisted(address _user) public view returns (bool _isWhitelisted) {
        
        // If the block number is after launch, nobody is whitelisted...
        if (block.number > startBlock) {
            
            // Even if their official status in the code says they are, it has no effect.
            // This function switches it off by a returning 'false' to all checks after launch.
            return false;
        }
        
        // If they get to this point, display their actual whitelist status
        _isWhitelisted = _isOriginalGangster[_user];
    }
}