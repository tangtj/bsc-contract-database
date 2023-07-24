// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IBUSD {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
}

contract BluesevenInvestment {
    address private owner;
    address private partner;
    uint256 private totalInvested;
    uint256 private constant MIN_INVESTMENT_BUSD = 5 * (10 ** 18); // 5 BUSD (considering 18 decimal places)
    uint256 private constant INTEREST_RATE = 5; // 5% weekly profit
    uint256 private constant WEEK_IN_SECONDS = 1 weeks;
    uint256 private constant LOTTERY_INTERVAL = 1 weeks;
    uint256 private constant LOTTERY_DIGITS = 5;
    uint256 private constant MIN_LOTTERY_INVESTMENT_BUSD = 200 * (10 ** 18); // 200 BUSD (considering 18 decimal places)
    uint256 private constant REFERRAL_BONUS_RATE = 3;
    uint256 private constant CUSTOM_LINK_BONUS_RATE = 10;
    uint256 private constant MINIMUM_HOLDING_PERIOD = 15 days;

    IBUSD private busd;
    mapping(address => Investor) private investors;
    mapping(bytes32 => address) private referrals;

    uint256 private busdBalance;
    uint256 private additionalFunds; // Variable to track additional funds deposited by the owner or partner

    struct Investor {
        uint256 investedAmount;
        uint256 totalInvested;
        uint256 lastDepositTimestamp;
        uint256 lastWithdrawalTimestamp;
        uint256 referralBonus;
        uint256 lotteryNumber;
    }

    event Deposit(address indexed investor, uint256 amount, uint256 lotteryNumber, uint256 totalInvested);
    event Withdrawal(address indexed investor, uint256 amount);
    event TransferToTelegram(string message);
    event InitialInvestmentWithdrawn(address indexed investor, uint256 amount);
    event ReferralBonus(address indexed referrer, address indexed referral, uint256 amount);
    event ReferralBonusWithdrawn(address indexed investor, uint256 amount);

    constructor(address _busdAddress, address _partner) {
        owner = msg.sender;
        partner = _partner;
        busd = IBUSD(_busdAddress);
    }

    modifier onlyOwnerOrPartner() {
        require(msg.sender == owner || msg.sender == partner, "Only owner or partner can perform this action.");
        _;
    }

    modifier onlyOnSaturday() {
        require((block.timestamp % 1 weeks) == 6 days, "Withdrawals are only allowed on Saturdays.");
        _;
    }

    function invest(uint256 amount) public {
        require(amount >= MIN_INVESTMENT_BUSD, "Minimum investment amount not reached.");

        uint256 lotteryNumber = 0;
        if (amount >= MIN_LOTTERY_INVESTMENT_BUSD && busd.allowance(msg.sender, address(this)) >= MIN_LOTTERY_INVESTMENT_BUSD) {
            lotteryNumber = generateLotteryNumber();
        }

        Investor storage investor = investors[msg.sender];

        // Update the total invested by the investor
        investor.totalInvested += amount;

        investor.investedAmount = amount;
        investor.lastDepositTimestamp = block.timestamp;
        investor.lastWithdrawalTimestamp = block.timestamp;
        investor.referralBonus = 0;
        investor.lotteryNumber = lotteryNumber;

        totalInvested += amount;

        require(busd.transferFrom(msg.sender, address(this), amount), "Failed to transfer BUSD.");

        busdBalance += amount;

        emit Deposit(msg.sender, amount, lotteryNumber, investor.totalInvested);
    }

    function investWithReferral(uint256 amount, bytes32 referralCode) external {
        require(amount >= MIN_INVESTMENT_BUSD, "Minimum investment amount not reached.");

        address referrer = referrals[referralCode];
        require(referrer != address(0), "Invalid referral code.");

        uint256 referralBonus = (amount * REFERRAL_BONUS_RATE) / 100;

        Investor storage referrerInvestor = investors[referrer];
        referrerInvestor.referralBonus += referralBonus;

        invest(amount); // Call the original invest function

        emit ReferralBonus(referrer, msg.sender, referralBonus);
    }

    function generateLotteryNumber() private view returns (uint256) {
        return uint256(keccak256(abi.encodePacked(block.timestamp, msg.sender))) % (10 ** LOTTERY_DIGITS);
    }

    function calculateProfit(address investorAddress) private view returns (uint256) {
        Investor memory investor = investors[investorAddress];
        uint256 timeDiff = block.timestamp - investor.lastWithdrawalTimestamp;
        uint256 weeklyProfit = (investor.investedAmount * INTEREST_RATE) / 100;
        uint256 totalProfit = (timeDiff / WEEK_IN_SECONDS) * weeklyProfit;
        return totalProfit;
    }

    function withdrawProfit() external onlyOnSaturday {
        Investor storage investor = investors[msg.sender];
        require(investor.investedAmount > 0, "No investment found.");

        uint256 profit = calculateProfit(msg.sender);
        require(profit > 0, "No profit available for withdrawal.");

        investor.lastWithdrawalTimestamp = block.timestamp;

        // Include the additional funds deposited by the owner or partner
        uint256 totalProfit = profit + (additionalFunds / totalInvested) * investor.investedAmount;

        // Subtract the portion of additional funds used for this investor
        additionalFunds -= (additionalFunds / totalInvested) * investor.investedAmount;

        require(busd.transfer(msg.sender, totalProfit), "Failed to transfer profit.");

        emit Withdrawal(msg.sender, totalProfit);
    }

    function withdrawFunds() external onlyOwnerOrPartner {
        uint256 contractBalance = busdBalance;
        require(contractBalance > 0, "No funds available for withdrawal.");
        require(busd.transfer(owner, contractBalance), "Failed to transfer BUSD.");

        busdBalance = 0;
    }

    function getContractBalance() external view returns (uint256) {
        return busdBalance;
    }

    function getReferralLink(address investorAddress) external pure returns (string memory) {
        bytes32 hash = keccak256(abi.encodePacked(investorAddress));
        string memory baseLink = "https://blueseven.finance/referral?ref=";
        return string(abi.encodePacked(baseLink, toAsciiString(hash)));
    }

    function getReferralBonus(address investorAddress) external view returns (uint256) {
        Investor memory investor = investors[investorAddress];
        return investor.referralBonus;
    }

    function withdrawReferralBonus() external {
        Investor storage investor = investors[msg.sender];
        require(investor.referralBonus > 0, "No referral bonus available for withdrawal.");

        uint256 bonus = investor.referralBonus;
        investor.referralBonus = 0;

        require(busd.transfer(msg.sender, bonus), "Failed to transfer referral bonus.");

        emit ReferralBonusWithdrawn(msg.sender, bonus);
    }

    function toAsciiString(bytes32 data) private pure returns (string memory) {
        bytes memory s = new bytes(64);
        for (uint i = 0; i < 32; i++) {
            uint8 b = uint8(data[i]);
            s[i * 2] = bytes1(uint8(b / 16 + 48));
            s[i * 2 + 1] = bytes1(uint8(b % 16 + 48));
        }
        return string(s);
    }

    function withdrawInitialInvestment() external {
        Investor storage investor = investors[msg.sender];
        require(investor.investedAmount > 0, "No investment found.");
        require(block.timestamp >= investor.lastDepositTimestamp + MINIMUM_HOLDING_PERIOD, "Minimum holding period not met.");

        uint256 initialInvestment = investor.investedAmount;

        investor.investedAmount = 0;

        require(busd.transfer(msg.sender, initialInvestment), "Failed to transfer initial investment.");

        emit InitialInvestmentWithdrawn(msg.sender, initialInvestment);
    }

    // Função para obter o saldo total investido por um investidor
    function getTotalInvested(address investorAddress) external view returns (uint256) {
        return investors[investorAddress].totalInvested;
    }

    // Função para obter o saldo total investido no contrato
    function getTotalInvestedInContract() external view returns (uint256) {
        return totalInvested;
    }

    // Função para adicionar saldo ao contrato
    function addFundsToContract(uint256 amount) external onlyOwnerOrPartner {
        require(amount > 0, "Amount should be greater than zero.");
        additionalFunds += amount;
        totalInvested += amount;
        busdBalance += amount;
    }

    // Função para gerar um link de indicação para um divulgador
    function generateReferralLink(bytes32 referralCode) external onlyOwnerOrPartner {
        require(referrals[referralCode] == address(0), "Referral code already generated.");
        address referrer = msg.sender;
        referrals[referralCode] = referrer;
    }
}