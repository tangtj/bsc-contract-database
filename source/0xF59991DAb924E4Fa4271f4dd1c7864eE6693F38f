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
        uint256 betTime; // 新增此行
        Prediction prediction;
        Result result;
    }
    
    event BetResult(address indexed user, bool won);
    event BettingDurationUpdated(uint256 newDuration);

    mapping(address => Bet[]) public userBets;
    uint256 public ROUND_DURATION = 5 minutes; // 更改為5分鐘
    uint256 public startTime;

    address public owner;

    constructor(address _priceFeed) {
        priceFeed = AggregatorV3Interface(_priceFeed);
        owner = msg.sender;
        startTime = block.timestamp;
    }

    receive() external payable {} // 添加回退函數

    function getCurrentRound() public view returns (uint256) {
        return (block.timestamp - startTime) / ROUND_DURATION;
    }

    function getNextRoundStartTime() public view returns (uint256) {
        uint256 currentRound = getCurrentRound();
        return startTime + (currentRound + 1) * ROUND_DURATION;
    }

    function placeBet(Prediction _prediction) external payable {
        require(msg.value > 0, "Bet amount should be greater than 0");
        int256 currentPrice = getCurrentPrice();
        userBets[msg.sender].push(Bet({
            user: msg.sender,
            amount: msg.value,
            initialPrice: uint256(currentPrice),
            betTime: block.timestamp, // 設置此值
            prediction: _prediction,
            result: Result.PENDING
        }));
    }

    function claim() external {
        uint256 totalReward = 0;

        for (uint i = 0; i < userBets[msg.sender].length; i++) {
            Bet storage bet = userBets[msg.sender][i];

            if (bet.result == Result.PENDING) {
                uint256 betEndTime = bet.betTime + ROUND_DURATION;
                if (block.timestamp >= betEndTime) {
                    int256 currentPrice = getCurrentPrice();

                    if (bet.prediction == Prediction.UP && uint256(currentPrice) > bet.initialPrice) {
                        bet.result = Result.WON;
                        totalReward += bet.amount * 2;
                    } else if (bet.prediction == Prediction.DOWN && uint256(currentPrice) < bet.initialPrice) {
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

    function checkResult(address _user, uint256 round) external view returns (string memory) {
        require(userBets[_user].length > round, "No bet history found for the given round");
        Bet memory specificBet = userBets[_user][round];
        
        uint256 betRoundEndTime = specificBet.betTime + ROUND_DURATION;
        if (block.timestamp < betRoundEndTime) {
            return "Waiting for the betting time to end for the given round";
        }

        int256 currentPrice = getCurrentPrice();
        bool won = false;
        if (specificBet.prediction == Prediction.UP && uint256(currentPrice) > specificBet.initialPrice) {
            won = true;
        } else if (specificBet.prediction == Prediction.DOWN && uint256(currentPrice) < specificBet.initialPrice) {
            won = true;
        }

        return won ? "You won!" : "You lost!";
    }

    function withdrawFunds(uint256 amount) external {
        require(msg.sender == owner, "Only the contract owner can withdraw");
        require(amount <= address(this).balance, "Insufficient funds in the contract");
        payable(owner).transfer(amount);
    }

    function getCurrentPrice() public view returns (int256) {
        (, int256 price, , , ) = priceFeed.latestRoundData();
        return price;
    }

    function getUserBetHistory(address _user) external view returns (Bet[] memory) {
        return userBets[_user];
    }

    function updateBettingDuration(uint256 newDuration) external {
        require(msg.sender == owner, "Only the contract owner can update the betting duration");
        ROUND_DURATION = newDuration;
        emit BettingDurationUpdated(newDuration);
    }
}