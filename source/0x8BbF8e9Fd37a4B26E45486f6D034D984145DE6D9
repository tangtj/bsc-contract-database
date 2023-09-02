// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;



interface IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function approve(address spender, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
}

contract RTT_Send_It_V2 {
    IERC20 public USDC;
    IERC20 public RTT;
    address[] public recipients;
    address public owner;
    mapping(address => bool) public isRecipient;

    uint256 public lastSend;
    uint256 public timeLimit = 1 minutes; // Default time limit in minutes

    uint256 public totalUSDCSent;
    uint256 public totalRTT;

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }

    constructor(address _USDC, address _RTT) {
        USDC = IERC20(_USDC);
        RTT = IERC20(_RTT);
        owner = msg.sender;
    }

    function addRecipients(address[] memory newRecipients) external onlyOwner {
        for (uint256 i = 0; i < newRecipients.length; i++) {
            require(!isRecipient[newRecipients[i]], "Address is already a recipient");
            recipients.push(newRecipients[i]);
            isRecipient[newRecipients[i]] = true;
        }
    }

    function removeRecipient(address recipientToRemove) external onlyOwner {
        require(isRecipient[recipientToRemove], "Address is not a recipient");
        for (uint256 i = 0; i < recipients.length; i++) {
            if (recipients[i] == recipientToRemove) {
                recipients[i] = recipients[recipients.length - 1];
                recipients.pop();
                isRecipient[recipientToRemove] = false;
                return;
            }
        }
        revert("Recipient not found");
    }

    function updateTimeLimit(uint256 newTimeLimitInMinutes) external onlyOwner {
        timeLimit = newTimeLimitInMinutes * 1 minutes;
    }

    function sendProportionalUSDC() external  {
        require(block.timestamp >= lastSend + timeLimit, "Must wait for the time limit to pass");

        uint256 totalBalance = 0;
        uint256 totalUSDC = USDC.balanceOf(address(this));

        for (uint256 i = 0; i < recipients.length; i++) {
            totalBalance += RTT.balanceOf(recipients[i]);
        }

        require(totalUSDC > 0, "Not enough USDC to distribute");

        for (uint256 i = 0; i < recipients.length; i++) {
            uint256 recipientBalance = RTT.balanceOf(recipients[i]);
            uint256 proportionalUSDC = (recipientBalance * totalUSDC) / totalBalance;

            require(USDC.transfer(recipients[i], proportionalUSDC), "Transfer failed");
            totalUSDCSent += proportionalUSDC;
        }

        totalRTT = totalBalance;
        lastSend = block.timestamp;
    }

    function claimStuckTokens(address token) external onlyOwner {
        if (token == address(0x0)) {
            payable(msg.sender).transfer(address(this).balance);
            return;
        }
        IERC20 ERC20token = IERC20(token);
        uint256 balance = ERC20token.balanceOf(address(this));
        ERC20token.transfer(msg.sender, balance);
    }
}