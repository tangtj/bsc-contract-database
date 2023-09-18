// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ReferralSystem {
    address public admin1;
    address public admin2;
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
    }

    function invest(address referrer) external payable {
        require(msg.value >= 50 ether && msg.value <= 10000 ether, "Investment amount must be between $50 and $10,000");

        uint256 admin1Reward = (msg.value * 15) / 100;
        uint256 admin2Reward = (msg.value * 15) / 100;

        uint256 uplinerReward;
        uint256 secondUplinerReward = (msg.value * 4) / 100;
        uint256 thirdUplinerReward = (msg.value * 2) / 100;
        uint256 fourthUplinerReward = (msg.value * 1) / 100;
        uint256 fifthUplinerReward = (msg.value * 1) / 100;
        uint256 sixthUplinerReward = (msg.value * 1) / 100;
        uint256 seventhUplinerReward = (msg.value * 1) / 100;

        if (msg.value >= 1000 ether) {
            // For investments of $1000 or more, 2% daily return.
            uplinerReward = (msg.value * 20) / 1000;
        } else if (msg.value >= 500 ether) {
            // For investments between $500 and $999, 1.5% daily return.
            uplinerReward = (msg.value * 15) / 1000;
        } else {
            // For investments between $50 and $499, 1% daily return.
            uplinerReward = (msg.value * 10) / 1000;
        }

        uint256 investmentAmount = msg.value;

        balances[msg.sender] += investmentAmount;
        uplinerRewards[msg.sender] += uplinerReward;

        investments[msg.sender] = Investment({
            amount: msg.value,
            startDate: block.timestamp
        });

        // Check if the referrer is valid and not the same as the investor.
        if (referrer != msg.sender && referrer != address(0)) {
            uplinerRewards[referrer] += secondUplinerReward;
            secondUplinerRewards[referrer] += secondUplinerReward;

            // Check if there is a 3rd upliner.
            address thirdUpliner = uplinerRewards[referrer] > 0 ? referrer : address(0);

            // If there is a 3rd upliner, reward them.
            if (thirdUpliner != address(0)) {
                uplinerRewards[thirdUpliner] += thirdUplinerReward;
                thirdUplinerRewards[thirdUpliner] += thirdUplinerReward;

                // Check if there is a 4th upliner.
                address fourthUpliner = uplinerRewards[thirdUpliner] > 0 ? thirdUpliner : address(0);

                // If there is a 4th upliner, reward them.
                if (fourthUpliner != address(0)) {
                    uplinerRewards[fourthUpliner] += fourthUplinerReward;
                    fourthUplinerRewards[fourthUpliner] += fourthUplinerReward;

                    // Check if there is a 5th upliner.
                    address fifthUpliner = uplinerRewards[fourthUpliner] > 0 ? fourthUpliner : address(0);

                    // If there is a 5th upliner, reward them.
                    if (fifthUpliner != address(0)) {
                        uplinerRewards[fifthUpliner] += fifthUplinerReward;
                        fifthUplinerRewards[fifthUpliner] += fifthUplinerReward;

                        // Check if there is a 6th upliner.
                        address sixthUpliner = uplinerRewards[fifthUpliner] > 0 ? fifthUpliner : address(0);

                        // If there is a 6th upliner, reward them.
                        if (sixthUpliner != address(0)) {
                            uplinerRewards[sixthUpliner] += sixthUplinerReward;
                            sixthUplinerRewards[sixthUpliner] += sixthUplinerReward;

                            // Check if there is a 7th upliner.
                            address seventhUpliner = uplinerRewards[sixthUpliner] > 0 ? sixthUpliner : address(0);

                            // If there is a 7th upliner, reward them.
                            if (seventhUpliner != address(0)) {
                                uplinerRewards[seventhUpliner] += seventhUplinerReward;
                                seventhUplinerRewards[seventhUpliner] += seventhUplinerReward;
                            }
                        }
                    }
                }
            }
        }

        balances[admin1] += admin1Reward;
        balances[admin2] += admin2Reward;
    }

    function withdraw() external {
        require(balances[msg.sender] > 0, "No balance to withdraw");
        
        uint256 investmentAmount = investments[msg.sender].amount;
        uint256 startDate = investments[msg.sender].startDate;

        require(block.timestamp >= startDate + 1 days, "You can withdraw after 1 day");
        require(block.timestamp <= startDate + 300 days, "Withdrawal period expired");

        uint256 withdrawAmount;
        
        if (investmentAmount >= 1000 ether) {
            withdrawAmount = (investmentAmount * 20) / 1000;
        } else if (investmentAmount >= 500 ether) {
            withdrawAmount = (investmentAmount * 15) / 1000;
        } else {
            withdrawAmount = (investmentAmount * 10) / 1000;
        }

        require(withdrawAmount > 0, "Insufficient balance for withdrawal");

        balances[msg.sender] -= withdrawAmount;
        payable(msg.sender).transfer(withdrawAmount);
    }

    function getContractBalance() external view returns (uint256) {
        return address(this).balance;
    }
}