//SPDX-License-Identifier: MIT Licensed
pragma solidity ^0.8.6;

interface IBEP20 {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

    function SupplyPerPhase() external view returns (uint256);

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

contract GALA2presale {
    IBEP20 public GALA2 = IBEP20(0xb4CF9B4624A485340C504C3D2D2556Ef4750EBeC);
    IBEP20 public USDT = IBEP20(0x55d398326f99059fF775485246999027B3197955);
    IBEP20 public BUSD = IBEP20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56);
    IBEP20 public USDC = IBEP20(0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d);

    address payable public owner;

    uint256 public tokenPerUsd = 200 * 1e8;
    uint256 public soldToken;
    uint256 public SupplyPerPhase = 500_000_000 * 1e8;
    uint256 public amountRaisedUSDT;
    uint256 public amountRaisedBUSD;
    uint256 public amountRaisedUSDC;
    address payable public fundReceiver =
        payable(0x613e4b06a44848D93EAADE56D53B360e49A3303d);

    uint256 public constant divider = 100;

    bool public presaleStatus;

    struct user {
        uint256 USDT_balance;
        uint256 BUSD_balance;
        uint256 USDC_balance;
        uint256 token_balance;
    }

    mapping(address => user) public users;

    modifier onlyOwner() {
        require(msg.sender == owner, "PRESALE: Not an owner");
        _;
    }

    event BuyToken(address indexed _user, uint256 indexed _amount);

    constructor() {
        owner = payable(0x613e4b06a44848D93EAADE56D53B360e49A3303d);

        presaleStatus = true;
    }

    // to buy token during preSale time with USDT => for web3 use
    function buyTokenUSDT(uint256 amount) public {
        require(soldToken <= SupplyPerPhase, "All Sold");

        USDT.transferFrom(msg.sender, fundReceiver, amount);

        uint256 numberOfTokens;
        numberOfTokens = USDToToken(amount);
        GALA2.transfer(msg.sender, numberOfTokens);

        soldToken = soldToken + (numberOfTokens);
        amountRaisedUSDT = amountRaisedUSDT + (amount);

        users[msg.sender].USDT_balance += amount;

        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens);
    }

    // to buy token during preSale time with BUSD => for web3 use
    function buyTokenBUSD(uint256 amount) public {
        require(soldToken <= SupplyPerPhase, "All Sold");

        BUSD.transferFrom(msg.sender, fundReceiver, amount);

        uint256 numberOfTokens;
        numberOfTokens = USDToToken(amount);
        GALA2.transfer(msg.sender, numberOfTokens);

        soldToken = soldToken + (numberOfTokens);
        amountRaisedBUSD = amountRaisedBUSD + (amount);

        users[msg.sender].BUSD_balance += amount;

        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens);
    }

    // to buy token during preSale time with USDC => for web3 use
    function buyTokenUSDC(uint256 amount) public {
        require(soldToken <= SupplyPerPhase, "All Sold");

        USDC.transferFrom(msg.sender, fundReceiver, amount);

        uint256 numberOfTokens;
        numberOfTokens = USDToToken(amount);
        GALA2.transfer(msg.sender, numberOfTokens);

        soldToken = soldToken + (numberOfTokens);
        amountRaisedUSDC = amountRaisedUSDC + (amount);

        users[msg.sender].USDC_balance += amount;

        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens);
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

    // to check number of token for given usdt
    function USDToToken(uint256 _amount) public view returns (uint256) {
        uint256 numberOfTokens = (_amount * (tokenPerUsd)) / (1e18);
        return numberOfTokens;
    }

    // to change Price of the token
    function changePrice(uint256 _price) external onlyOwner {
        tokenPerUsd = _price;
    }

    // transfer ownership
    function changeOwner(address payable _newOwner) external onlyOwner {
        owner = _newOwner;
    }

    // change tokens
    function changeToken(address _token) external onlyOwner {
        GALA2 = IBEP20(_token);
    }

    //change USDT
    function changeUSDT(address _USDT) external onlyOwner {
        USDT = IBEP20(_USDT);
    }

    //change BUSD
    function changeBUSD(address _BUSD) external onlyOwner {
        USDC = IBEP20(_BUSD);
    }

    //change USDC
    function changeUSDC(address _USDC) external onlyOwner {
        BUSD = IBEP20(_USDC);
    }

    // to draw out tokens
    function transferTokens(IBEP20 token, uint256 _value) external onlyOwner {
        token.transfer(msg.sender, _value);
    }
}