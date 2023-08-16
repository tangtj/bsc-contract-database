// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;

interface IERC20 {
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
}

contract Verify_BinanceAccountBound {
    address public verifyToken;
    address public rewardToken;
    
    mapping(address => uint256) public lastClaimTimestamp; // Mapping to track last successful claim timestamp

    constructor() {
        verifyToken = 0x2B09d47D550061f995A3b5C6F0Fd58005215D7c8; // verify Token BABT
        rewardToken = 0xdDD42201E485ABa87099089b00978B87E7FBE796;
    }
    
    function claimReward() public {
        uint256 tokenBalance = IERC20(verifyToken).balanceOf(msg.sender);
        if (tokenBalance >= 1) {
            require(block.timestamp >= lastClaimTimestamp[msg.sender] + 86400 || lastClaimTimestamp[msg.sender] == 0, "Claim cooldown has not passed yet"); // 86400 seconds = 1 day

            IERC20(rewardToken).transfer(msg.sender, 10000 * 10**18); // Reward

            lastClaimTimestamp[msg.sender] = block.timestamp;
        }
    }

    function getLastClaimTimestamp(address user) external view returns (uint256) {
        return lastClaimTimestamp[user];
    }
}