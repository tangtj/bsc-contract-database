// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Ownable {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _owner = msg.sender;
        emit OwnershipTransferred(address(0), msg.sender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(msg.sender == _owner, "Ownable: caller is not the owner");
        _;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract NFTCollector is Ownable {
    uint256 public constant MAX_DEPOSITS = 100;
    uint256 public constant PRICE_INCREMENT_PERCENT = 1;
    uint256 public constant INITIAL_PRICE = 0.01 ether;
    uint256 public constant INITIAL_DEPOSITS = 61; // Hard coded initial deposits

    uint256 public depositCount;
    uint256 public price;
    bool public depositsPaused; // State variable to pause/unpause deposits

    mapping(address => bool) private depositors; // Track addresses that have made deposits
    address[] private depositorsList;

    event Deposit(address indexed account, uint256 indexed depositCount, uint256 price);
    event Withdrawal(address indexed account, uint256 amount);
    event DepositsPaused(bool paused);
    event DepositSuccess(address indexed account, uint256 indexed depositCount, uint256 price);

    modifier canDeposit() {
        require(depositCount < MAX_DEPOSITS, "Maximum number of deposits reached.");
        require(!depositsPaused, "Deposits are paused.");
        _;
    }

    constructor() {
        require(INITIAL_DEPOSITS <= MAX_DEPOSITS, "Initial deposits cannot exceed MAX_DEPOSITS");
        depositCount = 0;
        price = INITIAL_PRICE;
        depositsPaused = false; // Deposits are not paused initially
    }

    function deposit() external payable canDeposit {
        uint256 currentPrice = getCurrentPrice();
        require(msg.value == currentPrice, "Invalid deposit amount. Please send exactly the current price.");

        depositCount++;
        emit Deposit(msg.sender, depositCount, currentPrice);
        emit DepositSuccess(msg.sender, depositCount, currentPrice);

        // Increase the price for the next deposit
        if (depositCount >= INITIAL_DEPOSITS) {
            price = price + (price * PRICE_INCREMENT_PERCENT / 100);
        }

        depositors[msg.sender] = true; // Mark the sender as a depositor
        depositorsList.push(msg.sender); // Add the sender to the depositors list
    }

function getCurrentPrice() public view returns (uint256) {
    if (depositCount >= INITIAL_DEPOSITS) {
        // Calculate the price increment based on the number of deposits made
        uint256 priceIncrement = INITIAL_PRICE * PRICE_INCREMENT_PERCENT / 100;
        // Calculate the current price by adding the price increment to the initial price
        return INITIAL_PRICE + (depositCount - INITIAL_DEPOSITS) * priceIncrement;
    } else {
        // Return the initial price if the number of deposits is less than INITIAL_DEPOSITS
        return INITIAL_PRICE;
    }
}

    function getTotalDeposits() external view returns (uint256) {
        return depositCount;
    }

    function getDepositors() external view returns (address[] memory) {
        return depositorsList;
    }

    function withdrawBalance() external onlyOwner {
        uint256 balance = address(this).balance;
        require(balance > 0, "Contract has no balance to withdraw.");
        payable(owner()).transfer(balance);
        emit Withdrawal(owner(), balance);
    }

    function pauseDeposits() external onlyOwner {
        depositsPaused = true;
        emit DepositsPaused(true);
    }

    function unpauseDeposits() external onlyOwner {
        depositsPaused = false;
        emit DepositsPaused(false);
    }
}