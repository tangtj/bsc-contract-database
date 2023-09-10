// SPDX-License-Identifier: MIT
// Raffle for Promotion with Reward Token XBITS Token BITSBAI
// Website: https://bitsbai.xyz/
// Other Social just visit our website to check.		

pragma solidity 0.8.19;

interface IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function transfer(address recipient, uint256 amount) external returns (bool);
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

contract Raffle is Ownable {
    struct Winner {
        address winner;
        uint prize;
		uint prize2;
        uint drawNumber;
        uint drawDate;
        uint totalTicketsSold;
        uint totalPlayers;
        uint totalPrizePool;
    }
    
    address payable[] public players;
    IERC20 public token; //token for buying the ticket and it's main reward
	IERC20 public token2; //token for consolation and additional prize
	address public devWallet = 0x104b222D10C6E17b378aB3b60faB4E0be0323Eb3;
	address public designatedWallet = 0x104b222D10C6E17b378aB3b60faB4E0be0323Eb3;
	//bool public enableClaimBack = false; //no claimback as this has consolidation prize
	uint public entryFee = 1 * 10**18;
    uint public maxTickets = 20;
    uint public min_Players = 2;
    uint public drawInterval = 23 hours; // intended 24hrs but made 23 hours to make it little flexible
    uint public lastDraw;
    uint public tax = 20; // 20%
    uint public constant MAX_TAX = 20; // 20%
	uint public minTickets = 50;
	uint public prize2 = 100000 * 10**18; //main prize
	uint public consoPrize2 = 1000 * 10**18; //consolidation prize XBT
		    
	address public tokenAddress = 0x78a7bC13d606f8c6D408C2918d31e74c9F0f37c1; //BAI Token
	uint public minHoldings = 10000 * (10 ** 18);
	bool public minHoldingsCheckEnabled = false;
    bool public isPaused = false;

    Winner[] public winners;

    address[] public whitelistedAddresses;
	mapping(address => bool) public whitelist;
    mapping(address => bool) public receivedConsoPrize;
	bool public isWhitelistEnabled = true;
    
    event PlayerEntered(address indexed player, uint tickets);
    event WinnerDrawn(address indexed winner, uint prize, uint drawNumber);
    
    function setToken(address _token) public onlyOwner {
        token = IERC20(_token);
    }
	
	function setToken2(address _token2) public onlyOwner {
    token2 = IERC20(_token2);
	}
    
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

	//this is for BAI Token Holdings - input BAI Token Address Here
	function setTokenAddress(address _tokenAddress) public onlyOwner {
		tokenAddress = _tokenAddress;
	}

	function setMinHoldings(uint _minHoldings) public onlyOwner {
		minHoldings = _minHoldings;
	}
	
	function setPrize2(uint _prize2) public onlyOwner {
    prize2 = _prize2;
	}
		
	function setconsoPrize2(uint _consoPrize2) public onlyOwner {
		consoPrize2 = _consoPrize2;
	}

	function setMinHoldingsCheckEnabled(bool _enabled) public onlyOwner {
		minHoldingsCheckEnabled = _enabled;
	}

	function enter(uint _tickets) public {
		require(!isPaused, "The contract is paused");																	
		require(_tickets > 0 && _tickets <= maxTickets, "Invalid number of tickets");
        require(!isWhitelistEnabled || whitelist[msg.sender], "You are not whitelisted");
		require(token.transferFrom(msg.sender, address(this), entryFee * _tickets), "Token transfer failed. Make sure you have approved the contract to spend the required amount of tokens on your behalf.");
		// Check if the sender wallet has holdings of the specified token with minimum value
		if (minHoldingsCheckEnabled) {
			IERC20 tokenHoldingReq = IERC20(tokenAddress);
			require(tokenHoldingReq.balanceOf(msg.sender) >= minHoldings, "Insufficient token holdings");
		}

		require(getTicketCount(msg.sender) + _tickets <= maxTickets, "Total tickets can't be more than maxTickets limit");

		// Send a portion of the entry fee to the dev wallet
		if (devWallet != address(0)) {
			require(token.transfer(devWallet, (entryFee * _tickets * tax) / 100), "Token transfer to dev wallet failed");
		}
		
		for (uint i = 0; i < _tickets; i++) {
			players.push(payable(msg.sender));
		}
		
		// Check if there are enough tokens in the contract
		require(token2.balanceOf(address(this)) >= consoPrize2, "Not enough tokens in the contract to send the consolation prize");
		
        // Check if the wallet has already received the consolation prize
        if (!receivedConsoPrize[msg.sender]) {
            // Send the consolation prize to the user
            token2.transfer(msg.sender, consoPrize2);
            
            // Mark the wallet as having received the consolation prize
            receivedConsoPrize[msg.sender] = true;
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

			uint prize = token.balanceOf(address(this));

			token.transfer(winnerAddress, prize);
			
			// Check if there are enough tokens in the contract
			require(token2.balanceOf(address(this)) >= prize2, "Not enough tokens in the contract to send the prize to the winner");
			
			// Transfer the configured amount of the second token to the winner
			token2.transfer(winnerAddress, prize2);

			// Store information about the winner
			winners.push(Winner(winnerAddress, prize, prize2, winners.length + 1, block.timestamp, players.length, getUniquePlayerCount(), prize));

            // Reset the receivedConsoPrize mapping for all players
            for (uint i = 0; i < players.length; i++) {
                receivedConsoPrize[players[i]] = false;
            }
    
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
        return token.balanceOf(address(this));
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

	/* function setEnableClaimBack(bool _enableClaimBack) public onlyOwner {
		enableClaimBack = _enableClaimBack;
		emit EnableClaimBackUpdated(_enableClaimBack);
	} */

	/*  function claimBack() public {
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
		require(token.transfer(msg.sender, amountToReturn), "Token transfer failed");
		
		emit ClaimBack(msg.sender, ticketCount);
	} */


	function getAllWinnersRec() public view returns (uint, uint, address, uint, uint, uint, uint) {
		uint prize = getPrize();
		uint ticketCount = players.length;
		address lastWinnerAddress;
		uint lastWinnerPrize;
		uint lastWinnerPrize2;
		uint lastDrawNumber;
		uint lastTicketCount;
		
		if (winners.length > 0) {
			Winner memory lastWinner = winners[winners.length - 1];
			lastWinnerAddress = lastWinner.winner;
			lastWinnerPrize = lastWinner.prize;
			lastWinnerPrize2 = lastWinner.prize2;
			lastDrawNumber = lastWinner.drawNumber;
			lastTicketCount = lastWinner.totalTicketsSold;
		}
		
		return (prize, ticketCount, lastWinnerAddress, lastWinnerPrize, lastWinnerPrize2, lastDrawNumber, lastTicketCount);
	}
	
	function meetDrawCheck() public view returns (bool) {
        return players.length >= minTickets && getUniquePlayerCount() >= min_Players && block.timestamp >= lastDraw + drawInterval;
    }

	function toggleWhitelist() public onlyOwner {
		isWhitelistEnabled = !isWhitelistEnabled;
	}
	
	function addToWhitelist(address[] memory _wallets) public onlyOwner {
    for (uint i = 0; i < _wallets.length; i++) {
			address wallet = _wallets[i];
			if (!whitelist[wallet]) {
				whitelist[wallet] = true;
				whitelistedAddresses.push(wallet);
			}
		}
	}
	
    function removeFromWhitelist(address _wallet) public onlyOwner {
        if (whitelist[_wallet]) {
            whitelist[_wallet] = false;
            for (uint i = 0; i < whitelistedAddresses.length; i++) {
                if (whitelistedAddresses[i] == _wallet) {
                    whitelistedAddresses[i] = whitelistedAddresses[whitelistedAddresses.length - 1];
                    whitelistedAddresses.pop();
                    break;
                }
            }
        }
    }

    function getWhitelist() public view returns (address[] memory) {
        return whitelistedAddresses;
    }

	function deleteAllWhitelist() public onlyOwner {
		for (uint i = 0; i < whitelistedAddresses.length; i++) {
			whitelist[whitelistedAddresses[i]] = false;
		}
		delete whitelistedAddresses;
	}

}