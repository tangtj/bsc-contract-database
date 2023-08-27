// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    function balanceOf(address account) external view returns (uint256);
}

contract Business {
    address public owner;
    address public tokenAddress = 0xdDD42201E485ABa87099089b00978B87E7FBE796;

    struct DepositInfo {
        uint256 amount;
        uint256 depositTime;
    }

    mapping(address => DepositInfo) public userDeposits;
    mapping(address => uint256) public userTotalDeposits_pev;
    mapping(address => uint256) public depositTime_pev;
    address[] public addresses;

    event TokensDeposited(address indexed user, uint256 tokenAmount);
    event TokensWithdrawn(address indexed user, uint256 tokenAmount, address tokenAddress);

    constructor() {
        owner = msg.sender;

        percentageForLevel.push(10); // % F1
        percentageForLevel.push(5);  // % F2
        percentageForLevel.push(2);  // % F3
        percentageForLevel.push(2);  // % F4
        percentageForLevel.push(2);  // % F5
        percentageForLevel.push(2);  // % F6
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    uint256 private TokenPer_USD;

    function TokenPerUSD(uint256 _amount) public onlyOwner returns (bool) {
        TokenPer_USD = _amount;
        return true;
    }

    mapping(address => uint256) public userUSD;
    mapping(address => uint256) public userTotalIncome;
    mapping(address => uint256) public totalReinvested;
    mapping(address => uint256) public startDeposit;

    uint256[] public percentageForLevel;
    mapping(address => address) public f1;
    mapping(address => address) public f2;
    mapping(address => address) public f3;
    mapping(address => address) public f4;
    mapping(address => address) public f5;
    mapping(address => address) public f6;
function Deposit(uint256 _amount, address _referrer) external {
    //Reinvest
    if (userUSD[msg.sender] != 0) { // Check exist
            uint256 EarnedTOKEN = getEarnedTOKEN(msg.sender);
            uint256 DepositUSD  = getDepositUSD(msg.sender)/1000000000000000000;
            require(EarnedTOKEN > 0, "No earned USD to reinvest");
                uint256 reinvestAmount;
                uint256 withdrawalAmount;
            if (DepositUSD >= 20 && DepositUSD < 200) {// withdrawal40 - reinvest60 |  >= $20 | < $200
                    withdrawalAmount = (EarnedTOKEN * 40) / 100;
                    reinvestAmount = (EarnedTOKEN * 60) / 100;
            } 
             else if (DepositUSD >= 200 && DepositUSD < 500) {// withdrawal50 - reinvest50 |  >= $200 | < $500
                    withdrawalAmount = (EarnedTOKEN * 50) / 100;
                    reinvestAmount = (EarnedTOKEN * 50) / 100;
            }
            else if (DepositUSD >= 500 && DepositUSD < 1000) {// withdrawal60 - reinvest40 |  >= $500 | < $1000
                    withdrawalAmount = (EarnedTOKEN * 60) / 100;
                    reinvestAmount = (EarnedTOKEN * 40) / 100;
            }
            else if (DepositUSD >= 1000) {
                // withdrawal70 - reinvest30 | Total deposit > $1000 
                    withdrawalAmount = (EarnedTOKEN * 70) / 100;
                    reinvestAmount = (EarnedTOKEN * 30) / 100;
            }

        require(IERC20(tokenAddress).transfer(msg.sender, withdrawalAmount), "Withdrawal failed");
        emit TokensWithdrawn(msg.sender, withdrawalAmount, tokenAddress);

        // Transfer the reinvested amount in the contract
        require(IERC20(tokenAddress).transfer(address(this), reinvestAmount), "Transfer to contract failed");

        //Income recognition and reinvestment
        userTotalIncome[msg.sender] += withdrawalAmount;
        totalReinvested[msg.sender] += reinvestAmount;
    }
    // Deposit
    require(IERC20(tokenAddress).transferFrom(msg.sender, address(this), _amount), "Deposit failed");

    userTotalDeposits_pev[msg.sender] = userDeposits[msg.sender].amount;
    depositTime_pev[msg.sender] = userDeposits[msg.sender].depositTime;

    userDeposits[msg.sender].amount += _amount;
    userDeposits[msg.sender].depositTime = block.timestamp;

    uint256 USD = _amount / TokenPer_USD;
    userUSD[msg.sender] += USD;

    addresses.push(msg.sender);
    emit TokensDeposited(msg.sender, _amount);
            
        if (startDeposit[msg.sender] == 0) {// Update deposit start time
            startDeposit[msg.sender] = block.timestamp;
        }


    // F1 => F6
    if (f1[msg.sender] == address(0)) {
    f1[msg.sender] = _referrer; //F1
    if (f1[_referrer] != address(0)) {//F2
        f2[msg.sender] = f1[_referrer];
    }
    if (f2[_referrer] != address(0)) {//F3
        f3[msg.sender] = f2[_referrer];
    }
    if (f3[_referrer] != address(0)) {//F4
        f4[msg.sender] = f3[_referrer];
    }
    if (f4[_referrer] != address(0)) {//F5
        f5[msg.sender] = f4[_referrer];
    }
    if (f5[_referrer] != address(0)) {//F6
        f6[msg.sender] = f5[_referrer];
    }
    address f = _referrer;
    for (uint8 i = 0; i < 6; i++) {
        if (f == address(0)) {
            break; // Stop recording when there is no more superior
        }

        // Increase the amount lv F1 => F6 = %
        uint256 earnedAmount = (_amount * percentageForLevel[i]) / 100;
        
        // Record the amount => F
        userTotalIncome[f] += earnedAmount;

        // Move to the next F1 level
        f = f1[f];
    }
    
}

}

    function Withdraw() external {//Withdraw profit
        if (userUSD[msg.sender] != 0) { // Check exist
            uint256 EarnedTOKEN = getEarnedTOKEN(msg.sender);
            uint256 DepositUSD  = getDepositUSD(msg.sender)/1000000000000000000;
            require(EarnedTOKEN > 0, "No earned USD to reinvest");
                uint256 reinvestAmount;
                uint256 withdrawalAmount;
            if (DepositUSD >= 20 && DepositUSD < 200) {// withdrawal40 - reinvest60 |  >= $20 | < $200
                    withdrawalAmount = (EarnedTOKEN * 40) / 100;
                    reinvestAmount = (EarnedTOKEN * 60) / 100;
            } 
             else if (DepositUSD >= 200 && DepositUSD < 500) {// withdrawal50 - reinvest50 |  >= $200 | < $500
                    withdrawalAmount = (EarnedTOKEN * 50) / 100;
                    reinvestAmount = (EarnedTOKEN * 50) / 100;
            }
            else if (DepositUSD >= 500 && DepositUSD < 1000) {// withdrawal60 - reinvest40 |  >= $500 | < $1000
                    withdrawalAmount = (EarnedTOKEN * 60) / 100;
                    reinvestAmount = (EarnedTOKEN * 40) / 100;
            }
            else if (DepositUSD >= 1000) {
                // withdrawal70 - reinvest30 | Total deposit > $1000 
                    withdrawalAmount = (EarnedTOKEN * 70) / 100;
                    reinvestAmount = (EarnedTOKEN * 30) / 100;
            }
                require(IERC20(tokenAddress).transfer(msg.sender, withdrawalAmount), "Withdrawal failed");
                emit TokensWithdrawn(msg.sender, withdrawalAmount, tokenAddress);

                // Transfer the reinvested amount in the contract
                require(IERC20(tokenAddress).transfer(address(this), reinvestAmount), "Transfer to contract failed");

                //Income recognition and reinvestment
                userTotalIncome[msg.sender] += withdrawalAmount;
                totalReinvested[msg.sender] += reinvestAmount;

        }
          
        DepositInfo storage depositInfo = userDeposits[msg.sender];
        require(depositInfo.amount > 0, "No deposit to reinvest");
        userTotalDeposits_pev[msg.sender] = depositInfo.amount;
        depositTime_pev[msg.sender] = depositInfo.depositTime;
        depositInfo.depositTime = block.timestamp;
    }

    function Withdrawal() external {//Withdraw deposited tokens
        uint256 depositTime = block.timestamp - startDeposit[msg.sender];
        if (depositTime >= 86400) {
        // Withdraw deposited tokens
        uint256 withdrawalAmount = userDeposits[msg.sender].amount;
        require(withdrawalAmount > 0, "No deposited tokens to withdraw");
        
        require(IERC20(tokenAddress).transfer(address(this), withdrawalAmount), "Transfer to contract failed");

        // Reset userDeposits, userUSD, and startDeposit
        userDeposits[msg.sender].amount = 0;
        userUSD[msg.sender] = 0;
        startDeposit[msg.sender] = 0;
    } else {
        revert("You need to wait at least 24 hours before withdrawing.");
        }
    }


    function getTokenBalance() public view returns (uint256) {
        IERC20 token = IERC20(tokenAddress);
        return token.balanceOf(address(this));
    }

    function getUserTotalDeposits(address _user) public view returns (uint256) {
        return userDeposits[_user].amount;
    }

    function getTokenPerUSD() public view returns (uint256) {
    return TokenPer_USD;
    }

    function getDepositUSD(address user) public view returns (uint256) {
    return userUSD[user];
    }

    function getEarnedUSD(address user) public view returns (uint256) {
        uint256 timePassed = block.timestamp - userDeposits[user].depositTime;
        uint256 earnedAmount = (userUSD[user] * timePassed) * 1 / (100 * 86400); // 1% per Day
        return earnedAmount;
    }

    function getEarnedTOKEN(address user) public view returns (uint256) {
    uint256 earnedUSD = getEarnedUSD(user);
    uint256 earnedTokens = earnedUSD * TokenPer_USD;
    return earnedTokens;
    }

    function getTotalIncome(address user) public view returns (uint256) {
    return userTotalIncome[user];
    }

    function getTotalReinvested(address user) public view returns (uint256) {
    return totalReinvested[user];
    }

    function getStartDepositTime(address userAddress) public view returns (uint256) {
    return startDeposit[userAddress];
    }

    function getDepositTime(address _user) public view returns (uint256) {
        return userDeposits[_user].depositTime;
    }


    function getIncomeByLevel(address _user) external view returns (uint256[6] memory) {
    uint256[6] memory incomeByLevel;
    address f = _user;
    for (uint8 i = 0; i < 6; i++) {
        if (f == address(0)) {
            break; //Stop when there is no upper level left
        }
        // Take the profit of the corresponding level
        incomeByLevel[i] = userTotalIncome[f];

        // Move to the next F1 level
        f = f1[f];
    }
    return incomeByLevel;
    }

}