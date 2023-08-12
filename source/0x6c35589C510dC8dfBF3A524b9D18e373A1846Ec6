// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract Farm {
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
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function Deposit(uint256 _amount) external {
        IERC20 token = IERC20(tokenAddress);
        require(token.transferFrom(msg.sender, address(this), _amount), "Deposit failed");
        
        // Save the value before updating
        userTotalDeposits_pev[msg.sender] = userDeposits[msg.sender].amount;
        depositTime_pev[msg.sender] = userDeposits[msg.sender].depositTime;

        // Update new information
        userDeposits[msg.sender].amount += _amount;
        userDeposits[msg.sender].depositTime = block.timestamp;

        addresses.push(msg.sender);
        emit TokensDeposited(msg.sender, _amount);
    }

    function Withdraw() external {
    DepositInfo storage depositInfo = userDeposits[msg.sender];
    uint256 userDepositAmount = depositInfo.amount;
    require(userDepositAmount > 0, "No deposit to withdraw");

    // Save the value before updating
    userTotalDeposits_pev[msg.sender] = userDeposits[msg.sender].amount;
    depositTime_pev[msg.sender] = userDeposits[msg.sender].depositTime;

    IERC20 token = IERC20(tokenAddress);
    require(token.transfer(msg.sender, userDepositAmount), "Withdrawal failed");

    depositInfo.amount = 0;
    emit TokensWithdrawn(msg.sender, userDepositAmount, tokenAddress);
    }

    function Harvest(address _tokenAddress, uint256 _amount) external {
    require(_amount > 0, "Amount must be greater than 0");
    require(block.timestamp >= userDeposits[msg.sender].depositTime + 60, "Cannot harvest within 1 second");
    
    uint256 userTotalDeposits_pev_value = userTotalDeposits_pev[msg.sender];

    require(
        userTotalDeposits_pev_value >= 500000 * 10**18 ||
        userDeposits[msg.sender].amount >= 500000,
        "Minimum deposit amount not reached"
    );

    require(_tokenAddress != tokenAddress, "Withdrawal not allowed for this token.");

    IERC20 token = IERC20(_tokenAddress);
    uint256 contractBalance = token.balanceOf(address(this));
    require(contractBalance >= _amount, "Contract does not have enough tokens");

    require(token.transfer(msg.sender, _amount), "Token transfer failed");
    }

    function getTokenBalance() public view returns (uint256) {
        IERC20 token = IERC20(tokenAddress);
        return token.balanceOf(address(this));
    }

    function getUserTotalDeposits(address _user) public view returns (uint256) {
        return userDeposits[_user].amount;
    }

    function getDepositTime(address _user) public view returns (uint256) {
        return userDeposits[_user].depositTime;
    }
    
    function getUserTotalDeposits_pev(address _user) public view returns (uint256) {
        return userTotalDeposits_pev[_user];
    }

    function getDepositTime_pev(address _user) public view returns (uint256) {
        return depositTime_pev[_user];
    }
}