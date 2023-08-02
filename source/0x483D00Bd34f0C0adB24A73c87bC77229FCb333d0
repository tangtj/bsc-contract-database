//SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.19;

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

contract ProfitParadise {
    IERC20 public usdt;

    struct Player {
        uint256 deposit;
        uint256 earnings;
        uint256 lastUpdated;
        uint256 totalAirdrops;
        uint256 lastClaimed;
        uint256 totalClaimed;
        uint256 earningsRate;
        uint256 referrals;
        uint256 referredAmount;
        address referredBy;
        uint256 input;
    }
    struct Contest {
        address topDepositor;
        uint256 maxDeposit;
        bool claimed;
    }

    Contest public currentContest;

    mapping(address => Player) public players;
    mapping(address => bool) public hasDeposited;
    mapping(address => bool) public whitelist;
    address[] public playerList;
    address payable marketing;
    uint256 public contractLaunchTime;
    uint256 public topDepositPrizePool;
    uint256 public totalInvested;
    uint256 public totalReferralCount;
    uint256 public playerCount;
    uint256 public firstPool;
    uint256 public firstDepositValue;
    address public firstPlace;
    uint256 public secondPool;
    uint256 public secondDepositValue;
    address public secondPlace;
    uint256 public thirdPool;
    uint256 public thirdDepositValue;
    address public thirdPlace;
    uint256 public firstTotalEarned;
    uint256 public secondTotalEarned;
    uint256 public thirdTotalEarned;

    uint256 public topBalance;
    bool public reset;
    uint256 public resetTimestamp;

    uint256 public earningsRate = 300; // Base earnings rate is 3% 
    uint256 constant CLAIM_DURATION = 1 days; // Claims are open for 24 hours 
    uint256 constant CLAIM_INTERVAL = 7 days; // Claims are every 7 days 
    uint256 constant REWARD_INTERVAL = 1 days; // Earnings are daily 
    uint256 constant MULTIPLIER = 10000; // For math 
    uint256 constant REFERRAL_REWARD = 500; // 5% referral reward 
    uint256 constant TOP_DEPOSIT_PRIZE = 500; // 5% goes to rewards pool 

    constructor(
        address payable _marketingAddress, 
        address _usdtAddress,
        address[] memory _whitelist
    ) {
        marketing = _marketingAddress;
        usdt = IERC20(_usdtAddress);
        contractLaunchTime = 1690923600;

        for (uint256 i = 0; i < _whitelist.length; i++) {
            whitelist[_whitelist[i]] = true;
        }
    }

    // Function to deposit an amount, optionally with a referrer and option to create a team.
    function deposit(uint256 _amount, address _referrer) external payable {
        // Ensure the deposit is above the minimum required deposit of $10
        require(_amount >= 10 ether && block.timestamp >= contractLaunchTime && msg.sender == tx.origin, "Minimum deposit is $10");  

        // Transfer the deposit from the sender to this contract
        usdt.transferFrom(msg.sender, address(this), _amount); 

        // Calculate a portion of the deposit for the marketing fund
        uint256 marketingFund = _amount * 1000 / MULTIPLIER;

        // Transfer the calculated amount to the marketing fund
        usdt.transfer(marketing, marketingFund); 

        uint256 amount = _amount;
        // Player and team actions start here
        Player storage player = players[msg.sender];
        player.input += _amount;

        // If the player has deposited before,
        // Calculate and update their earnings and principal.
        if (player.deposit > 0) {
            (uint256 playersEarnings, uint256 principal) = calculateRewards(msg.sender);
            player.lastUpdated = block.timestamp;
            player.earnings += playersEarnings;
            player.deposit = principal;
        }

        // If the player is depositing for the first time,
        // Add a bonus to their deposit and add them to the player list.
        // If they are whitelisted, double up to 500 usdt
        if (!hasDeposited[msg.sender]) {
            if (whitelist[msg.sender]) {
                if (amount <= 250 ether) {
                    amount += amount;
                } else {
                    amount += 250 ether;
                }
            } else {
                amount += (_amount * 10) / 100;
            }
            player.earningsRate = getEarningsRate();
            playerList.push(msg.sender);
            hasDeposited[msg.sender] = true;
            playerCount +=1;
        }
        
        // If the player is referred by someone else,
        // Update the referrer's stats and transfer them the referral reward.
        if (_referrer != address(0) && _referrer != msg.sender) {
            Player storage referrer = players[_referrer];
            referrer.referrals += 1;
            referrer.referredAmount += _amount;
            uint256 airdropAmount = (_amount * REFERRAL_REWARD) / MULTIPLIER;
            usdt.transfer(_referrer, airdropAmount);
            
            // If the referrer is also part of a team, transfer half the referral reward to the team owner.
            if (referrer.referredBy != address(0)) {
                Player storage upline = players[referrer.referredBy];
                usdt.transfer(referrer.referredBy, airdropAmount / 2);
                upline.totalAirdrops += airdropAmount / 2;

                if (upline.referredBy != address(0)) {
                    Player storage upline2 = players[referrer.referredBy];
                    usdt.transfer(upline.referredBy, airdropAmount / 4);
                    upline2.totalAirdrops += airdropAmount / 4;
                }
            }
            referrer.totalAirdrops += airdropAmount;
            amount += (_amount * 10) / 100;
            totalReferralCount += 1;
        }

        if (currentContest.claimed) {
            startNewContest();
        }

        // If it's a claim day, update the top depositor if necessary
        if (isClaimDay()) { 
            if (_amount > currentContest.maxDeposit) {
                currentContest.maxDeposit = _amount;
                currentContest.topDepositor = msg.sender;
            }
        }

        // Calculate the portions of the deposit for the different pools
        uint256 firstAllocation = (_amount * 5) / 100;
        uint256 secondAllocation = (_amount * 3) / 100;
        uint256 thirdAllocation = _amount / 100;

        // Add the portions to the pools
        firstPool += firstAllocation;
        secondPool += secondAllocation;
        thirdPool += thirdAllocation;

        // Update the player's deposit and max payout
        player.lastUpdated = block.timestamp;
        player.deposit += amount;
        updateTopUsers(msg.sender, player.deposit);

        // Add the deposited amount to the total invested and to the rebalance reward pool
        totalInvested += amount;
        topDepositPrizePool += (_amount * TOP_DEPOSIT_PRIZE) / MULTIPLIER;

        if (usdt.balanceOf(address(this)) > topBalance) {
            topBalance = usdt.balanceOf(address(this));
        }
    }

    function updateTopUsers(address depositor, uint256 amount) internal {
        // Check if the deposit is greater than the current first deposit
        if (amount > firstDepositValue) {
            // Shift the first place to the second place
            secondPlace = firstPlace;
            secondDepositValue = firstDepositValue;

            // Shift the second place to the third place
            thirdPlace = secondPlace;
            thirdDepositValue = secondDepositValue;

            // Update the first place with the new deposit
            firstPlace = depositor;
            firstDepositValue = amount;
        } else if (amount > secondDepositValue) { // Check if the deposit is greater than the current second deposit
            // Shift the second place to the third place
            thirdPlace = secondPlace;
            thirdDepositValue = secondDepositValue;

            // Update the second place with the new deposit
            secondPlace = depositor;
            secondDepositValue = amount;
        } else if (amount > thirdDepositValue) { // Check if the deposit is greater than the current third deposit
            // Update the third place with the new deposit
            thirdPlace = depositor;
            thirdDepositValue = amount;
        }
    }

    function getEarningsRate() public view returns (uint256) {
        uint256 launchTime = contractLaunchTime;
        uint256 currentTime = block.timestamp;
        
        uint256 timeElapsed = currentTime - launchTime;
        uint256 weeksElapsed = timeElapsed / 1 weeks;
        
        uint256 baseEarningsRate = earningsRate; // This represents 3.0%
        uint256 incrementPerWeek = 50; // This represents 0.5%

        uint256 currentEarningsRate = baseEarningsRate + (weeksElapsed * incrementPerWeek);
        
        return currentEarningsRate;
    }

    // Calculates the reward for a player based on their deposit and the elapsed time since their last update, accounting for penalties and reversals.
    function calculateRewards(address _playerAddress) public view returns (uint256 rewards, uint256 principal) {
        // Access the player's information
        Player storage player = players[_playerAddress];

        if (player.deposit == 0) {
            return (0,0);
        } else {
            // Calculate the player's expected rewards based on their deposit, earnings rate, and time since last update
            uint256 expectedRewards = ((block.timestamp - player.lastUpdated) * player.deposit * player.earningsRate) / (MULTIPLIER * REWARD_INTERVAL);

            // Return the adjusted or expected rewards, along with the adjusted principal
            return (expectedRewards, player.deposit);
        }
    }

    // Checks whether it's claim day, which is a 24-hour period that occurs every 7 days
    function isClaimDay() public view returns (bool) {
        uint256 timeSinceLaunch = block.timestamp - contractLaunchTime;
        return timeSinceLaunch >= CLAIM_INTERVAL && timeSinceLaunch % (CLAIM_INTERVAL + CLAIM_DURATION) < CLAIM_DURATION;
    }

    // Allows top user to claim profit share
    function claimProfitShare() external returns (uint256) {
        require(msg.sender == firstPlace || msg.sender == secondPlace || msg.sender == thirdPlace, "You are not a top user.");
        Player storage player = players[msg.sender];

        uint256 prize;
        
        if (msg.sender == firstPlace) {
            prize = firstPool;
            firstPool = 0;
        } else if (msg.sender == secondPlace) {
            prize = secondPool;
            secondPool = 0;
        } else if (msg.sender == thirdPlace) {
            prize = thirdPool;
            thirdPool = 0;
        }
        
        player.totalClaimed += prize;
        usdt.transfer(msg.sender, prize * 90 / 100);
        usdt.transfer(marketing, prize * 10 / 100);

        checkForReset();
        return prize;
    }

    // Allows a player to redeem their initial deposit if certain conditions are met, specifically if their referred amount is at least five times their deposit.
    function redeemInitial() external {
        // Access the player's information
        Player storage player = players[msg.sender];

        // Ensure the player has referred enough to redeem their initial deposit
        require(player.referredAmount >= player.deposit * 5, "error");

        // Calculate the amount to be paid to the player
        uint256 payment = ((player.deposit * 80) / 100) - player.totalClaimed;

        // Transfer the payment to the player and update their claimed amount
        player.totalClaimed += payment;
        usdt.transfer(msg.sender, payment);

        // Update the time of the player's last update and claim
        player.lastUpdated = block.timestamp;
        player.lastClaimed = block.timestamp;

        // Reset the player's earnings
        player.earnings = 0;
        checkForReset();
    }

    // Allows a player to claim their earnings as a reward, with an option to tip the developer a percentage of the earnings
    function claimRewards(bool _tip) external returns ( uint256 paidRewards, uint256 principalBal, uint256 claimed ){
        // Access the player's information
        Player storage player = players[msg.sender];

        // Calculate the player's earnings and principal
        (uint256 playersEarnings, uint256 principal) = calculateRewards(msg.sender);
        player.deposit = principal;

        // Ensure it's claim day and the player has earnings to claim
        require(isClaimDay() && playersEarnings > 0, "error");

        // Reset the player's earnings and update the time of their last update and claim
        player.earnings = 0;
        player.lastUpdated = block.timestamp;
        player.lastClaimed = block.timestamp;

        // If the player has chosen to tip the developer, calculate the tip amount and transfer it to the marketing wallet
        if (_tip) {
            uint256 tipAmount = (playersEarnings * 5) / 100;
            playersEarnings -= tipAmount;
            usdt.transfer(marketing, tipAmount);
        }
        
        // Update the player's total claimed amount
        player.totalClaimed += playersEarnings;

        // Transfer the player's earnings to them 
        usdt.transfer(msg.sender, playersEarnings); 
        checkForReset();
        return (playersEarnings, player.deposit, player.totalClaimed);
    }
    
    // Claim reward function for top depositor
    function claimTopDepositPrize() external {
        require(msg.sender == currentContest.topDepositor && !currentContest.claimed && !isClaimDay(), "Not eligible to claim");
        uint256 prize = topDepositPrizePool;
        topDepositPrizePool = 0;
        
        currentContest.claimed = true;
        usdt.transfer(currentContest.topDepositor, prize * 90 / 100);
        usdt.transfer(marketing, prize * 10 / 100);

        Player storage player = players[msg.sender];
        player.totalClaimed += prize * 90 / 100;
        checkForReset();
    }


    // Function to start a new contest
    function startNewContest() internal {
        currentContest = Contest({
            topDepositor: address(0),
            maxDeposit: 0,
            claimed: false
        });
    }

    // Returns the remaining time of the current claim day in seconds
    function claimDayTimeRemaining() public view returns (uint) {
        uint256 timeSinceLaunch = block.timestamp - contractLaunchTime;
        if (timeSinceLaunch >= CLAIM_INTERVAL && timeSinceLaunch % CLAIM_INTERVAL < CLAIM_DURATION) {
            return CLAIM_DURATION - (timeSinceLaunch % CLAIM_INTERVAL);
        } else {
            return 0;
        }
    }

    // Helper function for the Frontend; Returns addresses, deposits, and their earnings rates
    function getPlayersInfo() public view returns (address[] memory, uint256[] memory, uint256[] memory) {
        address[] memory addresses = new address[](playerList.length);
        uint256[] memory deposits = new uint256[](playerList.length);
        uint256[] memory earningsRates = new uint256[](playerList.length);

        for (uint256 i = 0; i < playerList.length; i++) {
            addresses[i] = playerList[i];
            deposits[i] = players[playerList[i]].deposit;
            earningsRates[i] = players[playerList[i]].earningsRate;
        }

        return (addresses, deposits, earningsRates);
    }

    // Returns the timestamp of the next claim day.
    function getNextClaimDay() public view returns (uint256 nextClaimDay) {
        uint256 timeSinceLaunch = block.timestamp - contractLaunchTime;
        uint256 cyclesSinceLaunch = timeSinceLaunch / (CLAIM_INTERVAL + CLAIM_DURATION);
        return contractLaunchTime + (cyclesSinceLaunch + 1) * (CLAIM_INTERVAL + CLAIM_DURATION);
    }

    // Checks if TVL is at 20% of ATH, if so, game is wiped clean and started again with TVL carryover (like BLXN Limitless)
    function checkForReset() internal {
        uint256 resetThreshold = (topBalance * 20) / 100;
        if (usdt.balanceOf(address(this)) <= resetThreshold) {
            // Erase all players and their whitelist status
            for(uint256 i = 0; i < playerList.length; i++) {
                delete players[playerList[i]];
                delete hasDeposited[playerList[i]];
                whitelist[playerList[i]] = false;
            }
            
            // Erase player list
            delete playerList;
            
            // Erase contest
            delete currentContest;
            
            // Reset all stats
            totalInvested = 0;
            totalReferralCount = 0;
            topDepositPrizePool = 0;
            playerCount = 0;

            firstPool = 0;
            firstDepositValue = 0;
            firstPlace = address(0);

            secondPool = 0;
            secondDepositValue = 0;
            secondPlace = address(0);

            thirdPool = 0;
            thirdDepositValue = 0;
            thirdPlace = address(0);

            firstTotalEarned = 0;
            secondTotalEarned = 0;
            thirdTotalEarned = 0;

            topBalance = 0;

            contractLaunchTime = block.timestamp + 1 days;
            reset = true;
            resetTimestamp = block.timestamp;
            
            // Increase earnings rate by 1%
            earningsRate += 100; // since the rate is represented as a basis point (1% = 100 basis points = 10 in this case)
        }
    }

}