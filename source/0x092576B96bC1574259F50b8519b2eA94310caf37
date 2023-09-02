/**
 *Submitted for verification at BscScan.com on 2023-08-30
*/

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

/**
 * @title Counters
 * @author Matt Condon (@shrugs)
 * @dev Provides counters that can only be incremented, decremented or reset. This can be used e.g. to track the number
 * of elements in a mapping, issuing ERC721 ids, or counting request ids.
 *
 * Include with 'using Counters for Counters.Counter;'
 */
library Counters {
    struct Counter {
        // This variable should never be directly accessed by users of the library: interactions must be restricted to
        // the library's function. As of Solidity v0.5.2, this cannot be enforced, though there is a proposal to add
        // this feature: see https://github.com/ethereum/solidity/issues/4637
        uint256 _value; // default: 0
    }

    function current(Counter storage counter) internal view returns (uint256) {
        return counter._value;
    }

    function increment(Counter storage counter) internal {
        unchecked {
            counter._value += 1;
        }
    }

    function decrement(Counter storage counter) internal {
        uint256 value = counter._value;
        require(value > 0, "Counter: decrement overflow");
        unchecked {
            counter._value = value - 1;
        }
    }

    function reset(Counter storage counter) internal {
        counter._value = 0;
    }
}

/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner account will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

contract Lottery is Ownable {
    using Counters for Counters.Counter;
  
    // Lottery data
    struct LotteryData {
        uint256 startTime;
        uint256 endTime;
        uint8 tax;
        uint256 minTicketAmount;
        uint256 totalCollected; 
    }

    // Lottery ticket
    struct Ticket {
        uint256 lotteryId;
        uint256 amount;
    }

    mapping(uint lotteryId => LotteryData) private lotteries;
    mapping(address player => Ticket) private tickets;
    mapping(uint lotteryId => address[]) private winners;

    Counters.Counter public lotteryId;
    IERC20 public lotteryToken;
    address[] private _currentPlayers; // players in the current lottery
    uint256[5] private _blockTracker;
    uint private _currentBlockTrackerIndex;

    // admin configurable variables
    uint256 public roundDuration;
    uint256 public minTicketAmount;
    uint8 public tax;
    address public feeAddress;

    event NewLottery(uint256 lotteryId, uint256 endTime, uint256 minTicketAmount, uint8 tax);
    event NewTicket(address indexed player, uint256 lotteryId, uint256 amount);
    event TicketWithdrawn(address indexed player, uint256 amount);

    constructor(address _lotteryToken, address _feeAddress) {
        lotteryToken = IERC20(_lotteryToken);
        feeAddress = _feeAddress;
        tax = 1; // 1%
        minTicketAmount = 1000e18;
        roundDuration = 2 weeks;
        _currentBlockTrackerIndex = 0;
    }
    /* 
    * Admin functions
    */
    function setFeeAddress(address _feeAddress) external onlyOwner {
        feeAddress = _feeAddress;
    }

    function setTax(uint8 _tax) external onlyOwner {
        tax = _tax;
    }

    function setMinTicketAmount(uint256 _minTicketAmount) external onlyOwner {
        minTicketAmount = _minTicketAmount;
    }

    function setRoundDuration(uint256 _roundDuration) external onlyOwner {
        roundDuration = _roundDuration;
    }

    function createLottery() external onlyOwner {
        require(!lotteryIsActive(), "Active lottery");
        require(
            winners[lotteryId.current()].length > 0 || _currentPlayers.length == 0,
            "Call findWinners() before creating new lottery"
        );
        lotteryId.increment();
        uint256 _endTime = block.timestamp + roundDuration;
        lotteries[lotteryId.current()] = LotteryData({
            startTime: block.timestamp,
            endTime: _endTime,
            tax: tax,
            minTicketAmount: minTicketAmount,
            totalCollected: 0
        });
        // reset players
        delete _currentPlayers;
        emit NewLottery(lotteryId.current(), _endTime, minTicketAmount, tax);
    }

    function findWinners() external onlyOwner {
        require(!lotteryIsActive(), "Lottery is active");
        require(winners[lotteryId.current()].length == 0, "Winners already found");
    
        if (_currentPlayers.length < 3) {
            winners[lotteryId.current()] = _currentPlayers;
        } else {
            uint256 chunk = _currentPlayers.length / 3;
            uint256 randomIndex1 = _random(0, chunk);
            uint256 randomIndex2 = _random(chunk, chunk * 2);
            uint256 randomIndex3 = _random(chunk * 2, _currentPlayers.length);
            winners[lotteryId.current()] = [_currentPlayers[randomIndex1], _currentPlayers[randomIndex2], _currentPlayers[randomIndex3]];
        }
    }

    /* 
    * Private functions
    */
    function _random(uint256 _min, uint256 _max) private view returns (uint256) {
        require(_min < _max, "Invalid range");
        return (uint256(keccak256(abi.encodePacked(_min, _max, block.prevrandao, block.timestamp, _currentPlayers))) % (_max - _min)) + _min;
    }

    function _play(address _player, uint256 _amount) internal {
        // deduct and transfer tax
        uint256 _tax = _amount * lotteries[lotteryId.current()].tax / 100;
        lotteryToken.transfer(feeAddress, _tax);

        tickets[_player].amount = _amount - _tax;
        tickets[_player].lotteryId = lotteryId.current();
        lotteries[lotteryId.current()].totalCollected += _amount;
        _currentPlayers.push(_player);
        emit NewTicket(_player, lotteryId.current(), _amount);
    }

    /* 
    * Public functions
    */
    function getWinners(uint _lotteryId) external view returns (address[] memory) {
        return winners[_lotteryId];
    }

    function getLottery(uint _lotteryId) external view returns (LotteryData memory) {
        return lotteries[_lotteryId];
    }

    function getTicket(address _player) external view returns (Ticket memory) {
        return tickets[_player];
    }
    
    function lotteryIsActive() public view returns (bool) {
        return lotteries[lotteryId.current()].endTime > block.timestamp;
    }

    function currentLottory() public view returns (LotteryData memory) {
        return lotteries[lotteryId.current()];
    }

    // Join the current lottery
    function play(uint256 amount) external {
        require(lotteryIsActive(), "Lottery is not active");

        address _player = _msgSender();
        require(tickets[_player].lotteryId != lotteryId.current(), "Already played");
        require(amount >= lotteries[lotteryId.current()].minTicketAmount, "Amount is less than min ticket amount");
        
        uint256 previousBalance = lotteryToken.balanceOf(address(this));
        require(lotteryToken.transferFrom(_player, address(this), amount), "Transfer failed");
        require(lotteryToken.balanceOf(address(this)) >= previousBalance + amount, "Balance is not correct");
        // add existing tickets if any
        _play(_player, amount + tickets[_player].amount);

        // blocktracker
        _blockTracker[_currentBlockTrackerIndex] = block.number;
        _currentBlockTrackerIndex = (_currentBlockTrackerIndex + 1) % 5; // Update index circularly
    }

    // Transfer from a previous lottery to the current lottery
    function transferTicket() external {
        address _player = _msgSender();
        Ticket memory _ticket = tickets[_player];
        require(
            _ticket.lotteryId != lotteryId.current() && lotteryIsActive(), 
            "Same lottery or lottery is not active"
        );
        require(_ticket.amount > lotteries[lotteryId.current()].minTicketAmount, "Amount is less than min ticket amount");
        _play(_player, _ticket.amount);
    }

    // Player withdraws their ticket
    function withdrawTicket() external {
        address _player = _msgSender();
        Ticket memory _ticket = tickets[_player];
        require(
            _ticket.lotteryId != lotteryId.current() || !lotteryIsActive(), 
            "Lottery is not finished"
        );
        require(_ticket.amount > 0, "Zero ticket");
        require(lotteryToken.transfer(_player, _ticket.amount), "Transfer failed");
        
        delete tickets[_player];
        emit TicketWithdrawn(_player, _ticket.amount);
    }

    function getBlockTracker() public view returns (uint256[5] memory) {
        return _blockTracker;
    }
}