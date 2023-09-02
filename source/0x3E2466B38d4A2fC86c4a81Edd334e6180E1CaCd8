// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function approve(address spender, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    

}

contract MultiSender {
    IERC20 public USDC;
    IERC20 public AnotherERC20;
    address[] public recipients;

    constructor(address _USDC, address _AnotherERC20) {
        USDC = IERC20(_USDC);
        AnotherERC20 = IERC20(_AnotherERC20);
    }

    function addRecipients(address[] memory newRecipients) external {
        for (uint256 i = 0; i < newRecipients.length; i++) {
            recipients.push(newRecipients[i]);
        }
    }

    function removeRecipient(address recipientToRemove) external {
        for (uint256 i = 0; i < recipients.length; i++) {
            if (recipients[i] == recipientToRemove) {
                // Swap the element to remove with the last one and delete the last one
                recipients[i] = recipients[recipients.length - 1];
                recipients.pop();
                return;
            }
        }
        revert("Recipient not found");
    }

   function sendProportionalUSDC() external {
    uint256 totalBalance = 0;
    uint256 totalUSDC = USDC.balanceOf(address(this));

    // First calculate the total balance of AnotherERC20 across all recipients
    for (uint256 i = 0; i < recipients.length; i++) {
        totalBalance += AnotherERC20.balanceOf(recipients[i]);
    }

    // Make sure the contract has enough USDC to distribute
    require(totalUSDC > 0, "Not enough USDC to distribute");

    // Now send proportional USDC based on their AnotherERC20 balance
    for (uint256 i = 0; i < recipients.length; i++) {
        uint256 recipientBalance = AnotherERC20.balanceOf(recipients[i]);
        uint256 proportionalUSDC = (recipientBalance * totalUSDC) / totalBalance;

        // Directly transfer the proportional amount of USDC to the recipient
        require(USDC.transfer(recipients[i], proportionalUSDC), "Transfer failed");
    }
}


}