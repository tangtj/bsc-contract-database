// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address to, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function approve(address spender, uint256 value) external returns (bool);
}

contract BnbToSkfExchange {
    IERC20 public skfToken;
    address payable public owner = payable(0xaa22c163Ed32daF0e3d5d568EdF24E5b4bc4dE4C);
    uint256 public price = 2 * 10**15; // 0.002 BNB in Wei

    struct Purchase {
        address buyer;
        uint256 bnbAmount;
        uint256 skfAmount;
        uint256 timestamp;
    }

    Purchase[] public purchases;

    constructor(address _skfToken) {
        skfToken = IERC20(_skfToken);
        owner = payable(msg.sender);

        // Approve the contract to spend tokens from the owner's balance
        skfToken.approve(address(this), type(uint256).max);
    }

    receive() external payable {
        buyTokens();
    }

    function buyTokens() public payable {
        require(msg.value > 0, "Invalid amount");

        uint256 skfAmount = (msg.value * 10**6) / price; // assuming SKF has 18 decimals
        require(skfToken.balanceOf(address(this)) >= skfAmount, "Insufficient SKF available");

        // Transfer SKF tokens from the contract to the buyer
        skfToken.transfer(msg.sender, skfAmount);

        purchases.push(Purchase(msg.sender, msg.value, skfAmount, block.timestamp));
    }


    function getPurchasesCount() external view returns (uint256) {
        return purchases.length;
    }

    function getPurchase(uint256 index) external view returns (address, uint256, uint256, uint256) {
        require(index < purchases.length, "Invalid index");
        Purchase memory purchase = purchases[index];
        return (purchase.buyer, purchase.bnbAmount, purchase.skfAmount, purchase.timestamp);
    }
    function withdrawBNB() external {
    require(msg.sender == owner, "Only owner can call this function");
    uint256 balance = address(this).balance;
    require(balance > 0, "No BNB to withdraw");
    owner.transfer(balance);
}
struct BnbTransaction {
    address sender;
    uint256 amount;
    uint256 timestamp;
}

mapping(address => BnbTransaction[]) public bnbTransactions;

function getAllBnbTransactions() external view returns (BnbTransaction[][] memory) {
    BnbTransaction[][] memory allTransactions = new BnbTransaction[][](purchases.length);

    for (uint256 i = 0; i < purchases.length; i++) {
        allTransactions[i] = bnbTransactions[purchases[i].buyer];
    }

    return allTransactions;
}
function withdrawSKF(uint256 amount) external {
        require(msg.sender == owner, "Only owner can call this function");
        require(skfToken.balanceOf(address(this)) >= amount, "Insufficient SKF available");
        skfToken.transfer(owner, amount);
    }

}