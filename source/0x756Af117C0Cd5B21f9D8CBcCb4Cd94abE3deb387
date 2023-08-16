// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface ERC20 {
    function transfer(address to, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract MUSDICO {
    address public owner;
    ERC20 public token;

    uint256 public currentPrice = 23; // Price of tokens per BNB
    bool public presaleActive = true; 

    uint256 public totalSold = 0;
    uint256 public totalFund = 0 ether;

    constructor(address _tokenAddress) {
        owner = msg.sender;
        token = ERC20(_tokenAddress);

    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    // Allow the contract to receive Ether and convert it to tokens.
    receive() external payable {
        buyToken();
    }

    // Critical Recommendation: Implement input validation to ensure safe processing.
    function buyToken() public payable {

        // Critical Recommendation: Ensure that presale is active before allowing token purchase.
        require(presaleActive, "Presale is not active!");

        // Calculate the amount of tokens based on the sent Ether and current price.
        uint256 _amount = msg.value * currentPrice;

        // Critical Recommendation: Ensure that the contract has enough tokens to fulfill the purchase.
        require(_amount <= token.balanceOf(address(this)), "ICO Sold Out");

        // Transfer tokens to the buyer.
        bool success = token.transfer(msg.sender, _amount);
        
        // Critical Recommendation: Check if token transfer was successful.
        require(success, "Failed to transfer token!");
        
        // Log the tokens purchased event.
        emit TokensPurchased(msg.sender, _amount);
    }

    // Allow the owner to update the token price.
    function updatePrice(uint256 newPrice) public onlyOwner {
        currentPrice = newPrice;
    }

    // Allow the owner to withdraw accumulated Ether from the contract.
    function withdrawBNB() public onlyOwner {
        address payable ownerAddress = payable(owner);
        ownerAddress.transfer(address(this).balance);
    }

    function withdrawMUSD() public onlyOwner {

        token.transfer(owner, token.balanceOf(address(this)));
    }



    // Important Recommendation: Log events for transparency and tracking.
    event TokensPurchased(address indexed buyer, uint256 amount);

}