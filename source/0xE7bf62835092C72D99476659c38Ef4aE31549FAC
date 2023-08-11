/**
 *  ██████╗░███████╗███████╗██╗  ░█████╗░██████╗░███████╗░██████╗
    ██╔══██╗██╔════╝██╔════╝██║  ██╔══██╗██╔══██╗██╔════╝██╔════╝
    ██║░░██║█████╗░░█████╗░░██║  ███████║██████╔╝█████╗░░╚█████╗░
    ██║░░██║██╔══╝░░██╔══╝░░██║  ██╔══██║██╔═══╝░██╔══╝░░░╚═══██╗
    ██████╔╝███████╗██║░░░░░██║  ██║░░██║██║░░░░░███████╗██████╔╝
    ╚═════╝░╚══════╝╚═╝░░░░░╚═╝  ╚═╝░░╚═╝╚═╝░░░░░╚══════╝╚═════╝░

    ██████╗░░█████╗░██╗░░░██╗██╗░░░░░███████╗████████╗████████╗███████╗
    ██╔══██╗██╔══██╗██║░░░██║██║░░░░░██╔════╝╚══██╔══╝╚══██╔══╝██╔════╝
    ██████╔╝██║░░██║██║░░░██║██║░░░░░█████╗░░░░░██║░░░░░░██║░░░█████╗░░
    ██╔══██╗██║░░██║██║░░░██║██║░░░░░██╔══╝░░░░░██║░░░░░░██║░░░██╔══╝░░
    ██║░░██║╚█████╔╝╚██████╔╝███████╗███████╗░░░██║░░░░░░██║░░░███████╗
    ╚═╝░░╚═╝░╚════╝░░╚═════╝░╚══════╝╚══════╝░░░╚═╝░░░░░░╚═╝░░░╚══════╝
 */

/**
 * SPDX-License-Identifier: MIT
 */ 
pragma solidity ^0.8.0;

contract defiapesroulette {
    address payable public owner;
    address payable public firstTaxRecipient = payable(0xbb84634CCD07bFbA76D7B8040A49826c45B3601D); // First hardcoded tax recipient wallet
    address payable public secondTaxRecipient = payable(0xCd7d45DB2c8b208bCD9d933d70fdD74ECf96DC1a); // Second hardcoded tax recipient wallet
    uint256 public taxPercentage = 10;
    bool public gameStarted = false;
    uint256 public entryLimit;
    uint256 private prizeTransferred;

    event Received(address sender, uint256 amount);
    event BNBTransferred(address indexed recipient, uint256 amount);
    event PrizeTransferred(address indexed recipient, uint256 amount);
    event GameStarted();
    event GameEnded();
    event EntryLimitSet(uint256 limit);
    event TaxPercentageSet(uint256 percentage);

    constructor() {
        owner = payable(msg.sender);
    }

    receive() external payable onlyGameStarted {
        require(msg.value == entryLimit, "Amount does not match the entry limit");
        emit Received(msg.sender, msg.value);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    modifier onlyGameStarted() {
        require(gameStarted, "Game has not started yet");
        _;
    }

    function startGame() external onlyOwner {
        require(!gameStarted, "Game is already in progress");
        gameStarted = true;
        emit GameStarted();
    }

    function endGame() external onlyOwner {
        require(gameStarted, "Game has not started yet");
        gameStarted = false;
        emit GameEnded();
    }

    function transferBNB(address payable recipient, uint256 amount) external onlyOwner onlyGameStarted {
        require(address(this).balance >= amount, "Insufficient BNB balance in the contract");

        (bool success, ) = recipient.call{value: amount, gas: gasleft()}("");
        require(success, "BNB transfer failed");

        emit BNBTransferred(recipient, amount);
    }

    function transferPrize(address payable recipient) external onlyOwner {
        require(address(this).balance > 0, "No BNB balance in the contract");

        uint256 contractBalance = address(this).balance;
        uint256 taxAmount = (contractBalance * taxPercentage) / 100;
        prizeTransferred = contractBalance - taxAmount;

        uint256 halfTax = taxAmount / 2;

        (bool success1, ) = recipient.call{value: prizeTransferred, gas: gasleft()}("");
        require(success1, "Prize transfer failed");

        (bool success2, ) = firstTaxRecipient.call{value: halfTax, gas: gasleft()}("");
        require(success2, "First tax transfer failed");

        (bool success3, ) = secondTaxRecipient.call{value: halfTax, gas: gasleft()}("");
        require(success3, "Second tax transfer failed");

        emit PrizeTransferred(recipient, prizeTransferred);
    }

    function getContractBalance() external view returns (uint256) {
        return address(this).balance;
    }

    function getEntryLimit() external view returns (uint256) {
        return entryLimit;
    }

    function getPrizeTransferred() external view returns (uint256) {
        return prizeTransferred;
    }

    function setEntryLimit(uint256 limit) external onlyOwner {
        entryLimit = limit;
        emit EntryLimitSet(limit);
    }

    function setTaxPercentage(uint256 percentage) external onlyOwner {
        require(percentage <= 100, "Tax percentage must be between 0 and 100");
        taxPercentage = percentage;
        emit TaxPercentageSet(percentage);
    }
    
    function setFirstTaxRecipient(address payable recipient) external onlyOwner {
        require(recipient != address(0), "Invalid recipient address");
        firstTaxRecipient = recipient;
    }
    
    function setSecondTaxRecipient(address payable recipient) external onlyOwner {
        require(recipient != address(0), "Invalid recipient address");
        secondTaxRecipient = recipient;
    }
}