//SPDX-License-Identifier: MIT Licensed
pragma solidity ^0.8.0;

interface IERC20 {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 value) external;

    function transfer(address to, uint256 value) external;

    function transferFrom(address from, address to, uint256 value) external;

    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Transfer(address indexed from, address indexed to, uint256 value);
}

interface AggregatorV3Interface {
    function decimals() external view returns (uint8);

    function description() external view returns (string memory);

    function version() external view returns (uint256);

    function getRoundData(
        uint80 _roundId
    )
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

contract presale {
    IERC20 public Token;
    IERC20 public USDT = IERC20(0x55d398326f99059fF775485246999027B3197955);
    IERC20 public BUSD = IERC20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56);
    AggregatorV3Interface public priceFeeD;

    address payable public owner;

    uint256 public tokenPerUsd = 10000 ether; 
    uint256 public soldToken;
    uint256 public totalSupply = 10_000_000_000 ether;
    uint256 public tokenForSell = 355_555_555 ether;
    uint256 public nextPrice= 8000 ether;
    uint256 public StageCount = 1;
    uint256 public referralBonus = 10;
    uint256 public amountRaised;
    uint256 public amountRaisedUSDT;
    uint256 public amountRaisedBUSD;
    address payable public fundReceiver;

    uint256 public constant divider = 100;

    bool public presaleStatus;
    bool public enableClaim;

    struct user {
        uint256 native_balance;
        uint256 usdt_balance;
        uint256 busd_balance;
        uint256 token_balance;
        uint256 claimed_token;
        uint256 bonus_claimed;
    }

    mapping(address => user) public users; 

    modifier onlyOwner() {
        require(msg.sender == owner, "PRESALE: Not an owner");
        _;
    }

    event BuyToken(address indexed _user, uint256 indexed _amount);
    event ClaimToken(address indexed _user, uint256 indexed _amount);
    event UpdatePrice(uint256 _oldPrice, uint256 _newPrice); 
    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        fundReceiver = payable(0x8cB348416fa6c8BFEDe3C5E52DADfD4EeEC93420);
        Token = IERC20(0x72F38C33c47d8d9d0BbdABea2Db946Fd503D9881);
        owner = payable(0x8cB348416fa6c8BFEDe3C5E52DADfD4EeEC93420);
        priceFeeD = AggregatorV3Interface(
            0x0567F2323251f0Aab15c8dFb1967E4e8A7D42aeE
        );
        presaleStatus = true;
    }

    receive() external payable {}

    // to get real time price of Eth
    function getLatestPrice() public view returns (uint256) {
        (, int256 price, , , ) = priceFeeD.latestRoundData();
        return uint256(price);
    }

    // to buy token during preSale time with Eth => for web3 use

    function buyToken(address payable _ref) public payable {
        require(presaleStatus == true, " Presale is Paused, check back later");
  
        uint256 numberOfTokens;
        numberOfTokens = NativeToToken(msg.value);
        uint256 bonus = ((msg.value)* referralBonus)/100;
        _ref.transfer(bonus);
        soldToken = soldToken + (numberOfTokens);
        require(
            soldToken <= tokenForSell,
            "Low  Token in pool, Try less amount or wait for next stage"
        );
        amountRaised = amountRaised + (msg.value);

        users[msg.sender].native_balance =
            users[msg.sender].native_balance +
            (msg.value);
        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens);
        users[msg.sender].bonus_claimed += bonus;     
    }

    // to buy token during preSale time with USDT => for web3 use
    function buyTokenUSDT(address _ref, uint256 amount) public {
        require(presaleStatus == true, "Presale is Paused, check back later");
        
        uint256 bonus = ((amount)* referralBonus)/100;
        USDT.transferFrom(msg.sender,_ref,bonus);
        USDT.transferFrom(msg.sender, fundReceiver, amount-bonus);
        uint256 numberOfTokens;
        numberOfTokens = usdtToToken(amount);

        soldToken = soldToken + (numberOfTokens);
        require(
            soldToken <= tokenForSell,
            "Low Tokens in pool, Try less amount or wait for next stage"
        );
        amountRaisedUSDT = amountRaisedUSDT + (amount);

        users[msg.sender].usdt_balance += amount;

        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens);
        users[msg.sender].bonus_claimed += bonus;    
    }
    // to buy token during preSale time with BUSD => for web3 use
    function buyTokenBUSD(address _ref,uint256 amount) public {
        require(presaleStatus == true, "Presale is Paused, check back later");
        uint256 bonus = ((amount)* referralBonus)/100;
        BUSD.transferFrom(msg.sender,_ref,bonus);
        BUSD.transferFrom(msg.sender, fundReceiver, amount-bonus);

        uint256 numberOfTokens;
        numberOfTokens = usdtToToken(amount);

        soldToken = soldToken + (numberOfTokens);
        require(
            soldToken <= tokenForSell,
            "Low Tokens in pool, Try less amount or wait for next stage"
        );
        amountRaisedBUSD = amountRaisedBUSD + (amount);

        users[msg.sender].busd_balance += amount;

        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens);
        users[msg.sender].bonus_claimed += bonus;    
    }

    // Claim bought tokens
    function claimTokens() external {
        require(enableClaim == true, "Presale : Claim not active yet");
        require(users[msg.sender].token_balance != 0, "Presale: 0 to claim");

        user storage _usr = users[msg.sender];

        Token.transferFrom(owner, msg.sender, _usr.token_balance);
        _usr.claimed_token += _usr.token_balance;
        _usr.token_balance -= _usr.token_balance;

        emit ClaimToken(msg.sender, _usr.token_balance);
    }

    function EnableClaim(bool _state) external onlyOwner {
        enableClaim = _state;
    }

    function PresaleStatus(bool _off) external onlyOwner {
        presaleStatus = _off;
    }

    // to check number of token for given Eth
    function NativeToToken(uint256 _amount) public view returns (uint256) {
        uint256 EthToUsd = (_amount * (getLatestPrice())) / (1 ether);
        uint256 numberOfTokens = (EthToUsd * (tokenPerUsd)) / (1e8);
        return numberOfTokens;
    }

    // to check number of token for given usdt
    function usdtToToken(uint256 _amount) public view returns (uint256) {
        uint256 numberOfTokens = (_amount * (tokenPerUsd)) / (1e18);
        return numberOfTokens;
    }

    // to change Price of the token
    function changePrice(
        uint256 _price,
        uint256 _nextPrice,
        uint256 _tokenForSell,
        uint256 _StageCount
        
    ) external onlyOwner {
        uint256 oldPrice = tokenPerUsd;
        tokenPerUsd = _price;
        nextPrice = _nextPrice;

        tokenForSell = soldToken + _tokenForSell;
        StageCount = _StageCount; 

        emit UpdatePrice(oldPrice, _price);
    }

    function changeBonus(uint256 _referralBonus) external onlyOwner { 
        referralBonus = _referralBonus;
 
    }
    function ChangeSupply(
        uint256 _supply,
        uint256 _sold,
        uint256 _raised,
        uint256 _raisedInUsdt,
        uint256 _raisedBusd
    ) external onlyOwner {
        totalSupply = _supply;
        soldToken = _sold;
        amountRaised = _raised;
        amountRaisedUSDT = _raisedInUsdt; 
        amountRaisedBUSD = _raisedBusd;
    }

    // transfer ownership
    function changeOwner(address payable _newOwner) external onlyOwner {
        require(
            _newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        address _oldOwner = owner;
        owner = _newOwner;

        emit OwnershipTransferred(_oldOwner, _newOwner);
    }

    // change tokens
    function changeToken(address _token) external onlyOwner {
        Token = IERC20(_token);
    }

    //change USDT
    function changeUSDT(address _USDT, address _BUSD) external onlyOwner {
        USDT = IERC20(_USDT);
        BUSD = IERC20(_BUSD);
    }

    // to draw funds for liquidity
    function initiateTransfer(uint256 _value) external onlyOwner {
        fundReceiver.transfer(_value);
    }

    // to draw funds for liquidity
    function changeFundReciever(address _addr) external onlyOwner {
        fundReceiver = payable(_addr);
    }

    // to draw out tokens
    function transferTokens(IERC20 token, uint256 _value) external onlyOwner {
        token.transfer(msg.sender, _value);
    }
 
}