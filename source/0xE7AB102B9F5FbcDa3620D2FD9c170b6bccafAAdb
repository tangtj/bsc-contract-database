//SPDX-License-Identifier: MIT Licensed
pragma solidity ^0.8.10;

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
    IERC20 public Zuaito;

    AggregatorV3Interface public priceFeeD;

    address payable public owner;

    uint256 public tokenPerUsd = 11111111111 * 1e10;
    uint256 public bonusToken = 0;
    uint256 public totalUsers;
    uint256 public soldToken;
    uint256 public SupplyPerPhase = 300000000 ether;

    uint256 public amountRaised;
    address payable public fundReceiver =
        payable(0x297fcf8C5dc96A75d77944a457D9Dd31f6067457);

    uint256 public constant divider = 100;
    address[] public UsersAddresses;

    bool public presaleStatus;
    mapping(address => bool) public oldBuyer;
    struct user {
        uint256 native_balance;
        uint256 token_balance;
        uint256 claimed_token;
    }

    mapping(address => user) public users;

    modifier onlyOwner() {
        require(msg.sender == owner, "PRESALE: Not an owner");
        _;
    }

    event BuyToken(address indexed _user, uint256 indexed _amount);

    constructor(address _token) {
        Zuaito = IERC20(_token);
        owner = payable(msg.sender);
        priceFeeD = AggregatorV3Interface(
            0x0567F2323251f0Aab15c8dFb1967E4e8A7D42aeE
        );
        presaleStatus = true;
    }

    receive() external payable {}

    // to get real time price of Bnb
    function getLatestPrice() public view returns (uint256) {
        (, int256 price, , , ) = priceFeeD.latestRoundData();
        return uint256(price);
    }

    // to buy token during preSale time with Bnb => for web3 use

    function buyToken() public payable {
        require(presaleStatus == true, "Presale : Presale is finished");
        require(soldToken <= SupplyPerPhase, "All Sold");
        if (oldBuyer[msg.sender] != true) {
            totalUsers += 1;
        }
        uint256 numberOfTokens;
        numberOfTokens = NativeToToken(msg.value);
        uint256 bonus = (bonusToken * numberOfTokens) / divider;
        soldToken = soldToken + (numberOfTokens);
        amountRaised = amountRaised + (msg.value);
        fundReceiver.transfer(msg.value);

        users[msg.sender].native_balance =
            users[msg.sender].native_balance +
            (msg.value);
        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens + bonus);
        oldBuyer[msg.sender] = true;

        UsersAddresses.push(msg.sender);
    }

    // to change preSale amount limits
    function setSupplyPerPhase(
        uint256 _SupplyPerPhase,
        uint256 _soldToken
    ) external onlyOwner {
        SupplyPerPhase = _SupplyPerPhase;
        soldToken = _soldToken;
    }

    function stopPresale(bool _off) external onlyOwner {
        presaleStatus = _off;
    }

    // to check number of token for given Bnb
    function NativeToToken(uint256 _amount) public view returns (uint256) {
        uint256 BnbToUsd = (_amount * (getLatestPrice())) / (1 ether);
        uint256 numberOfTokens = (BnbToUsd * (tokenPerUsd)) / (1e8);
        return numberOfTokens;
    }

    // to change Price of the token
    function changePrice(uint256 _price) external onlyOwner {
        tokenPerUsd = _price;
    }

    // to change bonus %
    function changeBonus(uint256 _bonus) external onlyOwner {
        bonusToken = _bonus;
    }

    // transfer ownership
    function changeOwner(address payable _newOwner) external onlyOwner {
        owner = _newOwner;
    }

    // change tokens
    function changeToken(address _token) external onlyOwner {
        Zuaito = IERC20(_token);
    }

    // to draw funds for liquidity
    function transferFunds(uint256 _value) external onlyOwner {
        owner.transfer(_value);
    }

    // to draw out tokens
    function transferTokens(IERC20 token, uint256 _value) external onlyOwner {
        token.transfer(msg.sender, _value);
    }
}