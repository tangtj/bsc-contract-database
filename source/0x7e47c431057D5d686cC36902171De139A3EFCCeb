// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address to, uint256 amount) external returns (bool);
}

contract ClaimVGT {
    address public owner;
    address public tokenAddress;
    uint256 public claimAmount = 5 * 10**18; 

    mapping(address => bool) public hasClaimed;
    bool private locked; // Mutex to prevent reentrancy

    event TokensClaimed(address indexed user, uint256 amount);   

    constructor(address _tokenAddress) {
        owner = msg.sender;
        tokenAddress = _tokenAddress;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    modifier noReentrancy() {
        require(!locked, "Reentrant call");
        locked = true;
        _;
        locked = false;
    }

    function setClaimAmount(uint256 _amount) external onlyOwner {
        claimAmount = _amount;
    }

    function claimTokens() external noReentrancy {
        require(!hasClaimed[msg.sender], "You have already claimed tokens");

        IERC20 token = IERC20(tokenAddress);
        require(token.transfer(msg.sender, claimAmount), "Token transfer failed");

        hasClaimed[msg.sender] = true;
        emit TokensClaimed(msg.sender, claimAmount);
    }

    function withdrawTokens(address _to, uint256 _amount) external onlyOwner {
        IERC20 token = IERC20(tokenAddress);
        require(token.transfer(_to, _amount), "Token transfer failed");
    }
}