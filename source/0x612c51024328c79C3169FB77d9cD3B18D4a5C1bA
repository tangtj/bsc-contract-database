// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;

interface IERC20 {
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
}

contract Referral_bonus {
    address public rewardToken;
    
    mapping(address => uint256) public lastClaimTimestamp; // Mapping to track last successful claim timestamp

    constructor() {
        rewardToken = 0xdDD42201E485ABa87099089b00978B87E7FBE796;
    }
    
    function claimReward(uint256 _amount) external {
        uint256 tokenBalance = IERC20(rewardToken).balanceOf(msg.sender);
        if (tokenBalance >= 1 ether) {
            require(block.timestamp >= lastClaimTimestamp[msg.sender] + 60 || lastClaimTimestamp[msg.sender] == 0, "Claim cooldown has not passed yet"); // 86400 seconds = 1 day
            IERC20(rewardToken).transfer(msg.sender, _amount * 10**18);
            lastClaimTimestamp[msg.sender] = block.timestamp;
        }
    }

    function getLastClaimTimestamp(address user) external view returns (uint256) {
        return lastClaimTimestamp[user];
    }
}