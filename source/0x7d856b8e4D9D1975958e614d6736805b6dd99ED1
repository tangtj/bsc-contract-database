// SPDX-License-Identifier: GPL-2.0

pragma solidity 0.8.17;

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

contract BITCOINFStaking {
    struct StakeItem {
        uint256 counter;
        address beneficiary;
        uint32 liquidated;
        uint256 term;//in minutes: 1 day = 1,440.00 mins
        uint256 stakedRate;
        uint256 amount;
        uint256 claimAmount;//Cumulative Claimed Amount;
        uint256 startDate;
        uint256 lastClaim;
    }

    IBEP20 private stakeToken;
    uint256 private stakeCounter;
    address private immutable owner;
    uint256 private pauseState; //1-off | 0-on
    uint256 private frozenAmount;

    mapping(uint256 => StakeItem) private stakeList;
    mapping(address => uint256[]) private holderCounterList;
    mapping(uint256 => mapping(uint256 => uint256)) private terms;
    
    //time constants: SECONDSINDAY = 86400, DAYSINYEAR = 365;
    uint256 private constant INDEX_RATE = 1;
    uint256 private constant INDEX_MINAMOUNT = 2;

    event Staked(uint256 counter);
    event Unstaked(uint256 counter, bool soon);
    event InterestClaimed(uint256 counter);
    event Refunded(uint256 amount);

    modifier onlyOwner {
      require(msg.sender == owner, "Only for owner");
      _;
    }

    modifier contractInitialized {
      require(pauseState == 0, "contract is not ready");
      _;
    }

    constructor(address _stakeToken) {        
        owner = msg.sender;
        stakeToken = IBEP20 (_stakeToken);
    }

    function setTerm(uint256 term, uint256 rate, uint256 amount) external onlyOwner {
        //rate: //100 => 1%.
        //rates[term] = rate;
        //minAmounts[term] = amount;
        terms[term][INDEX_RATE] = rate;
        terms[term][INDEX_MINAMOUNT] = amount;
    }

    function getTerm(uint256 term) external view returns (uint256 rate, uint256 minAmount) {
        return (terms[term][INDEX_RATE], terms[term][INDEX_MINAMOUNT]);
    }

    function getContractConfigurations() external view returns (address _owner, address _stakeToken, uint256 _pausedState, uint256 _stakeCounter, uint256 _frozenAmount) {
        return (owner, address(stakeToken), pauseState, stakeCounter, frozenAmount);
    }
 
    function setContractState(uint256 state) external onlyOwner {
        require(state == 1 || state == 0, "wrong state");
        pauseState = state;
    }
    
    //only applicable on testnet
    function setTokenAddress(address _stakeToken) external onlyOwner {
        stakeToken = IBEP20 (_stakeToken);
    }
    
    function calculateInterest(uint256 counter) public view returns (uint256) {
        StakeItem memory item = stakeList[counter];
        // seconds from last withdraw until now => (block.timestamp - item.lastClaim);
        uint256 maxTimestamp = item.startDate + (item.term * 60);
        uint256 lastClaim = item.lastClaim > maxTimestamp ? maxTimestamp : item.lastClaim;
        uint256 availableSeconds = block.timestamp - lastClaim;
        uint256 maxSeconds = (item.startDate + (item.term * 60)) - lastClaim;
        if(availableSeconds > maxSeconds) {
            availableSeconds = maxSeconds;
        }
        //interest = amount * seconds from last withdraw * 1 second interest rate
        return _perAnnual(item.amount * (availableSeconds) * item.stakedRate);        
    }

    function _perAnnual(uint256 amount) internal pure returns(uint256) {
        return amount / 365 / 86400 / 10000;
    }
    
    function getStakeItem(uint256 counter) external view returns (StakeItem memory) {
        return stakeList[counter];
    }

    /*
    *   all old and current stakes
    */
    function getStakeCounterByHolder(address holder) external view returns (uint256[] memory) {
        return holderCounterList[holder];
    }

    /*
    *   get all stake items from a holder with liquidated status
    *   0: being staked, not liquidated yet
    *   1: unstaked, already liquidated
    *   2: both
    */
    function getStakeItemsByHolder(address holder, uint32 status) external view returns (StakeItem[] memory) {
        require(status == 0 || status == 1 || status == 2, "wrong status");
        uint256 stakeCount = holderCounterList[holder].length;
        uint256 selectedStakeCount = 0;
        uint256 selectedIndex = 0;

        //first loop: populate array size
        for (uint256 i = 0; i < stakeCount; i++) {
            if(status == 2 || stakeList[holderCounterList[holder][i]].liquidated == status) {
                selectedStakeCount++;              
            }
        }
        //second loop: memory allocation
        StakeItem[] memory list = new StakeItem[](selectedStakeCount);
        for (uint256 i = 0; i < stakeCount; i++) {
            if(status == 2 || stakeList[holderCounterList[holder][i]].liquidated == status) {
                list[selectedIndex++] = stakeList[holderCounterList[holder][i]];                          
            }
        }
        return list;
    }

    function _maxInterest(uint256 amount, uint256 term /*minute*/, uint256 rate /*annual value*/) internal pure returns (uint256) {
        return _perAnnual(amount * (term * 60) * rate);
    }

    /*
    * user safety protection:
    *  - any user who has enough minimum amount can join the program
    *  - but the contract must have enough fund to pay interest
    *  - it could happen a small discrepancy in amount calculation as of block timestamp algorithm
    */
    function stake(uint256 amount, uint256 term) external contractInitialized returns (uint256 counter) {
        require(terms[term][INDEX_RATE] > 0, "term not available");
        require(amount >= terms[term][INDEX_MINAMOUNT], "stake amount too low");
        uint256 maxInterest = _maxInterest(amount, term, terms[term][INDEX_RATE]);
        require(stakeToken.balanceOf(address(this)) - frozenAmount >= maxInterest, "contract runs out of fund");
        stakeToken.transferFrom(msg.sender, address(this), amount);
        stakeCounter++;
        stakeList[stakeCounter] = StakeItem({
            counter: stakeCounter,
            beneficiary: msg.sender,
            liquidated: 0,
            term: term,
            stakedRate: terms[term][INDEX_RATE],
            amount: amount,
            claimAmount: 0,
            startDate: block.timestamp,
            lastClaim: block.timestamp
        });
        holderCounterList[msg.sender].push(stakeCounter);
        //freeze user's funds
        _freeze(amount + maxInterest);
        emit Staked(stakeCounter);
        return stakeCounter;
    }

    function _freeze(uint256 amount) internal {
        frozenAmount += amount;
    }

    function _unfreeze(uint256 amount) internal {
        frozenAmount -= amount;
    }
    
    function _checkValidItem(StakeItem memory item) internal view {
        require(msg.sender == item.beneficiary, "wrong beneficiary");
        require(item.liquidated == 0, "counter already unstaked");
    }

    /*
    *   soon allow users unstake before term period
    *   need to refund all claimed interest
    */
    function unstake(uint256 counter) external {
        StakeItem storage item = stakeList[counter];
        _checkValidItem(item);
        uint256 transferAmount;
        bool soon = (item.startDate + (item.term * 60) <= block.timestamp) ? false : true;

        if(soon) {
            //calculate full interest
            uint256 maxInterest = _maxInterest(item.amount, item.term, item.stakedRate); 
            //deduct claimed amount
            transferAmount = item.amount - item.claimAmount;
            //unfreeze amount
            _unfreeze(item.amount + maxInterest - item.claimAmount);
        } else {
            //withdraw both interest and original amount
            uint256 interest = calculateInterest(counter);
            transferAmount = item.amount + interest;
            item.claimAmount += interest;
            //unfreeze amount
            _unfreeze(transferAmount);
        }

        item.lastClaim = block.timestamp;
        item.liquidated = 1;
        stakeToken.transfer(item.beneficiary, transferAmount);
        emit Unstaked(counter, soon);
    }

    function claimInterest(uint256 counter) external {
        StakeItem storage item = stakeList[counter];
        _checkValidItem(item);
        uint256 transferAmount = calculateInterest(counter);
        require(transferAmount > 0, "no interest to claim");
        stakeToken.transfer(item.beneficiary, transferAmount);
        item.lastClaim = block.timestamp;
        item.claimAmount += transferAmount;
        //unfreeze claimed amount
        _unfreeze(transferAmount);
        emit InterestClaimed(counter);
    }

    /*
    * owner can only withdraw extra amount
    */
    function refund(uint256 amount) external onlyOwner {
        require(amount <= stakeToken.balanceOf(address(this)) - frozenAmount, "not enough amount to refund");
        stakeToken.transfer(msg.sender, amount);
        emit Refunded(amount);
    }

    /*
    *   clear tokens accidentially dropped to the vault
    */
    function clearVisitingToken(address _visitingToken) external onlyOwner {
        require(_visitingToken != address(stakeToken), "only available for visiting token");
        uint256 amount = IBEP20(_visitingToken).balanceOf(address(this));
        IBEP20(_visitingToken).transfer(msg.sender, amount);
    }
}


/*
    *           *   * * * * * * *   *       *
    * *       * *         *         *     *  
    *   *   *   *         *         *   *    
    *     *     *         *         * *      
    *           *         *         *   *    
    *           *         *         *     *  
    *           *         *         *       *
   contact @mtkungfu on telegram for suggestions
 */