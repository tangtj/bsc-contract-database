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
    uint256 public totalDeposits;
    uint256 public maxDeposits = 49;
    uint256 public currentPrice;
    uint256 public basePrice = 0.01 ether;
    uint256 public depositIncreasePercentage = 1; // 1% increase for each deposit
    bool public paused = false;

    mapping(address => uint256) public deposits;
    address[] public depositors;

    event Deposit(address indexed user, uint256 amount);
    event Withdrawal(address indexed user, uint256 amount);
    event Paused(bool isPaused);

    modifier whenNotPaused() {
        require(!paused, "Contract is paused");
        _;
    }

    constructor() {
        // No need to set 'owner' again, it's already initialized in the Ownable constructor.
        currentPrice = basePrice;
    }

    function getCurrentPrice() public view returns (uint256) {
        return currentPrice;
    }

    function getNumberOfDeposits() public view returns (uint256) {
        return totalDeposits;
    }

    function deposit() public payable whenNotPaused {
        require(totalDeposits < maxDeposits, "No more deposits allowed");
        require(msg.value == currentPrice, "Invalid deposit amount");

        deposits[msg.sender] += 1;
        totalDeposits += 1;
        currentPrice = (currentPrice * 101) / 100; // Increase by 1%

        if (deposits[msg.sender] == 1) {
            depositors.push(msg.sender);
        }

        emit Deposit(msg.sender, msg.value);
    }

    function withdrawBalance() public onlyOwner {
        uint256 balance = address(this).balance;
        require(balance > 0, "Contract balance is empty");
        payable(owner()).transfer(balance);

        emit Withdrawal(owner(), balance);
    }

    function pause() public onlyOwner {
        paused = true;
        emit Paused(true);
    }

    function unpause() public onlyOwner {
        paused = false;
        emit Paused(false);
    }

    function getDepositors() public view returns (address[] memory) {
        return depositors;
    }
}