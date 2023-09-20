// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ReferralSystem {
    address public admin1;
    address public admin2;
    address public owner;  // New owner variable
    mapping(address => uint256) public uplinerRewards;
    mapping(address => uint256) public secondUplinerRewards;
    mapping(address => uint256) public thirdUplinerRewards;
    mapping(address => uint256) public fourthUplinerRewards;
    mapping(address => uint256) public fifthUplinerRewards;
    mapping(address => uint256) public sixthUplinerRewards;
    mapping(address => uint256) public seventhUplinerRewards;
    mapping(address => uint256) public balances;

    struct Investment {
        uint256 amount;
        uint256 startDate;
    }

    mapping(address => Investment) public investments;

    constructor(address _admin1, address _admin2) {
        admin1 = _admin1;
        admin2 = _admin2;
        owner = msg.sender; // Set the contract deployer as the initial owner
    }

    // Modifier to restrict access to only the contract owner
    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function");
        _;
    }

    // Function to transfer ownership to a new address
    function transferOwnership(address _newOwner) external onlyOwner {
        owner = _newOwner;
    }

    // Function to allow admins to withdraw available funds
    function withdrawAdminFunds(address _adminAddress) external onlyOwner {
        uint256 adminBalance = balances[_adminAddress];
        require(adminBalance > 0, "Admin has no available balance to withdraw");
        balances[_adminAddress] = 0;
        payable(_adminAddress).transfer(adminBalance);
    }

    function invest(address referrer) external payable {
        // Rest of your existing invest function...

        // ... (upliner rewards, investments tracking, etc.)
    }

    function withdraw() external {
        // Rest of your existing withdraw function...

        // ... (withdrawal logic based on investmentAmount)
    }

    function getContractBalance() external view returns (uint256) {
        return address(this).balance;
    }
}