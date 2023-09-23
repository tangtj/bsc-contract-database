// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)

pragma solidity ^0.8.0;

/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and smsg.data, they should not be accessed in such a direct
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
// File: Ownable.sol


// OpenZeppelin Contracts v4.4.1 (access/Ownable.sol)

pragma solidity ^0.8.0;


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
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
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

// File: BlockchainLottery.sol

pragma solidity ^0.8.0;

contract BlockchainLottery is Ownable {

  address public winner;
  address[] public ticketHolders;  
  address public roundWinner;
  address public feewallet = 0x90b0fd4e8baABFF5B75bC8cD5D371280F634482b;

  uint256 public ticketCount;
  uint256 public minimumTicketCount = 0;
  uint256 public lotteryIndex = 0;
  uint256 public maximumTicketCount = 50000;
  uint256 public feePercentage = 30; 
  uint256[] public depositedAmounts;   
  uint256 public drawCount = 0;
  uint256 public ticketPrice = 0.03 ether;
  uint256 public winningTicketNumber;
  uint256 public startTime = 1723193670;
  uint256 public endTime;
  uint256 public duration = 604800;

  mapping(address => mapping(uint256 => uint256)) public myTicketCount;
  mapping(address => mapping(uint256 => uint256[])) public myTickets;

  struct Records {
        uint256 drawNumber;
        address walletAddress;
        uint256 prize;
        uint256 winningTicketNumber;
        uint256 lotteryIndex;        
  }

  Records[] public records;

  constructor(){
  }
  
    function buyTickets(uint256 ticketAmount) external payable {

      require(startTime <= block.timestamp, "Project not yet launched");
      require(endTime >= block.timestamp, "Project already finished");
      require(ticketCount + ticketAmount <= maximumTicketCount, "Ticket limit reached");

      if(msg.sender != owner()){
      require(msg.value >= ticketPrice * ticketAmount, "Not enough funds available");
      }

      myTicketCount[msg.sender][lotteryIndex] = myTicketCount[msg.sender][lotteryIndex] + ticketAmount;

      for(uint256 x = 0; x < ticketAmount; x++){
      ticketCount = ticketCount + 1;

      myTickets[msg.sender][lotteryIndex].push(ticketCount);
      ticketHolders.push(msg.sender);
      } 
      
    }

  function distributePrize() public onlyOwner {    

    uint256 prizeMoney = address(this).balance;

    (bool fees, ) = payable(roundWinner).call{value: (prizeMoney * feePercentage) / 100}("");
    require(fees);

    (bool ownerWallet, ) = payable(owner()).call{value: (prizeMoney * (100 - feePercentage))/ 100}("");
    require(ownerWallet);      

    Records memory newRecords = Records(drawCount, roundWinner, (prizeMoney * feePercentage / 100), winningTicketNumber, lotteryIndex);

    records.push(newRecords);
    lotteryIndex++;
    delete ticketHolders;

    restartBuying();       
  }


  function manualNormalPrizeDistribute(address manualWinnerAddress) external onlyOwner{

          uint256 prizeMoney = address(this).balance;

          (bool fees, ) = payable(manualWinnerAddress).call{value: (prizeMoney * feePercentage) / 100}("");
          require(fees);

          (bool ownerWallet, ) = payable(owner()).call{value: (prizeMoney * (100 - feePercentage))/ 100}("");
          require(ownerWallet);

          Records memory newRecords = Records(drawCount, roundWinner, (prizeMoney * feePercentage / 100), winningTicketNumber, lotteryIndex);

          records.push(newRecords);
          lotteryIndex++;
          delete ticketHolders;

          restartBuying();  
  }   
    
  function startBuying() public onlyOwner {
    startTime = block.timestamp;
    endTime = block.timestamp + duration;

  }    

  function restartBuying() internal {
    endTime = block.timestamp + duration;
  }    

  function randomNum(uint256 _mod, uint256 _seed, uint256 _salt) internal view returns(uint256) {
    uint256 num = uint256(keccak256(abi.encodePacked(block.timestamp, msg.sender, _seed, _salt))) % _mod;
    return num;
  }

  function getTicketCount() public view returns (uint256) {
  return ticketCount;
  }

  function getRecords() public view returns (Records[] memory) {
    return records;
  }

  function getRecord(uint256 index) public view returns (Records memory) {
    return records[index];
}

  function drawLottery() public onlyOwner {

    require(ticketCount >= minimumTicketCount, "Ticket count is less than the minimum");

    uint256 winningLotteryNumber = randomNum(ticketCount, block.timestamp * 3, drawCount + 5) + 1;  
    roundWinner = ticketHolders[winningLotteryNumber];

    distributePrize();
    drawCount++;
    ticketCount = 0;
  } 

  function deposit() external payable onlyOwner{
    depositedAmounts.push(msg.value);
  }

  function withdraw() external onlyOwner{
    (bool main, ) = payable(owner()).call{value: address(this).balance}("");
    require(main);
  }

  function setMinimumTicketCount(uint256 _minimumTicketCount) public onlyOwner{
    minimumTicketCount = _minimumTicketCount;
  }

  function setWallets(address _feewallet) public onlyOwner{
    feewallet = _feewallet;    
  }

  function setDrawCount(uint256 _drawCount) external onlyOwner{
    drawCount = _drawCount;
  }

  function setLotteryIndex(uint256 _lotteryIndex) external onlyOwner{
    lotteryIndex = _lotteryIndex;
  }

  function setMaximumTicketCount(uint256 _maximumTicketCount) external onlyOwner{
    maximumTicketCount = _maximumTicketCount;
  }

  function setTicketPrice(uint256 _ticketPrice) external onlyOwner{
    ticketPrice = _ticketPrice; 
  }

  function setTax(uint256 _taxPercentage) external onlyOwner{
    feePercentage  = _taxPercentage ;
  }
  
  function setendTime(uint256 _endTime) external onlyOwner{
    endTime  = _endTime ;
  }

  function setDuration(uint256 _duration) external onlyOwner{
    duration = _duration;
  }

  function getLotteryIndex() external view returns (uint256) {
  return lotteryIndex;
  }
  
}