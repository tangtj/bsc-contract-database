// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

interface IERC165 {
    function supportsInterface(bytes4 interfaceId) external view returns (bool);
}

interface IERC721 is IERC165 {
    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);

    event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);

    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
 
    function balanceOf(address owner) external view returns (uint256 balance);

    function ownerOf(uint256 tokenId) external view returns (address owner);

    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId,
        bytes calldata data
    ) external;

    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

    function transferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

    function approve(address to, uint256 tokenId) external;

    function setApprovalForAll(address operator, bool _approved) external;

    function getApproved(uint256 tokenId) external view returns (address operator);

    function isApprovedForAll(address owner, address operator) external view returns (bool);
}

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

contract RaceGame {
    event RaceCreated(uint raceId);
    event JoinRace(uint raceId, address joined, uint nftJoined);
    event RaceStarted(uint raceId);
    event RaceFull(uint raceId);
    event RaceComplete(uint raceId, address winner, uint amount);

    address DEAD = 0x000000000000000000000000000000000000dEaD;
    
    struct Race {
        uint id;
        string eventName;
        uint status; // 0 initialized, 1 complete, 2 finished
        address[] players;
        uint256[] nftIds;
        uint maxPlayers;
        uint entryFee;
        bool started;
        address winner;
        uint nftWinner;
        uint distance;
        bool isGiveAway;
        address tokenUsed;
        uint qtyWinners;
    }

    struct RaceTimestamp {
        uint id;
        uint createdAt;
        uint completedAt;
    }

    struct UserData {
        address userAddress;
        address tokenAddress;
        uint amount;
    }
    
    uint public raceCounter;
    bool public gameIsPaused = false;
    address claimWallet;
    mapping(uint => Race) public races;
    mapping(uint => RaceTimestamp) public racesTime;
    mapping(address => uint) public balances;
    mapping(address => mapping(address => uint)) public balancePerRacerAndToken;
    mapping(uint => mapping(address => bool)) public racePlayers;
    mapping(uint => uint) public countNftWinnings; // receive an nft id, and return cant of winnings
    mapping(uint => uint) public countNftPoints;
    mapping(address => uint) public countNftWinningsOnAddress; // receive an nft id, and return cant of winnings
    mapping(address => uint) public countNftPointsOnAddress;

    mapping(address => bool) public tokensAvailable;
    address[] public tokenAvailableList;


    mapping (uint => mapping (address => uint)) raceAddressNft;
    mapping(address => bool) public isInBlacklist;


    mapping(uint => uint) public xpForRacers;

    uint256 public COMMISSION_ON_LEAVE = 4;

    address public owner;
    uint public commission;
    uint public accumulatedCommission;
    mapping(address => uint) public accumulatedCommissionPerToken;
    address public tokenNft;

    constructor(address _tokenNft)
    {
        owner = msg.sender;
        claimWallet = msg.sender;
        commission = 4;
        tokenNft = _tokenNft;
        xpForRacers[1] = 100;
        xpForRacers[2] = 50;
        xpForRacers[3] = 25;
        tokensAvailable[DEAD] = true;
        tokenAvailableList.push(DEAD);

    }
    
    function createRace(uint _maxPlayers, uint _entryFee, uint _distance, string memory _eventName, uint _nftId, bool _isGiveAway, address _tokenToPlay, uint _qtyWinners) public payable {
        require(tokensAvailable[_tokenToPlay], "Token not able to pay.");
        require(!isInBlacklist[msg.sender], "User is in blacklist");
        require(!gameIsPaused, "Game is paused");
        require(_maxPlayers > 0, "Max players must be greater than 0");
        if (_tokenToPlay != DEAD && _tokenToPlay != address(0)) {
            if(_isGiveAway) {
                require(_entryFee == 0, "Entry fee has to be 0");
            } else {
                require(_entryFee > 0, "Entry fee has to be greater than 0");
            }
            require(_entryFee <= IERC20(_tokenToPlay).balanceOf(msg.sender), "Balance has to be greater than entry fee.");
            IERC20(_tokenToPlay).transferFrom(msg.sender, address(this), _entryFee);
        } else {
            if(_isGiveAway) {
                require(_entryFee == 0, "Entry fee has to be 0");
            } else {
                require(_entryFee > 0, "Entry fee has to be greater than 0");
            }
            require(msg.value >= _entryFee, "Entry fee has to be greater or equal to entry fee.");
        }
        if (_nftId > 0) {
            require(IERC721(tokenNft).ownerOf(_nftId) == msg.sender, "You don't own this NFT");
        }

        raceCounter++;
        races[raceCounter] = Race(raceCounter, _eventName, 0, new address[](0), new uint256[](0), _maxPlayers, _entryFee, false, address(0), 0, _distance, _isGiveAway, _tokenToPlay, _qtyWinners);
        racesTime[raceCounter] = RaceTimestamp(raceCounter, block.timestamp, block.timestamp);
        emit RaceCreated(raceCounter);
        if (!_isGiveAway) {
            autoJoinRace(raceCounter, _nftId);
        }
    }

    function autoJoinRace(uint _raceId, uint _nftId) internal {
        races[_raceId].players.push(msg.sender);
        races[_raceId].nftIds.push(_nftId);
        racePlayers[_raceId][msg.sender] = true;
        emit JoinRace(_raceId, msg.sender, _nftId);
        if (races[_raceId].players.length == races[_raceId].maxPlayers) {
            startRace(_raceId);
            emit RaceFull(_raceId);
        }
    }
    
    function joinRace(uint _raceId, uint _nftId) public payable {
        require(_raceId > 0 && _raceId <= raceCounter, "Invalid race ID");
        require(!isInBlacklist[msg.sender], "User is in blacklist");
        require(!gameIsPaused, "Game is paused");
        require(!races[_raceId].started, "Race has already started");
        require(races[_raceId].players.length < races[_raceId].maxPlayers, "Race is already full");
        require(!racePlayers[_raceId][msg.sender], "Already joined the race");
        if (races[_raceId].tokenUsed != DEAD) {
            uint balanceOf = IERC20(races[_raceId].tokenUsed).balanceOf(msg.sender);
            require((races[_raceId].isGiveAway && msg.value == 0) || (!races[_raceId].isGiveAway && balanceOf >= races[_raceId].entryFee), "Entry fee is incorrect (t)");
            if(!races[_raceId].isGiveAway) {
                IERC20(races[_raceId].tokenUsed).transferFrom(msg.sender, address(this), races[_raceId].entryFee);
            }
        } else {
            require((races[_raceId].isGiveAway && msg.value == 0) || (!races[_raceId].isGiveAway && msg.value == races[_raceId].entryFee), "Entry fee is incorrect");
        }

        if (_nftId > 0) {
            require(IERC721(tokenNft).ownerOf(_nftId) == msg.sender, "You don't own this NFT");
            raceAddressNft[_raceId][msg.sender] = _nftId;
        }

        races[_raceId].players.push(msg.sender);
        races[_raceId].nftIds.push(_nftId);
        racePlayers[_raceId][msg.sender] = true;
        emit JoinRace(_raceId, msg.sender, _nftId);
        if (races[_raceId].players.length == races[_raceId].maxPlayers) {
            startRace(_raceId);
            emit RaceFull(_raceId);
        }
    }

    function startRace(uint _raceId) internal {
        require(_raceId > 0 && _raceId <= raceCounter, "Invalid race ID");
        require(!races[_raceId].started, "Race has already started");
        require(races[_raceId].players.length == races[_raceId].maxPlayers, "Race is not full");
        races[_raceId].started = true;
        races[_raceId].status = 1;
        emit RaceStarted(_raceId);
    }
    
    function completeRace(uint _raceId) public onlyOwner {
        require(_raceId > 0 && _raceId <= raceCounter, "Invalid race ID");
        require(races[_raceId].started, "Race has not started yet");
        require(races[_raceId].winner == address(0), "Race winner has already been determined");

        uint randomIndex = uint(keccak256(abi.encodePacked(block.timestamp, block.difficulty))) % races[_raceId].players.length;

        // Define winner
        races[_raceId].winner = races[_raceId].players[randomIndex];
        races[_raceId].nftWinner = raceAddressNft[_raceId][races[_raceId].players[randomIndex]];

        uint totalPrize = races[_raceId].entryFee * races[_raceId].players.length;
        uint commisionOfRaceDividend = totalPrize * commission;
        uint commisionOfRace = commisionOfRaceDividend / 100;
        address secondAddress = races[_raceId].players[(randomIndex + 1) % races[_raceId].players.length];
        address thirdAddress = races[_raceId].players[(randomIndex + 2) % races[_raceId].players.length];

        if (races[_raceId].isGiveAway) {
            // We need to set the entry fee as the total amount (not multiply it with the total number of players)
            totalPrize = races[_raceId].entryFee;
            commisionOfRaceDividend = totalPrize * commission;
            commisionOfRace = commisionOfRaceDividend / 100;

            // If is giveaway i have to check if has 1 winner or 3
            totalPrize = races[_raceId].entryFee - commisionOfRace;
            if (races[_raceId].qtyWinners == 1) {
                //Case of one winner, normal pay
                if (races[_raceId].tokenUsed != DEAD) {
                    balancePerRacerAndToken[races[_raceId].winner][races[_raceId].tokenUsed] += totalPrize;
                    accumulatedCommissionPerToken[races[_raceId].tokenUsed] += commisionOfRace;
                } else {
                    balances[races[_raceId].winner] += totalPrize;
                    accumulatedCommission += commisionOfRace;
                }
            } else {
                // case of 3 winners, pay as equal
                if (races[_raceId].tokenUsed != DEAD) {
                    balancePerRacerAndToken[races[_raceId].winner][races[_raceId].tokenUsed] += totalPrize / 3;
                    balancePerRacerAndToken[secondAddress][races[_raceId].tokenUsed] += totalPrize / 3;
                    balancePerRacerAndToken[thirdAddress][races[_raceId].tokenUsed] += totalPrize / 3;
                    accumulatedCommissionPerToken[races[_raceId].tokenUsed] += commisionOfRace;
                } else {
                    balances[races[_raceId].winner] += totalPrize / 3;
                    balances[secondAddress] += totalPrize / 3;
                    balances[thirdAddress] += totalPrize / 3;
                    accumulatedCommission += commisionOfRace;
                }
            }
        } else {
            // Add points and winner race nft
            countNftWinnings[races[_raceId].nftWinner] = countNftWinnings[races[_raceId].nftWinner] + 1;
            countNftPoints[races[_raceId].nftWinner] = countNftPoints[races[_raceId].nftWinner] + xpForRacers[1];

            // Add points and winner race address
            countNftWinningsOnAddress[races[_raceId].winner] = countNftWinningsOnAddress[races[_raceId].winner] + 1;
            countNftPointsOnAddress[races[_raceId].winner] = countNftPointsOnAddress[races[_raceId].winner] + xpForRacers[1];

            //Define second
            uint nftSecondPlace = raceAddressNft[_raceId][secondAddress];
            // Add points to second nft
            countNftPoints[nftSecondPlace] = countNftPoints[nftSecondPlace] + xpForRacers[2];

            // Add points to second address
            countNftPointsOnAddress[secondAddress] = countNftPointsOnAddress[secondAddress] + xpForRacers[2];

            //Define Third
            if (races[_raceId].players.length > 2) {
                uint nftThirdPlace = raceAddressNft[_raceId][thirdAddress];
                // Add points to third one
                countNftPoints[nftThirdPlace] = countNftPoints[nftThirdPlace] + xpForRacers[3];

                // Add points to third one address
                countNftPointsOnAddress[thirdAddress] = countNftPointsOnAddress[thirdAddress] + xpForRacers[3];
            }

            if (races[_raceId].tokenUsed != DEAD) {
                balancePerRacerAndToken[races[_raceId].winner][races[_raceId].tokenUsed] += totalPrize - commisionOfRace;
                accumulatedCommissionPerToken[races[_raceId].tokenUsed] += commisionOfRace;
            } else {
                balances[races[_raceId].winner] += totalPrize - commisionOfRace;
                accumulatedCommission += commisionOfRace;
            }
        }

        races[_raceId].status = 2;
        racesTime[_raceId].completedAt = block.timestamp;
        emit RaceComplete(_raceId, races[_raceId].winner, totalPrize - commisionOfRace);

    }

    // Helpers
    function getRace(uint _raceId) public view returns (Race memory) {
        return races[_raceId];
    }

    function setIsInBlacklist(address _addy, bool _blacklisted) public onlyOwner {
        isInBlacklist[_addy] = _blacklisted;
    }

    function setClaimWallet(address _claimWallet) public onlyOwner {
        claimWallet = _claimWallet;
    }

    function setOptions(uint _commission, uint _commissionOnLeave, bool _gameIsPaused) public onlyOwner {
        commission = _commission;
        COMMISSION_ON_LEAVE = _commissionOnLeave;
        gameIsPaused = _gameIsPaused;
    }

    function claim(address _tokenAddress) public  {
        if (_tokenAddress == DEAD) {
            require(balances[msg.sender] > 0, "User doesnt have funds");
            payable(msg.sender).transfer(balances[msg.sender]);
            balances[msg.sender] = 0;
        } else {
            require(balancePerRacerAndToken[msg.sender][_tokenAddress] > 0, "User doesnt have funds (tokens)");
            IERC20(_tokenAddress).approve(address(this), IERC20(_tokenAddress).balanceOf(address(this)));
            IERC20(_tokenAddress).transfer(msg.sender, balancePerRacerAndToken[msg.sender][_tokenAddress]);
            balancePerRacerAndToken[msg.sender][_tokenAddress] = 0;
        }
    }

    function claimCommission() public onlyOwner {
        require(accumulatedCommission > 0, "User doesnt have funds");
        payable(claimWallet).transfer(accumulatedCommission);
        accumulatedCommission = 0;
    }

    function claimCommissionToken(address _tokenAddress) public onlyOwner {
        IERC20(_tokenAddress).approve(address(this), IERC20(_tokenAddress).balanceOf(address(this)));
        IERC20(_tokenAddress).transfer(claimWallet, accumulatedCommissionPerToken[_tokenAddress]);
        accumulatedCommissionPerToken[_tokenAddress] = 0;
    }

    function getRacesIdByStatus(uint _status) public view returns (uint[] memory) {
        require(_status >= 0 && _status <= 3, "Invalid race status");

        // First, we need to determine how many races have the requested status
        uint count = 0;
        for (uint i = 1; i <= raceCounter; i++) {
            if (races[i].status == _status) {
                count++;
            }
        }

        // Now we know the size of racesByStatus, so we can initialize it
        uint[] memory racesByStatus = new uint[](count);
        uint index = 0;
        for (uint i = 1; i <= raceCounter; i++) {
            if (races[i].status == _status) {
                racesByStatus[index] = races[i].id;
                index++;
            }
        }

        return racesByStatus;
    }

    function getRacesIdByStatusAndGiveAway(uint _status, bool _isGiveAway) public view returns (uint[] memory) {
        require(_status >= 0 && _status <= 3, "Invalid race status");

        // First, we need to determine how many races have the requested status
        uint count = 0;
        for (uint i = 1; i <= raceCounter; i++) {
            if (races[i].status == _status && races[i].isGiveAway == _isGiveAway) {
                count++;
            }
        }

        // Now we know the size of racesByStatus, so we can initialize it
        uint[] memory racesByStatus = new uint[](count);
        uint index = 0;
        for (uint i = 1; i <= raceCounter; i++) {
            if (races[i].status == _status && races[i].isGiveAway == _isGiveAway) {
                racesByStatus[index] = races[i].id;
                index++;
            }
        }

        return racesByStatus;
    }

    function getNFTForRaceAndUser(uint256 _raceId, address _userId) public view returns (uint) {
        return raceAddressNft[_raceId][_userId];
    }

    function setXpForRacers(uint index, uint value) public onlyOwner {
        xpForRacers[index] = value;
    }

    function setTokenNft(address _tokenNft) public onlyOwner {
        tokenNft = _tokenNft;
    }

    function setNftPointsAddresses(address[] memory nftIds, uint[] memory nftPoints) public onlyOwner {
        require(nftIds.length == nftPoints.length, "Sizes of ids and points has to be similar");
        
        for(uint i = 0; i < nftIds.length; i++) {
            countNftPointsOnAddress[nftIds[i]] = nftPoints[i];
        }
    }

    function setNftWinningsAddresses(address[] memory nftIds, uint[] memory nftWinnings) public onlyOwner {
        require(nftIds.length == nftWinnings.length, "Sizes of ids and points has to be similar");
        
        for(uint i = 0; i < nftIds.length; i++) {
            countNftWinningsOnAddress[nftIds[i]] = nftWinnings[i];
        }
    }

    function setNftPoints(uint[] memory nftIds, uint[] memory nftPoints) public onlyOwner {
        require(nftIds.length == nftPoints.length, "Sizes of ids and points has to be similar");
        
        for(uint i = 0; i < nftIds.length; i++) {
            countNftPoints[nftIds[i]] = nftPoints[i];
        }
    }

    function setNftWinnings(uint[] memory nftIds, uint[] memory nftWinnings) public onlyOwner {
        require(nftIds.length == nftWinnings.length, "Sizes of ids and points has to be similar");
        
        for(uint i = 0; i < nftIds.length; i++) {
            countNftWinnings[nftIds[i]] = nftWinnings[i];
        }
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can perform this action");
        _;
    }

    function leaveRace(uint _raceId) public {
        require(_raceId > 0 && _raceId <= raceCounter, "Invalid race ID");
        require(!races[_raceId].started, "Race has already started");
        require(racePlayers[_raceId][msg.sender], "You are not part of this race");

        // Remove player from the race
        for (uint i = 0; i < races[_raceId].players.length; i++) {
            if (races[_raceId].players[i] == msg.sender) {
                races[_raceId].players[i] = races[_raceId].players[races[_raceId].players.length - 1];
                races[_raceId].players.pop();

                races[_raceId].nftIds[i] = races[_raceId].nftIds[races[_raceId].nftIds.length - 1];
                races[_raceId].nftIds.pop();
                break;
            }
        }
        
        // Update the race players mapping
        racePlayers[_raceId][msg.sender] = false;

        // If not giveaway, return fee
        if (!races[_raceId].isGiveAway) {
            uint commissionOnLeave = (races[_raceId].entryFee * COMMISSION_ON_LEAVE) / 100;

            uint totalToReturn = races[_raceId].entryFee - commissionOnLeave;
            accumulatedCommission += commissionOnLeave;

            if (races[_raceId].tokenUsed != DEAD) {
                IERC20(races[_raceId].tokenUsed).approve(address(this), IERC20(races[_raceId].tokenUsed).balanceOf(address(this)));
                IERC20(races[_raceId].tokenUsed).transfer(msg.sender, totalToReturn);
            } else {
                payable(msg.sender).transfer(totalToReturn);
            }

        }

    }

    function addTokenPay(address tokenPay) external onlyOwner {
        require(!tokensAvailable[tokenPay], "Token pay already added.");
        tokensAvailable[tokenPay] = true;
        tokenAvailableList.push(tokenPay);
    }

    function removeTokenPay(address tokenPay) external onlyOwner {
        require(tokensAvailable[tokenPay], "Token Pay not found.");
        tokensAvailable[tokenPay] = false;
        for (uint i = 0; i < tokenAvailableList.length; i++) {
            if (tokenAvailableList[i] == tokenPay) {
                tokenAvailableList[i] = tokenAvailableList[tokenAvailableList.length - 1];
                tokenAvailableList.pop();
                break;
            }
        }
    }

    function ableToClaim(address _userAddy) public view returns (UserData[] memory) {
        UserData[] memory returnData = new UserData[](tokenAvailableList.length + 1);
        returnData[0] = UserData({userAddress: _userAddy, tokenAddress: address(0x0), amount: balances[_userAddy]});
        
        for (uint i = 0; i < tokenAvailableList.length; i++) {
            returnData[i + 1] = UserData({userAddress: _userAddy, tokenAddress: tokenAvailableList[i], amount: balancePerRacerAndToken[_userAddy][tokenAvailableList[i]]});
        }
        
        return returnData;
    }

    function cancelRace(uint _raceId) public onlyOwner {
        require(_raceId > 0 && _raceId <= raceCounter, "Invalid race ID");
        require(!races[_raceId].started, "Race has already started");

        IERC20(races[_raceId].tokenUsed).approve(address(this), IERC20(races[_raceId].tokenUsed).balanceOf(address(this)));
        // Refund all players if not giveAway
        if (!races[_raceId].isGiveAway) {
            for (uint i = 0; i < races[_raceId].players.length; i++) {
                if (races[_raceId].tokenUsed != DEAD) {
                    IERC20(races[_raceId].tokenUsed).transfer(races[_raceId].players[i], races[_raceId].entryFee);
                } else if (!isContract(races[_raceId].players[i])) {
                    payable(races[_raceId].players[i]).transfer(races[_raceId].entryFee);
                } else {
                    payable(owner).transfer(races[_raceId].entryFee);
                }
            }
        }

        races[_raceId].players = new address[](0);
        races[_raceId].status = 3;
    }

    function getActiveRaces() public view returns (uint[] memory) {
        uint count = 0;
        for (uint i = 1; i <= raceCounter; i++) {
            if (races[i].status == 0) {
                count++;
            }
        }

        uint[] memory activeRaces = new uint[](count);

        uint j = 0;
        for (uint i = 1; i <= raceCounter; i++) {
            if (races[i].status == 0) {
                activeRaces[j] = i;
                j++;
            }
        }

        return activeRaces;
    }

    function getPlayersByRaceId(uint256 raceId) public view returns (address[] memory) {
        return races[raceId].players;
    }

    function getNftsByRaceId(uint256 raceId) public view returns (uint256[] memory) {
        return races[raceId].nftIds;
    }

    function unstuck(uint256 _amount, address _addy) onlyOwner public {
        if (_addy == address(0)) {
            (bool sent,) = address(msg.sender).call{value: _amount}("");
            require(sent, "funds has to be sent");
        } else {
            bool approve_done = IERC20(_addy).approve(address(this), IERC20(_addy).balanceOf(address(this)));
            require(approve_done, "CA cannot approve tokens");
            require(IERC20(_addy).balanceOf(address(this)) > 0, "No tokens");
            IERC20(_addy).transfer(msg.sender, _amount);
        }
    }

    function isContract(address addr) internal view returns (bool) {
        return addr.code.length > 0;
    }

    receive() external payable {}
    fallback() external payable {}
}