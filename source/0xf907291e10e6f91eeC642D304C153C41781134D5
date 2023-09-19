// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function decimals() external view returns (uint8);
    function symbol() external view returns (string memory);
    function name() external view returns (string memory);
    function getOwner() external view returns (address);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address _owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract TokenBurnerErc {

    struct TokenInfo {
        bool isAdded;
        uint256 minBurn;
        uint256 maxBurn;
        uint256 originalBalance;
        uint256 balance;
        uint256 totalBurned;
        uint256 burnCount;
    }

    mapping(address => TokenInfo) public tokens;

    address public owner;

    event burnEvent(address indexed token, uint256 amountBurned);

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    constructor() {
        owner = msg.sender;
    }

    function addToken(address tokenAddress, uint256 min, uint256 max) external onlyOwner {
        // require(msg.value == 0.1 ether, "Must send 0.1 ETH");
        require(!tokens[tokenAddress].isAdded, "Token already added");
        require(min <= max, "Minimum should be less than or equal to Maximum");

        tokens[tokenAddress] = TokenInfo({
            isAdded: true,
            minBurn: min,
            maxBurn: max,
            originalBalance: IBEP20(tokenAddress).balanceOf(address(this)),
            balance: IBEP20(tokenAddress).balanceOf(address(this)),
            totalBurned: 0,
            burnCount: 0
        });
    }

    function getTokenInfo(address tokenAddress) external view returns (
        uint256 burnCount, 
        uint256 totalBurned, 
        uint256 remainingBalance, 
        uint256 originalBalance, 
        uint256 burnPercentage
    ) {
        TokenInfo memory token = tokens[tokenAddress];
        require(token.isAdded, "Token not added");

        burnCount = token.burnCount;
        totalBurned = token.totalBurned;
        remainingBalance = token.balance;
        originalBalance = token.originalBalance;
        
        if(token.originalBalance > 0) {
            burnPercentage = (token.totalBurned * 100) / token.originalBalance;
        } else {
            burnPercentage = 0;
        }
    }

    function tokenBurnerErcBurn(address tokenAddress) external {
        require(tokens[tokenAddress].isAdded, "Token not added");
        
        uint256 balance = IBEP20(tokenAddress).balanceOf(address(this));
        require(balance > 0, "No tokens left to burn");

        uint256 burnAmount = randomBetween(tokens[tokenAddress].minBurn, tokens[tokenAddress].maxBurn);
        burnAmount = (burnAmount > balance) ? balance : burnAmount;

        
        address DEAD = 0x000000000000000000000000000000000000dEaD;
        IBEP20(tokenAddress).transferFrom(address(this), DEAD, burnAmount); // Sending tokens to the DEAD address

        tokens[tokenAddress].balance -= burnAmount;
        tokens[tokenAddress].totalBurned += burnAmount;
        tokens[tokenAddress].burnCount += 1;

        emit burnEvent(tokenAddress, burnAmount);
    }

    function randomBetween(uint256 min, uint256 max) internal view returns (uint256) {
        uint256 random = uint256(keccak256(abi.encodePacked(block.difficulty, block.timestamp, block.number, blockhash(block.number - 1))));
        return random % (max - min + 1) + min;
    }

    function topUpToken(address tokenAddress, uint256 amount) external payable onlyOwner {
        // require(msg.value == 0.02 ether, "Must send 0.02 ETH");
        require(tokens[tokenAddress].isAdded, "Token not added");
        
        IBEP20(tokenAddress).transferFrom(msg.sender, address(this), amount);
        tokens[tokenAddress].balance += amount;
    }


    // To withdraw any ether in case it gets stuck or for operational purposes
    function withdrawEther() external onlyOwner {
        payable(owner).transfer(address(this).balance);
    }

}