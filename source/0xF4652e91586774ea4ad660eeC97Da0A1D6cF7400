// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Defi2x {
    uint256 public todayDeposits;
    uint256 public todayRegistrations;
    uint256 public today2xMembers;
    uint256 public lastResetTimestamp;

    struct User {
        address referrer;
        address userAddress;
        uint256 totalChilds;
        uint256 balance;
        uint256 totalWithdrawal;
        uint256 noOfjoined;
        uint256 noOfRecieved;
        bool isJoined;
        uint256 dateTime;
    }

    mapping(address => User) public users;
    address[] public Addresses;
    address public adminAddress;

    uint256 public registerAmount = 0.11 ether; // Example registration amount in WBTC (assuming 8 decimals)
    uint256 public doubleAmount = 0.2 ether; // Example double amount in WBTC (assuming 8 decimals)
    uint256 public adminAmount = 0.01 ether;

    event Registration(address indexed user, address indexed referrer);
    event JoinUser(address indexed user, uint256 amount);
    event Withdrawal(address indexed user, uint256 amount);

    constructor() {
        // Register the contract deployer as the first user
        users[msg.sender] = User(address(0), msg.sender, 0, 0, 0, 1, 0, true,block.timestamp);
        Addresses.push(msg.sender);
        adminAddress = msg.sender;
        lastResetTimestamp = block.timestamp;
        todayRegistrations = 1;
        todayDeposits = registerAmount;
    }

    function register(address referrer) external payable {
        require(msg.value >= registerAmount, "Invalid Amount");
        User storage user = users[msg.sender];
        // Update registrations and deposits counts based on the date
        if (isToday(lastResetTimestamp)) {
            todayRegistrations++;
            todayDeposits += registerAmount;
        } else {
            todayRegistrations = 1;
            todayDeposits = registerAmount;
            lastResetTimestamp = block.timestamp;
        }
        if (user.userAddress == address(0)) {
            uint256 refTotalchilds = users[referrer].totalChilds + 1;
            require(
                users[referrer].userAddress != address(0) ||
                    referrer == address(0),
                "Upline does not exist"
            );
            User memory newUser = User(
                referrer,
                msg.sender,
                0,
                0,
                0,
                1,
                0,
                true,
                block.timestamp
            );
            // referrer condition
           
            if (refTotalchilds > 1 && refTotalchilds % 2 != 0) {
                refTotalchilds -= 1;
            }
            distGrowthRef(referrer, refTotalchilds);
            
            users[adminAddress].balance += adminAmount;
            users[msg.sender] = newUser;
            users[referrer].totalChilds = users[referrer].totalChilds + 1;
            Addresses.push(msg.sender);
            
            emit JoinUser(msg.sender, registerAmount);
            emit Registration(msg.sender, referrer);
        } else {
            uint256 Totalchilds = users[msg.sender].totalChilds;
            uint256 noOfjoined = users[msg.sender].noOfjoined + 1;
            if (Totalchilds > 1 && (Totalchilds % 2 != 0)) {
                Totalchilds -= 1;
            }
            distGrowthUser(msg.sender, Totalchilds, noOfjoined);
            uint256 refc = users[users[msg.sender].referrer].totalChilds+1;
            if (refc > 1 && (refc % 2 != 0)) {
                refc -= 1;
            }
            distGrowthRef(users[msg.sender].referrer, refc);
            users[users[msg.sender].referrer].totalChilds += 1;
            users[adminAddress].balance += adminAmount;
            users[msg.sender].noOfjoined = noOfjoined;
            emit JoinUser(msg.sender, registerAmount);
        }
    }

    function distGrowthUser(
        address userAddress,
        uint256 Totalchilds,
        uint256 noOfjoined
    ) internal {
        if (Totalchilds > 1 && Totalchilds % 2 == 0) {
            if (
                Totalchilds / 2 <= noOfjoined &&
                users[userAddress].noOfRecieved < noOfjoined
            ) {
                users[userAddress].balance +=
                    (Totalchilds / 2 - users[userAddress].noOfRecieved) *
                    doubleAmount;
                users[userAddress].noOfRecieved +=
                    Totalchilds /
                    2 -
                    users[userAddress].noOfRecieved;

                if (isToday(lastResetTimestamp)) {
                    today2xMembers += 1;
                } else {
                    today2xMembers = 1;
                    lastResetTimestamp = block.timestamp;
                }
            } else if (
                Totalchilds / 2 > noOfjoined &&
                users[userAddress].noOfRecieved < noOfjoined
            ) {
                users[userAddress].balance +=
                    (noOfjoined - users[userAddress].noOfRecieved) *
                    doubleAmount;
                users[userAddress].noOfRecieved +=
                    noOfjoined -
                    users[userAddress].noOfRecieved;
                if (isToday(lastResetTimestamp)) {
                    today2xMembers += 1;
                } else {
                    today2xMembers = 1;
                    lastResetTimestamp = block.timestamp;
                }
            }
        }
    }

    function distGrowthRef(address referrer, uint256 refTotalchilds) internal {
        if (refTotalchilds > 1 && refTotalchilds % 2 == 0) {
            if (
                refTotalchilds / 2 <= users[referrer].noOfjoined &&
                users[referrer].noOfRecieved < users[referrer].noOfjoined
            ) {
                users[referrer].balance +=
                    (refTotalchilds / 2 - users[referrer].noOfRecieved) *
                    doubleAmount;
                users[referrer].noOfRecieved +=
                    refTotalchilds /
                    2 -
                    users[referrer].noOfRecieved;
                    if (isToday(lastResetTimestamp)) {
                    today2xMembers += 1;
                } else {
                    today2xMembers = 1;
                    lastResetTimestamp = block.timestamp;
                }
            } else if (
                refTotalchilds / 2 > users[referrer].noOfjoined &&
                users[referrer].noOfRecieved < users[referrer].noOfjoined
            ) {
                users[referrer].balance +=
                    (users[referrer].noOfjoined -
                        users[referrer].noOfRecieved) *
                    doubleAmount;
                users[referrer].noOfRecieved +=
                    users[referrer].noOfjoined -
                    users[referrer].noOfRecieved;
                if (isToday(lastResetTimestamp)) {
                    today2xMembers += 1;
                } else {
                    today2xMembers = 1;
                    lastResetTimestamp = block.timestamp;
                }
            }
        }
    }

    function getAllUsers() public view returns (User[] memory) {
        User[] memory allUsers = new User[](Addresses.length);
        for (uint256 i = 0; i < Addresses.length; i++) {
            allUsers[i] = users[Addresses[i]];
        }
        return allUsers;
    }

    function isToday(uint256 timestamp) internal view returns (bool) {
        uint256 today = block.timestamp / 86400; // 86400 seconds in a day
        uint256 timestampDay = timestamp / 86400;
        return today == timestampDay;
    }

    function withdraw() public {
        require(users[msg.sender].balance > 0, "Insufficient balance");
        payable(msg.sender).transfer(users[msg.sender].balance);
        users[msg.sender].totalWithdrawal += users[msg.sender].balance;
        emit Withdrawal(msg.sender, users[msg.sender].balance);
        users[msg.sender].balance = 0;
    }
}