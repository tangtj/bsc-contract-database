/**
 *Submitted for verification at Etherscan.io on 2023-02-12
 */

pragma solidity 0.8.15;

//SPDX-License-Identifier: MIT Licensed

interface IToken {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 value) external;

    function transfer(address to, uint256 value) external;

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external;

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

contract Presale {
    IToken public BDINU = IToken(0xBD79ef478C6C11e92D0e99fe87C97243cC425D3a);
    IToken public USDT = IToken(0x55d398326f99059fF775485246999027B3197955);
    IToken public BUSD = IToken(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56);
    AggregatorV3Interface public priceFeedEth;

    address payable public owner;

    uint256 public tokenPerUsd = 4761904760000000000; //4.7 tokens for $1
    uint256 public preSaleStartTime;
    uint256 public soldToken;
    uint256 public totalSupply = 15_000_000 ether; //presale tokens
    uint256 public availableBonus;
    uint256 public totalBonuses;
    uint256 public amountRaisedEth;
    uint256 public amountRaisedUSDT;
    uint256 public amountRaisedBUSD;
    uint256 public minimumDollar = 100 * 10**USDT.decimals(); //min buy usdt
    uint256 public minimumETH = 0.05 ether; //min buy eth
    uint256 public constant divider = 100;
    uint256[6] public balance = [50, 100, 250, 500, 1000, 5000];
    uint256[6] public bonuses = [30, 50, 100, 200, 300, 500];

    bool public presaleStatus;

    struct user {
        uint256 Bnb_balance;
        uint256 busd_balance;
        uint256 usdt_balance;
        uint256 token_balance;
        uint256 user_bonus;
    }

    mapping(address => user) public users;

    modifier onlyOwner() {
        require(msg.sender == owner, "PRESALE: Not an owner");
        _;
    }

    event BuyTokenBnb(address indexed _user, uint256 indexed _amount);
    event BuyTokenUSDT(address indexed _user, uint256 indexed _amount);
    event BuyTokenBUSD(address indexed _user, uint256 indexed _amount);
    event ClaimTokens(address indexed _user, uint256 indexed _amount);

    constructor() {
        owner = payable(0xcB4a56B6b5BeFfF46Db2D20F879c41De2d23346b);
        priceFeedEth = AggregatorV3Interface(
            0x0567F2323251f0Aab15c8dFb1967E4e8A7D42aeE
        );
        preSaleStartTime = block.timestamp;
        presaleStatus = true;
    }

    //0x5f4eC3Df9cbd43714FE2740f5E3616155c5b8419 eth/usd mainnet
    //0x0567F2323251f0Aab15c8dFb1967E4e8A7D42aeE bnb/usd mainnet
    //0x694AA1769357215DE4FAC081bf1f309aDC325306 eth/usd sepolia testnet
    //0xD4a33860578De61DBAbDc8BFdb98FD742fA7028e eth/usd goerli tesnet
    //0x2514895c72f50D8bd4B4F9b1110F0D6bD2c97526 bnb/usd bsc testnet
    //0xdAC17F958D2ee523a2206206994597C13D831ec7 usdt mainnet ETH
    //0x55d398326f99059fF775485246999027B3197955 usdt bsc network
    //0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56 busd bsc network
    //0x9331b55D9830EF609A2aBCfAc0FBCE050A52fdEa busd bsc testnet
    //0xBD79ef478C6C11e92D0e99fe87C97243cC425D3a BCONG bsc mainnet

    receive() external payable {}

    // to get real time price of Eth
    function getLatestPriceBnb() private view returns (uint256) {
        (, int256 price, , , ) = priceFeedEth.latestRoundData();
        return uint256(price);
    }

    function bnbToUsd(uint256 bnbAmount) public view returns (uint256) {
        (, int256 price, , , ) = priceFeedEth.latestRoundData();
        require(price > 0, "Price not available");

        uint256 usdValue = (uint256(price) * bnbAmount * 10**USDT.decimals()) /
            (10**8); // Value in USD with 8 decimal places

        return usdValue / 1e18;
    }

    // to buy token during preSale time with Eth => for web3 use

    function buyTokenBnb() public payable {
        require(presaleStatus == true, "Presale : Presale is finished");
        require(msg.value >= minimumETH, "Presale : Unsuitable Amount");
        require(soldToken <= totalSupply, "All Sold");

        uint256 numberOfTokens;
        numberOfTokens = EthToToken(msg.value);
        owner.transfer(msg.value);
        soldToken = soldToken + (numberOfTokens);

        uint256 _bonus = calculateBonus(msg.value, true, false, false);
        uint256 _userBonus = applyBonus(numberOfTokens, _bonus);
        amountRaisedEth = amountRaisedEth + (msg.value);
        users[msg.sender].Bnb_balance =
            users[msg.sender].Bnb_balance +
            (msg.value);
        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens);
        users[msg.sender].user_bonus =
            users[msg.sender].user_bonus +
            (_userBonus);
            totalBonuses += _userBonus;
        emit BuyTokenBnb(msg.sender, msg.value);
    }

    // to buy token during preSale time with USDT => for web3 use
    function buyTokenUSDT(uint256 amount) public {
        require(presaleStatus == true, "Presale : Presale is finished");
        require(amount >= minimumDollar, "Minimum Amount is $100");
        require(soldToken <= totalSupply, "All Sold");
        uint256 numberOfTokens;
        numberOfTokens = usdtToToken(amount);
        USDT.transferFrom(msg.sender, owner, amount);
        soldToken = soldToken + (numberOfTokens);
        uint256 _bonus = calculateBonus(amount, false, true, false);
        uint256 _userBonus = applyBonus(numberOfTokens, _bonus);
        amountRaisedUSDT = amountRaisedUSDT + (amount);
        users[msg.sender].usdt_balance =
            users[msg.sender].usdt_balance +
            (amount);
        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens);
        users[msg.sender].user_bonus =
            users[msg.sender].user_bonus +
            (_userBonus);
            totalBonuses += _userBonus;
        emit BuyTokenUSDT(msg.sender, amount);
    }

    // to buy token during preSale time with USDT => for web3 use
    function buyTokenBUSD(uint256 amount) public {
        require(presaleStatus == true, "Presale : Presale is finished");
        require(amount >= minimumDollar, "Minimum Amount is $100");
        require(soldToken <= totalSupply, "All Sold");
        uint256 numberOfTokens;
        numberOfTokens = busdToToken(amount);
        BUSD.transferFrom(msg.sender, owner, amount);
        soldToken = soldToken + (numberOfTokens);
        uint256 _bonus = calculateBonus(amount, false, false, true);
        uint256 _userBonus = applyBonus(numberOfTokens, _bonus);
        amountRaisedBUSD = amountRaisedBUSD + (amount);
        users[msg.sender].busd_balance =
            users[msg.sender].busd_balance +
            (amount);
        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens);
            users[msg.sender].user_bonus =
            users[msg.sender].user_bonus +
            (_userBonus);
            totalBonuses += _userBonus;
        emit BuyTokenBUSD(msg.sender, amount);
    }

    function claimTokens() external {
        require(!presaleStatus, "Presale not over yet");
        require(
            users[msg.sender].token_balance > 0,
            "Not enough tokens balance"
        );
        uint256 numberOfTokens = (users[msg.sender].token_balance) + (users[msg.sender].user_bonus) ;
        BDINU.transfer(msg.sender, numberOfTokens);
        availableBonus -= users[msg.sender].user_bonus;
        users[msg.sender].token_balance = 0;
        users[msg.sender].user_bonus = 0;
        emit ClaimTokens(msg.sender, numberOfTokens);
    }

    function calculateBonus(
        uint256 amount,
        bool usingBnb,
        bool usingUsdt,
        bool usingBusd
    ) private view returns (uint256) {
        uint256 _userBonus;

        if (usingBnb) {
            uint256 _bnbToUsd = bnbToUsd(amount);

            for (uint256 i = 0; i < balance.length; i++) {
                if (
                    _bnbToUsd >= balance[i] * 10**USDT.decimals() &&
                    (_bnbToUsd < balance[i + 1] * 10**USDT.decimals() ||
                        i == balance.length - 1)
                ) {
                    _userBonus = bonuses[i];
                    break;
                }
            }
        } else if (usingUsdt) {
            for (uint256 i = 0; i < balance.length; i++) {
                if (
                    amount >= balance[i] * 10**USDT.decimals() &&
                    (amount < balance[i + 1] * 10**USDT.decimals() ||
                        i == balance.length - 1)
                ) {
                    _userBonus = bonuses[i];
                    break;
                }
            }
        } else if (usingBusd) {
            for (uint256 i = 0; i < balance.length; i++) {
                if (
                    amount >= balance[i] * 10**BUSD.decimals() &&
                    (amount < balance[i + 1] * 10**BUSD.decimals() ||
                        i == balance.length - 1)
                ) {
                    _userBonus = bonuses[i];
                    break;
                }
            }
        }

        return _userBonus;
    }

    function applyBonus(uint256 amount, uint256 bonusPercantgae)
        private
        pure
        returns (uint256)
    {
        return (amount * bonusPercantgae) / divider;
    }

    // to check percentage of token sold
    function getProgress() external view returns (uint256 _percent) {
        uint256 remaining = totalSupply -
            (soldToken / (10**(BDINU.decimals())));
        remaining = (remaining * (divider)) / (totalSupply);
        uint256 hundred = 100;
        return hundred - (remaining);
    }

    function stopPresale(bool state) external onlyOwner {
        presaleStatus = state;
    }

    //owner can bonuses in the contract
    function addBonus(uint256 _amount) external onlyOwner {
        BDINU.transferFrom(msg.sender, address(this), _amount);
        availableBonus += _amount;
    }

    // to check number of token for given Eth
    function EthToToken(uint256 _amount) public view returns (uint256) {
        uint256 EthToUsd = (_amount * (getLatestPriceBnb())) / (1 ether);
        uint256 numberOfTokens = (EthToUsd * (tokenPerUsd)) / (1e8);
        return numberOfTokens;
    }

    // to check number of token for given usdt
    function usdtToToken(uint256 _amount) public view returns (uint256) {
        uint256 numberOfTokens = (_amount * (tokenPerUsd)) /
            10**(USDT.decimals());
        return numberOfTokens;
    }

    // to check number of token for given busd
    function busdToToken(uint256 _amount) public view returns (uint256) {
        uint256 numberOfTokens = (_amount * (tokenPerUsd)) /
            10**(BUSD.decimals());
        return numberOfTokens;
    }

    // to change Price of the token
    function changePrice(uint256 _price) external onlyOwner {
        tokenPerUsd = _price;
    }

    // to change preSale time duration
    function setPreSaleTime(uint256 _startTime) external onlyOwner {
        preSaleStartTime = _startTime + block.timestamp;
    }

    // transfer ownership
    function changeOwner(address payable _newOwner) external onlyOwner {
        owner = _newOwner;
    }

    // change tokens
    function changeToken(address _token) external onlyOwner {
        BDINU = IToken(_token);
    }

    // change minimum buy
    function changeMinimumLimits(uint256 _inDollar, uint256 _inEth)
        external
        onlyOwner
    {
        minimumDollar = _inDollar;
        minimumETH = _inEth;
    }

    // change supply
    function changeTotalSupply(uint256 _total) external onlyOwner {
        totalSupply = _total;
    }

    //change USDT
    function changeUSDT(address _USDT) external onlyOwner {
        USDT = IToken(_USDT);
    }

    //change BUSD
    function changeBUSD(address _BUSD) external onlyOwner {
        BUSD = IToken(_BUSD);
    }

    // to draw funds for liquidity
    function transferFundsBnb(uint256 _value) external onlyOwner {
        require(address(this).balance > 0, "Not enough bnb");
        owner.transfer(_value);
    }

    // to draw out tokens
    function transferTokens(IToken token, uint256 _value) external onlyOwner {
        require(
            token.balanceOf(address(this)) > 0,
            "Not enough tokens to withdraw"
        );
        token.transfer(msg.sender, _value);
    }

    // to get current UTC time
    function getCurrentTime() external view returns (uint256) {
        return block.timestamp;
    }

    // to get contract Eth balance
    function contractBalanceEth() external view returns (uint256) {
        return address(this).balance;
    }

    //to get contract USDT balance
    function contractBalanceUSDT() external view returns (uint256) {
        return USDT.balanceOf(address(this));
    }

    //to get contract BUSD balance
    function contractBalanceBUSD() external view returns (uint256) {
        return BUSD.balanceOf(address(this));
    }

    //to get contract USDT balance
    function contractBalanceBDINU() external view returns (uint256) {
        return BDINU.balanceOf(address(this));
    }

    // to get contract token balance
    function getContractTokenApproval() external view returns (uint256) {
        return BDINU.allowance(owner, address(this));
    }
    
    // to change the balance requirement for bonus
    function changeBalance(uint256 _balance, uint8 index) external onlyOwner{
        require(balance.length >= index, "Invalid index");
        balance[index] = _balance;
    }

    // to change the bonus percantage
    function changeBonusPer(uint256 _per, uint8 index) external onlyOwner{
        require(bonuses.length >= index, "Invalid index");
        bonuses[index] = _per;
    }
}