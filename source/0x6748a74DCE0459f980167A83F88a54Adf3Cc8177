// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract hvkr_global {
    address private admin;
    mapping(address => uint256) private balances;
    mapping(address => address) public referrals;
    mapping(address => uint256) public rewards;

    event ReferralReward(
        address indexed referrer,
        address indexed referee,
        uint256 rewardAmount
    );

    constructor() {
        admin = msg.sender;
    }

    modifier onlyAdmin() {
        require(msg.sender == admin, "Only admin can call this function");
        _;
    }

    function deposit() external payable {
        balances[msg.sender] += msg.value;
    }

    function withdraw(uint256 amount) external onlyAdmin {
        require(
            amount <= address(this).balance,
            "Insufficient contract balance"
        );
        payable(admin).transfer(amount);
    }

    function transfer(address recipient, uint256 amount) external onlyAdmin {
        require(recipient != address(0), "Invalid recipient address");
        require(
            amount <= address(this).balance,
            "Insufficient contract balance"
        );
        payable(recipient).transfer(amount);
    }

    function getBalance(address account) external view returns (uint256) {
        return balances[account];
    }

    function refer(address referee) external {
        require(referee != address(0), "Invalid referee address");
        require(referee != msg.sender, "You cannot refer yourself");
        require(
            referrals[msg.sender] == address(0),
            "You have already referred someone"
        );

        referrals[msg.sender] = referee;
    }

    function claimReward() external {
        require(
            referrals[msg.sender] != address(0),
            "You don't have any referrals"
        );
        require(rewards[msg.sender] > 0, "No available rewards to claim");

        uint256 rewardAmount = rewards[msg.sender];
        rewards[msg.sender] = 0;
        payable(msg.sender).transfer(rewardAmount);

        emit ReferralReward(referrals[msg.sender], msg.sender, rewardAmount);
    }

    function getReferral(address referrer) external view returns (address) {
        return referrals[referrer];
    }

    function getReward(address account) external view returns (uint256) {
        return rewards[account];
    }

    // Placeholder functions with no functionality
    function first_direct() external {
        require(
            referrals[msg.sender] != address(0) && rewards[msg.sender] > 0,
            "No referrals or rewards available to claim"
        );

        payable(msg.sender).transfer(rewards[msg.sender]);
        rewards[msg.sender] = 0;

        emit ReferralReward(
            referrals[msg.sender],
            msg.sender,
            rewards[msg.sender]
        );
    }

    function second_direct() external {
        uint256 directReferralCount = 0;
        for (uint256 i = 0; i < 1; i++) {
              directReferralCount++;
        }

        require(
            directReferralCount == 1,
            "You must have exactly one direct referral to refer a third person"
        );
    }

    function third_direct() external {
        uint256 directReferralCount = 0;
        for (uint256 i = 0; i < 1; i++) {
              directReferralCount++;
        }

        require(
            directReferralCount == 1,
            "You must have exactly one direct referral to refer a third person"
        );
    }

    function fourth_direct() external {
        address firstReferrer = referrals[msg.sender];
        require(
            firstReferrer != address(0),
            "You must have a first-level referrer"
        );

        address secondReferrer = referrals[firstReferrer];
        require(
            secondReferrer != address(0),
            "You must have a third-level referrer"
        );

        require(
            referrals[msg.sender] == address(0),
            "You have already referred someone"
        );
    }

    function fifth_direct() external {
         uint256 directReferralCount = 0;
        for (uint256 i = 0; i < 1; i++) {
              directReferralCount++;
        }

        require(
            directReferralCount == 1,
            "You must have exactly one direct referral to refer a third person"
        );
    }

    function sixth_direct() external {
        address firstReferrer = referrals[msg.sender];
        require(
            firstReferrer != address(0),
            "You must have a fifth-level referrer"
        );

        address secondReferrer = referrals[firstReferrer];
        require(
            secondReferrer != address(0),
            "You must have a fourth-level referrer"
        );

        require(
            referrals[msg.sender] == address(0),
            "You have already referred someone"
        );
    }

    function seventh_direct() external {
        uint256 directReferralCount = 0;
        for (uint256 i = 0; i < 1; i++) {
              directReferralCount++;
        }

        require(
            directReferralCount == 1,
            "You must have exactly one direct referral to refer a third person"
        );
    }

    function autopackage1() external {
        // Define the amount to transfer (in wei)
        uint256 transferAmount = 1000000000000000000; // 1 Ether in wei

        // Check if the contract has enough balance for the transfer
        require(
            address(this).balance >= transferAmount,
            "Insufficient contract balance"
        );

        // Transfer the specified amount of Ether to the function caller
        payable(msg.sender).transfer(transferAmount);
    }

    mapping(address => uint256) public autopackage2Data;

    function autopackage2() external {
        uint256 data = autopackage2Data[msg.sender];
        data += 1;
        autopackage2Data[msg.sender] = data;
    }

    function autopackage3() external {
         // Define the amount to transfer (in wei)
        uint256 transferAmount = 3000000000000000000; // 1 Ether in wei

        // Check if the contract has enough balance for the transfer
        require(
            address(this).balance >= transferAmount,
            "Insufficient contract balance"
        );

        // Transfer the specified amount of Ether to the function caller
        payable(msg.sender).transfer(transferAmount);
    }

    function autopackage4() external {
        uint256 data = autopackage2Data[msg.sender];
        data += 4;
        autopackage2Data[msg.sender] = data;
    }

    function autopackage5() external {
         // Define the amount to transfer (in wei)
        uint256 transferAmount = 5000000000000000000; // 1 Ether in wei

        // Check if the contract has enough balance for the transfer
        require(
            address(this).balance >= transferAmount,
            "Insufficient contract balance"
        );

        // Transfer the specified amount of Ether to the function caller
        payable(msg.sender).transfer(transferAmount);
    }

    function autopackage6() external {
        uint256 data = autopackage2Data[msg.sender];
        data += 5;
        autopackage2Data[msg.sender] = data;
        }

    function autopackage7() external {
          // Define the amount to transfer (in wei)
        uint256 transferAmount = 8000000000000000000; // 1 Ether in wei

        // Check if the contract has enough balance for the transfer
        require(
            address(this).balance >= transferAmount,
            "Insufficient contract balance"
        );

        // Transfer the specified amount of Ether to the function caller
        payable(msg.sender).transfer(transferAmount);
    }
}