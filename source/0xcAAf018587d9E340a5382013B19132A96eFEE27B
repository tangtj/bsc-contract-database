// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    function balanceOf(address account) external view returns (uint256);
}

contract Staking {
    address private owner;
    address public tokenAddress = 0xBF6268d386903623804DdB98dC83C7fC319CCFf3;

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
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

        mapping(address => uint256) public userTotalIncome;
        mapping(address => uint256) public startDeposit;
    function Deposit(uint256 _amount) external {
        //Takeprofit
        uint256 EarnedTOKEN = getEarnedTOKEN(msg.sender);
        if (EarnedTOKEN > 0) {
        require(IERC20(tokenAddress).transfer(msg.sender, EarnedTOKEN), "Withdrawal failed");
        emit TokensWithdrawn(msg.sender, EarnedTOKEN, tokenAddress);
        }

        //Income recognition
        userTotalIncome[msg.sender] += EarnedTOKEN;

        // Deposit
        require(IERC20(tokenAddress).transferFrom(msg.sender, address(this), _amount), "Deposit failed");

        userTotalDeposits_pev[msg.sender] = userDeposits[msg.sender].amount;
        depositTime_pev[msg.sender] = userDeposits[msg.sender].depositTime;

        userDeposits[msg.sender].amount += _amount;
        userDeposits[msg.sender].depositTime = block.timestamp;

        addresses.push(msg.sender);
        emit TokensDeposited(msg.sender, _amount);
            
        if (startDeposit[msg.sender] == 0) {
            startDeposit[msg.sender] = block.timestamp;// Update deposit start time
        }
    }

    function Takeprofit() external {//Takeprofit
        uint256 EarnedTOKEN = getEarnedTOKEN(msg.sender);
        require(IERC20(tokenAddress).transfer(msg.sender, EarnedTOKEN), "Withdrawal failed");
        emit TokensWithdrawn(msg.sender, EarnedTOKEN, tokenAddress);

        //Income recognition
        userTotalIncome[msg.sender] += EarnedTOKEN;

        DepositInfo storage depositInfo = userDeposits[msg.sender];
        require(depositInfo.amount > 0, "No deposit to take profit");
        userTotalDeposits_pev[msg.sender] = depositInfo.amount;
        depositTime_pev[msg.sender] = depositInfo.depositTime;
        depositInfo.depositTime = block.timestamp;  
    }

    function Withdrawal() external {//Withdraw deposited tokens
        uint256 depositTime = block.timestamp - startDeposit[msg.sender];
        if (depositTime >= 60) { // 2592000 seconds = 30 days pass condition.
        // Withdraw deposited tokens
        uint256 withdrawalAmount = userDeposits[msg.sender].amount;
        require(withdrawalAmount > 0, "No deposited tokens to withdraw");
        
        require(IERC20(tokenAddress).transfer(address(this), withdrawalAmount), "Transfer to contract failed");

        // Reset userDeposits, and startDeposit
        userDeposits[msg.sender].amount = 0;
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

    function getEarnedTOKEN(address user) public view returns (uint256) {
        uint256 timePassed = block.timestamp - userDeposits[user].depositTime;
        uint256 earnedAmount = (userDeposits[msg.sender].amount * timePassed) * 12 / (1000 * 86400); // 1.2% per Day
        return earnedAmount;
    }

    function getTotalIncome(address user) public view returns (uint256) {
    return userTotalIncome[user];
    }

    function getStartDepositTime(address userAddress) public view returns (uint256) {
    return startDeposit[userAddress];
    }

    function getDepositTime(address _user) public view returns (uint256) {
        return userDeposits[_user].depositTime;
    }
}