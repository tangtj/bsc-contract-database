//SPDX-License-Identifier: MIT Licensed
pragma solidity 0.8.18;

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

    AggregatorV3Interface public priceFeeD;

    address payable public owner;

    uint256 public tokenPerUsd = 454545 * 1e14;
    uint256 public totalUsers;
    uint256 public soldToken;
    uint256 public totalSupply = 1_000_000_000 ether;
    uint256 public tokenForSell = 1_000_000_000  ether;
    uint256 public nextPrice;
    uint256 public StageCount = 1;
    uint256 public amountRaised;
    uint256 public amountRaisedUSDT; 
    uint256 public bonus = 5;
    address payable public fundReceiver;

    uint256 public constant divider = 100;

    bool public presaleStatus;
    bool public enableClaim;

    struct user {
        uint256 native_balance;
        uint256 usdt_balance; 
        uint256 token_balance;
        uint256 claimed_token;
        uint256 reward;
    }

    mapping(address => user) public users;
    mapping(address => uint256) public wallets;

    modifier onlyOwner() {
        require(msg.sender == owner, "PRESALE: Not an owner");
        _;
    }

    event BuyToken(address indexed _user, uint256 indexed _amount);
    event ClaimToken(address indexed _user, uint256 indexed _amount);
    event UpdatePrice(uint256 _oldPrice, uint256 _newPrice);
    event UpdateMinPurchase(
        uint256 _oldMinNative,
        uint256 _newMinNative,
        uint256 _oldMinUsdt,
        uint256 _newMinUsdt
    );
    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        fundReceiver = payable(0x3badb3A77859e35E2f6E82D38D9ec64dCA593fC7);
        Token = IERC20(0x53f1e15ed3Cea8c1d4Adc4BE2DDE4BA33715a922);
        owner = payable(0x3badb3A77859e35E2f6E82D38D9ec64dCA593fC7);
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

    function buyToken(address _ref) public payable {
        require(presaleStatus == true, " Presale is Paused, check back later");

        uint256 numberOfTokens;
        numberOfTokens = NativeToToken(msg.value);
        soldToken = soldToken + (numberOfTokens);
        require(
            soldToken <= tokenForSell,
            "Low  Token in pool, Try less amount or wait for next stage"
        );
        amountRaised = amountRaised + (msg.value);
        
        uint256 refBonus = (numberOfTokens*bonus)/100;

        users[msg.sender].native_balance =
            users[msg.sender].native_balance +
            (msg.value);

        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens + refBonus);

        users[_ref].token_balance =
            users[_ref].token_balance +
            (refBonus);
            
        users[_ref].reward +=refBonus;
        users[msg.sender].reward +=refBonus;
             
            
    }

    // to buy token during preSale time with USDT => for web3 use
    function buyTokenUSDT(address _ref,uint256 amount) public {
        require(presaleStatus == true, "Presale is Paused, check back later");
        USDT.transferFrom(msg.sender, fundReceiver, amount);

        uint256 numberOfTokens;
        numberOfTokens = usdtToToken(amount);

        soldToken = soldToken + (numberOfTokens);
        require(
            soldToken <= tokenForSell,
            "Low Tokens in pool, Try less amount or wait for next stage"
        );
        amountRaisedUSDT = amountRaisedUSDT + (amount);
        uint256 refBonus = (numberOfTokens*bonus)/100;
        users[msg.sender].usdt_balance += amount;

        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens + refBonus);

        users[_ref].token_balance =
            users[_ref].token_balance +
            (refBonus);
        users[_ref].reward +=refBonus;
        users[msg.sender].reward +=refBonus;
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

    function changeBonus(uint256 _ref) external onlyOwner {
        bonus = _ref;
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

    function ChangeSupply(
        uint256 _supply,
        uint256 _sold,
        uint256 _raised,
        uint256 _raisedInUsdt
    ) external onlyOwner {
        totalSupply = _supply;
        soldToken = _sold;
        amountRaised = _raised;
        amountRaisedUSDT = _raisedInUsdt;
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
    function changeUSDT(address _USDT) external onlyOwner {
        USDT = IERC20(_USDT);
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

    function ETHbuyers(
        address[] memory wallet,
        uint256[] memory amount
    ) public onlyOwner {
        require(wallet.length == amount.length, "Invalid data length");
        for (uint256 i = 0; i < wallet.length; i++) {
            wallets[wallet[i]] += amount[i];
        }
    }

    function ClaimForEth() public {
        require(enableClaim == true, "Presale : Claim not active yet");
        require(wallets[msg.sender] > 0, "already claimed");
        Token.transferFrom(owner, msg.sender, wallets[msg.sender]);
        wallets[msg.sender] = 0;
    }
}