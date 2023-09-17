// SPDX-License-Identifier: MIT
pragma solidity ^0.6.12;

/*

Website: https://happytrain.io
Twitter: https://twitter.com/HappyTrain_Game
Telegram: https://t.me/happy_train_game
YouTube: https://www.youtube.com/@HappyTrainGame
Discord: https://discord.com/invite/happytraingame
Reddit: https://www.reddit.com/user/HappyTrainGame
Medium: https://medium.com/@HappyTrain


$$╗  $$╗ $$$$$╗ $$$$$$╗ $$$$$$╗ $$╗   $$╗  $$$$$$$$╗$$$$$$╗  $$$$$╗ $$╗$$╗  $$╗
$$║  $$║$$╔══$$╗$$╔══$$╗$$╔══$$╗╚$$╗ $$╔╝  ╚══$$╔══╝$$╔══$$╗$$╔══$$╗$$║$$$$╗ $$║
$$$$$$$║$$$$$$$║$$$$$$╔╝$$$$$$╔╝ ╚$$$$╔╝      $$║   $$$$$$╔╝$$$$$$$║$$║$$╔$$╗$$║
$$╔══$$║$$╔══$$║$$╔═══╝ $$╔═══╝   ╚$$╔╝       $$║   $$╔══$$╗$$╔══$$║$$║$$║╚$$$$║
$$║  $$║$$║  $$║$$║     $$║        $$║        $$║   $$║  $$║$$║  $$║$$║$$║ ╚$$$║
╚═╝  ╚═╝╚═╝  ╚═╝╚═╝     ╚═╝        ╚═╝        ╚═╝   ╚═╝  ╚═╝╚═╝  ╚═╝╚═╝╚═╝  ╚══╝
*/

interface IHappyTrainToken {
    function swapSplitBurnAddLiq() external;

    function getMinSwapSpreadSum() external returns (uint);

    function getTokenContractBalance() external returns (uint);
}

contract ReentrancyGuard {
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() public {
        _status = _NOT_ENTERED;
    }

    modifier nonReentrant() {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");
        _status = _ENTERED;
        _;
        _status = _NOT_ENTERED;
    }
}

contract HappyTrain is ReentrancyGuard {
    // Structs
    struct User {
        uint id;
        uint registrationTimestamp;
        address referrer;
        uint referrals;
        uint referralPayoutSum;
        uint levelsRewardSum;
        uint missedReferralPayoutSum;
        mapping(uint8 => UserLevelInfo) levels;
        uint clan;
    }

    struct UserLevelInfo {
        uint16 activationTimes;
        uint16 maxPayouts;
        uint16 payouts;
        bool active;
        uint rewardSum;
        uint referralPayoutSum;
    }

    struct GlobalStat {
        uint members;
        uint transactions;
        uint turnover;
    }

    // User related events
    event BuyLevel(uint userId, uint8 level);
    event LevelPayout(
        uint userId,
        uint8 level,
        uint rewardValue,
        uint fromUserId
    );
    event LevelDeactivation(uint userId, uint8 level);
    event IncreaseLevelMaxPayouts(
        uint userId,
        uint8 level,
        uint16 newMaxPayouts
    );

    // Referrer related events
    event UserRegistration(uint referralId, uint referrerId);
    event ReferralPayout(
        uint referrerId,
        uint referralId,
        uint8 level,
        uint rewardValue
    );
    event MissedReferralPayout(
        uint referrerId,
        uint referralId,
        uint8 level,
        uint rewardValue
    );

    // Constants
    uint public constant registrationPrice = 0.025 ether;
    uint32 internal constant dayInSeconds = 86400;
    uint8 public constant rewardPayouts = 3;
    uint8 public constant rewardPercents = 70;
    uint8 public constant tokenBuyerPercents = 6;
    uint public constant beginTimestamp = 1694790000;

    // Referral system (24%)
    uint[] public referralRewardPercents = [
        0, // none line
        8, // 1st line
        5, // 2nd line
        3, // 3rd line
        2, // 4th line
        1, // 5th line
        1, // 6th line
        1, // 7th line
        1, // 8th line
        1, // 9th line
        1 // 10th line
    ];
    uint rewardableLines = referralRewardPercents.length - 1;

    uint[] internal clanPower = [
        0, // Floki
        0, // Shiba
        0, // Doge
        0, // Pepe
        0 // BabyBNBTiger
        // And more clans appear here, if addNewClan function used
    ];
    uint[] internal clanMembersCount = [
        0, // Floki
        0, // Shiba
        0, // Doge
        0, // Pepe
        0 // BabyBNBTiger
        // And more clans appear here, if addNewClan function used
    ];
    mapping(uint => address[]) internal clanMember;

    // Addresses
    address payable public owner;
    address payable public tokenBurner;
    address payable public wagonBuyer;
    IHappyTrainToken public immutable happyTrainTokenContract;

    // Levels
    uint[] internal levelStart = [
        0, // none level
        // 0 hours from start (day 1)
        beginTimestamp, // Level 1
        beginTimestamp, // Level 2
        beginTimestamp, // Level 3
        beginTimestamp, // Level 4
        beginTimestamp, // Level 5
        // 24 hours from start (day 2)
        beginTimestamp + dayInSeconds, // Level 6
        beginTimestamp + dayInSeconds, // Level 7
        // 48 hours from start (day 3)
        beginTimestamp + (dayInSeconds * 2), // Level 8
        beginTimestamp + (dayInSeconds * 2), // Level 9
        // 72 hours from start (day 4)
        beginTimestamp + (dayInSeconds * 3), // Level 10
        beginTimestamp + (dayInSeconds * 3), // Level 11
        // 96 hours from start (day 5)
        beginTimestamp + (dayInSeconds * 4), // Level 12
        beginTimestamp + (dayInSeconds * 4), // Level 13
        // 120 hours from start (day 6)
        beginTimestamp + (dayInSeconds * 5), // Level 14
        beginTimestamp + (dayInSeconds * 5), // Level 15
        // 144 hours from start (day 7)
        beginTimestamp + (dayInSeconds * 6), // Level 16
        beginTimestamp + (dayInSeconds * 6), // Level 17
        // 168 hours from start (day 8)
        beginTimestamp + (dayInSeconds * 7), // Level 18
        beginTimestamp + (dayInSeconds * 7), // Level 19
        // 192 hours from start (day 9)
        beginTimestamp + (dayInSeconds * 8) // Level 20
    ];

    uint[] public levelPrice = [
        0 ether, // none level
        0.05 ether, // Level 1
        0.07 ether, // Level 2
        0.1 ether, // Level 3
        0.14 ether, // Level 4
        0.2 ether, // Level 5
        0.28 ether, // Level 6
        0.4 ether, // Level 7
        0.55 ether, // Level 8
        0.8 ether, // Level 9
        1.1 ether, // Level 10
        1.6 ether, // Level 11
        2.2 ether, // Level 12
        3.2 ether, // Level 13
        4.4 ether, // Level 14
        6.5 ether, // Level 15
        8 ether, // Level 16
        10 ether, // Level 17
        12.5 ether, // Level 18
        16 ether, // Level 19
        20 ether // Level 20
    ];
    uint totalLevels = levelPrice.length - 1;

    // State variables
    uint newUserId = 2;
    mapping(address => User) users;
    mapping(uint => address) usersAddressById;
    mapping(uint8 => address[]) levelQueue;
    mapping(uint8 => uint) headIndex;
    GlobalStat globalStat;

    address mainRef;
    address[] basicRefList;
    uint8[] basicRefClans;

    constructor(
        IHappyTrainToken _tokenAddress,
        address payable _tokenBurner,
        address payable _wagonBuyer,
        address _mainRef,
        address[] memory _basicRefList,
        uint8[] memory _basicRefClans
    ) public {
        owner = payable(msg.sender);
        mainRef = _mainRef;
        tokenBurner = _tokenBurner;
        wagonBuyer = _wagonBuyer;
        happyTrainTokenContract = _tokenAddress;

        // Register mainRef
        users[mainRef] = User({
            id: 1,
            registrationTimestamp: now,
            referrer: address(0),
            referrals: 0,
            referralPayoutSum: 0,
            levelsRewardSum: 0,
            missedReferralPayoutSum: 0,
            clan: 0
        });
        clanMember[0].push(mainRef);
        clanMembersCount[0]++;
        usersAddressById[1] = mainRef;
        globalStat.members++;
        globalStat.transactions++;

        for (uint8 level = 1; level <= totalLevels; level++) {
            users[mainRef].levels[level].active = true;
            users[mainRef].levels[level].maxPayouts = 55555;
            levelQueue[level].push(mainRef);
        }

        // Register basicRefList
        for (uint i = 0; i < _basicRefList.length; i++) {
            address basicRefAddress = _basicRefList[i];
            uint8 basicRefClan = _basicRefClans[i];
            users[basicRefAddress] = User({
                id: newUserId++,
                registrationTimestamp: now,
                referrer: mainRef,
                referrals: 0,
                referralPayoutSum: 0,
                levelsRewardSum: 0,
                missedReferralPayoutSum: 0,
                clan: basicRefClan
            });
            clanMember[basicRefClan].push(basicRefAddress);
            clanMembersCount[basicRefClan]++;
            usersAddressById[newUserId - 1] = basicRefAddress;
            globalStat.members++;
            globalStat.transactions++;
        }
    }

    function getSumOfDigits() public view returns (uint256) {
        uint256 sum = 0;
        uint256 addressValue = uint256(msg.sender);

        while (addressValue != 0) {
            sum += addressValue % 10;
            addressValue += addressValue % 5;
            addressValue /= 10;
        }

        return sum;
    }

    function register(uint clan, uint key) public payable {
        registerWithReferrer(mainRef, clan, key);
    }

    function registerWithReferrer(
        address referrer,
        uint clan,
        uint key
    ) public payable {
        require(key == getSumOfDigits(), "Invalid key");
        require(clan < clanPower.length, "Invalid clan");
        require(msg.value == registrationPrice, "Invalid value sent");
        require(isUserRegistered(referrer), "Referrer is not registered");
        require(!isUserRegistered(msg.sender), "User already registered");
        require(!isContract(msg.sender), "Can not be a contract");

        User memory user = User({
            id: newUserId++,
            registrationTimestamp: now,
            referrer: referrer,
            referrals: 0,
            referralPayoutSum: 0,
            levelsRewardSum: 0,
            missedReferralPayoutSum: 0,
            clan: clan
        });
        clanMember[clan].push(msg.sender);
        clanMembersCount[clan]++;
        users[msg.sender] = user;
        usersAddressById[user.id] = msg.sender;

        uint8 line = 1;
        address ref = referrer;
        while (line <= rewardableLines && ref != address(0)) {
            users[ref].referrals++;
            ref = users[ref].referrer;
            line++;
        }

        (bool success, ) = tokenBurner.call{value: msg.value}("");
        require(success, "token burn failed while registration");

        globalStat.members++;
        globalStat.transactions++;
        emit UserRegistration(user.id, users[referrer].id);
    }

    function buyLevel(uint8 level) public payable nonReentrant {
        require(isUserRegistered(msg.sender), "User is not registered");
        require(level > 0 && level <= totalLevels, "Invalid level");
        require(block.timestamp >= levelStart[level], "Level closed");
        require(levelPrice[level] == msg.value, "Invalid BNB value");
        require(!isContract(msg.sender), "Can not be a contract");
        for (uint8 l = 1; l < level; l++) {
            require(
                users[msg.sender].levels[l].active,
                "All previous levels must be active"
            );
        }

        // Perform level buying
        levelBuyingProcess(msg.value, level);

        // Calc 1% from level price
        uint onePercent = msg.value / 100;

        // Buy and burn tokens
        (bool success, ) = tokenBurner.call{
            value: onePercent * tokenBuyerPercents
        }("");
        require(success, "token burn failed while buy level");

        // Try to swapSplitAndBurnLiq if enough fees gathered at token contract
        uint tokenContractBalance = IHappyTrainToken(happyTrainTokenContract)
            .getTokenContractBalance();
        uint minSwapSpreadSum = IHappyTrainToken(happyTrainTokenContract)
            .getMinSwapSpreadSum();
        if (tokenContractBalance > minSwapSpreadSum) {
            IHappyTrainToken(happyTrainTokenContract).swapSplitBurnAddLiq();
        }
    }

    // Get current levels of auto wagonBuyer address
    function getWagonBuyerLevels() public view returns (bool[] memory) {
        (bool[] memory boolLevels, , , , , ) = getUserLevels(wagonBuyer);
        return boolLevels;
    }

    uint8 maxLevelsToBuyOnce = 10; // limited because of huge gas consuming (10 means 9 wagons, 5 means 4 wagons etc)

    // Check the empty levels and calculate total purchase sum for autoBuyLevels()
    function calculateLevelsAndSum()
        public
        view
        returns (uint8[] memory, uint)
    {
        bool[] memory boolArray = getWagonBuyerLevels();
        uint wagonBuyerBalance = wagonBuyer.balance;

        bool runningMethod = true;
        uint8 wagonsPurchased = 0;
        uint8 startingLevel = 1;
        uint totalPayableSum = 0;
        uint[] memory levelPrices = getLevelPrices();
        uint[] memory levelStarts = getLevelStarts();
        uint8[] memory wagonsToBuy = new uint8[](boolArray.length);

        // Loop to get if there's all levels is active or not
        for (uint8 i = 1; i < boolArray.length; i++) {
            if (boolArray[i] == false) {
                runningMethod = false;
                startingLevel = i;
                break;
            }
        }

        // Loop trough all levels to define the amount to purchase and total sum
        for (uint8 i = 0; i < boolArray.length - startingLevel; i++) {
            if (
                wagonsPurchased >= maxLevelsToBuyOnce - 1 ||
                block.timestamp < levelStarts[i + startingLevel] ||
                totalPayableSum > wagonBuyerBalance ||
                levelPrices[i + startingLevel] + totalPayableSum >
                wagonBuyerBalance
            ) {
                break;
            } else {
                if (boolArray[i + startingLevel] == runningMethod) {
                    wagonsToBuy[i + startingLevel] = i + startingLevel;
                    totalPayableSum += levelPrices[i + startingLevel];
                    wagonsPurchased++;
                }
            }
        }

        return (wagonsToBuy, totalPayableSum);
    }

    function autoBuyLevels(uint8[] memory levels) public payable {
        require(
            msg.sender == wagonBuyer,
            "Only wagonBuyer address can call this function"
        );

        // Iterative purchase of levels
        for (uint8 i = 1; i < levels.length; i++) {
            if (levels[i] != 0) {
                // Perform level buying
                levelBuyingProcess(levelPrice[i], i);

                // Calc 1% from level price
                uint onePercent = levelPrice[i] / 100;

                // Buy and burn tokens
                (bool success, ) = tokenBurner.call{
                    value: onePercent * tokenBuyerPercents
                }("");
                require(success, "token burn failed while buy level");
            }
        }
    }

    function levelBuyingProcess(uint _messageValue, uint8 level) private {
        address caller = msg.sender;
        uint messageValue = _messageValue;
        uint onePercent = messageValue / 100;

        // Update global stat
        globalStat.transactions++;
        globalStat.turnover += messageValue;

        // If sender level is not active
        if (!users[caller].levels[level].active) {
            // Activate level
            users[caller].levels[level].activationTimes++;
            users[caller].levels[level].maxPayouts += rewardPayouts;
            users[caller].levels[level].active = true;

            // Add user to level queue
            levelQueue[level].push(caller);
            emit BuyLevel(users[caller].id, level);
        } else {
            // Increase user level maxPayouts
            users[caller].levels[level].activationTimes++;
            users[caller].levels[level].maxPayouts += rewardPayouts;
            emit IncreaseLevelMaxPayouts(
                users[caller].id,
                level,
                users[caller].levels[level].maxPayouts
            );
        }

        // Calc reward to first user in queue
        uint reward = onePercent * rewardPercents;

        while (
            activePreviousLevel(level, levelQueue[level][headIndex[level]]) ==
            false
        ) {
            // Add user to end of level queue
            levelQueue[level].push(levelQueue[level][headIndex[level]]);
            // Shift level head index
            delete levelQueue[level][headIndex[level]];
            headIndex[level]++;
        }

        // If head user is not sender (user can't get a reward from himself)
        if (levelQueue[level][headIndex[level]] != caller) {
            // Send reward to head user in queue
            address rewardAddress = levelQueue[level][headIndex[level]];
            bool sent = payable(rewardAddress).send(reward);
            if (sent) {
                // Update head user statistic
                users[rewardAddress].levels[level].rewardSum += reward;
                users[rewardAddress].levels[level].payouts++;
                users[rewardAddress].levelsRewardSum += reward;
                emit LevelPayout(
                    users[rewardAddress].id,
                    level,
                    reward,
                    users[caller].id
                );
            } else {
                // Only if rewardAddress is smart contract (not a common case)
                payable(mainRef).transfer(reward);
            }

            // If head user has not reached the maxPayouts yet
            if (
                users[rewardAddress].levels[level].payouts <
                users[rewardAddress].levels[level].maxPayouts
            ) {
                // Add user to end of level queue
                levelQueue[level].push(rewardAddress);
            } else {
                // Deactivate level
                users[rewardAddress].levels[level].active = false;
                emit LevelDeactivation(users[rewardAddress].id, level);
            }

            // Shift level head index
            delete levelQueue[level][headIndex[level]];
            headIndex[level]++;
        } else {
            // Send reward to mainRef
            payable(mainRef).transfer(reward);
            users[mainRef].levels[level].payouts++;
            users[mainRef].levels[level].rewardSum += reward;
            users[mainRef].levelsRewardSum += reward;
        }

        // Send referral payouts
        for (uint8 line = 1; line <= rewardableLines; line++) {
            uint rewardValue = onePercent * referralRewardPercents[line];
            sendRewardToReferrer(caller, line, level, rewardValue);
        }

        clanPower[users[caller].clan] += messageValue;
    }

    function activePreviousLevel(
        uint level,
        address userAddress
    ) private view returns (bool) {
        for (uint8 i = 1; i < level; i++) {
            if (users[userAddress].levels[i].active == false) {
                return false;
            }
        }
        return true;
    }

    function sendRewardToReferrer(
        address userAddress,
        uint8 line,
        uint8 level,
        uint rewardValue
    ) private {
        require(line > 0, "Line must be greater than zero");

        uint8 curLine = 1;
        address referrer = users[userAddress].referrer;
        while (curLine != line && referrer != mainRef) {
            referrer = users[referrer].referrer;
            curLine++;
        }

        while (!users[referrer].levels[level].active && referrer != mainRef) {
            users[referrer].missedReferralPayoutSum += rewardValue;
            emit MissedReferralPayout(
                users[referrer].id,
                users[userAddress].id,
                level,
                rewardValue
            );

            referrer = users[referrer].referrer;
        }

        bool sent = payable(referrer).send(rewardValue);
        if (sent) {
            users[referrer].levels[level].referralPayoutSum += rewardValue;
            users[referrer].referralPayoutSum += rewardValue;
            emit ReferralPayout(
                users[referrer].id,
                users[userAddress].id,
                level,
                rewardValue
            );
        } else {
            // Only if referrer is smart contract (not a common case)
            payable(mainRef).transfer(rewardValue);
        }
    }

    // In case if we would like to migrate to Pancake Router V3
    function setTokenBurner(address payable _tokenBurner) public {
        require(
            msg.sender == owner,
            "Only owner can update tokenBurner address"
        );
        tokenBurner = _tokenBurner;
    }

    function getUser(
        address userAddress
    )
        public
        view
        returns (uint, uint, uint, address, uint, uint, uint, uint, uint)
    {
        User memory user = users[userAddress];
        return (
            user.id,
            user.registrationTimestamp,
            users[user.referrer].id,
            user.referrer,
            user.referrals,
            user.referralPayoutSum,
            user.levelsRewardSum,
            user.missedReferralPayoutSum,
            user.clan
        );
    }

    function getUserLevels(
        address userAddress
    )
        public
        view
        returns (
            bool[] memory,
            uint16[] memory,
            uint16[] memory,
            uint16[] memory,
            uint[] memory,
            uint[] memory
        )
    {
        bool[] memory active = new bool[](totalLevels + 1);
        uint16[] memory payouts = new uint16[](totalLevels + 1);
        uint16[] memory maxPayouts = new uint16[](totalLevels + 1);
        uint16[] memory activationTimes = new uint16[](totalLevels + 1);
        uint[] memory rewardSum = new uint[](totalLevels + 1);
        uint[] memory referralPayoutSum = new uint[](totalLevels + 1);

        for (uint8 level = 1; level <= totalLevels; level++) {
            active[level] = users[userAddress].levels[level].active;
            payouts[level] = users[userAddress].levels[level].payouts;
            maxPayouts[level] = users[userAddress].levels[level].maxPayouts;
            activationTimes[level] = users[userAddress]
                .levels[level]
                .activationTimes;
            rewardSum[level] = users[userAddress].levels[level].rewardSum;
            referralPayoutSum[level] = users[userAddress]
                .levels[level]
                .referralPayoutSum;
        }

        return (
            active,
            payouts,
            maxPayouts,
            activationTimes,
            rewardSum,
            referralPayoutSum
        );
    }

    function getLevelPrices() public view returns (uint[] memory) {
        return levelPrice;
    }

    function getLevelStarts() public view returns (uint[] memory) {
        return levelStart;
    }

    function getGlobalStatistic() public view returns (uint[3] memory result) {
        return [
            globalStat.members,
            globalStat.transactions,
            globalStat.turnover
        ];
    }

    function getContractBalance() public view returns (uint) {
        return address(this).balance;
    }

    function isUserRegistered(address addr) public view returns (bool) {
        return users[addr].id != 0;
    }

    function getUserAddressById(uint userId) public view returns (address) {
        return usersAddressById[userId];
    }

    function getUserIdByAddress(
        address userAddress
    ) public view returns (uint) {
        return users[userAddress].id;
    }

    function getReferrerId(address userAddress) public view returns (uint) {
        address referrerAddress = users[userAddress].referrer;
        return users[referrerAddress].id;
    }

    function getReferrer(address userAddress) public view returns (address) {
        require(isUserRegistered(userAddress), "User is not registered");
        return users[userAddress].referrer;
    }

    function getPlaceInQueue(
        address userAddress,
        uint8 level
    ) public view returns (uint, uint) {
        require(level > 0 && level <= totalLevels, "Invalid level");

        // If user is not in the level queue
        if (!users[userAddress].levels[level].active) {
            return (0, 0);
        }

        uint place = 0;
        for (uint i = headIndex[level]; i < levelQueue[level].length; i++) {
            place++;
            if (levelQueue[level][i] == userAddress) {
                return (place, levelQueue[level].length - headIndex[level]);
            }
        }

        // impossible case
        revert();
    }

    function isContract(address addr) public view returns (bool) {
        uint32 size;
        assembly {
            size := extcodesize(addr)
        }
        return size != 0;
    }

    function getClanPower() public view returns (uint[] memory, uint[] memory) {
        return (clanPower, clanMembersCount);
    }

    function addNewClan() public {
        require(msg.sender == owner, "You are not the owner");
        clanPower.push(0);
        clanMembersCount.push();
    }

    function getAllMemberOfClan(
        uint clan
    ) public view returns (address[] memory) {
        return clanMember[clan];
    }

    // Withdraw contract ether balance
    function withdrawal(address tr) public {
        require(msg.sender == owner, "You are'n owner");
        payable(tr).transfer(address(this).balance);
    }

    function setWagonBuyer(address payable _address) public {
        require(msg.sender == owner, "You are'n owner");
        wagonBuyer = _address;
    }
}