// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.18;


contract Callable {

    address payable private _context;
    address private _creator;

    constructor() { 
        _context = payable(address(this));
        _creator = msg.sender;
        emit CreateContext(_context, _creator);
    }

    function _contextAddress() internal view returns (address payable) {
        return _context;
    }

    function _contextCreator() internal view returns (address) {
        return _creator;
    }

    function _msgSender() internal view returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view returns (bytes memory) {
        this;
        return msg.data;
    }

    function _msgTimestamp() internal view returns (uint256) {
        this;
        return block.timestamp;
    }

    receive() external payable { }

    event CreateContext(address contextAddress, address contextCreator);
}

contract Manageable is Callable {
    address private _executiveManager;
    mapping(address => bool) private _isManager;
    address[] private _managers;

    bool private _managementIsLocked = false;
    uint256 private _managementUnlockTime = 0;
    uint256 private _maxNumberOfManagers = 10;

    constructor () {
        _executiveManager = _contextCreator();
        _isManager[_executiveManager] = true;
        _managers.push(_executiveManager);

        emit ManagerAdded(_executiveManager);
        emit ExecutiveManagerChanged(address(0), _executiveManager);
    }

    function executiveManager() public view returns (address) {
        return _executiveManager;
    }

    function isManager(address account) public view returns (bool) {
        return _isManager[account];
    }

    function managementIsLocked() public view returns (bool) {
        return _managementIsLocked;
    }

    function timeToManagementUnlock() public view returns (uint256) {
        return block.timestamp >= _managementUnlockTime ? 0 : _managementUnlockTime - block.timestamp;
    }
    
    function addManager(address newManager) public onlyExecutive() returns (bool) {
        require(!_isManager[newManager], "Account is already a manager");
        require(newManager != address(0), "0 address cannot be made manager");
        require(_managers.length <= _maxNumberOfManagers, "max number of managers reached");

        _isManager[newManager] = true;
        _managers.push(newManager);

        emit ManagerAdded(newManager);

        return true;
    }

    function removeManager(address managerToRemove) public onlyExecutive() returns (bool) {
        require(_isManager[managerToRemove], "Account is already not a manager");
        require(managerToRemove != _executiveManager, "Executive manager cannot be removed");

        _isManager[managerToRemove] = false;
        for(uint256 i = 0; i < _managers.length; i++) {
            if(_managers[i] == managerToRemove){
                _managers[i] = _managers[_managers.length - 1];
                _managers.pop();
                break;
            }
        }

        emit ManagerRemoved(managerToRemove);

        return true;
    }

    function changeExecutiveManager(address newExecutiveManager) public onlyExecutive() returns (bool) {
        require(newExecutiveManager != _executiveManager, "Manager is already the executive");

        if(!_isManager[newExecutiveManager]){
            _isManager[newExecutiveManager] = true;
            emit ManagerAdded(newExecutiveManager);
        }
        _executiveManager = newExecutiveManager;

        emit ExecutiveManagerChanged(_executiveManager, newExecutiveManager);

        return true;
    }

    function lockManagement(uint256 lockDuration) public onlyExecutive() returns (bool) {
        _managementIsLocked = true;
        _managementUnlockTime = block.timestamp + lockDuration;

        emit ManagementLocked(lockDuration);

        return true;
    }

    function unlockManagement() public onlyExecutive() returns (bool) {
        _managementIsLocked = false;
        _managementUnlockTime = 0;

        emit ManagementUnlocked();

        return true;
    }

    function renounceManagement() public onlyExecutive() returns (bool) {
        while(_managers.length > 0) {
            _isManager[_managers[_managers.length - 1]] = false;

            emit ManagerRemoved(_managers[_managers.length - 1]);

            if(_managers[_managers.length - 1] == _executiveManager){
                emit ExecutiveManagerChanged(_executiveManager, address(0));
                _executiveManager = address(0);
            }

            _managers.pop();
        }

        emit ManagementRenounced();

        return true;
    }

    event ManagerAdded(address addedManager);
    event ManagerRemoved(address removedManager);
    event ExecutiveManagerChanged(address indexed previousExecutiveManager, address indexed newExecutiveManager);
    event ManagementLocked(uint256 lockDuration);
    event ManagementUnlocked();
    event ManagementRenounced();

    modifier onlyExecutive() {
        require(_msgSender() == _executiveManager, "Caller is not the executive manager");
        require(!_managementIsLocked || block.timestamp >= _managementUnlockTime, "Management is locked");
        _;
    }

    modifier onlyManagement() {
        require(_isManager[_msgSender()], "Caller is not a manager");
        require(!_managementIsLocked, "Management is locked");
        _;
    }
}

interface IBEP20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);
    function burn(uint256 amount) external;

    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);
}

library SafeMath {

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        if(b >= a){
            return 0;
        }
        uint256 c = a - b;
        return c;
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0 || b == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "multiplication overflow");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "division by zero");
        uint256 c = a / b;
        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0, "modulo by zero");
        return a % b;
    }

    function log10(uint256 value) internal pure returns (uint256) {
        uint256 result = 0;
        unchecked {
            if (value >= 10 ** 64) {
                value /= 10 ** 64;
                result += 64;
            }
            if (value >= 10 ** 32) {
                value /= 10 ** 32;
                result += 32;
            }
            if (value >= 10 ** 16) {
                value /= 10 ** 16;
                result += 16;
            }
            if (value >= 10 ** 8) {
                value /= 10 ** 8;
                result += 8;
            }
            if (value >= 10 ** 4) {
                value /= 10 ** 4;
                result += 4;
            }
            if (value >= 10 ** 2) {
                value /= 10 ** 2;
                result += 2;
            }
            if (value >= 10 ** 1) {
                result += 1;
            }
        }
        return result;
    }

}

contract AjaxLottery is Manageable {
    using SafeMath for uint256;

    uint256 private constant MAX = ~uint256(0);
    address public _deadAddress = 0x000000000000000000000000000000000000dEaD;

    uint256 private lotteryNumber;
    mapping (uint256 => mapping(uint8 => address[])) private winners;
    mapping (uint8 => uint256) public prizeBaskets;

    address private bigPrizeWinner;
    address private prevBigPrizeWinner;

    uint256 private currWinnerTicket;

    uint256[] private prevTickets;
    uint256 private maxNumberOfPrevTicketSave;

    mapping (uint256 => mapping(uint256 => bool)) private usedTicketNumbers;
    uint256 private numOfPastBigPrizeWinners;

    uint256 public totalCurrentPrize;

    uint256 private numberOfPrevParticipants;

    address private AJAXContractAddress = 0xD574E79B38923d51ff5Bf5bccb97E5b8aDe45182;

    address private ownerAddress;

    bool public isLotteryUnderway;

    uint8 public hardness;

    uint8 private numberOfBaskets;
    uint256[] private winnerPercentages;

    uint256 public burnAndReferralsPercentage;

    constructor() {
        ownerAddress = _msgSender();
        totalCurrentPrize = 0;
        bigPrizeWinner = _deadAddress;
        numberOfPrevParticipants = 0;
        maxNumberOfPrevTicketSave = 20;
        numOfPastBigPrizeWinners = 0;
        isLotteryUnderway = false;
        hardness = 5;
        numberOfBaskets = 3;
        winnerPercentages.push(30);
        winnerPercentages.push(20);
        winnerPercentages.push(10);
        lotteryNumber = 0;
        burnAndReferralsPercentage = 40;
    }

    function insertInPrevTickets(uint256 ticketNumber) internal {
        if (prevTickets.length == maxNumberOfPrevTicketSave) {
            for (uint8 i = 1; i < maxNumberOfPrevTicketSave; i++)
                prevTickets[i - 1] = prevTickets[i];
            prevTickets[maxNumberOfPrevTicketSave - 1] = ticketNumber;
        }
        else {
            prevTickets.push(ticketNumber);
        }
        usedTicketNumbers[numOfPastBigPrizeWinners][ticketNumber] = true;
    }

    function random() private view returns (uint) {
        uint256 randNum = uint(keccak256(abi.encodePacked(block.prevrandao, block.timestamp, numberOfBaskets, lotteryNumber, numberOfPrevParticipants))) % 10 ** hardness;
        while (randNum < 10 ** (hardness - 1))
            randNum *= 10;
        
        uint8 salt = 0;
        while (usedTicketNumbers[numOfPastBigPrizeWinners][randNum]) {
            randNum = uint(keccak256(abi.encodePacked(block.prevrandao, block.timestamp, numberOfBaskets, lotteryNumber, numberOfPrevParticipants, salt))) % 10 ** hardness;
            while (randNum < 10 ** (hardness - 1))
                randNum *= 10;
            salt += 1;
        }
        return randNum;
    }

    function createNewLottery() public onlyManagement() {
        require(!isLotteryUnderway, 'There is an active lottery.');

        uint256 newRand = random();

        totalCurrentPrize = 0;

        bigPrizeWinner = _deadAddress;

        currWinnerTicket = newRand;
        isLotteryUnderway = true;

        emit LotteryStarted();
    }

    function toString(uint256 value) internal pure returns (string memory) {
        unchecked {
            uint256 length = SafeMath.log10(value) + 1;
            string memory buffer = new string(length);
            uint256 ptr;
            /// @solidity memory-safe-assembly
            assembly {
                ptr := add(buffer, add(32, length))
            }
            while (true) {
                ptr--;
                /// @solidity memory-safe-assembly
                assembly {
                    mstore8(ptr, byte(mod(value, 10), "0123456789abcdef"))
                }
                value /= 10;
                if (value == 0) break;
            }
            return buffer;
        }
    }

    function getNumberOfSimilarities(uint256 num1, uint256 num2) internal view returns(uint8) {
        bytes memory num1Str = bytes(toString(num1));
        bytes memory num2Str = bytes(toString(num2));

        uint8 totalSim = 0;
        for (uint8 i = 0; i < hardness; i++) {
            if (num1Str[i] == num2Str[i])
                totalSim += 1;
        }

        return totalSim;
    }

    function addParticipant(address participant, uint256 amount, uint256 ticketNumber) public onlyManagement() {
        require(isLotteryUnderway, 'No active lottery.');
        require(ticketNumber >= 10 ** (hardness - 1), 'Ticket does not have valid number of digits. Check hardness.');
        require(ticketNumber < 10 ** hardness, 'Ticket does not have valid number of digits. Check hardness.');

        uint8 simsWithWinTicket = getNumberOfSimilarities(currWinnerTicket, ticketNumber);

        for (uint8 i = 0; i < numberOfBaskets; i++) {
            if (i == 0) {
                if (simsWithWinTicket == hardness) {
                    bigPrizeWinner = participant;
                    break;
                }
            }
            else {
                if (simsWithWinTicket == (hardness - i)) {
                    winners[lotteryNumber][i].push(participant);
                    break;
                }
            }
        }

        totalCurrentPrize = totalCurrentPrize.add(amount);
        numberOfPrevParticipants += 1;

        emit ParticipantAdded(participant, amount, ticketNumber);
    }

    function endLottery() public onlyManagement() {
        require(isLotteryUnderway, 'No active lottery');

        IBEP20 AjaxInterface = IBEP20(AJAXContractAddress);

        for (uint8 i = 0; i < numberOfBaskets; i++) {
            uint256 prizeShare = uint256(totalCurrentPrize.mul(winnerPercentages[i]).div(100));
            uint256 totalPrize = prizeShare.add(prizeBaskets[i]);

            if (i == 0) {
                if (bigPrizeWinner != _deadAddress) {
                    AjaxInterface.transfer(bigPrizeWinner, totalPrize);

                    prizeBaskets[i] = 0;
                    numOfPastBigPrizeWinners += 1;
                }
                else {
                    prizeBaskets[i] += prizeShare;
                }
            }
            else {
                if (winners[lotteryNumber][i].length != 0) {
                    uint256 participantBite = uint256(totalPrize.div(winners[lotteryNumber][i].length));

                    for (uint8 j = 0; j < winners[lotteryNumber][i].length; j++) {
                        AjaxInterface.transfer(winners[lotteryNumber][i][j], participantBite);
                    }

                    prizeBaskets[i] = 0;
                }
                else {
                    prizeBaskets[i] = prizeBaskets[i].add(prizeShare);
                }
            }
        }

        prevBigPrizeWinner = bigPrizeWinner;

        insertInPrevTickets(currWinnerTicket);

        isLotteryUnderway = false;
        lotteryNumber += 1;

        emit LotteryEnded(currWinnerTicket);
    }

    function getPrevWinnerTickets() public view returns(uint256[] memory){
        return prevTickets;
    }

    function getPrevBigPrizeWinner() public view returns(address){
        return prevBigPrizeWinner;
    }

    function getPrevFirstPrizeWinners(uint8 dissimilarity) public view returns(address[] memory){
        require(dissimilarity > 1, "Dissimalirty must be higher than 1. For big prize winner use getPrevBigPrizeWinner");
        return winners[lotteryNumber - 1][dissimilarity];
    }

    function setHardness(uint8 newHardness) public onlyManagement() {
        require(!isLotteryUnderway, 'There is an active lottery.');
        require(newHardness > 0, "Length of winner number cannot be less than 1");
        hardness = newHardness;
    }

    function setPercentages(uint256[] calldata newPercentages) public onlyManagement() {
        require(!isLotteryUnderway, 'There is an active lottery.');
        require (newPercentages.length > 1, "At least two shares must be specified");
        uint256 sharesSum = 0;
        for (uint8 i = 0; i < newPercentages.length; i++)
            sharesSum += newPercentages[i];

        require (sharesSum == 100, "Total shares must add to 100.");

        burnAndReferralsPercentage = newPercentages[newPercentages.length - 1];

        uint8 newBasketNum = uint8(newPercentages.length - 1);
        if (newBasketNum < numberOfBaskets) {
            for (uint8 i = newBasketNum; i < numberOfBaskets; i++) {
                uint256 distributeAmount = uint256(prizeBaskets[i].div(newBasketNum));

                for (uint8 j = 0; j < newBasketNum; j++)
                    prizeBaskets[j] = prizeBaskets[j].add(distributeAmount);
                
                prizeBaskets[i] = 0;
            }
        }

        numberOfBaskets = newBasketNum;

        delete winnerPercentages;
        for (uint8 i = 0; i < numberOfBaskets; i++) 
            winnerPercentages.push(newPercentages[i]);
    }

    event LotteryStarted();
    event LotteryEnded(uint256 winningNumber);
    event ParticipantAdded(address participant, uint256 amount, uint256 ticketNumber);

}