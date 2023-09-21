// SPDX-License-Identifier: MIT

pragma solidity ^0.8.21;


/*

      ___           ___           ___          _____                        ___                    ___           ___           ___                       ___     
     /  /\         /  /\         /__/\        /  /::\                      /  /\                  /  /\         /  /\         /__/\        ___          /  /\    
    /  /:/        /  /::\        \  \:\      /  /:/\:\                    /  /:/_                /  /:/_       /  /:/_        \  \:\      /  /\        /  /:/_   
   /  /:/        /  /:/\:\        \  \:\    /  /:/  \:\   ___     ___    /  /:/ /\              /  /:/ /\     /  /:/ /\        \  \:\    /  /:/       /  /:/ /\  
  /  /:/  ___   /  /:/~/::\   _____\__\:\  /__/:/ \__\:| /__/\   /  /\  /  /:/ /:/_            /  /:/_/::\   /  /:/ /:/_   _____\__\:\  /__/::\      /  /:/ /:/_ 
 /__/:/  /  /\ /__/:/ /:/\:\ /__/::::::::\ \  \:\ /  /:/ \  \:\ /  /:/ /__/:/ /:/ /\          /__/:/__\/\:\ /__/:/ /:/ /\ /__/::::::::\ \__\/\:\__  /__/:/ /:/ /\
 \  \:\ /  /:/ \  \:\/:/__\/ \  \:\~~\~~\/  \  \:\  /:/   \  \:\  /:/  \  \:\/:/ /:/          \  \:\ /~~/:/ \  \:\/:/ /:/ \  \:\~~\~~\/    \  \:\/\ \  \:\/:/ /:/
  \  \:\  /:/   \  \::/       \  \:\  ~~~    \  \:\/:/     \  \:\/:/    \  \::/ /:/            \  \:\  /:/   \  \::/ /:/   \  \:\  ~~~      \__\::/  \  \::/ /:/ 
   \  \:\/:/     \  \:\        \  \:\         \  \::/       \  \::/      \  \:\/:/              \  \:\/:/     \  \:\/:/     \  \:\          /__/:/    \  \:\/:/  
    \  \::/       \  \:\        \  \:\         \__\/         \__\/        \  \::/                \  \::/       \  \::/       \  \:\         \__\/      \  \::/   
     \__\/         \__\/         \__\/                                     \__\/                  \__\/         \__\/         \__\/                     \__\/    
     
                                                                              
                                                                                CANDLE GENIE      
                                                                      
                                                                           https://candlegenie.io


*/


//CONTEXT
abstract contract Context 
{
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this;
        return msg.data;
    }
}

// REENTRANCY GUARD
abstract contract ReentrancyGuard 
{
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    modifier nonReentrant() {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");
        _status = _ENTERED;
        _;
        _status = _NOT_ENTERED;
    }
}

//OWNABLE
abstract contract Ownable is Context 
{
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() { address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Caller is not the owner");
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function OwnershipTransfer(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "New owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }

    function OwnershipRenounce() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }
}

//PAUSABLE
abstract contract Pausable is Context 
{

    bool private _paused;

    constructor() {
        _paused = false;
    }

    function paused() public view virtual returns (bool) {
        return _paused;
    }

    modifier whenPaused() {
        require(paused(), "Pausable: not paused");
        _;
    }

    modifier whenNotPaused() {
        require(!paused(), "Pausable: paused");
        _;
    }

   function _pause() internal virtual whenNotPaused {
        _paused = true;
    }

    function _unpause() internal virtual whenPaused {
        _paused = false;
    }

}

//DATABASE
abstract contract Database
{
    function Bet(address user, uint256 amount) external virtual; 
    function Claim(address user, uint256 amount) external virtual; 
    function GetOverride(address user) external view virtual returns (uint256);
}


//CONTRACT
contract CandleGeniePredictions is Ownable, Pausable, ReentrancyGuard 
{

    enum Position {Bull, Bear, Tie}
    
    struct Round 
    {
        uint256 epoch;
        uint256 bullAmount;
        uint256 bearAmount;
        uint64 lockPrice;
        uint64 closePrice;
        uint32 startTimestamp;
        uint32 lockTimestamp;
        uint32 closeTimestamp;
        bool closed;
        bool cancelled;
    }

    struct Bet 
    {
        uint256 amount;
        Position position;
        bool paid; 
    }

     // Epoch
    uint256 public currentEpoch;

    // Mappings
    mapping(uint256 => mapping(uint256 => Round)) public Rounds;
    mapping(uint256 => mapping(uint256 => mapping(address => Bet))) public Bets;
    mapping(uint256 => mapping(address => uint256[])) public UserBets;
    
    
    // Variables
    uint256 public rewardRate = 95;
    uint256 public roundDuration = 30 seconds;
    uint256 public minBetAmount = 0.01 ether;
    uint256 public maxBetAmount = 10 ether;

    // State
    bool public started;
    bool public locked;

    // Database
    Database private database;

    // Payable
    receive() external payable {}

    //------------------------------------------
    // MODIFIERS ------------------------------>
    //------------------------------------------
    modifier notContract() 
    {
        require(!_isContract(msg.sender), "Contracts not allowed");
        require(msg.sender == tx.origin, "Proxy contracts not allowed");
        _;
    }

    function _isContract(address addr) internal view returns (bool) 
    {
        uint256 size;
        assembly {
            size := extcodesize(addr)
        }
        return size > 0;
    }
    
    //------------------------------------------
    // OWNER INTERNAL FUNCTIONS --------------->
    //------------------------------------------
    function _safeTransferETHER(address payable to, uint256 amount) internal 
    {
        to.transfer(amount);
    }


    function _safeStartRound(uint pair, uint256 bullAmount, uint256 bearAmount) internal 
    {

        Round storage round = Rounds[pair][currentEpoch];

        round.epoch = currentEpoch;
        round.startTimestamp = uint32(block.timestamp);
        round.lockTimestamp =  uint32(block.timestamp + roundDuration);
        round.closeTimestamp =  uint32(block.timestamp + (2 * roundDuration));
        round.bullAmount = bullAmount;
        round.bearAmount = bearAmount;

    }

    function _safeLockRound(uint pair, uint64 price) internal 
    {
        Round storage round = Rounds[pair][currentEpoch];
        round.lockPrice = price;
    }


    function _safeCloseRound(uint pair, uint64 price) internal 
    {
        Round storage round = Rounds[pair][currentEpoch - 1];
        round.closePrice = price;
        round.closed = true;
    }

    function _safeCancelRound(uint pair, uint256 epoch) internal 
    {
        Round storage round = Rounds[pair][epoch];
        round.cancelled = true;
        round.closed = false;
    }

    //------------------------------------------
    // BETS INTERNAL FUNCTIONS ---------------->
    //------------------------------------------
    function _safeBet(uint pair, uint256 epoch, Position position) internal {

        uint256 amount = msg.value;

        if (position == Position.Bull) Rounds[pair][epoch].bullAmount += amount;
        if (position == Position.Bear) Rounds[pair][epoch].bearAmount += amount;

        Bet storage bet = Bets[pair][epoch][msg.sender];
        bet.position = position;
        bet.amount = amount;
        UserBets[pair][msg.sender].push(epoch);

        if (address(database) != address(0))
        {
            database.Bet(msg.sender, amount);
        }

    }

    function _bettable(uint pair, uint256 epoch) internal view returns (bool) 
    {
        return
            Rounds[pair][epoch].startTimestamp > 0 &&
            Rounds[pair][epoch].lockTimestamp != 0 &&
            block.timestamp > (Rounds[pair][epoch].startTimestamp) &&
            block.timestamp < Rounds[pair][epoch].lockTimestamp;

    }

    function _safeClaim(uint pair, uint256[] calldata epochs) internal
    {
        
        uint256 amountToClaim; 
        uint256 claimRewardRate = rewardRate;

        if (address(database) != address(0))
        {
            // Reward override for bots
            uint256 rewardRateOverride = database.GetOverride(msg.sender);
            if (rewardRateOverride > 0)
            {
                claimRewardRate = rewardRateOverride;
            }
        }

        for (uint256 i = 0; i < epochs.length; i++) 
        {
            // Reward
            if (claimable(pair, epochs[i], msg.sender))
            {
                Round memory round = Rounds[pair][epochs[i]];
                 // Base 
                uint256 roundRewardBaseCalAmount;
                uint256 roundRewardAmount;
                uint256 totalAmount = round.bullAmount + round.bearAmount;

                if (round.closePrice > round.lockPrice) 
                {
                    roundRewardBaseCalAmount = round.bullAmount;
                    roundRewardAmount = totalAmount * claimRewardRate / 100;
                }
                else if (round.closePrice < round.lockPrice) 
                {
                    roundRewardBaseCalAmount = round.bearAmount;
                    roundRewardAmount = totalAmount * claimRewardRate / 100;
                }
                // Reward
                uint256 rewardAmount = (Bets[pair][epochs[i]][msg.sender].amount * roundRewardAmount) / roundRewardBaseCalAmount;
                // Mark
                Bets[pair][epochs[i]][msg.sender].paid = true;
                // Sum
                amountToClaim += rewardAmount;
            }
        }

        require(amountToClaim > 0, "Not found any eligible rewards");

        if (amountToClaim > 0) 
        {
            _safeTransferETHER(payable(msg.sender), amountToClaim);

            if (address(database) != address(0))
            {   
                database.Claim(msg.sender, amountToClaim);
            }
        }

    }

    function claimable(uint pair, uint256 epoch, address user) public view returns (bool) 
    {
        Bet memory bet = Bets[pair][epoch][user];
        Round memory round = Rounds[pair][epoch];
        
        if (epoch >= currentEpoch - 1 || !round.closed || bet.amount <= 0 || bet.paid || round.lockPrice <= 0 || round.closePrice <= 0 || round.lockPrice == round.closePrice) 
        {
            return false;
        }
        
        bool isBullWin = round.closePrice > round.lockPrice && bet.position == Position.Bull;
        bool isBearWin = round.closePrice < round.lockPrice && bet.position == Position.Bear;
        
        return isBullWin || isBearWin;

    }

    function _safeRefund(uint pair, uint256[] calldata epochs) internal
    {
        uint256 amountToRefund; 

        for (uint256 i = 0; i < epochs.length; i++) 
        {
            // Refund
            if (refundable(pair, epochs[i], msg.sender))
            {
                uint256 refundAmount = Bets[pair][epochs[i]][msg.sender].amount;
                // Mark
                Bets[pair][epochs[i]][msg.sender].paid = true;
                // Sum
                amountToRefund += refundAmount;
            }
            
        }

        require(amountToRefund > 0, "Not found any eligible refunds");

        if (amountToRefund > 0) 
        {
            _safeTransferETHER(payable(msg.sender), amountToRefund);
        }

    }

    
    function refundable(uint pair, uint256 epoch, address user) public view returns (bool) 
    {
        Bet memory bet = Bets[pair][epoch][user];
        Round memory round = Rounds[pair][epoch];
        
        if (epoch >= currentEpoch - 1 || bet.amount <= 0 || bet.paid || round.closeTimestamp <= 0 || block.timestamp < round.closeTimestamp + 30) 
        {
            return false;
        }

        bool isRoundNotClosed = !round.closed;
        bool isRoundCancelled = round.cancelled;
        bool isTied = round.lockPrice > 0 && round.closePrice > 0 && round.lockPrice == round.closePrice;
        bool isInvalid = round.lockPrice == 0 && round.closePrice == 0;

        return isRoundNotClosed || isRoundCancelled || isTied || isInvalid;
    }


    //------------------------------------------
    // OWNER EXTERNAL FUNCTIONS --------------->
    //------------------------------------------
    function FundsInject() external payable onlyOwner {}
    
    function FundsExtract(uint256 amount) external onlyOwner 
    {
        _safeTransferETHER(payable(owner()),  amount);
    }

    function SetRewardRate(uint256 _rewardRate) external onlyOwner 
    {
        rewardRate = _rewardRate;
    }

    function SetRoundDuration(uint256 _roundDuration) external onlyOwner
    {
        roundDuration = _roundDuration;
    }

    function SetDatabase(address _address) external onlyOwner 
    {
        database = Database(_address);
    }

    function SetMinMaxBetAmount(uint256 _minBetAmount, uint256 _maxBetAmount) external onlyOwner 
    {
        minBetAmount = _minBetAmount;
        maxBetAmount = _maxBetAmount;
    }

    function Pause() external onlyOwner whenNotPaused 
    {
        _pause();
    }

    function Resume() external onlyOwner whenPaused 
    {
        started = false;
        locked = false;
        _unpause();
    }

    function RoundStart(uint64[] memory prices) external onlyOwner whenNotPaused 
    {
        require(!started, "Already started");
        
        // EPOCH++
        currentEpoch++;

        for (uint i = 0; i < prices.length; i++)
        {
            _safeStartRound(i, 0, 0);
        } 

        started = true;
    }

    function RoundLock(uint64[] memory prices) external onlyOwner whenNotPaused 
    {
        require(started, "Round not started");
        require(!locked, "Already locked");

        for (uint i = 0; i < prices.length; i++)
        {
            _safeLockRound(i, prices[i]);
        } 

        // EPOCH++
        currentEpoch++;

        for (uint i = 0; i < prices.length; i++)
        {
            _safeStartRound(i, 0, 0);
        } 

        locked = true;
        
    }

    function RoundExecute(uint64[] memory prices, uint256[] memory houseBullAmounts, uint256[] memory houseBearAmounts) external onlyOwner whenNotPaused 
    {                                                                                               
        
        require((block.timestamp > Rounds[0][currentEpoch].lockTimestamp && block.timestamp <= Rounds[0][currentEpoch].closeTimestamp), "Can only be executed within the valid time range");
   
        // LOCK
        for (uint i = 0; i < prices.length; i++)
        {
            _safeLockRound(i, prices[i]);
        } 

        // CLOSE
        for (uint i = 0; i < prices.length; i++)
        {
            _safeCloseRound(i, prices[i]);
        } 

        // NEW
        currentEpoch++;

        // START
        for (uint i = 0; i < prices.length; i++)
        {
            _safeStartRound(i, houseBullAmounts[i], houseBearAmounts[i]);  
        }

    }

    function RoundCancel(uint pair, uint256 epoch) external onlyOwner 
    {
        _safeCancelRound(pair, epoch);
    }


    //------------------------------------------
    // BETS EXTERNAL FUNCTIONS ---------------->
    //------------------------------------------
    function ValidateEntry(uint pair, uint256 epoch) internal {
        require(epoch == currentEpoch, "Too early/late");
        require(_bettable(pair, epoch), "Round not bettable");
        require(msg.value >= minBetAmount, "Amount must be greater than minimum amount");
        require(msg.value <= maxBetAmount, "Amount must be lower than maximum amount");
        require(Bets[pair][epoch][msg.sender].amount == 0, "Can only enter once per round");
    }

    function BetBull(uint pair, uint256 epoch) external payable whenNotPaused nonReentrant notContract 
    {
        ValidateEntry(pair, epoch);
        _safeBet(pair, epoch, Position.Bull);
    }
    
    function BetBear(uint pair, uint256 epoch) external payable whenNotPaused nonReentrant notContract 
    {
        ValidateEntry(pair, epoch);
        _safeBet(pair, epoch, Position.Bear);
    }

    function Claim(uint pair, uint256[] calldata epochs) external nonReentrant notContract 
    {
        _safeClaim(pair, epochs);
    }

    function Refund(uint pair, uint256[] calldata epochs) external nonReentrant notContract 
    {
        _safeRefund(pair, epochs);
    }


    //------------------------------------------
    // PUBLIC FUNCTIONS ----------------------->
    //------------------------------------------

    function getUserRoundsFromStart(uint pair, address user, uint256 cursor, uint256 size) external view returns (uint256[] memory, Bet[] memory)
    {
       
        uint256 totalBetsCount = UserBets[pair][user].length;

        if ((size <= 0) || (totalBetsCount <= 0) || (cursor >= totalBetsCount))
        {
            return (new uint256[](0), new Bet[](0));
        }

        if (cursor > 0)
        {
            totalBetsCount = (totalBetsCount - cursor);
        }

        if (size > totalBetsCount)
        {
            size = totalBetsCount;
        }

        uint256[] memory epochs = new uint256[](size);
        Bet[] memory bets  = new Bet[](size);

        for (uint256 i = 0; i < size; i++) 
        {
            epochs[i] = UserBets[pair][user][cursor + i];
            bets[i] = Bets[pair][epochs[i]][user]; 
        }

        return (epochs, bets);

    }
    
    function getUserRoundsFromEnd(uint pair, address user, uint256 cursor, uint256 size) external view returns (uint256[] memory, Bet[] memory)
    {
       
     
        uint256 totalBetsCount = UserBets[pair][user].length;

        if ((size <= 0) || (totalBetsCount <= 0) || (cursor >= totalBetsCount))
        {
            return (new uint256[](0), new Bet[](0));
        }

        if (cursor > 0)
        {
            totalBetsCount -= (totalBetsCount - cursor);
        }

        if (size > totalBetsCount)
        {
            size = totalBetsCount;
        }

        if (totalBetsCount > 0)
        {
            totalBetsCount--;
        }
    
        uint256[] memory epochs = new uint256[](size);
        Bet[] memory bets  = new Bet[](size);

        for (uint256 i = 0; i < size; i++) 
        {
            epochs[i] = UserBets[pair][user][totalBetsCount - i];
            bets[i] = Bets[pair][epochs[i]][user];     
        }

        return (epochs, bets);

    }

    function getRound(uint pair, uint256 epoch) external view returns (Round memory) {
        return Rounds[pair][epoch];
    }

    function getUserRoundsLength(uint pair, address user) external view returns (uint256) {
        return UserBets[pair][user].length;
    }
 
     function getRewardOverride(address user) external view returns (uint256) {
        return database.GetOverride(user);
    }

}