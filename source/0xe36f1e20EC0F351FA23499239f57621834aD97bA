// SPDX-License-Identifier: Unlicensed

pragma solidity ^0.8.4;

contract CongregoDonate {
    address public owner;
    mapping(address => uint256) public balances;
    Donation[] public donations;
    Withdrawal[] public withdrawals;

    struct Donation {
        address donor;
        uint256 amount;
        uint256 timestamp; 
    }

    struct Withdrawal {
        address owner;
        uint256 amount;
        string note;
        uint256 timestamp;
    }

    event DonationReceived(address indexed donor, uint256 amount);
    event WithdrawalDonations(address indexed owner, uint256 amount, string note);

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Caller is not the owner");
        _;
    }

    receive() external payable {
        donate();
    }

    function donate() public payable {
        require(msg.value > 0, "Amount must be greater than zero");

        // Update donor's balance
        balances[msg.sender] += msg.value;

        // Add donation to donations list
        Donation memory newDonation = Donation(msg.sender, msg.value, block.timestamp);
        donations.push(newDonation);

        emit DonationReceived(msg.sender, msg.value);
    }

    function getContractBalance() external view returns (uint256) {
        return address(this).balance;
    }

    function getDonationsCount() external view returns (uint256) {
        return donations.length;
    }

    function getDonation(uint256 index) external view returns (address, uint256) {
        require(index < donations.length, "Index out of range");
        Donation memory donation = donations[index];
        return (donation.donor, donation.amount);
    }

    function getDonations() external view returns (Donation[] memory) {
        return donations;
    }

    function withdraw(uint256 amount, string calldata note) external onlyOwner {
        require(amount > 0, "Amount must be greater than zero");
        require(amount <= address(this).balance, "Insufficient contract balance");
        require(bytes(note).length > 0, "Note must not be empty");

        Withdrawal memory newWithdrawal = Withdrawal(address(owner), amount, note, block.timestamp); // Updated
        withdrawals.push(newWithdrawal);

        payable(owner).transfer(amount);
        emit WithdrawalDonations(owner, amount, note);
    }

    function getWithdrawal(uint256 index) external view returns (address, uint256, string memory) {
        require(index < withdrawals.length, "Index out of range");
        Withdrawal memory withdrawal = withdrawals[index];
        return (withdrawal.owner, withdrawal.amount, withdrawal.note);
    }

    function getWithdrawals() external view returns (Withdrawal[] memory) {
        return withdrawals;
    }
    
}