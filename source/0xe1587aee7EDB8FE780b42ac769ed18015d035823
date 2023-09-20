// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

library Strings {
    function toString(uint256 value) internal pure returns (string memory) {
        if (value == 0) {
            return "0";
        }
        uint256 temp = value;
        uint256 digits;
        while (temp != 0) {
            digits++;
            temp /= 10;
        }
        bytes memory buffer = new bytes(digits);
        while (value != 0) {
            digits -= 1;
            buffer[digits] = bytes1(uint8(48 + uint256(value % 10)));
            value /= 10;
        }
        return string(buffer);
    }
}

// Define the IERC20 interface
interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    // Add other ERC20 functions here...
}

contract PepesPick6Lottery {
    string public  GameName;
    string public HowToPlay;
    address public owner;
    uint256 public jackpot;
    uint256 public ticketPrice;
    uint256 public lastDrawTime;
    bool public isDrawingNumbers = false;

    uint256 public constant weeklyDrawInterval = 1 days;

    uint256[] public winningNumbers;
     
    string public name = "Pepes Pick 6 Lottery";
    string public symbol = "Pepes Pick 6 Lottery";

    struct Ticket {
        uint256[] numbers;
        address player;
    }

    Ticket[] public tickets;

    address public tokenContractAddress;

    struct RoundWinner {
        address winnerAddress;
        uint256 tokensWon;
        uint256 drawDate;
    }

    RoundWinner[] public lastRoundWinners;
mapping(address => uint256) public addressToTicketCount;

    // Modifier to restrict access to certain functions
modifier onlyOwner() {
    require(msg.sender == owner, "Only the owner can call this function");
    _;
}


    // Constructor to initialize the contract
    constructor() {
        owner = msg.sender;
       ticketPrice = 100000;
       tokenContractAddress = 0x44C20e6001a17b72fB6A05bdF4a1ABE70A1BC033;
       lastDrawTime = uint256(block.timestamp);
       // Initialize any additional variables here...
    }



 function DonateLiquidity(uint256 amount) external nonReentrant {
        require(IERC20(tokenContractAddress).transferFrom(msg.sender, address(this), amount), "Token transfer failed.");
        jackpot += amount;
    }


    function setRules(string memory rules) external onlyOwner{

        HowToPlay = rules;

    }

    function setTicketPrice(uint256 price) external  onlyOwner nonReentrant{

        ticketPrice = price;

    }


     function setTokenContracts(address contractAddress) external  onlyOwner nonReentrant{

        tokenContractAddress = contractAddress;

    }


    function setName(string memory newname) external  onlyOwner{

        GameName = newname;

    }


    function buyTicket(uint256 num1, uint256 num2, uint256 num3, uint256 num4, uint256 num5, uint256 num6) external nonReentrant  {
    require(areNumbersUnique(num1, num2, num3, num4, num5, num6), "You must pick 6 different numbers.");
    require(IERC20(tokenContractAddress).transferFrom(msg.sender, address(this), ticketPrice), "Ticket purchase failed.");

    uint256[] memory chosenNumbers = new uint256[](6);
    chosenNumbers[0] = num1;
    chosenNumbers[1] = num2;
    chosenNumbers[2] = num3;
    chosenNumbers[3] = num4;
    chosenNumbers[4] = num5;
    chosenNumbers[5] = num6;

    Ticket memory newTicket = Ticket({
        numbers: chosenNumbers,
        player: msg.sender
    });
    tickets.push(newTicket);
    addressToTicketCount[msg.sender]++;

    // Add the ticket price to the prize pool
    jackpot += ticketPrice;
}


function getLastWinningNumbersAsString() external view returns (string memory) {
    string memory result = "";
    
    for (uint256 i = 0; i < winningNumbers.length; i++) {
        if (i > 0) {
            result = string(abi.encodePacked(result, ",", uint256ToString(winningNumbers[i])));
        } else {
            result = uint256ToString(winningNumbers[i]);
        }
    }
    
    return result;
}

   
    // Function to calculate the CST date and time for the next draw date
    function getNextDrawDateTimeCST() public view returns (string memory) {
      //  int256 cstOffset = -6 hours; // CST is UTC-6
        uint256 cstTimestamp = lastDrawTime - (21600) + weeklyDrawInterval ; //int256(block.timestamp) + cstOffset;

        // Convert the CST time to a readable string format
        uint256 unixTimestamp = uint256(cstTimestamp);
        uint256 unixDay = unixTimestamp / 86400; // Number of days since Unix epoch
        uint256 secondsInDay = unixTimestamp % 86400; // Number of seconds in the current day
        uint256 currentYear;
        uint256 currentMonth;
        uint256 currentDay;
        uint256 currentHour;
        uint256 currentMinute;

        // Calculate the current year, month, and day
        uint256 secondsAccountedFor = 0;
        bool isLeapYear = false;

        for (currentYear = 1970; ; currentYear++) {
            isLeapYear = (currentYear % 4 == 0 && (currentYear % 100 != 0 || currentYear % 400 == 0));

            if (isLeapYear) {
                if (secondsAccountedFor + 31622400 > unixDay * 86400 + secondsInDay) {
                    break;
                }
                secondsAccountedFor += 31622400;
            } else {
                if (secondsAccountedFor + 31536000 > unixDay * 86400 + secondsInDay) {
                    break;
                }
                secondsAccountedFor += 31536000;
            }
        }

        for (currentMonth = 1; ; currentMonth++) {
            uint256 daysInMonth = 0;
            if (currentMonth == 2) {
                if (isLeapYear) {
                    daysInMonth = 29;
                } else {
                    daysInMonth = 28;
                }
            } else if (
                currentMonth == 4 ||
                currentMonth == 6 ||
                currentMonth == 9 ||
                currentMonth == 11
            ) {
                daysInMonth = 30;
            } else {
                daysInMonth = 31;
            }

            if (secondsAccountedFor + daysInMonth * 86400 > unixDay * 86400 + secondsInDay) {
                break;
            }
            secondsAccountedFor += daysInMonth * 86400;
        }

        currentDay = (unixDay * 86400 + secondsInDay - secondsAccountedFor) / 86400 + 1;

        // Calculate the current hour and minute
        currentHour = (secondsInDay / 3600) % 24;
        currentMinute = (secondsInDay / 60) % 60;
        currentHour++;
        string memory month;
        if (currentMonth == 1) month = "January";
        else if (currentMonth == 2) month = "February";
        else if (currentMonth == 3) month = "March";
        else if (currentMonth == 4) month = "April";
        else if (currentMonth == 5) month = "May";
        else if (currentMonth == 6) month = "June";
        else if (currentMonth == 7) month = "July";
        else if (currentMonth == 8) month = "August";
        else if (currentMonth == 9) month = "September";
        else if (currentMonth == 10) month = "October";
        else if (currentMonth == 11) month = "November";
        else month = "December";

        string memory ampm = "AM";
        if (currentHour >= 12) {
            ampm = "PM";
            if (currentHour > 12) {
                currentHour -= 12;
            }
        }

        string memory cstTime = string(abi.encodePacked(
            month,
            " ",
            Strings.toString(currentDay),
            ", ",
            Strings.toString(currentYear),
            " ",
            Strings.toString(currentHour),
            ":",
            Strings.toString(currentMinute),
            " ",
            ampm,
            " CST"
        ));
        return cstTime;
    }



function getLastDrawTimeCST() external view returns (string memory) {
    int256 cstOffset = -6 hours; // CST is UTC-6
    int256 cstTimestamp = int256(block.timestamp) + cstOffset;
    
    // Convert the CST time to a readable string format
    uint256 unixTimestamp = uint256(cstTimestamp);
    uint256 unixDay = unixTimestamp / 86400; // Number of days since Unix epoch
    uint256 secondsInDay = unixTimestamp % 86400; // Number of seconds in the current day
    uint256 currentYear;
    uint256 currentMonth;
    uint256 currentDay;

    // Calculate the current year, month, and day
    uint256 secondsAccountedFor = 0;
    bool isLeapYear = false;

    for (currentYear = 1970; ; currentYear++) {
        isLeapYear = (currentYear % 4 == 0 && (currentYear % 100 != 0 || currentYear % 400 == 0));

        if (isLeapYear) {
            if (secondsAccountedFor + 31622400 > unixDay * 86400 + secondsInDay) {
                break;
            }
            secondsAccountedFor += 31622400;
        } else {
            if (secondsAccountedFor + 31536000 > unixDay * 86400 + secondsInDay) {
                break;
            }
            secondsAccountedFor += 31536000;
        }
    }

    for (currentMonth = 1; ; currentMonth++) {
        uint256 daysInMonth = 0;
        if (currentMonth == 2) {
            if (isLeapYear) {
                daysInMonth = 29;
            } else {
                daysInMonth = 28;
            }
        } else if (
            currentMonth == 4 ||
            currentMonth == 6 ||
            currentMonth == 9 ||
            currentMonth == 11
        ) {
            daysInMonth = 30;
        } else {
            daysInMonth = 31;
        }

        if (secondsAccountedFor + daysInMonth * 86400 > unixDay * 86400 + secondsInDay) {
            break;
        }
        secondsAccountedFor += daysInMonth * 86400;
    }

    currentDay = (unixDay * 86400 + secondsInDay - secondsAccountedFor) / 86400 + 1;
    
    string memory month;
    if (currentMonth == 1) month = "January";
    else if (currentMonth == 2) month = "February";
    else if (currentMonth == 3) month = "March";
    else if (currentMonth == 4) month = "April";
    else if (currentMonth == 5) month = "May";
    else if (currentMonth == 6) month = "June";
    else if (currentMonth == 7) month = "July";
    else if (currentMonth == 8) month = "August";
    else if (currentMonth == 9) month = "September";
    else if (currentMonth == 10) month = "October";
    else if (currentMonth == 11) month = "November";
    else month = "December";

    string memory cstTime = string(abi.encodePacked(month, " ", Strings.toString(currentDay), ", ", Strings.toString(currentYear), " CST"));
    return cstTime;
}


function uint256ToString(uint256 number) internal pure returns (string memory) {
    if (number == 0) {
        return "0";
    }
    
    uint256 temp = number;
    uint256 digits;
    
    while (temp != 0) {
        digits++;
        temp /= 10;
    }
    
    bytes memory buffer = new bytes(digits);
    while (number != 0) {
        digits -= 1;
        buffer[digits] = bytes1(uint8(48 + uint256(number % 10)));
        number /= 10;
    }
    
    return string(buffer);
}


// Function to check if a set of numbers contains unique values
function areNumbersUnique(
    uint256 num1, uint256 num2, uint256 num3, uint256 num4, uint256 num5, uint256 num6
) internal pure returns (bool) {
    return (num1 != num2 && num1 != num3 && num1 != num4 && num1 != num5 && num1 != num6 &&
            num2 != num3 && num2 != num4 && num2 != num5 && num2 != num6 &&
            num3 != num4 && num3 != num5 && num3 != num6 &&
            num4 != num5 && num4 != num6 &&
            num5 != num6);
}

function getLastRoundWinners() external view returns (RoundWinner[] memory) {
    return lastRoundWinners;
}


function drawNumbers() external nonReentrant {
    string memory next = getNextDrawDateTimeCST() ;
    require(block.timestamp >= lastDrawTime + weeklyDrawInterval,  string(abi.encodePacked("It's not time for the next draw yet." , next))   );
    isDrawingNumbers = true;
    // Perform the drawing of winning numbers (for demonstration, we generate random numbers)
    // Replace this with a more secure and random number generation mechanism
        delete winningNumbers;

    for (uint i = 0; i < 6; i++) {
        winningNumbers.push(uint256(keccak256(abi.encodePacked(block.timestamp, i))) % 50 + 1);
    }

    address[] memory winners;

    for (uint i = 0; i < tickets.length; i++) {
        uint256 matchingNumbers = countMatchingNumbers(tickets[i].numbers);

        if (matchingNumbers >= 3) {
            winners = appendIfNotExists(winners, tickets[i].player);
        }
    }

    if (winners.length > 0) {
        uint256 jackpotShare = (30 * jackpot) / 100;
        uint256 fiveNumbersShare = (5 * jackpot) / 100;
        uint256 fourNumbersShare = (7 * jackpot) / 100;
        uint256 threeNumbersShare = (10 * jackpot) / 100;

        if (winners.length > 1) {
            jackpotShare /= uint256(winners.length);
            fiveNumbersShare /= uint256(winners.length);
            fourNumbersShare /= uint256(winners.length);
            threeNumbersShare /= uint256(winners.length);
        }

        for (uint i = 0; i < winners.length; i++) {
            uint256 matchingNumbers = countMatchingNumbers(getPlayerNumbers(winners[i]));

            if (matchingNumbers == 6) {
                // Jackpot winner
                IERC20(tokenContractAddress).transfer(winners[i], jackpotShare);
            } else if (matchingNumbers == 5) {
                // 5 numbers match
                IERC20(tokenContractAddress).transfer(winners[i], fiveNumbersShare);
            } else if (matchingNumbers == 4) {
                // 4 numbers match
                IERC20(tokenContractAddress).transfer(winners[i], fourNumbersShare);
            } else if (matchingNumbers == 3) {
                // 3 numbers match
                IERC20(tokenContractAddress).transfer(winners[i], threeNumbersShare);
            }

            // Save information about the winners of the current round
            RoundWinner memory roundWinner = RoundWinner({
                winnerAddress: winners[i],
                tokensWon: matchingNumbers * jackpotShare / 3, // Calculate actual winnings
                drawDate: block.timestamp
            });

            lastRoundWinners.push(roundWinner);
        }

        isDrawingNumbers = false;
    }

    // Reset the lottery for the next round
    delete tickets;
    lastDrawTime = block.timestamp;
}


// Function to count the number of matching numbers between a player's numbers and the winning numbers
function countMatchingNumbers(uint256[] memory _playerNumbers) internal view returns (uint256) {
    require(_playerNumbers.length == 6, "Player must choose exactly 6 numbers.");

    uint256 count = 0;
    for (uint i = 0; i < 6; i++) {
        for (uint j = 0; j < 6; j++) {
            if (_playerNumbers[i] == winningNumbers[j]) {
                count++;
            }
        }
    }

    return count;
}


// Function to get the numbers chosen by a specific player
function getPlayerNumbers(address player) public view returns (uint256[] memory) {
    for (uint i = 0; i < tickets.length; i++) {
        if (tickets[i].player == player) {
            return tickets[i].numbers;
        }
    }

    // Return an empty array if the player has no tickets
    uint256[] memory emptyArray = new uint256[](6);
    return emptyArray;
}


// Function to get the current prize pool amount
function getCurrentPrizePool() external view returns (uint256) {
    return jackpot;
}

// Function to allow eligible users to claim a free ticket with 6 different numbers
function claimFreeTicket(uint256 num1, uint256 num2, uint256 num3, uint256 num4, uint256 num5, uint256 num6) external nonReentrant  {
    require(!hasFreeTicket(msg.sender), "You already have a free ticket.");
    require(areNumbersUnique(num1, num2, num3, num4, num5, num6), "You must pick 6 different numbers.");
    
    // Assign the chosen numbers to the user and create a free ticket
    uint256[] memory chosenNumbers = new uint256[](6);
    chosenNumbers[0] = num1;
    chosenNumbers[1] = num2;
    chosenNumbers[2] = num3;
    chosenNumbers[3] = num4;
    chosenNumbers[4] = num5;
    chosenNumbers[5] = num6;
    
    Ticket memory newTicket = Ticket({
        numbers: chosenNumbers,
        player: msg.sender
    });
        addressToTicketCount[msg.sender]++;

    tickets.push(newTicket);
}

// Function to check if a user already has a free ticket
function hasFreeTicket(address _user) internal view returns (bool) {
    return addressToTicketCount[_user] > 0;
}


bool private locked;

modifier nonReentrant() {
    require(!locked, "Reentrant call");
    locked = true;
    _;
    locked = false;
}

    // Add functions here...



// Helper function to append an address to an array if it doesn't already exist
function appendIfNotExists(address[] memory array, address element) internal pure returns (address[] memory) {
    for (uint i = 0; i < array.length; i++) {
        if (array[i] == element) {
            return array;
        }
    }
    address[] memory newArray = new address[](array.length + 1);
    for (uint i = 0; i < array.length; i++) {
        newArray[i] = array[i];
    }
    newArray[array.length] = element;
    return newArray;
}


}