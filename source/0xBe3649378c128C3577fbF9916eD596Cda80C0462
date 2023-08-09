// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract farmingXMM {
    address public owner;
    address public tokenAddress = 0xdDD42201E485ABa87099089b00978B87E7FBE796; // Replace with the actual token address

    struct Reinvestment {
        uint256 amount;
        uint256 timestamp;
    }

    mapping(address => uint256) public userDeposits;
    mapping(address => uint256) public userLastDepositTime;
    mapping(address => Reinvestment[]) public userReinvestments;

    event TokensDeposited(address indexed user, uint256 tokenAmount);
    event TokensWithdrawn(address indexed user, uint256 tokenAmount);
    event InterestClaimed(address indexed user, uint256 interestAmount);
    event Reinvested(address indexed user, uint256 reinvestAmount);

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function depositTokens(uint256 _amount) external {
        IERC20 token = IERC20(tokenAddress);
        require(token.transferFrom(msg.sender, address(this), _amount), "Deposit failed");
        userDeposits[msg.sender] += _amount;
        userLastDepositTime[msg.sender] = block.timestamp; // Update last deposit time
        emit TokensDeposited(msg.sender, _amount);
    }

    function withdrawTokens(uint256 _amount) external {
        uint256 accumulatedInterest = getAccumulatedInterest(msg.sender);
        require(accumulatedInterest >= _amount, "Insufficient balance");

        IERC20 token = IERC20(tokenAddress);
        require(token.transfer(msg.sender, _amount), "Withdrawal failed");

        userDeposits[msg.sender] -= accumulatedInterest; // Deduct both principal and interest
        emit TokensWithdrawn(msg.sender, _amount);
    }

    function claimInterestAndReinvest() external {
        uint256 interestAmount = calculateInterest(msg.sender);
        require(interestAmount > 0, "No interest to claim");

        userLastDepositTime[msg.sender] = block.timestamp; // Update last deposit time
        userDeposits[msg.sender] += interestAmount; // Reinvest the interest

        emit InterestClaimed(msg.sender, interestAmount);
        reinvestInterest(interestAmount);
    }

    function reinvestInterest(uint256 _amount) internal {
        userReinvestments[msg.sender].push(Reinvestment({
            amount: _amount,
            timestamp: block.timestamp
        }));
        emit Reinvested(msg.sender, _amount);
    }

    function getTotalDeposits() public view returns (uint256) {
        return IERC20(tokenAddress).balanceOf(address(this));
    }

    function getUserTotalDeposits(address _user) public view returns (uint256) {
        return userDeposits[_user];
    }

    function getUserLastDepositTime(address _user) public view returns (uint256) {
        return userLastDepositTime[_user];
    }

    function getUserReinvestments(address _user) public view returns (Reinvestment[] memory) {
        return userReinvestments[_user];
    }
 
    function calculateInterest(address _user) internal view returns (uint256) {
        uint256 depositTime = userLastDepositTime[_user];
        uint256 elapsedTime = block.timestamp - depositTime;
        uint256 interestRatePerDay = 2; // 2% interest per day
        uint256 interestAmount = (userDeposits[_user] * interestRatePerDay * elapsedTime) / (100 * 86400); // 86400 seconds in a day
        return interestAmount;
    }

    function getAccumulatedInterest(address _user) public view returns (uint256) {
        uint256 interestAmount = calculateInterest(_user);
        return userDeposits[_user] + interestAmount;
    }
}