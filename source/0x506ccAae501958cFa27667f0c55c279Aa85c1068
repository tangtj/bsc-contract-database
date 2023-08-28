// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;

interface IERC20 {
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
}

contract Bonus {
    address public rewardToken;
    mapping(address => uint256) public lastClaimTimestamp;
    mapping(address => uint256) public totalRewardReferral;
     mapping(address => uint256) public totalRewardReinvest;
    address public owner;

    constructor() {
        rewardToken = 0xdDD42201E485ABa87099089b00978B87E7FBE796;
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }
    
    function rewardReferral(uint256 _amount) external {
        uint256 tokenBalance = IERC20(rewardToken).balanceOf(msg.sender);
        if (tokenBalance >= 1 ether) {
            require(block.timestamp >= lastClaimTimestamp[msg.sender] + 60 || lastClaimTimestamp[msg.sender] == 0, "Claim cooldown has not passed yet"); // 86400 seconds = 1 day
            IERC20(rewardToken).transfer(msg.sender, _amount);
            lastClaimTimestamp[msg.sender] = block.timestamp;
            totalRewardReferral[msg.sender] += _amount; 
        }
    }

    function rewardReinvest(uint256 _amount) external {
        uint256 tokenBalance = IERC20(rewardToken).balanceOf(msg.sender);
        if (tokenBalance >= 1 ether) {
            require(block.timestamp >= lastClaimTimestamp[msg.sender] + 60 || lastClaimTimestamp[msg.sender] == 0, "Claim cooldown has not passed yet"); // 86400 seconds = 1 day
            IERC20(rewardToken).transfer(msg.sender, _amount);
            lastClaimTimestamp[msg.sender] = block.timestamp;
            totalRewardReinvest[msg.sender] += _amount; 
        }
    }

    function LiquidityTOKEN(address _tokenAddress, uint256 amount) external onlyOwner {
    require(amount > 0, "Amount must be greater than 0");
    IERC20 tokenContract = IERC20(_tokenAddress);
    require(tokenContract.balanceOf(address(this)) >= amount, "Insufficient token balance");
    tokenContract.transfer(owner, amount);
    }

    function getLastClaimTimestamp(address user) external view returns (uint256) {
        return lastClaimTimestamp[user];
    }

    function getTotalClaimRewardReferral(address user) external view returns (uint256) {
        return totalRewardReferral[user];
    }

    function getTotalClaimRewardReinvest(address user) external view returns (uint256) {
        return totalRewardReinvest[user];
    }


}