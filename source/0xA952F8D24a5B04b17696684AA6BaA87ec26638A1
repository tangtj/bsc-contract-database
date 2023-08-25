// SPDX-License-Identifier: MIT
// RaffleBNB Token BITSBAI
// Website: https://bitsbai.xyz/
// Other Social just visit our website to check.

pragma solidity 0.8.19;

interface IERC20 {
    function balanceOf(address account) external view returns (uint256);
}

contract Ownable {
    address public owner;
    
    constructor() {
        owner = msg.sender;
    }
    
    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }
    
    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "New owner cannot be the zero address");
        owner = newOwner;
    }
}

contract RaffleBNB is Ownable {
    struct Winner {
        address winner;
        uint prize;
        uint drawNumber;
        uint drawDate;
        uint totalTicketsSold;
        uint totalPlayers;
        uint totalPrizePool;
    }
    
    address payable[] public players;
	address public devWallet = 0x104b222D10C6E17b378aB3b60faB4E0be0323Eb3;
	address public designatedWallet = 0x104b222D10C6E17b378aB3b60faB4E0be0323Eb3;
	bool public enableClaimBack = false;
	uint public entryFee = 0.1 * 10**18;
    uint public maxTickets = 20;
    uint public min_Players = 2;
    uint public drawInterval = 23 hours; // intended 24hrs but made 23 hours to make it little flexible
    uint public lastDraw;
    uint public tax = 10; // 15%
    uint public constant MAX_TAX = 20; // 20%
	uint public minTickets = 20;
	
	address public tokenAddress = 0xF1CD113CC6E01E336DEE56658592Dde766a33e81; //BAI Token
	uint public minHoldings = 10000 * (10 ** 18);
	bool public minHoldingsCheckEnabled = false;
    bool public isPaused = true;

    Winner[] public winners;

    event PlayerEntered(address indexed player, uint tickets);
    event WinnerDrawn(address indexed winner, uint prize, uint drawNumber);
	event EnableClaimBackUpdated(bool enableClaimBack);
	event ClaimBack(address indexed player, uint tickets);

	function setDevWallet(address _devWallet) public onlyOwner {
        devWallet = _devWallet;
    }

	function setDesignatedWallet(address _designatedWallet) public onlyOwner {
        designatedWallet = _designatedWallet;
    }

    function setDrawInterval(uint _drawInterval) public onlyOwner {
        drawInterval = _drawInterval;
    }

    function setTax(uint _tax) public onlyOwner {
        require(_tax <= MAX_TAX, "Tax cannot be greater than the maximum allowed value");
        tax = _tax;
    }

    function setMaxTickets(uint _maxTickets) public onlyOwner {
        maxTickets = _maxTickets;
    }

	function setmin_Players(uint _min_Players) public onlyOwner {
        min_Players = _min_Players;
    }

        function togglePause() public onlyOwner {
        isPaused = !isPaused;
    }

	function setTokenAddress(address _tokenAddress) public onlyOwner {
		tokenAddress = _tokenAddress;
	}

	function setMinHoldings(uint _minHoldings) public onlyOwner {
		minHoldings = _minHoldings;
	}

	function setMinHoldingsCheckEnabled(bool _enabled) public onlyOwner {
		minHoldingsCheckEnabled = _enabled;
	}

	function enter(uint _tickets) public payable {
        require(!isPaused, "The contract is paused");
		require(_tickets > 0 && _tickets <= maxTickets, "Invalid number of tickets");
		require(msg.value == entryFee * _tickets, "Incorrect amount of BNB sent");
		
		// Check if the sender wallet has holdings of the specified token with minimum value
		if (minHoldingsCheckEnabled) {
			IERC20 token = IERC20(tokenAddress);
			require(token.balanceOf(msg.sender) >= minHoldings, "Insufficient token holdings");
		}		

		// Send a portion of the entry fee to the dev wallet
		if (devWallet != address(0)) {
			payable(devWallet).transfer((msg.value * tax) / 100);
		}

		for (uint i = 0; i < _tickets; i++) {
			players.push(payable(msg.sender));
		}

		emit PlayerEntered(msg.sender, _tickets);
	}

	function setMinTickets(uint _minTickets) public onlyOwner {
			minTickets = _minTickets;
	}

	function drawWinner() public {
			require(msg.sender == owner || msg.sender == designatedWallet, "Only the owner or the designated wallet can call this function");
			require(getUniquePlayerCount() >= min_Players, "Not enough unique players in the raffle");
			require(players.length >= minTickets, "Not enough tickets sold");
			require(block.timestamp >= lastDraw + drawInterval, "Not enough time has passed since the last draw");

			uint index = random() % players.length;
			address payable winnerAddress = players[index];

			uint prize = address(this).balance;

			winnerAddress.transfer(prize);

			// Store information about the winner
			winners.push(Winner(winnerAddress, prize, winners.length + 1, block.timestamp, players.length, getUniquePlayerCount(), prize));

			// Reset the raffle
			delete players;

			// Update the timestamp of the last draw
			lastDraw = block.timestamp;

			emit WinnerDrawn(winnerAddress, prize, winners.length);
	}


	function getUniquePlayerCount() public view returns (uint) {
			uint count = 0;
			address[] memory seen;

			for (uint i = 0; i < players.length; i++) {
				if (!contains(seen, players[i])) {
					count++;
					seen = addToArray(seen, players[i]);
				}
			}

			return count;
	}

	function contains(address[] memory arr, address value) internal pure returns (bool) {
			for (uint i = 0; i < arr.length; i++) {
				if (arr[i] == value) {
					return true;
				}
			}

			return false;
	}

	function addToArray(address[] memory arr, address value) internal pure returns (address[] memory) {
			address[] memory newArr = new address[](arr.length + 1);

			for (uint i = 0; i < arr.length; i++) {
				newArr[i] = arr[i];
			}

			newArr[arr.length] = value;

			return newArr;
	}

    function getWinners() public view returns (Winner[] memory) {
        return winners;
    }

    function getTicketCount(address _player) public view returns (uint) {
        uint count = 0;

        for (uint i = 0; i < players.length; i++) {
            if (players[i] == _player) {
                count++;
            }
        }

        return count;
    }

    function getPlayerCount() public view returns (uint) {
        uint count = 0;

        for (uint i = 0; i < players.length; i++) {
                count++;
        }

        return count;
    }

    function getPrize() public view returns (uint) {
        return address(this).balance;
    }

    function random() private view returns (uint) {
        return uint(keccak256(abi.encodePacked(block.prevrandao, block.timestamp, players)));
    }

    function setEntryFee(uint _entryFee) public onlyOwner {
        entryFee = _entryFee;
    }

    function getLatestDrawNumber() public view returns (uint) {
    return winners.length;
    }

    function getLatestWinner() public view returns (Winner memory) {
        require(winners.length > 0, "No winners yet");
        return winners[winners.length - 1];
    }

    function setEnableClaimBack(bool _enableClaimBack) public onlyOwner {
        enableClaimBack = _enableClaimBack;
        emit EnableClaimBackUpdated(_enableClaimBack);
    }

    function claimBack() public {
        require(enableClaimBack, "Claim back is not enabled");
        uint ticketCount = getTicketCount(msg.sender);
        require(ticketCount > 0, "You do not have any tickets to claim back");

        // Remove the player's tickets
        for (uint j = 0; j < ticketCount; j++) {
            for (uint i = 0; i < players.length; i++) {
                if (players[i] == msg.sender) {
                    players[i] = players[players.length - 1];
                    players.pop();
                    break;
                }
            }
        }

        uint amountToReturn = (entryFee * ticketCount * (100 - tax)) / 100;
        payable(msg.sender).transfer(amountToReturn);

        emit ClaimBack(msg.sender, ticketCount);
    }


    function getAllWinnersRec() public view returns (uint, uint, address, uint, uint, uint) {
        uint prize = getPrize();
        uint ticketCount = players.length;
        address lastWinnerAddress;
        uint lastWinnerPrize;
        uint lastDrawNumber;
        uint lastTicketCount;

        if (winners.length > 0) {
            Winner memory lastWinner = winners[winners.length - 1];
            lastWinnerAddress = lastWinner.winner;
            lastWinnerPrize = lastWinner.prize;
            lastDrawNumber = lastWinner.drawNumber;
            lastTicketCount = lastWinner.totalTicketsSold;
        }

        return (prize, ticketCount, lastWinnerAddress, lastWinnerPrize, lastDrawNumber, lastTicketCount);
    }

    function meetDrawCheck() public view returns (bool) {
	return players.length >= minTickets && getUniquePlayerCount() >= min_Players && block.timestamp >= lastDraw + drawInterval;
	}
}