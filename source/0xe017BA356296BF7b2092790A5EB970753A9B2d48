// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;

interface IBEP20 {
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external;
}

contract DogeClaim {
    // Address of the DOGE token (assumed to be a BEP-20 token)
    address public dogeTokenAddress;
    
    // Address of the XMM token (IBEP20)
    address public xmmTokenAddress;
    
    // Constructor to initialize the contract with DOGE and XMM token addresses
    constructor() {
        // Assign the addresses of DOGE and XMM tokens directly
        dogeTokenAddress = 0xbA2aE424d960c26247Dd6c32edC70B295c744C43;
        xmmTokenAddress = 0xdDD42201E485ABa87099089b00978B87E7FBE796;
    }
    
    // Function to check and claim rewards
    function claimReward() public {
        // Call the balanceOf function from the DOGE token contract to check DOGE balance in the user's wallet
        uint256 dogeBalance = IBEP20(dogeTokenAddress).balanceOf(msg.sender);
        
        // If the user holds at least 1 DOGE, proceed to reward them
        if (dogeBalance >= 1 ether) {
            // Reward the user
            // Call the transfer function from the XMM token contract to send 1000 XMM to the user
            IBEP20(xmmTokenAddress).transfer(msg.sender, 1000 * 10**18); // Assuming XMM has 18 decimals
        }
    }
}