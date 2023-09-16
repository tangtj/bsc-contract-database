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
    struct Bet {
        address user;
        uint256 amount;
        uint256 initialPrice;
        Prediction prediction;
    }
    
    mapping(address => Bet) public bets;
    uint256 public bettingDuration = 2 minutes;
    uint256 public betEndTime;

    constructor(address _priceFeed) {
        priceFeed = AggregatorV3Interface(_priceFeed);
    }

    function placeBet(Prediction _prediction) external payable {
        require(bets[msg.sender].amount == 0, "Already placed a bet");
        require(msg.value > 0, "Bet amount should be greater than 0");
        int256 currentPrice = getCurrentPrice();
        bets[msg.sender] = Bet({
            user: msg.sender,
            amount: msg.value,
            initialPrice: uint256(currentPrice),
            prediction: _prediction
        });
        betEndTime = block.timestamp + bettingDuration;
    }

    function claim() external {
        require(bets[msg.sender].amount > 0, "No active bet found");
        require(block.timestamp > betEndTime, "Betting time not yet over");
        
        int256 currentPrice = getCurrentPrice();
        bool won = false;
        if (bets[msg.sender].prediction == Prediction.UP && uint256(currentPrice) > bets[msg.sender].initialPrice) {
            won = true;
        } else if (bets[msg.sender].prediction == Prediction.DOWN && uint256(currentPrice) < bets[msg.sender].initialPrice) {
            won = true;
        }

        if (won) {
            uint256 winnings = bets[msg.sender].amount * 2;
            payable(msg.sender).transfer(winnings);
        }
        
        delete bets[msg.sender];
    }

    function getCurrentPrice() public view returns (int256) {
        (, int256 price, , , ) = priceFeed.latestRoundData();
        return price;
    }
}