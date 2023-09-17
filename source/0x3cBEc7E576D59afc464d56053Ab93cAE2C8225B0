// File: @chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol


pragma solidity ^0.8.0;

interface AggregatorV3Interface {
  function decimals() external view returns (uint8);

  function description() external view returns (string memory);

  function version() external view returns (uint256);

  function getRoundData(uint80 _roundId)
    external
    view
    returns (
      uint80 roundId,
      int256 answer,
      uint256 startedAt,
      uint256 updatedAt,
      uint80 answeredInRound
    );

  function latestRoundData()
    external
    view
    returns (
      uint80 roundId,
      int256 answer,
      uint256 startedAt,
      uint256 updatedAt,
      uint80 answeredInRound
    );
}

// File: contracts/new.sol


pragma solidity ^0.8.0;


contract PricePrediction {
    AggregatorV3Interface internal priceFeed;
    enum Prediction { UP, DOWN }
    enum Result { PENDING, WON, LOST }

    struct Bet {
        address user;
        uint256 amount;
        uint256 initialPrice;
        uint256 betTime;
        Prediction prediction;
        Result result;
        uint256 userRound;
        uint256 gameRound;
        uint256 endingPrice; // Added this to store the ending price
    }

    event BetResult(address indexed user, bool won);
    event BettingDurationUpdated(uint256 newDuration);

    mapping(address => Bet[]) public userBets;
    uint256 public constant BETTING_DURATION = 15 seconds;
    uint256 public ROUND_DURATION = 15 seconds;
    uint256 public startTime;
    mapping(address => uint256) public userRound;
    uint256 public gameRound;

    address public owner;

    constructor(address _priceFeed) {
        priceFeed = AggregatorV3Interface(_priceFeed);
        owner = msg.sender;
        startTime = block.timestamp;
        gameRound = getCurrentRound() + 1;
    }

    receive() external payable {} 

    function getCurrentRound() public view returns (uint256) {
        return (block.timestamp - startTime) / ROUND_DURATION;
    }

    function getGameRound() public view returns (uint256) {
        return gameRound;
    }

    function getNextRoundStartTime() public view returns (uint256) {
        uint256 currentRound = getCurrentRound();
        return startTime + (currentRound + 1) * ROUND_DURATION;
    }

    function placeBet(Prediction _prediction) external payable {
        require(msg.value > 0, "Bet amount should be greater than 0");

        int256 currentPrice = getCurrentPrice();
        gameRound = getCurrentRound() + 1;

        userRound[msg.sender]++;
        userBets[msg.sender].push(Bet({
            user: msg.sender,
            amount: msg.value,
            initialPrice: uint256(currentPrice),
            betTime: block.timestamp,
            prediction: _prediction,
            result: Result.PENDING,
            userRound: userRound[msg.sender],
            gameRound: gameRound,
            endingPrice: 0  // Initialize with 0
        }));
    }

    function claim() external {
        uint256 totalReward = 0;

        for (uint i = 0; i < userBets[msg.sender].length; i++) {
            Bet storage bet = userBets[msg.sender][i];

            if (bet.result == Result.PENDING) {
                uint256 betEndTime = bet.betTime + ROUND_DURATION;
                if (block.timestamp >= betEndTime) {
                    int256 endingPrice = getEndingPrice(bet.gameRound);

                    // If the ending price is the same as the initial price, then the user lost.
                    if (endingPrice == int256(bet.initialPrice)) {
                        bet.result = Result.LOST;
                        continue;
                    }

                    if ((bet.prediction == Prediction.UP && endingPrice > int256(bet.initialPrice)) ||
                        (bet.prediction == Prediction.DOWN && endingPrice < int256(bet.initialPrice))) {
                        bet.result = Result.WON;
                        totalReward += bet.amount * 2;
                    } else {
                        bet.result = Result.LOST;
                    }
                }
            }
        }

        if (totalReward > 0) {
            payable(msg.sender).transfer(totalReward);
        }
    }

    function checkResult(address _user, uint256 targetGameRound) external view returns (string memory) {
        require(targetGameRound <= gameRound, "This game round is in the future!");

        bool found = false;
        Bet memory specificBet;
        
        for (uint i = 0; i < userBets[_user].length; i++) {
            if (userBets[_user][i].gameRound == targetGameRound) {
                specificBet = userBets[_user][i];
                found = true;
                break;
            }
        }

        require(found, "No bet found for the specified game round.");

        uint256 betRoundEndTime = specificBet.betTime + ROUND_DURATION;
        if (block.timestamp < betRoundEndTime) {
            return "Waiting for the betting time to end for the given round";
        }

        int256 endingPrice = getEndingPrice(targetGameRound);

        if (endingPrice == int256(specificBet.initialPrice)) {
            return "You lost!";
        }

        bool won = (specificBet.prediction == Prediction.UP && endingPrice > int256(specificBet.initialPrice)) ||
                (specificBet.prediction == Prediction.DOWN && endingPrice < int256(specificBet.initialPrice));

        return won ? "You won!" : "You lost!";
    }

    function getEndingPrice(uint256 targetGameRound) internal view returns (int256) {
        uint256 roundEndTime = startTime + targetGameRound * ROUND_DURATION;

        uint80 latestRoundId;
        (latestRoundId, , , , ) = priceFeed.latestRoundData();
        uint80 startedRoundId = latestRoundId;

        while (startedRoundId > 0) {
            (, int256 price, , uint256 updatedAt, ) = priceFeed.getRoundData(startedRoundId);
            if (updatedAt <= roundEndTime) {
                return price;
            }
            startedRoundId--;
        }
        return getCurrentPrice();  // Return the current price if no price data is found for the target round.
    }

    function withdrawFunds(uint256 amount) external {
        require(msg.sender == owner, "Only the contract owner can withdraw");
        require(amount <= address(this).balance, "Insufficient funds in the contract");
        payable(owner).transfer(amount);
    }

    function getCurrentPrice() internal view returns (int256) {
        (, int256 price, , , ) = priceFeed.latestRoundData();
        return price;
    }

    function getUserBetHistory(address _user) external view returns (Bet[] memory) {
        return userBets[_user];
    }
}