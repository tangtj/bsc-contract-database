//SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

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

contract Presale {
    IBEP20 public MYTO  = IBEP20(0xA2a6e0829DF31a3F727b89dEDaC338e4dA03781c);
    IBEP20 public USDT  = IBEP20(0x55d398326f99059fF775485246999027B3197955);
    IBEP20 public BUSD  = IBEP20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56);
    IBEP20 public USDC  = IBEP20(0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d);
    IBEP20 public USDD  = IBEP20(0xd17479997F34dd9156Deef8F95A52D81D265be9c);
    IBEP20 public FDUSD = IBEP20(0xc5f0f7b66764F6ec8C8Dff7BA683102295E16409);
    IBEP20 public TUSD  = IBEP20(0x40af3827F39D0EAcBF4A168f8D4ee67c121D11c9);

    address payable public owner;

    uint256 public tokenPerUsd = 5000 * 1e18;
    uint256 public soldTokenAllPresale;
    uint256 public soldTokenPerPhase;
    uint256 public SupplyPerPhase = 50000000 * 1e18;
    uint256 public amountRaisedAllPresale;
    uint256 public amountRaisedPerPhase;
    uint256 public amountRaisedUSDT;
    uint256 public amountRaisedBUSD;
    uint256 public amountRaisedUSDC;
    uint256 public amountRaisedUSDD;
    uint256 public amountRaisedFDUSD;
    uint256 public amountRaisedTUSD;
    address payable public fundReceiver =
        payable(0xC57aA0417D0183Dd709095a96A6703d4c4dDEd1E);

    uint256 public constant divider = 100;

    bool public presaleStatus;

    struct user {
        uint256 USDT_balance;
        uint256 BUSD_balance;
        uint256 USDC_balance;
        uint256 USDD_balance;
        uint256 FDUSD_balance;
        uint256 TUSD_balance;
        uint256 MYTO_balance;
    }

    mapping(address => user) public users;

    modifier onlyOwner() {
        require(msg.sender == owner, "PRESALE: Not an owner");
        _;
    }

    event BuyToken(address indexed _user, uint256 indexed _amount);

    constructor() {
        owner = payable(0x86AdF1171a4994e40F82D29C4198373AAf956800);
        presaleStatus = true;
    }

    // to buy token during preSale time with USDT => for web3 use
    function buyTokenUSDT(uint256 amount) public {
        require(soldTokenPerPhase <= SupplyPerPhase, "All Sold");

        USDT.transferFrom(msg.sender, fundReceiver, amount);

        uint256 numberOfTokens;
        numberOfTokens = StableToToken(amount);
        MYTO.transfer(msg.sender, numberOfTokens);

        soldTokenPerPhase = soldTokenPerPhase + (numberOfTokens);
        soldTokenAllPresale = soldTokenAllPresale  + (numberOfTokens);

        amountRaisedUSDT = amountRaisedUSDT + (amount);
        amountRaisedPerPhase = amountRaisedPerPhase + (amount);
        amountRaisedAllPresale = amountRaisedAllPresale + (amount);

        users[msg.sender].USDT_balance += amount;

        users[msg.sender].MYTO_balance =
            users[msg.sender].MYTO_balance +
            (numberOfTokens);
    }

    // to buy token during preSale time with BUSD => for web3 use
    function buyTokenBUSD(uint256 amount) public {
        require(soldTokenPerPhase <= SupplyPerPhase, "All Sold");

        BUSD.transferFrom(msg.sender, fundReceiver, amount);

        uint256 numberOfTokens;
        numberOfTokens = StableToToken(amount);
        MYTO.transfer(msg.sender, numberOfTokens);

        soldTokenPerPhase = soldTokenPerPhase + (numberOfTokens);
        soldTokenAllPresale = soldTokenAllPresale  + (numberOfTokens);

        amountRaisedBUSD = amountRaisedBUSD + (amount);
        amountRaisedPerPhase = amountRaisedPerPhase + (amount);
        amountRaisedAllPresale = amountRaisedAllPresale + (amount);

        users[msg.sender].BUSD_balance += amount;

        users[msg.sender].MYTO_balance =
            users[msg.sender].MYTO_balance +
            (numberOfTokens);
    }

    // to buy token during preSale time with USDC => for web3 use
    function buyTokenUSDC(uint256 amount) public {
        require(soldTokenPerPhase <= SupplyPerPhase, "All Sold");

        USDC.transferFrom(msg.sender, fundReceiver, amount);

        uint256 numberOfTokens;
        numberOfTokens = StableToToken(amount);
        MYTO.transfer(msg.sender, numberOfTokens);

        soldTokenPerPhase = soldTokenPerPhase + (numberOfTokens);
        soldTokenAllPresale = soldTokenAllPresale  + (numberOfTokens);

        amountRaisedUSDC = amountRaisedUSDC + (amount);
        amountRaisedPerPhase = amountRaisedPerPhase + (amount);
        amountRaisedAllPresale = amountRaisedAllPresale + (amount);

        users[msg.sender].USDC_balance += amount;

        users[msg.sender].MYTO_balance =
            users[msg.sender].MYTO_balance +
            (numberOfTokens);
    }

    // to buy token during preSale time with USDD => for web3 use
    function buyTokenUSDD(uint256 amount) public {
        require(soldTokenPerPhase <= SupplyPerPhase, "All Sold");

        USDD.transferFrom(msg.sender, fundReceiver, amount);

        uint256 numberOfTokens;
        numberOfTokens = StableToToken(amount);
        MYTO.transfer(msg.sender, numberOfTokens);

        soldTokenPerPhase = soldTokenPerPhase + (numberOfTokens);
        soldTokenAllPresale = soldTokenAllPresale  + (numberOfTokens);

        amountRaisedUSDD = amountRaisedUSDD + (amount);
        amountRaisedPerPhase = amountRaisedPerPhase + (amount);
        amountRaisedAllPresale = amountRaisedAllPresale + (amount);

        users[msg.sender].USDD_balance += amount;

        users[msg.sender].MYTO_balance =
            users[msg.sender].MYTO_balance +
            (numberOfTokens);
    }

    // to buy token during preSale time with FDUSD => for web3 use
    function buyTokenFDUSD(uint256 amount) public {
        require(soldTokenPerPhase <= SupplyPerPhase, "All Sold");

        FDUSD.transferFrom(msg.sender, fundReceiver, amount);

        uint256 numberOfTokens;
        numberOfTokens = StableToToken(amount);
        MYTO.transfer(msg.sender, numberOfTokens);

        soldTokenPerPhase = soldTokenPerPhase + (numberOfTokens);
        soldTokenAllPresale = soldTokenAllPresale  + (numberOfTokens);

        amountRaisedFDUSD = amountRaisedFDUSD + (amount);
        amountRaisedPerPhase = amountRaisedPerPhase + (amount);
        amountRaisedAllPresale = amountRaisedAllPresale + (amount);

        users[msg.sender].FDUSD_balance += amount;

        users[msg.sender].MYTO_balance =
            users[msg.sender].MYTO_balance +
            (numberOfTokens);
    }

    // to buy token during preSale time with TUSD => for web3 use
    function buyTokenTUSD(uint256 amount) public {
        require(soldTokenPerPhase <= SupplyPerPhase, "All Sold");

        TUSD.transferFrom(msg.sender, fundReceiver, amount);

        uint256 numberOfTokens;
        numberOfTokens = StableToToken(amount);
        MYTO.transfer(msg.sender, numberOfTokens);

        soldTokenPerPhase = soldTokenPerPhase + (numberOfTokens);
        soldTokenAllPresale = soldTokenAllPresale  + (numberOfTokens);

        amountRaisedTUSD = amountRaisedTUSD + (amount);
        amountRaisedPerPhase = amountRaisedPerPhase + (amount);
        amountRaisedAllPresale = amountRaisedAllPresale + (amount);

        users[msg.sender].TUSD_balance += amount;

        users[msg.sender].MYTO_balance =
            users[msg.sender].MYTO_balance +
            (numberOfTokens);
    }

    // to change preSale amount limits
    function setSupplyPerPhase(
        uint256 _SupplyPerPhase,
        uint256 _soldTokenPerPhase,
        uint256 _amountRaisedPerPhase
    ) external onlyOwner {
        SupplyPerPhase = _SupplyPerPhase;
        soldTokenPerPhase = _soldTokenPerPhase;
        amountRaisedPerPhase = _amountRaisedPerPhase;
    }

    //change tokenPerUsd
    function changetokenPerUsd(uint256 _tokenPerUsd) external onlyOwner {
        tokenPerUsd = _tokenPerUsd;
    }

    function stopPresale(bool _false) external onlyOwner {
        presaleStatus = _false;
    }

    function startPresale(bool _true) external onlyOwner {
        presaleStatus = _true;
    }

    // to check number of token for given stable coin
    function StableToToken(uint256 _amount) public view returns (uint256) {
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
        MYTO = IBEP20(_token);
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

    //change USDD
    function changeUSDD(address _USDD) external onlyOwner {
        USDD = IBEP20(_USDD);
    }

    //change FDUSD
    function changeFDUSD(address _FDUSD) external onlyOwner {
        FDUSD = IBEP20(_FDUSD);
    }

    //change TUSD
    function changeTUSD(address _TUSD) external onlyOwner {
        TUSD = IBEP20(_TUSD);
    }

    // to draw out tokens
    function transferTokens(IBEP20 token, uint256 _value) external onlyOwner {
        token.transfer(msg.sender, _value);
    }
}