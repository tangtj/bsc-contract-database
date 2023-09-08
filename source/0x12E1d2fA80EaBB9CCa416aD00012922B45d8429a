//SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

interface IBEP20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function SupplyPerPhase() external view returns (uint256);
    function balanceOf(address owner) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 value) external;
    function transfer(address to, uint256 value) external;
    function transferFrom(address from, address to, uint256 value) external;
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Transfer(address indexed from, address indexed to, uint256 value);
}

contract Presale {
    IBEP20 public myToken = IBEP20(0x83f512cBa8ad0E1e9Fc0Feb77a2360eF3d7Eb80A);
    IBEP20 public USDT = IBEP20(0x55d398326f99059fF775485246999027B3197955);

    address payable public owner;
    uint256 public referralBonusRate = 10; // 10% referral bonus in USDT
    uint256 public referralBonusTokenRate = 15; // 15% referral bonus in tokens
    uint256 public tokenPerUsd = 100 * 1e9;
    uint256 public soldToken;
    uint256 public SupplyPerPhase = 105000000 * 1e6;
    uint256 public amountRaisedUSDT;
    address payable public fundReceiver = payable(0x719d2810cCdE2cf290F04Eeb3421521854F952EA);
    address public defaultReferrer = 0xEC11772f73c02FB4d25377820EfE9CBCecDA7c19;
    uint256 public constant divider = 100;

    bool public presaleStatus;

    struct user {
        uint256 USDT_balance;
        uint256 token_balance;
    }

    mapping(address => user) public users;

    modifier onlyOwner() {
        require(msg.sender == owner, "PRESALE: Not an owner");
        _;
    }

    event BuyToken(address indexed _user, uint256 indexed _amount);

    constructor() {
        owner = payable(0xEC11772f73c02FB4d25377820EfE9CBCecDA7c19);
        presaleStatus = true;
    }

    // to buy token during preSale time with USDT => for web3 use
    function buyTokenUSDT(uint256 amount, address referrer) public {
    require(presaleStatus, "Presale is not active");
    require(soldToken <= SupplyPerPhase, "All Sold");
    
    // Check if referrer is not specified or is the same as the buyer
    if (referrer == address(0) || referrer == msg.sender) {
        referrer = defaultReferrer;
    }

    // Calculate the referral bonus amounts
    uint256 usdtReferralBonus = (amount * referralBonusRate) / 100;
    uint256 tokenReferralBonus = (StableToToken(amount) * referralBonusTokenRate) / 100;

    // Transfer USDT to the referrer
    USDT.transferFrom(msg.sender, referrer, usdtReferralBonus);

    // Transfer tokens to the referrer
    myToken.transfer(referrer, tokenReferralBonus);
    
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
        myToken = IBEP20(_token);
    }

    //change USDT
    function changeUSDT(address _USDT) external onlyOwner {
        USDT = IBEP20(_USDT);
    }

    // to draw out tokens
    function transferTokens(IBEP20 token, uint256 _value) external onlyOwner {
        token.transfer(msg.sender, _value);
    }
}