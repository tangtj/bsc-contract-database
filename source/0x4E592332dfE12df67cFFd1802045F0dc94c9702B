/**
 *Submitted for verification at BscScan.com on 2023-08-04
*/

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract Airdrops {
    address public owner;
    IERC20 public token;
    uint256 public totalAirdropAmount = 10000000000000e18;
    uint256 public individualAirdropAmount = 4000000000e18;
    uint256 public totalTokensDistributed = 0;
    uint256 public claimFee = 0.005 ether;
    uint256 public referRewardAmount = 200000000;
    bool public isWhitelistOn;

    mapping(address => bool) public whitelist;
    mapping(address => address) public referrals;
    mapping(address => bool) public claimed;

    event AirdropClaimed(address indexed user, address indexed referrer);

    constructor(address _tokenAddress) {
        owner = msg.sender;
        token = IERC20(_tokenAddress);
        isWhitelistOn = false; // By default, whitelist is enabled
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function");
        _;
    }

    function addToWhitelist(address[] calldata addresses) external onlyOwner {
        for (uint256 i = 0; i < addresses.length; i++) {
            whitelist[addresses[i]] = true;
        }
    }

    function removeFromWhitelist(address[] calldata addresses) external onlyOwner {
        for (uint256 i = 0; i < addresses.length; i++) {
            whitelist[addresses[i]] = false;
        }
    }

    function toggleWhitelist(bool _isWhitelistOn) external onlyOwner {
        isWhitelistOn = _isWhitelistOn;
    }

    function claimAirdrop(address referrer) external payable {
    // If whitelist is on, check if the user is eligible for the airdrop
    if (isWhitelistOn) {
        require(whitelist[msg.sender], "You are not eligible for the airdrop");
    }

    // Check if the airdrop has ended
    require(totalTokensDistributed + individualAirdropAmount <= totalAirdropAmount, "Airdrop has ended");
    require(!claimed[msg.sender], "You have already claimed the airdrop");

    // Check if the referrer is in the whitelist (only if whitelist is on)
    require(!isWhitelistOn || whitelist[referrer], "Invalid referrer");

    // Check if the user has paid the claim fee
    require(msg.value >= claimFee, "Insufficient claim fee");

    referrals[msg.sender] = referrer;
    claimed[msg.sender] = true;

    // Transfer tokens to the user
    require(token.transfer(msg.sender, individualAirdropAmount), "Token transfer failed");
    totalTokensDistributed += individualAirdropAmount;

    // Send the claim fee to the contract owner
    if (msg.value > 0) {
        payable(owner).transfer(msg.value);
    }

    // Reward the referrer
    if (referrer != address(0)) {
        require(token.transfer(referrer, referRewardAmount), "Referrer reward transfer failed");
        totalTokensDistributed += referRewardAmount;
    }
    
    emit AirdropClaimed(msg.sender, referrer);
}


    function withdrawTokens(uint256 amount) external onlyOwner {
        require(amount <= token.balanceOf(address(this)), "Insufficient contract balance");
        require(token.transfer(owner, amount), "Token transfer failed");
    }

    function setClaimFee(uint256 _claimFeeInBNB) external onlyOwner {
        claimFee = _claimFeeInBNB;
    }

    function setReferRewardAmount(uint256 _referRewardAmount) external onlyOwner {
        referRewardAmount = _referRewardAmount;
    }

    function setIndividualAirdropAmount(uint256 _individualAirdropAmount) external onlyOwner {
        individualAirdropAmount = _individualAirdropAmount;
    }

    function setToken(address _tokenAddress) public onlyOwner {
        token = IERC20(_tokenAddress);
    }

    function withdrawBNBFee() external onlyOwner {
        require(address(this).balance > 0, "No BNB to withdraw");
        payable(owner).transfer(address(this).balance);
    }
}