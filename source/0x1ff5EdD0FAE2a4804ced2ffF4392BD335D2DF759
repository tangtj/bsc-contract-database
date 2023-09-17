/**
 *Submitted for verification at testnetstage.bscscan.com on 2023-09-15
 */

/**
 *Submitted for verification at testnetstage.bscscan.com on 2023-09-15
 */

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
    IERC20 public Token;
    IERC20 public USDT = IERC20(0x55d398326f99059fF775485246999027B3197955);
    IERC20 public SANABEL = IERC20(0x75685Cbc08650e62A4d1d9a9cD5Dea63541D0418);

    AggregatorV3Interface public priceFeeD;

    address payable public owner;
    uint256 public tokenPerUsd = 100 * 1e8;
    uint256 public tokenPerSANABEL = 1 * 1e5;

    uint256 public soldToken;
    uint256 public minPurchase = 5000 * 1e8;
    uint256 public maxPurchase = 100_000 * 1e8;
    uint256 public totalSupply = 7_000_000 * 1e8;
    uint256 public reward = 10;
    uint256 public amountRaised;
    uint256 public amountRaisedUSDT;
    uint256 public amountRaisedSANABEL;
    address payable public fundReceiver;
    uint256 public sale_start_at;
    uint256 public sale_ends_at;

    uint256 public constant divider = 100;

    struct user {
        uint256 native_balance;
        uint256 usdt_balance;
        uint256 SANABEL_balance;
        uint256 token_balance;
        uint256 ref_token_balance;
    }

    mapping(address => user) public users;

    modifier onlyOwner() {
        require(msg.sender == owner, "PRESALE: Not an owner");
        _;
    }

    event BuyToken(address indexed _user, uint256 indexed _amount);
    event UpdatePrice(uint256 _oldPrice, uint256 _newPrice);
    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor(uint256 _start) {
        fundReceiver = payable(0x11E9A0907A777bf22Ad28c03AA82c0f6E37E98b6);
        Token = IERC20(address(0xC2f5bee51bA7244CBbcb304D8E723761817B34aF));
        owner = payable(msg.sender);
        priceFeeD = AggregatorV3Interface(
            0x0567F2323251f0Aab15c8dFb1967E4e8A7D42aeE
        );
        sale_start_at = _start;
        sale_ends_at = sale_start_at + 27 days;
    }

    receive() external payable {}

    // to get real time price of Eth
    function getLatestPrice() public view returns (uint256) {
        (, int256 price, , , ) = priceFeeD.latestRoundData();
        return uint256(price);
    }

    // to buy token during preSale time with Eth => for web3 use

    function buyToken(address _ref) public payable {
        require(
            block.timestamp >= sale_start_at,
            " Presale is not started yet, check back later"
        );
        require(
            block.timestamp <= sale_ends_at,
            " Presale is over, check back later"
        );
        (, uint256 price, ) = phaseandPrice();
        uint256 numberOfTokens;
        numberOfTokens = NativeToToken(msg.value, price);
        require(numberOfTokens >= minPurchase, "minimum purchase is 5k");
        uint256 ref_percent = (numberOfTokens * reward) / 100;
        soldToken = soldToken + (numberOfTokens);
        require(soldToken <= totalSupply, "Low balance in pool ");
        amountRaised = amountRaised + (msg.value);
        fundReceiver.transfer(msg.value);
        users[msg.sender].native_balance =
            users[msg.sender].native_balance +
            (msg.value);
        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens);
        require(
            users[msg.sender].token_balance <= maxPurchase,
            "max purchase reached"
        );

        users[_ref].token_balance = users[_ref].token_balance + (ref_percent);
        Token.transfer(msg.sender, numberOfTokens);
        Token.transfer(_ref, ref_percent);
    }

    // to buy token during preSale time with USDT => for web3 use
    function buyTokenUSDT(address _ref, uint256 amount) public {
        require(
            block.timestamp >= sale_start_at,
            " Presale is not started yet, check back later"
        );
        require(
            block.timestamp <= sale_ends_at,
            " Presale is over, check back later"
        );
        (, uint256 price, ) = phaseandPrice();

        USDT.transferFrom(msg.sender, fundReceiver, amount);

        uint256 numberOfTokens;
        numberOfTokens = usdtToToken(amount, price);
        require(numberOfTokens >= minPurchase, "minimum purchase is 5k");

        uint256 ref_percent = (numberOfTokens * reward) / 100;
        soldToken = soldToken + (numberOfTokens);
        require(soldToken <= totalSupply, "Low balance in pool ");
        amountRaisedUSDT = amountRaisedUSDT + (amount);

        users[msg.sender].usdt_balance += amount;

        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens);
        require(numberOfTokens >= minPurchase, "minimum purchase is 5k");
        require(
            users[msg.sender].token_balance <= maxPurchase,
            "max purchase reached"
        );
        users[_ref].token_balance = users[_ref].token_balance + (ref_percent);
        Token.transfer(msg.sender, numberOfTokens);
        Token.transfer(_ref, ref_percent);
    }

    // to buy token during preSale time with USDc => for web3 use
    function buyTokenSANABEL(address _ref, uint256 amount) public {
        require(
            block.timestamp <= sale_ends_at,
            " Presale is over, check back later"
        );
        SANABEL.transferFrom(msg.sender, fundReceiver, amount);

        uint256 numberOfTokens;
        numberOfTokens = SANABELToToken(amount);
        uint256 ref_percent = (numberOfTokens * reward) / 100;

        soldToken = soldToken + (numberOfTokens);
        require(soldToken <= totalSupply, "Low balance in pool ");
        amountRaisedSANABEL = amountRaisedSANABEL + (amount);

        users[msg.sender].SANABEL_balance += amount;
        require(
            users[msg.sender].SANABEL_balance <= 20_000_000 * 1e9,
            "max purchase with SANABEL is 20k QTAXI"
        );

        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens);
        users[_ref].token_balance = users[_ref].token_balance + (ref_percent);
        Token.transfer(msg.sender, numberOfTokens);
        Token.transfer(_ref, ref_percent);
    }

    // to check number of token for given Eth
    function NativeToToken(
        uint256 _amount,
        uint256 _price
    ) public view returns (uint256) {
        uint256 EthToUsd = (_amount * (getLatestPrice())) / (1 ether);
        uint256 numberOfTokens = (EthToUsd * (_price)) / (1e8);
        return numberOfTokens;
    }

    // to check number of token for given usdt
    function usdtToToken(
        uint256 _amount,
        uint256 _price
    ) public pure returns (uint256) {
        uint256 numberOfTokens = (_amount * (_price)) / (1e18);
        return numberOfTokens;
    }

    // to check number of token for given usdt
    function SANABELToToken(uint256 _amount) public view returns (uint256) {
        uint256 numberOfTokens = (_amount * (tokenPerSANABEL)) / (1e9);
        return numberOfTokens;
    }

    function ChangeSupply(
        uint256 _supply,
        uint256 _sold,
        uint256 _raised,
        uint256 _raisedInUsdt,
        uint256 _raisedInSANABEL
    ) external onlyOwner {
        totalSupply = _supply;
        soldToken = _sold;
        amountRaised = _raised;
        amountRaisedUSDT = _raisedInUsdt;
        amountRaisedSANABEL = _raisedInSANABEL;
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

    function changeTime(uint256 _start, uint256 _end) external onlyOwner {
        sale_start_at = _start;
        sale_ends_at = _end;
    }

    // change tokens
    function changeToken(address _token) external onlyOwner {
        Token = IERC20(_token);
    }

    //change tokens for buy
    function changeStableTokens(
        address _USDT,
        address _SANABEL
    ) external onlyOwner {
        USDT = IERC20(_USDT);
        SANABEL = IERC20(_SANABEL);
    }

    function phaseandPrice()
        public
        view
        returns (uint256 _phase, uint256 _tokenPerUsd, uint256 _stageEndTime)
    {
        if (
            block.timestamp > sale_start_at &&
            block.timestamp < sale_start_at + 10 days
        ) {
            _tokenPerUsd = 100 * 1e8;
            _phase = 1;
            _stageEndTime = sale_start_at + 10 days;
        } else if (
            block.timestamp > sale_start_at + 10 days &&
            block.timestamp < sale_start_at + 17 days
        ) {
            _tokenPerUsd = 6666 * 1e6;
            _phase = 2;
            _stageEndTime = sale_start_at + 17 days;
        } else if (block.timestamp > sale_start_at + 17 days) {
            _tokenPerUsd = 50 * 1e8;
            _phase = 3;
            _stageEndTime = sale_start_at + 27 days;
        }
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